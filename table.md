<form id="inventory-form">
  <div>
    <label for="date">Date:</label>
    <input type="text" id="date" name="date">
  </div>
  <div>
    <label for="action">Action:</label>
    <input type="text" id="action" name="action">
  </div>
  <div>
    <label for="user">User:</label>
    <input type="text" id="user" name="user">
  </div>
  <div>
    <label for="item">Item:</label>
    <input type="text" id="item" name="item">
  </div>
  <div>
    <label for="quantity">Quantity:</label>
    <input type="text" id="quantity" name="quantity">
  </div>
  <button type="submit" id="add-activity-btn">Add Activity</button>

<style>
  #inventory-form {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
  }

  #inventory-form div {
    display: flex;
    flex-direction: column;
    margin-bottom: 10px;
  }

  label {
    font-weight: bold;
    margin-bottom: 5px;
  }

  input[type="text"] {
    padding: 10px;
    font-size: 16px;
    border-radius: 5px;
    border: 1px solid gray;
  }

  #add-activity-btn {
    padding: 10px 20px;
    background-color: #4CAF50;
    color: white;
    border-radius: 5px;
    border: none;
    cursor: pointer;
    font-size: 16px;
    margin-top: 20px;
  }

  #inventory-table {
    border-collapse: collapse;
    width: 100%;
  }

  #inventory-table th, td {
    border: 1px solid black;
    padding: 8px;
    text-align: left;
  }

  #inventory-table th {
    background-color: lightgray;
  }
</style>

</form>

<table id="inventory-table">
  <tr>
    <th>Date</th>
    <th>Action</th>
    <th>User</th>
    <th>Item</th>
    <th>Quantity</th>
  </tr>
</table>

<script>
const form = document.getElementById('inventory-form');
const table = document.getElementById('inventory-table');

form.addEventListener('submit', function(event) {
  event.preventDefault();

  const date = document.getElementById('date').value;
  const action = document.getElementById('action').value;
  const user = document.getElementById('user').value;
  const item = document.getElementById('item').value;
  const quantity = document.getElementById('quantity').value;

  const row = table.insertRow();
  const dateCell = row.insertCell(0);
  const actionCell = row.insertCell(1);
  const userCell = row.insertCell(2);
  const itemCell = row.insertCell(3);
  const quantityCell = row.insertCell(4);
  const actionCellLast = row.insertCell(5);

  dateCell.innerHTML = date;
  actionCell.innerHTML = action;
  userCell.innerHTML = user;
  itemCell.innerHTML = item;
  quantityCell.innerHTML = quantity;

  const editBtn = document.createElement('button');
  editBtn.innerHTML = 'Edit';
  editBtn.classList.add('edit-btn');
  editBtn.addEventListener('click', function() {
    // code for edit functionality here
  editBtn.addEventListener('click', function() {
  // Get the current values in the cells
  const date = dateCell.innerHTML;
  const action = actionCell.innerHTML;
  const user = userCell.innerHTML;
  const item = itemCell.innerHTML;
  const quantity = quantityCell.innerHTML;

  // Clear the cells
  dateCell.innerHTML = '';
  actionCell.innerHTML = '';
  userCell.innerHTML = '';
  itemCell.innerHTML = '';
  quantityCell.innerHTML = '';
  actionCellLast.innerHTML = '';

  // Create new input elements
  const dateInput = document.createElement('input');
  dateInput.value = date;
  const actionInput = document.createElement('input');
  actionInput.value = action;
  const userInput = document.createElement('input');
  userInput.value = user;
  const itemInput = document.createElement('input');
  itemInput.value = item;
  const quantityInput = document.createElement('input');
  quantityInput.value = quantity;

  // Append the input elements to the cells
  dateCell.appendChild(dateInput);
  actionCell.appendChild(actionInput);
  userCell.appendChild(userInput);
  itemCell.appendChild(itemInput);
  quantityCell.appendChild(quantityInput);

  // Create a save button
  const saveBtn = document.createElement('button');
  saveBtn.innerHTML = 'Save';
  saveBtn.classList.add('save-btn');
  saveBtn.addEventListener('click', function() {
    // Save the values in the input elements to the cells
    dateCell.innerHTML = dateInput.value;
    actionCell.innerHTML = actionInput.value;
    userCell.innerHTML = userInput.value;
    itemCell.innerHTML = itemInput.value;
    quantityCell.innerHTML = quantityInput.value;

    // Remove the input elements and save button
    dateCell.removeChild(dateInput);
    actionCell.removeChild(actionInput);
    userCell.removeChild(userInput);
    itemCell.removeChild(itemInput);
    quantityCell.removeChild(quantityInput);
    actionCellLast.removeChild(saveBtn);

    // Add the edit and delete buttons back to the action cell
    actionCellLast.appendChild(editBtn);
    actionCellLast.appendChild(deleteBtn);
  });

  // Append the save button to the action cell
  actionCellLast.appendChild(saveBtn);
});
  });
  
  const deleteBtn = document.createElement('button');
  deleteBtn.innerHTML = 'Delete';
  deleteBtn.classList.add('delete-btn');
  deleteBtn.addEventListener('click', function() {
    // code for delete functionality here
    row.remove();
  });

  actionCellLast.appendChild(editBtn);
  actionCellLast.appendChild(deleteBtn);
});

</script>

<style>
  .edit-btn, .delete-btn {
    background-color: #4CAF50;
    border: none;
    color: white;
    padding: 6px 14px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 14px;
    margin: 4px 2px;
    border-radius: 5px;
    cursor: pointer;
  }

  .edit-btn {
    background-color: #008CBA;
  }

  .delete-btn {
    background-color: #f44336;
  }
</style>


<!-- The JavaScript code does the following:

Retrieves references to the form element and table element using document.getElementById().

Attaches an event listener to the form element that listens for the "submit" event. The event listener is a function that is called whenever the form is submitted.

In the event listener function, the function prevents the default form submission behavior using event.preventDefault().

The function retrieves the values of the form input fields using document.getElementById() and stores them in variables.

The function creates a new row in the table using the table.insertRow() method, and then inserts cells into the new row using the row.insertCell() method.

The function sets the contents of the cells using the innerHTML property. The contents of the cells are the values of the form input fields.

As a result, when the form is submitted, the values of the form input fields are added as a new row in the table. -->