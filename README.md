# 8A2-Ba-Khifrom pathlib import Path
import zipfile

base = Path("/mnt/data/ai_ba_khi_nhat")
base.mkdir(exist_ok=True)

html = """<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#100e0b">
  <title>Ai bá khí nhất?</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <main class="app">
    <header class="heading">
      <p class="eyebrow">THE ULTIMATE RANDOMIZER</p>
      <h1>AI BÁ KHÍ NHẤT?</h1>
      <p class="subtitle">Vận mệnh đã được định đoạt...</p>
    </header>

    <section class="upload-panel">
      <label class="upload-box" for="imageInput">
        <span class="folder-icon">📁</span>
        <strong>CHỌN ẢNH TỪ THIẾT BỊ</strong>
        <small>JFIF · JPG · JPEG · JEG · PNG</small>
        <input id="imageInput" type="file"
          accept=".jfif,.jpg,.jpeg,.jeg,.png,image/jpeg,image/png" multiple>
      </label>
      <div class="list-heading">
        <span>Danh sách ảnh</span>
        <strong id="imageCount">0 ảnh</strong>
      </div>
      <div id="imageList" class="image-list"></div>
    </section>

    <section class="draw-section">
      <p class="section-label">✦ ĐANG TÌM KIẾM CHÂN MỆNH ✦</p>
      <div id="stage" class="stage">
        <div id="placeholder" class="placeholder">
          <div class="crown">👑</div>
          <h2>AI SẼ LÀ NGƯỜI ĐƯỢC CHỌN?</h2>
          <p>Hãy tải ảnh lên để bắt đầu</p>
        </div>
        <img id="preview" class="preview hidden" alt="Ảnh đang được chọn">
        <div id="winnerBanner" class="winner-banner hidden">
          <span>✦ THE CHOSEN ONE ✦</span>
          <strong>BÁ KHÍ NHẤT!</strong>
        </div>
      </div>

      <p id="status" class="status" aria-live="polite">Sẵn sàng khai chiến!</p>
      <div id="timer" class="timer">05.00</div>
      <button id="startButton" class="start-button" disabled>BẮT ĐẦU</button>
      <button id="resetButton" class="reset-button" type="button">Đặt lại kết quả</button>
    </section>

    <footer>POWERED BY RANDOM DESTINY · 2026</footer>
  </main>
  <script src="script.js"></script>
</body>
</html>
"""

