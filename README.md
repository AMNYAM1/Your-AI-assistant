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

h1, h2 {
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
    margin: 15px auto;
    padding: 15px;
    border-radius: 12px;
    max-width: 90%;
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

.progress-container {
    background: #e0e0e0;
    border-radius: 12px;
    overflow: hidden;
    margin: 20px auto;
    height: 20px;
    width: 80%;
}

.progress-bar {
    height: 100%;
    width: 0;
    background: #bbaeff;
    transition: width 0.5s ease;
}
</style>
</head>
<body>

<div class="container">

<div class="card" id="app">
    <h1>Твой ИИ-ассистент</h1>
    <div class="progress-container">
        <div class="progress-bar" id="progress"></div>
    </div>
    <p id="question"></p>
    <div id="answers"></div>
</div>

</div>
<!-- КЛАССИФИКАЦИЯ -->
<div class="card">
    <h2>Классификация ИИ</h2>
    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/4712/4712109.png">
        <p><b>Большие языковые модели (LLM)</b> — обработка и генерация текста.</p>
    </div>
    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/2910/2910768.png">
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
<div class="card">
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

<script>
const questions = [
    {q: "Что ты хочешь создавать?", a:[
        {text: "Тексты", value: "text"},
        {text: "Картинки", value: "image"},
        {text: "Код", value: "code"},
        {text: "Презентации", value: "slides"}
    ]},
    {q: "В какой сфере хочешь использовать?", a:[
        {text: "Учёба", value: "study"},
        {text: "Работа", value: "work"},
        {text: "Развлечение", value: "fun"}
    ]},
    {q: "Твой уровень?", a:[
        {text: "Новичок", value: "beginner"},
        {text: "Продвинутый", value: "pro"}
    ]},
    {q: "Важен ли бесплатный доступ?", a:[
        {text: "Да", value: "free"},
        {text: "Нет", value: "paid"}
    ]},
    {q: "Важна ли простота использования?", a:[
        {text: "Да, максимально просто", value: "easy"},
        {text: "Не важна", value: "any"}
    ]}
];

let step = 0;
let answers = {};

function showQuestion() {
    const q = questions[step];
    document.getElementById("question").innerText = q.q;

    const answersDiv = document.getElementById("answers");
    answersDiv.innerHTML = "";

    // Кнопки ответов
    q.a.forEach(option => {
        const btn = document.createElement("button");
        btn.innerText = option.text;
        btn.onclick = () => next(option.value);
        answersDiv.appendChild(btn);
    });

    // Кнопка назад
    if (step > 0) {
        const backBtn = document.createElement("button");
        backBtn.innerText = "← Назад";
        backBtn.onclick = prev;
        answersDiv.appendChild(backBtn);
    }

    updateProgress();
}

function next(value) {
    answers["q" + step] = value;
    step++;

    if(step < questions.length){
        showQuestion();
    } else {
        showResult();
    }
}

function prev() {
    if (step > 0) {
        step--;
        showQuestion();
    }
}

function updateProgress() {
    const progress = document.getElementById("progress");
    progress.style.width = ((step + 1) / questions.length * 100) + "%";
}

function showResult() {
    const app = document.getElementById("app");

    let tools = {
        ChatGPT: 0,
        Claude: 0,
        Midjourney: 0,
        "DALL·E": 0,
        "GitHub Copilot": 0,
        Gamma: 0
    };

    // Баллы прибавляем
    if (answers.q0 === "text") {
        tools.ChatGPT += 2;
        tools.Claude += 2;
    }

    if (answers.q1 === "study") tools.ChatGPT += 2;
    if (answers.q1 === "work") tools["GitHub Copilot"] += 2;

    if (answers.q2 === "beginner") tools.ChatGPT += 2;
    if (answers.q2 === "pro") tools.Claude += 2;

    if (answers.q0 === "image") {
        tools.Midjourney += 2;
        tools["DALL·E"] += 2;
    }

    if (answers.q0 === "code") tools["GitHub Copilot"] += 3;
    if (answers.q0 === "slides") tools.Gamma += 3;

    if (answers.q3 === "free") tools.ChatGPT += 1;

    // Сортировка
    const sorted = Object.entries(tools)
        .sort((a, b) => b[1] - a[1])
        .slice(0, 3);

    let html = "<h2>Рекомендации:</h2>";

    sorted.forEach(([tool, score]) => {
        html += `<div class="result-card"><b>${tool}</b><br>Рейтинг: ${score}</div>`;
    });

    html += `<button onclick="location.reload()">Начать заново</button>`;

    app.innerHTML = html;
}

showQuestion();
</script>

</body>
</html>
