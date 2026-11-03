[index.html](https://github.com/user-attachments/files/34334149/index.html)
# project7_sukigao9sen
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>MY FAVORITE FACE</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    background: #22C0EF;
    font-family: Arial, sans-serif;
    color: white;
    text-align: center;
}

.container {
    padding: 20px;
    max-width: 600px;
    margin: 0 auto;
}

h1 {
    color: #FFFFFF;
    letter-spacing: 3px;
    font-size: 28px;
    font-weight: bold;
    margin-bottom: 5px;
}

button {
    background: #e88da5;
    color: white;
    border: none;
    border-radius: 30px;
    padding: 14px 40px;
    font-size: 16px;
    cursor: pointer;
    margin-top: 25px;
}

button:hover {
    background: #d66f8c;
}

.hidden {
    display: none;
}

.round {
    color: #075985;
    font-weight: bold;
    margin-top: 15px;
    font-size: 18px;
}

/* 9枚表示用（ROUND 1用） */
.photo-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-top: 20px;
}

/* 3〜4択＆同票決戦用の選択グリッド */
.choice-grid {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-top: 20px;
    flex-wrap: wrap;
}

.photo {
    background: white;
    border: 4px solid transparent;
    border-radius: 12px;
    overflow: hidden;
    cursor: pointer;
    transition: 0.2s;
    width: 100%;
    max-width: 160px;
}

.photo.selected {
    border-color: #e88da5;
    transform: scale(0.97);
}

.photo img {
    display: block;
    width: 100%;
    aspect-ratio: 3 / 4;
    object-fit: cover;
}

.photo p {
    margin: 8px 0;
    font-size: 13px;
    color: #075985;
    font-weight: bold;
    padding: 0 4px;
    word-break: break-all;
}

.counter {
    color: #FFFFFF !important;
    font-weight: bold;
    margin-top: 20px;
    font-size: 18px;
}

.site-logo {
    width: 90%;
    max-width: 400px;
    height: auto;
    margin-bottom: 20px;
}

.progress-container {
    width: 100%;
    max-width: 400px;
    height: 12px;
    background: white;
    border-radius: 20px;
    margin: 15px auto;
    overflow: hidden;
}

.progress-bar {
    width: 0%;
    height: 100%;
    background: #FFF000;
    border-radius: 20px;
    transition: width 0.3s;
}

/* 予選通過者一覧 */
.preview-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
    margin-top: 15px;
    max-height: 350px;
    overflow-y: auto;
    padding: 5px;
}
.preview-card {
    background: white;
    border-radius: 10px;
    overflow: hidden;
}
.preview-card img {
    width: 100%;
    aspect-ratio: 3 / 4;
    object-fit: cover;
    display: block;
}
.preview-card p {
    margin: 5px 0;
    font-size: 11px;
    color: #075985;
    font-weight: bold;
}

/* 最終ランキング用（4:3の比率に調整） */
.rank-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 12px;
    margin-top: 20px;
}
.rank-card {
    background: white;
    border-radius: 12px;
    overflow: hidden;
    position: relative;
}
.rank-badge {
    position: absolute;
    top: 5px;
    left: 5px;
    background: #e88da5;
    color: white;
    padding: 2px 8px;
    font-weight: bold;
    border-radius: 6px;
    font-size: 12px;
    z-index: 2;
}
.rank-card img {
    width: 100%;
    aspect-ratio: 4 / 3; /* ご指定の4:3比率 */
    object-fit: cover;
    display: block;
}
.rank-card p {
    margin: 6px 0;
    font-size: 12px;
    color: #075985;
    font-weight: bold;
}

.share-btn {
    background: #000000;
    color: white;
    margin-right: 10px;
}
.share-btn:hover {
    background: #333333;
}
</style>
</head>

<body>

