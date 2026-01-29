<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Crystal Clear Mic Monitor</title>

<style>
  body {
    margin: 0;
    height: 100vh;
    background: linear-gradient(135deg, #0f2027, #203a43, #2c5364);
    display: flex;
    align-items: center;
    justify-content: center;
    font-family: system-ui, sans-serif;
    color: white;
  }

  .card {
    background: rgba(0,0,0,0.35);
    backdrop-filter: blur(12px);
    padding: 32px 40px;
    border-radius: 18px;
    width: 340px;
    text-align: center;
    box-shadow: 0 12px 40px rgba(0,0,0,0.45);
  }

  h1 {
    margin: 0 0 8px;
    font-size: 1.6em;
  }

  p {
    margin: 0 0 20px;
    font-size: 0.9em;
    opacity: 0.85;
  }

  button {
    width: 100%;
    padding: 14px;
    font-size: 1em;
    border: none;
    border-radius: 12px;
    cursor: pointer;
    color: white;
    background: linear-gradient(135deg, #00c6ff, #0072ff);
  }

  button:hover {
    opacity: 0.9;
  }

  label {
    display: block;
    margin-top: 22px;
    margin-bottom: 6px;
    font-size: 0.9em;
  }

  input[type="range"] {
    width: 100%;
  }

  .status {
    margin-top: 14px;
    font-size: 0.85em;
    opacity: 0.8;
  }
</style>
</head>

<body>

<div class="card">
  <h1>🎧 Mic Monitor</h1>
  <p>Raw, low-latency mic playback</p>

  <button onclick="startMic()">Start Monitoring</button>

  <label>Feedback Volume</label>
  <input id="volume" type="range" min="0" max="1.2" step="0.01" value="0.8">

  <div class="status" id="status">Mic inactive</div>
</div>

<script>
let audioCtx;
let gainNode;

async function startMic() {
  if (audioCtx) return;

  const stream = await navigator.mediaDevices.getUserMedia({
    audio: {
      echoCancellation: false,
      noiseSuppression: false,
      autoGainControl: false,
      channelCount: 1,
      sampleRate: 48000
    }
  });

  audioCtx = new AudioContext({ sampleRate: 48000 });

  const micSource = audioCtx.createMediaStreamSource(stream);
  gainNode = audioCtx.createGain();
  gainNode.gain.value = 0.8;

  micSource.connect(gainNode);
  gainNode.connect(audioCtx.destination);

  document.getElementById("status").textContent =
    "Monitoring active • 48kHz • Raw mic";
}

document.getElementById("volume").addEventListener("input", e => {
  if (gainNode) gainNode.gain.value = e.target.value;
});
</script>

</body>
</html>
