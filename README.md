<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Подбор нейросети</title>

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

/* Анимации */
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

<!-- QUIZ -->
<div class="card fade show" id="app">
    <h1>🤖 Подбор нейросети</h1>
    <p id="question"></p>
    <div id="answers"></div>
</div>

<!-- КЛАССИФИКАЦИЯ -->
<div class="card fade">
    <h2>🧠 Классификация искусственного интеллекта</h2>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/4712/4712109.png">
        <p><b>Текстовые ИИ</b> — пишут тексты, отвечают на вопросы, помогают учиться.</p>
    </div>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/2910/2910768.png">
        <p><b>Генераторы изображений</b> — создают картинки и дизайн.</p>
    </div>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/2721/2721293.png">
        <p><b>Кодовые ассистенты</b> — помогают писать программы.</p>
    </div>

    <div class="section">
        <img src="https://cdn-icons-png.flaticon.com/512/3135/3135755.png">
        <p><b>Презентационные ИИ</b> — делают слайды и оформление.</p>
    </div>
</div>

<!-- РАЗРАБОТЧИКИ -->
<div class="card fade">
    <h2>🏢 Кто создаёт нейросети?</h2>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/4/4d/OpenAI_Logo.svg">
        <p><b>OpenAI</b> — создатели ChatGPT и DALL·E</p>
    </div>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/5/51/Microsoft_logo.svg">
        <p><b>Microsoft</b> — развивает Copilot и AI-инструменты</p>
    </div>

    <div class="section">
        <img src="https://upload.wikimedia.org/wikipedia/commons/a/ab/Meta-Logo.png">
        <p><b>Meta</b> — создаёт открытые модели ИИ</p>
    </div>

</div>

</div>

<script>
const questions = [
    {
        q: "Что ты хочешь делать?",
        a: [
            {text: "Писать тексты", value: "text"},
            {text: "Создавать картинки", value: "image"},
            {text: "Программировать", value: "code"},
            {text: "Делать презентации", value: "slides"}
        ]
    },
    {
        q: "Для чего тебе это нужно?",
        a: [
            {text: "Учёба", value: "study"},
            {text: "Работа", value: "work"},
            {text: "Развлечение", value: "fun"}
        ]
    },
    {
        q: "Насколько ты опытен?",
        a: [
            {text: "Новичок", value: "beginner"},
            {text: "Продвинутый", value: "pro"}
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

    let html = "<h2>🎉 Твои рекомендации:</h2>";

    if (answers.q0 === "text") {
        html += `<div class="result-card"><b>ChatGPT</b><br>Лучше всего для текстов и учёбы</div>`;
        html += `<div class="result-card"><b>Jasper</b><br>Подходит для маркетинга</div>`;
    }

    if (answers.q0 === "image") {
        html += `<div class="result-card"><b>Midjourney</b><br>Художественные изображения</div>`;
        html += `<div class="result-card"><b>DALL·E</b><br>Генерация картинок</div>`;
    }

    if (answers.q0 === "code") {
        html += `<div class="result-card"><b>GitHub Copilot</b><br>Помогает писать код</div>`;
    }

    if (answers.q0 === "slides") {
        html += `<div class="result-card"><b>Gamma</b><br>Создаёт презентации</div>`;
    }

    app.innerHTML = html;
}

showQuestion();

/* АНИМАЦИЯ ПРИ СКРОЛЛЕ */
const faders = document.querySelectorAll('.fade');

const appearOptions = {
    threshold: 0.2
};

const appearOnScroll = new IntersectionObserver(function(entries, observer) {
    entries.forEach(entry => {
        if (!entry.isIntersecting) return;
        entry.target.classList.add("show");
        observer.unobserve(entry.target);
    });
}, appearOptions);

faders.forEach(fader => {
    appearOnScroll.observe(fader);
});
</script>

</body>
</html>
