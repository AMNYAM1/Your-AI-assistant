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
    margin-bottom: 20px;
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

.info-card {
    background: #e7e0ff;
    padding: 20px;
    margin: 10px 0;
    border-radius: 12px;
    display: flex;
    align-items: center;
    transition: 0.5s;
    opacity: 0;
    transform: translateY(20px);
}

.info-card.show {
    opacity: 1;
    transform: translateY(0);
}

.info-card img {
    width: 60px;
    height: 60px;
    border-radius: 12px;
    margin-right: 15px;
}

.info-card div {
    text-align: left;
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

<div class="card fade" id="classification">
    <h2>Классификация нейросетей и разработчики</h2>
    <div id="network-info">
        <!-- Здесь появятся карточки нейросетей -->
    </div>
</div>
</div>

<script>
let step = 0;
let answers = {};

const questions = [
    { q: "Что ты хочешь делать?", a: [
        {text: "Тексты", value: "text"},
        {text: "Картинки", value: "image"},
        {text: "Код", value: "code"},
        {text: "Презентации", value: "slides"}
    ]},
    { q: "Для каких целей?", a: [
        {text: "Учёба", value: "study"},
        {text: "Работа", value: "work"},
        {text: "Развлечение", value: "fun"}
    ]},
    { q: "На каком устройстве будешь работать?", a: [
        {text: "Телефон", value: "mobile"},
        {text: "Компьютер", value: "pc"}
    ]},
    { q: "Твой уровень?", a: [
        {text: "Новичок", value: "beginner"},
        {text: "Продвинутый", value: "pro"}
    ]},
    { q: "Важен ли бесплатный доступ?", a: [
        {text: "Да", value: "free"},
        {text: "Не важно", value: "any"}
    ]}
];

const neuralNetworks = [
    {name: "ChatGPT", type: "Языковая модель", developer: "OpenAI", reason: [], score: 0, img: "https://i.imgur.com/5U70nY1.png"},
    {name: "Claude", type: "Языковая модель", developer: "Anthropic", reason: [], score: 0, img: "https://i.imgur.com/XQkYUPM.png"},
    {name: "Bard", type: "Языковая модель", developer: "Google", reason: [], score: 0, img: "https://i.imgur.com/x5YI9bK.png"},
    {name: "Gemma", type: "Языковая модель", developer: "Google", reason: [], score: 0, img: "https://i.imgur.com/8tHZKZZ.png"},
    {name: "Gemini", type: "Языковая модель", developer: "Google", reason: [], score: 0, img: "https://i.imgur.com/8QJbG2v.png"},
    {name: "Midjourney", type: "Генератор изображений", developer: "Midjourney Inc.", reason: [], score: 0, img: "https://i.imgur.com/TJbX0GJ.png"},
    {name: "DALL·E", type: "Генератор изображений", developer: "OpenAI", reason: [], score: 0, img: "https://i.imgur.com/yF8nH5k.png"},
    {name: "Stable Diffusion", type: "Генератор изображений", developer: "Stability AI", reason: [], score: 0, img: "https://i.imgur.com/3HkqV1k.png"},
    {name: "GitHub Copilot", type: "Помощник кодирования", developer: "GitHub / OpenAI", reason: [], score: 0, img: "https://i.imgur.com/pkUHF8C.png"},
    {name: "Replit Ghostwriter", type: "Помощник кодирования", developer: "Replit", reason: [], score: 0, img: "https://i.imgur.com/f0HTiXG.png"},
    {name: "Gamma", type: "Генератор презентаций", developer: "Gamma App", reason: [], score: 0, img: "https://i.imgur.com/AJ0k0yb.png"},
    {name: "Tome", type: "Генератор презентаций", developer: "Tome App", reason: [], score: 0, img: "https://i.imgur.com/jW0eH0k.png"}
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
    neuralNetworks.forEach(n => { n.score = 0; n.reason = []; });

    neuralNetworks.forEach(n => {
        // Направление
        if (answers.q0 === "text" && n.type === "Языковая модель") { 
            n.score += 3; n.reason.push("подходит для работы с текстом"); 
        }
        if (answers.q0 === "image" && n.type === "Генератор изображений") { 
            n.score += 3; n.reason.push("лучший для генерации изображений"); 
        }
        if (answers.q0 === "code" && n.type === "Помощник кодирования") { 
            n.score += 3; n.reason.push("специализируется на коде"); 
        }
        if (answers.q0 === "slides" && n.type === "Генератор презентаций") { 
            n.score += 3; n.reason.push("создаёт презентации"); 
        }

        // Учёба
        if (answers.q1 === "study" && n.name === "Gemini") { 
            n.score += 3; 
            n.reason.push("специализируется на учебе и физике"); 
        }

        // Устройство
        if (answers.q2 === "mobile" && ["ChatGPT","Bard","Claude","Gemma","Gemini"].includes(n.name)) {
            n.score += 2; n.reason.push("удобно на телефоне");
        }
        if (answers.q2 === "pc" && ["GitHub Copilot","Replit Ghostwriter"].includes(n.name)) {
            n.score += 2; n.reason.push("лучше работает на компьютере");
        }

        // Уровень
        if (answers.q3 === "beginner" && ["ChatGPT","DALL·E","Gamma","Gemma","Gemini"].includes(n.name)) {
            n.score += 2; n.reason.push("подходит новичкам");
        }
        if (answers.q3 === "pro" && ["Claude","Midjourney","Stable Diffusion"].includes(n.name)) {
            n.score += 2; n.reason.push("подходит продвинутым");
        }
    });

    neuralNetworks.sort((a,b) => b.score - a.score);

    app.innerHTML = "<h2> Результаты:</h2>";

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

    // блок классификации с плавной анимацией
    const infoContainer = document.getElementById("network-info");
    infoContainer.innerHTML = "";
    neuralNetworks.forEach((n, i) => {
        const card = document.createElement("div");
        card.className = "info-card";
        card.innerHTML = `<img src="${n.img}" alt="${n.name}"><div><strong>${n.name}</strong><br>Тип: ${n.type}<br>Разработчик: ${n.developer}</div>`;
        infoContainer.appendChild(card);
        setTimeout(() => card.classList.add("show"), 200 * i); // плавное появление
    });

    document.getElementById("classification").classList.add("show");
}
</script>

</body>
</html>
</html>

</body>
</html>
