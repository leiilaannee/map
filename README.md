<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NYC Locations</title>
    <link rel="stylesheet" href="https://unpkg.com/leaflet/dist/leaflet.css" />
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            display: flex;
            height: 100vh;
            overflow: hidden;
        }
 
        .container {
            display: flex;
            width: 100%;
        }
 
        #map {
            width: 75%;
            height: 100vh;
            position: relative;
        }
 
        .info-box {
            position: absolute;
            bottom: 10px;
            left: 10px;
            width: 200px; /* Default width */
            height: auto;
            background: white;
            border: 1px solid #ddd;
            padding: 10px; /* Default padding */
            box-shadow: 0 0 10px rgba(0,0,0,0.2);
            display: none;
            z-index: 1000;
            transition: height 0.3s, padding 0.3s; /* Smooth transition */
        }
 
        .info-box img {
            width: 100%;
            height: auto;
        }
 
        .info-box.small {
            height: 80px; /* Shorter height on hover */
            padding: 5px; /* Reduced padding on hover */
            overflow: hidden; /* Hide overflow content */
        }
 
        .sidebar {
            width: 25%;
            background: #f4f4f4;
            padding: 10px;
            height: 100vh;
            overflow-y: auto;
            position: relative;
        }
 
        .sidebar ul {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
 
        .sidebar li {
            padding: 10px;
            cursor: pointer;
            border-bottom: 1px solid #ddd;
        }
 
        .sidebar li:hover {
            background: #ddd;
        }
 
        .sidebar li.active {
            background: #bbb;
        }
    </style>
</head>
<body>
    <div class="container">
        <div id="map"></div>
        <div class="info-box" id="info-box">
            <img id="info-image" src="" alt="Location Image">
            <h2 id="info-name"></h2>
            <p id="info-description"></p>
        </div>
        <div class="sidebar">
            <ul id="location-list">
                <!-- List items will be populated by JavaScript -->
            </ul>
        </div>
    </div>
 
    <script src="https://unpkg.com/leaflet/dist/leaflet.js"></script>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const locations = [
                { id: 1, name: 'Statue of Liberty', lat: 40.6892, lng: -74.0445, image: 'statue_of_liberty.jpg', description: 'A colossal neoclassical sculpture on Liberty Island in New York Harbor.' },
                { id: 2, name: 'Central Park', lat: 40.7851, lng: -73.9683, image: 'central_park.jpg', description: 'A large public park in New York City.' },
                { id: 3, name: 'Times Square', lat: 40.7580, lng: -73.9855, image: 'times_square.jpg', description: 'A major commercial intersection and entertainment hub in Midtown Manhattan.' },
                { id: 4, name: 'Brooklyn Bridge', lat: 40.7061, lng: -73.9969, image: 'brooklyn_bridge.jpg', description: 'A historic suspension bridge connecting Manhattan and Brooklyn.' },
                { id: 5, name: 'Empire State Building', lat: 40.748817, lng: -73.985428, image: 'empire_state_building.jpg', description: 'An iconic skyscraper in Midtown Manhattan.' },
                { id: 6, name: 'Metropolitan Museum of Art', lat: 40.7792, lng: -73.9633, image: 'met_museum.jpg', description: 'One of the largest and most prestigious art museums in the world.' },
                { id: 7, name: 'Grand Central Terminal', lat: 40.7527, lng: -73.9772, image: 'grand_central_terminal.jpg', description: 'A historic train station located in Midtown Manhattan.' },
                { id: 8, name: 'The High Line', lat: 40.7471, lng: -74.0048, image: 'high_line.jpg', description: 'An elevated linear park built on a historic freight rail line.' },
                { id: 9, name: 'One World Trade Center', lat: 40.7127, lng: -74.0134, image: 'one_world_trade_center.jpg', description: 'The main building of the rebuilt World Trade Center complex.' }
            ];
 
            const map = L.map('map').setView([40.7128, -74.0060], 12); // Center the map on NYC
 
            L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            }).addTo(map);
 
            const infoBox = document.getElementById('info-box');
            const infoImage = document.getElementById('info-image');
            const infoName = document.getElementById('info-name');
            const infoDescription = document.getElementById('info-description');
            const locationList = document.getElementById('location-list');
 
            const markers = {}; // To keep track of markers
 
            locations.forEach(location => {
                // Add marker to the map
                const marker = L.marker([location.lat, location.lng]).addTo(map);
                markers[location.id] = marker;
 
                // Add marker mouseover event
                marker.on('mouseover', function() {
                    infoImage.src = location.image;
                    infoName.textContent = location.name;
                    infoDescription.textContent = location.description;
                    infoBox.classList.add('small');
                    infoBox.style.display = 'block';
                    // Adjust box position based on marker location
                    const { lat, lng } = marker.getLatLng();
                    const position = map.latLngToLayerPoint([lat, lng]);
                    infoBox.style.top = `${position.y}px`;
                    infoBox.style.left = `${position.x + marker._icon.clientWidth}px`;
                });
 
                marker.on('mouseout', function() {
                    infoBox.classList.remove('small');
                    infoBox.style.display = 'none';
                });
 
                // Add location to sidebar
                const li = document.createElement('li');
                li.textContent = location.name;
                li.dataset.id = location.id;
                li.addEventListener('click', function() {
                    map.setView([location.lat, location.lng], 15); // Zoom in and center the map
                    infoImage.src = location.image;
                    infoName.textContent = location.name;
                    infoDescription.textContent = location.description;
                    infoBox.classList.remove('small');
                    infoBox.style.display = 'block';
                    // Highlight active item in sidebar
                    document.querySelectorAll('.sidebar li').forEach(item => item.classList.remove('active'));
                    li.classList.add('active');
                });
                locationList.appendChild(li);
            });
 
            // Close info box when clicking outside
            document.addEventListener('click', function(event) {
                if (!event.target.closest('.info-box') && !event.target.closest('.leaflet-marker-icon')) {
                    infoBox.classList.remove('small');
                    infoBox.style.display = 'none';
                }
            });
        });
    </script>
</body>
</html>
