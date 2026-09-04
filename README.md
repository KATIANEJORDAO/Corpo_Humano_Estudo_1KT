# quiz_corpo_humano_1K
#Quiz do corpo humano
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QuizBoard - Corpo Humano</title>
    <style>
        :root {
            --bg: #0f172a;
            --card: #1e293b;
            --card-hover: #334155;
            --accent: #38bdf8;
            --accent2: #a78bfa;
            --correct: #4ade80;
            --wrong: #f87171;
            --text: #e2e8f0;
            --text-muted: #94a3b8;
            --gold: #fbbf24;
            --silver: #cbd5e1;
            --bronze: #d97706;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            background: var(--bg);
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 1rem;
            background-image:
                radial-gradient(ellipse at 20% 50%, rgba(56, 189, 248, 0.08) 0%, transparent 60%),
                radial-gradient(ellipse at 80% 50%, rgba(167, 139, 250, 0.08) 0%, transparent 60%);
        }

        .quiz-container {
            width: 100%;
            max-width: 900px;
            background: var(--card);
            border-radius: 2rem;
            padding: 2rem 2rem 2.5rem;
            box-shadow: 0 25px 50px -12px rgba(0, 0, 0, 0.6),
                0 0 0 1px rgba(255, 255, 255, 0.05);
            border: 1px solid rgba(255, 255, 255, 0.08);
            position: relative;
            overflow: hidden;
        }

        .quiz-container::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(90deg, var(--accent), var(--accent2), var(--accent));
            background-size: 200% 100%;
            animation: shimmer 3s linear infinite;
        }

        @keyframes shimmer {
            0% {
                background-position: -200% 0;
            }
            100% {
                background-position: 200% 0;
            }
        }

        .header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 1.5rem;
            flex-wrap: wrap;
            gap: 0.75rem;
        }

        .title {
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        .title-icon {
            font-size: 2.2rem;
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%,
            100% {
                transform: scale(1);
            }
            50% {
                transform: scale(1.1);
            }
        }

        .title h1 {
            color: var(--text);
            font-size: 1.8rem;
            font-weight: 700;
            letter-spacing: -0.02em;
            background: linear-gradient(135deg, var(--accent), var(--accent2));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }

        .score-display {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            background: rgba(255, 255, 255, 0.05);
            padding: 0.5rem 1rem;
            border-radius: 2rem;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .score-display .medal {
            font-size: 1.5rem;
        }

        .score-display span {
            color: var(--text);
            font-weight: 600;
            font-size: 1.1rem;
        }

        .score-value {
            color: var(--gold) !important;
            font-size: 1.3rem !important;
        }

        .progress-bar-container {
            width: 100%;
            height: 8px;
            background: rgba(255, 255, 255, 0.08);
            border-radius: 1rem;
            margin-bottom: 1.5rem;
            overflow: hidden;
        }

        .progress-bar {
            height: 100%;
            background: linear-gradient(90deg, var(--accent), var(--accent2));
            border-radius: 1rem;
            transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .question-counter {
            color: var(--text-muted);
            font-size: 0.9rem;
            margin-bottom: 1rem;
            text-align: center;
            letter-spacing: 0.05em;
            text-transform: uppercase;
            font-weight: 500;
        }

        .question-card {
            background: rgba(255, 255, 255, 0.03);
            border-radius: 1.5rem;
            padding: 1.75rem 1.5rem;
            margin-bottom: 1.5rem;
            border: 1px solid rgba(255, 255, 255, 0.06);
        }

        .question-text {
            color: var(--text);
            font-size: 1.25rem;
            font-weight: 600;
            line-height: 1.5;
            margin-bottom: 0.5rem;
        }

        .question-category {
            display: inline-block;
            background: rgba(56, 189, 248, 0.12);
            color: var(--accent);
            font-size: 0.75rem;
            font-weight: 600;
            padding: 0.3rem 0.75rem;
            border-radius: 1rem;
            text-transform: uppercase;
            letter-spacing: 0.08em;
            margin-bottom: 0.75rem;
        }

        .options-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 0.75rem;
        }

        .option-btn {
            background: rgba(255, 255, 255, 0.04);
            border: 2px solid rgba(255, 255, 255, 0.1);
            border-radius: 1rem;
            padding: 1rem 1.25rem;
            color: var(--text);
            font-size: 1rem;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.25s ease;
            position: relative;
            text-align: left;
            display: flex;
            align-items: center;
            gap: 0.75rem;
            letter-spacing: 0.01em;
            min-height: 60px;
        }

        .option-btn:hover:not(.disabled) {
            background: rgba(255, 255, 255, 0.08);
            border-color: rgba(255, 255, 255, 0.25);
            transform: translateY(-2px);
            box-shadow: 0 8px 20px -8px rgba(0, 0, 0, 0.4);
        }

        .option-btn:active:not(.disabled) {
            transform: translateY(0);
            box-shadow: none;
        }

        .option-letter {
            display: flex;
            align-items: center;
            justify-content: center;
            width: 32px;
            height: 32px;
            background: rgba(255, 255, 255, 0.08);
            border-radius: 50%;
            font-weight: 700;
            font-size: 0.85rem;
            color: var(--text-muted);
            flex-shrink: 0;
            transition: all 0.25s ease;
        }

        .option-btn:hover:not(.disabled) .option-letter {
            background: rgba(56, 189, 248, 0.2);
            color: var(--accent);
        }

        .option-btn.correct {
            background: rgba(74, 222, 128, 0.12);
            border-color: var(--correct);
            animation: correctFlash 0.6s ease;
        }

        .option-btn.correct .option-letter {
            background: var(--correct);
            color: #000;
        }

        .option-btn.wrong {
            background: rgba(248, 113, 113, 0.12);
            border-color: var(--wrong);
            animation: wrongShake 0.5s ease;
        }

        .option-btn.wrong .option-letter {
            background: var(--wrong);
            color: #000;
        }

        .option-btn.disabled {
            cursor: default;
            opacity: 0.7;
        }

        .option-btn.show-correct {
            border-color: var(--correct);
            background: rgba(74, 222, 128, 0.08);
        }

        @keyframes correctFlash {
            0% {
                box-shadow: 0 0 0 0 rgba(74, 222, 128, 0.5);
            }
            100% {
                box-shadow: 0 0 0 20px rgba(74, 222, 128, 0);
            }
        }

        @keyframes wrongShake {
            0%,
            100% {
                transform: translateX(0);
            }
            20% {
                transform: translateX(-8px);
            }
            40% {
                transform: translateX(8px);
            }
            60% {
                transform: translateX(-5px);
            }
            80% {
                transform: translateX(5px);
            }
        }

        .feedback-message {
            text-align: center;
            font-size: 1rem;
            font-weight: 600;
            margin: 0.75rem 0 0;
            min-height: 1.5rem;
            transition: all 0.3s ease;
        }

        .feedback-message.success {
            color: var(--correct);
        }
        .feedback-message.error {
            color: var(--wrong);
        }
        .feedback-message.info {
            color: var(--accent);
        }

        .nav-buttons {
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 1rem;
            margin-top: 1.25rem;
            flex-wrap: wrap;
        }

        .btn {
            padding: 0.8rem 1.5rem;
            border-radius: 1rem;
            font-size: 0.95rem;
            font-weight: 600;
            border: none;
            cursor: pointer;
            transition: all 0.25s ease;
            letter-spacing: 0.03em;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .btn-primary {
            background: linear-gradient(135deg, var(--accent), var(--accent2));
            color: #0f172a;
            box-shadow: 0 4px 15px -3px rgba(56, 189, 248, 0.4);
        }

        .btn-primary:hover:not(:disabled) {
            transform: translateY(-2px);
            box-shadow: 0 8px 25px -5px rgba(56, 189, 248, 0.5);
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.06);
            color: var(--text);
            border: 1px solid rgba(255, 255, 255, 0.12);
        }

        .btn-secondary:hover:not(:disabled) {
            background: rgba(255, 255, 255, 0.1);
            transform: translateY(-2px);
        }

        .btn:disabled {
            opacity: 0.4;
            cursor: not-allowed;
            transform: none !important;
            box-shadow: none !important;
        }

        .btn-restart {
            background: rgba(251, 191, 36, 0.12);
            color: var(--gold);
            border: 1px solid rgba(251, 191, 36, 0.3);
        }

        .btn-restart:hover {
            background: rgba(251, 191, 36, 0.2);
            transform: translateY(-2px);
        }

        .results-screen {
            text-align: center;
            padding: 2rem 1rem;
        }

        .results-screen .big-icon {
            font-size: 5rem;
            margin-bottom: 1rem;
            animation: bounce 1.5s ease infinite;
        }

        @keyframes bounce {
            0%,
            100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-15px);
            }
        }

        .results-screen h2 {
            color: var(--text);
            font-size: 2rem;
            margin-bottom: 0.5rem;
        }

        .results-screen .final-score {
            color: var(--gold);
            font-size: 3.5rem;
            font-weight: 800;
            margin: 1rem 0;
        }

        .results-screen .message {
            color: var(--text-muted);
            font-size: 1.1rem;
            margin-bottom: 1.5rem;
            line-height: 1.6;
        }

        .results-stats {
            display: flex;
            justify-content: center;
            gap: 2rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
        }

        .stat-item {
            background: rgba(255, 255, 255, 0.04);
            padding: 1rem 1.5rem;
            border-radius: 1rem;
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .stat-item .stat-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: var(--text);
        }
        .stat-item .stat-label {
            font-size: 0.8rem;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 0.08em;
        }

        .stat-item.correct-stat .stat-value {
            color: var(--correct);
        }
        .stat-item.wrong-stat .stat-value {
            color: var(--wrong);
        }

        @media (max-width: 640px) {
            .quiz-container {
                padding: 1.25rem 1rem 1.75rem;
                border-radius: 1.5rem;
            }

            .options-grid {
                grid-template-columns: 1fr;
            }

            .title h1 {
                font-size: 1.4rem;
            }

            .question-text {
                font-size: 1.05rem;
            }

            .option-btn {
                padding: 0.8rem 1rem;
                font-size: 0.9rem;
                min-height: 50px;
            }

            .nav-buttons {
                flex-direction: column;
                gap: 0.5rem;
            }

            .btn {
                width: 100%;
                justify-content: center;
            }

            .results-screen .final-score {
                font-size: 2.5rem;
            }

            .results-stats {
                gap: 0.75rem;
            }
        }

        @media (max-width: 380px) {
            .header {
                flex-direction: column;
                align-items: stretch;
                text-align: center;
            }
            .title {
                justify-content: center;
            }
            .score-display {
                justify-content: center;
            }
        }
    </style>
