<!DOCTYPE html>
<html>
<head>
    <title>Blood Donor Finder with Location</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #ffe6e6;
            text-align: center;
            margin: 0;
        }

        header {
            background: darkred;
            color: white;
            padding: 15px;
        }

        .container {
            width: 90%;
            max-width: 600px;
            margin: 20px auto;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 0px 10px gray;
        }

        input, select, button {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
        }

        button {
            background: red;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background: darkred;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }

        th, td {
            border: 1px solid black;
            padding: 8px;
        }

        th {
            background: red;
            color: white;
        }
    </style>
</head>

<body>

<header>
    <h1>🩸 Blood Donor Finder (With Location)</h1>
</header>

<div class="container">

    <h2>Register as Donor</h2>
    <input type="text" id="name" placeholder="Full Name">
    
    <select id="bloodGroup">
        <option value="">Select Blood Group</option>
        <option>A+</option><option>A-</option>
        <option>B+</option><option>B-</option>
        <option>O+</option><option>O-</option>
        <option>AB+</option><option>AB-</option>
    </select>

    <input type="text" id="city" placeholder="City">
    <input type="text" id="phone" placeholder="Phone Number">

    <button onclick="getLocation()">Detect Location</button>
    <p id="locationStatus"></p>

    <button onclick="addDonor()">Register</button>

    <h2>Search Donor</h2>
    <select id="searchGroup">
        <option value="">Select Blood Group</option>
        <option>A+</option><option>A-</option>
        <option>B+</option><option>B-</option>
        <option>O+</option><option>O-</option>
        <option>AB+</option><option>AB-</option>
    </select>

    <button onclick="searchDonor()">Search</button>

    <div id="result"></div>

</div>

<script>
    let donors = JSON.parse(localStorage.getItem("donors")) || [];
    let latitude = "";
    let longitude = "";

    function getLocation() {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(function(position) {
                latitude = position.coords.latitude;
                longitude = position.coords.longitude;
                document.getElementById("locationStatus").innerHTML =
                    "Location detected: " + latitude + ", " + longitude;
            });
        } else {
            alert("Geolocation not supported!");
        }
    }

    function addDonor() {
        let name = document.getElementById("name").value;
        let bloodGroup = document.getElementById("bloodGroup").value;
        let city = document.getElementById("city").value;
        let phone = document.getElementById("phone").value;

        if (!name || !bloodGroup || !city || !phone || !latitude) {
            alert("Please fill all fields and detect location!");
            return;
        }

        donors.push({name, bloodGroup, city, phone, latitude, longitude});
        localStorage.setItem("donors", JSON.stringify(donors));

        alert("Donor Registered Successfully!");
        location.reload();
    }

    function searchDonor() {
        let group = document.getElementById("searchGroup").value;
        let resultDiv = document.getElementById("result");

        let filtered = donors.filter(d => d.bloodGroup === group);

        if (filtered.length === 0) {
            resultDiv.innerHTML = "<p>No donors found.</p>";
            return;
        }

        let table = "<table><tr><th>Name</th><th>City</th><th>Phone</th><th>Location</th></tr>";

        filtered.forEach(d => {
            table += `<tr>
                        <td>${d.name}</td>
                        <td>${d.city}</td>
                        <td>${d.phone}</td>
                        <td>${d.latitude}, ${d.longitude}</td>
                      </tr>`;
        });

        table += "</table>";
        resultDiv.innerHTML = table;
    }
</script>

</body>
</html>
