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
    <h1>Твой ИИ-ассистент</h1>
    <div id="content">
        <p>Хочешь узнать, какая нейросеть подходит именно тебе?</p>
        <button onclick="startQuiz()">Пройти тест</button>
    </div>
</div>
</div>

<script>
let step = 0;
let answers = {};

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

    let results = [
        {
            name: "ChatGPT",
            score: 0,
            reason: []
        },
        {
            name: "Midjourney",
            score: 0,
            reason: []
        },
        {
            name: "GitHub Copilot",
            score: 0,
            reason: []
        },
        {
            name: "Gamma",
            score: 0,
            reason: []
        }
    ];

    results.forEach(r => {

        if (answers.q0 === "text" && r.name === "ChatGPT") {
            r.score += 3;
            r.reason.push("подходит для работы с текстом");
        }

        if (answers.q0 === "image" && r.name === "Midjourney") {
            r.score += 3;
            r.reason.push("лучший для генерации изображений");
        }

        if (answers.q0 === "code" && r.name === "GitHub Copilot") {
            r.score += 3;
            r.reason.push("специализируется на программировании");
        }

        if (answers.q0 === "slides" && r.name === "Gamma") {
            r.score += 3;
            r.reason.push("создаёт презентации");
        }

        if (answers.q2 === "mobile" && r.name === "ChatGPT") {
            r.score += 2;
            r.reason.push("удобен на телефоне");
        }

        if (answers.q2 === "pc" && r.name === "GitHub Copilot") {
            r.score += 2;
            r.reason.push("лучше работает на компьютере");
        }

        if (answers.q3 === "beginner" && r.name === "ChatGPT") {
            r.score += 2;
            r.reason.push("подходит новичкам");
        }

    });

    results.sort((a,b) => b.score - a.score);

    app.innerHTML = "<h2>🎉 Результаты:</h2>";

    results.slice(0,2).forEach(r => {
        let percent = Math.min(100, r.score * 20);

        app.innerHTML += `
            <div class="result-card">
                <h3>${r.name} — ${percent}% совпадение</h3>
                <p>Почему: ${r.reason.join(", ")}</p>
            </div>
        `;
    });

    app.innerHTML += `<button onclick="startQuiz()">Пройти заново</button>`;
}
</script>

</body>
</html>
