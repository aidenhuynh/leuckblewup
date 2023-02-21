<html lang="en">
<head>
    <title>Inventory Management</title>
</head>
<body>
    <h1>Inventory Management</h1>
    <i>Enter the name of your company to get started</i>
    <input placeholder="Username" id="user" />
    <p>If you already have saved inventory, enter your company name above and hit "load inventory"</p>
    <button onclick="loadInventory()">Load Inventory</button>


    <hr />
    <div id="existingInventory">
    </div>
    <div id="inventoryBox">
        <div>
    <h2>Add inventory</h2>
    <form id="aninventory">
        <input placeholder="Inventory Name" id="inventory_name" />
        <input placeholder="Quantity" id="quantity" />
        <input placeholder="Price" id="price" />
        <input placeholder="Cost" id="cost" />
        <input placeholder="Delivery" id="delivery" />
        <textarea placeholder="Extra Notes" id="extra_notes"></textarea>
        <input type="submit" />
    </form>

</div>
<div id="previewInventory">
    <h2>Preview Inventory</h2>
    <p id="InventoryName">Inventory Name: N/A</p>
    <p id="Quantity">Quantity: N/A</p>
    <p id="Price">Price: N/A</p>
    <p id="Cost">Cost: N/A</p>
    <p id="Delivery">Delivery: N/A</p>
    <p id="ExtraNotes">Extra Notes: N/A</p>
    <button onclick="deleteInventory()">Delete Inventory</button>
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

    const loadInventory = () => {
        const user = document.getElementById("user").value
        if (user === "") {alert("Invalid company!"); return}
        try {
        fetch(url + new URLSearchParams({username: user})).then(data => data.json()).then(json => {
            document.getElementById("existingInventory").innerHTML = ""
            
            json.forEach(inventory => {
                const button = document.createElement("button")
                button.innerHTML = inventory.inventory_name
                inventoryLoader[inventory.id] = JSON.parse(JSON.stringify(inventory))
                button.onclick = () => previewInventory(inventoryLoader[inventory.id])
                document.getElementById("existingInventory").appendChild(button)
            })

            localStorage.setItem("user", user)
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

    document.getElementById("aninventory").addEventListener("submit", (e) => {
        e.preventDefault();
        e.stopImmediatePropagation();
        const fields = [
            "inventory_name",
            "user",
            "quantity",
            "price",
            "cost",
            "delivery",
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
        dict["username"] = dict["user"]
        delete dict["user"]
        dict["quantity"] = parseInt(dict["quantity"])
        dict["price"] = parseInt(dict["price"])
        dict["cost"] = parseInt(dict["cost"])
        dict["delivery"] = parseInt(dict["delivery"])

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

    const maybeUser = localStorage.getItem("user")
    if (maybeUser !== null) {
        document.getElementById("user").value = maybeUser
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



