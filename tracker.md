<html>
  <head>
    <title>Phone Number Lookup</title>
  </head>
  <body>
    <div>
      <label for="user_id">User ID:</label>
      <input type="text" id="user_id" placeholder="Enter user ID">
    </div>
    <div>
      <label for="phone_number">Phone Number:</label>
      <input type="text" id="phone_number" placeholder="Enter phone number">
      <button id="submit_btn">Submit</button>
    </div>
    <br>
    <table id="result_table">
      <tr>
        <th>User ID</th>
        <th>Phone Number</th>
        <th>Location</th>
        <th>Timezone</th>
        <th>Time</th>
      </tr>
    </table>
<script>
const submitBtn = document.querySelector('#submit_btn');
const resultTable = document.querySelector('#result_table');
submitBtn.addEventListener('click', async () => {
  const userId = document.querySelector('#user_id').value;
  const phoneNumber = document.querySelector('#phone_number').value;
  const data = { user_id: userId, phone_number: phoneNumber };
  const response = await fetch('/api/phone', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify(data)
  });
  if (!response.ok) {
    console.error('Error:', response.statusText);
    return;
  }
  const newData = await response.json();
  resultTable.innerHTML = `
    <tr>
      <td>${newData.user_id}</td>
      <td>${newData.phone_number}</td>
      <td>${newData.location}</td>
      <td>${newData.timezone}</td>
      <td>${newData.time}</td>
    </tr>
  ` + resultTable.innerHTML;
});
async function displayTable() {
  const response = await fetch('/api/phone');
  if (!response.ok) {
    console.error('Error:', response.statusText);
    return;
  }
  const data = await response.json();
  const rows = data.map(item => `
    <tr>
      <td>${item.user_id}</td>
      <td>${item.phone_number}</td>
      <td>${item.location}</td>
      <td>${item.timezone}</td>
      <td>${item.time}</td>
    </tr>
  `).join('');
  resultTable.innerHTML = `
    <tr>
      <th>User ID</th>
      <th>Phone Number</th>
      <th>Location</th>
      <th>Timezone</th>
      <th>Time</th>
    </tr>
  ` + rows;
}
displayTable();
</script>
  </body>
</html>
