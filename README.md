<!DOCTYPE html>
<html>
<head>
<title>মাল্টিপল মেসেঞ্জার</title>
<style>
  .chat-box-container {
    width: 230px;
    height: 380px;
    border: 1px solid #ccc;
    /* margin: 10px; */
    margin-left: 4px;
    float: left; /* পাশাপাশি চ্যাট বক্স দেখানোর জন্য */
  }
  .chat-header {
    background-color: #f0f0f0;
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: 1px solid #ccc;
  }
  .chat-header h2 {
    margin: 0;
  }
  .chat-header button {
    background: none;
    border: none;
    font-size: 1.2em;
    cursor: pointer;
  }
  .chat-box {
    height: 280px;
    overflow-y: auto;
    display: flex;
    flex-direction: column-reverse;
    align-items: flex-end;
    padding: 10px;
  }
  .message-input-container {
   
    display: flex;
    margin-top: 4rem;
  }
  .message-input {
    flex-grow: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  .send-button, .love-button {
    padding: 8px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 5px;
  }
  .send-button {
    background-color: #007bff;
    color: white;
  }
  .love-button {
    background-color: #0fa0ee;
    color: white;
  }
  .sent-message {
    text-align: right;
    margin-left: auto;
    background-color: #dcf8c6;
    padding: 8px 12px;
    border-radius: 8px;
    margin-bottom: 5px;
    max-width: 80%;
    word-wrap: break-word;
  }
  .sent-message small {
    display: block;
    font-size: 0.7em;
    color: #666;
    margin-top: 3px;
  }
</style>
</head>
<body>

<div class="chat-box-container" id="chat-user1">
  <div class="chat-header">
    <h2>My Love</h2>
    <div>
      <button>&#x1F4F9;</button> <button>&#x1F4DE;</button>
    </div>
  </div>
  <div class="chat-box"></div>
  <div class="message-input-container">
    <input type="text" class="message-input" placeholder="Enter your message...">
    <button class="send-button" onclick="sendMessage('user1')">send</button>
    <button class="love-button" onclick="sendLove('user1')">🖤</button>
  </div>
</div>



<script>
  window.onload = function() {
    loadMessages('user1');
    loadMessages('user2');
    setupInputListeners('user1');
    setupInputListeners('user2');
  };
  
  function sendMessage(userId) {
    var messageInput = document.querySelector(`#chat-${userId} .message-input`);
    var message = messageInput.value.trim();
    
    if (message === "") {
      return;
    }

    var chatBox = document.querySelector(`#chat-${userId} .chat-box`);
    var timestamp = new Date().toLocaleTimeString();
    
    var newMessage = {
      text: message,
      time: timestamp
    };

    var storageKey = `chatMessages_${userId}`;
    var messages = JSON.parse(localStorage.getItem(storageKey)) || [];
    messages.unshift(newMessage);
    localStorage.setItem(storageKey, JSON.stringify(messages));

    var p = document.createElement("p");
    p.classList.add("sent-message");
    p.innerHTML = message + "<br><small>" + timestamp + "</small>";
    chatBox.prepend(p);

    messageInput.value = "";
    
    if (messageInput.value.trim() === "") {
      document.querySelector(`#chat-${userId} .send-button`).style.display = "none";
      document.querySelector(`#chat-${userId} .love-button`).style.display = "inline-block";
    }
  }
  
  function sendLove(userId) {
    var chatBox = document.querySelector(`#chat-${userId} .chat-box`);
    var timestamp = new Date().toLocaleTimeString();
    
    var newMessage = {
      text: "🖤",
      time: timestamp
    };

    var storageKey = `chatMessages_${userId}`;
    var messages = JSON.parse(localStorage.getItem(storageKey)) || [];
    messages.unshift(newMessage);
    localStorage.setItem(storageKey, JSON.stringify(messages));

    var p = document.createElement("p");
    p.classList.add("sent-message");
    p.innerHTML = "🖤" + "<br><small>" + timestamp + "</small>";
    chatBox.prepend(p);
  }

  function loadMessages(userId) {
    var chatBox = document.querySelector(`#chat-${userId} .chat-box`);
    var storageKey =` chatMessages_${userId}`;
    var messages = JSON.parse(localStorage.getItem(storageKey)) || [];
    
    messages.forEach(function(msg) {
      var p = document.createElement("p");
      p.classList.add("sent-message");
      p.innerHTML = msg.text + "<br><small>" + msg.time + "</small>";
      chatBox.append(p);
    });
  }
  
  function setupInputListeners(userId) {
    var messageInput = document.querySelector(`#chat-${userId} .message-input`);
    var sendButton = document.querySelector(`#chat-${userId} .send-button`);
    var loveButton = document.querySelector(`#chat-${userId} .love-button`);

    messageInput.addEventListener("keyup", function(event) {
      if (event.keyCode === 13) {
        sendMessage(userId);
      }
    });
    
    messageInput.addEventListener("input", function() {
      if (this.value.trim() !== "") {
        sendButton.style.display = "inline-block";
        loveButton.style.display = "none";
      } else {
        sendButton.style.display = "none";
        loveButton.style.display = "inline-block";
      }
    });

    // Initial setup for buttons
    if (messageInput.value.trim() === "") {
      sendButton.style.display = "none";
    } else {
      loveButton.style.display = "none";
    }
  }
</script>

</body>
</html> 
