var app = require('express').createServer();
var io = require('socket.io').listen(app);

app.listen(8888);

//routing
app.get('/', function(req, res) {
  res.sendfile(__dirname + '/index.html');
});

// usernames for those connected to the chat
var usernames = {};

io.sockets.on('connection', function (socket) {

  //Listens on sendchat
  socket.on('sendchat', function (data) {
    // we tell the client to execute 'updatechat' with 2 params
    io.sockets.emit('updatechat', socket.username, data);
  });

  // when the client emits 'adduser', this listens and executes
  socket.on('adduser', function(username){
    socket.username = username;
    usernames[username] = username;
    socket.emit('updatechat', 'SERVER', 'you have connected');
    socket.broadcast.emit('updatechat', 'SERVER', username + ' has connected');
    io.sockets.emit('updateusers', usernames);
  });

  // when the user disconnects
  socket.on('disconnect', function(){
    delete usernames[socket.username];
    io.sockets.emit('updateusers', usernames);
    socket.broadcast.emit('updatechat', 'SERVER', socket.username + ' has disconnected');
  });

});

