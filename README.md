<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>لوحة حضور الأطفال</title>

<style>
body{
    font-family: Tahoma, Arial;
    background:#f5f7fa;
    margin:0;
    padding:20px;
}

.header{
    background:#1f2937;
    color:white;
    padding:15px;
    border-radius:10px;
    margin-bottom:20px;
}

.card{
    background:white;
    border-radius:10px;
    padding:20px;
    box-shadow:0 2px 10px rgba(0,0,0,.1);
}

table{
    width:100%;
    border-collapse:collapse;
}

th,td{
    padding:12px;
    border-bottom:1px solid #ddd;
    text-align:center;
}

th{
    background:#f1f5f9;
}

.status{
    width:120px;
    font-weight:bold;
    color:white;
    border-radius:20px;
    padding:8px;
}

.present{
    background:#22c55e;
}

.absent{
    background:#ef4444;
}

input{
    width:300px;
    padding:12px;
    border:1px solid #ccc;
    border-radius:8px;
    margin-bottom:20px;
}
</style>
</head>
<body>

<div class="header">
    <h2>لوحة حضور الأطفال</h2>
</div>

<div class="card">

    <input
        type="text"
        id="qrInput"
        placeholder="امسح QR Code هنا..."
        autofocus
    >

    <table>
        <thead>
            <tr>
                <th>#</th>
                <th>اسم الطفل</th>
                <th>رقم QR</th>
                <th>الحالة</th>
            </tr>
        </thead>

        <tbody id="studentsTable">

            <tr data-qr="1001">
                <td>1</td>
                <td>أحمد محمد</td>
                <td>1001</td>
                <td>
                    <div class="status absent">غائب</div>
                </td>
            </tr>

            <tr data-qr="1002">
                <td>2</td>
                <td>سارة علي</td>
                <td>1002</td>
                <td>
                    <div class="status absent">غائب</div>
                </td>
            </tr>

            <tr data-qr="1003">
                <td>3</td>
                <td>يوسف خالد</td>
                <td>1003</td>
                <td>
                    <div class="status absent">غائب</div>
                </td>
            </tr>

            <tr data-qr="1004">
                <td>4</td>
                <td>ليان أحمد</td>
                <td>1004</td>
                <td>
                    <div class="status absent">غائب</div>
                </td>
            </tr>

        </tbody>
    </table>

</div>

<script>

const qrInput = document.getElementById("qrInput");

qrInput.addEventListener("change", function(){

    const qrCode = this.value.trim();

    const row = document.querySelector(
        `tr[data-qr="${qrCode}"]`
    );

    if(row){

        const status = row.querySelector(".status");

        status.classList.remove("absent");
        status.classList.add("present");

        status.innerHTML = "حاضر";

    }else{

        alert("QR غير موجود");
    }

    this.value = "";
});

</script>

</body>
</html>
