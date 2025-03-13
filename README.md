<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>砲丸投げゲーム</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            font-family: Arial, sans-serif;
            background: linear-gradient(135deg, #87CEEB, #E0FFFF);
            min-height: 100vh;
            overflow: hidden;
            user-select: none;
        }
        #character-select-screen, #game-screen {
            display: none;
            flex-direction: column;
            align-items: center;
            width: 100%;
            max-width: 600px;
        }
        #character-select-screen.active, #game-screen.active {
            display: flex;
        }
        #character-select {
            display: flex;
            gap: 20px;
            margin: 20px 0;
        }
        .char-option {
            width: 100px;
            height: 100px;
            border-radius: 10px;
            cursor: pointer;
            transition: transform 0.2s;
            border: 3px solid transparent;
        }
        .char-option:hover {
            transform: scale(1.1);
        }
        .selected {
            border-color: #FFD700;
        }
        #game-container {
            position: relative;
            width: 100%;
            height: 400px;
            background: linear-gradient(to bottom, #00B7EB 0%, #FFFFFF 70%, #F5E050 100%);
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.2);
            overflow: hidden;
            margin-top: 20px;
        }
        #shisa { /* ドット絵風シーサー */
            position: absolute;
            top: 10px;
            left: 10px;
            width: 80px;
            height: 80px;
            z-index: 1;
        }
        #shisa::before { /* 体 */
            content: '';
            position: absolute;
            width: 50px;
            height: 40px;
            background: #8B4513;
            top: 30px;
            left: 15px;
            border: 3px solid #654321;
            box-shadow: inset 5px 5px #A0522D;
        }
        #shisa::after { /* 頭 */
            content: '';
            position: absolute;
            width: 40px;
            height: 40px;
            background: #8B4513;
            border-radius: 50%;
            top: 0;
            left: 20px;
            border: 3px solid #654321;
            box-shadow: inset 5px 5px #A0522D;
        }
        #shisa-eyes { /* 目 */
            position: absolute;
            width: 10px;
            height: 10px;
            background: #FFFFFF;
            border-radius: 50%;
            top: 15px;
            left: 30px;
            box-shadow: 15px 0 #FFFFFF, 5px 5px #000000, 20px 5px #000000;
        }
        #pineapple { /* ドット絵風パイナップル */
            position: absolute;
            bottom: 60px;
            left: 10px;
            width: 70px;
            height: 90px;
            z-index: 1;
        }
        #pineapple::before { /* 実 */
            content: '';
            position: absolute;
            width: 40px;
            height: 60px;
            background: #FFD700;
            top: 30px;
            left: 15px;
            clip-path: polygon(50% 0%, 100% 20%, 80% 100%, 20% 100%, 0% 20%);
            border: 3px solid #DAA520;
            box-shadow: inset 5px 5px #FFA500;
        }
        #pineapple::after { /* 葉 */
            content: '';
            position: absolute;
            width: 30px;
            height: 40px;
            background: #228B22;
            top: 0;
            left: 20px;
            clip-path: polygon(50% 0%, 100% 100%, 0% 100%);
            border: 3px solid #006400;
            box-shadow: inset 5px 5px #32CD32;
        }
        #pineapple-detail { /* 実の模様 */
            position: absolute;
            width: 8px;
            height: 8px;
            background: #DAA520;
            top: 45px;
            left: 25px;
            box-shadow: 15px 0 #DAA520, 7px 15px #DAA520, 10px 25px #DAA520;
        }
        #hibiscus { /* ドット絵風ハイビスカス */
            position: absolute;
            top: 10px;
            right: 10px;
            width: 80px;
            height: 80px;
            z-index: 1;
        }
        #hibiscus::before { /* 花びら */
            content: '';
            position: absolute;
            width: 60px;
            height: 60px;
            background: #FF69B4;
            border-radius: 50%;
            top: 10px;
            left: 10px;
            clip-path: polygon(50% 0%, 100% 30%, 70% 100%, 30% 100%, 0% 30%);
            border: 3px solid #C71585;
            box-shadow: inset 5px 5px #FF1493;
        }
        #hibiscus::after { /* 中心 */
            content: '';
            position: absolute;
            width: 20px;
            height: 20px;
            background: #FFFF00;
            border-radius: 50%;
            top: 30px;
            left: 30px;
            border: 3px solid #FFD700;
            box-shadow: inset 3px 3px #FFA500;
        }
        #hibiscus-stamen { /* 雄しべ */
            position: absolute;
            width: 8px;
            height: 30px;
            background: #FF4500;
            top: 15px;
            left: 36px;
            border: 2px solid #CD5C5C;
        }
        #palm-tree { /* ドット絵風ヤシの木 */
            position: absolute;
            bottom: 0;
            right: 10px;
            width: 80px;
            height: 140px;
            z-index: 1;
        }
        #palm-tree::before { /* 幹 */
            content: '';
            position: absolute;
            width: 20px;
            height: 90px;
            background: #8B4513;
            bottom: 0;
            left: 30px;
            border: 3px solid #654321;
            box-shadow: inset 5px 5px #A0522D;
        }
        #palm-tree::after { /* 葉 */
            content: '';
            position: absolute;
            width: 70px;
            height: 70px;
            background: #228B22;
            top: 0;
            left: 5px;
            clip-path: polygon(50% 0%, 100% 50%, 70% 100%, 30% 100%, 0% 50%);
            border: 3px solid #006400;
            box-shadow: inset 5px 5px #32CD32;
        }
        #palm-tree-leaves { /* 追加の葉 */
            position: absolute;
            width: 50px;
            height: 50px;
            background: #228B22;
            top: 20px;
            left: 15px;
            clip-path: polygon(50% 0%, 100% 40%, 60% 100%, 40% 100%, 0% 40%);
            border: 3px solid #006400;
        }
        #character {
            position: absolute;
            bottom: 50px;
            left: 50%;
            transform: translateX(-50%);
            width: 80px;
            height: 80px;
            transition: transform 0.5s ease-out;
            z-index: 2;
        }
        #gauge-container {
            position: absolute;
            bottom: 20px;
            width: 50%;
            left: 25%;
            height: 20px;
            background: #ddd;
            border-radius: 10px;
            overflow: hidden;
            z-index: 2;
        }
        #gauge {
            width: 0;
            height: 100%;
            background: linear-gradient(90deg, #FFD700, #FF6347);
            animation: gaugeFill 2s infinite;
        }
        @keyframes gaugeFill {
            0% { width: 0; }
            100% { width: 100%; }
        }
        .button-container {
            display: flex;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
            margin: 20px 0;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            border: none;
            border-radius: 25px;
            background: #4CAF50;
            color: white;
            cursor: pointer;
            transition: transform 0.2s, background 0.2s;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        button:hover {
            transform: scale(1.05);
            background: #45a049;
        }
        #score {
            font-size: 24px;
            color: #333;
            margin: 10px 0;
        }
        #message {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 32px;
            color: #FFD700;
            text-shadow: 2px 2px 4px #333;
            display: none;
            z-index: 2;
            text-align: center; /* 改行時に中央揃え */
        }
    </style>
