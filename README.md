<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Выбери свою нейросеть</title>
<style>
    body {
        font-family: Arial, sans-serif;
        background: linear-gradient(135deg, #1e1e2f, #3a3a6a);
        color: white;
        text-align: center;
        padding: 20px;
    }

    .container {
        max-width: 700px;
        margin: auto;
        background: rgba(255,255,255,0.1);
        padding: 20px;
        border-radius: 15px;
        backdrop-filter: blur(10px);
    }

    button {
        margin: 10px;
        padding: 10px 20px;
        border: none;
        border-radius: 10px;
        cursor: pointer;
        background: #6c63ff;
        color: white;
        font-size: 16px;
        transition: 0.3s;
    }

    button:hover {
        background: #8a84ff;
    }

    .hidden {
        display: none;
    }

    .result {
        margin-top: 20px;
        padding: 15px;
        background: rgba(0,0,0,0.3);
        border-radius: 10px;
    }

    .other {
        margin-top: 20px;
        font-size: 14px;
        opacity: 0.8;
    }
</style>
</head>
<body>

<div class="container">
    <h1>🤖 Какая нейросеть тебе подходит?</h1>

    <div id="quiz">
        <p id="question">Что ты хочешь делать?</p>

        <div id="answers">
            <button onclick="answer('text')">Писать тексты</button>
            <button onclick="answer('image')">Создавать картинки</button>
            <button onclick="answer('code')">Программировать</button>
        </div>
    </div>

    <div id="result" class="result hidden"></div>
    <div id="other" class="other hidden"></div>
</div>

<script>
let step = 1;
let userChoice = {};

function answer(choice) {
    if (step === 1) {
        userChoice.type = choice;
        step++;
        nextQuestion();
    } else if (step === 2) {
        userChoice.goal = choice;
        showResult();
    }
}

function nextQuestion() {
    const q = document.getElementById("question");
    const a = document.getElementById("answers");

    q.innerText = "Для чего тебе это нужно?";
    a.innerHTML = `
        <button onclick="answer('study')">Учёба</button>
        <button onclick="answer('work')">Работа</button>
        <button onclick="answer('fun')">Развлечение</button>
    `;
}

function showResult() {
    document.getElementById("quiz").classList.add("hidden");

    const result = document.getElementById("result");
    const other = document.getElementById("other");

    let recommendation = "";
    let description = "";
    let othersText = "";

    if (userChoice.type === "text") {
        recommendation = "ChatGPT";
        description = "Отлично подходит для написания текстов, эссе, ответов и идей.";

        othersText = "Другие варианты: Jasper (маркетинг), Copy.ai (копирайтинг)";
    }

    if (userChoice.type === "image") {
        recommendation = "Midjourney";
        description = "Создаёт красивые изображения по описанию.";

        othersText = "Другие варианты: DALL·E, Stable Diffusion";
    }

    if (userChoice.type === "code") {
        recommendation = "GitHub Copilot";
        description = "Помогает писать код и ускоряет разработку.";

        othersText = "Другие варианты: Codeium, Tabnine";
    }

    result.classList.remove("hidden");
    other.classList.remove("hidden");

    result.innerHTML = `
        <h2>Тебе подходит: ${recommendation}</h2>
        <p>${description}</p>
    `;

    other.innerText = othersText;
}
</script>

</body>
</html>