<div class="container">
    <img src="project_logo.png" class="site-logo" alt="PROJECT">

    <h1>PROJECT 7 好き顔9選</h1>

    <!-- ホーム画面 -->
    <section id="home">
        <p>99人の中から、あなたの好きな顔を見つけよう。</p>
        <button onclick="startGame()">START</button>
    </section>

    <!-- ROUND 1 (予選) -->
    <section id="game" class="hidden">
        <p class="round" id="round"></p>
        <div class="progress-container">
            <div class="progress-bar" id="progressBar"></div>
        </div>
        <h2>WHO IS YOUR FAVORITE?</h2>
        <p>好きな人を何人でも選んでね♡</p>

        <div class="photo-grid" id="photoGrid"></div>

        <p class="counter" id="counter">現在 0人選択中</p>
        <button onclick="nextRound1()">NEXT</button>
    </section>

    <!-- ラウンド1終了画面 -->
    <section id="round1Complete" class="hidden">
        <h2>🎉 ROUND 1 終了！ 🎉</h2>
        <p id="round1ResultText">予選で選ばれたメンバーはこちら！</p>
        <div class="preview-grid" id="round1PreviewGrid"></div>
        <button onclick="startRound2()">ROUND 2 (TOP 9選抜) へ進む</button>
    </section>

    <!-- ROUND 2 (TOP 9選抜) -->
    <section id="round2" class="hidden">
        <p class="round" id="round2Title"></p>
        <div class="progress-container">
            <div class="progress-bar" id="round2ProgressBar"></div>
        </div>
        <h2>WHO IS YOUR FAVORITE?</h2>
        <p>この中で好きな人を1人選んでね！</p>
        <div class="choice-grid" id="round2Grid"></div>
    </section>

    <!-- ラウンド2終了画面 -->
    <section id="round2Complete" class="hidden">
        <h2>✨ ROUND 2 終了！ ✨</h2>
        <p>TOP 9のメンバーが決定しました！続いて順位決定戦に進みます。</p>
        <button onclick="startRound3()">順位決定戦 (ROUND 3) へ進む</button>
    </section>

    <!-- 同票決着画面 -->
    <section id="tieBreak" class="hidden">
        <p class="round">同票決定戦</p>
        <p id="tieBreakText">票が並んだ方の中から選んでください！</p>
        <div class="choice-grid" id="tieBreakGrid"></div>
    </section>

    <!-- ROUND 3 (順位決定戦：二択) -->
    <section id="round3" class="hidden">
        <p class="round" id="round3Title"></p>
        <div class="progress-container">
            <div class="progress-bar" id="round3ProgressBar"></div>
        </div>
        <h2>どっちの顔が好き？</h2>
        <div class="choice-grid" id="round3Grid"></div>
    </section>

    <!-- 最終結果画面 -->
    <section id="finish" class="hidden">
        <h2>🎉 TOP 9 決定！ 🎉</h2>
        <p>あなたの好き顔ベスト9はこちらです！</p>
        <p style="font-size: 13px; background: rgba(0,0,0,0.15); padding: 8px; border-radius: 8px;">
            💡 スマホの場合は画像を長押しして保存できるよ！
        </p>
        <div class="rank-grid" id="rankGrid"></div>
        
        <div style="margin-top:20px;">
            <button class="share-btn" onclick="shareToX()">𝕏 で結果をシェア</button>
            <button onclick="startGame()">もう一度遊ぶ</button>
        </div>
    </section>

</div>

<script>
let groups = [];
let currentRound = 0;
let selected = new Set();
let votes = {};
let totalSelectedCount = 0; // 予選全体で選んだ総人数を記録

const names = [
    "アベ ユラ", "キム・ヨンフン", "クォン・ヨンヒョン", "カン・ワンソク", "キム・ユンス", "キム・テソン", "マージンシャン", "スヴィアト", "ユ・ヒョンスン", "イ・ジュチャン",
    "チョン・ミヌク", "ツジイ リョウマ", "イチェン", "カン・ジミン", "キム・ウノ", "キム・テユ", "パク・ジュンソ", "シン・ジェウォン", "ユ・ヒド", "イ・ジュンソン",
    "チョン・ドゥンヘソル", "パンアティラ", "カン・ヒョヌ", "キム・ジョンミン", "キム・ヒョヌ", "パク・ジュヌ", "アサカ コウタロウ", "ユン・ジュハン", "イ・ジフン", "チョン・セユン",
    "ペッ", "カン・ヒミン", "キム・ジョンユン", "キム・ヒョンジェ", "パク・チャニョン", "アン・ジュンウォン", "キム・ゴヌ", "イ・ハンビン", "チョン・スンウォン", "ハ・ソクヒ",
    "コ・ミンソン", "キム・ジュヒョン", "ナム・ジウン", "ベク・ジホ", "アヤロン アダム", "イ・ノユル", "イ・ヒョビン", "チョ・ヒョジン", "ホウカンイ", "クォン・ヤンウ",
    "キム・ジュヌ", "リンリン", "ビンファ", "アンディー", "イ・ユジュン", "イム・シウ", "チェ・ヒジュ", "フクヤマ ソウタ", "クォン・イェウン", "キム・ジミン",
    "リンウェイチョン", "サクラダ ケンシン", "ヤマグチサトシ", "イ・ドギョン", "イム・ヒョヌ", "チェ・ジョンフン", "キム・ダヌ", "サンタ", "ヤン・ジュホ", "チャン・ギョンヨン",
    "キム・ドフン", "ソ・ギョンベ", "オウム", "チャン・ヨジュン", "キム・ミンジュン", "ソ・ジウォン", "ヨム・イェチャン", "チャン・インジェ", "キム・ソンミン", "ソン・スンホ",
    "オオバヤシ ユウセイ", "キム・ソンジュン", "オ・スンチャン", "キム・シユル", "オ・ヨンウン", "キム・シフン", "オ・テファン", "キム・ジファン", "オク・チャンヒョン", "マナベ ジン",
    "ウチェンユ", "ソン・ヒョンソク", "ウ・ハジュン", "ユ・ヨンソ", "ユ・ジアン", "イ・ウンソ", "チャン・ヒョンソク", "チェ・ジュヨン", "カン・ミンソ"
];

