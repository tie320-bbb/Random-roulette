
<!DOCTYPE html>
<html lang="ko">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>행운의 돌림판</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    padding: 25px 15px 40px;
    min-height: 100vh;
    background: #fff5df;
    color: #61452b;
    font-family: "Malgun Gothic", "Apple SD Gothic Neo", sans-serif;
    text-align: center;
}

.container {
    max-width: 650px;
    margin: auto;
    padding: 30px 20px;
    background: #fffdf6;
    border: 3px solid #f3d89d;
    border-radius: 28px;
    box-shadow: 0 10px 30px #c69c5230;
}

h1 {
    margin: 0 0 10px;
    font-size: 32px;
    color: #d87532;
}

.description {
    color: #98794e;
    font-size: 16px;
    margin-bottom: 25px;
}

.wheel-wrapper {
    width: min(100%, 420px);
    margin: 15px auto 0;
    position: relative;
}

canvas {
    display: block;
    width: 100%;
    height: auto;
}

.pointer {
    position: absolute;
    z-index: 3;
    top: -8px;
    left: 50%;
    transform: translateX(-50%);
    width: 0;
    height: 0;
    border-left: 18px solid transparent;
    border-right: 18px solid transparent;
    border-top: 38px solid #e65f2a;
    filter: drop-shadow(0 3px 2px #9e4c2d55);
}

.wheel-center {
    position: absolute;
    width: 22px;
    height: 22px;
    background: #fff9e9;
    border: 5px solid #bd743b;
    border-radius: 50%;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 2;
    pointer-events: none;
}

/* 시작 버튼 */
.start-area {
    min-height: 75px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 15px 0 10px;
}

.start-button {
    width: 220px;
    padding: 17px;
    background: #e97832;
    color: white;
    border: none;
    border-radius: 18px;
    font-size: 23px;
    font-weight: bold;
    cursor: pointer;
    box-shadow: 0 5px 0 #b95724;
    transition: .2s;
}

.start-button:hover {
    background: #d96525;
    transform: translateY(-2px);
}

.start-button:active {
    transform: translateY(3px);
    box-shadow: none;
}

.start-button:disabled {
    opacity: .7;
    cursor: wait;
}

/* 결과 표시 */
.result {
    display: none;
    margin: 15px auto 20px;
    padding: 22px 12px;
    background: #fff0bd;
    border: 3px dashed #e6ad48;
    border-radius: 20px;
    animation: pop .45s ease;
}

.result-label {
    color: #997343;
    font-size: 19px;
}

.result-name {
    color: #df6828;
    font-size: clamp(32px, 8vw, 46px);
    font-weight: 900;
    margin-top: 8px;
    overflow-wrap: anywhere;
}

@keyframes pop {
    0% { transform: scale(.7); opacity: 0; }
    100% { transform: scale(1); opacity: 1; }
}

/* 항목 입력 */
.editor {
    margin-top: 25px;
    padding: 22px;
    background: #fff8e9;
    border-radius: 20px;
    text-align: left;
}

.editor h2 {
    margin: 0 0 15px;
    font-size: 22px;
    color: #a76630;
}

.input-row {
    display: flex;
    gap: 8px;
}

input {
    flex: 1;
    min-width: 0;
    padding: 14px;
    font-size: 16px;
    border: 2px solid #e9cf99;
    border-radius: 12px;
    outline: none;
}

input:focus {
    border-color: #e99a43;
}

.add-button {
    padding: 12px 17px;
    background: #78a45e;
    color: white;
    font-size: 16px;
    font-weight: bold;
    border: none;
    border-radius: 12px;
    cursor: pointer;
}

.item-list {
    display: flex;
    flex-wrap: wrap;
    gap: 9px;
    margin-top: 17px;
}

.item {
    display: flex;
    align-items: center;
    gap: 8px;
    padding: 9px 12px;
    background: white;
    border: 1px solid #ead4a8;
    border-radius: 30px;
    font-size: 15px;
}

.color-dot {
    width: 13px;
    height: 13px;
    border-radius: 50%;
}

.delete-button {
    border: none;
    background: none;
    color: #d36c51;
    font-size: 19px;
    cursor: pointer;
    padding: 0;
}

.count {
    margin-top: 13px;
    color: #a58b65;
    font-size: 14px;
    text-align: center;
}

.reset-button {
    margin-top: 20px;
    padding: 13px 25px;
    border: none;
    border-radius: 13px;
    background: #f2dfb8;
    color: #79582d;
    font-size: 17px;
    font-weight: bold;
    cursor: pointer;
}

.reset-button:hover {
    background: #e9d09b;
}

.notice {
    margin-top: 18px;
    color: #a58b65;
    font-size: 13px;
}

@media(max-width: 450px) {
    .container {
        padding: 25px 13px;
    }

    h1 {
        font-size: 26px;
    }

    .input-row {
        flex-direction: column;
    }

    .add-button {
        padding: 14px;
    }

    .editor {
        padding: 17px;
    }
}
</style>
</head>

<body>

<div class="container">

    <h1>🎡 행운의 돌림판</h1>

    <div class="description">
        고민하지 말고 돌려보세요! 오늘의 행운은 누구에게?
    </div>

    <!-- 돌림판 -->
    <div class="wheel-wrapper">
        <div class="pointer"></div>
        <canvas id="wheel" width="600" height="600"></canvas>
        <div class="wheel-center"></div>
    </div>

    <!-- 시작 / 다시 돌리기 버튼 -->
    <div class="start-area" id="startArea">
        <button class="start-button" id="startButton"
                onclick="spinWheel()">
            🎯 시작하기
        </button>
    </div>

    <!-- 당첨 결과 -->
    <div class="result" id="result">
        <div class="result-label">🎉 오늘의 당첨 항목은?</div>
        <div class="result-name" id="resultName"></div>
    </div>

    <!-- 항목 편집 -->
    <div class="editor">
        <h2>✏️ 돌림판 항목 설정</h2>

        <div class="input-row">
            <input
                type="text"
                id="itemInput"
                maxlength="20"
                placeholder="새로운 항목을 입력하세요"
            >
            <button class="add-button" onclick="addItem()">
                + 항목 추가
            </button>
        </div>

        <div class="item-list" id="itemList"></div>

        <div class="count" id="count">
            4 / 8개 항목
        </div>
    </div>

    <button class="reset-button" onclick="resetWheel()">
        🔄 다시 고르기
    </button>

    <div class="notice">
        💛 항목은 최소 2개부터 최대 8개까지 설정할 수 있어요.
    </div>

</div>

<script>
const canvas = document.getElementById("wheel");
const ctx = canvas.getContext("2d");

const colors = [
    "#F28C45", // 주황
    "#79A95B", // 초록
    "#F2C94C", // 노랑
    "#E98278", // 분홍
    "#77B9B0", // 민트
    "#A88BC7", // 보라
    "#E6A84F", // 황금
    "#6FA0C8"  // 파랑
];

let items = [
    "치킨",
    "피자",
    "떡볶이",
    "햄버거"
];

let rotation = 0;
let spinning = false;
let resultTimer = null;
let resultVisible = false;

// 돌림판 그리기
function drawWheel() {
    const count = items.length;
    const cx = 300;
    const cy = 300;
    const radius = 280;
    const angle = Math.PI * 2 / count;

    ctx.clearRect(0, 0, 600, 600);

    for (let i = 0; i < count; i++) {
        const start = rotation + i * angle;
        const end = start + angle;
        const middle = start + angle / 2;

        // 부채꼴 영역
        ctx.beginPath();
        ctx.moveTo(cx, cy);
        ctx.arc(cx, cy, radius, start, end);
        ctx.closePath();

        ctx.fillStyle = colors[i];
        ctx.fill();

        ctx.lineWidth = 6;
        ctx.strokeStyle = "#fff9e9";
        ctx.stroke();

        // 항목 이름
        ctx.save();
        ctx.translate(cx, cy);
        ctx.rotate(middle);

        ctx.textAlign = "right";
        ctx.textBaseline = "middle";
        ctx.fillStyle = "#ffffff";
        ctx.font = "bold 27px sans-serif";

        ctx.shadowColor = "#60402080";
        ctx.shadowBlur = 3;

        let label = items[i];

        if (label.length > 10) {
            label = label.substring(0, 9) + "...";
        }

        ctx.fillText(label, radius - 25, 0);

        ctx.restore();
    }

    // 바깥 테두리
    ctx.beginPath();
    ctx.arc(cx, cy, radius, 0, Math.PI * 2);
    ctx.strokeStyle = "#fffdf5";
    ctx.lineWidth = 12;
    ctx.stroke();
}

// 항목 목록 표시
function updateList() {
    const list = document.getElementById("itemList");
    list.innerHTML = "";

    items.forEach((item, index) => {
        const div = document.createElement("div");
        div.className = "item";

        const dot = document.createElement("span");
        dot.className = "color-dot";
        dot.style.backgroundColor = colors[index];

        const name = document.createElement("span");
        name.textContent = item;

        const del = document.createElement("button");
        del.className = "delete-button";
        del.textContent = "×";
        del.setAttribute("aria-label", item + " 삭제");
        del.onclick = () => removeItem(index);

        div.append(dot, name, del);
        list.appendChild(div);
    });

    document.getElementById("count").textContent =
        items.length + " / 8개 항목";

    drawWheel();
}

// 항목 추가
function addItem() {
    if (spinning) return;

    const input = document.getElementById("itemInput");
    const value = input.value.trim();

    if (!value) {
        alert("항목 이름을 입력해주세요!");
        input.focus();
        return;
    }

    if (items.length >= 8) {
        alert("최대 8개까지 추가할 수 있어요!");
        return;
    }

    items.push(value);
    input.value = "";

    updateList();
    clearResult();
    setStartButton("start");

    input.focus();
}

// 항목 삭제
function removeItem(index) {
    if (spinning) return;

    if (items.length <= 2) {
        alert("최소 2개의 항목이 필요해요!");
        return;
    }

    items.splice(index, 1);

    updateList();
    clearResult();
    setStartButton("start");
}

// 엔터키로 추가
document.getElementById("itemInput").addEventListener(
    "keydown",
    function(event) {
        if (event.key === "Enter") {
            addItem();
        }
    }
);

// 버튼 상태 변경
function setStartButton(type) {
    const area = document.getElementById("startArea");

    if (type === "start") {
        area.innerHTML = `
            <button class="start-button" id="startButton"
                    onclick="spinWheel()">
                🎯 시작하기
            </button>
        `;
    } else if (type === "again") {
        area.innerHTML = `
            <button class="start-button" id="startButton"
                    onclick="spinWheel()">
                🔄 다시 돌리기
            </button>
        `;
    } else {
        const button = document.getElementById("startButton");
        if (button) button.disabled = true;
    }
}

// 결과 초기화
function clearResult() {
    clearTimeout(resultTimer);
    resultTimer = null;
    resultVisible = false;

    document.getElementById("result").style.display = "none";
}

// 돌림판 회전
function spinWheel() {
    if (spinning) return;

    if (items.length < 2) {
        alert("항목을 2개 이상 설정해주세요!");
        return;
    }

    clearResult();
    spinning = true;

    setStartButton("disabled");

    const count = items.length;
    const angle = Math.PI * 2 / count;

    // 당첨 항목을 무작위로 결정
    const winner = Math.floor(Math.random() * count);

    // 위쪽 포인터가 선택된 영역의 중앙에 오도록 계산
    const pointerAngle = -Math.PI / 2;
    const targetAngle =
        pointerAngle - (winner + 0.5) * angle;

    const fullCircle = Math.PI * 2;

    const current =
        ((rotation % fullCircle) + fullCircle) % fullCircle;

    const desired =
        ((targetAngle % fullCircle) + fullCircle) % fullCircle;

    let distance = desired - current;

    if (distance < 0) {
        distance += fullCircle;
    }

    // 5~7바퀴 회전
    const turns = 5 + Math.floor(Math.random() * 3);
    const totalRotation = fullCircle * turns + distance;

    const startRotation = rotation;
    const duration = 5000;
    const startTime = performance.now();

    function animate(now) {
        const elapsed = now - startTime;
        const progress = Math.min(elapsed / duration, 1);

        // 처음에는 빠르고 마지막에는 천천히 멈추는 효과
        const ease = 1 - Math.pow(1 - progress, 5);

        rotation =
            startRotation + totalRotation * ease;

        drawWheel();

        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            rotation =
                ((rotation % fullCircle) + fullCircle) % fullCircle;

            drawWheel();

            spinning = false;

            showResult(items[winner]);
        }
    }

    requestAnimationFrame(animate);
}

// 당첨 결과 표시
function showResult(name) {
    const result = document.getElementById("result");
    const resultName = document.getElementById("resultName");

    resultName.textContent = "🎉 " + name;

    result.style.display = "block";
    resultVisible = true;

    // 2.5초 후 결과를 숨기고 다시 돌리기 버튼 표시
    resultTimer = setTimeout(() => {
        result.style.display = "none";
        resultVisible = false;

        setStartButton("again");
    }, 2500);
}

// 다시 고르기: 결과 초기화 및 돌림판 정지 위치 초기화
function resetWheel() {
    if (spinning) {
        alert("돌림판이 멈춘 후 다시 고를 수 있어요!");
        return;
    }

    clearResult();

    rotation = 0;
    drawWheel();

    setStartButton("start");
}

// 최초 실행
updateList();

</script>

</body>
</html>