</head>
<body>
    <div id="character-select-screen" class="active">
        <h2>キャラクターを選択してください</h2>
        <div id="character-select">
            <img src="https://pbs.twimg.com/profile_images/1890585622500503552/rOXm_mUw_400x400.jpg" class="char-option" onclick="selectCharacter(this)">
            <img src="https://pbs.twimg.com/profile_images/1896637829989507072/xMy2qp-l_400x400.jpg" class="char-option" onclick="selectCharacter(this)">
        </div>
        <div class="button-container">
            <button onclick="startGameFromSelection()">スタート</button>
        </div>
    </div>
    <div id="game-screen">
        <div id="game-container">
            <div id="shisa"><div id="shisa-eyes"></div></div> <!-- ドット絵シーサー -->
            <div id="pineapple"><div id="pineapple-detail"></div></div> <!-- ドット絵パイナップル -->
            <div id="hibiscus"><div id="hibiscus-stamen"></div></div> <!-- ドット絵ハイビスカス -->
            <div id="palm-tree"><div id="palm-tree-leaves"></div></div> <!-- ドット絵ヤシの木 -->
            <img id="character" src="">
            <div id="gauge-container">
                <div id="gauge"></div>
            </div>
            <div id="message"></div>
        </div>
        <div class="button-container" id="game-buttons">
            <button onclick="stopAndJump()">ストップ</button>
        </div>
        <div class="button-container" id="retry-button" style="display: none;">
            <button onclick="restartGame()">もう一回</button>
            <button onclick="backToSelect()">戻る</button>
        </div>
        <div id="score">スコア: 0</div>
    </div>

    <script>
        let selectedChar = null;
        let isPlaying = false;
        let score = 0;
        const character = document.getElementById('character');
        const gauge = document.getElementById('gauge');
        const message = document.getElementById('message');
        const scoreDisplay = document.getElementById('score');
        const selectScreen = document.getElementById('character-select-screen');
        const gameScreen = document.getElementById('game-screen');
        const gameButtons = document.getElementById('game-buttons');
        const retryButton = document.getElementById('retry-button');

        function selectCharacter(element) {
            if (isPlaying) return;
            document.querySelectorAll('.char-option').forEach(opt => opt.classList.remove('selected'));
            element.classList.add('selected');
            selectedChar = element.src;
            character.src = selectedChar;
            character.style.display = 'block';
        }

        function startGameFromSelection() {
            if (!selectedChar) return;
            selectScreen.classList.remove('active');
            gameScreen.classList.add('active');
            startGame();
        }

        function startGame() {
            if (isPlaying) return;
            isPlaying = true;
            gauge.style.animationPlayState = 'running';
            gauge.style.width = '0';
            character.style.transform = 'translateX(-50%)';
            score = 0;
            scoreDisplay.textContent = `スコア: ${score}`;
            message.style.display = 'none';
            gameButtons.style.display = 'flex';
            retryButton.style.display = 'none';
        }

        function stopAndJump() {
            if (!isPlaying) return;
            isPlaying = false;
            gauge.style.animationPlayState = 'paused';

            const gaugeWidth = parseFloat(getComputedStyle(gauge).width);
            const containerWidth = gauge.parentElement.offsetWidth;
            const timingScore = Math.min(100, Math.floor((gaugeWidth / containerWidth) * 100));
            score = timingScore;
            scoreDisplay.textContent = `スコア: ${score}`;
            const jumpHeight = timingScore * 3;
            character.style.transform = `translate(-50%, -${jumpHeight}px)`;

            let messageText;
            if (score >= 99 && score < 100) {
                messageText = 'おめでとう！<br>しょーんもな𖤐';
            } else if (score >= 95 && score < 99) {
                messageText = '惜しい！やり直し！';
            } else if (score >= 90 && score < 95) {
                messageText = '惜しくもない！やり直し！';
            } else {
                messageText = 'やる気ある？ｗ';
            }
            message.innerHTML = messageText; // <br>を反映するためにinnerHTMLを使用

            message.style.display = 'block';

            gameButtons.style.display = 'none';
            retryButton.style.display = 'flex';

            setTimeout(() => {
                character.style.transform = 'translateX(-50%)';
            }, 500);
        }

        function restartGame() {
            isPlaying = false;
            startGame();
        }

        function backToSelect() {
            isPlaying = false;
            character.style.transform = 'translateX(-50%)';
            score = 0;
            scoreDisplay.textContent = `スコア: ${score}`;
            message.style.display = 'none';
            gauge.style.animationPlayState = 'running';
            gauge.style.width = '0';
            gameButtons.style.display = 'flex';
            retryButton.style.display = 'none';
            selectScreen.classList.add('active');
            gameScreen.classList.remove('active');
        }
    </script>
</body>
</html>
