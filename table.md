<style>
    table {
        table-layout:fixed
    }

    .star:hover {
        filter: brightness(95%);
        transition: filter 0.2s;
    }

    .star {
        cursor: pointer
    }

    .main td span {
        cursor: pointer;
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
        <th colspan="2" style="width:auto; text-align:left">Quantity</th>
        <td></td>
    </tr>
    <tr>
        <td><button style="width:140%" type="submit" id="addButton" onclick="addData()">Add</button></td>
        <td><input class="adders" style="width=150%" placeholder="MM-DD-YYYY" id="dateInput" ></td>
        <td><input class="adders" placeholder="Item Name" id="itemInput"></td>
        <td><input class="adders" placeholder="Action" id="actionInput"></td>
        <td><input class="adders" style="width:400%" placeholder="Quantity" id="quantityInput"></td>
        <td colspan="2" ></td>
    </tr>
    <tr><td></td><td colspan="6"><i id="textRow"></i></td></tr>
</table>

<i>Note: Click on items in the table to edit them.</i>
<table>
    <tbody id="bruh">
    <tr>
        <th style="width:auto"></th>
        <th style="width:15%">Date</th>
        <th style="width:15%">Item</th>
        <th style="width:13%">Action</th>
        <th style="width:auto; text-align:right">Quantity</th>
        <th></th>
    </tr>
    </tbody>
</table>
<button type="submit" onclick="clearStars();search(dataList)">Clear favorites</button>

<script>
const mainApi = "https://jasj-inventory.duckdns.org/api/mainData"
// const mainApi = "http://127.0.0.1:5000/api/mainData/"

var uid = "aidenhuynh"

var dataTypes = ["date", "item", "action", "quantity"]

var uidNum =""

const optionsGET = {
    mode: 'cors',
    method: 'GET'
}

const optionsDELETE =  {
    mode: 'cors',
    body: JSON.stringify(""),
    method: 'DELETE'
}

var dataList = []

var boxStatus = false

function getItems(list) {
    for (let i = 0; i < list.length; i++) {
        let starId = "Star: " + list[i]["id"]
        document.getElementById('bruh').innerHTML += '\
        <tr class="main"> \
            <td style="text-align:center"><img id="' + starId + `" class="star" onclick="favorite('` + starId + `')" src="images/graystar.png" height="40px" width="40px"></td>\
            <td id = "date_` + list[i]["id"] + `" onclick = "editData('date_` + list[i]["id"] + `')"><span id="datespan_` + list[i]["id"] + `">` + list[i]["date"] + `</span></td> \
            <td id = "item_` + list[i]["id"] + `" onclick = "editData('item_` + list[i]["id"] + `')"><span id="itemspan_` + list[i]["id"] + `">` + list[i]["item"] + `</span></td> \
            <td id = "action_` + list[i]["id"] + `" onclick = "editData('action_` + list[i]["id"] + `')"><span id="actionspan_` + list[i]["id"] + `">` + list[i]["action"] + `</span></td> \
            <td  id = "quantity_` + list[i]["id"] + `" class="quantity" onclick = "editData('quantity_` + list[i]["id"] + `')"><span id="quantityspan_` + list[i]["id"] + `">` + list[i]["quantity"] + `</span></td> \
            <td style="text-align:center"><img id="` + starId + `" class="star" onclick="dataDelete(` + list[i]["id"] + `)" src="images/deletebutton.png" height="15px"</td>
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
        <th style='width:auto; text-align:right'>Quantity</th> \
        <th></th> \
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
                <th style='width:auto; text-align:right'>Quantity</th> \
                <th></th> \
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
                    <th style='width:auto; text-align:right'>Quantity</th> \
                    <th></th> \
                </tr> \
                "
        
            for (let n = 0; n < favoritesList.length; n++) {
                var starId = localStorage.getItem(localStorage.key(n))


                document.getElementById('bruh').innerHTML += '\
                <tr> \
                    <td style="text-align:center"><img id="' + starId + `" class="star" onclick="favorite('` + starId + `')" src="images/star.png" height="40px" width="40px"></td>\
                    <td>` + list[n]["date"] + `</td> \
                    <td>` + list[n]["item"] + `</td> \
                    <td>` + list[n]["action"] + `</td> \
                    <td class="quantity">` + list[n]["quantity"] + `</td> \
                    <td style="text-align:center"><img id="` + starId + `" class="star" onclick="dataDelete(` + list[n]["id"] + `)" src="images/deletebutton.png" height="15px"</td>
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
                <th style='width:auto; text-align:right'>Quantity</th> \
                <th></th> \
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

function clearStars() {
    for (let i=0; i < localStorage.length; i++) {
        if (localStorage.getItem(localStorage.key(i)).includes("Star")) {
            localStorage.removeItem(i)
        }
    }
}

function isNumeric(str) {
  if (typeof str != "string") return false
  return !isNaN(str) && !isNaN(parseFloat(str))
}

function logStorage() {
    for (let i=0; i < localStorage.length; i++) {
        console.log(localStorage.key(i) + ": " + localStorage.getItem(localStorage.key(i)))
    }
}

function patternCheck(type, id) {
    value = document.getElementById(id).value

    if (type.includes("date")) {
        if (/^[0-9]{2}-[0-9]{2}-[0-9]{4}/.test(value) == false) {
            return alert("Invalid input: Date\n\nPlease follow the following format: MM-DD-YYYY"); false
        }
        else {
            return true
        }
    }
    else if (type.includes("item")) {
        if (value == "") {
            return alert("Invalid input: Item\n\nPlease do not leave the text box blank."); false
        }
        else {
            return true
        }
    }
    else if (type.includes("action")) {
        if (value == "") {
            return alert("Invalid input: Action\n\nPlease do not leave the text box blank."); false
        }
        else {
            return true
        }
    }
    else if (type.includes("quantity")) {
        if (isNumeric(value) == false) {
            return alert("Invalid input: Quantity\n\nPlease enter a number."); false
        }
        else {
            return true
        }
    }
}

function addData() {
    textBox = document.getElementById('textRow')
    id = dataList.slice(-1)[0]["id"] + 1
    date = document.getElementById('dateInput').value
    item = document.getElementById('itemInput').value
    action = document.getElementById('actionInput').value
    quantity = document.getElementById('quantityInput').value
    missingFields = []
    textBox.innerHTML = ""

    for (let i = 0; i < dataTypes.length; i++) {
        if (patternCheck(dataTypes[i], dataTypes[i] + "Input")) {
        }
        else {
            missingFields.push(dataTypes[i])
        }
    }

    if (missingFields.length == 1) {
        textBox.innerHTML += "<b>Invalid field: </b> " + missingFields[0]
    }
    else if (missingFields.length != 0){
        textBox.innerHTML = "<b>Invalid fields: </b>"

        for (let n = 0; n < missingFields.length - 1; n++) {
            textBox.innerHTML += missingFields[n] + ", "
        }
        textBox.innerHTML += "and " + missingFields[missingFields.length - 1] + "."
    }
    else {
        textBox.innerHTML = "Item successfully added"

        newData = {
                "id": id,
                "date": date,
                "action": action,
                "item": item,
                "quantity": quantity,
                }

        dataList.push(newData)

        fetch(mainApi+"PUT", {
            mode: 'cors',
            body: JSON.stringify([uidNum, newData]),
            method: 'PUT'
            }
        )

        search(dataList)
    }
}

function editData(itemId) {
    var item = document.getElementById(itemId)
    var type = ""
    var id = itemId.slice(itemId.indexOf("_")+1)
    var input = ""

    for (let i = 0; i < dataTypes.length; i++) {
        if (itemId.includes(dataTypes[i])) {
            type = dataTypes[i]
        }
    }

    if (item.innerHTML.slice(0, 6) != "<input") {
        for (let i = 0; i < dataList.length; i++) {
            if (dataList[i]["id"] == id) {
                var index = i
            }
        }

        item.innerHTML = "<input style='width:100%' placeholder='" + dataList[index][type] + "' id='" + itemId + "_input'>"

        input = document.getElementById(itemId + '_input')

        input.focus()
        input.addEventListener("keyup", function() {
            if (event.key === "Escape") {
                item.innerHTML = "<span>" + dataList[index][type] + "</span>"
            }
            else if (event.key === "Enter") {
                input = document.getElementById(itemId + '_input')
                if (patternCheck(type, itemId + '_input')) {
                    item.innerHTML = "<span>" + input.value + "</span>"

                    for (let i = 0; i < dataList.length; i++) {
                        if (dataList[i]["id"] == id) {
                            dataList[i][type] = input.value
                        
                            newData = {
                                "id": dataList[i]["id"],
                                "date": dataList[i]["date"],
                                "action": dataList[i]["action"],
                                "item": dataList[i]["item"],
                                "quantity": dataList[i]["quantity"],
                            }

                            fetch(mainApi+"PATCH", {
                                mode: 'cors',
                                body: JSON.stringify([uidNum, id, newData]),
                                method: 'PATCH'
                                }
                            )
                        }
                    }
                }
            }
        })
    }
}

function dataDelete(id) {
    console.log(id)
    fetch(mainApi+"DELETE", {
            mode: 'cors',
            body: JSON.stringify([uidNum, id]),
            method: 'DELETE'
            }
        )
    console.log(dataList)
    for (let i = 0; i < dataList.length; i++) {
        if (id == dataList[i]["id"]) {
            console.log(dataList[i] + "removed")
            dataList.splice(i, 1)
            console.log(dataList)
        }
    }
    search(dataList)
}

searchBar.addEventListener("keyup", function() {
            search(dataList)
        }
    )

function apiGet() {
    fetch(mainApi, optionsGET)
        .then(response => response.json().then(data => {
            for (let i = 0; i < data.length; i++) {
                if (data[i]["uid"] == uid) {
                    uidNum = i
                    dataList = data[i]["userData"]
                }
            }
            getItems(dataList)
            }
        )
    )
}

apiGet()
</script>