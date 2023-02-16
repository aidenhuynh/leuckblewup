<style>
    table {
        table-layout:fixed
    }

    .quantity {
        text-align:right
    }

    td {
        padding:5px 10px;
    }

    input.search {
        color: #434343;
        border: 0px;
        margin-left: 2%;
        width: 86.7%;
        white-space: nowrap
    }

    input.check {
        margin-right: 2%
    }

    input.adders{
        width:100%
    }
</style>

# Inventory:
<label for="searchBar"> Search: <input class="search" name="searchBar" id="searchBar" placeholder="Enter item name here"></label>
<br><br>
<label><input type="checkbox" class="check" id="checkBox" onclick="showFavorites(dataList)"> Show favorites only</label>
<!-- checkbox for showing only favorites -->

<table>
    <tr>
        <th style="width:auto"></th>
        <th style="width:15%">Date</th>
        <th style="width:15%">Item</th>
        <th style="width:13%">Action</th>
        <th style="width:auto">User</th>
        <th style="width:auto; text-align:right">Quantity</th>
    </tr>
    <tr>
        <td><button style="width:100%" type="submit" id="addButton" onclick="addData()">Add</button></td>
        <td><input class="adders" placeholder="MM-DD-YYYY" id="dateInput" pattern="[0-9]{2}-[0-9]{2}-[0-9]{2}"></td>
        <td><input class="adders" placeholder="Item Name" id="itemInput"></td>
        <td><input class="adders" placeholder="Action" id="actionInput"></td>
        <td><input class="adders" placeholder="User" id="userInput"></td>
        <td><input class="adders" placeholder="Quantity" id="quantityInput"></td>
    </tr>
    <tr><td></td><td colspan="5"><i id="textRow"></i></td></tr>
</table>

<table>
    <tbody id="bruh">
    <tr>
        <th style="width:auto"></th>
        <th style="width:15%">Date</th>
        <th style="width:15%">Item</th>
        <th style="width:13%">Action</th>
        <th style="width:auto">User</th>
        <th style="width:auto; text-align:right">Quantity</th>
    </tr>
    </tbody>
</table>

<script>
// Notes for making this work when we have backend:

// change images
// fix createRow
// change all the "list"

const options = {
  method: 'GET',
};

dataList = [
    {
        "id":1,
        "date":"01-05-2023",
        "action":"Shipped",
        "user":"aidenhuynh",
        "item":"Pencils",
        "quantity":"1500",
    },
    {
        "id":2,
        "date":"02-07-2023",
        "action":"Delivered",
        "user":"TheGerbil21",
        "item":"Pens",
        "quantity":"1000",
    },
    {
        "id":3,
        "date":"02-02-2023",
        "action":"Packaged",
        "user":"aidenhuynh",
        "item":"Markers",
        "quantity":"300",
    },
    {
        "id":4,
        "date":"01-15-2023",
        "action":"In Transit",
        "user":"aidenhuynh",
        "item":"Highlighters",
        "quantity":"100",
    },
    {
        "id":5,
        "date":"01-05-2023",
        "action":"Shipped",
        "user":"aidenhuynh",
        "item":"Pencils",
        "quantity":"1500",
    },
    {
        "id":6,
        "date":"02-07-2023",
        "action":"Delivered",
        "user":"TheGerbil21",
        "item":"Pens",
        "quantity":"1000",
    },
    {
        "id":7,
        "date":"02-02-2023",
        "action":"Packaged",
        "user":"aidenhuynh",
        "item":"Markers",
        "quantity":"300",
    },
    {
        "id":8,
        "date":"01-15-2023",
        "action":"In Transit",
        "user":"aidenhuynh",
        "item":"Highlighters",
        "quantity":"100",
    },
    {
        "id":9,
        "date":"01-05-2023",
        "action":"Shipped",
        "user":"aidenhuynh",
        "item":"Pencils",
        "quantity":"1500",
    },
    {
        "id":10,
        "date":"02-07-2023",
        "action":"Delivered",
        "user":"TheGerbil21",
        "item":"Pens",
        "quantity":"1000",
    },
    {
        "id":11,
        "date":"02-02-2023",
        "action":"Packaged",
        "user":"aidenhuynh",
        "item":"Markers",
        "quantity":"300",
    },
    {
        "id":12,
        "date":"01-15-2023",
        "action":"In Transit",
        "user":"aidenhuynh",
        "item":"Highlighters",
        "quantity":"100",
    },
]

