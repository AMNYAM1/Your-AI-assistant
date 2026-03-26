<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Выбери свою нейросеть</title>

<style>
body {
    margin: 0;
    font-family: 'Segoe UI', sans-serif;
    background: linear-gradient(135deg, #f7e8ff, #e0f7fa);
    color: #333;
}

.container {
    max-width: 700px;
    margin: 60px auto;
    background: white;
    padding: 30px;
    border-radius: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.1);
    text-align: center;
    transition: 0.5s;
}

h1 {
    margin-bottom: 20px;
}

button {
    display: block;
    width: 80%;
    margin: 10px auto;
    padding: 12px;
    border: none;
    border-radius: 12px;
    background: #d0c4ff;
    color: #333;
    font-size: 16px;
    cursor: pointer;
    transition: 0.3s;
}

button:hover {
    background: #bbaeff;
    transform: scale(1.03);
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
    margin-top: 15px;
    padding: 15px;
    border-radius: 12px;
}

.small {
    font-size: 14px;
    opacity: 0.7;
}
</style>

</head>
<body>

<div class="container fade show" id="app">
    <h1> Подбор нейросети</h1>
    <p id="question"></p>
    <div id="answers"></div>
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
        q: "Для чего тебе нужен ИИ помощник?",
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
            {text: "Средний уровень", value: "middle"},
            {text: "Продвинутый", value: "pro"}
        ]
    },
    {
        q: "Важно ли, чтобы сервис был бесплатным?",
        a: [
            {text: "Важно", value: "free"},
            {text: "Не важно", value: "any"}
        ]
    }
];

let step = 0;
let answers = {};

function showQuestion() {
    const app = document.getElementById("app");
    app.classList.remove("show");

    setTimeout(() => {
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

        app.classList.add("show");
    }, 300);
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

    let results = [];

    if (answers.q0 === "text") {
        results.push({
            name: "ChatGPT",
            desc: "Лучше всего для текстов, учёбы и общения"
        });
        results.push({
            name: "Jasper",
            desc: "Подходит для маркетинга и копирайтинга"
        });
    }

    if (answers.q0 === "image") {
        results.push({
            name: "Midjourney",
            desc: "Очень красивые и художественные изображения"
        });
        results.push({
            name: "DALL·E",
            desc: "Генерация изображений по описанию"
        });
    }

    if (answers.q0 === "code") {
        results.push({
            name: "GitHub Copilot",
            desc: "Помогает писать код быстрее"
        });
    }

    if (answers.q0 === "slides") {
        results.push({
            name: "Gamma",
            desc: "Создаёт презентации автоматически"
        });
    }

    app.innerHTML = "<h2> Твои рекомендации:</h2>";

    results.forEach(r => {
        app.innerHTML += `
            <div class="result-card">
                <h3>${r.name}</h3>
                <p>${r.desc}</p>
            </div>
        `;
    });

    app.innerHTML += `
        <p class="small">
        💡 Попробуй разные нейросети — у каждой свои сильные стороны!
        </p>
    `;
}

showQuestion();
</script>

</body>
</html>
