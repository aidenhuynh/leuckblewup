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









<html lang="en">
<head>
    <title>Inventory</title>
</head>
<body>
    <h1>Inventory</h1>
    <i>Enter the name of your company to get started</i>
    <input placeholder="Company" id="company" />
    <p>If you already have saved inventory, enter your company name above and hit "load inventory"</p>
    <button onclick="loadInventory()">Load Inventory</button>

  <hr />
    <div id="existingInventory">
    </div>
    <div id="InventoryBox">
        <div>
    <h2>Add Inventory</h2>
    <form id="anInventory">
        <input placeholder="Name of Inventory" id="inventory_name" />
        <input placeholder="Action" id="action" />
        <input placeholder="Quantity" id="quantity" />
        <textarea placeholder="Extra Notes" id="extra_notes"></textarea>
        <input type="submit" />
    </form>

</div>
<div id="previewInventory">
    <h2>Preview Inventory</h2>
    <p id="InventoryName">Inventory Name: N/A</p>
    <p id="Action">Action: N/A</p>
    <p id="Quantity">Quantity: N/A</p>
    <p id="ExtraNotes">Extra Notes: N/A</p>
    <button onclick="deleteInventory()">Delete Inventory</button>
</div>
</div>

</body>

<script>
    let InventoryLoader = {}
    let currentInventory = -1
    // change in AWS
    const url = ""

    const previewInventory = (inventory) => {
        document.getElementById("InventoryName").innerHTML = "Inventory Name: " + inventory.inventory_name
        document.getElementById("Action").innerHTML = "Action: " + inventory.action
        document.getElementById("Quantity").innerHTML = "Quantity: " + inventory.quantity + " of them"
        document.getElementById("ExtraNotes").innerHTML = "Extra Notes:\n" + inventory.extra_notes
        currentInventory = inventory.id
    }

    const loadInventory = () => {
        const company = document.getElementById("company").value
        if (company === "") {alert("Invalid company name!"); return}
        try {
        fetch(url + new URLSearchParams({Company: company})).then(data => data.json()).then(json => {
            document.getElementById("existingInventory").innerHTML = ""
            
            json.forEach(inventory => {
                const button = document.createElement("button")
                button.innerHTML = inventory.inventory_name
                inventoryLoader[inventory.id] = JSON.parse(JSON.stringify(inventory))
                button.onclick = () => previewInventory(inventoryLoader[inventory.id])
                document.getElementById("existingInventory").appendChild(button)
            })

            localStorage.setItem("company", company)
        })
    } catch {
        alert("Company not found!")
    }
    }

    const deleteInventory = () => {
        if (currentInventory < 0) {
            alert("invalid inventory!")
            return
        }

        fetch(url, {
            method: 'DELETE',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({id: currentInventory})
        }).then(() => {
            alert("Success, deleted!")
            loadInventory()
        })
    }

    document.getElementById("anInventory").addEventListener("submit", (e) => {
        e.preventDefault();
        e.stopImmediatePropagation();
        const fields = [
            "inventory_name",
            "company",
            "action",
            "quantity",
            "extra_notes"
        ]
        const values = fields.map((f) => document.getElementById(f).value)
        if (values.indexOf("") !== -1) {
            alert("Please fill out all the fields!")
            return
        }

        const zip = (a, b) => a.map((k, i) => [k, b[i]]);
        const dict = Object.fromEntries(
            zip(
                fields.map(f => f.toLowerCase()), 
                values
            )
        )
        dict["Company"] = dict["company"]
        delete dict["company"]
        dict["action"] = parseInt(dict["action"])
        dict["quantity"] = parseInt(dict["quantity"])
        

        fetch(url, {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(dict)
        }).then((data) =>data.json()).then(data => {
            previewInventory(data)
            loadInventory()
        })
    })

    const maybeCompany = localStorage.getItem("company")
    if (maybeCompany !== null) {
        document.getElementById("company").value = maybeCompany
        loadInventory()
    }
</script>

<style>
    input, textarea {
        display: block;
        width: 400px;

        background-color: white;
        outline: none;
        border: 1px solid brown;
        padding: 4px;
        margin: 6px 0px;
        font-size: 18px;
    }

    hr {
        margin-top: 20px;
    }

    #existingInventory {
        display: flex;
        gap: 15px;
        margin-bottom: 15px;
    }

    button {
        background-color: #ffcc00;
        outline: none;
        border: 1px solid #ffcc00;
        color: black;
        padding: 6px;
        font-size: 18px;
        transition: all 0.1s linear;
    }

    button:hover {
        background-color: #aa8800;
        border: 1px solid #aa8800;
        transform: translateY(-5px);
    }

    #InventoryBox {
        display: flex;
        flex-direction: row;
        gap: 40px
    }

    #previewInventory > p {
        margin: 4px 0px;
        font-size: 18px;
    }
</style>
</html>