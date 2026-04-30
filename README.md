function initMap(){

    // 📍 ملعب كلية طب الأسنان - جامعة قنا (الموقع الصحيح على الخريطة)
    const dentistryField = [26.1559, 32.7168];

    map = L.map('map').setView(dentistryField, 18);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '&copy; OpenStreetMap'
    }).addTo(map);

    // 🏟️ موقع الملعب
    L.marker(dentistryField).addTo(map)
        .bindPopup("🏟️ ملعب كلية طب الأسنان - جامعة قنا")
        .openPopup();

    // 🔥 توزيع الطاقة (حول الملعب)
    L.circle(dentistryField, {
        color: "red",
        radius: 25,
        fillOpacity: 0.4
    }).addTo(map).bindPopup("طاقة عالية 🔥");

    L.circle([26.1563, 32.7162], {
        color: "yellow",
        radius: 25,
        fillOpacity: 0.3
    }).addTo(map).bindPopup("طاقة متوسطة ⚡");

    L.circle([26.1552, 32.7174], {
        color: "green",
        radius: 25,
        fillOpacity: 0.3
    }).addTo(map).bindPopup("طاقة منخفضة 🌱");
}
