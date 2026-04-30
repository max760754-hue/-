# -<!DOCTYPE html>
<html lang="ar">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Energy GIS Dashboard</title>

<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"/>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>
body {
    margin: 0;
    font-family: Arial, sans-serif;
    direction: rtl;
    background: #0f172a;
    color: white;
}

/* Sidebar */
.sidebar {
    position: fixed;
    width: 220px;
    height: 100%;
    background: #111827;
    padding: 20px;
}

.sidebar h2 {
    color: #38bdf8;
}

.sidebar button {
    width: 100%;
    margin: 8px 0;
    padding: 10px;
    border: none;
    border-radius: 8px;
    background: #1f2937;
    color: white;
    cursor: pointer;
}

.sidebar button:hover {
    background: #38bdf8;
    color: black;
}

/* Main */
.main {
    margin-right: 240px;
    padding: 20px;
}

/* Header */
.header {
    background: linear-gradient(90deg, #0ea5e9, #8b5cf6);
    padding: 20px;
    border-radius: 12px;
    margin-bottom: 20px;
}

/* Cards */
.cards {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
}

.card {
    flex: 1;
    min-width: 180px;
    background: #1f2937;
    padding: 15px;
    border-radius: 12px;
    border-left: 4px solid #38bdf8;
}

/* Sections */
.section {
    display: none;
}

.active {
    display: block;
}

/* Map */
#map {
    height: 420px;
    border-radius: 12px;
}
</style>
</head>

<body>

<div class="sidebar">
    <h2>⚡ Energy GIS</h2>
    <button onclick="showSection('dashboard')">Dashboard</button>
    <button onclick="showSection('mapSec')">الخريطة</button>
    <button onclick="showSection('charts')">التحليل</button>
</div>

<div class="main">

    <div class="header">
        <h2>جامعة قنا - كلية الآداب</h2>
        <p>نظام مراقبة توليد الطاقة</p>
    </div>

    <!-- DASHBOARD -->
    <div id="dashboard" class="section active">

        <div class="cards">
            <div class="card">⚡ إجمالي الطاقة<br><b>34.75 kWh</b></div>
            <div class="card">👣 عدد الخطوات<br><b>118,650</b></div>
            <div class="card">📊 متوسط الطاقة<br><b>0.293 Wh</b></div>
        </div>

        <h3>📊 توزيع المناطق</h3>
        <canvas id="barChart"></canvas>
    </div>

    <!-- MAP -->
    <div id="mapSec" class="section">
        <h3>🗺️ الخريطة التفاعلية</h3>
        <div id="map"></div>
    </div>

    <!-- CHARTS -->
    <div id="charts" class="section">
        <h3>📈 تحليل الطاقة</h3>
        <canvas id="pieChart"></canvas>
    </div>

</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>

<script>

let map;
let mapInitialized = false;
let chartsInitialized = false;

/* NAVIGATION */
function showSection(id){
    document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
    document.getElementById(id).classList.add('active');

    // Initialize map only when needed
    if(id === 'mapSec' && !mapInitialized){
        initMap();
        mapInitialized = true;
    }

    // Fix map rendering bug
    if(id === 'mapSec' && map){
        setTimeout(()=> map.invalidateSize(), 200);
    }

    // Charts lazy init
    if(id === 'dashboard' && !chartsInitialized){
        initCharts();
        chartsInitialized = true;
    }
}

/* MAP INIT */
function initMap(){
    map = L.map('map').setView([26.1551, 32.7160], 17);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap'
    }).addTo(map);

    L.circle([26.1552, 32.7162], {color:'red', radius:20}).addTo(map).bindPopup("طاقة عالية 🔥");
    L.circle([26.1548, 32.7158], {color:'yellow', radius:20}).addTo(map).bindPopup("طاقة متوسطة ⚡");
    L.circle([26.1555, 32.7166], {color:'green', radius:20}).addTo(map).bindPopup("طاقة منخفضة 🌱");
}

/* CHARTS */
function initCharts(){

    new Chart(document.getElementById('barChart'), {
        type: 'bar',
        data: {
            labels: ['الوسطى','الشمال','الجنوب'],
            datasets: [{
                data: [8.6, 7.3, 6.1],
                backgroundColor: ['#38bdf8','#a78bfa','#34d399']
            }]
        }
    });

    new Chart(document.getElementById('pieChart'), {
        type: 'pie',
        data: {
            labels: ['عالية','متوسطة','منخفضة'],
            datasets: [{
                data: [40, 35, 25],
                backgroundColor: ['#ef4444','#facc15','#22c55e']
            }]
        }
    });
}

</script>

</body>
</html>