css = """* { box-sizing: border-box; }

:root {
  color-scheme: dark;
  --gold: #ffca62;
  --gold-light: #f8e7c5;
  --muted: #b9a98f;
}

body {
  margin: 0;
  min-height: 100vh;
  padding: 28px 16px;
  font-family: Inter, system-ui, -apple-system, "Segoe UI", sans-serif;
  color: var(--gold-light);
  background: radial-gradient(ellipse at top, #38230d, #100e0b 65%);
}

.app {
  width: min(100%, 680px);
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 24px;
}

.heading { text-align: center; }
.eyebrow {
  margin: 0 0 9px;
  color: #d99737;
  font-size: 11px;
  font-weight: 800;
  letter-spacing: .45em;
}
h1 {
  margin: 0;
  color: var(--gold);
  font-size: clamp(34px, 8vw, 54px);
  line-height: 1.1;
  text-shadow: 0 0 22px #b66c1a;
}
.subtitle { color: #b9a98f; font-size: 14px; }

.upload-panel {
  width: 100%;
  padding: 14px;
  border: 1px solid #785126;
  border-radius: 18px;
  background: #17120e;
}
.upload-box {
  min-height: 145px;
  border: 2px dashed #785126;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  cursor: pointer;
  transition: background .2s, border-color .2s;
}
.upload-box:hover { background: #2a1b0d; border-color: var(--gold); }
.upload-box input { display: none; }
.folder-icon { font-size: 32px; }
.upload-box strong { color: #f4d9a7; }
.upload-box small { color: #a9997e; font-size: 11px; }

.list-heading {
  display: flex;
  justify-content: space-between;
  margin: 15px 2px 10px;
  color: #b9a98f;
  font-size: 13px;
}
.list-heading strong { color: var(--gold); }
.image-list {
  display: grid;
  grid-template-columns: repeat(5, minmax(0, 1fr));
  gap: 9px;
}
.image-tile { position: relative; min-width: 0; }
.image-tile img {
  display: block;
  width: 100%;
  aspect-ratio: 1;
  object-fit: cover;
  border-radius: 8px;
  border: 1px solid #785126;
}
.image-tile span {
  display: block;
  overflow: hidden;
  margin-top: 4px;
  color: #c7b79e;
  font-size: 10px;
  text-overflow: ellipsis;
  white-space: nowrap;
}

.draw-section {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 13px;
}
.section-label {
  margin: 0;
  color: #d99737;
  font-size: 11px;
  font-weight: 800;
  letter-spacing: .3em;
  text-align: center;
}
.stage {
  position: relative;
  width: 100%;
  height: 330px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid #785126;
  border-radius: 17px;
  background: linear-gradient(145deg, #21180f, #090807);
}
.placeholder { padding: 16px; text-align: center; }
.crown { margin-bottom: 14px; font-size: 68px; }
.placeholder h2 { color: #f2d39c; font-size: 18px; }
.placeholder p { color: #a9997e; font-size: 12px; }
.preview { width: 100%; height: 100%; object-fit: contain; }
.hidden { display: none !important; }

.winner-banner {
  position: absolute;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
  padding: 35px 12px 17px;
  background: linear-gradient(transparent, #100b07ed);
}
.winner-banner span {
  color: var(--gold);
  font-size: 11px;
  font-weight: 800;
  letter-spacing: .35em;
}
.winner-banner strong { color: #ffe2a3; font-size: 27px; }

.status {
  min-height: 20px;
  margin: 0;
  color: #e7c995;
  font-size: 13px;
  text-align: center;
}
.timer {
  color: var(--gold);
  font-size: 48px;
  font-weight: 900;
  font-variant-numeric: tabular-nums;
}
.start-button {
  width: 100%;
  padding: 17px 12px;
  border: 0;
  border-radius: 12px;
  color: #211207;
  background: linear-gradient(100deg, #a65b13, #ffce6c, #a65b13);
  box-shadow: 0 5px 25px #b8752844;
  font-size: 19px;
  font-weight: 900;
  letter-spacing: .15em;
  cursor: pointer;
  transition: transform .15s, filter .15s;
}
.start-button:not(:disabled):hover { filter: brightness(1.12); }
.start-button:not(:disabled):active { transform: scale(.98); }
.start-button:disabled { opacity: .4; cursor: not-allowed; }
.reset-button {
  border: 0;
  background: transparent;
  color: #b9a98f;
  text-decoration: underline;
  text-underline-offset: 4px;
  cursor: pointer;
}
footer {
  color: #756650;
  font-size: 10px;
  letter-spacing: .25em;
  text-align: center;
}

.stage.shaking { animation: shake .09s infinite; }
.stage.glowing { box-shadow: 0 0 14px #d38b25, 0 0 38px #ffca62, 0 0 70px #a85b16; }
@keyframes shake {
  0%, 100% { transform: translateX(0); }
  25% { transform: translateX(-3px); }
  75% { transform: translateX(3px); }
}
@media (max-width: 430px) {
  .stage { height: 270px; }
  .image-list { grid-template-columns: repeat(4, minmax(0, 1fr)); }
}
"""

