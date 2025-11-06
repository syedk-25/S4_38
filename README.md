# S4_38
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>CyberAware - Phishing Detection & Training</title>
  <style>
    body {
      font-family: "Poppins", sans-serif;
      margin: 0;
      padding: 0;
      background: #f3f6fa;
      color: #222;
    }
    header {
      background: #0a2540;
      color: white;
      padding: 20px 0;
      text-align: center;
    }
    nav {
      background: #133b63;
      display: flex;
      justify-content: center;
      gap: 25px;
      padding: 10px 0;
    }
    nav a {
      color: white;
      text-decoration: none;
      font-weight: 500;
    }
    nav a:hover {
      color: #00d4ff;
    }
    section {
      padding: 40px 10%;
    }
    h1, h2 {
      color: #0a2540;
    }
    .btn {
      background: #007bff;
      color: white;
      border: none;
      padding: 10px 20px;
      cursor: pointer;
      border-radius: 5px;
      margin-top: 10px;
    }
    .btn:hover {
      background: #0056b3;
    }
    .card {
      background: white;
      border-radius: 10px;
      padding: 25px;
      margin-top: 20px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }
    footer {
      background: #0a2540;
      color: white;
      text-align: center;
      padding: 15px;
      margin-top: 40px;
    }
    input, textarea {
      width: 100%;
      padding: 10px;
      margin-top: 8px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .chart-placeholder {
      height: 200px;
      background: linear-gradient(135deg, #007bff22, #00d4ff33);
      border-radius: 10px;
      text-align: center;
      line-height: 200px;
      font-weight: bold;
      color: #0a2540;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <header>
    <h1>CyberAware</h1>
    <p>Integrated Phishing Detection & Adaptive Awareness System</p>
  </header>

  <nav>
    <a href="#home">Home</a>
    <a href="#detect">Threat Detection</a>
    <a href="#training">Adaptive Training</a>
    <a href="#dashboard">Dashboard</a>
    <a href="#contact">Contact</a>
  </nav>

  <section id="home">
    <h2>Welcome to CyberAware</h2>
    <p>
      CyberAware helps organizations detect phishing threats automatically and provide 
      personalized awareness training based on user interactions. 
      This integrated system bridges the gap between technical detection and user education.
    </p>
    <img src="https://cdn-icons-png.flaticon.com/512/619/619034.png" alt="Cybersecurity" width="200" />
  </section>

  <section id="detect">
    <h2>Phishing Threat Detection</h2>
    <div class="card">
      <p>Paste email content or suspicious link below to simulate detection:</p>
      <textarea id="emailInput" rows="5" placeholder="Paste suspicious email or link here..."></textarea>
      <button class="btn" onclick="detectPhishing()">Analyze</button>
      <p id="detectionResult"></p>
    </div>
  </section>

  <section id="training">
    <h2>Adaptive Awareness Training</h2>
    <div class="card">
      <p>Based on your last interaction, your next training topic is:</p>
      <h3 id="trainingTopic">Recognizing Suspicious Links</h3>
      <button class="btn" onclick="changeTraining()">Next Topic</button>
    </div>
  </section>

  <section id="dashboard">
    <h2>Analytics Dashboard</h2>
    <div class="card">
      <p>Overview of phishing detection and awareness trends:</p>
      <div class="chart-placeholder">📊 Graph Placeholder</div>
    </div>
  </section>

  <section id="contact">
    <h2>Contact Us</h2>
    <div class="card">
      <p>Get in touch for partnerships or demo requests:</p>
      <input type="text" placeholder="Your Name" />
      <input type="email" placeholder="Your Email" />
      <textarea rows="4" placeholder="Your Message"></textarea>
      <button class="btn">Submit</button>
    </div>
  </section>

  <footer>
    <p>© 2025 CyberAware | Secure. Educate. Empower.</p>
  </footer>

  <script>
    function detectPhishing() {
      const text = document.getElementById("emailInput").value.toLowerCase();
      const result = document.getElementById("detectionResult");

      if (!text) {
        result.innerHTML = "⚠️ Please enter some text to analyze.";
        result.style.color = "orange";
        return;
      }

      const suspiciousWords = ["urgent", "verify", "login", "password", "bank", "click here", "confirm"];
      const found = suspiciousWords.filter(word => text.includes(word));
      
      if (found.length > 0) {
        result.innerHTML = "🚨 Potential phishing detected! Keywords found: " + found.join(", ");
        result.style.color = "red";
      } else {
        result.innerHTML = "✅ No major phishing indicators found.";
        result.style.color = "green";
      }
    }

    const topics = [
      "Recognizing Suspicious Links",
      "Identifying Fake Domains",
      "Avoiding Credential Theft",
      "Reporting Phishing Emails",
      "Safe Password Practices"
    ];
    let current = 0;

    function changeTraining() {
      current = (current + 1) % topics.length;
      document.getElementById("trainingTopic").innerText = topics[current];
    }
  </script>
</body>
</html>

