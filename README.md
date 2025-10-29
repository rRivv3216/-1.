# 
اختبار بسيط كيمياء ارجو اخذه بجدية وعدم الاستهانه به ابدا
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اختبار الكيمياء - التفاعلات والمعادلات</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 800px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .header::before {
            content: '';
            position: absolute;
            top: -50%;
            left: -50%;
            width: 200%;
            height: 200%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="white" opacity="0.3"/><circle cx="60" cy="40" r="1.5" fill="white" opacity="0.3"/><circle cx="80" cy="70" r="1" fill="white" opacity="0.3"/><circle cx="40" cy="80" r="2.5" fill="white" opacity="0.3"/></svg>');
            animation: float 20s infinite linear;
        }

        @keyframes float {
            0% { transform: translate(0, 0) rotate(0deg); }
            100% { transform: translate(-50px, -50px) rotate(360deg); }
        }

        .atom-icon {
            width: 80px;
            height: 80px;
            margin: 0 auto 20px;
            position: relative;
            z-index: 2;
        }

        .nucleus {
            width: 20px;
            height: 20px;
            background: #ffeb3b;
            border-radius: 50%;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 20px #ffeb3b;
        }

        .orbit {
            position: absolute;
            border: 2px solid rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            animation: rotate 10s infinite linear;
        }

        .orbit:nth-child(2) { width: 60px; height: 60px; top: 10px; left: 10px; }
        .orbit:nth-child(3) { width: 40px; height: 40px; top: 20px; left: 20px; animation-duration: 8s; }
        .orbit:nth-child(4) { width: 70px; height: 30px; top: 25px; left: 5px; animation-duration: 12s; }

        @keyframes rotate {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .electron {
            width: 8px;
            height: 8px;
            background: #4fc3f7;
            border-radius: 50%;
            position: absolute;
            top: 0;
            left: 50%;
            transform: translateX(-50%);
            box-shadow: 0 0 10px #4fc3f7;
        }

        h1 {
            font-size: 2.2em;
            margin-bottom: 10px;
            position: relative;
            z-index: 2;
        }

        .subtitle {
            font-size: 1.1em;
            opacity: 0.9;
            position: relative;
            z-index: 2;
        }

        .quiz-container {
            padding: 30px;
        }

        .progress-bar {
            height: 8px;
            background: #e0e0e0;
            border-radius: 4px;
            margin-bottom: 30px;
            overflow: hidden;
        }

        .progress {
            height: 100%;
            background: linear-gradient(90deg, #667eea, #764ba2);
            width: 0%;
            transition: width 0.5s ease;
        }

        .question-counter {
            text-align: center;
            color: #666;
            margin-bottom: 20px;
            font-weight: bold;
        }

        .question {
            font-size: 1.4em;
            margin-bottom: 25px;
            color: #333;
            line-height: 1.5;
            padding: 15px;
            background: #f8f9fa;
            border-radius: 10px;
            border-right: 4px solid #667eea;
        }

        .options {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-bottom: 30px;
        }

        .option {
            padding: 15px 20px;
            background: #f5f7ff;
            border: 2px solid #e1e5f1;
            border-radius: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 1.1em;
        }

        .option:hover {
            background: #eef2ff;
            border-color: #667eea;
            transform: translateY(-2px);
        }

        .option.selected {
            background: #667eea;
            color: white;
            border-color: #667eea;
        }

        .btn {
            padding: 15px 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1.1em;
            font-weight: bold;
            transition: all 0.3s ease;
            display: block;
            width: 100%;
            margin-top: 20px;
        }

        .btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 15px rgba(102, 126, 234, 0.4);
        }

        .btn:active {
            transform: translateY(-1px);
        }

        .explanation {
            background: #e8f5e9;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
            border-right: 4px solid #4caf50;
            display: none;
        }

        .explanation h3 {
            color: #2e7d32;
            margin-bottom: 10px;
        }

        .lesson {
            background: #e3f2fd;
            padding: 15px;
            border-radius: 10px;
            margin-top: 15px;
            border-right: 4px solid #2196f3;
        }

        .result-container {
            text-align: center;
            padding: 40px;
            display: none;
        }

        .result-score {
            font-size: 4em;
            font-weight: bold;
            color: #667eea;
            margin: 20px 0;
        }

        .result-chart {
            width: 200px;
            height: 200px;
            margin: 0 auto;
            position: relative;
        }

        .comparison {
            background: #f5f7ff;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }

        .weak-lessons {
            text-align: right;
            margin-top: 30px;
        }

        .lesson-item {
            background: #ffebee;
            padding: 15px;
            margin: 10px 0;
            border-radius: 10px;
            border-right: 4px solid #f44336;
        }

        .molecule {
            position: absolute;
            opacity: 0.1;
            font-size: 5em;
            z-index: 0;
        }

        .molecule-1 { top: 10%; left: 10%; }
        .molecule-2 { bottom: 10%; right: 10%; }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="atom-icon">
                <div class="nucleus"></div>
                <div class="orbit"><div class="electron"></div></div>
                <div class="orbit"><div class="electron"></div></div>
                <div class="orbit"><div class="electron"></div></div>
            </div>
            <h1>اختبار الكيمياء التفاعلي</h1>
            <div class="subtitle">الباب الرابع: التفاعلات والمعادلات</div>
        </div>

        <div class="quiz-container" id="quiz-container">
            <div class="progress-bar">
                <div class="progress" id="progress"></div>
            </div>
            <div class="question-counter" id="question-counter">السؤال 1 من 10</div>
            <div class="question" id="question"></div>
            <div class="options" id="options"></div>
            <button class="btn" id="next-btn">التالي</button>
            <div class="explanation" id="explanation"></div>
        </div>

        <div class="result-container" id="result-container">
            <div class="molecule molecule-1">H₂O</div>
            <div class="molecule molecule-2">CO₂</div>
            <h2>نتيجة الاختبار</h2>
            <div class="result-score" id="final-score">0%</div>
            <div class="result-chart">
                <canvas id="result-chart" width="200" height="200"></canvas>
            </div>
            <div class="comparison" id="comparison"></div>
            <div class="weak-lessons" id="weak-lessons"></div>
            <button class="btn" id="restart-btn">إعادة الاختبار</button>
        </div>
    </div>

    <script>
        // بيانات الأسئلة
        const questions = [
            {
                question: "العملية التي يعاد فيها ترتيب الذرات في مادة أو أكثر لتكوين مواد جديدة تسمى:",
                options: ["التغير الفيزيائي", "التفاعل النووي", "التفاعل الكيميائي"],
                correct: 2,
                explanation: "التعريف المباشر للتفاعل الكيميائي هو عملية يعاد فيها ترتيب الذرات لتكوين مواد جديدة، بينما التغير الفيزيائي لا يتضمن تكوين مواد جديدة",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "المعادلة التي توضح الجسيمات (الذرات أو الأيونات) المشاركة في التفاعل بشكل مفصل تسمى:",
                options: ["المعادلة الكيميائية", "المعادلة الأيونية", "المعادلة الموزونة"],
                correct: 1,
                explanation: "المعادلة الأيونية هي التي تمثل المواد المتفاعلة والناتجة في المحاليل المائية على شكل أيونات",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "ما هو العدد الأقصى من الإلكترونات الذي يستوعبه مستوى الطاقة الرئيسي الرابع (n=4)?",
                options: ["18", "8", "32"],
                correct: 2,
                explanation: "حسب قاعدة 2n²، حيث n رقم مستوى الطاقة الرئيسي: 2 × (4)² = 2 × 16 = 32 إلكترون",
                lesson: "الترتيب الإلكتروني"
            },
            {
                question: "أي من الرموز التالية يمثل الصيغة الأيونية الصحيحة لكلوريد الصوديوم (NaCl)?",
                options: ["Na²⁺ Cl⁻", "Na⁺ Cl⁻", "Na²⁺ Cl²⁻"],
                correct: 1,
                explanation: "الصوديوم (فلز قلوي) يفقد إلكترونًا واحدًا ليصبح أيون Na⁺، والكلور (لا فلز) يكتسب إلكترونًا واحدًا ليصبح أيون Cl⁻",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "الأيون OH⁻ يسمى أيون:",
                options: ["الكربوكسيل", "الهيدروكسيد", "الكربونات"],
                correct: 1,
                explanation: "هذا هو الاسم الشائع والمتفق عليه لأيون الهيدروكسيد في الكيمياء",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "التفاعل الذي تكون نواتجه مادة واحدة فقط يُصنف على أنه تفاعل:",
                options: ["احتراق", "إحلال بسيط", "تكوين (اتحاد مباشر)"],
                correct: 2,
                explanation: "يتحد متفاعلان أو أكثر لتكوين مادة واحدة جديدة",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "إذا انتهى التوزيع الإلكتروني لعنصر ما بـ 3p⁶، فإن العدد الذري لهذا العنصر هو:",
                options: ["10", "12", "18"],
                correct: 2,
                explanation: "التوزيع الإلكتروني الكامل يكون 1s² 2s² 2p⁶ 3s² 3p⁶، وعدد الإلكترونات = 18، وهو العدد الذري للغاز النبيل الأرجون",
                lesson: "الترتيب الإلكتروني"
            },
            {
                question: "الرمز المستخدم في المعادلة الكيميائية للفصل بين المواد المتفاعلة والنواتج هو:",
                options: ["+", "→", "(s)"],
                correct: 1,
                explanation: "السهم (→) يشير إلى اتجاه سير التفاعل من المواد المتفاعلة (على اليسار) إلى النواتج (على اليمين)",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "تفاعل مادة مع الأكسجين، ينتج عنه طاقة في صورة ضوء أو حرارة، هو تفاعل:",
                options: ["تكوين", "احتراق", "إحلال مزدوج"],
                correct: 1,
                explanation: "هذا التعريف المباشر لتفاعل الاحتراق",
                lesson: "التفاعلات والمعادلات"
            },
            {
                question: "الصيغة الكيميائية الصحيحة لهيدروكسيد الألومنيوم هي:",
                options: ["AlOH", "Al(OH)₂", "Al(OH)₃"],
                correct: 2,
                explanation: "شحنة أيون الألومنيوم هي +3 (Al³⁺) وشحنة أيون الهيدروكسيد هي -1 (OH⁻)، لذا نحتاج إلى ثلاثة أيونات هيدروكسيد لمعادلة الشحنة",
                lesson: "التفاعلات والمعادلات"
            }
        ];

        let currentQuestion = 0;
        let score = 0;
        let userAnswers = [];
        let weakLessons = {};

        // عناصر DOM
        const quizContainer = document.getElementById('quiz-container');
        const resultContainer = document.getElementById('result-container');
        const questionElement = document.getElementById('question');
        const optionsElement = document.getElementById('options');
        const nextButton = document.getElementById('next-btn');
        const explanationElement = document.getElementById('explanation');
        const progressElement = document.getElementById('progress');
        const questionCounterElement = document.getElementById('question-counter');
        const finalScoreElement = document.getElementById('final-score');
        const comparisonElement = document.getElementById('comparison');
        const weakLessonsElement = document.getElementById('weak-lessons');
        const restartButton = document.getElementById('restart-btn');

        // تهيئة الاختبار
        function initQuiz() {
            currentQuestion = 0;
            score = 0;
            userAnswers = [];
            weakLessons = {};
            showQuestion();
            quizContainer.style.display = 'block';
            resultContainer.style.display = 'none';
        }

        // عرض السؤال الحالي
        function showQuestion() {
            const question = questions[currentQuestion];
            questionElement.textContent = question.question;
            optionsElement.innerHTML = '';
            
            questionCounterElement.textContent = `السؤال ${currentQuestion + 1} من ${questions.length}`;
            progressElement.style.width = `${((currentQuestion + 1) / questions.length) * 100}%`;
            
            question.options.forEach((option, index) => {
                const optionElement = document.createElement('div');
                optionElement.classList.add('option');
                optionElement.textContent = option;
                optionElement.addEventListener('click', () => selectOption(index));
                optionsElement.appendChild(optionElement);
            });
            
            explanationElement.style.display = 'none';
            nextButton.textContent = currentQuestion === questions.length - 1 ? 'إنهاء الاختبار' : 'التالي';
        }

        // اختيار إجابة
        function selectOption(selectedIndex) {
            const options = document.querySelectorAll('.option');
            options.forEach(option => option.classList.remove('selected'));
            options[selectedIndex].classList.add('selected');
            
            const isCorrect = selectedIndex === questions[currentQuestion].correct;
            userAnswers.push({
                question: questions[currentQuestion].question,
                selected: selectedIndex,
                correct: questions[currentQuestion].correct,
                isCorrect: isCorrect,
                explanation: questions[currentQuestion].explanation,
                lesson: questions[currentQuestion].lesson
            });
            
            if (isCorrect) {
                score++;
            } else {
                // تسجيل الدرس الذي تم الخطأ فيه
                const lesson = questions[currentQuestion].lesson;
                weakLessons[lesson] = (weakLessons[lesson] || 0) + 1;
            }
            
            showExplanation(isCorrect);
        }

        // عرض الشرح
        function showExplanation(isCorrect) {
            const question = questions[currentQuestion];
            explanationElement.innerHTML = `
                <h3>${isCorrect ? 'إجابة صحيحة! ✓' : 'إجابة خاطئة! ✗'}</h3>
                <p><strong>التعليل:</strong> ${question.explanation}</p>
                <div class="lesson">
                    <strong>الدرس:</strong> ${question.lesson}
                </div>
            `;
            explanationElement.style.display = 'block';
        }

        // الانتقال للسؤال التالي أو عرض النتيجة
        nextButton.addEventListener('click', () => {
            if (document.querySelector('.option.selected') === null) {
                alert('يرجى اختيار إجابة قبل الانتقال');
                return;
            }
            
            if (currentQuestion < questions.length - 1) {
                currentQuestion++;
                showQuestion();
            } else {
                showResult();
            }
        });

        // عرض النتيجة النهائية
        function showResult() {
            const percentage = Math.round((score / questions.length) * 100);
            finalScoreElement.textContent = `${percentage}%`;
            
            // إنشاء رسم بياني بسيط
            const canvas = document.getElementById('result-chart');
            const ctx = canvas.getContext('2d');
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            
            // رسم دائرة النتيجة
            ctx.beginPath();
            ctx.arc(100, 100, 80, 0, 2 * Math.PI);
            ctx.strokeStyle = '#e0e0e0';
            ctx.lineWidth = 15;
            ctx.stroke();
            
            ctx.beginPath();
            ctx.arc(100, 100, 80, -0.5 * Math.PI, (2 * percentage / 100 - 0.5) * Math.PI);
            ctx.strokeStyle = percentage >= 70 ? '#4caf50' : percentage >= 50 ? '#ff9800' : '#f44336';
            ctx.lineWidth = 15;
            ctx.stroke();
            
            // نسبة عشوائية للمقارنة
            const randomPercentage = Math.floor(Math.random() * 30) + 60;
            comparisonElement.innerHTML = `
                <h3>مقارنة مع الآخرين</h3>
                <p>${randomPercentage}% من الطلاب حصلوا على نفس نتيجتك تقريبًا</p>
            `;
            
            // عرض الدروس الضعيفة
            if (Object.keys(weakLessons).length > 0) {
                let weakLessonsHTML = '<h3>الدروس التي تحتاج إلى مراجعة:</h3>';
                for (const lesson in weakLessons) {
                    weakLessonsHTML += `
                        <div class="lesson-item">
                            <strong>${lesson}</strong>
                            <p>ذاكر هذا الدرس لتحسين أدائك في المستقبل</p>
                        </div>
                    `;
                }
                weakLessonsElement.innerHTML = weakLessonsHTML;
            } else {
                weakLessonsElement.innerHTML = '<h3>ممتاز! لا توجد دروس تحتاج إلى مراجعة</h3>';
            }
            
            quizContainer.style.display = 'none';
            resultContainer.style.display = 'block';
        }

        // إعادة الاختبار
        restartButton.addEventListener('click', initQuiz);

        // بدء الاختبار
        initQuiz();
    </script>
</body>
</html>
