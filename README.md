<!DOCTYPE html>
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
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px 20px;
            text-align: center;
        }

        .header h1 {
            font-size: 24px;
            margin-bottom: 10px;
        }

        .header p {
            font-size: 14px;
            opacity: 0.9;
        }

        .progress-container {
            background: #f0f0f0;
            height: 8px;
            position: relative;
        }

        .progress-bar {
            background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
            height: 100%;
            width: 0%;
            transition: width 0.3s ease;
        }

        .content {
            padding: 30px 20px;
        }

        .start-screen, .question-screen, .result-screen {
            display: none;
        }

        .start-screen.active, .question-screen.active, .result-screen.active {
            display: block;
        }

        .start-screen {
            text-align: center;
        }

        .start-screen h2 {
            color: #667eea;
            margin-bottom: 20px;
            font-size: 22px;
        }

        .feature-list {
            text-align: left;
            margin: 30px 0;
            padding: 0 20px;
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
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
        }

        .btn:active {
            transform: translateY(0);
        }

        .question-number {
            color: #667eea;
            font-size: 14px;
            font-weight: bold;
            margin-bottom: 10px;
        }

        .phase-indicator {
            display: inline-block;
            background: #f0f0f0;
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 12px;
            margin-bottom: 15px;
            color: #666;
        }

        .question-text {
            font-size: 18px;
            line-height: 1.6;
            margin-bottom: 30px;
            color: #333;
            font-weight: 500;
        }

        .slider-container {
            margin: 40px 0;
        }

        .slider-labels {
            display: flex;
            justify-content: space-between;
            margin-bottom: 15px;
            font-size: 12px;
            color: #666;
        }

        .slider-labels span {
            flex: 1;
            text-align: center;
            padding: 0 5px;
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
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            cursor: pointer;
            box-shadow: 0 2px 10px rgba(102, 126, 234, 0.5);
        }

        input[type="range"]::-moz-range-thumb {
            width: 25px;
            height: 25px;
            border-radius: 50%;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            cursor: pointer;
            border: none;
            box-shadow: 0 2px 10px rgba(102, 126, 234, 0.5);
        }

        .slider-value {
            text-align: center;
            margin-top: 10px;
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
        }

        .navigation-buttons {
            display: flex;
            gap: 10px;
            margin-top: 30px;
        }

        .btn-back {
            background: #e0e0e0;
            color: #666;
            flex: 1;
        }

        .btn-next {
            flex: 2;
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
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            margin-bottom: 30px;
        }

        .result-type h3 {
            font-size: 22px;
            margin-bottom: 10px;
        }

        .result-type p {
            font-size: 14px;
            opacity: 0.9;
        }

        .result-section {
            margin-bottom: 30px;
            padding: 20px;
            background: #f8f9fa;
            border-radius: 15px;
            border-left: 4px solid #667eea;
        }

        .result-section h4 {
            color: #667eea;
            font-size: 18px;
            margin-bottom: 15px;
        }

        .result-section p, .result-section ul {
            line-height: 1.8;
            color: #555;
        }

        .result-section ul {
            margin-top: 10px;
            padding-left: 20px;
        }

        .result-section li {
            margin: 8px 0;
        }

        .action-buttons {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin-top: 30px;
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

        .score-display {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
            padding: 20px;
            background: white;
            border-radius: 10px;
        }

        .score-item {
            text-align: center;
        }

        .score-item .label {
            font-size: 12px;
            color: #666;
            margin-bottom: 5px;
        }

        .score-item .value {
            font-size: 24px;
            font-weight: bold;
            color: #667eea;
        }

        @media (max-width: 480px) {
            .header h1 {
                font-size: 20px;
            }

            .question-text {
                font-size: 16px;
            }

            .slider-labels span {
                font-size: 10px;
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

        <div class="progress-container">
            <div class="progress-bar" id="progressBar"></div>
        </div>

        <div class="content">
            <!-- スタート画面 -->
            <div class="start-screen active" id="startScreen">
                <h2>疲労の原因を特定し、<br>理想の体の状態を手に入れる</h2>
                <ul class="feature-list">
                    <li>12の質問で疲労の原因を科学的に分析</li>
                    <li>あなたの理想の体の状態を明確化</li>
                    <li>9つのパターンから最適な改善プランを提案</li>
                    <li>所要時間: 約3分</li>
                </ul>
                <button class="btn" onclick="startDiagnosis()">診断を始める</button>
            </div>

            <!-- 質問画面 -->
            <div class="question-screen" id="questionScreen">
                <div class="phase-indicator" id="phaseIndicator"></div>
                <div class="question-number" id="questionNumber"></div>
                <div class="question-text" id="questionText"></div>
                
                <div class="slider-container">
                    <div class="slider-labels">
                        <span>全く<br>当てはまらない</span>
                        <span>当てはまらない</span>
                        <span>わからない</span>
                        <span>当てはまる</span>
                        <span>非常に<br>当てはまる</span>
                    </div>
                    <input type="range" min="1" max="5" value="3" id="slider" oninput="updateSliderValue()">
                    <div class="slider-value" id="sliderValue">3</div>
                </div>

                <div class="navigation-buttons">
                    <button class="btn btn-back" onclick="previousQuestion()" id="backBtn">戻る</button>
                    <button class="btn btn-next" onclick="nextQuestion()" id="nextBtn">次へ</button>
                </div>
            </div>

            <!-- 結果画面 -->
            <div class="result-screen" id="resultScreen">
                <h2>🎯 診断結果</h2>
                
                <div class="result-type" id="resultType"></div>

                <div class="score-display" id="scoreDisplay"></div>

                <div class="result-section">
                    <h4>📊 あなたの現在の状態</h4>
                    <p id="currentStatus"></p>
                </div>

                <div class="result-section">
                    <h4>🎯 あなたが求める理想の状態</h4>
                    <p id="idealStatus"></p>
                </div>

                <div class="result-section">
                    <h4>💡 具体的な改善アクションプラン</h4>
                    <ul id="actionPlan"></ul>
                </div>

                <div class="result-section">
                    <h4>🔬 おすすめのアプローチ</h4>
                    <p id="recommendedApproach"></p>
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
        // 質問データ
        const questions = [
            // フェーズ1: 疲労の原因（Q1-Q6）
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "朝、目覚めても疲れが残っており、日中も体が重い、だるいと感じる。",
                category: "body"
            },
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "運動や活動を増やしていないのに、慢性的な肩こりや腰の痛みに悩んでいる。",
                category: "body"
            },
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "仕事中や会話中、集中力が急に途切れたり、物忘れや簡単なミスが増えた。",
                category: "brain"
            },
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "夜中に目が覚める、寝つきが悪いなど、睡眠の質が悪く、常に頭が休まらない。",
                category: "brain"
            },
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "便秘や下痢などのお通じの不調、または風邪や口内炎が治りにくい。",
                category: "organ"
            },
            {
                phase: "フェーズ1: 疲労の原因を特定",
                text: "以前に比べ、何をしても疲れが抜けにくく、肌荒れや体重増加が起こりやすい。",
                category: "organ"
            },
            // フェーズ2: 顧客のジョブ（Q7-Q12）
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "疲れの原因を明確に特定し、迷わず回復できる科学的な方法を知りたい。",
                category: "functional"
            },
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "仕事や趣味で最高の集中力とパフォーマンスを長時間維持したい。",
                category: "functional"
            },
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "ストレスやイライラから解放され、心に余裕をもってリラックスしたい。",
                category: "emotional"
            },
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "自分の健康状態をコントロールできているという安心感と自信を得たい。",
                category: "emotional"
            },
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "見た目が若々しく、エネルギッシュで、周りから「疲れていないね」と言われたい。",
                category: "social"
            },
            {
                phase: "フェーズ2: 理想の状態を明確化",
                text: "仕事や社会的な役割で、常に最高の状態でいる理想の自分を実現したい。",
                category: "social"
            }
        ];

        // 診断結果データ
        const resultData = {
            "body-functional": {
                title: "体の疲れ × 機能的ジョブ",
                subtitle: "回復力を高め、パフォーマンスを最大化したいあなた",
                currentStatus: "あなたの体は慢性的な疲労状態にあり、朝起きても疲れが残り、体が重くだるい状態が続いています。肩こりや腰痛などの身体的な不調も現れており、回復力が低下しています。この状態では、仕事や日常生活でのパフォーマンスが十分に発揮できていない可能性があります。",
                idealStatus: "あなたは疲労の根本原因を解決し、最高のパフォーマンスを長時間維持できる体を求めています。科学的なアプローチで効率的に回復し、仕事や趣味で最大限の能力を発揮できる状態を目指しています。",
                actions: [
                    "質の高い睡眠環境を整える（寝室の温度管理、遮光カーテン、適切な寝具）",
                    "週3回、30分の軽い有酸素運動（ウォーキング、ストレッチ）で血流改善",
                    "タンパク質とビタミンB群を意識した食事で筋肉疲労を回復",
                    "15分の昼寝（パワーナップ）で午後のパフォーマンス向上",
                    "マグネシウム・カルシウムのサプリメントで筋肉の緊張緩和"
                ],
                approach: "科学的な回復メソッドと栄養管理を組み合わせたアプローチが最適です。睡眠の質を測定できるデバイスを活用し、データに基づいて改善を進めることで、効率的に体の疲労を解消し、パフォーマンスの向上を実現できます。"
            },
            "body-emotional": {
                title: "体の疲れ × 感情的ジョブ",
                subtitle: "心身の安心感とリラックスを取り戻したいあなた",
                currentStatus: "体の疲労が蓄積し、朝起きるのがつらく、一日中体が重い状態が続いています。この身体的な不調が心理的なストレスにもつながり、常に疲れている感覚があなたの心の余裕を奪っています。",
                idealStatus: "あなたは体の疲れから解放され、心に余裕を持ってリラックスできる状態を求めています。自分の体をコントロールできているという安心感と、ストレスから解放された穏やかな日々を望んでいます。",
                actions: [
                    "毎日10分の瞑想・マインドフルネスで心身のリラックス",
                    "週2回の温浴療法（入浴剤を使った38-40℃の半身浴20分）",
                    "アロマセラピー（ラベンダー、カモミール）で副交感神経を活性化",
                    "ヨガやストレッチで体の緊張をほぐし、心身の調和を図る",
                    "就寝1時間前のデジタルデトックスで睡眠の質向上"
                ],
                approach: "心身のリラクゼーションを重視したホリスティックなアプローチが効果的です。体の疲れを癒すことで心の安定も得られるため、リラックスできる環境作りと、自律神経を整えるケアを優先しましょう。"
            },
            "body-social": {
                title: "体の疲れ × 社会的ジョブ",
                subtitle: "若々しく活力ある外見を取り戻したいあなた",
                currentStatus: "体の疲労が顔や姿勢に現れ、実年齢よりも疲れて見られることが増えています。肩こりや姿勢の悪化により、外見的な印象も低下している可能性があります。周囲から「疲れているね」と言われることが増えていませんか？",
                idealStatus: "あなたは若々しく、エネルギッシュな外見を取り戻し、周囲から「元気だね」「若々しいね」と言われる状態を望んでいます。社会的な場面で常に最高の自分でいられることを目指しています。",
                actions: [
                    "姿勢改善エクササイズ（背筋・体幹トレーニング）で若々しい立ち姿に",
                    "コラーゲン・ビタミンCの摂取で肌のハリと弾力を改善",
                    "表情筋トレーニングで明るく若々しい表情を維持",
                    "週1回のリンパマッサージで顔のむくみ・くすみを解消",
                    "十分な水分摂取（1日2L）で肌の潤いと代謝を促進"
                ],
                approach: "外見改善と体力向上を組み合わせたアプローチが最適です。姿勢改善と美容ケアを並行して行うことで、見た目の若々しさとエネルギッシュな印象を手に入れることができます。"
            },
            "brain-functional": {
                title: "脳の疲れ × 機能的ジョブ",
                subtitle: "集中力とパフォーマンスを最大化したいあなた",
                currentStatus: "脳の疲労により、集中力や思考力が続かず、仕事や会話中にミスが増えています。睡眠の質も悪く、脳が十分に休息できていません。この状態では、本来の能力を発揮することが難しくなっています。",
                idealStatus: "あなたは脳の疲労を解消し、長時間にわたって高い集中力とパフォーマンスを維持できる状態を求めています。科学的な方法で脳機能を最適化し、仕事や学習で最大限の成果を出したいと考えています。",
                actions: [
                    "ポモドーロ・テクニック（25分集中+5分休憩）で脳の疲労を防ぐ",
                    "オメガ3脂肪酸（魚油、ナッツ）で脳機能をサポート",
                    "カフェインとL-テアニンの組み合わせで集中力向上",
                    "睡眠前のブルーライトカット（2時間前から）で深い睡眠を確保",
                    "脳トレアプリや読書で認知機能を維持・向上"
                ],
                approach: "脳科学に基づいた認知パフォーマンス向上プログラムが最適です。睡眠の質を改善し、栄養面から脳をサポートすることで、持続可能な高パフォーマンス状態を実現できます。"
            },
            "brain-emotional": {
                title: "脳の疲れ × 感情的ジョブ",
                subtitle: "心の安定とリラックスを取り戻したいあなた",
                currentStatus: "脳の過労により、イライラや不安感が強くなり、感情のコントロールが難しくなっています。睡眠の質も悪く、精神的な余裕が失われています。この状態は心の健康にも影響を与えています。",
                idealStatus: "あなたは脳の疲労から解放され、心に余裕を持ち、穏やかでリラックスした毎日を送りたいと考えています。感情を安定させ、自分自身をコントロールできているという安心感を得たいと望んでいます。",
                actions: [
                    "毎日15分のマインドフルネス瞑想で脳のリセット",
                    "自然の中での散歩（グリーンエクササイズ）で精神的リフレッシュ",
                    "GABAやテアニンのサプリメントでリラックス効果を促進",
                    "ジャーナリング（感情の書き出し）で思考の整理",
                    "スマホ・PCの使用時間制限で脳の休息時間を確保"
                ],
                approach: "メンタルヘルスケアと脳の休息を重視したアプローチが効果的です。デジタルデトックスと自然療法を取り入れることで、脳をリセットし、心の安定を取り戻すことができます。"
            },
            "brain-social": {
                title: "脳の疲れ × 社会的ジョブ",
                subtitle: "社会的な場面で最高の自分でいたいあなた",
                currentStatus: "脳の疲労により、会話や仕事でミスが増え、社会的な場面でのパフォーマンスが低下しています。集中力の欠如や物忘れが、あなたの社会的な評価に影響を与えている可能性があります。",
                idealStatus: "あなたは社会的な場面で常に聡明で、機敏に対応できる自分でいたいと考えています。仕事やコミュニティで理想的な役割を果たし、周囲から信頼される存在でありたいと望んでいます。",
                actions: [
                    "朝のルーティン確立（瞑想+軽い運動）で一日のパフォーマンス向上",
                    "プレゼン前のパワーポーズで自信とパフォーマンスを強化",
                    "ビタミンB群サプリで脳のエネルギー代謝をサポート",
                    "重要な会議前の15分仮眠で集中力を最大化",
                    "週末の完全休養日を設けて脳の回復を促進"
                ],
                approach: "社会的パフォーマンスを最大化するための戦略的な脳のコンディショニングが必要です。重要な場面での集中力を高めるテクニックと、日常的な脳のケアを組み合わせることで、理想の社会的役割を果たせます。"
            },
            "organ-functional": {
                title: "内臓の疲れ × 機能的ジョブ",
                subtitle: "根本から体調を改善し、活力を最大化したいあなた",
                currentStatus: "内臓の疲労により、エネルギー代謝が低下し、何をしても疲れが抜けない状態が続いています。腸内環境の乱れや免疫力の低下により、風邪をひきやすく、全身の不調が現れています。",
                idealStatus: "あなたは内臓の健康を根本から改善し、高いエネルギーレベルを維持できる体を求めています。科学的なアプローチで体調不良を解消し、常に最高のコンディションでパフォーマンスを発揮したいと考えています。",
                actions: [
                    "発酵食品（ヨーグルト、納豆、キムチ）で腸内環境を改善",
                    "食物繊維（野菜、海藻、きのこ）で便通を整える",
                    "16時間断食（インターミッテントファスティング）で内臓を休ませる",
                    "プロバイオティクスサプリで腸内フローラをサポート",
                    "抗酸化物質（ビタミンC・E、ポリフェノール）で細胞の老化を防ぐ"
                ],
                approach: "腸内環境の改善と栄養療法を中心としたアプローチが最適です。内臓の機能を回復させることで、エネルギー代謝が向上し、疲れにくい体質へと変わっていきます。定期的な検査で進捗を確認しながら進めましょう。"
            },
            "organ-emotional": {
                title: "内臓の疲れ × 感情的ジョブ",
                subtitle: "体の内側から安心感と健康を取り戻したいあなた",
                currentStatus: "内臓の不調により、慢性的な疲労感と体調不良が続き、心理的な不安やストレスも感じています。お腹の調子が悪いと気分も沈みがちで、健康に対する不安が常に付きまとっています。",
                idealStatus: "あなたは内臓の健康を改善し、体の内側から安心感を得たいと考えています。体調をコントロールできているという自信と、心身ともにリラックスできる状態を望んでいます。",
                actions: [
                    "温かいスープや消化の良い食事で内臓を労わる",
                    "腹式呼吸や腸マッサージでリラックスと腸の蠕動運動を促進",
                    "ストレス軽減のためのヨガや瞑想",
                    "ハーブティー（カモミール、ペパーミント）で消化器系をサポート",
                    "規則正しい食事時間で体内リズムを整える"
                ],
                approach: "心と腸のつながり（腸脳相関）を意識したホリスティックケアが効果的です。腸内環境を整えることで、セロトニン（幸せホルモン）の生成が促進され、心の安定にもつながります。"
            },
            "organ-social": {
                title: "内臓の疲れ × 社会的ジョブ",
                subtitle: "内側から輝く若々しさを手に入れたいあなた",
                currentStatus: "内臓の疲労が肌荒れや体重増加として現れ、外見に影響を与えています。内側の不調が外見の老化として表れ、若々しさが失われつつあります。",
                idealStatus: "あなたは内臓の健康を改善し、内側から輝く若々しい外見を手に入れたいと考えています。周囲から「元気で若々しい」と評価され、社会的な場面で理想の自分を表現したいと望んでいます。",
                actions: [
                    "抗酸化作用の高い食品（ベリー類、緑茶、ダークチョコレート）で美肌効果",
                    "コラーゲンペプチドとビタミンCで肌のハリを改善",
                    "適切な水分摂取とデトックス効果のある食材で老廃物を排出",
                    "グルテンフリー・低糖質食で腸内環境を整え、肌荒れを改善",
                    "定期的な運動で代謝を上げ、体重をコントロール"
                ],
                approach: "インナービューティーと腸活を組み合わせたアプローチが最適です。内臓の健康が外見の若々しさに直結するため、食事と生活習慣の改善で内側から輝く美しさを実現できます。"
            }
        };

        let currentQuestion = 0;
        let answers = [];

        function startDiagnosis() {
            document.getElementById('startScreen').classList.remove('active');
            document.getElementById('questionScreen').classList.add('active');
            currentQuestion = 0;
            answers = [];
            showQuestion();
        }

        function showQuestion() {
            const question = questions[currentQuestion];
            document.getElementById('phaseIndicator').textContent = question.phase;
            document.getElementById('questionNumber').textContent = `質問 ${currentQuestion + 1} / 12`;
            document.getElementById('questionText').textContent = question.text;
            
            // スライダーをリセット
            const slider = document.getElementById('slider');
            slider.value = answers[currentQuestion] || 3;
            updateSliderValue();

            // 戻るボタンの表示制御
            document.getElementById('backBtn').style.display = currentQuestion === 0 ? 'none' : 'block';
            
            // 次へボタンのテキスト変更
            document.getElementById('nextBtn').textContent = currentQuestion === 11 ? '結果を見る' : '次へ';

            // プログレスバー更新
            const progress = ((currentQuestion + 1) / 12) * 100;
            document.getElementById('progressBar').style.width = progress + '%';
        }

        function updateSliderValue() {
            const value = document.getElementById('slider').value;
            document.getElementById('sliderValue').textContent = value;
        }

        function previousQuestion() {
            if (currentQuestion > 0) {
                currentQuestion--;
                showQuestion();
            }
        }

        function nextQuestion() {
            // 現在の回答を保存
            const value = parseInt(document.getElementById('slider').value);
            answers[currentQuestion] = value;

            if (currentQuestion < 11) {
                currentQuestion++;
                showQuestion();
            } else {
                // 診断結果を表示
                showResult();
            }
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

            const result = calculateResult();
            const resultKey = `${result.fatigueType}-${result.jobType}`;
            const data = resultData[resultKey];

            // 結果タイプを表示
            document.getElementById('resultType').innerHTML = `
                <h3>${data.title}</h3>
                <p>${data.subtitle}</p>
            `;

            // スコア表示
            const fatigueNames = {
                body: '体の疲れ',
                brain: '脳の疲れ',
                organ: '内臓の疲れ'
            };
            const jobNames = {
                functional: '機能的ジョブ',
                emotional: '感情的ジョブ',
                social: '社会的ジョブ'
            };

            document.getElementById('scoreDisplay').innerHTML = `
                <div class="score-item">
                    <div class="label">疲労の原因</div>
                    <div class="value">${fatigueNames[result.fatigueType]}</div>
                </div>
                <div class="score-item">
                    <div class="label">理想の状態</div>
                    <div class="value">${jobNames[result.jobType]}</div>
                </div>
            `;

            // 各セクションを表示
            document.getElementById('currentStatus').textContent = data.currentStatus;
            document.getElementById('idealStatus').textContent = data.idealStatus;
            
            const actionPlanHTML = data.actions.map(action => `<li>${action}</li>`).join('');
            document.getElementById('actionPlan').innerHTML = actionPlanHTML;
            
            document.getElementById('recommendedApproach').textContent = data.approach;

            // プログレスバーを100%に
            document.getElementById('progressBar').style.width = '100%';
        }

        function copyResult() {
            const result = calculateResult();
            const resultKey = `${result.fatigueType}-${result.jobType}`;
            const data = resultData[resultKey];

            const fatigueNames = {
                body: '体の疲れ',
                brain: '脳の疲れ',
                organ: '内臓の疲れ'
            };
            const jobNames = {
                functional: '機能的ジョブ',
                emotional: '感情的ジョブ',
                social: '社会的ジョブ'
            };

            const text = `
🌟 疲労診断ツール - 診断結果 🌟

【あなたのタイプ】
${data.title}
${data.subtitle}

【疲労の原因】${fatigueNames[result.fatigueType]}
【理想の状態】${jobNames[result.jobType]}

📊 現在の状態
${data.currentStatus}

🎯 あなたが求める理想の状態
${data.idealStatus}

💡 具体的な改善アクションプラン
${data.actions.map((action, i) => `${i + 1}. ${action}`).join('\n')}

🔬 おすすめのアプローチ
${data.approach}

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
            document.getElementById('progressBar').style.width = '0%';
            currentQuestion = 0;
            answers = [];
        }
    </script>
</body>
</html>
