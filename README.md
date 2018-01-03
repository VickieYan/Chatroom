# v-chatRoom
一个简单的聊天室

启动
```
nodemon index.js
```
# socket.io
安装
```
npm install --save socket.io
```
服务器端
```javascript
var io = require('socket.io')
//当用户连接上时
io.on('connection', function(socket) {
   console.log('a user connected') 
   //当用户断开连接时
   socket.on('disconnect', function() {
       console.log('user disconnected')
   })
})
```
客户端
```html
//此处有巨坑
/*当使用了socket.io模块后，socket.io会把以 /socket.io 开头的请求都给截拦了下来，其他的请求再让express处理。*/
<script src="/socket.io/socket.io.js"></script>
```
```javascript
const socket = io()
$('form').submit(function() {
    //发信息
    socket.emit('chat message', $('input').val())
})
```
服务器端
```javascript
io.on('connection', function(socket) {
   console.log('a user connected') 
   //当用户断开连接时
   socket.on('disconnect', function() {
       console.log('user disconnected')
   })
    //收到信息
    socket.on('chat message', function(msg) {
        //广播信息 想让全世界知道
        io.emit('chat message', msg)
    })
})
```
客户端
```javascript
//呈现信息
socket.on('chat message', function(msg) {
    $('ul').append($('<li>').text(msg))
})
```