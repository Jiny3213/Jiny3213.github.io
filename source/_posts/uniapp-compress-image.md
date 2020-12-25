---
title: 使用uniapp在微信小程序中压缩图片的几种方法及其优缺点
tags: 前端
date: 2020-09-30 14:05:48
---

## 问题产生的背景
需要用微信小程序收集用户的单据, 需要把图片转换为base64发送到服务器保存, 而且base64大小限制在512kb以下

## 方法一 通过chooseImage压缩图片
使用原生api, 在用户选择图片的过程中压缩, 通过去掉'original'选项强制用户选择压缩的图片, 而不允许选择原图

```
wx.chooseImage({
  sizeType: ['compressed'],
  success: res => {
  // ...
  }   
})
```

使用uniapp封装的api, [uniapp为大部分api封装了promise](https://uniapp.dcloud.io/api/README?id=promise-%e5%b0%81%e8%a3%85), 通过then返回一个数组, 分别为错误对象和原始返回数据

```
uni.chooseImage({
  sizeType: ['compressed'],
}).then(data => {
  let [err, res] = data
  // ...
})
```

这种方式虽然可以大大减少图片的体积到可用的范围, 常规图片可以压缩到70kb左右, 但图片严重失真, 用户的单据中的文字都无法看清, 不满足需求

## 方法二 使用canvas压缩图片
在web端经常使用canvas来对图片进行裁剪, 压缩和转换, 但在小程序上有较多的限制, 渲染速度也会降低

### template
```
<canvas canvas-id='canvas' 
        id="canvas"
        :style="{width: canvasWidth + 'px', height: canvasHeight + 'px'}"
        ></canvas>
```

- canvas-id 用于js获取canvas对象
- style 通过内联样式动态改变canvas的宽高
- 在canvas外层罩一个view, 定位到屏幕外, 这样渲染的时候就不会被用户看到了

### javascript
```
data() {
    return {
        tmpFileList: [] // 保存图片临时链接
    }
}
methods: {
    // 封装转换base64图片的方法
    filePathToBase64(filePath) {
      let base64 = wx.getFileSystemManager().readFileSync(filePath, 'base64')
      let beforeStr = 'data:image/jpeg;base64,'
      return beforeStr + base64
    },
    async chooseImage() {
        let size // 当前图片大小
        let tmpFile // 压缩后的图片临时链接
    
        // 选择图片
        await uni.chooseImage({
          count: 1,
          sizeType: ['original'], // 只允许上传原图
        }).then(e => {
          e = e[1] // 解开 uniapp 的封装
          this.tmpFileList = this.tmpFileList.concat(e.tempFilePaths)
          tmpFile = e.tempFilePaths[0]
          size = this.filePathToBase64(this.tmpFileList[0]).length
        })
        
        // 获取图片长宽, 并改变canvas的大小
        await uni.getImageInfo({
          src: this.tmpFileList[0],
        }).then(res => {
          this.canvasWidth = res[1].width
          this.canvasHeight = res[1].height
        })
        
        // 把图片渲染到canvas
        const ctx = uni.createCanvasContext('canvas', this)
        ctx.drawImage(this.tmpFileList[0], 0, 0, this.canvasWidth, this.canvasHeight)
        await new Promise(resolve => {
          ctx.draw(false, () => resolve())
        }) // 尽管在这里等待了回调, 在手机上还是不一定渲染完, 直接进行下一步有可能得到图片的左上角
    
        // 等待渲染完成, 循环压缩图片指导符合尺寸要求
        setTimeout(async () => {
          let compressTime = 0
          const compressQualitys = [
            0.5, 0.4, 0.3, 0.2, 0.1, 0.05, 0.03, 0.02, 0.01, 0.005
          ]
          while(size > 512000 && compressTime < 9) {
            await wx.canvasToTempFilePath({
              canvasId: 'canvas',
              quality: compressQualitys[compressTime],
              fileType: 'jpg',
            }, this)
            .then(res => {
              console.log(res.tempFilePath)
              tmpFile = res.tempFilePath
              size = this.filePathToBase64(res.tempFilePath).length
            })
            compressTime++
          }
          this.tmpFileList.push(tmpFile)
        }, 500) 
    }
}

```

然而这个方法还是存在问题

- 循环压缩非常消耗手机性能, 如果压缩10次的话要花费大量的时间
- 即使压缩了10次, 太大的图片还是不能符合大小要求, 而且图片会严重失真

## 方法三 通过compressImage压缩图片
小程序为我们提供了compressImage的api来压缩图片，其性能比canvas渲染压缩更快，用户等待时间少
```
async function compressImage(imagePath, targetSize=512) {
  let currentBase64 = filePathToBase64(imagePath)
  let repeatTime = 0 // 重复压缩次数
  let compressedPath = imagePath // 压缩后的图片文件路径
  while (currentBase64.length / 1024 > targetSize) {
    await uni.compressImage({
      src: imagePath,
      quality: 50 - 5 * repeatTime,
    })
    .then(res => {
      let [err, data] = res
      currentBase64 = filePathToBase64(data.tempFilePath)
      compressedPath = data.tempFilePath
      console.log(`压缩第${repeatTime + 1}次, 压缩质量为${50 - 5 * repeatTime}%,压缩后大小为: ${currentBase64.length}`)
          })
    if(repeatTime >= 9) {
      console.log('循环次数过多, 强制停止压缩')
      break
    }
    repeatTime++
  }
  console.log(`压缩完成, 当前大小为${currentBase64.length}`)
  return {imagePath: compressedPath, base64: currentBase64}
}
```

这个方法的缺点是，在压缩的过程中会保存临时图片到本地相册，如果压缩了10次就会保存10张临时图片，用户会在相册中看到一大堆重复的图片，严重影响用户体验

但在用户手机上保存临时图片不加限制也不作清理，我觉得这个更多的是微信的锅

## 总结

在小程序上进行前端的图片压缩有各方面的限制和缺点，不要期待小程序对图片压缩的能力，这种事情还是交给后端吧
