<style>
    td {
        table-layout:fixed;
        padding:5px 10px;
    }

    .gridrow {
        width:30px;
        height:30px;
        text-align:center;
        border: 1px solid #434343; /* I tried to use sass here but it didn't work :( */
        border-top: 1px solid #434343;
        border-bottom: 1px solid #434343;
        padding:5px 5px;

    }

    .outputtd {
        width:auto
    }

    input.search {
        color: #434343;
        border: 0px;
        margin-left: 2%;
        width: 80%;
        white-space: nowrap
    }

    input.check {
        margin-right: 2%
    }
</style>

## Crafting Recipes
<label for="searchBar"> Search: <input class="search" name="searchBar" id="searchBar" placeholder="Enter item name here"></label>
<br><br>
<label><input type="checkbox" class="check" id="checkBox" onclick="showFavorites(dataList)"> Show favorites only</label>
<!-- checkbox for showing only favorites -->

<table>
    <tbody id="bruh">
    <tr>
        <th></th>
        <th>Date</th>
        <th>Action</th>
        <th>User</th>
        <th>Item</th>
        <th>Quantity</th>
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
        "date":"01-05-2023",
        "action":"Shipped",
        "user":"aidenhuynh",
        "item":"Pencils",
        "quantity":"1500",
    },
    {
        "date":"02-07-2023",
        "action":"Delivered",
        "user":"TheGerbil21",
        "item":"Pens",
        "quantity":"1000",
    },
    {
        "date":"02-02-2023",
        "action":"Packaged",
        "user":"aidenhuynh",
        "item":"Markers",
        "quantity":"300",
    },
    {
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
// update when there is backend

var boxStatus = false

function getRecipes(list) {
    for (let i = 0; i < list.length; i++) {
        let starId = "Star: " + list[i]["item"]
        document.getElementById('bruh').innerHTML += '\
        <tr> \
            <td style="text-align:center"><img id="' + starId + `" onclick="favorite('` + starId + `')" 'src="images/graystar.png" height="50px" width="auto"></td>\
            <td>` + list[i]["date"] + `</td> \
            <td>` + list[i]["item"] + `</td> \
            <td>` + list[i]["action"] + `</td> \
            <td>` + list[i]["user"] + `</td> \
            <td>` + list[i]["quantity"] + `</td> \
        </tr> \
        `

    for (let a = 0; a < localStorage.length; a++) {
        if (localStorage.getItem(localStorage.key(a)) == starId) {
            console.log("Favorited from localStorage: " + starId)
            document.getElementById(starId).src = 'images/star.webp'
        }
    }
}
}

function search(list) {
    document.getElementById('bruh').innerHTML = " \
    <tr> \
        <th>Item</th> \
        <th colspan='3'>Recipe </th> \
        <th colspan='3'></th> \
        <th>Output</th> \
    </tr> \
    "

    results = []
    input = document.getElementById('searchBar').value.toLowerCase()

    if (input == "" || input == null) {
        getRecipes(dataList)
    }
    else {
        for (let i = 0; i < list.length; i++) {
            item = list[i]["item"].toLowerCase()

            if (item.includes(input) == true) {
                results.push(list[i])
            }
        }
        if (results.length == 0) {
            document.getElementById('bruh').innerHTML = "\
            <tr> \
            <th>Item</th> \
            <th colspan='3'>Recipe </th> \
            <th colspan='3'></th> \
            <th>Output</th> \
            </tr> \
            <tr><td colspan='8'><i>No results found.</i></td></tr> \
            "
            getRecipes(dataList)
        }
        else {
        getRecipes(results)
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
                if (localStorage.getItem(localStorage.key(i)).slice(6) == list[k]["item"]) {
                    favoritesList.push(list[k])
                    console.log(favoritesList)
                }
            }
        }

        if (favoritesList.length !== 0) {

            document.getElementById('bruh').innerHTML = " \
                <tr> \
                    <th></th> \
                    <th>Date</th> \
                    <th>Action</th> \
                    <th>User</th> \
                    <th>Item</th> \
                    <th>Quantity</th> \
                </tr> \
                "
        
            for (let n = 0; n < favoritesList.length; n++) {
                var starId = localStorage.getItem(localStorage.key(n))


                document.getElementById('bruh').innerHTML += '\
                <tr> \
                    <td style="text-align:center"><img id="' + starId + `" onclick="favorite('` + starId + `')" 'src="images/graystar.png" height="50px" width="auto"></td>\
                    <td>` + list[i]["date"] + `</td> \
                    <td>` + list[i]["item"] + `</td> \
                    <td>` + list[i]["action"] + `</td> \
                    <td>` + list[i]["user"] + `</td> \
                    <td>` + list[i]["quantity"] + `</td> \
                </tr>`
            }
        }
        else {
            document.getElementById('bruh').innerHTML = "\
            <tr> \
            <th>Item</th> \
            <th colspan='3'>Recipe </th> \
            <th colspan='3'></th> \
            <th>Output</th> \
            </tr> \
            <tr><td colspan='8'><i>No favorites found.</i></td></tr> \
            "
            getRecipes(dataList)
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

searchBar.addEventListener("keyup", function() {
            search(dataList)
        }
    )

getRecipes(dataList)
</script>