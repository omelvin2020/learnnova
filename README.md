
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Grade 7 Learning Hub</title>

<style>
*{box-sizing:border-box}
body{
 margin:0;
 font-family:Arial,sans-serif;
 background:linear-gradient(135deg,#e5f3ff,#f4eaff);
 color:#28334d;
}
header{
 background:white;
 text-align:center;
 padding:28px 15px;
 border-radius:0 0 25px 25px;
 box-shadow:0 4px 15px #0001;
}
h1{color:#6846c7}
.container{max-width:1000px;margin:auto;padding:20px}
.grid{
 display:grid;
 grid-template-columns:repeat(auto-fit,minmax(210px,1fr));
 gap:17px;
}
.card{
 background:white;
 padding:25px;
 text-align:center;
 border-radius:18px;
 cursor:pointer;
 box-shadow:0 4px 12px #0001;
 transition:.2s;
}
.card:hover{transform:translateY(-4px);background:#f4efff}
.emoji{font-size:42px}
button{
 background:#6846c7;
 color:white;
 border:0;
 padding:13px 20px;
 margin:7px;
 border-radius:12px;
 font-size:16px;
 cursor:pointer;
}
button:hover{background:#4c31a1}
.panel{
 background:white;
 padding:25px;
 border-radius:20px;
 margin-top:20px;
 box-shadow:0 4px 12px #0001;
}
.hidden{display:none}
.paper-question{
 background:#f5f2ff;
 padding:15px;
 margin:15px 0;
 border-radius:13px;
 text-align:left;
}
.option{display:block;margin:9px 0}
.correct{background:#d7f8df}
.wrong{background:#ffe0e0}
.feedback{padding:10px;border-radius:10px;margin-top:8px}
#result{font-size:25px;color:#6846c7;font-weight:bold}
footer{text-align:center;padding:25px}
</style>
</head>

<body>

<header>
 <h1>🎓 Grade 7 Learning Hub</h1>
 <h2>English Medium Online Examination Centre</h2>
 <p>📚 7 Subjects | 📝 21 Papers | 🏆 100 Marks Each</p>
</header>

<div class="container">

<!-- HOME PAGE -->
<div id="home">
 <h2>📚 Choose Your Subject</h2>
 <div class="grid" id="subjects"></div>
</div>

<!-- SUBJECT PAGE -->
<div id="subjectPage" class="hidden">
 <div class="panel">
  <h1 id="subjectTitle"></h1>
  <p>Grade 7 English Medium</p>
  <h3>Choose your online paper</h3>
  <div id="paperButtons"></div>
  <button onclick="goHome()">🏠 Home</button>
 </div>
</div>

<!-- EXAM PAGE -->
<div id="examPage" class="hidden">
 <div class="panel">
  <h1 id="examTitle"></h1>
  <p>35 Questions • 100 Marks • Six Question Types</p>
  <div id="questions"></div>
  <button onclick="submitExam()">🏆 Submit Paper</button>
  <button onclick="goHome()">🏠 Home</button>
  <div id="result"></div>
  <div id="corrections"></div>
 </div>
</div>

</div>

<footer>🌟 Learn Today, Succeed Tomorrow! 🌟</footer>

<script>

/* SUBJECTS */

const subjects=[
 ["English","📘"],
 ["Science","🔬"],
 ["Mathematics","➗"],
 ["Civic Education","🏛️"],
 ["Geography","🌍"],
 ["Health","❤️"],
 ["ICT","💻"]
];

const subjectArea={
 "English":"Grammar, vocabulary, reading and writing",
 "Science":"Living things, matter, energy and the environment",
 "Mathematics":"Numbers, fractions, geometry and measurement",
 "Civic Education":"Rights, responsibilities and citizenship",
 "Geography":"Maps, weather and the environment",
 "Health":"Healthy habits, safety and wellbeing",
 "ICT":"Computers and information technology"
};

let currentSubject="";
let currentPaper=1;
let currentQuestions=[];
let submitted=false;

const home=document.getElementById("home");
const subjectPage=document.getElementById("subjectPage");
const examPage=document.getElementById("examPage");

function showPage(page){
 home.classList.add("hidden");
 subjectPage.classList.add("hidden");
 examPage.classList.add("hidden");
 page.classList.remove("hidden");
}

function makeHome(){
 const box=document.getElementById("subjects");
 box.innerHTML="";
 subjects.forEach(s=>{
  box.innerHTML+=`
   <div class="card" onclick="openSubject('${s[0]}')">
    <div class="emoji">${s[1]}</div>
    <h2>${s[0]}</h2>
    <p>3 Online Papers</p>
   </div>`;
 });
}

function openSubject(name){
 currentSubject=name;
 document.getElementById("subjectTitle").textContent=
  subjects.find(s=>s[0]===name)[1]+" Grade 7 "+name;
 document.getElementById("paperButtons").innerHTML=
  [1,2,3].map(n=>
   `<button onclick="startPaper(${n})">📝 Paper ${n}</button>`
  ).join("");
 showPage(subjectPage);
}

function goHome(){
 showPage(home);
}

function question(q,options,answer,explanation,type){
 return {q,options,answer,explanation,type};
}

/* SAMPLE QUESTIONS FOR EACH SUBJECT */
const questionBanks={

"English":[
 question("Choose the plural of 'child'.",["Childs","Children","Childes"],"Children","The plural of child is children.","MCQ"),
 question("Choose the past tense of 'go'.",["Goed","Went","Going"],"Went","Went is the past tense of go.","MCQ"),
 question("Choose the correct article: She is ___ honest girl.",["a","an","the"],"an","Use an before a vowel sound.","MCQ"),
 question("Which word is an adjective?",["Beautiful","Quickly","Running"],"Beautiful","Beautiful describes a noun.","MCQ"),
 question("Choose the opposite of ancient.",["Old","Modern","Historic"],"Modern","Modern is the opposite of ancient.","MCQ")
],

"Science":[
 question("Which organ pumps blood around the body?",["Heart","Lungs","Stomach"],"Heart","The heart pumps blood around the body.","MCQ"),
 question("Which planet is known as the Red Planet?",["Earth","Mars","Venus"],"Mars","Mars is commonly called the Red Planet.","MCQ"),
 question("Plants make food mainly in their ___.",["Roots","Leaves","Flowers"],"Leaves","Leaves contain chlorophyll and are the main site of photosynthesis.","MCQ"),
 question("Which state of matter has a fixed shape?",["Solid","Liquid","Gas"],"Solid","A solid has a fixed shape.","MCQ"),
 question("Which gas do humans need for respiration?",["Oxygen","Nitrogen","Carbon dioxide"],"Oxygen","Humans use oxygen for respiration.","MCQ")
],

"Mathematics":[
 question("What is 12 × 8?",["86","96","108"],"96","12 multiplied by 8 equals 96.","MCQ"),
 question("What is 3/4 of 20?",["10","15","18"],"15","20 divided by 4 is 5; 5 multiplied by 3 is 15.","MCQ"),
 question("Which is an even number?",["17","23","36"],"36","36 is divisible by 2.","MCQ"),
 question("How many degrees are in a right angle?",["45°","90°","180°"],"90°","A right angle measures 90 degrees.","MCQ"),
 question("What is the perimeter of a square with sides of 5 cm?",["10 cm","20 cm","25 cm"],"20 cm","Perimeter = 4 × side = 20 cm.","MCQ")
],

"Civic Education":[
 question("What is a responsibility?",["A duty","A toy","A game"],"A duty","A responsibility is something a person should do.","MCQ"),
 question("Which is public property?",["A public library","A private toy","A personal bag"],"A public library","A public library is provided for community use.","MCQ"),
 question("Why should we follow rules?",["To maintain order","To create problems","To waste time"],"To maintain order","Rules help people live and work together safely.","MCQ"),
 question("Which action shows respect?",["Listening politely","Shouting at others","Damaging property"],"Listening politely","Listening politely shows respect.","MCQ"),
 question("Who helps maintain law and order?",["Police officers","Farmers only","Artists only"],"Police officers","Police officers help maintain law and order.","MCQ")
],

"Geography":[
 question("Which is the largest ocean?",["Indian Ocean","Pacific Ocean","Arctic Ocean"],"Pacific Ocean","The Pacific Ocean is the largest ocean.","MCQ"),
 question("What is used to show directions on a map?",["Compass rose","Thermometer","Clock"],"Compass rose","A compass rose shows directions.","MCQ"),
 question("Which is a natural landform?",["Mountain","Road","Building"],"Mountain","A mountain is a natural landform.","MCQ"),
 question("What is the capital of Sri Lanka?",["Kandy","Sri Jayawardenepura Kotte","Matale"],"Sri Jayawardenepura Kotte","Sri Jayawardenepura Kotte is Sri Lanka's legislative capital.","MCQ"),
 question("Which instrument measures rainfall?",["Rain gauge","Barometer","Compass"],"Rain gauge","A rain gauge measures rainfall.","MCQ")
],

"Health":[
 question("Which habit helps keep teeth healthy?",["Brushing twice daily","Eating sweets all day","Never brushing"],"Brushing twice daily","Brushing twice daily helps maintain oral health.","MCQ"),
 question("Which food group helps build the body?",["Protein-rich foods","Only sweets","Soft drinks"],"Protein-rich foods","Protein helps growth and repair.","MCQ"),
 question("Why is clean water important?",["It supports good health","It causes pollution","It replaces sleep"],"It supports good health","Clean water helps the body function properly.","MCQ"),
 question("Which activity supports physical fitness?",["Walking","Sitting all day","Skipping sleep"],"Walking","Walking is a form of physical activity.","MCQ"),
 question("What should you do before eating?",["Wash your hands","Touch dirty surfaces","Avoid cleaning"],"Wash your hands","Handwashing helps prevent the spread of germs.","MCQ")
],

"ICT":[
 question("What does ICT stand for?",["Information and Communication Technology","Internet Computer Text","International Coding Test"],"Information and Communication Technology","ICT means Information and Communication Technology.","MCQ"),
 question("Which device is used to type text?",["Keyboard","Monitor","Speaker"],"Keyboard","A keyboard is an input device used to type.","MCQ"),
 question("Which device displays information?",["Monitor","Mouse","Microphone"],"Monitor","A monitor displays information from a computer.","MCQ"),
 question("Which is an example of an input device?",["Mouse","Printer","Speaker"],"Mouse","A mouse sends input to a computer.","MCQ"),
 question("Which action helps keep an online account safe?",["Use a strong password","Share your password","Use the same easy password everywhere"],"Use a strong password","A strong, private password helps protect an account.","MCQ")
]

};

/* BUILD A 35-QUESTION PAPER FROM 5 SUBJECT QUESTIONS + 30 TYPE VARIATIONS */

function buildQuestions(){
 const base=questionBanks[currentSubject];
 const q=[];
 const paperShift=currentPaper-1;

 // A: Multiple choice – 5
 base.forEach((item,i)=>{
  q.push({...item,type:"Multiple Choice"});
 });

 // B: True or False – 5
 base.forEach((item,i)=>{
  q.push(question(
   "True or False: "+item.q+" Correct answer: "+item.answer,
   ["True","False"],
   "True",
   item.explanation,
   "True or False"
  ));
 });

 // C: Fill in the blanks – 5
 base.forEach((item,i)=>{
  q.push(question(
   "Fill in the blank: "+item.q,
   item.options,
   item.answer,
   item.explanation,
   "Fill in the Blanks"
  ));
 });

 // D: Matching – 10
 const matchPairs={
  "English":[["Noun","Naming word"],["Verb","Action word"],["Adjective","Describing word"],["Library","Place for books"],["Modern","Opposite of ancient"],["Went","Past tense of go"],["Children","Plural of child"],["Glad","Synonym of happy"],["When","Asks about time"],["Beautiful","Correct spelling"]],
  "Science":[["Heart","Pumps blood"],["Mars","Red Planet"],["Leaves","Main food-making part"],["Solid","Fixed shape"],["Oxygen","Used in respiration"],["Roots","Absorb water"],["Sun","Star"],["Water","Liquid"],["Stem","Supports plant"],["Lungs","Help breathing"]],
  "Mathematics":[["12 × 8","96"],["3/4 of 20","15"],["Right angle","90°"],["Square perimeter","4 × side"],["Even number","Divisible by 2"],["1/2","0.5"],["Triangle","3 sides"],["1 metre","100 centimetres"],["1 hour","60 minutes"],["Area","Surface covered"]],
  "Civic Education":[["Responsibility","A duty"],["Public library","Public property"],["Rules","Maintain order"],["Respect","Polite behaviour"],["Police","Maintain law and order"],["Community","People living together"],["Rights","Entitlements"],["Cooperation","Working together"],["Environment","Our surroundings"],["Citizenship","Membership of a country"]],
  "Geography":[["Pacific","Largest ocean"],["Compass rose","Shows directions"],["Mountain","Natural landform"],["Sri Jayawardenepura Kotte","Legislative capital"],["Rain gauge","Measures rainfall"],["River","Flowing water"],["Equator","0° latitude"],["Map key","Explains symbols"],["Weather","Daily atmospheric condition"],["Island","Land surrounded by water"]],
  "Health":[["Brushing teeth","Oral hygiene"],["Protein","Growth and repair"],["Clean water","Supports health"],["Walking","Physical activity"],["Handwashing","Prevents spread of germs"],["Sleep","Restores the body"],["Vegetables","Provide vitamins"],["Exercise","Supports fitness"],["First aid","Immediate basic help"],["Hygiene","Clean habits"]],
  "ICT":[["ICT","Information and Communication Technology"],["Keyboard","Input device"],["Monitor","Output device"],["Mouse","Input device"],["Strong password","Account protection"],["Printer","Output device"],["Speaker","Output device"],["Microphone","Input device"],["Browser","Opens websites"],["File","Stores information"]]
 };
 (matchPairs[currentSubject]||matchPairs["English"]).forEach((pair,i)=>{
  q.push(question("Matching: "+pair[0], [pair[1],"Incorrect match A","Incorrect match B"],pair[1],
  pair[0]+" matches with "+pair[1]+".","Matching"));
 });

 // E: Short answer – 5
 const shortPrompts={
  "English":[["Write the plural of 'book'.","books"],["Write the past tense of 'play'.","played"],["Give an adjective to describe a flower.","beautiful"],["Write the opposite of 'hot'.","cold"],["Make a sentence using 'because'.","because"]],
  "Science":[["Name the organ that pumps blood.","heart"],["Name the process by which plants make food.","photosynthesis"],["Name one state of matter.","solid"],["Which gas do humans need for respiration?","oxygen"],["Name the star closest to Earth.","sun"]],
  "Mathematics":[["What is 7 × 9?","63"],["What is half of 50?","25"],["How many sides does a hexagon have?","6"],["How many centimetres are in one metre?","100"],["What is 100 divided by 4?","25"]],
  "Civic Education":[["Name one public property.","library"],["What is a responsibility?","duty"],["Name one way to show respect.","listen"],["Why do communities need rules?","order"],["Name one public service.","education"]],
  "Geography":[["Name the largest ocean.","pacific"],["What instrument shows directions?","compass"],["Name one natural landform.","mountain"],["What does a rain gauge measure?","rainfall"],["Name the legislative capital of Sri Lanka.","sri jayawardenepura kotte"]],
  "Health":[["Name one healthy habit.","handwashing"],["How often should you brush your teeth each day?","twice"],["Name one protein-rich food.","egg"],["What should you do before eating?","wash hands"],["Name one physical activity.","walking"]],
  "ICT":[["What does ICT stand for?","information and communication technology"],["Name one input device.","keyboard"],["Name one output device.","monitor"],["What is used to protect an online account?","password"],["Name a program used to open websites.","browser"]]
 };
 (shortPrompts[currentSubject]||shortPrompts["English"]).forEach((item,i)=>{
  q.push({q:"Short answer: "+item[0],answer:item[1],type:"Short Answer",explanation:"Suggested answer: "+item[1]+".",options:null});
 });

 // F: Complete the sentences – 5
 const sentencePrompts={
  "English":[["A person, place or thing is called a ___.","noun"],["An action word is called a ___.","verb"],["The opposite of day is ___.","night"],["The past tense of eat is ___.","ate"],["A word that describes a noun is an ___.","adjective"]],
  "Science":[["The heart pumps ___.","blood"],["Plants absorb water through their ___.","roots"],["The Sun is a ___.","star"],["Water freezes to form ___.","ice"],["Humans breathe in ___.","oxygen"]],
  "Mathematics":[["A triangle has ___ sides.","3"],["A right angle is ___ degrees.","90"],["One metre equals ___ centimetres.","100"],["A square has ___ equal sides.","4"],["Half of 18 is ___.","9"]],
  "Civic Education":[["We should respect the rights of ___.","others"],["Public property belongs to the ___.","public"],["Rules help maintain ___.","order"],["Working together is called ___.","cooperation"],["A good citizen follows rules and ___.","responsibilities"]],
  "Geography":[["A map uses symbols to show ___.","features"],["The Pacific is an ___.","ocean"],["A mountain is a natural ___.","landform"],["A compass shows ___.","directions"],["Rainfall is measured using a ___.","rain gauge"]],
  "Health":[["We should wash our hands with soap and ___.","water"],["A balanced diet contains different ___.","nutrients"],["Regular exercise helps keep us ___.","healthy"],["We should brush our teeth ___ a day.","twice"],["Clean water helps prevent ___.","disease"]],
  "ICT":[["A keyboard is an ___ device.","input"],["A monitor is an ___ device.","output"],["A computer stores information in ___.","files"],["A web browser opens ___.","websites"],["Keep your password ___.","private"]]
 };
 (sentencePrompts[currentSubject]||sentencePrompts["English"]).forEach(item=>{
  q.push({q:"Complete the sentence: "+item[0],answer:item[1],type:"Complete the Sentences",options:null,explanation:"The missing word is "+item[1]+"."});
 });

 return q.slice(0,35);
}

function startPaper(number){
 currentPaper=number;
 submitted=false;
 currentQuestions=buildQuestions();
 document.getElementById("examTitle").textContent=
  "📝 Grade 7 "+currentSubject+" – Paper "+number;
 document.getElementById("result").textContent="";
 document.getElementById("corrections").innerHTML="";
 renderQuestions();
 showPage(examPage);
}

function renderQuestions(){
 const area=document.getElementById("questions");
 area.innerHTML="";
 const typeNames=["Multiple Choice","True or False","Fill in the Blanks","Matching","Short Answer","Complete the Sentences"];
 let html="";
 let index=0;
 typeNames.forEach(type=>{
  const list=currentQuestions.filter(q=>q.type===type);
  if(!list.length)return;
  html+=`<h2>${type}</h2>`;
  list.forEach(q=>{
   index++;
   const id="answer"+index;
   q.id=id;
   html+=`<div class="paper-question">
    <strong>${index}. ${q.q}</strong><br>`;
   if(q.options){
    html+=q.options.map(o=>
     `<label class="option"><input type="radio" name="${id}" value="${o}"> ${o}</label>`
    ).join("");
   }else{
    html+=`<input type="text" id="${id}" placeholder="Type your answer" style="padding:10px;width:100%;margin-top:10px">`;
   }
   html+=`</div>`;
  });
 });
 area.innerHTML=html;
}

function getUserAnswer(q){
 if(q.options){
  const checked=document.querySelector(`input[name="${q.id}"]:checked`);
  return checked?checked.value:"";
 }
 return document.getElementById(q.id).value.trim();
}

function submitExam(){
 if(submitted)return;
 submitted=true;
 let score=0;
 let html="<h2>Answer Review</h2>";
 currentQuestions.forEach((q,i)=>{
  const user=getUserAnswer(q);
  let correct;
  if(q.type==="Short Answer"||q.type==="Complete the Sentences"){
   correct=user.toLowerCase()===q.answer.toLowerCase();
  }else{
   correct=user===q.answer;
  }
  if(correct)score+= (q.type==="Matching"?2: q.type==="Short Answer"||q.type==="Complete the Sentences"?2:2);
  html+=`<div class="paper-question ${correct?"correct":"wrong"}">
   <strong>Question ${i+1}: ${correct?"Correct ✓":"Incorrect ✗"}</strong><br>
   Your answer: ${user||"Not answered"}<br>
   Correct answer: <b>${q.answer}</b><br>
   ${q.explanation}
  </div>`;
 });
 // Normalize score to 100 from 35 equally weighted questions.
 score=Math.round(score/70*100);
 document.getElementById("result").textContent="Your Score: "+score+" / 100";
 document.getElementById("corrections").innerHTML=html+
  '<button onclick="goHome()">🏠 Return Home</button>';
 document.querySelectorAll("#questions input").forEach(x=>x.disabled=true);
}

makeHome();

</script>
</body>
</html>
