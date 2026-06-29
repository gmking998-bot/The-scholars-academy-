# The-scholars-academy-
<!DOCTYPE html>
<html>
<head>
<title>Cloud ERP System</title>
<meta name="viewport" content="width=device-width, initial-scale=1">

<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js"></script>

<style>
body{font-family:Arial;margin:0;background:#f4f6f9}
.box{max-width:400px;margin:80px auto;background:#fff;padding:20px;border-radius:10px}
input,button{width:100%;padding:10px;margin:5px 0}
button{background:#0b1a3e;color:#fff;border:none}
#app{display:none;padding:15px}
.card{background:#fff;padding:15px;margin:10px 0;border-radius:10px}
</style>
</head>

<body>

<!-- LOGIN -->
<div id="login" class="box">
<h3>Cloud ERP Login</h3>
<input id="email" placeholder="Email">
<input id="pass" type="password" placeholder="Password">
<button onclick="login()">Login</button>
<button onclick="signup()">Create Account</button>
</div>

<!-- APP -->
<div id="app">

<h2>📚 Academy Cloud ERP</h2>
<button onclick="logout()">Logout</button>

<div class="card">
<h3>Add Student</h3>
<input id="name" placeholder="Name">
<input id="cls" placeholder="Class">
<input id="fee" placeholder="Fee">
<button onclick="addStudent()">Save</button>
</div>

<div class="card">
<h3>Students</h3>
<div id="list"></div>
</div>

</div>

<script>

/* ── FIREBASE CONFIG (REPLACE THIS) ── */
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
};

firebase.initializeApp(firebaseConfig);

const auth = firebase.auth();
const db = firebase.firestore();

/* ── AUTH ── */
function signup(){
auth.createUserWithEmailAndPassword(email.value, pass.value)
.then(()=>alert("Account created"));
}

function login(){
auth.signInWithEmailAndPassword(email.value, pass.value)
.then(()=>document.getElementById("login").style.display="none")
.then(()=>document.getElementById("app").style.display="block")
.then(load);
}

function logout(){
auth.signOut().then(()=>location.reload());
}

/* ── STUDENTS CLOUD ── */
function addStudent(){
db.collection("students").add({
name:name.value,
cls:cls.value,
fee:fee.value,
status:"Unpaid",
time:Date.now()
});
load();
}

function load(){
db.collection("students").onSnapshot(snapshot=>{
let html="";
snapshot.forEach(doc=>{
let d=doc.data();

html+=`
<div class="card">
<b>${d.name}</b><br>
Class: ${d.cls}<br>
Fee: ${d.fee}<br>
Status: ${d.status}
</div>`;
});

list.innerHTML=html;
});
}

</script>

</body>
</html>
