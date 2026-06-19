<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>أكاديمية SCORE - نظام الحضور</title>
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">
<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Cairo', sans-serif;
}

:root{
--primary:#1e40af;
--secondary:#3b82f6;
--accent:#60a5fa;
--dark:#1e3a8a;
--light:#eff6ff;
--white:#ffffff;
--gray:#e5e7eb;
}

body{
background:linear-gradient(135deg, #eff6ff 0%, #dbeafe 100%);
min-height:100vh;
display:flex;
}

.sidebar{
width:280px;
background:linear-gradient(180deg, var(--dark) 0%, var(--primary) 100%);
color:var(--white);
padding:25px 20px;
position:fixed;
height:100vh;
right:0;
top:0;
box-shadow:-5px 0 20px rgba(0,0,0,0.1);
z-index:1000;
transition:transform 0.3s ease;
}

.logo-section{
text-align:center;
padding-bottom:30px;
border-bottom:1px solid rgba(255,255,255,0.2);
margin-bottom:25px;
}

.logo-section img{
width:80px;
height:80px;
border-radius:50%;
margin-bottom:15px;
border:3px solid var(--accent);
object-fit:cover;
}

.logo-section h1{
font-size:24px;
font-weight:700;
color:var(--accent);
}

.logo-section p{
font-size:12px;
color:rgba(255,255,255,0.7);
margin-top:5px;
}

.nav-menu{
list-style:none;
}

.nav-item{
margin-bottom:10px;
}

.nav-link{
display:flex;
align-items:center;
gap:12px;
padding:15px 20px;
color:rgba(255,255,255,0.8);
text-decoration:none;
border-radius:12px;
transition:all 0.3s ease;
cursor:pointer;
font-size:15px;
font-weight:600;
}

.nav-link:hover,
.nav-link.active{
background:rgba(96, 165, 250, 0.2);
color:var(--white);
transform:translateX(-5px);
}

.nav-link i{
font-size:20px;
}

.admin-section{
margin-top:30px;
padding-top:20px;
border-top:1px solid rgba(255,255,255,0.2);
}

.admin-btn{
width:100%;
padding:12px;
background:rgba(255,255,255,0.1);
border:2px solid var(--accent);
color:var(--white);
border-radius:10px;
cursor:pointer;
font-size:14px;
font-weight:600;
transition:all 0.3s ease;
}

.admin-btn:hover{
background:var(--accent);
color:var(--dark);
}

.main-content{
margin-right:280px;
flex:1;
padding:30px;
}

.page{
display:none;
animation:fadeIn 0.5s ease;
}

.page.active{
display:block;
}

@keyframes fadeIn{
from{opacity:0;transform:translateY(10px);}
to{opacity:1;transform:translateY(0);}
}

.page-header{
background:var(--white);
padding:25px 30px;
border-radius:20px;
margin-bottom:25px;
box-shadow:0 4px 15px rgba(0,0,0,0.05);
display:flex;
align-items:center;
justify-content:space-between;
}

.page-header h2{
font-size:28px;
color:var(--dark);
font-weight:700;
}

.page-header .icon{
font-size:40px;
}

.card{
background:var(--white);
padding:25px 30px;
border-radius:20px;
margin-bottom:25px;
box-shadow:0 4px 15px rgba(0,0,0,0.05);
}

.card h3{
font-size:20px;
color:var(--dark);
margin-bottom:20px;
font-weight:600;
}

.form-group{
display:flex;
gap:15px;
flex-wrap:wrap;
margin-bottom:20px;
}

.form-group input,
.form-group select{
flex:1;
min-width:200px;
padding:12px 18px;
border:2px solid var(--gray);
border-radius:12px;
font-size:15px;
transition:all 0.3s ease;
}

.form-group input:focus,
.form-group select:focus{
outline:none;
border-color:var(--secondary);
box-shadow:0 0 0 3px rgba(45, 143, 45, 0.1);
}

button{
padding:12px 25px;
border:none;
border-radius:12px;
cursor:pointer;
color:white;
font-size:15px;
font-weight:600;
transition:all 0.3s ease;
display:inline-flex;
align-items:center;
gap:8px;
}

button:hover{
transform:translateY(-2px);
box-shadow:0 4px 12px rgba(0,0,0,0.15);
}

.add{background:linear-gradient(135deg, var(--secondary), var(--primary));}
.delete{background:linear-gradient(135deg, #ef4444, #dc2626);}
.reset{background:linear-gradient(135deg, #f59e0b, #d97706);}
.export{background:linear-gradient(135deg, #8b5cf6, #7c3aed);}

.stats{
display:flex;
gap:20px;
margin-top:25px;
flex-wrap:wrap;
}

.stat-card{
flex:1;
min-width:180px;
background:linear-gradient(135deg, var(--light), #dbeafe);
padding:25px;
border-radius:16px;
text-align:center;
box-shadow:0 4px 12px rgba(0,0,0,0.05);
}

.stat-card .number{
font-size:36px;
font-weight:700;
color:var(--primary);
margin-bottom:5px;
}

.stat-card .label{
font-size:14px;
color:var(--dark);
font-weight:600;
}

table{
width:100%;
border-collapse:collapse;
margin-top:15px;
}

th{
background:linear-gradient(135deg, var(--light), #dbeafe);
color:var(--dark);
padding:15px;
text-align:center;
font-weight:600;
font-size:14px;
}

td{
padding:15px;
border-bottom:1px solid var(--gray);
text-align:center;
font-size:14px;
}

tr:hover td{
background:var(--light);
}

.present{
background:linear-gradient(135deg, var(--secondary), var(--primary));
color:white;
padding:8px 16px;
border-radius:20px;
display:inline-block;
font-size:13px;
font-weight:600;
}

.absent{
background:linear-gradient(135deg, #ef4444, #dc2626);
color:white;
padding:8px 16px;
border-radius:20px;
display:inline-block;
font-size:13px;
font-weight:600;
}

.checkbox{
width:22px;
height:22px;
accent-color:var(--secondary);
}

.group-card{
background:var(--light);
padding:20px;
border-radius:16px;
margin-bottom:20px;
border:2px solid var(--gray);
}

.group-card h3{
color:var(--primary);
margin-bottom:15px;
font-size:18px;
}

.qr-section{
background:linear-gradient(135deg, var(--light), #dbeafe);
padding:30px;
border-radius:20px;
text-align:center;
margin-bottom:25px;
}

.qr-section input{
width:100%;
max-width:400px;
padding:15px 20px;
border:2px solid var(--gray);
border-radius:12px;
font-size:16px;
text-align:center;
}

@media(max-width:768px){
.sidebar{
width:100%;
height:auto;
position:relative;
transform:none;
}
.main-content{
margin-right:0;
padding:15px;
}
.page-header{
flex-direction:column;
text-align:center;
gap:15px;
}
.form-group{
flex-direction:column;
}
.form-group input,
.form-group select{
width:100%;
}
.stats{
flex-direction:column;
}
.stat-card{
width:100%;
}
.card{
padding:20px;
}
table{
font-size:12px;
}
th,td{
padding:10px 5px;
}
.nav-menu{
display:flex;
flex-wrap:wrap;
gap:10px;
}
.nav-item{
flex:1;
min-width:120px;
}
.nav-link{
flex-direction:column;
text-align:center;
gap:5px;
padding:10px;
font-size:13px;
}
.nav-link span{
font-size:24px;
}
.admin-section{
margin-top:15px;
}
}
</style>
</head>

<body>

<!-- Sidebar -->
<aside class="sidebar">
<div class="logo-section">
<img src="logo.png" alt="SCORE Logo">
<h1>أكاديمية SCORE</h1>
<p>نظام إدارة الحضور</p>
</div>

<ul class="nav-menu">
<li class="nav-item">
<a class="nav-link active" onclick="showPage('dashboard')">
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
<span>لوحة التحكم</span>
</a>
</li>
<li class="nav-item">
<a class="nav-link" onclick="showPage('supervisors')">
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
<span>المشرفين</span>
</a>
</li>
<li class="nav-item">
<a class="nav-link" onclick="showPage('children')">
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><path d="M12 6v6l4 2"></path></svg>
<span>الأطفال</span>
</a>
</li>
<li class="nav-item">
<a class="nav-link" onclick="showPage('groups')">
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
<span>المجموعات</span>
</a>
</li>
</ul>

<div class="admin-section">
<button class="admin-btn" id="adminBtn" onclick="toggleAdminMode()">
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><rect x="3" y="11" width="18" height="11" rx="2" ry="2"></rect><path d="M7 11V7a5 5 0 0 1 10 0v4"></path></svg>
التسجيل كأدمن
</button>
</div>
</aside>

<!-- Main Content -->
<main class="main-content">

<!-- Dashboard Page -->
<div id="dashboard" class="page active">
<div class="page-header">
<h2>� لوحة التحكم</h2>
<svg class="icon" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="3" width="7" height="7"></rect><rect x="14" y="3" width="7" height="7"></rect><rect x="14" y="14" width="7" height="7"></rect><rect x="3" y="14" width="7" height="7"></rect></svg>
</div>

<div class="qr-section">
<h3><svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><path d="M23 19a2 2 0 0 1-2 2H3a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h4l2-3h6l2 3h4a2 2 0 0 1 2 2z"></path><circle cx="12" cy="13" r="4"></circle></svg>تسجيل الحضور</h3>
<input type="text" id="qrInput" placeholder="امسح QR أو أدخل رقم الطفل">
</div>

<div class="stats">
<div class="stat-card">
<div class="number" id="presentCount">0</div>
<div class="label">الحضور</div>
</div>
<div class="stat-card">
<div class="number" id="absentCount">0</div>
<div class="label">الغياب</div>
</div>
<div class="stat-card">
<div class="number" id="totalCount">0</div>
<div class="label">الإجمالي</div>
</div>
</div>
</div>

<!-- Supervisors Page -->
<div id="supervisors" class="page">
<div class="page-header">
<h2>المشرفين</h2>
<svg class="icon" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
</div>

<div class="card">
<h3>إضافة مشرف جديد</h3>
<div class="form-group">
<input type="text" id="supervisorName" placeholder="اسم المشرف">
<button class="add" onclick="addSupervisor()">
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
إضافة مشرف
</button>
</div>
</div>

<div class="card">
<h3>قائمة المشرفين</h3>
<table>
<thead>
<tr>
<th>المشرف</th>
<th>عدد الأطفال</th>
<th>حذف</th>
</tr>
</thead>
<tbody id="supervisorsTable"></tbody>
</table>
</div>
</div>

<!-- Children Page -->
<div id="children" class="page">
<div class="page-header">
<h2>الأطفال</h2>
<svg class="icon" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"></circle><path d="M12 6v6l4 2"></path></svg>
</div>

<div class="card">
<h3>إضافة طفل جديد</h3>
<div class="form-group">
<input type="text" id="childName" placeholder="اسم الطفل">
<select id="childSupervisor">
<option value="">اختر المشرف</option>
</select>
<input type="date" id="startDate">
<button class="add" onclick="addChild()">
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><line x1="12" y1="5" x2="12" y2="19"></line><line x1="5" y1="12" x2="19" y2="12"></line></svg>
إضافة طفل
</button>
</div>
</div>

<div class="card">
<div style="margin-bottom:20px;">
<button class="reset" onclick="resetAttendance()">
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><polyline points="23 4 23 10 17 10"></polyline><polyline points="1 20 1 14 7 14"></polyline><path d="M3.51 9a9 9 0 0 1 14.85-3.36L23 10M1 14l4.64 4.36A9 9 0 0 0 20.49 15"></path></svg>
إعادة الحضور
</button>
<button class="export" onclick="exportCSV()">
<svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="margin-left:8px"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"></path><polyline points="7 10 12 15 17 10"></polyline><line x1="12" y1="15" x2="12" y2="3"></line></svg>
تصدير Excel
</button>
</div>
<h3>قائمة الأطفال</h3>
<table>
<thead>
<tr>
<th>رقم الطفل</th>
<th>اسم الطفل</th>
<th>المشرف</th>
<th>تاريخ البدء</th>
<th>تاريخ التفقد</th>
<th>الحالة</th>
<th>التفقد</th>
<th>حذف</th>
</tr>
</thead>
<tbody id="tableBody"></tbody>
</table>
</div>
</div>

<!-- Groups Page -->
<div id="groups" class="page">
<div class="page-header">
<h2>المجموعات</h2>
<svg class="icon" width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2"></path><circle cx="9" cy="7" r="4"></circle><path d="M23 21v-2a4 4 0 0 0-3-3.87"></path><path d="M16 3.13a4 4 0 0 1 0 7.75"></path></svg>
</div>

<div id="groupsContainer"></div>
</div>

</main>

<script>
// Page Navigation
function showPage(pageId) {
// Hide all pages
document.querySelectorAll('.page').forEach(page => {
page.classList.remove('active');
});
// Show selected page
document.getElementById(pageId).classList.add('active');
// Update nav links
document.querySelectorAll('.nav-link').forEach(link => {
link.classList.remove('active');
});
event.target.closest('.nav-link').classList.add('active');
}
</script>

<script type="module">

import { initializeApp }
from "https://www.gstatic.com/firebasejs/11.8.1/firebase-app.js";

import {
getFirestore,
collection,
addDoc,
deleteDoc,
doc,
onSnapshot,
updateDoc,
getDocs
}
from "https://www.gstatic.com/firebasejs/11.8.1/firebase-firestore.js";

const firebaseConfig = {

apiKey: "AIzaSyA-aV5qGj27Vj0e8dtrXkz9Ckt5Tp-reyY",

authDomain:
"score-summer-camp.firebaseapp.com",

projectId:
"score-summer-camp",

storageBucket:
"score-summer-camp.firebasestorage.app",

messagingSenderId:
"836117896870",

appId:
"1:836117896870:web:730a652b3a8ab015105e67"

};

const app =
initializeApp(firebaseConfig);

const db =
getFirestore(app);

const childrenCollection =
collection(
db,
"children"
);

const supervisorsCollection =
collection(
db,
"supervisors"
);

let children = [];
let supervisors = [];
let adminMode = false;

window.toggleAdminMode = function(){

if(adminMode){

adminMode = false;

document.getElementById("adminBtn").innerText = "🔒 التسجيل كأدمن";

alert("تم قفل الإدارة");

return;

}

adminMode = true;

document.getElementById("adminBtn").innerText = "🔓 الإدارة مفتوحة";

alert("تم فتح الإدارة");

};

function requireAdmin(){

if(adminMode)
return true;

alert(
"الإدارة مقفلة"
);

return false;

}
async function createDefaultGroups(){

const snapshot =
await getDocs(
supervisorsCollection
);

if(snapshot.empty){
for(const group of groups){

await addDoc(
supervisorsCollection,
{
name:group
}
);

}

}

}

createDefaultGroups();
window.addSupervisor =
async function(){

if(!requireAdmin()) return;

const name =
document
.getElementById(
"supervisorName"
)
.value
.trim();

if(!name){

alert("أدخل اسم المشرف");

return;

}

await addDoc(
supervisorsCollection,
{
name:name
}
);

document
.getElementById(
"supervisorName"
)
.value="";


};

window.deleteSupervisor =
async function(docId){

if(
!confirm(
"حذف المشرف؟"
)
)
return;

await deleteDoc(
doc(
db,
"supervisors",
docId
)
);

};window.editSupervisor =
async function(docId){

const current =
supervisors.find(
s=>s.docId===docId
);

const newName =
prompt(
"اسم المشرف الجديد",
current.name
);

if(
!newName ||
newName.trim()===""
)
return;

await updateDoc(
doc(
db,
"supervisors",
docId
),
{
name:newName.trim()
}
);

};

function fillSupervisorSelect(){

let html =
'<option value="">اختر المشرف</option>';

supervisors.forEach(s=>{

html += `

<option value="${s.docId}">
${s.name}
</option>

`;

});

document
.getElementById(
"childSupervisor"
)
.innerHTML = html;

}

function renderSupervisors(){


let html = "";

supervisors.forEach(s=>{

const count =
children.filter(
c =>
c.supervisorId === s.docId
).length;

html += `

<tr>

<td>
${s.name}
</td>

<td>
${count}
</td>

<td>

<button
class="delete"
onclick="deleteSupervisor('${s.docId}')">

حذف

</button>

</td>

</tr>

`;

});

document
.getElementById(
"supervisorsTable"
)
.innerHTML = html;

}function getNextChildId()
{

if(children.length === 0)
return 1;

const maxId =
Math.max(
...children.map(
c => Number(c.childId || 0)
)
);

return maxId + 1;

}
  window.addChild =
async function(){

const name =
document
.getElementById(
"childName"
)
.value
.trim();


const supervisorId =
document
.getElementById(
"childSupervisor"
)
.value;

const startDate =
document
.getElementById(
"startDate"
)
.value;

if(
!name ||
!supervisorId
){

alert(
"أكمل البيانات"
);

return;

}

await addDoc(
childrenCollection,
{
name:name,
childId:getNextChildId(),
supervisorId:supervisorId,
startDate:startDate,
present:false
}
);

document
.getElementById(
"childName"
)
.value="";


};

window.deleteChild =
async function(docId){

if(
!confirm(
"حذف الطفل؟"
)
)
return;

await deleteDoc(
doc(
db,
"children",
docId
)
);

};

window.toggleAttendance =
async function(docId){

const child =
children.find(
c =>
c.docId === docId
);

await updateDoc(
doc(
db,
"children",
child.docId
),
{
present:true,
attendanceDate:new Date().toLocaleDateString('en-CA')
}
);

};

window.searchChild =
function(){

const value =
document
.getElementById(
"searchInput"
)
.value;

if(value===""){

renderTable(
children
);

return;

}

renderTable(

children.filter(
c =>
String(
c.childId
)
.includes(
value
)
)

);

};

function renderTable(data){

let html = "";

data.forEach(child=>{

const supervisor =
supervisors.find(
s =>
s.docId ===
child.supervisorId
);

html += `

<tr>

<td>
${child.childId}
</td>

<td>
${child.name}
</td>

<td>
${supervisor
?
supervisor.name
:
"-"}
</td>

<td>
${child.startDate || "-"}
</td>

<td>
${child.attendanceDate || "-"}
</td>

<td>

<span
class="${
child.present
?
'present'
:
'absent'
}">

${
child.present
?
'حاضر'
:
'غائب'
}

</span>

</td>

<td>

<input
type="checkbox"
class="checkbox"
${child.present ? 'checked' : ''}
onchange="toggleAttendance('${child.docId}')">

</td>

<td>

<button
class="delete"
onclick="deleteChild('${child.docId}')">

حذف

</button>

</td>

</tr>

`;

});

document
.getElementById(
"tableBody"
)
.innerHTML = html;

updateStats();

renderGroups();

renderSupervisors();

}
  function updateStats(){

const present =
children.filter(
c=>c.present
).length;

const absent =
children.length - present;

document
.getElementById(
"presentCount"
)
.innerText = present;

document
.getElementById(
"absentCount"
)
.innerText = absent;

document
.getElementById(
"totalCount"
)
.innerText = children.length;

}

function renderGroups(){

let html = "";

supervisors.forEach(s=>{

const kids =
children.filter(
c =>
c.supervisorId === s.docId
);

html += `

<div class="card">

<h3>

👨‍🏫 ${s.name}

(${kids.length})

</h3>

<table>

<thead>

<tr>

<th>رقم الطفل</th>

<th>الاسم</th>

<th>تاريخ البدء</th>

<th>الحالة</th>

</tr>

</thead>

<tbody>

`;

kids.forEach(child=>{

html += `

<tr>

<td>
${child.childId}
</td>

<td>
${child.name}
</td>

<td>
${child.startDate || "-"}
</td>

<td>

<span
class="${
child.present
?
'present'
:
'absent'
}">

${
child.present
?
'حاضر'
:
'غائب'
}

</span>

</td>

</tr>

`;

});

html += `

</tbody>

</table>

</div>

`;

});

document
.getElementById(
"groupsContainer"
)
.innerHTML = html;

}

window.resetAttendance =
async function(){

for(const child of children){

await updateDoc(
doc(
db,
"children",
child.docId
),
{
present:false
}
);

}

};

window.exportCSV =
function(){


let csv =
"رقم الطفل\tالاسم\tالمشرف\tتاريخ البدء\tتاريخ التفقد\tالحالة\n";
children.forEach(child=>{

const supervisor =
supervisors.find(
s=>s.docId===child.supervisorId
);

csv +=
`${child.childId}\t` +
`${child.name || ''}\t` +
`${supervisor ? supervisor.name : ''}\t` +
`${child.startDate || ''}\t` +
`${child.attendanceDate || ''}\t` +
`${child.present ? 'حاضر' : 'غائب'}\n`;

});

const BOM = "\uFEFF";

const blob =
new Blob(
[BOM + csv],
{
type:"text/csv;charset=utf-8;"
}
);

const link =
document.createElement("a");

link.href =
URL.createObjectURL(blob);

link.download =
"attendance.xls";

document.body.appendChild(link);

link.click();

document.body.removeChild(link);

};

onSnapshot(
supervisorsCollection,
(snapshot)=>{

supervisors = [];

snapshot.forEach(docu=>{

supervisors.push({

docId:docu.id,
...docu.data()

});

});

fillSupervisorSelect();

renderSupervisors();

renderTable(children);

}
);

onSnapshot(
childrenCollection,
(snapshot)=>{

children = [];

snapshot.forEach(docu=>{

children.push({

docId:docu.id,
...docu.data()

});

});

renderTable(
children
);

}
);

document
.getElementById(
"qrInput"
)
.addEventListener(
"change",
async function(){

const qr =
this.value.trim();

const child =
children.find(
c =>
String(
c.childId
) === qr
);

if(child){

await updateDoc(
doc(
db,
"children",
child.docId
),
{
present:true
}
);

}

this.value="";

}
);

</script>

</body>

</html>
