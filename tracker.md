<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Phone Number Location Lookup</title>
  </head>
  <body>
    <h1>Phone Number Location Lookup</h1>
    <form id="phone-form">
      <label for="user_id">User ID:</label>
      <input type="text" id="user_id" name="user_id" required><br>
      <label for="phone_number">Phone Number:</label>
      <input type="text" id="phone_number" name="phone_number" required><br>
      <button type="submit" id="submit-btn">Submit</button>
    </form>
    <div id="result"></div>
    <h2>Phone Data</h2>
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
      const form = document.getElementById('phone-form');
      const result = document.getElementById('result');
      const submitBtn = document.getElementById('submit-btn');
      const phoneTable = document.getElementById('phone-table');
      // Helper function to clear the table body
      function clearTable() {
        const tableBody = phoneTable.querySelector('tbody');
        tableBody.innerHTML = '';
      }
      // Helper function to add a row to the table
      function addRowToTable(rowData) {
        const tableBody = phoneTable.querySelector('tbody');
        const tableRow = document.createElement('tr');
        for (const cellData of rowData) {
          const cell = document.createElement('td');
          cell.textContent = cellData;
          tableRow.appendChild(cell);
        }
        tableBody.appendChild(tableRow);
      }
      form.addEventListener('submit', async (event) => {
        event.preventDefault();
        const formData = new FormData(event.target);
        try {
          const response = await fetch('/submit', {
            method: 'POST',
            body: formData
          });
          if (!response.ok) {
            throw new Error('Network response was not ok');
          }
          const data = await response.text();
          result.innerText = data;
          clearTable();
          // Fetch the updated data from the API and repopulate the table
          fetch('/phone')
            .then(response => response.json())
            .then(data => {
              for (const row of data) {
                addRowToTable(row);
              }
            });
        } catch (error) {
          console.error('Error:', error);
          result.innerText = 'An error occurred. Please try again later.';
        }
      });
      // Fetch the initial data from the API and populate the table
      fetch('/phone')
        .then(response => response.json())
        .then(data => {
          for (const row of data) {
            addRowToTable(row);
          }
        });
    </script>
  </body>
</html>