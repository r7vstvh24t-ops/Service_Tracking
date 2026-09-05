<!DOCTYPE html>  
<html lang="en">  
<head>  
<meta charset="UTF-8">  
<meta name="viewport" content="width=device-width, initial-scale=1.0">  
<title>Honda Service Tracking</title>  
  
<style>  
*{box-sizing:border-box}  
body{  
  margin:0;  
  font-family:Arial,sans-serif;  
  background:#f2f4f7;  
  color:#222;  
}  
header{  
  background:#111;  
  color:white;  
  padding:18px;  
  text-align:center;  
}  
header h1{  
  margin:0;  
  font-size:22px;  
}  
.container{  
  max-width:1100px;  
  margin:auto;  
  padding:15px;  
}  
.card{  
  background:white;  
  padding:18px;  
  border-radius:12px;  
  box-shadow:0 2px 10px #0001;  
  margin-bottom:15px;  
}  
.grid{  
  display:grid;  
  grid-template-columns:repeat(2,1fr);  
  gap:12px;  
}  
label{  
  font-weight:bold;  
  display:block;  
  margin-bottom:5px;  
}  
input,select,textarea,button{  
  width:100%;  
  padding:11px;  
  border:1px solid #ccc;  
  border-radius:8px;  
  font-size:15px;  
}  
textarea{min-height:70px}  
button{  
  background:#111;  
  color:white;  
  border:none;  
  cursor:pointer;  
  font-weight:bold;  
}  
button:hover{opacity:.85}  
.btn-green{background:#198754}  
.btn-blue{background:#0d6efd}  
.btn-red{background:#dc3545}  
.actions{  
  display:grid;  
  grid-template-columns:repeat(3,1fr);  
  gap:10px;  
  margin-top:15px;  
}  
.summary{  
  display:grid;  
  grid-template-columns:repeat(3,1fr);  
  gap:10px;  
}  
.stat{  
  background:#111;  
  color:white;  
  padding:15px;  
  border-radius:10px;  
  text-align:center;  
}  
.stat b{  
  display:block;  
  font-size:20px;  
  margin-top:5px;  
}  
table{  
  width:100%;  
  border-collapse:collapse;  
  margin-top:10px;  
  font-size:13px;  
}  
th,td{  
  border:1px solid #ddd;  
  padding:7px;  
  text-align:left;  
}  
th{  
  background:#111;  
  color:white;  
}  
.table-wrap{  
  overflow-x:auto;  
}  
@media(max-width:650px){  
  .grid,.summary,.actions{  
    grid-template-columns:1fr;  
  }  
  header h1{font-size:19px}  
}  
</style>  
</head>  
  
<body>  
  
<header>  
  <h1>HONDA SERVICE TRACKING</h1>  
  <small>Service Record Management</small>  
</header>  
  
<div class="container">  
  
<div class="card">  
<h2>New Service Record</h2>  
  
<div class="grid">  
  
<div>  
<label>Date</label>  
<input type="date" id="date">  
</div>  
  
<div>  
<label>Name</label>  
<select id="name">  
<option value="">Select Name</option>  
<option>Damith</option>  
<option>Buddikka</option>  
<option>Ranjith</option>  
<option>Salinga</option>  
<option>Rashmika</option>  
<option>Suresh</option>  
<option>Charith</option>  
<option>Sachin</option>  
<option>Pradeep</option>  
<option>Pallewatta</option>  
</select>  
</div>  
  
<div>  
<label>Cycle Number</label>  
<input type="text" id="cycle" placeholder="Enter Cycle Number">  
</div>  
  
<div>  
<label>Service Type</label>  
<select id="service" onchange="setSchedule()">  
<option value="">Select Service</option>  
</select>  
</div>  
  
<div>  
<label>Schedule Hours</label>  
<input type="text" id="schedule" readonly>  
</div>  
  
<div>  
<label>Start Time</label>  
<input type="time" id="start">  
</div>  
  
<div>  
<label>End Time</label>  
<input type="time" id="end" onchange="calculateActual()">  
</div>  
  
<div>  
<label>Actual Hours</label>  
<input type="text" id="actual" readonly>  
</div>  
  
<div>  
<label>Status</label>  
<select id="status">  
<option value="">Select Status</option>  
<option>Parts at Parts Center</option>  
<option>Parts to be brought by Owner</option>  
<option>Questions – Not Yet Approved</option>  
<option>Questions – Approved</option>  
<option>Sent for Welding</option>  
<option>Sent for Wash & Wax</option>  
</select>  
</div>  
  
<div>  
<label>Remarks</label>  
<textarea id="remarks" placeholder="Enter remarks"></textarea>  
</div>  
  
</div>  
  
<div class="actions">  
<button class="btn-green" onclick="saveRecord()">SAVE RECORD</button>  
<button class="btn-blue" onclick="exportCSV()">EXPORT CSV</button>  
<button class="btn-red" onclick="clearAll()">CLEAR ALL</button>  
</div>  
  
</div>  
  
<div class="card">  
<h2>Summary</h2>  
  
<div class="summary">  
  
<div class="stat">  
Total Records  
<b id="totalRecords">0</b>  
</div>  
  
<div class="stat">  
Scheduled Hours  
<b id="totalSchedule">0:00</b>  
</div>  
  
<div class="stat">  
Actual Hours  
<b id="totalActual">0:00</b>  
</div>  
  
</div>  
</div>  
  
<div class="card">  
  
<h2>Service Records</h2>  
  
<div class="table-wrap">  
  
<table>  
<thead>  
<tr>  
<th>Date</th>  
<th>Name</th>  
<th>Cycle</th>  
<th>Service</th>  
<th>Schedule</th>  
<th>Start</th>  
<th>End</th>  
<th>Actual</th>  
<th>Status</th>  
<th>Remarks</th>  
</tr>  
</thead>  
  
<tbody id="records"></tbody>  
  
</table>  
  
</div>  
  
</div>  
  
</div>  
  
<script>  
  
const services = {  
  
"PM 1":15,  
"PM 6":25,  
"PM 12":120,  
"PM 18":40,  
"PM 24":120,  
"PM 30":40,  
"PM 36":120,  
"PM 42":40,  
"PM 48":120,  
"PM 54":40,  
"PM 60":120,  
"PM 66":40,  
"PM 72":120,  
"PM 78":40,  
"PM 84":120,  
"PM 90":40,  
"PM 96":120,  
"PM 102":40,  
"Basic package":120,  
"M.Fornt package":180,  
"M.Rear package":120,  
"Clutch control package":120,  
"Fuel package":240,  
"Engine Head Overhaul":360,  
"Engine Full overhaul":960,  
"Annual Check":40  
  
};  
  
const serviceSelect=document.getElementById("service");  
  
for(let s in services){  
  
let option=document.createElement("option");  
  
option.value=s;  
option.textContent=s;  
  
serviceSelect.appendChild(option);  
  
}  
  
document.getElementById("date").valueAsDate=new Date();  
  
function formatMinutes(minutes){  
  
let h=Math.floor(minutes/60);  
let m=minutes%60;  
  
return h+":"+String(m).padStart(2,"0");  
  
}  
  
function setSchedule(){  
  
let service=document.getElementById("service").value;  
  
if(service){  
  
document.getElementById("schedule").value=  
formatMinutes(services[service]);  
  
}else{  
  
document.getElementById("schedule").value="";  
  
}  
  
}  
  
function timeToMinutes(time){  
  
let [h,m]=time.split(":").map(Number);  
  
return h*60+m;  
  
}  
  
function calculateActual(){  
  
let start=document.getElementById("start").value;  
let end=document.getElementById("end").value;  
  
if(!start || !end)return;  
  
let s=timeToMinutes(start);  
let e=timeToMinutes(end);  
  
let total=0;  
  
const periods=[  
[495,720],  
[780,1125],  
[1200,1350]  
];  
  
for(let p of periods){  
  
let from=Math.max(s,p[0]);  
let to=Math.min(e,p[1]);  
  
if(to>from){  
  
total+=to-from;  
  
}  
  
}  
  
document.getElementById("actual").value=  
formatMinutes(total);  
  
}  
  
function saveRecord(){  
  
let record={  
  
date:document.getElementById("date").value,  
name:document.getElementById("name").value,  
cycle:document.getElementById("cycle").value,  
service:document.getElementById("service").value,  
schedule:document.getElementById("schedule").value,  
start:document.getElementById("start").value,  
end:document.getElementById("end").value,  
actual:document.getElementById("actual").value,  
status:document.getElementById("status").value,  
remarks:document.getElementById("remarks").value  
  
};  
  
if(!record.date || !record.name || !record.cycle || !record.service){  
  
alert("Please enter Date, Name, Cycle Number and Service Type.");  
  
return;  
  
}  
  
let records=  
JSON.parse(localStorage.getItem("hondaRecords")||"[]");  
  
records.push(record);  
  
localStorage.setItem("hondaRecords",JSON.stringify(records));  
  
displayRecords();  
  
alert("Record Saved Successfully!");  
  
clearForm();  
  
}  
  
function clearForm(){  
  
document.getElementById("cycle").value="";  
document.getElementById("service").value="";  
document.getElementById("schedule").value="";  
document.getElementById("start").value="";  
document.getElementById("end").value="";  
document.getElementById("actual").value="";  
document.getElementById("status").value="";  
document.getElementById("remarks").value="";  
  
}  
  
function displayRecords(){  
  
let records=  
JSON.parse(localStorage.getItem("hondaRecords")||"[]");  
  
let tbody=document.getElementById("records");  
  
tbody.innerHTML="";  
  
let scheduleTotal=0;  
let actualTotal=0;  
  
records.forEach(r=>{  
  
let tr=document.createElement("tr");  
  
tr.innerHTML=`  
  
<td>${r.date}</td>  
<td>${r.name}</td>  
<td>${r.cycle}</td>  
<td>${r.service}</td>  
<td>${r.schedule}</td>  
<td>${r.start}</td>  
<td>${r.end}</td>  
<td>${r.actual}</td>  
<td>${r.status}</td>  
<td>${r.remarks}</td>  
  
`;  
  
tbody.appendChild(tr);  
  
scheduleTotal+=parseDuration(r.schedule);  
actualTotal+=parseDuration(r.actual);  
  
});  
  
document.getElementById("totalRecords").textContent=  
records.length;  
  
document.getElementById("totalSchedule").textContent=  
formatMinutes(scheduleTotal);  
  
document.getElementById("totalActual").textContent=  
formatMinutes(actualTotal);  
  
}  
  
function parseDuration(value){  
  
if(!value)return 0;  
  
let [h,m]=value.split(":").map(Number);  
  
return h*60+m;  
  
}  
  
function exportCSV(){  
  
let records=  
JSON.parse(localStorage.getItem("hondaRecords")||"[]");  
  
if(records.length===0){  
  
alert("No records to export.");  
  
return;  
  
}  
  
let csv="Date,Name,Cycle Number,Service Type,Schedule Hours,Start Time,End Time,Actual Hours,Status,Remarks\n";  
  
records.forEach(r=>{  
  
csv+=  
`"${r.date}","${r.name}","${r.cycle}","${r.service}","${r.schedule}","${r.start}","${r.end}","${r.actual}","${r.status}","${r.remarks}"\n`;  
  
});  
  
let blob=new Blob([csv],{type:"text/csv;charset=utf-8;"});  
  
let url=URL.createObjectURL(blob);  
  
let a=document.createElement("a");  
  
a.href=url;  
  
a.download="Honda_Service_Records.csv";  
  
a.click();  
  
URL.revokeObjectURL(url);  
  
}  
  
function clearAll(){  
  
if(confirm("Delete ALL service records?")){  
  
localStorage.removeItem("hondaRecords");  
  
displayRecords();  
  
}  
  
}  
  
displayRecords();  
  
</script>  
  
</body>  
</html>  
