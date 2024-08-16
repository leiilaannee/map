<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NYC Locations</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <style>
        /* Ensure the map has proper dimensions */
        #map {
            height: 500px; /* Adjust height as needed */
            width: 100%; /* Full width */
        }
    </style>
</head>
<body>
    <div id="map"></div>

    <!-- Load Leaflet JavaScript library -->
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script>
        // Initialize the map
        var map = L.map('map').setView([40.7128, -74.0060], 12); // Center on NYC

        // Add tile layer from OpenStreetMap
        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        }).addTo(map);

        // Add a marker and popup
        L.marker([40.7128, -74.0060]).addTo(map)
            .bindPopup('New York City')
            .openPopup();
    </script>
</body>
</html>
