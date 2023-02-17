<html>

<head>
    <title>Phone Location Finder</title>
</head>
<body>
    <h1>Phone Location Finder</h1>
    <form>
        <label for="user_id">User ID:</label>
        <input type="text" id="user_id" name="user_id"><br>
        <label for="phone_number">Phone Number:</label>
        <input type="text" id="phone_number" name="phone_number"><br>
        <input type="button" value="Submit" onclick="submitForm()">
    </form>
    <script>
        function submitForm() {
            const userId = document.getElementById("user_id").value;
            const phoneNumber = document.getElementById("phone_number").value;
            fetch("/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({user_id: userId, phone_number: phoneNumber})
            })
            .then(response => {
                if (response.ok) {
                    return response.json();
                } else {
                    throw new Error("Error submitting form data");
                }
            })
            .then(data => {
                console.log(data);
                // update the HTML table with the data
            })
            .catch(error => console.error(error));
        }
    </script>
</body>

<head>
    <meta charset="utf-8">
    <title>Phone Data</title>
</head>
<body>
    <table id="phone-table">
        <thead>
            <tr>
                <th>User ID</th>
                <th>Phone Number</th>
                <th>Location</th>
                <th>Timezone</th>
                <th>Time</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>
    <script>
        // Fetch the data from the API
        fetch('https://jasj-inventory.duckdns.org/api/phone')
            .then(response => response.json())
            .then(data => {
                // Get the table body
                const tableBody = document.querySelector('#phone-table tbody');
                // Add each row of data to the table
                data.forEach(row => {
                    // Create a new table row
                    const tableRow = document.createElement('tr');
                    // Add the data to the row
                    const userIdCell = document.createElement('td');
                    userIdCell.textContent = row[0];
                    tableRow.appendChild(userIdCell);
                    const phoneNumberCell = document.createElement('td');
                    phoneNumberCell.textContent = row[1];
                    tableRow.appendChild(phoneNumberCell);
                    const locationCell = document.createElement('td');
                    locationCell.textContent = row[2];
                    tableRow.appendChild(locationCell);
                    const timezoneCell = document.createElement('td');
                    timezoneCell.textContent = row[3];
                    tableRow.appendChild(timezoneCell);
                    const timeCell = document.createElement('td');
                    timeCell.textContent = row[4];
                    tableRow.appendChild(timeCell);
                    // Add the row to the table body
                    tableBody.appendChild(tableRow);
                });
            });
    </script>
</body>
</html>