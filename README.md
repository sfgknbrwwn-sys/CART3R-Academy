
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>CART3R ACADEMY</title>

<style>
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  background: #0b1220;
  color: white;
  font-family: Arial, sans-serif;
}
header {
  background: #111d30;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #26364b;
}
.logo {
  color: #55e2d0;
  font-size: 23px;
  font-weight: bold;
}
nav a {
  color: white;
  text-decoration: none;
  margin: 8px;
}
.hero {
  text-align: center;
  padding: 75px 20px;
}
.hero h1 {
  font-size: 38px;
}
.hero span {
  color: #55e2d0;
}
.hero p {
  color: #aab8ca;
  line-height: 1.8;
}
button {
  background: #55e2d0;
  color: #071521;
  border: none;
  padding: 13px 22px;
  border-radius: 9px;
  font-weight: bold;
  cursor: pointer;
}
section {
  padding: 25px 6%;
}
h2 {
  color: #55e2d0;
}
.stats {
  display: grid;
  grid-template-columns: repeat(3,1fr);
  gap: 12px;
}
.stat, .card, .quiz {
  background: #15243a;
  padding: 20px;
  border-radius: 14px;
  border: 1px solid #26364b;
}
.stat {
  text-align: center;
}
.lessons {
  display: grid;
  grid-template-columns: repeat(auto-fit,minmax(220px,1fr));
  gap: 15px;
}
.card p {
  color: #aab8ca;
  line-height: 1.7;
}
.card button {
  margin-top: 10px;
}
.quiz {
  max-width: 600px;
}
.answer {
  display: block;
  width: 100%;
  margin: 10px 0;
  background: #26364b;
  color: white;
}
.answer:hover {
  background: #35516d;
}
#result {
  color: #55e2d0;
  margin-top: 15px;
}
footer {
  text-align: center;
  padding: 30px;
  color: #8796aa;
}
@media(max-width:600px) {
  .hero h1 {font-size:29px;}
  nav a {font-size:12px;margin:3px;}
  .stats {grid-template-columns:1fr;}
}
</style>
</head>

<body>

<header>
  <div class="logo">CART3R.</div>
  <nav>
    <a href="#home">الرئيسية</a>
    <a href="#lessons">الدروس</a>
    <a href="#quiz">الاختبارات</a>
  </nav>
</header>

<div class="hero" id="home">
  <h1>مرحباً بك في <span>CART3R ACADEMY</span></h1>
  <p>تعلم الأمن السيبراني والهاك الأخلاقي<br>
  وابدأ رحلتك في عالم التقنية والحماية.</p>
  <button onclick="document.getElementById('lessons').scrollIntoView({behavior:'smooth'})">
    ابدأ التعلم
  </button>
</div>

<section>
  <div class="stats">
    <div class="stat">
      <h2 id="xp">0 XP</h2>
      <p>نقاط الخبرة</p>
    </div>
    <div class="stat">
      <h2 id="completed">0</h2>
      <p>الدروس المكتملة</p>
    </div>
    <div class="stat">
      <h2 id="level">1</h2>
      <p>المستوى</p>
    </div>
  </div>
</section>

<section id="lessons">
  <h2>📚 أكاديمية الدروس</h2>
  <div class="lessons" id="lessonList"></div>
</section>

<section id="quiz">
  <h2>🧠 اختبر معلوماتك</h2>
  <div class="quiz">
    <h3 id="question">ما الهدف من الأمن السيبراني؟</h3>
    <div id="answers"></div>
    <p id="result"></p>
    <button onclick="nextQuestion()">السؤال التالي</button>
  </div>
</section>

<footer>
  CART3R ACADEMY © 2026
  <p>تعلم بأخلاق، واحمِ العالم الرقمي.</p>
</footer>

<script>
const lessons = [
["مقدمة الأمن السيبراني","تعرف على مفهوم الأمن السيبراني وأهميته."],
["حماية كلمات المرور","تعلم إنشاء كلمات مرور قوية وتفعيل التحقق بخطوتين."],
["التصيد الإلكتروني","تعرف على الرسائل والروابط الاحتيالية."],
["أساسيات الشبكات","افهم IP وDNS وHTTPS."],
["حماية الأجهزة","تعلم تحديث الأجهزة وحماية بياناتك."],
["الهاك الأخلاقي","تعرف على الاختبار الأمني المصرح به."],
["أنظمة التشغيل","تعرف على أساسيات أنظمة التشغيل."],
["الخصوصية الرقمية","تعلم حماية معلوماتك الشخصية."],
["التشفير","تعرف على كيفية حماية البيانات بالتشفير."],
["تحليل المخاطر","تعلم اكتشاف المخاطر الأمنية وتقييمها."]
];

let xp = Number(localStorage.getItem("cart3r_xp") || 0);
let completed = JSON.parse(localStorage.getItem("cart3r_done") || "[]");

function updateStats() {
  document.getElementById("xp").textContent = xp + " XP";
  document.getElementById("completed").textContent = completed.length;
  document.getElementById("level").textContent =
    Math.floor(xp / 100) + 1;
}

function renderLessons() {
  const list = document.getElementById("lessonList");
  list.innerHTML = "";

  lessons.forEach((lesson,index) => {
    const done = completed.includes(index);

    list.innerHTML += `
      <div class="card">
        <h3>${index+1}. ${lesson[0]}</h3>
        <p>${lesson[1]}</p>
        <button onclick="completeLesson(${index})"
        ${done ? "disabled" : ""}>
        ${done ? "✓ مكتمل" : "إكمال الدرس +20 XP"}
        </button>
      </div>
    `;
  });
}

function completeLesson(index) {
  if(completed.includes(index)) return;

  completed.push(index);
  xp += 20;

  localStorage.setItem("cart3r_xp",xp);
  localStorage.setItem("cart3r_done",JSON.stringify(completed));

  renderLessons();
  updateStats();
  alert("مبروك! حصلت على 20 XP 🎉");
}

const questions = [
{
 q:"ما الهدف من الأمن السيبراني؟",
 a:["حماية البيانات","سرقة الحسابات","تعطيل الأجهزة"],
 c:0
},
{
 q:"ما معنى HTTPS؟",
 a:["اتصال مشفر","فيروس","كلمة مرور"],
 c:0
},
{
 q:"متى يجوز اختبار شبكة؟",
 a:["دون إذن","بإذن واضح","أي شبكة عامة"],
 c:1
},
{
 q:"ما فائدة المصادقة الثنائية؟",
 a:["زيادة الحماية","حذف الحساب","إبطاء الإنترنت"],
 c:0
}
];

let qIndex = 0;
let answered = false;

function showQuestion() {
  answered = false;
  const q = questions[qIndex];

  document.getElementById("question").textContent = q.q;
  document.getElementById("result").textContent = "";

  document.getElementById("answers").innerHTML =
    q.a.map((answer,i) =>
      `<button class="answer" onclick="checkAnswer(${i})">${answer}</button>`
    ).join("");
}

function checkAnswer(i) {
  if(answered) return;
  answered = true;

  const correct = questions[qIndex].c;

  if(i === correct) {
    xp += 10;
    document.getElementById("result").textContent =
      "إجابة صحيحة! +10 XP 🎉";
  } else {
    document.getElementById("result").textContent =
      "إجابة غير صحيحة. حاول التعلم مجدداً.";
  }

  localStorage.setItem("cart3r_xp",xp);
  updateStats();
}

function nextQuestion() {
  qIndex = (qIndex + 1) % questions.length;
  showQuestion();
}

renderLessons();
updateStats();
showQuestion();
</script>

</body>
</html>

