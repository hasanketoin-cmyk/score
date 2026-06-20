<div class="page-header">
<h2>🚌 نظام تفقد الباصات</h2>
</div>

<button
class="export"
onclick="exportBusAttendance()">

📊 تصدير حضور الباصات

</button>

<div id="busLinesContainer"></div>
</body>
</html>
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

const busLinesCollection =
collection(
db,
"busLines"
);

const busChildrenCollection =
collection(
db,
"busChildren"
);

let busLines = [];
let busChildren = [];

window.toggleBusAttendance =
async function(docId){

const child =
busChildren.find(
c => c.docId === docId
);

if(!child) return;

await updateDoc(
doc(
db,
"busChildren",
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

window.checkBusLine =
async function(lineId){

const kids =
busChildren.filter(
c => c.lineId === lineId
);

for(const child of kids){

await updateDoc(
doc(
db,
"busChildren",
child.docId
),
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
c => c.lineId === lineId
);

for(const child of kids){

await updateDoc(
doc(
db,
"busChildren",
child.docId
),
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
c => c.lineId === line.docId
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

<div style="margin-bottom:15px">

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

</div>

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
l => l.docId === child.lineId
);

csv +=
`${line ? line.name : ''}\t` +
`${line ? line.supervisor : ''}\t` +
`${child.name}\t` +
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
"bus_attendance.xls";

link.click();

};

onSnapshot(
busLinesCollection,
(snapshot)=>{

busLines = [];

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
(snapshot)=>{

busChildren = [];

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