function shuffle(array) {
    return array.sort(() => Math.random() - 0.5);
}

function imagePath(number) {
    if (number === 1) {
        return "images/image (1).webp";
    }
    return `images/image (${number}).jpg`;
}

// ==================== ROUND 1 ====================
function startGame() {
    const numbers = shuffle(Array.from({length: 99}, (_, i) => i + 1));

    groups = [];
    for (let i = 0; i < 99; i += 9) {
        groups.push(numbers.slice(i, i + 9));
    }

    currentRound = 0;
    votes = {};
    totalSelectedCount = 0;

    document.getElementById("home").classList.add("hidden");
    document.getElementById("round1Complete").classList.add("hidden");
    document.getElementById("round2").classList.add("hidden");
    document.getElementById("round2Complete").classList.add("hidden");
    document.getElementById("tieBreak").classList.add("hidden");
    document.getElementById("round3").classList.add("hidden");
    document.getElementById("finish").classList.add("hidden");
    document.getElementById("game").classList.remove("hidden");

    showRound1();
}

function showRound1() {
    selected.clear();

    document.getElementById("round").textContent = `ROUND 1 (予選)：${currentRound + 1} / 11`;
    document.getElementById("progressBar").style.width = `${((currentRound + 1) / 11) * 100}%`;

    const grid = document.getElementById("photoGrid");
    grid.innerHTML = "";

    groups[currentRound].forEach(number => {
        const card = document.createElement("div");
        card.className = "photo";

        const img = document.createElement("img");
        img.src = imagePath(number);
        img.alt = `Image ${number}`;
        img.onerror = () => { img.alt = "画像を読み込めません"; };

        const label = document.createElement("p");
        label.textContent = names[number - 1];

        card.appendChild(img);
        card.appendChild(label);

        card.addEventListener("click", () => {
            if (selected.has(number)) {
                selected.delete(number);
                card.classList.remove("selected");
            } else {
                selected.add(number);
                card.classList.add("selected");
            }
            updateCounterText();
        });

        grid.appendChild(card);
    });

    updateCounterText();
}

function updateCounterText() {
    // 現在のラウンドでの選択数 ＋ これまでのラウンドの総数（簡易表示としてその場での合計も計算可能）
    // ここでは「これまでの総計」を分かりやすくするために、クリックするたびに集計し直す方式にします
    let currentSelectedTotal = 0;
    // votesに今回の選択を仮足しした総数を計算
    let tempVotes = {...votes};
    selected.forEach(num => {
        tempVotes[num] = (tempVotes[num] || 0) + 1;
    });
    totalSelectedCount = Object.keys(tempVotes).length;

    document.getElementById("counter").textContent = `現在選択中：${selected.in} / 総合計ピックアップ：${totalSelectedCount}人`;
    // 少し分かりやすく修正：
    document.getElementById("counter").textContent = `この画面で${selected.size}人選択中 （累計選択：${Object.keys(votes).length + selected.size}人）`;
}

