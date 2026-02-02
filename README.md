<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>ETEA Practice Test</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600&display=swap" rel="stylesheet">
<style>
body{font-family:'Poppins',sans-serif;background:linear-gradient(135deg,#ff9a9e,#fecfef);margin:0}
.container{max-width:900px;margin:20px auto;background:#fff;padding:30px;border-radius:20px;box-shadow:0 10px 30px rgba(0,0,0,.2)}
h2{text-align:center}
.center{text-align:center}
input{width:100%;padding:12px;margin-bottom:15px;border-radius:10px;border:2px solid #ddd}
button{padding:12px;border:none;border-radius:10px;font-size:16px;font-weight:bold;cursor:pointer;margin:5px}
#startBtn{background:#2980b9;color:#fff;width:100%}
#timer{background:#e74c3c;color:#fff;padding:15px;text-align:center;border-radius:10px;margin-bottom:10px}
.stats-bar{display:flex;justify-content:space-between;background:#f8f9fa;padding:10px;border-radius:10px;margin-bottom:15px;font-weight:600;font-size:14px;border:1px solid #eee}
.option{border:2px solid #ddd;padding:15px;border-radius:10px;margin-bottom:12px;cursor:pointer}
.option.correct{background:#2980b9;color:#fff}
.option.wrong{background:#e74c3c;color:#fff}
.buttons{display:flex;justify-content:space-between}
#submitBtn{display:none;background:#27ae60;color:white;width:100%;margin-top:20px}
.subject-box{background:#f4f6f7;padding:12px;border-radius:10px;margin:8px 0}
.review-option.correct{background:#2980b9;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.review-option.wrong{background:#e74c3c;color:#fff;padding:8px;border-radius:8px;margin:4px 0}
.footer{text-align:center;color:#555;margin:20px 0;font-size:14px}
#resName{font-size: 22px; color: #2980b9; font-weight: 600;}
</style>
</head>
<body>

<div class="container center" id="loginDiv">
<h2>ETEA Practice Test</h2>
<input id="studentName" placeholder="Student Name">
<input id="rollNo" placeholder="Roll Number">
<button id="startBtn" onclick="startCountdown()">Start Test</button>
<div id="countdown" style="font-size:28px;color:red"></div>
</div>

<div class="container" id="quizDiv" style="display:none">
<div id="timer">Time Left: 40:00</div>
<div class="stats-bar">
    <span id="qCounter">Question: 1/100</span>
    <span id="attemptedCounter">Attempted: 0/100</span>
</div>
<div id="questionBox"></div>
<div id="optionsBox"></div>
<div class="buttons">
<button onclick="prevQ()">Previous</button>
<button onclick="skipQ()">Skip</button>
<button onclick="nextQ()" id="nextBtn" disabled>Next</button>
</div>
<button id="submitBtn" onclick="submitTest()">Submit Test</button>
</div>

<div class="container center" id="resultDiv" style="display:none">
<h2 id="resStatus"></h2>
<p id="resName"></p>
<p id="resScore"></p>
<p id="resTime"></p>
<h3>Subject-Wise Result</h3>
<div id="subjectResult"></div>
<button onclick="showReview()">Review Test</button>
<button onclick="location.reload()">Restart</button>
</div>

<div class="container" id="reviewDiv" style="display:none">
<h2 class="center">Test Review</h2>
<div id="reviewContent"></div>
<button onclick="backToResult()">Back to Result</button>
</div>

<div class="footer">
Created by <b>MUHAMMAD FAIZAN NAWAB</b>
</div>

<script>
 const questions = [
  

    // Mathematics
    {subject:"Math", q:"Smallest number exactly divisible by 8, 12, and 16:", o:["32","48","64","96"], c:1},
    {subject:"Math", q:"The sum of the smallest odd prime and the smallest even prime is:", o:["3","4","5","6"], c:2},
    {subject:"Math", q:"Simplify: (-20) + (-10) - (-15):", o:["-15","-45","5","-5"], c:0},
    {subject:"Math", q:"If set X = {1, 2, 3} and Y = {3, 4, 5}, find X ∪ Y:", o:["{3}","{1, 2, 3, 4, 5}","{1, 2, 4, 5}","∅"], c:1},
    {subject:"Math", q:"In a ratio a:b, if both are multiplied by 3, the ratio remains:", o:["3a:b","a:3b","unchanged","9a:9b"], c:2},
    {subject:"Math", q:"Solve for y: 4(y + 3) = 28:", o:["4","7","10","1"], c:0},
    {subject:"Math", q:"Price decreased from 50 to 40. Find the percentage decrease:", o:["10%","20%","25%","15%"], c:1},
    {subject:"Math", q:"Two angles are complementary. If one is 40°, the other is:", o:["50°","140°","90°","60°"], c:0},
    {subject:"Math", q:"The area of a rectangle is 50cm². If length is 10cm, width is:", o:["5cm","10cm","40cm","15cm"], c:0},
    {subject:"Math", q:"Which property is shown by a + b = b + a?", o:["Associative","Distributive","Commutative","Identity"], c:2},
    {subject:"Math", q:"If the perimeter of a square is 40cm, its side length is:", o:["5cm","10cm","20cm","160cm"], c:1},
    {subject:"Math", q:"A train travels 300km in 5 hours. Its speed in km/h is:", o:["50","60","70","1500"], c:1},
    {subject:"Math", q:"15% of 200 is:", o:["15","30","45","60"], c:1},
    {subject:"Math", q:"The degree of the polynomial 4x³ + 2x² - 7 is:", o:["1","2","3","4"], c:2},
    {subject:"Math", q:"If a = 3 and b = 2, find the value of a² - b²:", o:["1","5","13","7"], c:1},
    {subject:"Math", q:"The LCM of two prime numbers is always:", o:["1","Their sum","Their product","Their difference"], c:2},
    {subject:"Math", q:"A set with no elements is called:", o:["Universal set","Finite set","Null set","Power set"], c:2},
    {subject:"Math", q:"What is the value of (5 + 2)⁰?", o:["7","0","1","14"], c:2},
    {subject:"Math", q:"Find the value of m if 2:5 = m:20:", o:["4","8","10","12"], c:1},
    {subject:"Math", q:"An angle of 90° is called:", o:["Acute","Right","Obtuse","Straight"], c:1},
    {subject:"Math", q:"Find the HCF of 18 and 24:", o:["3","6","12","72"], c:1},
    {subject:"Math", q:"If A = {a, b}, the number of subsets is:", o:["2","3","4","5"], c:2},
    {subject:"Math", q:"How many vertices does a triangle have?", o:["1","2","3","4"], c:2},
    {subject:"Math", q:"Quadrilateral with four equal sides and 90° angles is a:", o:["Rhombus","Rectangle","Square","Trapezium"], c:2},
    {subject:"Math", q:"Convert 3/4 into percentage:", o:["25%","50%","75%","80%"], c:2},
    {subject:"Math", q:"If 2x - 5 = 11, then x is:", o:["3","8","6","16"], c:1},
    {subject:"Math", q:"The ratio of 500 meters to 1 kilometer is:", o:["1:2","2:1","5:1","1:5"], c:0},
    {subject:"Math", q:"Volume of a cuboid with dimensions 2cm, 3cm, and 4cm is:", o:["9cm³","24cm³","12cm³","20cm³"], c:1},
    {subject:"Math", q:"The successor of -1 is:", o:["-2","0","1","2"], c:1},
    {subject:"Math", q:"The square root of 169 is:", o:["11","12","13","14"], c:2},

    // Science
    {subject:"Science", q:"Which organelle is known as the 'brain' of the cell?", o:["Mitochondria","Nucleus","Ribosome","Vacuole"], c:1},
    {subject:"Science", q:"The process of breaking down food into simpler forms is:", o:["Respiration","Circulation","Digestion","Excretion"], c:2},
    {subject:"Science", q:"Which gas do plants release during photosynthesis?", o:["Carbon dioxide","Nitrogen","Oxygen","Hydrogen"], c:2},
    {subject:"Science", q:"Bile is produced in the:", o:["Stomach","Liver","Pancreas","Gallbladder"], c:1},
    {subject:"Science", q:"Transfer of pollen from anther to stigma is:", o:["Fertilization","Germination","Pollination","Transpiration"], c:2},
    {subject:"Science", q:"Lack of iron in the diet causes:", o:["Scurvy","Rickets","Anemia","Goiter"], c:2},
    {subject:"Science", q:"Yeast reproduces by which asexual method?", o:["Binary fission","Budding","Spore formation","Fragmentation"], c:1},
    {subject:"Science", q:"Which part of the blood carries oxygen?", o:["White Cells","Platelets","Red Blood Cells","Plasma"], c:2},
    {subject:"Science", q:"Stretching a rubber band stores which type of energy?", o:["Kinetic","Chemical","Elastic Potential","Thermal"], c:2},
    {subject:"Science", q:"The structural and functional unit of life is:", o:["Tissue","Organ","Cell","System"], c:2},
    {subject:"Science", q:"Fungi cell walls are made of:", o:["Cellulose","Chitin","Protein","Sugar"], c:1},
    {subject:"Science", q:"In which part of the leaf does most photosynthesis occur?", o:["Epidermis","Mesophyll","Veins","Stomata"], c:1},
    {subject:"Science", q:"Which instrument is used to measure body temperature?", o:["Barometer","Thermometer","Ammeter","Speedometer"], c:1},
    {subject:"Science", q:"Animals that eat only plants are called:", o:["Carnivores","Herbivores","Omnivores","Decomposers"], c:1},
    {subject:"Science", q:"The acid present in the human stomach is:", o:["Sulfuric Acid","Nitric Acid","Hydrochloric Acid","Acetic Acid"], c:2},
    {subject:"Science", q:"Which chamber of the heart receives deoxygenated blood from the body?", o:["Right Atrium","Left Atrium","Right Ventricle","Left Ventricle"], c:0},
    {subject:"Science", q:"Evaporation of water from plants creates a pull called:", o:["Gravity","Transpiration pull","Pressure","Friction"], c:1},
    {subject:"Science", q:"Night blindness is caused by the deficiency of:", o:["Vitamin A","Vitamin B","Vitamin C","Vitamin D"], c:0},
    {subject:"Science", q:"Iodine solution is used to test the presence of:", o:["Proteins","Fats","Starch","Vitamins"], c:2},
    {subject:"Science", q:"Bacteria are examples of:", o:["Multicellular","Unicellular","Non-living","Plants"], c:1},
    {subject:"Science", q:"The pipe connecting the throat to the lungs is:", o:["Esophagus","Trachea","Aorta","Vein"], c:1},
    {subject:"Science", q:"Calcium and Phosphorus are important for:", o:["Blood clotting","Brain health","Strong bones/teeth","Vision"], c:2},
    {subject:"Science", q:"Most of the chemical digestion takes place in the:", o:["Mouth","Large Intestine","Small Intestine","Esophagus"], c:2},
    {subject:"Science", q:"The physical appearance of an organism is its:", o:["Genotype","Phenotype","DNA","Chromosome"], c:1},
    {subject:"Science", q:"Xylem tissues in plants are responsible for transporting:", o:["Food","Water","Oxygen","Hormones"], c:1},
    {subject:"Science", q:"Roughage (fiber) in our diet helps in:", o:["Growth","Energy","Preventing constipation","Fighting germs"], c:2},
    {subject:"Science", q:"The male part of the flower is the:", o:["Carpel","Stamen","Petal","Sepal"], c:1},
    {subject:"Science", q:"Digestion of protein begins in the:", o:["Mouth","Stomach","Small Intestine","Pancreas"], c:1},
    {subject:"Science", q:"Smallest blood vessels where gas exchange occurs are:", o:["Arteries","Veins","Capillaries","Nerves"], c:2},
    {subject:"Science", q:"The movement of air in and out of the lungs is:", o:["Respiration","Breathing","Oxidation","Combustion"], c:1},

    // English
    {subject:"English", q:"Identify the Abstract Noun:", o:["Teacher","Bravery","Water","Book"], c:1},
    {subject:"English", q:"'Ali and I are friends. _ play together.'", o:["They","We","Us","Them"], c:1},
    {subject:"English", q:"Which word has a silent 'w'?", o:["Watch","Write","Window","Water"], c:1},
    {subject:"English", q:"Select the correctly punctuated sentence:", o:["where are you going","Where are you going?","Where are you going.","where are you going!"], c:1},
    {subject:"English", q:"'This is a bigger house.' 'Bigger' is which degree?", o:["Positive","Comparative","Superlative","None"], c:1},
    {subject:"English", q:"What is the antonym of 'Modern'?", o:["New","Ancient","Recent","Fresh"], c:1},
    {subject:"English", q:"Which is a Proper Noun?", o:["River","Mountain","Indus","City"], c:2},
    {subject:"English", q:"'She has been waiting _ two hours.'", o:["Since","From","For","By"], c:2},
    {subject:"English", q:"Identify the Collective Noun:", o:["Army","Gold","Happiness","Pen"], c:0},
    {subject:"English", q:"'I _ to school every day.'", o:["Go","Went","Going","Gone"], c:0},
    {subject:"English", q:"Choose the correct article: 'He is _ honest man.'", o:["A","An","The","No article"], c:1},
    {subject:"English", q:"Plural of 'Mouse' is:", o:["Mouses","Mices","Mice","Mouse"], c:2},
    {subject:"English", q:"'He runs fast.' The word 'fast' is an:", o:["Noun","Adjective","Adverb","Verb"], c:2},
    {subject:"English", q:"Feminine of 'Horse' is:", o:["Mare","Cow","Doe","Bitch"], c:0},
    {subject:"English", q:"Choose the correct spelling:", o:["Tommorrow","Tomorrow","Tommorow","Tommoro"], c:1},
    {subject:"English", q:"Identify the preposition: 'The cat jumped over the wall.'", o:["Jumped","Over","Wall","Cat"], c:1},
    {subject:"English", q:"Synonym of 'Large' is:", o:["Tiny","Huge","Small","Narrow"], c:1},
    {subject:"English", q:"'What is your name?' is which type of sentence?", o:["Assertive","Imperative","Interrogative","Exclamatory"], c:2},
    {subject:"English", q:"The possessive pronoun for 'He' is:", o:["Him","His","He's","Himself"], c:1},
    {subject:"English", q:"'Wait here _ I come back.'", o:["Until","But","Because","Or"], c:0},

    // Urdu
    {subject:"Urdu", q:"اسم معرفہ کی نشاندہی کریں:", o:["دریا","پہاڑ","کے ٹو","شہر"], c:2},
    {subject:"Urdu", q:"'بچہ روتا ہے' میں فعل کی پہچان کریں:", o:["بچہ","روتا","ہے","نے"], c:1},
    {subject:"Urdu", q:"'اقرار' کا متضاد کیا ہے؟", o:["اظہار","انکار","اقرار","پیار"], c:1},
    {subject:"Urdu", q:"محاورہ 'باغ باغ ہونا' کا کیا مطلب ہے؟", o:["پودے لگانا","بہت خوش ہونا","سیر کرنا","مالی بننا"], c:1},
    {subject:"Urdu", q:"'خورشید' اور 'سورج' آپس میں کیا ہیں؟", o:["متضاد","مترادف","مذکر مؤنث","واحد جمع"], c:1},
    {subject:"Urdu", q:"'بانگِ درا' کس کی تصنیف ہے؟", o:["غالب","علامہ اقبال","فیض","حالی"], c:1},
    {subject:"Urdu", q:"مؤنث لفظ کی نشاندہی کریں:", o:["قلم","کتاب","سکول","باغ"], c:1},
    {subject:"Urdu", q:"'خبر' کی جمع کیا ہے؟", o:["خبریں","اخبارات","خبروں","اخبار"], c:3},
    {subject:"Urdu", q:"'تندرستی ہزار نعمت ہے' میں تندرستی کیا ہے؟", o:["اسم نکرہ","اسم صفت","اسم معرفہ","فعل"], c:2},
    {subject:"Urdu", q:"'محنت' کا ہم قافیہ لفظ تلاش کریں:", o:["عظمت","کام","صلہ","نام"], c:0},

    // Islamiyat
    {subject:"Islamiyat", q:"اسلام کی پہلی جنگ کون سی تھی؟", o:["احد","بدر","خندق","خیبر"], c:1},
    {subject:"Islamiyat", q:"قرآن مجید میں کل کتنی سورتیں ہیں؟", o:["110","114","120","100"], c:1},
    {subject:"Islamiyat", q:"اللہ کے پہلے نبی کون تھے؟", o:["حضرت ابراہیمؑ","حضرت نوحؑ","حضرت آدمؑ","حضرت موسیٰؑ"], c:2},
    {subject:"Islamiyat", q:"'سیف اللہ' کس کا لقب ہے؟", o:["حضرت علیؓ","حضرت خالد بن ولیدؓ","حضرت عمرؓ","حضرت حمزہؓ"], c:1},
    {subject:"Islamiyat", q:"قرآن میں نماز کا ذکر کتنی بار آیا ہے؟", o:["500","700","300","100"], c:1},
    {subject:"Islamiyat", q:"زکوٰۃ اسلام کا کون سا رکن ہے؟", o:["دوسرا","تیسرا","چوتھا","پانچواں"], c:1},
    {subject:"Islamiyat", q:"بارش برسانے والے فرشتے کا نام کیا ہے؟", o:["حضرت جبرائیلؑ","حضرت میکائیلؑ","حضرت اسرافیلؑ","حضرت عزرائیلؑ"], c:1},
    {subject:"Islamiyat", q:"مسلمانوں کی پہلی ہجرت کس طرف تھی؟", o:["مدینہ","حبشہ","طائف","مصر"], c:1},
    {subject:"Islamiyat", q:"نبوت کے بعد آپﷺ مکہ میں کتنے سال رہے؟", o:["10","13","23","40"], c:1},
    {subject:"Islamiyat", q:"حضرت محمدﷺ کا تعلق کس قبیلے سے تھا؟", o:["بنو ہاشم","بنو امیہ","بکر","تیم"], c:0},

 
]

let answers=new Array(questions.length).fill(null);
let index=0,time=2400,startTime,timerInt;

function startCountdown(){
if(!studentName.value||!rollNo.value){alert("Fill all fields");return;}
let c=5;countdown.innerText=c;
let i=setInterval(()=>{
c--;countdown.innerText=c;
if(c===0){
clearInterval(i);
loginDiv.style.display="none";
quizDiv.style.display="block";
startTime=Date.now();
loadQ();startTimer();
}},1000);
}

function updateStats() {
    let attempted = answers.filter(a => a !== null).length;
    document.getElementById("qCounter").innerText = `Question: ${index + 1}/${questions.length}`;
    document.getElementById("attemptedCounter").innerText = `Attempted: ${attempted}/${questions.length}`;
}

function loadQ(){
let q=questions[index];
questionBox.innerHTML=`<h3>${index+1}. (${q.subject}) ${q.q}</h3>`;
optionsBox.innerHTML="";
q.o.forEach((op,i)=>{
let d=document.createElement("div");
d.className="option";
if(answers[index] === i) d.classList.add(i===q.c?"correct":"wrong");
d.innerText=op;
d.onclick=()=>{
if(answers[index]!=null)return;
answers[index]=i;
d.classList.add(i===q.c?"correct":"wrong");
nextBtn.disabled=false;
updateStats();
};
optionsBox.appendChild(d);
});
nextBtn.disabled=answers[index]==null;
submitBtn.style.display=index===questions.length-1?"block":"none";
updateStats();
}

function nextQ(){if(index<questions.length-1){index++;loadQ();}}
function prevQ(){if(index>0){index--;loadQ();}}
function skipQ(){index=(index+1)%questions.length;loadQ();}

function startTimer(){
timerInt=setInterval(()=>{
let m=Math.floor(time/60),s=time%60;
timer.innerText=`Time Left: ${m}:${s<10?'0':''}${s}`;
if(time--<=0){clearInterval(timerInt);submitTest();}
},1000);
}

function submitTest(){
clearInterval(timerInt);
quizDiv.style.display="none";
resultDiv.style.display="block";
let score=0,subjectStats={};
questions.forEach((q,i)=>{
if(!subjectStats[q.subject]) subjectStats[q.subject]={total:0,correct:0};
subjectStats[q.subject].total++;
if(answers[i]===q.c){score++;subjectStats[q.subject].correct++;}
});
let pct=Math.round(score/questions.length*100);
resStatus.innerText=pct>=40?"PASSED":"FAILED";
resName.innerText=`Student Name: ${studentName.value}`;
resScore.innerText=`Overall Score: ${score}/${questions.length} (${pct}%)`;
resTime.innerText=`Time Taken: ${Math.floor((Date.now()-startTime)/60000)} minutes`;
subjectResult.innerHTML="";
for(let s in subjectStats){
let x=subjectStats[s];
subjectResult.innerHTML+=`<div class="subject-box"><b>${s}</b>: ${x.correct}/${x.total} → ${Math.round(x.correct/x.total*100)}%</div>`;
}
}

function showReview(){
resultDiv.style.display="none";
reviewDiv.style.display="block";
reviewContent.innerHTML="";
questions.forEach((q,i)=>{
let html=`<h4>${i+1}. ${q.q}</h4>`;
q.o.forEach((op,idx)=>{
let cls="review-option";
if(idx===q.c) cls+=" correct";
else if(answers[i]===idx) cls+=" wrong";
html+=`<div class="${cls}">${op}</div>`;
});
reviewContent.innerHTML+=html;
});
}

function backToResult(){
reviewDiv.style.display="none";
resultDiv.style.display="block";
}
</script>
</body>
</html># ETEA31ONLINETESTFAIZAN
