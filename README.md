
<html lang="th">
<head>
<meta charset="UTF-8">
<title>ThaiMusicGame</title>
<link href="https://fonts.googleapis.com/css2?family=Itim&display=swap" rel="stylesheet">
<style>
    body { 
        font-family: 'Itim', cursive;
        text-align: center; 
        margin: 0;
        background: linear-gradient(to bottom right, #EAF6FF, #FFF7E6);
        background-attachment: fixed;
        color:#444;
        padding-bottom: 100px;
    }
    h1 { 
        margin: 20px;
        color: #FF6347; /* สีฟอนต์สดใส */
        font-size: 40px; /* ขนาดฟอนต์ใหญ่ขึ้น */
        text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.3); /* เพิ่มเงาฟอนต์ */
        background-color: #FFEB99; /* พื้นหลังสีพาสเทล */
        padding: 10px;
        border-radius: 12px;
        max-width: 90%; /* จำกัดขนาดความกว้างไม่เกิน 90% */
        max-height: 200px; /* จำกัดความสูงไม่เกิน 200px */
        overflow: hidden; /* ซ่อนข้อความที่เกินออก */
        white-space: nowrap; /* ทำให้ข้อความไม่หักบรรทัด */
        text-overflow: ellipsis; /* ตัดข้อความที่เกินออกด้วย "..." */
        display: inline-block; /* ปรับให้ไม่ล้น */
    }

    /* หน้าแรก */
    #intro { 
        display:flex; flex-direction:column; align-items:center; justify-content:center;
        height:100vh;
    }
    #intro h1 { font-size:48px; margin-bottom:20px; }
    #intro ul { text-align:left; max-width:500px; font-size:20px; }
    #startBtn {
        margin-top:30px; font-size:24px; padding:12px 30px;
        border:none; border-radius:12px;
        background:#A0E7E5; color:#444; cursor:pointer;
    }
    #startBtn:hover{ background:#7ED6D4; }

    /* ป้ายคำตอบ */
    .tiles { 
        display: grid; 
        grid-template-columns: repeat(3, 200px);
        grid-gap: 15px;
        justify-content: center;
        margin-top: 30px;
    }
    .tile {
        width: 200px; height: 120px;
        background: #FFEB99; color: #444;
        font-size: 22px; font-weight: bold;
        display: flex; justify-content: center; align-items: center;
        border-radius: 20px;
        box-shadow: 0 5px 8px rgba(0,0,0,0.2);
        transition: 0.3s;
    }
    .tile.open {
        background: #FFC78A; color: #5A3E2B;
        box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    }

    .show-btn { 
        margin-top: 20px; 
        font-size: 20px; 
        padding: 10px 20px; 
        border-radius:10px;
        border:none;
        background:#A0E7E5; 
        color:#444;
        cursor:pointer;
    }
    .show-btn:hover { background:#7ED6D4; }

    /* บอร์ดคะแนน */
    .board { 
        margin: 30px auto; 
        display: flex; 
        justify-content: center; 
        gap: 15px; 
        flex-wrap:wrap;
    }
    .team { 
        width: 200px; height: 150px; 
        border-radius: 20px;
        display:flex; flex-direction:column; align-items:center; justify-content:center;
        color:#333; font-size:20px; font-weight:bold;
        text-shadow: 1px 1px 2px #fff;
        box-shadow:0 5px 10px rgba(0,0,0,0.2);
    }
    .score { font-size: 36px; margin: 5px 0; }
    .team button { 
        font-size: 16px; margin:2px;
        padding:3px 10px; border:none; border-radius:5px; cursor:pointer;
    }

    /* ปุ่มควบคุม ติดด้านล่าง */
    .controls {
        position: fixed;
        bottom: 20px;
        left: 0;
        width: 100%;
        display: flex;
        justify-content: center;
        gap: 20px;
    }
    .controls button {
        font-size: 18px;
        padding: 8px 16px;
        border:none; border-radius:8px;
        background:#ffb84c; color:white;
        cursor:pointer;
    }
    .controls button:hover { background:#ff9900; }

    /* ซ่อนเกมตอนเริ่ม */
    #game { display:none; }
</style>
</head>
<body>

<!-- หน้าแรก -->
<div id="intro">
    <h1>เกมเปิดป้าย ดนตรีไทย</h1>
    <ul>
        <li>ตอบให้ตรงป้ายจึงจะได้คะแนน</li>
        <li>ตอบถูกแต่ไม่ตรงป้าย → ไม่ได้คะแนน</li>
        <li>กดปุ่ม <b>แสดงเฉลยทั้งหมด</b> เมื่อครูต้องการเฉลย</li>
        <li>คะแนนต่อข้อ: <b>100 คะแนน</b></li>
    </ul>
    <button id="startBtn" onclick="startGame()">เริ่มเกม</button>
</div>

<!-- เกมหลัก -->
<div id="game">
    <h1 id="question">คำถามจะอยู่ที่นี่</h1>

    <!-- ป้ายคำตอบ -->
    <div class="tiles" id="tiles"></div>
    <div>
        <button class="show-btn" onclick="showAll()">แสดงเฉลยทั้งหมด</button>
        <button class="show-btn" onclick="hideAll()">ปิดเฉลยทั้งหมด</button>
    </div>

    <!-- บอร์ดคะแนน -->
    <div class="board">
        <div class="team" style="background:#FFB3BA;">
            ทีม A
            <div class="score" id="scoreA">0</div>
            <button onclick="changeScore('A',100)">+100</button>
            <button onclick="changeScore('A',-100)">-100</button>
        </div>
        <div class="team" style="background:#BAE1FF;">
            ทีม B
            <div class="score" id="scoreB">0</div>
            <button onclick="changeScore('B',100)">+100</button>
            <button onclick="changeScore('B',-100)">-100</button>
        </div>
        <div class="team" style="background:#BAFFC9;">
            ทีม C
            <div class="score" id="scoreC">0</div>
            <button onclick="changeScore('C',100)">+100</button>
            <button onclick="changeScore('C',-100)">-100</button>
        </div>
        <div class="team" style="background:#FFFFBA;">
            ทีม D
            <div class="score" id="scoreD">0</div>
            <button onclick="changeScore('D',100)">+100</button>
            <button onclick="changeScore('D',-100)">-100</button>
        </div>
    </div>

    <!-- ปุ่มควบคุม -->
    <div class="controls">
        <button onclick="prevQuestion()">ข้อก่อนหน้า</button>
        <button onclick="nextQuestion()">ข้อถัดไป</button>
        <button onclick="toggleMusic()">🔊 เปิด/ปิดเพลง</button>
    </div>
</div>

<!-- ฝังเพลงพื้นหลังจาก YouTube -->
<iframe width="100%" height="250" src="https://www.youtube.com/embed/Vec5yrhU-z0?autoplay=1&loop=1&playlist=Vec5yrhU-z0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<script>
let current = 0;
const questions = [
    { q: "เครื่องดนตรีที่ไม่มีอยู่ในวงปี่พาทย์",
      a: ["จะเข้","ขิม","ซออู้","ซอด้วง","ซอสามสาย","ขลุ่ยเพียงออ"] },
    { q: "เครื่องดนตรีที่มีอยู่ในวงปี่พาทย์",
      a: ["ระนาดเอก","ระนาดทุ้ม","ปี่ใน","ฆ้องวงใหญ่","กลองทัด","กลองแขก"] },
    { q: "เครื่องดนตรีที่ไม่มีอยู่ในวงเครื่องสาย",
      a: ["ระนาดเอก","ปี่ใน","ปี่นอก","กลองแขก","ฉาบใหญ่","กลองทัด"] },
    { q: "เครื่องดนตรีที่มีอยู่ในวงเครื่องสาย",
      a: ["ซอด้วง","ซออู้","จะเข้","ขิม","ขลุ่ยเพียงออ","โทน"] },
    { q: "เครื่องประกอบจังหวะในวงเครื่องสาย",
      a: ["โทน","รำมะนา","ฉิ่ง","ฉาบเล็ก","กรับพวง","กลองตะโพน"] },
    { q: "เครื่องดนตรีที่มีอยู่ในวงมโหรี",
      a: ["ซอสามสาย","จะเข้","ขิม","ระนาดเอก","ฆ้องวงเล็ก","กลองทัด"] }
];

function startGame(){
    document.getElementById('intro').style.display='none';
    document.getElementById('game').style.display='block';
    showQuestion(current);
}

function showQuestion(i){
    document.getElementById('question').innerText = "ข้อที่ " + (i+1) + ": " + questions[i].q;
    const tilesDiv = document.getElementById('tiles');
    tilesDiv.innerHTML = "";
    questions[i].a.forEach((ans, idx) => {
        const tile = document.createElement('div');
        tile.className = "tile";
        tile.innerText = "ป้าย " + (idx+1);
        tilesDiv.appendChild(tile);
    });
}

function showAll(){
    document.querySelectorAll('.tile').forEach((tile, idx) => {
        tile.classList.add('open');
        tile.innerText = questions[current].a[idx];
    });
}

function hideAll(){
    document.querySelectorAll('.tile').forEach((tile, idx) => {
        tile.classList.remove('open');
        tile.innerText = "ป้าย " + (idx+1);
    });
}

function nextQuestion(){
    current = (current + 1) % questions.length;
    showQuestion(current);
}

function prevQuestion(){
    current = (current - 1 + questions.length) % questions.length;
    showQuestion(current);
}

function changeScore(team, delta){
    const scoreElement = document.getElementById("score" + team);
    let score = parseInt(scoreElement.innerText);
    scoreElement.innerText = score + delta;
}

function toggleMusic(){
    const bgm = document.getElementById('bgm');
    if(bgm.paused) bgm.play(); else bgm.pause();
}
</script>

</body>
</html>
