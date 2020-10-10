---
title: 使用uniapp在微信小程序中压缩图片
tags: 前端
date: 2020-09-30 14:05:48
---

## 问题产生的背景
需要用微信小程序收集用户的单据, 需要把图片转换为base64发送到服务器保存, 而且base64大小限制在256kb以下

## 微信小程序压缩图片的常规方式
使用原生api, 在用户选择图片的过程中压缩, 通过去掉'original'选项强制用户选择压缩的图片, 而不允许选择原图
```
wx.chooseImage({
  sizeType: ['compressed', 'original'],
  success: res => {
  // ...
  }   
})
```

使用uniapp封装的api, [uniapp为大部分api封装了promise](https://uniapp.dcloud.io/api/README?id=promise-%e5%b0%81%e8%a3%85), 通过then返回一个数组, 分别为错误对象和原始返回数据
```
uni.chooseImage({
  sizeType: ['compressed', 'original'],
}).then(data => {
  let [err, res] = data
  // ...
})
```

这种方式虽然可以大大减少图片的体积到可用的范围, 常规图片可以压缩到70kb左右, 但图片严重失真, 用户的单据中的文字都无法看清!!

因此, 只能让用户上传原图再考虑压缩了

## 使用canvas渲染图片进行压缩
在web端经常使用canvas来对图片进行裁剪, 压缩和转换, 但在小程序上有较多的限制, 但也不妨碍我们来尝试一下

考虑到上下文的强联系性和异步api, 因此以下均使用await方法

### canvas元素
```
<canvas
        canvas-id='canvas' 
        id="canvas"
        :style="{width: canvasWidth + 'px', height: canvasHeight + 'px'}"
        ></canvas>
```

- canvas-id 用于js获取canvas对象
- style 通过内联样式动态改变canvas的宽高
- 除此之外, 还要在canvas外层罩一个view, 定位到屏幕外, 这样渲染的时候就不会被用户看到了

### 渲染canvas
渲染前, 要先获取图片的长宽, 并动态修改canvas的宽高
```
await uni.getImageInfo({
  src: this.tmpFileList[0],
}).then(res => {
  this.canvasWidth = res[1].width
  this.canvasHeight = res[1].height
})
```

获取到图片的长宽之后, 把图片渲染到canvas
```
// 渲染到canvas
const ctx = uni.createCanvasContext('canvas', this)
ctx.drawImage(this.tmpFileList[0], 0, 0, this.canvasWidth, this.canvasHeight)
 // 这个api没有被封装为promise, 手动实现
await new Promise(resolve => {
  ctx.draw(false, () => resolve())
})
```

压缩图片, 并生成临时文件路径
```
uni.canvasToTempFilePath({
  canvasId: 'canvas',
  x: 0,
  y: 0,
  width: this.canvasWidth,
  height: this.canvasHeight,
  destHeight: this.canvasHeight,
  destWidth: this.canvasWidth,
  quality: 0.5, // 在这里调整图片的质量
  fileType: 'jpg',
}, this)
.then(res => {
  res = res[1]
  this.tmpFileList = this.tmpFileList.concat(res.tempFilePath)
  console.log('压缩后的图片信息如下')
  this.filePathToBase64(res.tempFilePath)
})
```

封装base64图片的方法, 可以在压缩前调用这个方法, 观察压缩前后base64大小的变化
```
filePathToBase64(filePath) {
  let base64 = wx.getFileSystemManager().readFileSync(filePath, 'base64')
  let beforeStr = 'data:image/jpeg;base64,'
  console.log(`base64的长度为${base64.length}`)
  return beforeStr + base64
},
```

### 结果
这个压缩方法在微信小程序模拟器上完美运行, 然而在手机上图片压缩后只剩图片的左上角, 也找不到问题出在哪, 所以搞了半天还是不能用, 怪就怪微信兼容性拉胯吧
