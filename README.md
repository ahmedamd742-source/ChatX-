<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>ChatX</title>

<style>
body {
  margin:0;
  background:#0B0F14;
  color:white;
  font-family:Arial;
}
header {
  padding:15px;
  text-align:center;
  background:#111827;
  font-size:20px;
}
#chat {
  height:70vh;
  overflow:auto;
  padding:10px;
}
.msg {
  margin:5px;
  padding:10px;
  border-radius:10px;
}
.me {background:#00bcd4;color:black;margin-left:auto;}
.other {background:#1f2937;}
#inputBox {
  position:fixed;
  bottom:0;
  width:100%;
  display:flex;
}
input {
  flex:1;
  padding:10px;
}
button {
  padding:10px;
  background:#00bcd4;
  border:none;
}
</style>
</head>

<body>

<header>💬 ChatX</header>

<div id="chat">Loading...</div>

<div id="inputBox">
<input id="msg" placeholder="Type message...">
<button onclick="send()">Send</button>
</div>

<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-firestore-compat.js"></script>

<script>
try {

const firebaseConfig = {
  apiKey: "AIzaSyA3o3CHKaORvCNlNpUCIU2MmaeVM6w_coo",
  authDomain: "chatx-781cf.firebaseapp.com",
  projectId: "chatx-781cf"
};

firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

const user = "User_" + Math.floor(Math.random()*10000);

function send(){
  let text = document.getElementById("msg").value;
  if(!text) return;

  db.collection("messages").add({
    text:text,
    user:user,
    time:Date.now()
  });

  document.getElementById("msg").value="";
}

db.collection("messages").orderBy("time")
.onSnapshot(snap=>{
  let chat = document.getElementById("chat");
  chat.innerHTML="";

  snap.forEach(doc=>{
    let d = doc.data();
    let div = document.createElement("div");

    if(d.user===user){
      div.style.background="#00bcd4";
      div.style.color="black";
      div.style.margin="5px";
      div.style.padding="10px";
      div.style.borderRadius="10px";
      div.style.textAlign="right";
    }else{
      div.style.background="#1f2937";
      div.style.margin="5px";
      div.style.padding="10px";
      div.style.borderRadius="10px";
    }

    div.innerHTML = "<b>"+d.user+"</b><br>"+d.text;
    chat.appendChild(div);
  });

  chat.scrollTop = chat.scrollHeight;
});

} catch(e) {
  document.getElementById("chat").innerHTML = "Error: " + e;
}
</script>

</body>
</html>
