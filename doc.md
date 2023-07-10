- [Vue与Flutter交互](#vue与flutter交互)
- [请求Api](#请求api)
  - [1. 打开相册](#1-打开相册)
  - [2. 打开相机](#2-打开相机)
  - [3. 选择联系人](#3-选择联系人)
  - [4. 扫描二维码](#4-扫描二维码)
  - [5. 选择文件](#5-选择文件)
- [Flutter向Vue交互](#flutter向vue交互)
  - [1. 发起](#1-发起)
  - [2. 接收](#2-接收)

# Vue与Flutter交互
```javascript
// 使用postMessage向Flutter端发起交互
sendFlutterChannel: function(){
    flutterJsChannel.postMessage('{"type":"SelectPhotoChannel","data":"给Flutter的参数,没有可不传"}')
}

// 接收Flutter交互
flutterChannel: function(data) {
 // {"type": "SelectPhotoChannel","data": "json","msg":"ok"}
}

mounted() {
	// 挂载到window,否则Flutter端无法执行
	window.flutterChannel = this.flutterChannel
	window.sendFlutterChannel = this.sendFlutterChannel
}

```

# 请求Api
> 通过js的postMessage进行交互

## 1. 打开相册
```javascript
// 请求
flutterJsChannel.postMessage('{"type":"SelectPhotoChannel","data":""}')

// 响应
{
    "type": "SelectPhotoChannel",
    "data": {
        "path": "/file:///system/xxx/xxx/xx.jpg",
        "fileName": "xx.jpg",
        "mimeType": "image/jpeg",
        "size": "465465",
        "sizeUnit": "字节"
    },
    "msg": "ok"
}
```

## 2. 打开相机

```javascript
// 请求
flutterJsChannel.postMessage('{"type":"OpenCameraChannel","data":""}')

// 响应
{
    "type": "OpenCameraChannel",
    "data": {
        "path": "/file:///system/xxx/xxx/xx.jpg",
        "fileName": "xx.jpg",
        "mimeType": "image/jpeg",
        "size": "465465",
        "sizeUnit": "字节"
    },
    "msg": "ok"
}
```

## 3. 选择联系人

```javascript
// 选择联系人
flutterJsChannel.postMessage('{"type":"SelectPeopleChannel","data":""}')

// 响应选择联系人
{
    "type": "SelectPeopleChannel",
    "data": {
        "depts": [],
        "users": []
    },
    "msg": "ok"
}
```

## 4. 扫描二维码

```javascript
// 扫描二维码
flutterJsChannel.postMessage('{"type":"ScanCodeChannel","data":""}')

// 响应
{
    "type": "ScanCodeChannel",
    "data": "二维码内容",
    "msg": "ok"
}
```

## 5. 选择文件

```javascript
// 请求
flutterJsChannel.postMessage('{"type":"SelectFileChannel","data":""}')

// 响应
{
    "type": "SelectFileChannel",
    "data": {
        "path": "/file:///system/xxx/xxx/嘿嘿嘿.mp4",
        "fileName": "嘿嘿嘿.mp4",
        "mimeType": "video/mp4",
        "size": "465465",
        "sizeUnit": "字节"
    },
    "msg": "ok"
}
```

更多API 待拓展

---

# Flutter向Vue交互

## 1. 发起
> 调用Webview的**runJavascriptReturningResult**('window.flutterChannel(传参)')向web发出交互

## 2. 接收
> 在Webview的**onMessageReceived**中接收Web端**postMessage**的交互请求

