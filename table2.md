<html lang="en">
<head>
    <title>Inventory Management</title>
</head>
<body>
    <h1>Inventory Management</h1>
    <i>Enter the name of your company to get started</i>
    <input placeholder="Username" id="user" />
    <p>If you already have saved inventory, enter your company name above and hit "load inventory"</p>
    <button onclick="loadInventories()">Load Inventory</button>
    <table class="teams" border="1">
      <tr>
        <th>ID</th>
        <th>Company</th>
        <th>Quantity</th>
        <th>Price</th>
        <th>Cost</th>
        <th>Delivery</th>
        <th>Extra Notes</th>
        <th>Inventory Name</th>
      </tr>
    </table>
    <hr />
    <div id="existingInventories">
    </div>
    <div id="inventoryBox">
        <div>
    <h2>Add inventory</h2>
    <form id="aninventory">
        <input required placeholder="Username" id="username" />
        <input required placeholder="Quantity" id="quantity" />
        <input required placeholder="Inventory Name" id="inventory_name" />
        <input required placeholder="Price" id="price" />
        <input required placeholder="Cost" id="cost" />
        <input required placeholder="Delivery" id="delivery" />
        <textarea required placeholder="Extra Notes" id="extra_notes"></textarea>
        <button type="submit" onclick="addInventory()">Add Inventory</button>
    </form>

</div>
</div>


</body>

<script>
    let inventoryLoader = {}
    let currentInventory = -1
    // change in AWS
    const url = ""

    const previewInventory = (inventory) => {
        document.getElementById("InventoryName").innerHTML = "Inventory Name: " + inventory.inventory_name
        document.getElementById("Quantity").innerHTML = "Quantity: " + inventory.quantity
        document.getElementById("Price").innerHTML = "Price: " + inventory.price + " dollars"
        document.getElementById("Cost").innerHTML = "Cost: " + inventory.cost + " dollars"
        document.getElementById("Delivery").innerHTML = "Delivery: " + inventory.delivery + " days"
        document.getElementById("ExtraNotes").innerHTML = "Extra Notes:\n" + inventory.extra_notes
        currentInventory = inventory.id
    }

    const addInventory = () => {
        const username = document.getElementById("username").value
        const quantity = document.getElementById("quantity").value
        const inventory_name = document.getElementById("inventory_name").value
        const price = document.getElementById("price").value
        const cost = document.getElementById("cost").value
        const delivery = document.getElementById("delivery").value
        const extra_notes = document.getElementById("extra_notes").value

        if (user === "") {alert("Invalid company!"); return}
        data = {username: username, quantity: quantity, inventory_name: inventory_name, price: price, cost: cost, delivery: delivery, extra_notes: extra_notes}
        var requestOptions = {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify(data)
        };
        try {
            fetch(`http://localhost:8086/inventory`, requestOptions)
            .then(response => response.json())
            .then(data => {
                console.log("Success");
                alert("Added Inventory");
            })
            .catch(error => console.error(error));
        } catch (e) {
            console.log(e);
        }
    }

    const loadInventories = () => {
        const teams = document.querySelector(".teams");
        var rowCount = teams.rows.length;
        for (var i = rowCount - 1; i >= 1; i--) {
            teams.deleteRow(i);
        }
        const user = document.getElementById("user").value
        if (user === "") {alert("Invalid company!"); return}
        try {
            fetch(`http://localhost:8086/inventory?username=` + user)
            .then(response => response.json())
            .then(data => {
                if (data.length > 0) {
                    data.forEach((data) => {
                    teams.innerHTML += `
                <tr>
                <td>${data.id}</td>
                    <td>${data.username}</td>
                    <td>${data.quantity}</td>
                    <td>${data.price}</td>
                    <td>${data.cost}</td>
                    <td>${data.delivery}</td>
                    <td>${data.extra_notes}</td>
                    <td>${data.inventory_name}</td>
                </tr>`;
                });
                } else {
                    alert("Invalid Name");
                }
            })
            .catch(error => console.error(error));
        } catch (e) {
            console.log(e);
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
            loadInventories()
        })
    }


    const maybeUser = localStorage.getItem("user")
    if (maybeUser !== null) {
        document.getElementById("user").value = maybeUser
        loadInventories()
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

    #existingInventories {
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

    #inventoryBox {
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



