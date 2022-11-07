# WebSockets

> Basado en el [video](https://www.youtube.com/watch?v=jD7FnbI76Hg) y el [repositorio](https://github.com/bradtraversy/chatcord/blob/master/public/js/main.js) de Traversy Media.

Los websockets son un modo de comunicación entre servidor y cliente. Ofrecen **comunicación bidireccional** y permiten un flujo de datos en **tiempo real**. La transferencia de datos ocurre sin usar ningún tipo de requests.



Vamos a implementar un chat en tiempo real con salas privadas.



Utilizamos la biblioteca socket.io tanto en el frontend como en el backend

```
npm install socket.io
```



## Lado Servidor

Creamos un servidor express y queremos servir los archivos ubicados dentro de la carpeta `public` de modo que cuando desde el navegador entremos en `localhost:3000` veremos el contenido de `index.html`.

En este caso queremos enviar un mensaje ante distintos eventos generados por el cliente `joinRoom`, `chatMessage`, `disconnect`. El concepto de room nos permite unirnos `socket.join(user.room)` y enviar mensajes a todos los clientes que formen parte de esa sala.

- `socket.emit` envía un mensaje sólo al emisor
- `io.emit` envía un mensaje a todos los clientes incluyendo al emisor
- `socket.broadcast.emit` envía un mensaje a todos los clientes excepto el emisor

```jsx
const path = require('path');
const http = require('http');
const express = require('express');
const socketio = require('socket.io');

const app = express();
const server = http.createServer(app);
const io = socketio(server);

// Set static folder
app.use(express.static(path.join(__dirname, 'public')));

const botName = 'ChatCord Bot';

// Run when client connects
io.on('connection', socket => {
  socket.on('joinRoom', ({ username, room }) => {
    const user = userJoin(socket.id, username, room);

    socket.join(user.room);

    // Welcome current user
    socket.emit('message', formatMessage(botName, 'Welcome to ChatCord!'));

    // Broadcast when a user connects
    socket.broadcast
      .to(user.room)
      .emit(
        'message',
        formatMessage(botName, `${user.username} has joined the chat`)
      );

    // Send users and room info
    io.to(user.room).emit('roomUsers', {
      room: user.room,
      users: getRoomUsers(user.room)
    });
  });

  // Listen for chatMessage
  socket.on('chatMessage', msg => {
    const user = getCurrentUser(socket.id);

    io.to(user.room).emit('message', formatMessage(user.username, msg));
  });

  // Runs when client disconnects
  socket.on('disconnect', () => {
    const user = userLeave(socket.id);

    if (user) {
      io.to(user.room).emit(
        'message',
        formatMessage(botName, `${user.username} has left the chat`)
      );

      // Send users and room info
      io.to(user.room).emit('roomUsers', {
        room: user.room,
        users: getRoomUsers(user.room)
      });
    }
  });
});

const PORT = process.env.PORT || 3000;

server.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

> Notar que a diferencia de `app.listen()` estamos utilizando `server.listen()`



## Lado Cliente

Luego en el cliente por ejemplo en `index.html` importamos el siguiente script

```jsx
<script src="/socket.io/socket.io.js"></script>
```

Con lo cual tendremos acceso a `const socket = io();`



Con `socket.emit('joinRoom', { username, room });` emitimos un mensaje de tipo `joinRoom` que será recibido por el servidor.

Con `socket.on('message', (message) => {});` obtenemos un mensaje `message` del servidor.

```jsx

const chatForm = document.getElementById('chat-form');
const chatMessages = document.querySelector('.chat-messages');
const roomName = document.getElementById('room-name');
const userList = document.getElementById('users');

// Get username and room from URL
const { username, room } = Qs.parse(location.search, {
  ignoreQueryPrefix: true,
});

const socket = io();

// Join chatroom
socket.emit('joinRoom', { username, room });

// Get room and users
socket.on('roomUsers', ({ room, users }) => {
  outputRoomName(room);
  outputUsers(users);
});

// Message from server
socket.on('message', (message) => {
  console.log(message);
  outputMessage(message);

  // Scroll down
  chatMessages.scrollTop = chatMessages.scrollHeight;
});

// Message submit
chatForm.addEventListener('submit', (e) => {
  e.preventDefault();

  // Get message text
  let msg = e.target.elements.msg.value;

  msg = msg.trim();

  if (!msg) {
    return false;
  }

  // Emit message to server
  socket.emit('chatMessage', msg);

  // Clear input
  e.target.elements.msg.value = '';
  e.target.elements.msg.focus();
});

// Output message to DOM
function outputMessage(message) {
  const div = document.createElement('div');
  div.classList.add('message');
  const p = document.createElement('p');
  p.classList.add('meta');
  p.innerText = message.username;
  p.innerHTML += `<span>${message.time}</span>`;
  div.appendChild(p);
  const para = document.createElement('p');
  para.classList.add('text');
  para.innerText = message.text;
  div.appendChild(para);
  document.querySelector('.chat-messages').appendChild(div);
}

// Add room name to DOM
function outputRoomName(room) {
  roomName.innerText = room;
}

// Add users to DOM
function outputUsers(users) {
  userList.innerHTML = '';
  users.forEach((user) => {
    const li = document.createElement('li');
    li.innerText = user.username;
    userList.appendChild(li);
  });
}

//Prompt the user before leave chat room
document.getElementById('leave-btn').addEventListener('click', () => {
  const leaveRoom = confirm('Are you sure you want to leave the chatroom?');
  if (leaveRoom) {
    window.location = '../index.html';
  } else {
  }
});
```

