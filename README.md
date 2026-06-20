<html lang="ar" dir="rtl">

<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">

<title>نظام تفقد الباصات</title>

<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;600;700&display=swap" rel="stylesheet">

<style>

*{
margin:0;
padding:0;
box-sizing:border-box;
font-family:'Cairo',sans-serif;
}

body{
background:#eef4ff;
padding:25px;
}

.page-header{
background:#fff;
padding:25px;
border-radius:20px;
margin-bottom:20px;
box-shadow:0 5px 15px rgba(0,0,0,.08);
}

.page-header h2{
color:#1e40af;
font-size:30px;
}

.export{
background:#7c3aed;
color:white;
border:none;
padding:12px 20px;
border-radius:10px;
cursor:pointer;
margin-bottom:20px;
font-size:16px;
}

.card{
background:white;
padding:20px;
margin-bottom:20px;
border-radius:20px;
box-shadow:0 5px 15px rgba(0,0,0,.08);
}

.card h3{
margin-bottom:10px;
color:#1e40af;
}

.card p{
margin-bottom:15px;
}

.add{
background:#16a34a;
color:white;
border:none;
padding:10px 15px;
border-radius:10px;
cursor:pointer;
margin-left:5px;
}

.reset{
background:#dc2626;
color:white;
border:none;
padding:10px 15px;
border-radius:10px;
cursor:pointer;
}

table{
width:100%;
border-collapse:collapse;
margin-top:15px;
}

th{
background:#2563eb;
color:white;
padding:12px;
}

td{
padding:12px;
text-align:center;
border:1px solid #ddd;
}

.present{
color:#16a34a;
font-weight:700;
}

.absent{
color:#dc2626;
font-weight:700;
}

.checkbox{
width:22px;
height:22px;
}

</style>

</head>

<body>

<div class="page-header">
<h2>🚌 نظام تفقد الباصات</h2>
</div>

<button
class="export"
onclick="exportBusAttendance()">

📊 تصدير حضور الباصات

</button>

<div id="busLinesContainer"></div>
<script type="module">

import { initializeApp }
from "https://www.gstatic.com/firebasejs/11.8.1/firebase-app.js";

import {
getFirestore,
collection,
doc,
onSnapshot,
updateDoc
}
from "https://www.gstatic.com/firebasejs/11.8.1/firebase-firestore.js";

const firebaseConfig = {

apiKey:"AIzaSyA-aV5qGj27Vj0e8dtrXkz9Ckt5Tp-reyY",

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

const busLinesCollection =
collection(db,"busLines");

const busChildrenCollection =
collection(db,"busChildren");

let busLines = [];
let busChildren = [];

window.toggleBusAttendance =
async function(docId){

const child =
busChildren.find(
c=>c.docId===docId
);

if(!child) return;

await updateDoc(
doc(db,"busChildren",docId),
{
present:!child.present,
attendanceDate:
!child.present
?
new Date().toLocaleDateString('en-CA')
:
""
}
);

};

window.checkBusLine =
async function(lineId){

const kids =
busChildren.filter(
c=>c.lineId===lineId
);

for(const child of kids){

await updateDoc(
doc(db,"busChildren",child.docId),
{
present:true,
attendanceDate:
new Date().toLocaleDateString('en-CA')
}
);

}

};

window.uncheckBusLine =
async function(lineId){

const kids =
busChildren.filter(
c=>c.lineId===lineId
);

for(const child of kids){

await updateDoc(
doc(db,"busChildren",child.docId),
{
present:false,
attendanceDate:""
}
);

}

};

function renderBusLines(){

let html = "";

busLines.forEach(line=>{

const kids =
busChildren.filter(
c=>c.lineId===line.docId
);

html += `

<div class="card">

<h3>
🚌 ${line.name}
(${kids.length})
</h3>

<p>
مشرف الباص:
${line.supervisor || "-"}
</p>

<button
class="add"
onclick="checkBusLine('${line.docId}')">

✅ تفقد الخط

</button>

<button
class="reset"
onclick="uncheckBusLine('${line.docId}')">

❌ إلغاء التفقد

</button>

<table>

<thead>
<tr>
<th>الطفل</th>
<th>الحالة</th>
<th>التفقد</th>
</tr>
</thead>

<tbody>
`;

kids.forEach(child=>{

html += `

<tr>

<td>${child.name}</td>

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

<input
type="checkbox"
class="checkbox"
${child.present ? "checked" : ""}
onchange="toggleBusAttendance('${child.docId}')">

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
"busLinesContainer"
).innerHTML = html;

}

window.exportBusAttendance =
function(){

let csv =
"الخط\tالمشرف\tالطفل\tالحالة\n";

busChildren.forEach(child=>{

const line =
busLines.find(
l=>l.docId===child.lineId
);

csv +=
`${line?.name || ''}\t` +
`${line?.supervisor || ''}\t` +
`${child.name}\t` +
`${child.present ? 'حاضر' : 'غائب'}\n`;

});

const blob =
new Blob(
["\uFEFF"+csv],
{
type:"text/csv;charset=utf-8;"
}
);

const link =
document.createElement("a");

link.href =
URL.createObjectURL(blob);

link.download =
"bus_attendance.xls";

link.click();

};

onSnapshot(
busLinesCollection,
snapshot=>{

busLines=[];

snapshot.forEach(docu=>{

busLines.push({
docId:docu.id,
...docu.data()
});

});

renderBusLines();

}
);

onSnapshot(
busChildrenCollection,
snapshot=>{

busChildren=[];

snapshot.forEach(docu=>{

busChildren.push({
docId:docu.id,
...docu.data()
});

});

renderBusLines();

}
);

</script>

</body>
</html>
