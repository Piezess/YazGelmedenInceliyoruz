
<html>
<head>
  <title>Sayacım</title>
  <script type="module">
    // Firebase modüllerini içe aktar
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getFirestore, doc, getDoc, onSnapshot, runTransaction } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-firestore.js";
    import { getAuth, signInAnonymously } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js";

    const firebaseConfig = {
      apiKey: "AIzaSyCODG-aD-lLacZzlsqVBEbcTuOeHJ7Vckc",
      authDomain: "edge-slims-sayaci.firebaseapp.com",
      projectId: "edge-slims-sayaci",
      storageBucket: "edge-slims-sayaci.appspot.com",
      messagingSenderId: "458518659405",
      appId: "1:458518659405:web:9af024ddce781d72e33704",
      measurementId: "G-3M67NFR2TL"
    };

    const app = initializeApp(firebaseConfig);
    const db = getFirestore(app);
    const auth = getAuth(app);

    signInAnonymously(auth).catch(console.error);

    const counterRef = doc(db, "counters", "main");

    // Sayaç dinleyici
    onSnapshot(counterRef, (docSnap) => {
      const data = docSnap.data();
      document.getElementById("counter").textContent = data ? data.count : 0;
    });

    // Sayaç artırıcı
    window.incrementCounter = async function () {
      try {
        await runTransaction(db, async (transaction) => {
          const docSnap = await transaction.get(counterRef);
          const newCount = (docSnap.exists() ? docSnap.data().count : 0) + 1;
          transaction.set(counterRef, { count: newCount });
        });
      } catch (e) {
        console.error("Sayaç artırılamadı:", e);
      }
    };

    // Geri sayım
    const countdownElement = document.getElementById("countdown");
    const targetDate = new Date("2025-12-31T23:59:00");

    function updateCountdown() {
      const now = new Date();
      const diff = targetDate - now;

      if (diff <= 0) {
        document.querySelector(".countdown-container").innerHTML = "<div>Süre doldu!</div>";
        return;
      }

      const days = Math.floor(diff / (1000 * 60 * 60 * 24));
      const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
      const minutes = Math.floor((diff / (1000 * 60)) % 60);
      const seconds = Math.floor((diff / 1000) % 60);

      document.getElementById("days").textContent = days;
      document.getElementById("hours").textContent = hours;
      document.getElementById("minutes").textContent = minutes;
      document.getElementById("seconds").textContent = seconds;
    }

    setInterval(updateCountdown, 1000);
    updateCountdown();
  </script>

  <style>
    body {
      background-color: #f0f0f0;
      margin: 0;
      font-family: Arial, sans-serif;
      text-align: center;
    }

    .banner {
      font-size: 36px;
      font-weight: bold;
      color: red;
      margin-top: 20px;
    }

    .counter {
      font-size: 100px;
      font-weight: bold;
      color: black;
      margin-top: 100px;
    }

    .counter-button {
      font-size: 24px;
      font-weight: bold;
      color: white;
      background-color: red;
      border: none;
      border-radius: 12px;
      padding: 15px 40px;
      margin-top: 30px;
      cursor: pointer;
    }

    .counter-button:hover {
      background-color: darkred;
    }

    .countdown-container {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 40px;
    }

    .time-box {
      background-color: white;
      border: 2px solid #ccc;
      border-radius: 10px;
      padding: 20px;
      width: 200px;  /* Increased width */
      text-align: center;
      box-shadow: 2px 2px 10px rgba(0,0,0,0.1);
    }

    .time-box span {
      font-size: 36px;
      font-weight: bold;
      color: #333;
    }

    .label {
      font-size: 14px;
      margin-top: 5px;
      color: #666;
    }
  </style>
</head>
<body>
  <div class="banner">ADIM ADIM EDGE SLIMS</div>
  <div class="counter" id="counter">0</div>
  <button class="counter-button" onclick="incrementCounter()">TIKLA&amp;ARTTIR</button>
  <div class="countdown-container">
    <div class="time-box"><span id="days">0</span><div class="label">Gün</div></div>
    <div class="time-box"><span id="hours">0</span><div class="label">Saat</div></div>
    <div class="time-box"><span id="minutes">0</span><div class="label">Dakika</div></div>
    <div class="time-box"><span id="seconds">0</span><div class="label">Saniye</div></div>
  </div>
</body>
</html>
