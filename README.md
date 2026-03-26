<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Your AI Assistant</title>

<style>
body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #f7e8ff, #e0f7fa);
    color: #333;
}

.container {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
}

.card {
    background: white;
    padding: 25px;
    border-radius: 20px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.08);
    margin-bottom: 30px;
    text-align: center;
}

h1 {
    text-align: center;
}

button {
    display: block;
    width: 80%;
    margin: 10px auto;
    padding: 12px;
    border: none;
    border-radius: 12px;
    background: #d0c4ff;
    cursor: pointer;
    transition: 0.3s;
}

button:hover {
    background: #bbaeff;
    transform: scale(1.03);
}

.result-card {
    background: #f3f0ff;
    margin-top: 15px;
    padding: 15px;
    border-radius: 12px;
}

.fade {
    opacity: 0;
    transform: translateY(40px);
    transition: 0.8s ease;
}

.fade.show {
    opacity: 1;
    transform: translateY(0);
}

.section {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-top: 20px;
}

.section img {
    width: 120px;
    border-radius: 12px;
}

@media (max-width: 600px) {
    .section {
        flex-direction: column;
    }
}
</style>
</head>
<body>

<div class="container">

<div class="card fade show" id="app">
    <h1>Твой ИИ-ассистент</h1>
    <p id="question"></p>
    <div id="answers"></div>
</div>

<!-- КЛАССИФИКАЦИЯ -->
<div class="card fade">
    <h2>Классификация ИИ</h2>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/4712/4712109.png">
        <p><b>Большие языковые модели (LLM)</b> — обработка и генерация текста.</p>
    </div>

    <div class="section">
        <img src="https://images.unsplash.com/photo-1513364776144-60967b0f800f">
        <p><b>Диффузионные модели</b> — генерация изображений.</p>
    </div>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/2721/2721293.png">
        <p><b>Кодовые модели</b> — генерация и анализ программного кода.</p>
    </div>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/3135/3135755.png">
        <p><b>Мультимодальные модели</b> — работают с текстом, изображениями и др.</p>
    </div>
</div>

<!-- РАЗРАБОТЧИКИ -->
<div class="card fade">
    <h2>Разработчики ИИ</h2>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/4/4d/OpenAI_Logo.svg">
        <p><b>OpenAI</b> — ChatGPT, DALL·E</p>
    </div>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/9/96/Microsoft_logo_%282012%29.svg">
        <p><b>Microsoft</b> — Copilot, Azure AI</p>
    </div>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/a/ab/Meta-Logo.png">
        <p><b>Meta</b> — открытые модели (LLaMA)</p>
    </div>

</div>

</div>

<script>
const questions = [
    {
        q: "Что ты хочешь создавать?",
        a: [
            {text: "Тексты", value: "text"},
            {text: "Картинки", value: "image"},
            {text: "Код", value: "code"},
            {text: "Презентации", value: "slides"}
        ]
    },
    {
        q: "В какой сфере хочешь использовать?",
        a: [
            {text: "Учёба", value: "study"},
            {text: "Работа", value: "work"},
            {text: "Развлечение", value: "fun"}
        ]
    },
    {
        q: "Твой уровень?",
        a: [
            {text: "Новичок", value: "beginner"},
            {text: "Продвинутый", value: "pro"}
        ]
    },
    {
        q: "Нужен ли бесплатный доступ?",
        a: [
            {text: "Да", value: "free"},
            {text: "Нет", value: "paid"}
        ]
    },
    {
        q: "Важна ли простота использования?",
        a: [
            {text: "Да, максимально просто", value: "easy"},
            {text: "Не важно", value: "any"}
        ]
    }
];

let step = 0;
let answers = {};

function showQuestion() {
    const q = questions[step];
    document.getElementById("question").innerText = q.q;

    const answersDiv = document.getElementById("answers");
    answersDiv.innerHTML = "";

    q.a.forEach(option => {
        const btn = document.createElement("button");
        btn.innerText = option.text;
        btn.onclick = () => next(option.value);
        answersDiv.appendChild(btn);
    });
}

function next(value) {
    answers["q" + step] = value;
    step++;

    if (step < questions.length) {
        showQuestion();
    } else {
        showResult();
    }
}

function showResult() {
    const app = document.getElementById("app");

    let html = "<h2>Рекомендации:</h2>";

    if (answers.q0 === "text") {
        html += `<div class="result-card"><b>ChatGPT</b><br>Универсальная LLM</div>`;
        html += `<div class="result-card"><b>Claude</b><br>Хорош для длинных текстов</div>`;
    }

    if (answers.q0 === "image") {
        html += `<div class="result-card"><b>Midjourney</b><br>Художественный стиль</div>`;
        html += `<div class="result-card"><b>DALL·E</b><br>Генерация по описанию</div>`;
    }

    if (answers.q0 === "code") {
        html += `<div class="result-card"><b>GitHub Copilot</b><br>Помощь в коде</div>`;
    }

    if (answers.q0 === "slides") {
        html += `<div class="result-card"><b>Gamma</b><br>Презентации</div>`;
    }

    app.innerHTML = html;
}

showQuestion();

/* АНИМАЦИЯ */
const faders = document.querySelectorAll('.fade');

const observer = new IntersectionObserver(entries => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add("show");
        }
    });
});

faders.forEach(el => observer.observe(el));
</script>

</body>
</html>
