[valorant_calc.html](https://github.com/user-attachments/files/24434018/valorant_calc.html)
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Valorant & FPS 설정 계산기 (Safe)</title>
<style>
    body { font-family: Arial, sans-serif; max-width: 900px; margin: auto; padding: 20px; background: #1e1e2f; color: #eee; }
    h1 { text-align: center; color: #ff4655; }
    section { margin-bottom: 30px; padding: 15px; background: #2a2a3f; border-radius: 10px; display: flex; flex-wrap: wrap; gap: 20px; }
    .input-group { flex: 1 1 250px; }
    label { display: block; margin: 8px 0 4px; }
    input, select, button { padding: 8px; margin-bottom: 10px; width: 100%; border-radius: 5px; border: none; }
    button { background: #ff4655; color: white; cursor: pointer; font-weight: bold; }
    .result { background: #1e1e2f; padding: 10px; border-radius: 5px; margin-top: 5px; }
    .card { background: #33334f; padding: 10px; border-radius: 10px; margin: 5px; text-align: center; width: 120px; }
    .crosshair { width: 50px; height: 50px; margin: 5px auto; position: relative; }
    .crosshair div { position: absolute; background: white; }
</style>
</head>
<body>

<h1>Valorant & FPS 설정 계산기 (Safe)</h1>

<!-- 1️⃣ eDPI 계산 + 프로 예시 -->
<section>
<div class="input-group">
<h2>1️⃣ eDPI 계산기</h2>
<label>DPI:</label>
<input type="number" id="dpi" placeholder="예: 800">
<label>게임 내 감도:</label>
<input type="number" id="sens" placeholder="예: 0.5" step="0.01">
<button onclick="calculateEDPI()">eDPI 계산</button>
<div class="result" id="edpiResult"></div>
</div>

<div class="input-group">
<h2>프로 선수 예시 (Safe)</h2>
<div class="card">
<div>🧑‍🎯 Low Sens</div>
<div>DPI: 400</div>
<div>감도: 0.6</div>
<div>eDPI: 240</div>
</div>
<div class="card">
<div>🧑‍🎯 Mid Sens</div>
<div>DPI: 800</div>
<div>감도: 0.35</div>
<div>eDPI: 280</div>
</div>
<div class="card">
<div>🧑‍🎯 High Sens</div>
<div>DPI: 1600</div>
<div>감도: 0.2</div>
<div>eDPI: 320</div>
</div>
</div>
</section>

<!-- 2️⃣ 게임 감도 변환 -->
<section>
<div class="input-group">
<h2>2️⃣ 다른 게임 → Valorant 변환</h2>
<label>게임 선택:</label>
<select id="gameSelect">
    <option value="csgo">CS:GO</option>
    <option value="apex">Apex Legends</option>
    <option value="overwatch">Overwatch 2</option>
    <option value="r6">Rainbow Six Siege</option>
    <option value="fortnite">Fortnite</option>
    <option value="pubg">PUBG</option>
</select>
<label>원래 게임 감도:</label>
<input type="number" id="otherSens" placeholder="예: 2.5" step="0.01">
<label>원래 게임 DPI:</label>
<input type="number" id="otherDpi" placeholder="예: 800">
<label>Valorant DPI:</label>
<input type="number" id="valorantDpi" placeholder="예: 800">
<button onclick="convertSens()">변환 계산</button>
<div class="result" id="convertResult"></div>
</div>
</section>

<!-- 3️⃣ 크로스헤어 설정 + 프로 예시 -->
<section>
<div class="input-group">
<h2>3️⃣ 크로스헤어 생성기</h2>
<label>색상(R,G,B):</label>
<input type="text" id="crosshairColor" placeholder="예: 255,0,0">
<label>두께:</label>
<input type="number" id="crosshairThickness" placeholder="예: 2">
<label>길이:</label>
<input type="number" id="crosshairLength" placeholder="예: 5">
<label>중앙 점:</label>
<select id="crosshairDot">
    <option value="1">있음</option>
    <option value="0">없음</option>
</select>
<button onclick="generateCrosshair()">스크립트 생성</button>
<div class="result" id="crosshairResult"></div>
</div>

<div class="input-group">
<h2>프로 크로스헤어 예시 (Safe)</h2>
<div class="card">
<div>🟢 Low Sens Style</div>
<div class="crosshair" id="ch1"></div>
</div>
<div class="card">
<div>🔵 Mid Sens Style</div>
<div class="crosshair" id="ch2"></div>
</div>
<div class="card">
<div>🔴 High Sens Style</div>
<div class="crosshair" id="ch3"></div>
</div>
</div>
</section>

<!-- 4️⃣ 해상도 & FOV -->
<section>
<div class="input-group">
<h2>4️⃣ 해상도 & FOV 추천</h2>
<label>해상도 선택:</label>
<select id="resolution">
    <option value="1920x1080">1920x1080</option>
    <option value="2560x1440">2560x1440</option>
    <option value="1280x1024">1280x1024</option>
    <option value="1024x768">1024x768</option>
</select>
<button onclick="recommendFOV()">추천 보기</button>
<div class="result" id="fovResult"></div>
</div>
</section>

<script>
// ---------- eDPI ----------
function calculateEDPI(){
    const dpi = parseFloat(document.getElementById('dpi').value);
    const sens = parseFloat(document.getElementById('sens').value);
    if(isNaN(dpi) || isNaN(sens)){ alert("숫자를 정확히 입력해주세요!"); return; }
    const edpi = dpi * sens;
    document.getElementById('edpiResult').innerText = `당신의 eDPI: ${edpi.toFixed(2)}`;
}

// ---------- 감도 변환 ----------
function convertSens(){
    const otherSens = parseFloat(document.getElementById('otherSens').value);
    const otherDpi = parseFloat(document.getElementById('otherDpi').value);
    const valorantDpi = parseFloat(document.getElementById('valorantDpi').value);
    if(isNaN(otherSens) || isNaN(otherDpi) || isNaN(valorantDpi)){ alert("숫자를 정확히 입력해주세요!"); return; }
    const valorantSens = (otherSens * otherDpi) / valorantDpi;
    document.getElementById('convertResult').innerText = `Valorant 추천 감도: ${valorantSens.toFixed(2)}`;
}

// ---------- 크로스헤어 ----------
function generateCrosshair(){
    const color = document.getElementById('crosshairColor').value.split(',').map(x=>parseInt(x.trim()));
    const thickness = parseInt(document.getElementById('crosshairThickness').value);
    const length = parseInt(document.getElementById('crosshairLength').value);
    const dot = document.getElementById('crosshairDot').value;
    const script = `cl_crosshair_color ${color.join(',')}; cl_crosshair_thickness ${thickness}; cl_crosshair_length ${length}; cl_crosshair_dot ${dot}`;
    document.getElementById('crosshairResult').innerText = script;
}

// 프로 스타일 십자선 예시 (CSS로 간단히)
function drawCrosshair(id, color, thickness, length){
    const container = document.getElementById(id);
    container.innerHTML = '';
    const vert = document.createElement('div');
    vert.style.width = `${thickness}px`;
    vert.style.height = `${length}px`;
    vert.style.left = `calc(50% - ${thickness/2}px)`;
    vert.style.top = `calc(50% - ${length/2}px)`;
    vert.style.backgroundColor = color;
    container.appendChild(vert);
    const hori = document.createElement('div');
    hori.style.width = `${length}px`;
    hori.style.height = `${thickness}px`;
    hori.style.left = `calc(50% - ${length/2}px)`;
    hori.style.top = `calc(50% - ${thickness/2}px)`;
    hori.style.backgroundColor = color;
    container.appendChild(hori);
}
drawCrosshair('ch1','lime',2,20);
drawCrosshair('ch2','cyan',2,25);
drawCrosshair('ch3','red',3,30);

// ---------- 해상도 & FOV ----------
function recommendFOV(){
    const res = document.getElementById('resolution').value;
    let fov = "기본 FOV 사용";
    if(res === "1920x1080") fov = "16:9 비율, 기본 FOV";
    else if(res === "2560x1440") fov = "16:9 비율, FOV 약간 넓힘 가능";
    else if(res === "1280x1024") fov = "5:4 비율, 시야 좁음";
    else if(res === "1024x768") fov = "4:3 비율, 시야 좁음";
    document.getElementById('fovResult').innerText = `추천: ${fov}`;
}
</script>

</body>
</html>