// fetch('https://pokeapi.co/api/v2/pokemon/', options)
//     .then(response => response.json().then(data => {
//     for (let i = 0; i < data.length; i++) {
//         dataList.push(data.sample[i])
//     }
//     }))
// update when there is backend and remove that ugly list of dictionaries

var boxStatus = false

function getItems(list) {
    for (let i = 0; i < list.length; i++) {
        let starId = "Star: " + list[i]["id"]
        document.getElementById('bruh').innerHTML += '\
        <tr> \
            <td style="text-align:center"><img id="' + starId + `" onclick="favorite('` + starId + `')" src="images/graystar.png" height="40px" width="40px"></td>\
            <td>` + list[i]["date"] + `</td> \
            <td>` + list[i]["item"] + `</td> \
            <td>` + list[i]["action"] + `</td> \
            <td>` + list[i]["user"] + `</td> \
            <td class="quantity">` + list[i]["quantity"] + `</td> \
        </tr> \
        `

        for (let a = 0; a < localStorage.length; a++) {
            if (localStorage.getItem(localStorage.key(a)) == starId) {
                console.log("Favorited from localStorage: " + starId)
                document.getElementById(starId).src = 'images/star.png'
            }
        }
    }
}



function search(list) {
    document.getElementById('bruh').innerHTML = " \
    <tr> \
        <th style='width:auto'></th> \
        <th style='width:15%'>Date</th> \
        <th style='width:15%'>Item</th> \
        <th style='width:13%'>Action</th> \
        <th style='width:auto'>User</th> \
        <th style='width:auto; text-align:right'>Quantity</th> \
    </tr> \
    "

    results = []
    input = document.getElementById('searchBar').value.toLowerCase()

    if (input == "" || input == null) {
        getItems(dataList)
    }
    else {
        for (let i = 0; i < list.length; i++) {
            item = list[i]["item"].toLowerCase()

            if (item.includes(input) == true) {
                results.push(list[i])
            }
        }
        if (results.length == 0) {
            document.getElementById('bruh').innerHTML = " \
            <tr> \
                <th style='width:auto'></th> \
                <th style='width:15%'>Date</th> \
                <th style='width:15%'>Item</th> \
                <th style='width:13%'>Action</th> \
                <th style='width:auto'>User</th> \
                <th style='width:auto; text-align:right'>Quantity</th> \
            </tr> \
            <tr><td></td><td colspan='5'><i>No results found.</i></td></tr> \
            "
            getItems(dataList)
        }
        else {
        getItems(results)
        }
    }
}

function favorite(star) {
    var checked = false

    for (var i = 0; i < localStorage.length; i++){
            if (localStorage.getItem(localStorage.key(i)) == star) {
                console.log("Star Removed: " + star.slice(6))
                document.getElementById(star).src = 'images/graystar.png'
                localStorage.removeItem(star)
                checked = true  
            }
        }

        if (checked == false) {
            console.log("Star Added: " + star.slice(6))
            document.getElementById(star).src = 'images/star.png'
            localStorage.setItem(star, star)
        }
    if (boxStatus == true) {
        boxStatus = false
        showFavorites(dataList)
    }
}

