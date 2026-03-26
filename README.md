<!DOCTYPE html>
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
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 10px 25px rgba(0,0,0,0.08);
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
    font-size: 16px;
}

button:hover {
    background: #bbaeff;
    transform: scale(1.05);
}

.fade {
    opacity: 0;
    transform: translateY(20px);
    transition: 0.5s;
}

.fade.show {
    opacity: 1;
    transform: translateY(0);
}

.result-card {
    background: #f3f0ff;
    padding: 15px;
    margin-top: 15px;
    border-radius: 12px;
}
</style>

</head>
<body>

<div class="container">
<div class="card fade show" id="app">
    <h1>🤖 Your AI Assistant</h1>
    <div id="content">
        <p>Хочешь узнать, какая нейросеть подходит именно тебе?</p>
        <button onclick="startQuiz()">🚀 Пройти тест</button>
    </div>
</div>
</div>

<script>
let step = 0;
let answers = {};

const questions = [
    {
        q: "Что ты хочешь делать?",
        a: [
            {text: "Тексты", value: "text"},
            {text: "Картинки", value: "image"},
            {text: "Код", value: "code"},
            {text: "Презентации", value: "slides"}
        ]
    },
    {
        q: "Для чего?",
        a: [
            {text: "Учёба", value: "study"},
            {text: "Работа", value: "work"},
            {text: "Развлечение", value: "fun"}
        ]
    },
    {
        q: "На каком устройстве будешь работать?",
        a: [
            {text: "Телефон", value: "mobile"},
            {text: "Компьютер", value: "pc"}
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
            {text: "Не важно", value: "any"}
        ]
    }
];

const neuralNetworks = [
    {name: "ChatGPT", type: "text", reason: [], score: 0},
    {name: "Claude", type: "text", reason: [], score: 0},
    {name: "Bard", type: "text", reason: [], score: 0},
    {name: "Midjourney", type: "image", reason: [], score: 0},
    {name: "DALL·E", type: "image", reason: [], score: 0},
    {name: "Stable Diffusion", type: "image", reason: [], score: 0},
    {name: "GitHub Copilot", type: "code", reason: [], score: 0},
    {name: "Replit Ghostwriter", type: "code", reason: [], score: 0},
    {name: "Gamma", type: "slides", reason: [], score: 0},
    {name: "Tome", type: "slides", reason: [], score: 0}
];

function startQuiz() {
    step = 0;
    answers = {};
    showQuestion();
}

function showQuestion() {
    const app = document.getElementById("content");
    const q = questions[step];

    app.innerHTML = `<p>${q.q}</p>`;

    q.a.forEach(option => {
        const btn = document.createElement("button");
        btn.innerText = option.text;
        btn.onclick = () => next(option.value);
        app.appendChild(btn);
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
    const app = document.getElementById("content");

    // Сброс
    neuralNetworks.forEach(n => { n.score = 0; n.reason = []; });

    neuralNetworks.forEach(n => {
        if (answers.q0 === "text" && n.type === "text") { n.score += 3; n.reason.push("подходит для работы с текстом"); }
        if (answers.q0 === "image" && n.type === "image") { n.score += 3; n.reason.push("лучший для генерации изображений"); }
        if (answers.q0 === "code" && n.type === "code") { n.score += 3; n.reason.push("специализируется на коде"); }
        if (answers.q0 === "slides" && n.type === "slides") { n.score += 3; n.reason.push("создаёт презентации"); }

        if (answers.q2 === "mobile" && (n.name === "ChatGPT" || n.name === "Bard" || n.name === "Claude")) { n.score += 2; n.reason.push("удобен на телефоне"); }
        if (answers.q2 === "pc" && (n.name === "GitHub Copilot" || n.name === "Replit Ghostwriter")) { n.score += 2; n.reason.push("лучше работает на компьютере"); }

        if (answers.q3 === "beginner" && (n.name === "ChatGPT" || n.name === "DALL·E" || n.name === "Gamma")) { n.score += 2; n.reason.push("подходит новичкам"); }
        if (answers.q3 === "pro" && (n.name === "Claude" || n.name === "Midjourney" || n.name === "Stable Diffusion")) { n.score += 2; n.reason.push("подходит продвинутым"); }
    });

    neuralNetworks.sort((a,b) => b.score - a.score);

    app.innerHTML = "<h2>🎉 Результаты:</h2>";

    neuralNetworks.slice(0,3).forEach(n => {
        let percent = Math.min(100, n.score * 20);
        app.innerHTML += `
            <div class="result-card">
                <h3>${n.name} — ${percent}% совпадение</h3>
                <p>Почему: ${n.reason.join(", ")}</p>
            </div>
        `;
    });

    app.innerHTML += `<button onclick="startQuiz()">Пройти заново</button>`;
}
</script>

</body>
</html>
