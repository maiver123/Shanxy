<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>SHANXY Movie Zone - Viewer</title>
  <style>
    body {
      background-color: #e0f7fa;
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
    }
    .login-screen {
      height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      background-color: #b2ebf2;
    }
    .movie-container {
      display: none;
      padding: 20px;
    }
    .movie-card {
      border: 2px solid #0288d1;
      background-color: white;
      margin: 20px;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.2);
    }
    input[type="password"] {
      padding: 10px;
      font-size: 16px;
      border-radius: 5px;
      border: 1px solid #0288d1;
    }
    button {
      margin-top: 10px;
      padding: 10px 20px;
      background-color: #0288d1;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0277bd;
    }
    textarea {
      width: 80%;
      padding: 10px;
      margin-top: 20px;
    }
  </style>
</head>
<body>

<div class="login-screen" id="loginScreen">
  <h2>Welcome to SHANXY 🎥</h2>
  <p>Please enter the user password:</p>
  <input type="password" id="userPassword" placeholder="Enter password..." />
  <button onclick="checkUserPassword()">Enter</button>
</div>

<div class="movie-container" id="movieContainer">
  <h1 style="color: #0288d1;">SHANXY Movie Zone - Viewer</h1>
  <div id="movieList"></div>

  <h3>Request a Movie or Send Feedback 📬</h3>
  <textarea id="feedback" rows="4" placeholder="Type your request or feedback here..."></textarea>
  <br>
  <button onclick="submitFeedback()">Send</button>
</div>

<script>
  const USER_PASSWORD = "USER";

  function checkUserPassword() {
    const entered = document.getElementById("userPassword").value;
    if (entered === USER_PASSWORD) {
      document.getElementById("loginScreen").style.display = "none";
      document.getElementById("movieContainer").style.display = "block";
      loadMovies();
    } else {
      alert("Incorrect password!");
    }
  }

  function loadMovies() {
    const movieList = document.getElementById("movieList");
    const movies = JSON.parse(localStorage.getItem("shanxyMovies") || "[]");

    movies.forEach(movie => {
      const card = document.createElement("div");
      card.className = "movie-card";

      if (movie.type === "link") {
        card.innerHTML = `<h3>${movie.name}</h3><iframe src="${movie.url}" width="100%" height="315" allowfullscreen></iframe>`;
      } else {
        card.innerHTML = `<h3>${movie.name}</h3><video width="100%" height="315" controls src="${movie.url}"></video>`;
      }

      movieList.appendChild(card);
    });
  }

  function submitFeedback() {
    const msg = document.getElementById("feedback").value;
    if (!msg.trim()) return alert("Feedback cannot be empty!");

    const feedbacks = JSON.parse(localStorage.getItem("shanxyFeedback") || "[]");
    feedbacks.push({ message: msg, time: new Date().toLocaleString() });
    localStorage.setItem("shanxyFeedback", JSON.stringify(feedbacks));

    alert("Your message has been sent!");
    document.getElementById("feedback").value = "";
  }
</script>

</body>
</html>
