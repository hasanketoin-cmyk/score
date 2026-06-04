<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لوحة حضور الأطفال</title>

<style>
*{
    margin:0;
    padding:0;
    box-sizing:border-box;
    font-family:Tahoma, Arial, sans-serif;
}

body{
    background:#f4f6f9;
    padding:20px;
}

.container{
    max-width:1200px;
    margin:auto;
}

.header{
    background:#1f2937;
    color:white;
    padding:20px;
    border-radius:10px;
    margin-bottom:20px;
}

.card{
    background:white;
    border-radius:10px;
    padding:20px;
    margin-bottom:20px;
    box-shadow:0 2px 10px rgba(0,0,0,.1);
}

h2{
    margin-bottom:15px;
}

.form-group{
    display:flex;
    gap:10px;
    flex-wrap:wrap;
}

input{
    padding:12px;
    border:1px solid #ddd;
    border-radius:8px;
    min-width:220px;
}

button{
    padding:12px 20px;
    border:none;
    border-radius:8px;
    cursor:pointer;
    color:white;
}

.btn-add{
    background:#22c55e;
}

.btn-search{
    background:#3b82f6;
}

.btn-delete{
    background:#ef4444;
}

table{
    width:100%;
    border-collapse:collapse;
}

th,td{
    padding:12px;
    text-align:center;
    border-bottom:1px solid #ddd;
}

th{
    background:#f1f5f9;
}

.present{
    background:#22c55e;
    color:white;
    padding:6px 12px;
    border-radius:20px;
    display:inline-block;
    min-width:80px;
}

.absent{
    background:#ef4444;
    color:white;
    padding:6px 12px;
    border-radius:20px;
    display:inline-block;
    min-width:80px;
}

.stats{
    display:flex;
    gap:20px;
    margin-top:15px;
}

.box{
    background:#f8fafc;
    padding:15px;
    border-radius:10px;
    min-width:150px;
    text-align:center;
    font-weight:bold;
}

.checkbox{
    width:20px;
    height:20px;
}

#qrInput{
    width:350px;
}
</style>
</head>

<body>

<div class="container">

<div class="header">
    <h1>📋 لوحة حضور الأطفال</h1>
</div>

<div class="card">
    <h2>➕ إضافة طفل جديد</h2>

    <div class="form-group">
        <input type="text" id="childName" placeholder="اسم الطفل">
        <input type="number" id="childId" placeholder="رقم الطفل">
        <button class="btn-add" onclick="addChild()">إضافة طفل</button>
    </div>
</div>

<div class="card">
    <h2>🔍 البحث</h2>

    <div class="form-group">
        <input type="number" id="searchInput" placeholder="ابحث برقم الطفل">
        <button class="btn-search" onclick="searchChild()">بحث</button>
        <button onclick="renderTable()" style="background:#6b7280">إظهار الكل</button>
    </div>
</div>

<div class="card">
    <h2>📷 تسجيل الحضور عبر QR Code</h2>

    <input
        type="text"
        id="qrInput"
        placeholder="امسح QR Code هنا"
    >
</div>

<div class="card">

    <h2>👶 قائمة الأطفال</h2>

    <table>

        <thead>
            <tr>
                <th>رقم الطفل</th>
                <th>اسم الطفل</th>
                <th>الحالة</th>
                <th>التفقد</th>
                <th>حذف</th>
            </tr>
        </thead>

        <tbody id="tableBody"></tbody>

    </table>

    <div class="stats">

        <div class="box">
            الحضور
            <div id="presentCount">0</div>
        </div>

        <div class="box">
            الغياب
            <div id="absentCount">0</div>
        </div>

        <div class="box">
            الإجمالي
            <div id="totalCount">0</div>
        </div>

    </div>

</div>

</div>

<script>

let children = [

    {
        id:1001,
        name:"أحمد محمد",
        present:false
    },

    {
        id:1002,
        name:"سارة علي",
        present:false
    },

    {
        id:1003,
        name:"يوسف خالد",
        present:false
    }

];

function renderTable(data = children){

    let html = "";

    data.forEach(child=>{

        html += `
        <tr>

            <td>${child.id}</td>

            <td>${child.name}</td>

            <td>
                <span class="${child.present ? 'present':'absent'}">
                    ${child.present ? 'حاضر':'غائب'}
                </span>
            </td>

            <td>
                <input
                    type="checkbox"
                    class="checkbox"
                    ${child.present ? 'checked':''}
                    onchange="toggleAttendance(${child.id})"
                >
            </td>

            <td>
                <button
                    class="btn-delete"
                    onclick="deleteChild(${child.id})">
                    حذف
                </button>
            </td>

        </tr>
        `;
    });

    document.getElementById("tableBody").innerHTML = html;

    updateStats();
}

function addChild(){

    let name =
    document.getElementById("childName").value;

    let id =
    document.getElementById("childId").value;

    if(name === "" || id === ""){
        alert("أدخل البيانات");
        return;
    }

    let exists = children.find(
        child => child.id == id
    );

    if(exists){
        alert("رقم الطفل موجود مسبقاً");
        return;
    }

    children.push({

        id:Number(id),
        name:name,
        present:false

    });

    document.getElementById("childName").value="";
    document.getElementById("childId").value="";

    renderTable();
}

function deleteChild(id){

    if(confirm("هل تريد حذف الطفل؟")){

        children =
        children.filter(
            child => child.id !== id
        );

        renderTable();
    }
}

function searchChild(){

    let value =
    document.getElementById("searchInput").value;

    if(value === ""){
        renderTable();
        return;
    }

    let result = children.filter(
        child =>
        child.id.toString().includes(value)
    );

    renderTable(result);
}

function toggleAttendance(id){

    let child =
    children.find(
        child => child.id === id
    );

    child.present = !child.present;

    renderTable();
}

function updateStats(){

    let present =
    children.filter(
        child => child.present
    ).length;

    let absent =
    children.length - present;

    document.getElementById("presentCount")
    .innerText = present;

    document.getElementById("absentCount")
    .innerText = absent;

    document.getElementById("totalCount")
    .innerText = children.length;
}

document
.getElementById("qrInput")
.addEventListener("change",function(){

    let qr =
    this.value.trim();

    let child =
    children.find(
        child => child.id == qr
    );

    if(child){

        child.present = true;

        renderTable();

        alert(
        "تم تسجيل حضور: "
        + child.name
        );

    }else{

        alert(
        "لم يتم العثور على الطفل"
        );
    }

    this.value="";
});

renderTable();

</script>

</body>
</html>