function nextRound1() {
    selected.forEach(number => {
        votes[number] = (votes[number] || 0) + 1;
    });

    currentRound++;

    if (currentRound < 11) {
        showRound1();
    } else {
        r2Candidates = Object.keys(votes).map(Number);
        if (r2Candidates.length < 9) {
            alert("ROUND 1で選ばれた人数が9人未満です。もう少し多めに選んでやり直してください！");
            startGame();
            return;
        }

        document.getElementById("game").classList.add("hidden");
        document.getElementById("round1ResultText").textContent = `予選突破：合計 ${r2Candidates.length} 人`;
        
        const previewGrid = document.getElementById("round1PreviewGrid");
        previewGrid.innerHTML = "";

        r2Candidates.forEach(number => {
            const card = document.createElement("div");
            card.className = "preview-card";

            const img = document.createElement("img");
            img.src = imagePath(number);

            const label = document.createElement("p");
            label.textContent = names[number - 1];

            card.appendChild(img);
            card.appendChild(label);
            previewGrid.appendChild(card);
        });

        document.getElementById("round1Complete").classList.remove("hidden");
    }
}

// ==================== ROUND 2 (TOP 9選抜) ====================
let r2Candidates = [];
let r2Scores = {};
let r2Queue = [];
let currentR2Index = 0;
let top9 = [];

function startRound2() {
    document.getElementById("round1Complete").classList.add("hidden");
    document.getElementById("round2").classList.remove("hidden");

    r2Scores = {};
    r2Candidates.forEach(id => r2Scores[id] = 0);

    r2Queue = [];
    let pool = [];
    for (let i = 0; i < 3; i++) {
        pool = pool.concat(shuffle([...r2Candidates]));
    }
    
    while (pool.length > 0) {
        let size = Math.min(Math.floor(Math.random() * 2) + 3, pool.length);
        if (pool.length === 5) size = 3;
        
        let group = [];
        let usedInGroup = new Set();
        
        while (group.length < size && pool.length > 0) {
            let candidate = pool.pop();
            if (!usedInGroup.has(candidate)) {
                usedInGroup.add(candidate);
                group.push(candidate);
            }
        }
        if (group.length > 0) {
            r2Queue.push(group);
        }
    }

    currentR2Index = 0;
    showR2Step();
}

function showR2Step() {
    if (currentR2Index >= r2Queue.length) {
        decideTop9();
        return;
    }

    const progress = (currentR2Index / r2Queue.length) * 100;
    document.getElementById("round2ProgressBar").style.width = `${progress}%`;
    document.getElementById("round2Title").textContent = `ROUND 2 (TOP 9選抜)：${currentR2Index + 1} / ${r2Queue.length}`;

    const grid = document.getElementById("round2Grid");
    grid.innerHTML = "";

    const currentGroup = r2Queue[currentR2Index];
    currentGroup.forEach(number => {
        const card = document.createElement("div");
        card.className = "photo";

        const img = document.createElement("img");
        img.src = imagePath(number);
        const label = document.createElement("p");
        label.textContent = names[number - 1];

        card.appendChild(img);
        card.appendChild(label);

        card.addEventListener("click", () => {
            r2Scores[number] = (r2Scores[number] || 0) + 1;
            currentR2Index++;
            showR2Step();
        });

        grid.appendChild(card);
    });
}

function decideTop9() {
    let sorted = Object.keys(r2Scores).map(Number).sort((a, b) => r2Scores[b] - r2Scores[a]);

    if (sorted.length <= 9) {
        top9 = sorted;
        showRound2Complete();
        return;
    }

    let thresholdScore = r2Scores[sorted[8]];
    let safeGroup = sorted.slice(0, 9).filter(id => r2Scores[id] > thresholdScore);
    let tieGroup = sorted.filter(id => r2Scores[id] === thresholdScore);
    let neededCount = 9 - safeGroup.length;

    if (tieGroup.length === neededCount) {
        top9 = safeGroup.concat(tieGroup);
        showRound2Complete();
    } else {
        top9 = safeGroup;
        startTieBreak(tieGroup, neededCount, (selectedSubset) => {
            top9 = top9.concat(selectedSubset);
            showRound2Complete();
        });
    }
}

function startTieBreak(candidates, count, callback) {
    document.getElementById("round2").classList.add("hidden");
    document.getElementById("tieBreak").classList.remove("hidden");
    
    let chosen = [];
    
    function renderTieStep() {
        if (chosen.length >= count || candidates.length === 0) {
            document.getElementById("tieBreak").classList.add("hidden");
            callback(chosen);
            return;
        }

        document.getElementById("tieBreakText").textContent = `同票の方が複数います。あと ${count - chosen.length} 人選んでください！`;
        const grid = document.getElementById("tieBreakGrid");
        grid.innerHTML = "";

        let displayGroup = candidates.slice(0, 4);

        displayGroup.forEach((number) => {
            const card = document.createElement("div");
            card.className = "photo";
            const img = document.createElement("img");
            img.src = imagePath(number);
            const label = document.createElement("p");
            label.textContent = names[number - 1];

            card.appendChild(img);
            card.appendChild(label);

            card.addEventListener("click", () => {
                chosen.push(number);
                let idx = candidates.indexOf(number);
                if (idx > -1) candidates.splice(idx, 1);
                renderTieStep();
            });
            grid.appendChild(card);
        });
    }
    renderTieStep();
}