</head>
<body>

    <div class="quiz-container" id="quizApp">
        <!-- Conteúdo renderizado via JavaScript -->
    </div>

    <script>
        (function() {
            // ============ DADOS DO QUIZ ============
            const questions = [{
                category: "Anatomia",
                question: "Qual é o maior órgão do corpo humano?",
                options: ["Fígado", "Pele", "Cérebro", "Coração"],
                correctIndex: 1,
                explanation: "A pele é o maior órgão, cobrindo cerca de 2m² em um adulto!"
            }, {
                category: "Sistema Esquelético",
                question: "Quantos ossos tem o corpo humano adulto?",
                options: ["206", "201", "210", "196"],
                correctIndex: 0,
                explanation: "Um adulto possui 206 ossos. Bebês nascem com ~270 que se fundem."
            }, {
                category: "Sistema Circulatório",
                question: "Qual é a função principal dos glóbulos vermelhos?",
                options: ["Combater infecções", "Coagular o sangue", "Transportar oxigênio", "Produzir hormônios"],
                correctIndex: 2,
                explanation: "Os glóbulos vermelhos (hemácias) transportam O₂ dos pulmões aos tecidos."
            }, {
                category: "Neurociência",
                question: "Quantos neurônios aproximadamente tem o cérebro humano?",
                options: ["1 milhão", "100 milhões", "10 bilhões", "86 bilhões"],
                correctIndex: 3,
                explanation: "O cérebro tem cerca de 86 bilhões de neurônios interconectados!"
            }, {
                category: "Sistema Digestório",
                question: "Onde ocorre a maior parte da absorção de nutrientes?",
                options: ["Estômago", "Intestino delgado", "Intestino grosso", "Esôfago"],
                correctIndex: 1,
                explanation: "O intestino delgado é o principal local de absorção de nutrientes."
            }, {
                category: "Sistema Muscular",
                question: "Qual é o músculo mais forte do corpo humano?",
                options: ["Bíceps", "Coração", "Masseter (mandíbula)", "Quadríceps"],
                correctIndex: 2,
                explanation: "O masseter, músculo da mandíbula, exerce a maior força proporcional."
            }, {
                category: "Órgãos",
                question: "Qual órgão é responsável por filtrar o sangue?",
                options: ["Pulmões", "Rins", "Fígado", "Baço"],
                correctIndex: 1,
                explanation: "Os rins filtram ~180 litros de sangue por dia, produzindo 1-2L de urina."
            }, {
                category: "Sistema Respiratório",
                question: "Qual a capacidade média de ar dos pulmões de um adulto?",
                options: ["2 litros", "4 litros", "6 litros", "8 litros"],
                correctIndex: 2,
                explanation: "A capacidade pulmonar total média é de cerca de 6 litros."
            }, {
                category: "Curiosidades",
                question: "Qual é a célula mais longa do corpo humano?",
                options: ["Neurônio motor", "Célula da pele", "Célula óssea", "Célula muscular"],
                correctIndex: 0,
                explanation: "Neurônios motores podem ter mais de 1 metro de comprimento!"
            }, {
                category: "Sistema Endócrino",
                question: "Qual glândula é chamada de 'glândula mestra'?",
                options: ["Tireoide", "Pâncreas", "Hipófise (pituitária)", "Suprarrenal"],
                correctIndex: 2,
                explanation: "A hipófise controla várias outras glândulas endócrinas do corpo."
            }, {
                category: "Sentidos",
                question: "Qual é o menor osso do corpo humano?",
                options: ["Martelo (ouvido)", "Estribo (ouvido)", "Bigorna (ouvido)", "Falange do dedinho"],
                correctIndex: 1,
                explanation: "O estribo, no ouvido médio, tem apenas ~3mm de comprimento."
            }, {
                category: "Sistema Imunológico",
                question: "Qual célula é responsável por 'memorizar' infecções?",
                options: ["Neutrófilo", "Linfócito T de memória", "Plaqueta", "Eosinófilo"],
                correctIndex: 1,
                explanation: "Linfócitos T de memória 'lembram' patógenos para resposta mais rápida."
            }];

            // ============ ESTADO ============
            let currentQuestionIndex = 0;
            let score = 0;
            let answers = []; // armazena se acertou em cada pergunta
            let selectedOptionIndex = null;
            let isAnswered = false;
            let quizFinished = false;

            // ============ ELEMENTOS ============
            const container = document.getElementById('quizApp');

            // ============ FUNÇÕES DE RENDER ============
            function render() {
                if (quizFinished) {
                    renderResults();
                } else {
                    renderQuestion();
                }
            }

            function renderQuestion() {
                const q = questions[currentQuestionIndex];
                const total = questions.length;
                const progress = ((currentQuestionIndex) / total) * 100;

                let optionsHTML = '';
                const letters = ['A', 'B', 'C', 'D'];

                q.options.forEach((opt, idx) => {
                    let btnClass = 'option-btn';

                    if (isAnswered) {
                        btnClass += ' disabled';
                        if (idx === q.correctIndex) {
                            btnClass += ' correct show-correct';
                        } else if (idx === selectedOptionIndex && idx !== q.correctIndex) {
                            btnClass += ' wrong';
                        } else {
                            btnClass += ' disabled';
                        }
                    }

                    optionsHTML += `
                        <button class="${btnClass}" data-option-index="${idx}" ${isAnswered ? 'disabled' : ''}>
                            <span class="option-letter">${letters[idx]}</span>
                            <span>${opt}</span>
                        </button>
                    `;
                });

                let feedbackHTML = '';
                let feedbackClass = 'info';

                if (isAnswered) {
                    const isCorrect = selectedOptionIndex === q.correctIndex;
                    if (isCorrect) {
                        feedbackHTML = '✅ Resposta correta! ' + q.explanation;
                        feedbackClass = 'success';
                    } else {
                        feedbackHTML = '❌ Resposta errada. ' + q.explanation;
                        feedbackClass = 'error';
                    }
                } else {
                    feedbackHTML = 'Escolha uma opção para continuar.';
                    feedbackClass = 'info';
                }

                const isLast = currentQuestionIndex === total - 1;
                const nextBtnText = isLast ? 'Ver Resultados 🏆' : 'Próxima Questão →';
                const nextBtnDisabled = !isAnswered;

                container.innerHTML = `
                    <div class="header">
                        <div class="title">
                            <span class="title-icon">🧠</span>
                            <h1>QuizBoard · Corpo Humano</h1>
                        </div>
                        <div class="score-display">
                            <span class="medal">🏅</span>
                            <span>Pontuação:</span>
                            <span class="score-value">${score}</span>
                            <span style="color:var(--text-muted);font-weight:400;">/ ${total}</span>
                        </div>
                    </div>

                    <div class="progress-bar-container">
                        <div class="progress-bar" style="width: ${progress}%;"></div>
                    </div>

                    <div class="question-counter">
                        Questão ${currentQuestionIndex + 1} de ${total}
                    </div>

                    <div class="question-card">
                        <span class="question-category">${q.category}</span>
                        <p class="question-text">${q.question}</p>
                    </div>

                    <div class="options-grid">
                        ${optionsHTML}
                    </div>

                    <div class="feedback-message ${feedbackClass}">
                        ${feedbackHTML}
                    </div>

                    <div class="nav-buttons">
                        <button class="btn btn-secondary" id="btnPrev" ${currentQuestionIndex === 0 ? 'disabled' : ''}>
                            ← Anterior
                        </button>
                        <button class="btn btn-primary" id="btnNext" ${nextBtnDisabled ? 'disabled' : ''}>
                            ${nextBtnText}
                        </button>
                    </div>

                    <div style="text-align:center; margin-top:0.75rem;">
                        <button class="btn btn-restart" id="btnRestart" style="padding:0.5rem 1rem; font-size:0.8rem;">
                            🔄 Reiniciar Quiz
                        </button>
                    </div>
                `;

                // Attach event listeners
                if (!isAnswered) {
                    document.querySelectorAll('.option-btn').forEach(btn => {
                        btn.addEventListener('click', handleOptionClick);
                    });
                }

                const prevBtn = document.getElementById('btnPrev');
                if (prevBtn) prevBtn.addEventListener('click', goToPrevious);

                const nextBtn = document.getElementById('btnNext');
                if (nextBtn) nextBtn.addEventListener('click', goToNext);

                const restartBtn = document.getElementById('btnRestart');
                if (restartBtn) restartBtn.addEventListener('click', restartQuiz);
            }

            function renderResults() {
                const total = questions.length;
                const correctCount = answers.filter(a => a === true).length;
                const wrongCount = total - correctCount;
                const percentage = Math.round((correctCount / total) * 100);

                let icon, title, message;

                if (percentage === 100) {
                    icon = '🏆';
                    title = 'Perfeito! Você é um gênio!';
                    message = 'Você acertou TODAS as questões! Conhecimento excepcional do corpo humano!';
                } else if (percentage >= 80) {
                    icon = '🥇';
                    title = 'Excelente!';
                    message = 'Você tem um ótimo conhecimento do corpo humano. Muito bem!';
                } else if (percentage >= 60) {
                    icon = '🥈';
                    title = 'Muito bom!';
                    message = 'Você sabe bastante sobre o corpo humano. Continue estudando!';
                } else if (percentage >= 40) {
                    icon = '🥉';
                    title = 'Bom começo!';
                    message = 'Você tem conhecimentos básicos. Que tal revisar anatomia?';
                } else {
                    icon = '📚';
                    title = 'Hora de estudar!';
                    message = 'O corpo humano é fascinante! Revise o conteúdo e tente novamente.';
                }

                container.innerHTML = `
                    <div class="results-screen">
                        <div class="big-icon">${icon}</div>
                        <h2>${title}</h2>
                        <div class="final-score">${correctCount} / ${total}</div>
                        <p class="message">${message}</p>

                        <div class="results-stats">
                            <div class="stat-item correct-stat">
                                <div class="stat-value">✅ ${correctCount}</div>
                                <div class="stat-label">Corretas</div>
                            </div>
                            <div class="stat-item wrong-stat">
                                <div class="stat-value">❌ ${wrongCount}</div>
                                <div class="stat-label">Incorretas</div>
                            </div>
                            <div class="stat-item">
                                <div class="stat-value">${percentage}%</div>
                                <div class="stat-label">Aproveitamento</div>
                            </div>
                        </div>

                        <div style="display:flex; gap:0.75rem; justify-content:center; flex-wrap:wrap;">
                            <button class="btn btn-primary" id="btnPlayAgain">
                                🔄 Jogar Novamente
                            </button>
                        </div>

                        <div style="margin-top:1.5rem;">
                            <button class="btn btn-restart" id="btnReviewAnswers" style="padding:0.5rem 1rem; font-size:0.8rem;">
                                📋 Rever Respostas
                            </button>
                        </div>
                    </div>
                `;

                document.getElementById('btnPlayAgain').addEventListener('click', restartQuiz);
                document.getElementById('btnReviewAnswers').addEventListener('click', reviewAnswers);
            }

            function renderReview() {
                let reviewHTML = `
                    <div class="header">
                        <div class="title">
                            <span class="title-icon">📋</span>
                            <h1>Revisão das Respostas</h1>
                        </div>
                        <div class="score-display">
                            <span class="medal">🏅</span>
                            <span>Pontuação:</span>
                            <span class="score-value">${score}</span>
                            <span style="color:var(--text-muted);font-weight:400;">/ ${questions.length}</span>
                        </div>
                    </div>
                    <div style="max-height:500px; overflow-y:auto; padding-right:0.5rem; margin-bottom:1rem;">
                `;

                const letters = ['A', 'B', 'C', 'D'];

                questions.forEach((q, qIdx) => {
                    const userAnswer = answers[qIdx];
                    const isCorrect = userAnswer === true;
                    const userSelectedIndex = userAnswer !== null ? (isCorrect ? q.correctIndex : userAnswer) : null;

                    // Reconstruct: we need to know which index was selected.
                    // We stored only boolean in answers. Let's also store selectedIndex in a parallel array.
                    // We'll use selectedAnswers array below.

                    let userChoiceText = 'Não respondida';
                    if (selectedAnswers[qIdx] !== null && selectedAnswers[qIdx] !== undefined) {
                        userChoiceText = q.options[selectedAnswers[qIdx]];
                    }

                    const correctAnswerText = q.options[q.correctIndex];

                    reviewHTML += `
                        <div style="background:rgba(255,255,255,0.03); border-radius:1rem; padding:1rem 1.25rem; margin-bottom:0.75rem; border:1px solid rgba(255,255,255,0.06);">
                            <div style="display:flex; justify-content:space-between; align-items:start; gap:0.5rem; flex-wrap:wrap;">
                                <span style="color:var(--text-muted); font-size:0.75rem; text-transform:uppercase; letter-spacing:0.08em;">${q.category} · Q${qIdx+1}</span>
                                <span style="font-size:0.85rem; font-weight:600; ${isCorrect ? 'color:var(--correct)' : 'color:var(--wrong)'}">
                                    ${isCorrect ? '✅' : '❌'} ${isCorrect ? 'Correta' : 'Incorreta'}
                                </span>
                            </div>
                            <p style="color:var(--text); font-weight:600; margin:0.5rem 0 0.25rem;">${q.question}</p>
                            <p style="color:var(--text-muted); font-size:0.85rem; margin:0.25rem 0;">
                                <strong style="color:var(--accent);">Sua resposta:</strong> ${userChoiceText}
                            </p>
                            <p style="color:var(--text-muted); font-size:0.85rem; margin:0.25rem 0;">
                                <strong style="color:var(--correct);">Resposta correta:</strong> ${correctAnswerText}
                            </p>
                            <p style="color:var(--text-muted); font-size:0.8rem; margin:0.25rem 0; font-style:italic;">
                                💡 ${q.explanation}
                            </p>
                        </div>
                    `;
                });

                reviewHTML += `
                    </div>
                    <div style="display:flex; gap:0.75rem; justify-content:center; flex-wrap:wrap;">
                        <button class="btn btn-primary" id="btnBackToResults">← Voltar aos Resultados</button>
                        <button class="btn btn-restart" id="btnRestartFromReview">🔄 Reiniciar</button>
                    </div>
                `;

                container.innerHTML = reviewHTML;

                document.getElementById('btnBackToResults').addEventListener('click', () => {
                    quizFinished = true;
                    render();
                });
                document.getElementById('btnRestartFromReview').addEventListener('click', restartQuiz);
            }

            // ============ AÇÕES ============
            function handleOptionClick(e) {
                if (isAnswered) return;

                const btn = e.currentTarget;
                const optionIndex = parseInt(btn.dataset.optionIndex);
                const q = questions[currentQuestionIndex];

                selectedOptionIndex = optionIndex;
                isAnswered = true;

                const isCorrect = optionIndex === q.correctIndex;
                answers[currentQuestionIndex] = isCorrect;
                selectedAnswers[currentQuestionIndex] = optionIndex;

                if (isCorrect) {
                    score++;
                }

                render();
            }

            function goToNext() {
                if (!isAnswered) return;

                if (currentQuestionIndex === questions.length - 1) {
                    quizFinished = true;
                    render();
                } else {
                    currentQuestionIndex++;
                    selectedOptionIndex = null;
                    isAnswered = false;
                    render();
                }
            }

            function goToPrevious() {
                if (currentQuestionIndex === 0) return;

                currentQuestionIndex--;
                // Restore previous answer state if exists
                if (answers[currentQuestionIndex] !== null && answers[currentQuestionIndex] !== undefined) {
                    isAnswered = true;
                    selectedOptionIndex = selectedAnswers[currentQuestionIndex] !== null ? selectedAnswers[
                        currentQuestionIndex] : null;
                } else {
                    isAnswered = false;
                    selectedOptionIndex = null;
                }
                render();
            }

            function reviewAnswers() {
                quizFinished = true; // keep state but show review
                renderReview();
            }

            function restartQuiz() {
                currentQuestionIndex = 0;
                score = 0;
                answers = new Array(questions.length).fill(null);
                selectedAnswers = new Array(questions.length).fill(null);
                selectedOptionIndex = null;
                isAnswered = false;
                quizFinished = false;
                render();
            }

            // ============ VARIÁVEIS AUXILIARES ============
            let selectedAnswers = new Array(questions.length).fill(null);

            // ============ INICIALIZAÇÃO ============
            function init() {
                answers = new Array(questions.length).fill(null);
                selectedAnswers = new Array(questions.length).fill(null);
                render();
            }

            init();
        })();
    </script>
</body>
</html>
