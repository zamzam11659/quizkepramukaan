# quizkepramukaan[Bola Petualang 3D.html](https://github.com/user-attachments/files/23286040/Bola.Petualang.3D.html)
<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Kuis Pramuka Dunia & Indonesia</title>
<style>
  body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: url('https://i.ibb.co/2S8DgXz/scout-bg.jpg') no-repeat center center fixed;
    background-size: cover;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: flex-start;
    min-height: 100vh;
    color: #fff;
    overflow-x: hidden;
  }

  .overlay {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.6);
    z-index: -1;
  }

  .quiz-container {
    background: rgba(0,0,0,0.8);
    padding: 30px 40px;
    border-radius: 15px;
    margin-top: 50px;
    width: 90%;
    max-width: 700px;
    text-align: center;
    box-shadow: 0 8px 20px rgba(0,0,0,0.5);
    animation: fadeIn 1s ease;
  }

  h2 {
    margin-bottom: 20px;
    font-size: 24px;
  }

  button.choice-btn {
    display: block;
    width: 100%;
    margin: 12px 0;
    padding: 14px;
    font-size: 18px;
    border: none;
    border-radius: 8px;
    background: #3498db;
    color: #fff;
    cursor: pointer;
    transition: all 0.3s ease;
  }

  button.choice-btn:hover {
    background: #2980b9;
    transform: scale(1.05);
  }

  #score {
    font-size: 20px;
    margin-top: 20px;
  }

  .creator {
    margin-top: 20px;
    font-size: 14px;
    color: #ccc;
  }

  .audio-controls {
    margin-top: 15px;
  }

  button#audio-toggle {
    background: #e67e22;
    border: none;
    padding: 10px 20px;
    border-radius: 8px;
    font-size: 14px;
    color: #fff;
    cursor: pointer;
    transition: 0.3s;
  }

  button#audio-toggle:hover {
    background: #d35400;
    transform: scale(1.05);
  }

  @keyframes fadeIn {
    from {opacity: 0; transform: translateY(-20px);}
    to {opacity:1; transform: translateY(0);}
  }

  /* Animasi jawaban benar/salah */
  .correct {
    background-color: #27ae60 !important;
  }

  .wrong {
    background-color: #c0392b !important;
  }

</style>
</head>
<body>

<div class="overlay"></div>

<div class="quiz-container">
  <h2 id="question">Pertanyaan dimuat …</h2>
  <div id="choices"></div>
  <div id="score">Skor: 0 dari 0</div>

  <div class="audio-controls">
    <button id="audio-toggle">Mute/Unmute Musik</button>
  </div>

  <div class="creator">Dibuat oleh: [Zam Zam Syaripatuloh]</div>
</div>

<audio id="bg-audio" autoplay loop>
  <source src="https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" type="audio/mpeg">
  Browser Anda tidak mendukung audio.
</audio>

<script>
  const questions = [
    { question: "Siapakah pendiri gerakan Pramuka dunia?", choices: ["Robert Baden‑Powell", "William Boyce", "Agnes Baden‑Powell", "Lord Mountbatten"], answer: "Robert Baden‑Powell" },
    { question: "Kapan gerakan Pramuka di Indonesia secara resmi dibentuk sebagai organisasi tunggal?", choices: ["1951", "1961", "1945", "1970"], answer: "1961" },
    { question: "Apa motto Pramuka Indonesia?", choices: ["Selalu Siap", "Be Prepared", "Selalu Aktif", "Selalu Bersatu"], answer: "Selalu Siap" },
    { question: "Apa lambang resmi Pramuka Indonesia?", choices: ["Tunas Kelapa", "Bunga Melati", "Garuda Pancasila", "Bintang Kejora"], answer: "Tunas Kelapa" },
    { question: "Siapakah yang menulis buku 'Scouting for Boys'?", choices: ["Robert Baden‑Powell", "Lord Baden", "William Boyce", "Daniel Carter"], answer: "Robert Baden‑Powell" },
    { question: "Di negara mana gerakan Scout pertama kali lahir?", choices: ["Inggris", "Amerika", "Jerman", "Prancis"], answer: "Inggris" },
    { question: "Kapan Hari Pramuka diperingati di Indonesia?", choices: ["14 Agustus", "17 Agustus", "1 Januari", "22 November"], answer: "14 Agustus" },
    { question: "Apa arti Tunas Kelapa dalam lambang Pramuka?", choices: ["Pertumbuhan dan Kesatuan", "Keberanian", "Kemandirian", "Persahabatan"], answer: "Pertumbuhan dan Kesatuan" },
    { question: "Gerakan Pramuka di Indonesia termasuk Golongan?", choices: ["Kwartir Nasional", "Scout Troop", "Boy Scout", "Girl Guide"], answer: "Kwartir Nasional" },
    { question: "Apa warna seragam utama Pramuka Indonesia?", choices: ["Cokelat", "Hijau", "Biru", "Hitam"], answer: "Cokelat" },
    // Tambahkan pertanyaan lainnya hingga 50
  ];

  let currentQuestion = 0;
  let score = 0;
  const audio = document.getElementById('bg-audio');

  function loadQuestion() {
    const q = questions[currentQuestion];
    document.getElementById("question").innerText = q.question;

    const choicesDiv = document.getElementById("choices");
    choicesDiv.innerHTML = "";
    q.choices.forEach(choice => {
      const btn = document.createElement("button");
      btn.innerText = choice;
      btn.classList.add("choice-btn");
      btn.onclick = () => selectAnswer(btn, choice);
      choicesDiv.appendChild(btn);
    });

    document.getElementById("score").innerText = `Skor: ${score} dari ${currentQuestion}`;
  }

  function selectAnswer(btn, choice) {
    const correct = questions[currentQuestion].answer;
    if (choice === correct) {
      score++;
      btn.classList.add('correct');
    } else {
      btn.classList.add('wrong');
    }

    // Tunda sebentar sebelum pindah pertanyaan
    setTimeout(() => {
      currentQuestion++;
      if (currentQuestion < questions.length) loadQuestion();
      else showFinalScore();
    }, 500);
  }

  function showFinalScore() {
    document.getElementById("question").innerText = `Selesai! Skor Anda: ${score} dari ${questions.length}`;
    document.getElementById("choices").innerHTML = "";
    document.getElementById("score").innerText = "";
  }

  document.getElementById('audio-toggle').addEventListener('click', () => {
    audio.paused ? audio.play() : audio.pause();
  });

  loadQuestion();
</script>

</body>
</html>