js = """const imageInput = document.getElementById("imageInput");
const imageCount = document.getElementById("imageCount");
const imageList = document.getElementById("imageList");
const stage = document.getElementById("stage");
const placeholder = document.getElementById("placeholder");
const preview = document.getElementById("preview");
const winnerBanner = document.getElementById("winnerBanner");
const statusText = document.getElementById("status");
const timer = document.getElementById("timer");
const startButton = document.getElementById("startButton");
const resetButton = document.getElementById("resetButton");

const allowedExtension = /\\\\.(jfif|jpe?g|jeg|png)$/i;
let images = [];
let objectUrls = [];
let isRunning = false;
let animationFrame = null;
let startTime = 0;
let selectedWinner = -1;
let lastPreviewIndex = -1;

function clearObjectUrls() {
  objectUrls.forEach((url) => URL.revokeObjectURL(url));
  objectUrls = [];
}

function showImage(index) {
  if (!images[index]) return;
  preview.src = objectUrls[index];
  preview.classList.remove("hidden");
  placeholder.classList.add("hidden");
}

function renderImageList() {
  imageCount.textContent = `${images.length} ảnh`;
  imageList.replaceChildren();

  images.forEach((file, index) => {
    const tile = document.createElement("div");
    tile.className = "image-tile";

    const img = document.createElement("img");
    img.src = objectUrls[index];
    img.alt = file.name;

    const name = document.createElement("span");
    name.textContent = file.name;
    name.title = file.name;

    tile.append(img, name);
    imageList.append(tile);
  });

  startButton.disabled = images.length === 0 || isRunning;
}

imageInput.addEventListener("change", (event) => {
  if (isRunning) return;

  const selectedFiles = Array.from(event.target.files || []).filter((file) => {
    return allowedExtension.test(file.name) &&
      (file.type === "" || file.type.startsWith("image/"));
  });

  if (selectedFiles.length === 0) {
    statusText.textContent = "Không tìm thấy ảnh hợp lệ. Hãy thử JPG, PNG hoặc JFIF.";
    event.target.value = "";
    return;
  }

  clearObjectUrls();
  images = selectedFiles;
  objectUrls = images.map((file) => URL.createObjectURL(file));

  selectedWinner = -1;
  preview.classList.add("hidden");
  placeholder.classList.remove("hidden");
  winnerBanner.classList.add("hidden");
  stage.classList.remove("glowing", "shaking");
  timer.textContent = "05.00";
  statusText.textContent = "Đã sẵn sàng! Nhấn bắt đầu nào.";

  renderImageList();
  event.target.value = "";
});

function finishDraw() {
  isRunning = false;
  cancelAnimationFrame(animationFrame);

  showImage(selectedWinner);
  stage.classList.remove("shaking");
  stage.classList.add("glowing");
  winnerBanner.classList.remove("hidden");

  timer.textContent = "00.00";
  statusText.textContent = "🎉 KẾT QUẢ CUỐI CÙNG ĐÃ ĐƯỢC ĐỊNH ĐOẠT!";
  startButton.disabled = false;
  startButton.textContent = "QUAY LẠI LẦN NỮA";
}

function animateDraw(now) {
  const elapsed = now - startTime;
  const remaining = Math.max(0, 5000 - elapsed);

  timer.textContent = (remaining / 1000).toFixed(2);

  if (remaining <= 0) {
    finishDraw();
    return;
  }

  // Ảnh chuyển nhanh lúc đầu và chậm dần về cuối.
  const progress = elapsed / 5000;
  const interval = 45 + progress * 300;
  const previewIndex = Math.floor(elapsed / interval) % images.length;

  if (previewIndex !== lastPreviewIndex) {
    lastPreviewIndex = previewIndex;
    showImage(previewIndex);
  }

  if (progress < 0.4) {
    statusText.textContent = "⚡ ĐANG QUÉT CÁC ỨNG VIÊN...";
  } else if (progress < 0.8) {
    statusText.textContent = "🔥 VẬN MỆNH ĐANG ĐƯỢC QUYẾT ĐỊNH...";
  } else {
    statusText.textContent = "👑 CHUẨN BỊ CÔNG BỐ NHÀ VÔ ĐỊCH!";
  }

  animationFrame = requestAnimationFrame(animateDraw);
}

startButton.addEventListener("click", () => {
  if (isRunning || images.length === 0) return;

  isRunning = true;
  selectedWinner = Math.floor(Math.random() * images.length);
  lastPreviewIndex = -1;

  startButton.disabled = true;
  startButton.textContent = "ĐANG QUAY...";
  winnerBanner.classList.add("hidden");
  stage.classList.remove("glowing");
  stage.classList.add("shaking");

  startTime = performance.now();
  animationFrame = requestAnimationFrame(animateDraw);
});

resetButton.addEventListener("click", () => {
  if (isRunning) {
    isRunning = false;
    cancelAnimationFrame(animationFrame);
  }

  selectedWinner = -1;
  stage.classList.remove("glowing", "shaking");
  winnerBanner.classList.add("hidden");
  preview.classList.add("hidden");
  placeholder.classList.remove("hidden");

  timer.textContent = "05.00";
  statusText.textContent = "Sẵn sàng khai chiến!";
  startButton.textContent = "BẮT ĐẦU";
  renderImageList();
});
"""

(base / "index.html").write_text(html, encoding="utf-8")
(base / "style.css").write_text(css, encoding="utf-8")
(base / "script.js").write_text(js, encoding="utf-8")

zip_path = Path("/mnt/data/ai_ba_khi_nhat_source.zip")
with zipfile.ZipFile(zip_path, "w", zipfile.ZIP_DEFLATED) as zf:
    for file in base.iterdir():
        zf.write(file, arcname=f"ai_ba_khi_nhat/{file.name}")

print("Đã tạo mã nguồn website:")
print(zip_path)
print(base / "index.html")
print(base / "style.css")
print(base / "script.js")