function showRound2Complete() {
    document.getElementById("round2").classList.add("hidden");
    document.getElementById("tieBreak").classList.add("hidden");
    document.getElementById("round2Complete").classList.remove("hidden");
}

// ==================== ROUND 3 (順位決定戦：二択) ====================
let r3Pairs = [];
let r3CurrentPairIndex = 0;
let r3Scores = {};

function startRound3() {
    document.getElementById("round2Complete").classList.add("hidden");
    document.getElementById("round3").classList.remove("hidden");

    r3Scores = {};
    top9.forEach(id => r3Scores[id] = 0);

    r3Pairs = [];
    let matchPool = [];
    top9.forEach(id => {
        let others = top9.filter(other => other !== id);
        shuffle(others);
        others.slice(0, 3).forEach(opponent => {
            let pair = id < opponent ? [id, opponent] : [opponent, id];
            if (!matchPool.some(p => p[0] === pair[0] && p[1] === pair[1])) {
                matchPool.push(pair);
            }
        });
    });

    r3Pairs = shuffle(matchPool);
    r3CurrentPairIndex = 0;
    showR3Pair();
}

function showR3Pair() {
    if (r3CurrentPairIndex >= r3Pairs.length) {
        finishGame();
        return;
    }

    const progress = (r3CurrentPairIndex / r3Pairs.length) * 100;
    document.getElementById("round3ProgressBar").style.width = `${progress}%`;
    document.getElementById("round3Title").textContent = `ROUND 3 (順位決定)：${r3CurrentPairIndex + 1} / ${r3Pairs.length}`;

    const grid = document.getElementById("round3Grid");
    grid.innerHTML = "";

    const pair = [...r3Pairs[r3CurrentPairIndex]];
    shuffle(pair);

    pair.forEach(number => {
        const card = document.createElement("div");
        card.className = "photo";
        const img = document.createElement("img");
        img.src = imagePath(number);
        const label = document.createElement("p");
        label.textContent = names[number - 1];

        card.appendChild(img);
        card.appendChild(label);

        card.addEventListener("click", () => {
            r3Scores[number] = (r3Scores[number] || 0) + 1;
            r3CurrentPairIndex++;
            showR3Pair();
        });
        grid.appendChild(card);
    });
}

// 最終結果の保持用
let finalSortedList = [];

// ==================== 最終結果発表 ====================
function finishGame() {
    document.getElementById("round3").classList.add("hidden");
    document.getElementById("finish").classList.remove("hidden");

    finalSortedList = [...top9].sort((a, b) => r3Scores[b] - r3Scores[a]);

    const rankGrid = document.getElementById("rankGrid");
    rankGrid.innerHTML = "";

    finalSortedList.forEach((number, index) => {
        const card = document.createElement("div");
        card.className = "rank-card";

        const badge = document.createElement("div");
        badge.className = "rank-badge";
        badge.textContent = `${index + 1}位`;

        const img = document.createElement("img");
        img.src = imagePath(number);

        const, label = document.createElement("p");
        // ※コード内のタイポ防止のため厳密に記述
        label.textContent = names[number - 1];

        card.appendChild(badge);
        card.appendChild(img);
        card.appendChild(label);
        rankGrid.appendChild(card);
    });
}

// X（Twitter）シェア機能
function shareToX() {
    let top3Names = finalSortedList.slice(0, 3).map((num, i) => `${i + 1}位: ${names[num - 1]}`).join("\n");
    let text = `私の「PROJECT 7 好き顔TOP 3」はこちら！\n\n${top3Names}\n\n#PROJECT7 #好き顔9選`;
    let url = `https://twitter.com/intent/tweet?text=${encodeURIComponent(text)}`;
    window.open(url, '_blank');
}
</script>

</body>
</html>

<img width="736" height="312" alt="project_logo" src="https://github.com/user-attachments/assets/28a86eb5-b7e7-4781-93d2-903a2321df3c" />

<img width="620" height="775" alt="image (2)" src="https://github.com/user-attachments/assets/056351f5-7458-4914-90bd-f021570b6834" />

