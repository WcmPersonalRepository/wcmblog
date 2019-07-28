# webrtc 入门

## 什么是 webrtc

webrtc 全称（Web Real-Time Communication）web 实时通讯技术，是一个基于标准化的行业性技术，旨在浏览器中引入时事通讯功能，并通过 H5 和 JavaScript 使用这些功能。

## webrtc 能干什么

- 可以和 SIP 终端通讯
- 可以和 jingle 终端通讯
- 可以和 PSTN 建立会话
- 可以与 SIP 和媒体网关建立会话

## webrtc 怎么用

- 1、获取本地媒体流

```js
//媒体约束
const constraints={ audio: true, video: { width: 1280, height: 720 }
//获取本地媒体权限
navigator.getUserMedia(constraints,attachMediaStream,errorCallback)
//获取本地媒体流失败回调
function errorCallback(err){
  console.log("The following error occurred: " + err.name);
}
```

- 2、建立对等连接

```js
//创建对等连接对象
let pc = new RTCPeerConnection()
//创建信令通道：webrtc中未实现信令标准，所以此处需要根据自己的业务情况实现信令通道
let signalingChannel = createSignlingChannel()

pc.onicecandidate = function(e) {
  //向信令服务器发送ice候选项
  signalingChannel.send(
    JSON.stringify({
      candidate: e.candidate
    })
  )
}
```

- 3、将媒体流关联对等连接

```js
//将本地媒体流关联对等连接
function attachMediaStream(stream){
  const audioTrack=stream.getAudioTracks()[0]
  const videoTrack=stream.getVideoTracks()[0]
  pc.addStream(videoTrack)
  pc.addStream(videoTrack)

  //将媒体ID发送给信令服务器发
  signalingChannel.send(JSON.stringify({
    audioTrackId:audioTrack.id,
    videoTrackId:videoTrack.id
  }))
}

```

- 4、交换会话描述

```js
pc.createOffer(function(desc) {
  //设置本地会话描述
  pc.setLocalDescription(desc)
  //将本地会话描述发送给远端
  signalingChannel.send(
    JSON.stringify({
      sdp: desc
    })
  )
})
//处理对等端传来的信令消息
signalingChannel.onmessage = function(msg) {
  let signling = JSON.parse(msg)
  console.log(signling)
  if (signling.sdp) {
    //设置远端会哈描述
    pc.setRemoteDescription(new RTCSessionDescription(signling.sdp))
  }
}
```