function showFavorites(list) {
    var favoritesList = []

    if (boxStatus == false) {
        console.log('box status was false')
        for (let i = 0; i < localStorage.length; i++) {
            for (let k = 0; k < list.length; k++) {
                if (localStorage.getItem(localStorage.key(i)).slice(6) == list[k]["id"]) {
                    favoritesList.push(list[k])
                    console.log(favoritesList)
                }
            }
        }

        if (favoritesList.length !== 0) {

            document.getElementById('bruh').innerHTML = " \
                <tr> \
                    <th style='width:auto'></th> \
                    <th style='width:15%'>Date</th> \
                    <th style='width:15%'>Item</th> \
                    <th style='width:13%'>Action</th> \
                    <th style='width:auto'>User</th> \
                    <th style='width:auto; text-align:right'>Quantity</th> \
                </tr> \
                "
        
            for (let n = 0; n < favoritesList.length; n++) {
                var starId = localStorage.getItem(localStorage.key(n))


                document.getElementById('bruh').innerHTML += '\
                <tr> \
                    <td style="text-align:center"><img id="' + starId + `" onclick="favorite('` + starId + `')" src="images/star.png" height="40px" width="40px"></td>\
                    <td>` + list[n]["date"] + `</td> \
                    <td>` + list[n]["item"] + `</td> \
                    <td>` + list[n]["action"] + `</td> \
                    <td>` + list[n]["user"] + `</td> \
                    <td class="quantity">` + list[n]["quantity"] + `</td> \
                </tr>`
            }
        }
        else {
            document.getElementById('bruh').innerHTML = " \
            <tr> \
                <th style='width:auto'></th> \
                <th style='width:15%'>Date</th> \
                <th style='width:15%'>Item</th> \
                <th style='width:13%'>Action</th> \
                <th style='width:auto'>User</th> \
                <th style='width:auto; text-align:right'>Quantity</th> \
            </tr> \
            <tr><td></td><td colspan='5'><i>No favorites found.</i></td></tr> \
            "
            getItems(dataList)
        }
    
    boxStatus = true
    }

    else {
        console.log('box status was true')
        search(dataList)
        boxStatus = false
    }
    
}

function logStorage() {
    for (let i=0; i < localStorage.length; i++) {
        console.log(localStorage.key(i) + ": " + localStorage.getItem(localStorage.key(i)))
    }
}

function dateCheck(input) {
    if (input.length == 10 && isNaN(input.slice(0, 2)) && isNaN(input.slice(3, 5)) && isNaN(input.slice(6, 10)) && input.slice(2,3) == "-" && input.slice(5, 6) == "-") {
        return true
    }
    else {
        return false
    }
}

function addData() {
    missingFields = []
    textBox = document.getElementById('textRow')
    id = dataList.length + 1
    date = document.getElementById('dateInput').value
    item = document.getElementById('itemInput').value
    action = document.getElementById('actionInput').value
    user = document.getElementById('userInput').value
    quantity = document.getElementById('quantityInput').value

    if (date == "" || item == "" || action == "" || user == "" || quantity == "") {
        textBox.innerHTML = "<b>Invalid field(s): </b>"

        if (date == "") {
            missingFields.push("date")
        }
        if (item == "") {
            missingFields.push("item")
        }
        if (action == "") {
            missingFields.push("action")
        }
        if (user == "") {
            missingFields.push("user")
        }
        if (quantity == "") {
            missingFields.push("quantity")
        }

        for (let i = 0; i < missingFields.length - 1; i++) {
            textBox.innerHTML += missingFields[i] + ", "
        }

        textBox.innerHTML += "and " + missingFields[missingFields.length - 1] + "."
    }
    else {
    textBox.innerHTML = "Item successfully added"

        dataList.push(
            {
            "id":id,
            "date":date,
            "action":action,
            "user":user,
            "item":item,
            "quantity":quantity,
            }
        )
    
        search(dataList)
    }


}

searchBar.addEventListener("keyup", function() {
            search(dataList)
        }
    )

getItems(dataList)
</script>