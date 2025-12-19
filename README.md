<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>疲労診断ツール - あなたの疲労タイプと理想の体の状態を診断</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', Meiryo, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 0;
            color: #333;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            min-height: 100vh;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px 20px 20px;
            text-align: center;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .header h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        .header p {
            font-size: 13px;
            opacity: 0.95;
            line-height: 1.6;
        }

        .content {
            padding: 0;
        }

        .start-screen, .question-screen, .result-screen {
            display: none;
            min-height: calc(100vh - 120px);
        }

        .start-screen.active, .question-screen.active, .result-screen.active {
            display: block;
        }

        .start-screen {
            text-align: center;
            padding: 30px 20px;
        }

        .start-screen h2 {
            color: #667eea;
            margin-bottom: 20px;
            font-size: 22px;
            line-height: 1.5;
        }

        .notice-box {
            background: #fff3cd;
            border-left: 4px solid #ffc107;
            padding: 15px;
            margin: 20px 0;
            border-radius: 5px;
            text-align: left;
        }

        .notice-box strong {
            color: #856404;
            display: block;
            margin-bottom: 5px;
        }

        .feature-list {
            text-align: left;
            margin: 30px 0;
            padding: 0 10px;
        }

        .feature-list li {
            margin: 15px 0;
            padding-left: 30px;
            position: relative;
            line-height: 1.6;
        }

        .feature-list li:before {
            content: "✓";
            position: absolute;
            left: 0;
            color: #667eea;
            font-weight: bold;
            font-size: 20px;
        }

        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            border-radius: 50px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin: 20px auto;
            display: block;
            width: 90%;
            max-width: 300px;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        .btn:active {
            transform: translateY(0);
        }

        .question-screen {
            padding: 20px;
        }

        .phase-header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 15px 20px;
            margin: 0 -20px 20px;
            text-align: center;
            font-weight: bold;
            position: sticky;
            top: 120px;
            z-index: 50;
        }

        .question-item {
            background: #f8f9fa;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 25px;
            border-left: 4px solid #667eea;
        }

        .question-number {
            color: #667eea;
            font-size: 13px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .question-text {
            font-size: 16px;
            line-height: 1.7;
            margin-bottom: 20px;
            color: #333;
            font-weight: 500;
        }

        .slider-container {
            margin: 20px 0;
        }

        .slider-labels {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            font-size: 11px;
            color: #666;
        }

        .slider-labels span {
            text-align: center;
            line-height: 1.3;
        }

        .slider-labels span:first-child {
            text-align: left;
        }

        .slider-labels span:last-child {
            text-align: right;
        }

        input[type="range"] {
            width: 100%;
            height: 8px;
            border-radius: 5px;
            background: #e0e0e0;
            outline: none;
            -webkit-appearance: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            cursor: pointer;
            box-shadow: 0 2px 10px rgba(102, 126, 234, 0.5);
        }

        input[type="range"]::-moz-range-thumb {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            cursor: pointer;
            border: none;
            box-shadow: 0 2px 10px rgba(102, 126, 234, 0.5);
        }

        .slider-value {
            text-align: center;
            margin-top: 10px;
            font-size: 22px;
            font-weight: bold;
            color: #667eea;
        }

        .submit-button-container {
            position: sticky;
            bottom: 0;
            background: white;
            padding: 20px;
            margin: 0 -20px;
            box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
        }

        .btn-submit {
            width: 100%;
            max-width: none;
            margin: 0;
        }

        .result-screen {
            padding: 20px;
        }

        .result-screen h2 {
            color: #667eea;
            font-size: 24px;
            margin-bottom: 20px;
            text-align: center;
        }

        .result-type {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 25px 20px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 30px;
        }

        .result-type .subtitle {
            font-size: 15px;
            opacity: 0.95;
            margin-bottom: 15px;
            line-height: 1.5;
        }

        .result-type h3 {
            font-size: 26px;
            margin-bottom: 10px;
            font-weight: bold;
        }

        .result-type .description {
            font-size: 13px;
            opacity: 0.9;
            margin-top: 15px;
            line-height: 1.6;
            padding-top: 15px;
            border-top: 1px solid rgba(255, 255, 255, 0.3);
        }

        .character-image {
            width: 150px;
            height: 150px;
            margin: 20px auto;
            border-radius: 50%;
            border: 4px solid white;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .radar-chart-container {
            background: white;
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
        }

        .radar-chart-container h4 {
            color: #667eea;
            font-size: 16px;
            margin-bottom: 15px;
            text-align: center;
        }

        .score-bars {
            margin: 20px 0;
        }

        .score-bar-item {
            margin-bottom: 15px;
        }

        .score-bar-label {
            display: flex;
            justify-content: space-between;
            margin-bottom: 5px;
            font-size: 12px;
            color: #555;
        }

        .score-bar-label .name {
            font-weight: bold;
            flex: 1;
        }

        .score-bar-label .value {
            color: #667eea;
            font-weight: bold;
        }

        .score-bar-bg {
            background: #e0e0e0;
            height: 20px;
            border-radius: 10px;
            overflow: hidden;
        }

        .score-bar-fill {
            height: 100%;
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            border-radius: 10px;
            transition: width 0.5s ease;
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 8px;
            color: white;
            font-size: 11px;
            font-weight: bold;
        }

        .score-section-title {
            font-size: 14px;
            color: #667eea;
            font-weight: bold;
            margin: 20px 0 10px;
            padding-bottom: 5px;
            border-bottom: 2px solid #667eea;
        }

        .result-section {
            margin-bottom: 25px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
            border-left: 4px solid #667eea;
        }

        .result-section h4 {
            color: #667eea;
            font-size: 17px;
            margin-bottom: 15px;
        }

        .result-section p {
            line-height: 1.8;
            color: #555;
            font-size: 15px;
        }

        .result-section ul {
            line-height: 1.8;
            color: #555;
            padding-left: 20px;
        }

        .result-section li {
            margin: 10px 0;
        }

        .action-buttons {
            display: flex;
            flex-direction: column;
            gap: 12px;
            margin-top: 30px;
            margin-bottom: 30px;
        }

        .btn-copy, .btn-restart {
            width: 100%;
        }

        .btn-copy {
            background: #28a745;
        }

        .toast {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: #28a745;
            color: white;
            padding: 15px 30px;
            border-radius: 50px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.3);
            opacity: 0;
            transition: opacity 0.3s;
            z-index: 1000;
        }

        .toast.show {
            opacity: 1;
        }

        @media (max-width: 480px) {
            .header h1 {
                font-size: 20px;
            }

            .question-text {
                font-size: 15px;
            }

            .slider-labels {
                font-size: 10px;
            }

            .score-bar-label .name {
                font-size: 11px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🌟 疲労診断ツール</h1>
            <p>あなたの疲労タイプと理想の状態を診断します</p>
        </div>

        <div class="content">
            <!-- スタート画面 -->
            <div class="start-screen active" id="startScreen">
                <h2>疲労の原因を特定し、<br>理想の体の状態を手に入れる</h2>
                
                <div class="notice-box">
                    <strong>💡 より正確な疲労原因の特定には</strong>
                    5分無料診断を推奨します
                </div>

                <ul class="feature-list">
                    <li>12の質問で疲労の原因を科学的に分析</li>
                    <li>あなたの理想の体の状態を明確化</li>
                    <li>9つのパターンから最適な改善プランを提案</li>
                    <li>所要時間: 約5分</li>
                </ul>
                <button class="btn" onclick="startDiagnosis()">診断を始める</button>
            </div>

            <!-- 質問画面 -->
            <div class="question-screen" id="questionScreen">
                <div class="phase-header" id="phaseHeader1">
                    📊 フェーズ1: 疲労の原因を特定
                </div>

                <!-- Q1 -->
                <div class="question-item">
                    <div class="question-number">質問 1 / 12</div>
                    <div class="question-text">朝、目覚めても疲れが残っており、日中も体が重い、だるいと感じる。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="0" oninput="updateSlider(0)">
                        <div class="slider-value" id="value-0">3</div>
                    </div>
                </div>

                <!-- Q2 -->
                <div class="question-item">
                    <div class="question-number">質問 2 / 12</div>
                    <div class="question-text">運動や活動を増やしていないのに、慢性的な肩こりや腰の痛みに悩んでいる。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="1" oninput="updateSlider(1)">
                        <div class="slider-value" id="value-1">3</div>
                    </div>
                </div>

                <!-- Q3 -->
                <div class="question-item">
                    <div class="question-number">質問 3 / 12</div>
                    <div class="question-text">仕事中や会話中、集中力が急に途切れたり、物忘れや簡単なミスが増えた。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="2" oninput="updateSlider(2)">
                        <div class="slider-value" id="value-2">3</div>
                    </div>
                </div>

                <!-- Q4 -->
                <div class="question-item">
                    <div class="question-number">質問 4 / 12</div>
                    <div class="question-text">夜中に目が覚める、寝つきが悪いなど、睡眠の質が悪く、常に頭が休まらない。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="3" oninput="updateSlider(3)">
                        <div class="slider-value" id="value-3">3</div>
                    </div>
                </div>

                <!-- Q5 -->
                <div class="question-item">
                    <div class="question-number">質問 5 / 12</div>
                    <div class="question-text">便秘や下痢などのお通じの不調、または風邪や口内炎が治りにくい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="4" oninput="updateSlider(4)">
                        <div class="slider-value" id="value-4">3</div>
                    </div>
                </div>

                <!-- Q6 -->
                <div class="question-item">
                    <div class="question-number">質問 6 / 12</div>
                    <div class="question-text">以前に比べ、何をしても疲れが抜けにくく、肌荒れや体重増加が起こりやすい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="5" oninput="updateSlider(5)">
                        <div class="slider-value" id="value-5">3</div>
                    </div>
                </div>

                <div class="phase-header" style="margin-top: 30px;">
                    🎯 フェーズ2: 理想の状態を明確化
                </div>

                <!-- Q7 -->
                <div class="question-item">
                    <div class="question-number">質問 7 / 12</div>
                    <div class="question-text">疲れの原因を明確に特定し、迷わず回復できる科学的な方法を知りたい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="6" oninput="updateSlider(6)">
                        <div class="slider-value" id="value-6">3</div>
                    </div>
                </div>

                <!-- Q8 -->
                <div class="question-item">
                    <div class="question-number">質問 8 / 12</div>
                    <div class="question-text">仕事や趣味で最高の集中力とパフォーマンスを長時間維持したい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="7" oninput="updateSlider(7)">
                        <div class="slider-value" id="value-7">3</div>
                    </div>
                </div>

                <!-- Q9 -->
                <div class="question-item">
                    <div class="question-number">質問 9 / 12</div>
                    <div class="question-text">ストレスやイライラから解放され、心に余裕をもってリラックスしたい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="8" oninput="updateSlider(8)">
                        <div class="slider-value" id="value-8">3</div>
                    </div>
                </div>

                <!-- Q10 -->
                <div class="question-item">
                    <div class="question-number">質問 10 / 12</div>
                    <div class="question-text">自分の健康状態をコントロールできているという安心感と自信を得たい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="9" oninput="updateSlider(9)">
                        <div class="slider-value" id="value-9">3</div>
                    </div>
                </div>

                <!-- Q11 -->
                <div class="question-item">
                    <div class="question-number">質問 11 / 12</div>
                    <div class="question-text">見た目が若々しく、エネルギッシュで、周りから「疲れていないね」と言われたい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="10" oninput="updateSlider(10)">
                        <div class="slider-value" id="value-10">3</div>
                    </div>
                </div>

                <!-- Q12 -->
                <div class="question-item">
                    <div class="question-number">質問 12 / 12</div>
                    <div class="question-text">仕事や社会的な役割で、常に最高の状態でいる理想の自分を実現したい。</div>
                    <div class="slider-container">
                        <div class="slider-labels">
                            <span>全く<br>当てはまらない<br>(1)</span>
                            <span></span>
                            <span></span>
                            <span></span>
                            <span>非常に<br>当てはまる<br>(5)</span>
                        </div>
                        <input type="range" min="1" max="5" value="3" class="slider" data-index="11" oninput="updateSlider(11)">
                        <div class="slider-value" id="value-11">3</div>
                    </div>
                </div>

                <div class="submit-button-container">
                    <button class="btn btn-submit" onclick="showResult()">診断結果を見る</button>
                </div>
            </div>

            <!-- 結果画面 -->
            <div class="result-screen" id="resultScreen">
                <h2>🎯 診断結果</h2>
                
                <div class="result-type" id="resultType"></div>

                <!-- スコア表示 -->
                <div class="radar-chart-container">
                    <h4>📊 あなたのスコア</h4>
                    
                    <div class="score-section-title">あなたの疲労度レベル</div>
                    <div class="score-bars" id="fatigueScores"></div>
                    
                    <div class="score-section-title">あなたの理想のコンディション</div>
                    <div class="score-bars" id="jobScores"></div>
                </div>

                <div class="result-section">
                    <h4>😓 あなたの疲労の原因</h4>
                    <p id="fatigueReason"></p>
                </div>

                <div class="result-section">
                    <h4>✨ あなたの理想の状態</h4>
                    <p id="idealStatus"></p>
                </div>

                <div class="result-section">
                    <h4>🔬 おすすめのアプローチ</h4>
                    <p id="recommendedApproach"></p>
                </div>

                <div class="result-section">
                    <h4>💡 具体的なリカバリー方法</h4>
                    <p id="recoveryMethod"></p>
                </div>

                <div class="action-buttons">
                    <button class="btn btn-copy" onclick="copyResult()">📋 結果をコピーして共有</button>
                    <button class="btn btn-restart" onclick="restartDiagnosis()">🔄 もう一度診断する</button>
                </div>
            </div>
        </div>
    </div>

    <div class="toast" id="toast">結果をコピーしました！</div>

    <script>
        let answers = [3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3];

        // 診断結果データ（PDFから取得）
        const resultData = {
            "body-functional": {
                name: "結果コミット・プレイヤー",
                subtitle: "結果を出したいストイックな努力家",
                description: "体力消耗が激しいけれど、目標達成のために「結果」を追求し続ける頑張り屋。体をメンテナンスすれば、より早くゴールに辿り着けると示唆。",
                fatigueReason: "常に高い活動量を維持しているため、骨格筋や細胞レベルでの物理的な消耗が激しい状態です。インボディで測定される体年齢の上昇や、リカバリー不足による慢性的な体のダルさとなって現れています。",
                idealStatus: "疲労による体のブレをなくし、最高の集中力と持久力で目標を達成する、タフで結果を出せる自分。",
                approach: "疲労回復効率の最大化とパフォーマンスの持続です。必須栄養素サプリメントとプロテインで、消耗した体を速やかに修復する必要があります。インボディで体組成の改善を定期的に確認しましょう。",
                recovery: "質の高いタンパク質摂取と軽めの有酸素運動（週2回程度）を習慣化し、運動後のクールダウンとストレッチを欠かさないようにしましょう。"
            },
            "body-emotional": {
                name: "安心リセット・ヒーラー",
                subtitle: "頑張り屋さんで休めない",
                description: "身体は限界なのに無理をしてしまう優しい人。「安心感」という土台を取り戻せば、心の平穏を得られることを強調。",
                fatigueReason: "肉体的な疲労が、「休まなければならない」という心の不安を引き起こしています。体が鉛のように重いにもかかわらず、休むことに罪悪感を覚えるため、疲労が蓄積し続けています。",
                idealStatus: "身体の不調から解放され、心からリラックスできる状態を取り戻すこと。自分自身を労る時間を持ち、穏やかな心の平穏を得たい。",
                approach: "「休むこと」への許可と神経系の安定です。抗ストレスサプリメントやリラックスを促す栄養素を導入し、体をゆるめるサポートをしましょう。",
                recovery: "深呼吸やマインドフルネスを毎日のルーティンに取り入れましょう。寝る1時間前のスマホ断ちと、アロマや入浴で副交感神経を優位にすることを徹底してください。"
            },
            "body-social": {
                name: "完璧ビジュアル・モデル",
                subtitle: "華やかな見た目の裏で努力を怠らない",
                description: "外見や役割を完璧に演じるため、「体力」を犠牲にしているタイプ。体力を回復すれば、「理想の見た目」がさらに輝くと訴求。",
                fatigueReason: "「常に美しく、疲れていない人」でいるための無理な自己管理や活動が、体力の限界を招いています。体の疲労が外見の輝きを失わせることへの強い不安を抱えています。",
                idealStatus: "疲れを感じさせないエネルギッシュで若々しい外見を維持し、社会的な役割を完璧に果たす自信を持った自分。",
                approach: "外見維持に必要なインナーケアの強化です。高品質プロテインや体組成改善サプリで土台を整え、最新スキンケア・美容機器で外側からも徹底ケアするトータル美容プランが必須です。",
                recovery: "睡眠時間を削るのではなく、質の高い睡眠（7時間以上）を確保してください。また、美容に効果的なビタミンやコラーゲンなどを積極的に摂取しましょう。"
            },
            "brain-functional": {
                name: "集中力プロフェッサー",
                subtitle: "思考を止めない知的な戦略家",
                description: "常に「集中力」と「効率」を追求し、脳を酷使しているクリエイティブな頑張り屋。脳の休息が、より高度な集中力を生むと示唆。",
                fatigueReason: "「思考を止められない」状態が続き、脳がオーバーヒートしています。これはHRV測定で見る自律神経の乱れに直結し、結果として集中力や判断力の低下を招いています。",
                idealStatus: "途切れることのない高度な集中力とブレない思考力を発揮し、目標を効率的に達成し続ける知的でタフな自分。",
                approach: "脳の酸化ストレスの軽減と神経伝達物質のサポートです。抗酸化サプリメントとオメガ3系脂肪酸の摂取が最優先です。HRV測定で「脳の休息度」を客観的に把握することが重要です。",
                recovery: "ポモドーロ・テクニック（集中と休憩のサイクル）を導入し、意識的に脳を休ませてください。作業中も1時間に1回、遠くを見て目を休ませる習慣をつけましょう。"
            },
            "brain-emotional": {
                name: "穏やかメンタル・サポーター",
                subtitle: "繊細な心を持つ共感体質",
                description: "周囲の環境や感情に敏感で、「心」が疲れてしまう優しい人。自分自身の心を守ることで、さらに優しくなれることを示唆。",
                fatigueReason: "他者の感情や環境の刺激に敏感なため、精神的なエネルギー消耗が激しく、心の安定が保てない状態です。小さなストレスでも不安やイライラに繋がりやすいです。",
                idealStatus: "外部の刺激に左右されず、常に心の平穏を保つこと。情緒的に安定し、自分を大切にする余裕を持った穏やかな自分。",
                approach: "神経系の安定化と栄養サポートです。マグネシウムやビタミンB群など、感情安定をサポートするサプリメントが有効です。また、「心の安全基地」を作るためのセルフケアが必要です。",
                recovery: "デジタル機器から離れる時間（特に週末）を設定しましょう。日記や感謝のリストを書くなど、頭の中の感情を言語化する習慣を持つことで、心の負荷が軽減されます。"
            },
            "brain-social": {
                name: "ハイグレード・CEO",
                subtitle: "多忙な毎日を乗りこなす",
                description: "「最高のパフォーマンス」を発揮し、常にエネルギッシュに見られることを望むリーダー気質。脳のメンテナンスでその状態を維持できると訴求。",
                fatigueReason: "「常に最高の自分」でいるため、疲労を隠して行動し続けています。脳が休む間がないため、急な集中力の低下や感情のコントロール不能といった形で限界が表面化しています。",
                idealStatus: "疲れを感じさせない高いエネルギーレベルを維持し、社会的な役割やキャリアにおいて、常に期待を上回る最高の成果を出せる自分。",
                approach: "リカバリーの最適化とエネルギー効率の向上です。高性能ストレスケアサプリメントで脳をリセットし、HRV測定で自律神経をチェックしながら、最高のパフォーマンスを発揮するための土台を築きましょう。",
                recovery: "毎日の入浴やサウナなどで、意識的に体を温める習慣を持ち、自律神経の切り替えを促しましょう。昼間に15分程度のパワーナップ（仮眠）を取り入れるのも効果的です。"
            },
            "organ-functional": {
                name: "代謝アップ・オーガナイザー",
                subtitle: "中身から変えて効率を上げたい",
                description: "栄養や健康に気を使うが、その努力が「内側」に反映されにくい体質。代謝を整えれば、すべての努力が報われることを強調。",
                fatigueReason: "内臓や腸の機能が低下しているため、栄養を効率よくエネルギーに変換できていません。努力しているのに体脂肪が落ちにくいなど、代謝の停滞が顕著です。プリズムioのスコアが低く出やすい状態です。",
                idealStatus: "代謝効率が最高の状態で、パフォーマンスが爆発的に向上し、疲労とは無縁のパワフルな自分。",
                approach: "代謝機能の根本改善と栄養効率の最大化です。腸内環境ケアサプリとビタミン・ミネラルサプリでエネルギー生産の土台を築きましょう。プリズムio測定で栄養の充足度を可視化し、アプローチを最適化します。",
                recovery: "朝一番にコップ一杯の水を飲み、腸を目覚めさせましょう。加工食品や小麦粉の摂取を減らし、食物繊維を意識的に多く摂る食生活に切り替えてください。"
            },
            "organ-emotional": {
                name: "守ってあげたいデリケートさん",
                subtitle: "お腹の調子が心に直結",
                description: "内臓の不調からくる「不安」や「体調不良」に敏感なタイプ。内側を整えることで、「心の安定」が得られると訴求。",
                fatigueReason: "内臓の不調（特に胃腸）が、不安やストレスとなって脳に伝わる「脳腸相関」の影響を受けています。内臓の不調が心の不安定さや漠然とした不安を引き起こしている状態です。",
                idealStatus: "内臓からくる体調の波がなくなり、常に安定した気分でいられること。内側から整うことで、心穏やかな毎日を手に入れたい。",
                approach: "脳腸相関を整えるインナーケアが重要です。消化器系をサポートするサプリメントや良質なプロバイオティクスを最優先で導入し、内臓の負担を減らしましょう。",
                recovery: "食事をゆっくり噛んで食べることを意識し、消化器官への負担を減らしましょう。温かい飲み物（ハーブティーなど）を習慣にし、体を内側から温めてください。"
            },
            "organ-social": {
                name: "エレガンス・インナービューティ",
                subtitle: "気品ある審美家",
                description: "「内側からの美」を追求し、結果を外見に反映させたい論理的なタイプ。内臓ケアこそが「揺るがない美しさの設計図」だと訴求。",
                fatigueReason: "内臓・代謝機能の低下が、肌のくすみ、むくみ、体型の崩れといった外見の悩みの根本原因となっています。外見を整える努力が、根本の不調によって報われていない状態です。",
                idealStatus: "内側から輝く揺るがない美しさを手に入れ、社会的な場面で常に最高の状態でいることを実現したい。",
                approach: "インナーケアとスキンケアの連携です。プリズムio測定で得た栄養データを元に、代謝改善サプリと美容サプリを設計し、美容製品で外側のケアも万全にする統合型プランが必要です。",
                recovery: "冷たい飲み物や食品を避け、体を冷やさない食生活を意識してください。週に数回、半身浴などで代謝を促し、水分循環を良くしましょう。"
            }
        };

        function startDiagnosis() {
            document.getElementById('startScreen').classList.remove('active');
            document.getElementById('questionScreen').classList.add('active');
            window.scrollTo(0, 0);
        }

        function updateSlider(index) {
            const slider = document.querySelectorAll('.slider')[index];
            const value = slider.value;
            answers[index] = parseInt(value);
            document.getElementById(`value-${index}`).textContent = value;
        }

        function calculateResult() {
            // 疲労の原因を計算
            const bodyScore = answers[0] + answers[1];
            const brainScore = answers[2] + answers[3];
            const organScore = answers[4] + answers[5];

            // 顧客のジョブを計算
            const functionalScore = answers[6] + answers[7];
            const emotionalScore = answers[8] + answers[9];
            const socialScore = answers[10] + answers[11];

            // 最大スコアを持つカテゴリーを特定
            let fatigueType = 'body';
            let maxFatigueScore = bodyScore;
            if (brainScore > maxFatigueScore) {
                fatigueType = 'brain';
                maxFatigueScore = brainScore;
            }
            if (organScore > maxFatigueScore) {
                fatigueType = 'organ';
                maxFatigueScore = organScore;
            }

            let jobType = 'functional';
            let maxJobScore = functionalScore;
            if (emotionalScore > maxJobScore) {
                jobType = 'emotional';
                maxJobScore = emotionalScore;
            }
            if (socialScore > maxJobScore) {
                jobType = 'social';
                maxJobScore = socialScore;
            }

            return {
                fatigueType,
                jobType,
                scores: {
                    body: bodyScore,
                    brain: brainScore,
                    organ: organScore,
                    functional: functionalScore,
                    emotional: emotionalScore,
                    social: socialScore
                }
            };
        }

        function showResult() {
            document.getElementById('questionScreen').classList.remove('active');
            document.getElementById('resultScreen').classList.add('active');
            window.scrollTo(0, 0);

            const result = calculateResult();
            const resultKey = `${result.fatigueType}-${result.jobType}`;
            const data = resultData[resultKey];

            // 結果タイプを表示（順序を変更：subtitle → name → description）
            document.getElementById('resultType').innerHTML = `
                <div class="subtitle">${data.subtitle}</div>
                <h3>${data.name}</h3>
                <div class="description">${data.description}</div>
            `;

            // スコアバーを表示（疲労の原因）
            const fatigueNames = {
                body: '体の疲れ',
                brain: '脳の疲れ',
                organ: '内臓の疲れ'
            };

            let fatigueHTML = '';
            for (const [key, name] of Object.entries(fatigueNames)) {
                const score = result.scores[key];
                const percentage = (score / 10) * 100;
                fatigueHTML += `
                    <div class="score-bar-item">
                        <div class="score-bar-label">
                            <span class="name">${name}</span>
                            <span class="value">${score} / 10</span>
                        </div>
                        <div class="score-bar-bg">
                            <div class="score-bar-fill" style="width: ${percentage}%">${score}</div>
                        </div>
                    </div>
                `;
            }
            document.getElementById('fatigueScores').innerHTML = fatigueHTML;

            // スコアバーを表示（理想の状態）
            const jobNames = {
                functional: '成果、効率、パフォーマンスを発揮したい',
                emotional: '心理的な安定、安心感、心の平穏の追求',
                social: '外見、役割、社会的評価を得たい'
            };

            let jobHTML = '';
            for (const [key, name] of Object.entries(jobNames)) {
                const score = result.scores[key];
                const percentage = (score / 10) * 100;
                jobHTML += `
                    <div class="score-bar-item">
                        <div class="score-bar-label">
                            <span class="name">${name}</span>
                            <span class="value">${score} / 10</span>
                        </div>
                        <div class="score-bar-bg">
                            <div class="score-bar-fill" style="width: ${percentage}%">${score}</div>
                        </div>
                    </div>
                `;
            }
            document.getElementById('jobScores').innerHTML = jobHTML;

            // 各セクションを表示
            document.getElementById('fatigueReason').textContent = data.fatigueReason;
            document.getElementById('idealStatus').textContent = data.idealStatus;
            document.getElementById('recommendedApproach').textContent = data.approach;
            document.getElementById('recoveryMethod').textContent = data.recovery;
        }

        function copyResult() {
            const result = calculateResult();
            const resultKey = `${result.fatigueType}-${result.jobType}`;
            const data = resultData[resultKey];

            const text = `
🌟 疲労診断ツール - 診断結果 🌟

【あなたのタイプ】
${data.subtitle}
『${data.name}』

${data.description}

📊 スコア
【あなたの疲労度レベル】
体の疲れ: ${result.scores.body}/10
脳の疲れ: ${result.scores.brain}/10
内臓の疲れ: ${result.scores.organ}/10

【あなたの理想のコンディション】
成果、効率、パフォーマンスを発揮したい: ${result.scores.functional}/10
心理的な安定、安心感、心の平穏の追求: ${result.scores.emotional}/10
外見、役割、社会的評価を得たい: ${result.scores.social}/10

😓 あなたの疲労の原因
${data.fatigueReason}

✨ あなたの理想の状態
${data.idealStatus}

🔬 おすすめのアプローチ
${data.approach}

💡 具体的なリカバリー方法
${data.recovery}

---
疲労診断ツールで、あなたも疲労の原因を特定しましょう！
            `.trim();

            // クリップボードにコピー
            navigator.clipboard.writeText(text).then(() => {
                const toast = document.getElementById('toast');
                toast.classList.add('show');
                setTimeout(() => {
                    toast.classList.remove('show');
                }, 3000);
            }).catch(err => {
                alert('コピーに失敗しました。手動でコピーしてください。');
            });
        }

        function restartDiagnosis() {
            document.getElementById('resultScreen').classList.remove('active');
            document.getElementById('startScreen').classList.add('active');
            window.scrollTo(0, 0);
            answers = [3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3, 3];
            
            // スライダーをリセット
            const sliders = document.querySelectorAll('.slider');
            sliders.forEach((slider, index) => {
                slider.value = 3;
                document.getElementById(`value-${index}`).textContent = '3';
            });
        }
    </script>
</body>
</html>
