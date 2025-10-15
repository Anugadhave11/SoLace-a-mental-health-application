# SOLACE – Mental Health Support

### 📘 Project Overview
This repository contains the final year project report for the web-based application **“SOLACE”**, designed to promote mental health awareness and provide accessible self-assessment tools.

### 🧑‍💻 Team 

- **Anushka Avinash Gadhave** – MITU23BTCSD013  
  

### 🎓 Guide
Prof. Dr. Rajani Sajjan  
MIT School of Computing, MIT-ADT University, Pune

### 🧩 Technologies Used
- HTML5, CSS3, JavaScript, Bootstrap
- Firebase (Authentication & Database)
- Visual Studio Code

### 🧠 Abstract
> Solace is a web-based mental health platform that offers guided self-assessment, mood tracking, and therapeutic content to help users understand their emotional state and access supportive resources.

---

### **Step 5 (Optional): Add Future Source Code**
If you later want to include your project’s **source code**, create folders like:

Solace-MentalHealthApp/
│
├── index.html
├── style.css
├── script.js
├── firebase.js
├── manifest.json
├── /assets/
│   ├── logo.png
│   ├── mood-icons/
│   └── videos/
└── README.md
INDEX.Html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Solace – Mental Health Support</title>
  <link rel="stylesheet" href="style.css" />
  <script defer src="firebase.js"></script>
  <script defer src="script.js"></script>
</head>

<body>
  <header>
    <h1>🌿 SOLACE</h1>
    <p>Your companion for better mental health</p>
  </header>

  <main id="app">
    <section id="auth-section">
      <h2>Sign In</h2>
      <input type="email" id="email" placeholder="Email" />
      <input type="password" id="password" placeholder="Password" />
      <button id="loginBtn">Login</button>
      <p>New user? <a id="signupLink">Sign up here</a></p>
    </section>

    <section id="dashboard" class="hidden">
      <h2>Welcome to Solace</h2>

      <div class="card">
        <h3>Mood Tracker</h3>
        <div class="mood-options">
          <button class="mood" data-mood="😊">😊</button>
          <button class="mood" data-mood="😐">😐</button>
          <button class="mood" data-mood="😔">😔</button>
        </div>
        <p id="moodMessage"></p>
      </div>

      <div class="card">
        <h3>Journal</h3>
        <textarea id="journalEntry" placeholder="Write your thoughts..."></textarea>
        <button id="saveJournal">Save Entry</button>
        <div id="journalList"></div>
      </div>

      <div class="card">
        <h3>Daily Affirmation</h3>
        <p id="affirmation">You are stronger than you think 💪</p>
        <button id="newAffirmation">Next</button>
      </div>

      <button id="logoutBtn" class="logout">Logout</button>
    </section>
  </main>

  <footer>
    <p>© 2025 Solace | Developed by MIT-ADT Students</p>
  </footer>
</body>
</html>

Style.css
body {
  font-family: 'Poppins', sans-serif;
  margin: 0;
  background: #f5f9f6;
  color: #333;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}
header {
  text-align: center;
  background: #a4c3b2;
  color: #fff;
  padding: 20px;
}
h1 { margin: 0; }

main {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 20px;
}
section {
  background: #fff;
  border-radius: 10px;
  padding: 25px;
  width: 100%;
  max-width: 500px;
  box-shadow: 0 3px 8px rgba(0,0,0,0.1);
}
.hidden { display: none; }
input, textarea, button {
  width: 100%;
  padding: 10px;
  margin: 8px 0;
  border: 1px solid #ccc;
  border-radius: 6px;
}
button {
  background: #a4c3b2;
  color: white;
  border: none;
  cursor: pointer;
}
button:hover {
  background: #7aa890;
}
.card {
  margin-top: 20px;
  background: #f9fdfb;
  border-radius: 8px;
  padding: 15px;
}
footer {
  text-align: center;
  padding: 10px;
  background: #a4c3b2;
  color: #fff;
}
Firebase

// Firebase Configuration
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

// Initialize Firebase
firebase.initializeApp(firebaseConfig);
const auth = firebase.auth();
const db = firebase.firestore();

const authSection = document.getElementById('auth-section');
const dashboard = document.getElementById('dashboard');
const loginBtn = document.getElementById('loginBtn');
const logoutBtn = document.getElementById('logoutBtn');
const signupLink = document.getElementById('signupLink');

const affirmationList = [
  "You are capable of amazing things 💫",
  "Every day is a fresh start 🌅",
  "Your feelings are valid ❤️",
  "Keep going, you are doing great 🌼",
];

let affirmationIndex = 0;
document.getElementById('newAffirmation').addEventListener('click', () => {
  affirmationIndex = (affirmationIndex + 1) % affirmationList.length;
  document.getElementById('affirmation').textContent = affirmationList[affirmationIndex];
});

signupLink.addEventListener('click', () => {
  const email = prompt("Enter your email:");
  const password = prompt("Create a password:");
  auth.createUserWithEmailAndPassword(email, password)
      .then(() => alert("Account created! Please login."))
      .catch(err => alert(err.message));
});

loginBtn.addEventListener('click', () => {
  const email = document.getElementById('email').value;
  const password = document.getElementById('password').value;
  auth.signInWithEmailAndPassword(email, password)
      .then(() => {
        authSection.classList.add('hidden');
        dashboard.classList.remove('hidden');
      })
      .catch(err => alert(err.message));
});

logoutBtn.addEventListener('click', () => {
  auth.signOut();
  dashboard.classList.add('hidden');
  authSection.classList.remove('hidden');
});

document.querySelectorAll('.mood').forEach(btn => {
  btn.addEventListener('click', () => {
    const mood = btn.dataset.mood;
    document.getElementById('moodMessage').textContent = `You selected ${mood}. Stay positive!`;
  });
});

document.getElementById('saveJournal').addEventListener('click', () => {
  const entry = document.getElementById('journalEntry').value;
  if (!entry) return alert("Write something first!");
  const div = document.createElement('div');
  div.textContent = `📝 ${new Date().toLocaleString()}: ${entry}`;
  document.getElementById('journalList').prepend(div);
  document.getElementById('journalEntry').value = '';
});

Final REadme.md

# 🌿 SOLACE – Mental Health Support

A web-based application that helps users check their emotional well-being, track moods, write journals, and get daily affirmations.

## 🔧 Built With
- HTML5, CSS3, JavaScript
- Firebase Authentication & Firestore
- Bootstrap (optional for styling)

## 🧠 Features
- Secure Login / Signup
- Mood Tracker with Emoji Options
- Personal Journal with Auto Timestamp
- Daily Affirmation Generator
- Firebase Integration

## 🚀 How to Run
1. Clone this repo:
   ```bash
   git clone https://github.com/YOUR_USERNAME/Solace-MentalHealthApp.git




