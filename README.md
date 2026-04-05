```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحدي المضارع البسيط - النفي والسؤال</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Tajawal:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Tajawal', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0;
            padding: 10px;
        }
        .glass-card {
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 2rem;
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04);
            width: 100%;
            max-width: 500px;
            overflow: hidden;
            transition: all 0.3s ease;
        }
        .option-btn {
            transition: all 0.2s transform;
            border: 2px solid #e2e8f0;
        }
        .option-btn:active {
            transform: scale(0.98);
        }
        .option-btn.correct {
            background-color: #10b981 !important;
            color: white;
            border-color: #059669;
        }
        .option-btn.wrong {
            background-color: #ef4444 !important;
            color: white;
            border-color: #dc2626;
        }
        .progress-bar {
            height: 8px;
            background-color: #e2e8f0;
            border-radius: 4px;
            margin-bottom: 20px;
        }
        .progress-fill {
            height: 100%;
            background: #667eea;
            border-radius: 4px;
            transition: width 0.3s ease;
        }
    </style>
</head>
<body>

<div class="glass-card p-6 md:p-8">
    <!-- Screen: Start -->
    <div id="start-screen" class="text-center">
        <div class="mb-6">
            <span class="text-6xl">📝</span>
        </div>
        <h1 class="text-3xl font-bold text-gray-800 mb-4">تحدي القواعد</h1>
        <p class="text-gray-600 mb-8">هل يمكنك إتقان النفي والسؤال في الـ <br><span class="font-bold text-indigo-600">Present Simple</span>؟</p>
        <button onclick="startGame()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 px-6 rounded-xl text-xl shadow-lg transition">
            ابدأ التحدي الآن
        </button>
    </div>

    <!-- Screen: Quiz -->
    <div id="quiz-screen" class="hidden">
        <div class="flex justify-between items-center mb-2">
            <span id="question-number" class="text-sm font-bold text-indigo-600">السؤال 1 من 10</span>
            <span id="score-display" class="text-sm font-bold text-green-600">النقاط: 0</span>
        </div>
        <div class="progress-bar">
            <div id="progress-fill" class="progress-fill" style="width: 10%"></div>
        </div>
        
        <div class="mb-8">
            <h2 id="question-text" class="text-xl font-bold text-gray-800 leading-relaxed text-center" dir="ltr">
                She ____ like milk.
            </h2>
        </div>

        <div id="options-container" class="space-y-3">
            <!-- Options will be injected here -->
        </div>
        
        <div id="feedback" class="mt-6 text-center font-bold hidden"></div>
    </div>

    <!-- Screen: Results -->
    <div id="result-screen" class="hidden text-center">
        <div id="result-icon" class="text-6xl mb-4">🏆</div>
        <h2 class="text-3xl font-bold text-gray-800 mb-2">انتهى التحدي!</h2>
        <p id="final-score-text" class="text-xl text-gray-600 mb-8">لقد حصلت على 8 من 10</p>
        
        <button onclick="location.reload()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-bold py-4 px-6 rounded-xl text-lg mb-4">
            إعادة المحاولة
        </button>
        <button onclick="shareGame()" class="w-full border-2 border-indigo-600 text-indigo-600 font-bold py-3 px-6 rounded-xl text-lg">
            نشر النتيجة على فيسبوك
        </button>
    </div>
</div>

<script>
    const questions = [
        { q: "She ____ like coffee.", options: ["don't", "doesn't", "not"], correct: 1, type: "negative" },
        { q: "____ you speak English?", options: ["Do", "Does", "Are"], correct: 0, type: "question" },
        { q: "They ____ play football on Sundays.", options: ["doesn't", "don't", "isn't"], correct: 1, type: "negative" },
        { q: "____ your father work in a bank?", options: ["Do", "Is", "Does"], correct: 2, type: "question" },
        { q: "I ____ want any sugar in my tea.", options: ["don't", "doesn't", "am not"], correct: 0, type: "negative" },
        { q: "Where ____ they live?", options: ["does", "do", "are"], correct: 1, type: "question" },
        { q: "He ____ study at night.", options: ["don't", "doesn't", "not"], correct: 1, type: "negative" },
        { q: "____ it rain a lot in London?", options: ["Does", "Do", "Is"], correct: 0, type: "question" },
        { q: "Cats ____ like water.", options: ["doesn't", "don't", "isn't"], correct: 1, type: "negative" },
        { q: "What time ____ the film start?", options: ["do", "is", "does"], correct: 2, type: "question" }
    ];

    let currentQuestionIndex = 0;
    let score = 0;
    let canAnswer = true;

    function startGame() {
        document.getElementById('start-screen').classList.add('hidden');
        document.getElementById('quiz-screen').classList.remove('hidden');
        showQuestion();
    }

    function showQuestion() {
        canAnswer = true;
        const q = questions[currentQuestionIndex];
        document.getElementById('question-number').innerText = `السؤال ${currentQuestionIndex + 1} من ${questions.length}`;
        document.getElementById('question-text').innerText = q.q;
        document.getElementById('progress-fill').style.width = `${((currentQuestionIndex + 1) / questions.length) * 100}%`;
        document.getElementById('score-display').innerText = `النقاط: ${score}`;
        
        const container = document.getElementById('options-container');
        container.innerHTML = '';
        document.getElementById('feedback').classList.add('hidden');

        q.options.forEach((opt, index) => {
            const btn = document.createElement('button');
            btn.className = "option-btn w-full p-4 rounded-xl text-lg font-semibold text-gray-700 bg-white hover:bg-gray-50 text-left px-6 transition duration-200";
            btn.style.direction = "ltr";
            btn.innerText = opt;
            btn.onclick = () => checkAnswer(index, btn);
            container.appendChild(btn);
        });
    }

    function checkAnswer(selectedIndex, btn) {
        if (!canAnswer) return;
        canAnswer = false;

        const correctIndex = questions[currentQuestionIndex].correct;
        const feedback = document.getElementById('feedback');
        feedback.classList.remove('hidden');

        if (selectedIndex === correctIndex) {
            score++;
            btn.classList.add('correct');
            feedback.innerText = "إجابة صحيحة! 🌟";
            feedback.className = "mt-6 text-center font-bold text-green-600";
        } else {
            btn.classList.add('wrong');
            const buttons = document.querySelectorAll('.option-btn');
            buttons[correctIndex].classList.add('correct');
            feedback.innerText = "للأسف، إجابة خاطئة. 💡";
            feedback.className = "mt-6 text-center font-bold text-red-600";
        }

        setTimeout(() => {
            currentQuestionIndex++;
            if (currentQuestionIndex < questions.length) {
                showQuestion();
            } else {
                showResults();
            }
        }, 1500);
    }

    function showResults() {
        document.getElementById('quiz-screen').classList.add('hidden');
        document.getElementById('result-screen').classList.remove('hidden');
        
        const finalScoreText = document.getElementById('final-score-text');
        finalScoreText.innerText = `لقد حصلت على ${score} من أصل ${questions.length}`;
        
        const icon = document.getElementById('result-icon');
        if (score >= 8) {
            icon.innerText = "🏆";
        } else if (score >= 5) {
            icon.innerText = "👏";
        } else {
            icon.innerText = "📚";
            finalScoreText.innerText += "\nتحتاج إلى مزيد من التدريب!";
        }
    }

    function shareGame() {
        const text = `لقد حصلت على ${score}/${questions.length} في تحدي قواعد اللغة الإنجليزية! هل يمكنك التفوق علي؟ 🔥`;
        const url = window.location.href;
        // In a real FB share, you'd use their SDK or a sharer URL
        const fbUrl = `https://www.facebook.com/sharer/sharer.php?u=${encodeURIComponent(url)}&quote=${encodeURIComponent(text)}`;
        window.open(fbUrl, '_blank');
    }
</script>

</body>
</html>

```
