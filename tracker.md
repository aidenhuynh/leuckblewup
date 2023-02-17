<html>
  <head>
    <meta charset="UTF-8">
    <title>Phone Number Table</title>
    <style>
      table {
        border-collapse: collapse;
        width: 100%;
      }
      th, td {
        text-align: left;
        padding: 8px;
      }
      th {
        background-color: #444444;
        color: white;
      }
      tr:nth-child(even) {
        background-color: #f2f2f2;
      }
    </style>
  </head>
  <body>
    <h1>Phone Number Table</h1>
    <form id="myForm">
      <label for="userID">User ID:</label>
      <input type="text" id="userID" name="userID"><br><br>
      <label for="phoneNumber">Phone Number:</label>
      <input type="text" id="phoneNumber" name="phoneNumber"><br><br>
      <button type="button" onclick="postData()">Submit</button>
    </form>
    <br><br>
    <table id="phoneTable">
      <tr>
        <th>User ID</th>
        <th>Phone Number</th>
        <th>Location</th>
        <th>Timezone</th>
        <th>Time</th>
      </tr>
    </table>
    <script>
      function postData() {
        let userID = document.getElementById("userID").value;
        let phoneNumber = document.getElementById("phoneNumber").value;
        fetch('/api/phone', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify({
            'userID': userID,
            'phoneNumber': phoneNumber
          })
        })
        .then(response => response.json())
        .then(data => {
          console.log(data);
          document.getElementById("userID").value = "";
          document.getElementById("phoneNumber").value = "";
          getPhoneData();
        });
      }
      function getPhoneData() {
        fetch('https://jasj-inventory.duckdns.org/api/phone')
        .then(response => response.json())
        .then(data => {
          console.log(data);
          let table = document.getElementById("phoneTable");
          table.innerHTML = `
            <tr>
              <th>User ID</th>
              <th>Phone Number</th>
              <th>Location</th>
              <th>Timezone</th>
              <th>Time</th>
            </tr>
          `;
          data.forEach(row => {
            table.innerHTML += `
              <tr>
                <td>${row.user_id}</td>
                <td>${row.phone_number}</td>
                <td>${row.location}</td>
                <td>${row.timezone}</td>
                <td>${row.time}</td>
              </tr>
            `;
          });
        });
      }
      document.addEventListener("DOMContentLoaded", function(event) {
        getPhoneData();
      });
    </script>
  </body>
</html>