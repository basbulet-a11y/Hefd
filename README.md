 >
<html lang="bn">
<head>
<meta charset="UTF-8">
<title>Video Speech to Text (No API)</title>
<style>
    body { font-family: Arial, sans-serif; display: flex; gap: 20px; padding: 20px; }
    #videoBox { width: 60%; }
    #transcript { width: 40%; border-left: 3px solid #ddd; padding-left: 20px; }
    .line {
        padding: 6px 10px;
        margin: 5px 0;
        border-radius: 6px;
        background: #f1f1f1;
        cursor: pointer;
    }
    .line.hidden { background: #ffb3b3; text-decoration: line-through; }
</style>
</head>

<body>

<div id="videoBox">
    <h2>ভিডিও আপলোড করুন</h2>
    <input type="file" id="videoInput" accept="video/*"><br><br>
    <video id="video" width="100%" controls></video>
</div>

<div id="transcript">
    <h2>বাংলা টেক্সট (Auto)</h2>
    <p id="status">Waiting for video…</p>
    <div id="lines"></div>
</div>

<script>
// ------------------------------
// NO API Speech-to-Text (Browser built-in)
// ------------------------------
const video = document.getElementById("video");
const linesBox = document.getElementById("lines");
const status = document.getElementById("status");

let recognizer = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
recognizer.lang = "bn-BD";   // Bangla
recognizer.continuous = true;
recognizer.interimResults = false;

// ভিডিও ইনপুট
document.getElementById("videoInput").addEventListener("change", function () {
    let fileURL = URL.createObjectURL(this.files[0]);
    video.src = fileURL;
    status.innerText = "▶ ভিডিও চালু করুন (Auto speech-to-text চলবে)";
});

// ভিডিও প্লে হলে STT চালু
video.onplay = () => {
    recognizer.start();
    status.innerText = "Listening… (বাংলা টেক্সট তৈরি হচ্ছে)";
};

// ভিডিও পজ হলে STT বন্ধ
video.onpause = () => {
    recognizer.stop();
    status.innerText = "Paused";
};

// লাইন ধরে ধরে টেক্সট বানানো
recognizer.onresult = (e) => {
    let text = e.results[e.resultIndex][0].transcript.trim();
    addLine(text);
};

// ক্লিক করলে Hide/Show
function addLine(text) {
    let div = document.createElement("div");
    div.className = "line";
    div.innerText = text;

    div.onclick = () => {
        div.classList.toggle("hidden");
    };

    linesBox.appendChild(div);
}
</script>

</body>
</html>
