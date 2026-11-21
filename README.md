
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>LearnNova - Grade 7 Learning Hub</title>
<style>
*{box-sizing:border-box}
body{margin:0;font-family:Arial,sans-serif;background:#eef4ff;color:#26334d}
header{background:linear-gradient(120deg,#2457d6,#713bd1);color:white;text-align:center;padding:30px 15px}
header h1{margin:0;font-size:32px}
header p{margin-bottom:0}
nav{background:white;padding:12px;text-align:center;box-shadow:0 2px 8px #0001}
button{border:0;border-radius:8px;padding:11px 16px;background:#315bd5;color:white;cursor:pointer;margin:4px;font-weight:bold}
button:hover{background:#193fa8}
button.secondary{background:#64748b}
.container{max-width:1000px;margin:25px auto;padding:15px}
.panel{background:white;padding:22px;border-radius:14px;box-shadow:0 4px 15px #0000000c;margin-bottom:20px}
.subjects{display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:14px}
.subject{background:#f2f5ff;border:1px solid #dce5ff;border-radius:12px;padding:20px;text-align:center}
.subject h3{margin-top:5px}
.paper-list{display:flex;flex-wrap:wrap;gap:10px}
.question{border:1px solid #e1e6f0;border-radius:12px;padding:16px;margin:15px 0}
.question h3{font-size:17px}
.option{display:block;padding:8px;margin:5px 0;background:#f5f7fc;border-radius:7px;cursor:pointer}
input[type=text],select{padding:10px;border:1px solid #cbd5e1;border-radius:7px;width:100%;max-width:450px;margin-top:8px}
.result{padding:20px;border-radius:12px;background:#e8f7ed;margin-top:20px}
.hidden{display:none}
.small{color:#64748b;font-size:13px}
footer{text-align:center;padding:25px;color:#64748b}
</style>
</head>
<body>

<header>
<h1>📚 LearnNova</h1>
<p>Grade 7 Learning Hub | Learn • Practise • Improve</p>
</header>

<nav>
<button onclick="showHome()">Home</button>
<button onclick="showSubjects()">All Subjects</button>
</nav>

<div class="container">

<section id="home" class="panel">
<h2>Welcome to Grade 7 Learning Hub!</h2>
<p>Choose a subject and practise with online examination papers.</p>
<ul>
<li>7 school subjects</li>
<li>3 practice papers for each subject</li>
<li>6 question types</li>
<li>Automatic marking</li>
<li>Results out of 100</li>
</ul>
<button onclick="showSubjects()">Start Learning</button>
</section>

<section id="subjectsPage" class="panel hidden">
<h2>Choose Your Subject</h2>
<div id="subjects" class="subjects"></div>
</section>

<section id="papersPage" class="panel hidden">
<button class="secondary" onclick="showSubjects()">← Back to Subjects</button>
<h2 id="subjectTitle"></h2>
<p>Select a practice paper.</p>
<div id="papers" class="paper-list"></div>
</section>

<section id="examPage" class="panel hidden">
<button class="secondary" onclick="showSubjects()">← Exit Paper</button>
<h2 id="examTitle"></h2>
<p class="small">Answer all questions. Each question is worth marks.</p>
<div id="questions"></div>
<button onclick="submitExam()">Submit Answers</button>
<button class="secondary" onclick="showPapers()">Back to Papers</button>
<div id="result"></div>
</section>

</div>

<footer>LearnNova | Grade 7 Learning Hub</footer>

<script>
const subjects = [
"English","Science","Mathematics","Civic Education",
"Geography","Health","ICT"
];

const banks = {
"English":[
["A noun is a naming word.","noun","verb","adjective","noun"],
["The opposite of hot is _____.","cold","warm","boiling","cold"],
["A verb shows an _____.","action","colour","place","action"],
["The plural of child is _____.","children","childs","childes","children"],
["A question ends with a _____.","question mark","full stop","comma","question mark"],
["The past tense of go is _____.","went","goed","going","went"]
],
"Science":[
["Which organ pumps blood?","heart","lungs","brain","heart"],
["Plants make food mainly in their _____.","leaves","roots","flowers","leaves"],
["Which is a source of light?","Sun","Moon","Earth","Sun"],
["Water freezes into _____.","ice","steam","air","ice"],
["Humans breathe in _____.","oxygen","carbon dioxide","nitrogen only","oxygen"],
["The planet we live on is _____.","Earth","Mars","Venus","Earth"]
],
"Mathematics":[
["What is 25 + 15?","40","35","45","40"],
["What is 9 × 6?","54","45","63","54"],
["What is half of 80?","40","20","60","40"],
["What is 100 - 37?","63","67","73","63"],
["A triangle has how many sides?","3","4","5","3"],
["What is 1/2 + 1/2?","1","1/2","2","1"]
],
"Civic Education":[
["What is a responsibility?","A duty","A game","A holiday","A duty"],
["Which protects public property?","Using it carefully","Damaging it","Writing on it","Using it carefully"],
["What helps people live peacefully?","Respect","Fighting","Discrimination","Respect"],
["What is a community?","People living together","One building","A vehicle","People living together"],
["Which is a public service?","Education","Private toy","Personal game","Education"],
["Why do we follow rules?","For safety and order","To create trouble","To stop learning","For safety and order"]
],
"Geography":[
["Which is the largest ocean?","Pacific Ocean","Indian Ocean","Arctic Ocean","Pacific Ocean"],
["What is the capital of Sri Lanka?","Sri Jayawardenepura Kotte","Kandy","Galle","Sri Jayawardenepura Kotte"],
["A map shows _____.","places","only animals","only food","places"],
["Which direction is opposite to north?","South","East","West","South"],
["A river flows into a _____.","sea or lake","mountain top","desert only","sea or lake"],
["The Earth is shaped approximately like a _____.","sphere","cube","flat square","sphere"]
],
"Health":[
["Which habit helps prevent illness?","Handwashing","Eating dirty food","Avoiding sleep","Handwashing"],
["Which food group helps build the body?","Protein foods","Sweets only","Soft drinks","Protein foods"],
["Why is exercise important?","Keeps the body healthy","Makes us ill","Stops growth","Keeps the body healthy"],
["Which is a healthy drink?","Clean water","Dirty water","Paint","Clean water"],
["How many hours of sleep do children generally need?","Adequate sleep each night","No sleep","One hour only","Adequate sleep each night"],
["What should we do before eating?","Wash hands","Touch dirt","Play in mud","Wash hands"]
],
"ICT":[
["What does CPU stand for?","Central Processing Unit","Computer Personal Unit","Control Power User","Central Processing Unit"],
["Which device is used to type?","Keyboard","Monitor","Speaker","Keyboard"],
["Which device displays information?","Monitor","Mouse","Microphone","Monitor"],
["What is the internet?","A global network","A keyboard","A printer","A global network"],
["Which is an input device?","Mouse","Monitor","Speaker","Mouse"],
["What should you use to protect an account?","A strong password","Your name only","No password","A strong password"]
]
};

let currentSubject="";
let currentPaper=1;
let examQuestions=[];

function hideAll(){
["home","subjectsPage","papersPage","examPage"].forEach(id=>{
document.getElementById(id).classList.add("hidden");
});
}

function showHome(){
hideAll();
document.getElementById("home").classList.remove("hidden");
}

function showSubjects(){
hideAll();
document.getElementById("subjectsPage").classList.remove("hidden");
const area=document.getElementById("subjects");
area.innerHTML="";
subjects.forEach(subject=>{
const div=document.createElement("div");
div.className="subject";
div.innerHTML=`<h3>${subject}</h3><p>3 Practice Papers</p>`;
const btn=document.createElement("button");
btn.textContent="Open Subject";
btn.onclick=()=>openSubject(subject);
div.appendChild(btn);
area.appendChild(div);
});
}

function openSubject(subject){
currentSubject=subject;
hideAll();
document.getElementById("papersPage").classList.remove("hidden");
document.getElementById("subjectTitle").textContent=subject;
const area=document.getElementById("papers");
area.innerHTML="";
for(let i=1;i<=3;i++){
const btn=document.createElement("button");
btn.textContent="Practice Paper "+i;
btn.onclick=()=>startExam(i);
area.appendChild(btn);
}
}

function showPapers(){
openSubject(currentSubject);
}

function startExam(paper){
currentPaper=paper;
hideAll();
document.getElementById("examPage").classList.remove("hidden");
document.getElementById("examTitle").textContent=
currentSubject+" - Practice Paper "+paper;
document.getElementById("result").innerHTML="";

const bank=banks[currentSubject];
const start=(paper-1)*2;
const chosen=[bank[start],bank[start+1]];

examQuestions=[];
chosen.forEach((item,i)=>{
examQuestions.push({type:"MCQ",question:item[0],options:item.slice(1,4),answer:item[4]});
examQuestions.push({type:"True / False",question:item[0],answerTF:i===0});
examQuestions.push({type:"Fill in the blank",question:item[0],answer:item[4]});
examQuestions.push({type:"Short answer",question:item[0],answer:item[4]});
examQuestions.push({type:"Matching",question:item[0],options:item.slice(1,4),answer:item[4]});
examQuestions.push({type:"Multiple selection",question:item[0],options:item.slice(1,4),answer:item[4]});
});
renderQuestions();
}

function renderQuestions(){
const area=document.getElementById("questions");
area.innerHTML="";
examQuestions.forEach((q,i)=>{
const div=document.createElement("div");
div.className="question";
let html=`<h3>Question ${i+1} — ${q.type}</h3><p>${q.question}</p>`;

if(q.type==="MCQ"){
q.options.forEach(option=>{
html+=`<label class="option"><input type="radio" name="q${i}" value="${option}"> ${option}</label>`;
});
}else if(q.type==="True / False"){
html+=`<label class="option"><input type="radio" name="q${i}" value="true"> True</label>
<label class="option"><input type="radio" name="q${i}" value="false"> False</label>`;
}else if(q.type==="Matching"){
html+=`<select id="q${i}"><option value="">Choose the matching answer</option>
${q.options.map(o=>`<option value="${o}">${o}</option>`).join("")}</select>`;
}else if(q.type==="Multiple selection"){
html+=`<p class="small">Select the answer you think is correct.</p>`;
q.options.forEach(option=>{
html+=`<label class="option"><input type="checkbox" name="q${i}" value="${option}"> ${option}</label>`;
});
}else{
html+=`<input type="text" id="q${i}" placeholder="Type your answer">`;
}
div.innerHTML=html;
area.appendChild(div);
});
}

function normalize(value){
return String(value||"").trim().toLowerCase().replace(/[.,!?]/g,"");
}

function getAnswer(i,q){
if(q.type==="MCQ"||q.type==="True / False"){
const selected=document.querySelector(`input[name="q${i}"]:checked`);
return selected?selected.value:"";
}
if(q.type==="Multiple selection"){
const selected=document.querySelector(`input[name="q${i}"]:checked`);
return selected?selected.value:"";
}
return document.getElementById("q"+i).value;
}

function submitExam(){
let correct=0;
examQuestions.forEach((q,i)=>{
const userAnswer=getAnswer(i,q);
let answer=q.answer;
if(q.type==="True / False") answer=String(q.answerTF);
if(normalize(userAnswer)===normalize(answer)) correct++;
});
const percentage=Math.round(correct/examQuestions.length*100);
document.getElementById("result").innerHTML=`
<div class="result">
<h2>Your Results</h2>
<h1>${percentage} / 100</h1>
<p>Correct answers: ${correct} out of ${examQuestions.length}</p>
<p>${percentage>=75?"Excellent work! Keep learning!":
percentage>=50?"Good effort! Practise more!":"Keep practising. You can improve!"}</p>
<button onclick="startExam(currentPaper)">Try Again</button>
<button class="secondary" onclick="showPapers()">Choose Another Paper</button>
</div>`;
}
showHome();
</script>
</body>
</html>
