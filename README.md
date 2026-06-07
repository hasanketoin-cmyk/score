<div style="text-align:left;margin-bottom:10px;">

<button
id="adminBtn"
class="search"
onclick="toggleAdminMode()">

🔒 فتح الإدارة

</button>


<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>نظام حضور لطلاب النادي الصيفي</title>

<style>
*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:Tahoma,sans-serif;
}

body{
background:#f3f4f6;
padding:20px;
}

.container{
max-width:1200px;
margin:auto;
}

.header{
background:#1f2937;
color:#fff;
padding:20px;
border-radius:10px;
margin-bottom:20px;
}

.card{
background:#fff;
padding:20px;
border-radius:10px;
margin-bottom:20px;
box-shadow:0 2px 8px rgba(0,0,0,.1);
}

.form-group{
display:flex;
gap:10px;
flex-wrap:wrap;
}

input{
padding:10px;
border:1px solid #ddd;
border-radius:8px;
}

button{
padding:10px 15px;
border:none;
border-radius:8px;
cursor:pointer;
color:white;
}

.add{background:#22c55e;}
.search{background:#3b82f6;}
.delete{background:#ef4444;}
.reset{background:#f59e0b;}
.export{background:#8b5cf6;}

table{
width:100%;
border-collapse:collapse;
margin-top:15px;
}

th,td{
padding:12px;
border-bottom:1px solid #ddd;
text-align:center;
}

th{
background:#f8fafc;
}

.present{
background:#22c55e;
color:white;
padding:5px 10px;
border-radius:20px;
display:inline-block;
min-width:80px;
}

.absent{
background:#ef4444;
color:white;
padding:5px 10px;
border-radius:20px;
display:inline-block;
min-width:80px;
}

.stats{
display:flex;
gap:15px;
margin-top:20px;
flex-wrap:wrap;
}

.stat{
background:#f8fafc;
padding:15px;
border-radius:10px;
min-width:140px;
text-align:center;
font-weight:bold;
}

.checkbox{
width:20px;
height:20px;
}

h2{
margin-bottom:15px;
}
</style>
</head>

<body>

<div class="container">

<div class="header">
<h1>📋 نظام حضور لطلاب النادي الصيفي</h1>
</div>
<div class="card">

<h2>👨‍🏫 إدارة المشرفين</h2>

<div class="form-group">

<input
type="text"
id="supervisorName"
placeholder="اسم المشرف">

<button
class="add"
onclick="addSupervisor()">

إضافة مشرف

</button>

</div>

<table>

<thead>
<tr>
<th>المشرف</th>
<th>عدد الأطفال</th>
<th>حذف</th>
</tr>
</thead>

<tbody id="supervisorsTable">

</tbody>

</table>

</div><div class="card">

<h2>➕ إضافة طفل</h2>

<div class="form-group">

<div class="form-group">

<input
type="text"
id="childName"
placeholder="اسم الطفل">

<select id="childSupervisor">

<option value="">
اختر المشرف
</option>

</select>

<select id="childBus">

<option value="">
اختر خط الباص
</option>

</select>

<input
type="date"
id="startDate">

<button
class="add"
onclick="addChild()">

إضافة طفل

</button>

</div>
<div class="card">

<h2>
📷 تسجيل الحضور
</h2>

<input
type="text"
id="qrInput"
placeholder="امسح QR أو أدخل رقم الطفل"
style="width:350px;">

</div>

</div>

<div class="card">

<h2>
👶 الأطفال
</h2>

<table>

<thead>

<tr>

<th>
رقم الطفل
</th>

<th>
اسم الطفل
</th>

<th>
المشرف
</th>

<th>

<th>
تاريخ البدء
</th>

<th>
تاريخ التفقد
</th>

<th>
الحالة
</th>

<th>
التفقد
</th>

<th>
حذف
</th>

</tr>

</thead>

<tbody id="tableBody">

</tbody>

</table>

<div style="margin-top:15px;">

<button
class="reset"
onclick="resetAttendance()">

إعادة الحضور

</button>

<button
class="export"
onclick="exportCSV()">

تصدير Excel

</button>

</div>

<div class="stats">

<div class="stat">

الحضور

<div id="presentCount">

0

</div>

</div>

<div class="stat">

الغياب

<div id="absentCount">

0

</div>

</div>

<div class="stat">

الإجمالي

<div id="totalCount">

0

</div>

</div>

</div>

</div>

<div class="card">

<h2>
📋 الأطفال حسب المشرف
</h2>

<div id="groupsContainer">

</div>

</div>

<div class="card">

<h2>🚌 خطوط الباصات</h2>

<table>

<thead>
<tr>
<th>اسم الخط</th>
<th>المشرف</th>
<th>عدد الأطفال</th>
</tr>
</thead>

<tbody id="busesTable">

</tbody>

</table>

<div id="busGroupsContainer">

</div>

</div>

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
const busesCollection =
collection(
db,
"buses"
);

let children = [];
let supervisors = [];
let buses = [];

async function createDefaultGroups(){

const snapshot =
await getDocs(
supervisorsCollection
);

if(snapshot.empty){

const groups = [

"المجموعة الأولى",
"المجموعة الثانية",
"المجموعة الثالثة",
"المجموعة الرابعة",
"المجموعة الخامسة"

];

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

const name =
document
.getElementById(
"supervisorName"
)
.value
.trim();

if(!name){

alert(
"أدخل اسم المشرف"
);

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
const busId =
document
.getElementById(
"childBus"
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
busId:busId,
startDate:startDate,
present:false,
busAttendance:false
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
docId
),
{
present: !child.present,

attendanceDate:
!child.present
?
new Date().toLocaleDateString('en-CA')
:
""
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
s => s.docId === child.supervisorId
);

html += `

<tr>

<td>${child.childId}</td>

<td>${child.name}</td>

<td>${child.startDate || "-"}</td>

<td>

<span class="${
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

<button
class="${child.present ? 'present' : 'absent'}"
onclick="toggleAttendance('${child.docId}')">

${child.present ? '✔ حاضر' : '✖ غائب'}

</button>

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

<div class="group-card">

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
<th>التفقد</th>
<th>حذف</th>
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

<span class="${
child.present ? 'present' : 'absent'
}">

${child.present ? 'حاضر' : 'غائب'}

</span>

</td>

<td>

<button
class="${child.present ? 'present' : 'absent'}"
onclick="toggleAttendance('${child.docId}')">

${child.present ? '🟢 حاضر' : '🔴 غائب'}

</button>

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
function renderBuses(){

let html = "";

buses.forEach(bus=>{

const kids =
children.filter(
c => c.busId === bus.docId
);

html += `

<div class="card">

<h3>
🚌 ${bus.name}
(${kids.length})
</h3>

<p>
👨‍✈️ المشرف: ${bus.supervisor}
</p>

<table>

<thead>

<tr>
<th>رقم الطفل</th>
<th>الاسم</th>
<th>الحالة</th>
</tr>

</thead>

<tbody>
`;

kids.forEach(child=>{

html += `

<tr>

<td>${child.childId}</td>

<td>${child.name}</td>

<td>

<span class="${
child.present ? 'present' : 'absent'
}">

${child.present ? 'حاضر' : 'غائب'}

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

document.getElementById(
"busGroupsContainer"
).innerHTML = html;

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
present:false,
attendanceDate:""
}
);

}

};


window.exportCSV =
function(){

window.exportCSV = function(){

let csv =
"رقم الطفل,الاسم,المشرف,تاريخ البدء,تاريخ التفقد,الحالة\n";
children.forEach(child=>{

const supervisor =
supervisors.find(
s=>s.docId===child.supervisorId
);

csv +=
`${child.childId},` +
`${child.name},` +
`${supervisor ? supervisor.name : ''},` +
`${child.startDate || ''},` +
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
"attendance.csv";

document.body.appendChild(link);

link.click();

document.body.removeChild(link);

};
children.forEach(child=>{

const supervisor =
supervisors.find(
s =>
s.docId ===
child.supervisorId
);

csv +=

`${child.childId},` +
`${child.name},` +
`${supervisor ? supervisor.name : ''},` +
`${child.startDate || ''},` +
`${child.present ? 'حاضر' : 'غائب'}\n`;

});

const blob =
new Blob(
[csv],
{
type:
'text/csv;charset=utf-8;'
}
);

const link =
document.createElement(
'a'
);

link.href =
URL.createObjectURL(
blob
);

link.download =
'attendance.csv';

link.click();

};
onSnapshot(
busesCollection,
(snapshot)=>{

buses = [];

snapshot.forEach(docu=>{

buses.push({
docId:docu.id,
...docu.data()
});

});

fillBusSelect();

renderBuses();

}
);
buses = [];

snapshot.forEach(docu=>{

buses.push({

docId:docu.id,
...docu.data()

});

});

fillBusSelect();

}
);
function fillBusSelect(){

let html =
'<option value="">اختر خط الباص</option>';

buses.forEach(bus=>{

html += `
<option value="${bus.docId}">
${bus.name}
</option>
`;

});

document.getElementById(
"childBus"
).innerHTML = html;

}
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
renderBuses();
  
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

renderTable(children);

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
