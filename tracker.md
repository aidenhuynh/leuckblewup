<html>
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