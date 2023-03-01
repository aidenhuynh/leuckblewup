<style>
    table {
        table-layout:auto;
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

<h2>DESMOS Marketplace</h2>

<table id="table1">
  <thead>
  <tr>
    <th>ID</th>
    <th>Date</th>
    <th>Product</th>
    <th>Stock</th>
    <th>Cost</th>
  </tr>
  <tr>
    <td>1677663382724</td>
    <td>02/18/2022</td>
    <td>Jonathan</td>
    <td>8</td>
    <td>1246.82</td>
  </tr>
  <tr>
    <td>1699432182724</td>
    <td>11/29/2018
    <td>PVC Pipes 4x10</td>
    <td>146</td>
    <td>18.95</td>
  </tr>
  <tr>
    <td>1786432115329</td>
    <td>02/05/2023
    <td>Decorative Confetti (20 g)</td>
    <td>28</td>
    <td>5.60</td>
  </tr>
  </thead>
  <tbody id="result">
    <!-- javascript generated data -->
  </tbody>
</table>

<h3>Log Inventory Details:</h3>
<form action="javascript:create_user()">
    <p><label>
        Date:
        <input type="text" name="date" id="date" placeholder="xx/xx/xxxx" pattern = "[0-9]{2}/[0-9]{2}/[0-9]{4}" required>
    </label></p>
    <p><label>
        Product:
        <input type="text" name="product" id="product" placeholder="product description" required>
    </label></p>
    <p><label>
        Stock:
        <input type="text" name="stock" id="stock" placeholder="# of product" pattern="[0-9]{1-5}" required>
    </label></p>
    <p><label>
        Cost:
        <input type="text" name="cost" id="cost" placeholder="xxxx.xx (in $)" pattern = "[0-9]{1-4}.[0-9]{2}" required>
    </label></p>
    <p>
        <button onclick="myFunction()">Create Product</button>
    </p>
</form>


<script>
  // prepare HTML result container for new output
  const resultContainer = document.getElementById("result");
  // prepare URL's to allow easy switch from deployment and localhost
  var url = "https://jasj-inventory.duckdns.org/api/market"
  //url = "http://localhost:8086/api/users"
  

  // const create_fetch = url + '/create';
  // const read_fetch = url + '/';

  // Load users on page entry
  read_users();


  // Display User Table, data is fetched from Backend Database
  function read_users() {
    // prepare fetch options
    const read_options = {
      method: 'GET', // *GET, POST, PUT, DELETE, etc.
      mode: 'cors', // no-cors, *cors, same-origin
      cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
      credentials: 'omit', // include, *same-origin, omit
      headers: {
        'Content-Type': 'application/json'
      },
    };

    // fetch the data from API
    fetch(read_fetch, read_options)
      // response is a RESTful "promise" on any successful fetch
      .then(response => {
        // check for response errors
        if (response.status !== 200) {
            const errorMsg = 'Database read error: ' + response.status;
            console.log(errorMsg);
            const tr = document.createElement("tr");
            const td = document.createElement("td");
            td.innerHTML = errorMsg;
            tr.appendChild(td);
            resultContainer.appendChild(tr);
            return;
        }
        // valid response will have json data
        response.json().then(data => {
            console.log(data);
            for (let row in data) {
              console.log(data[row]);
              add_row(data[row]);
            }
        })
    })
    // catch fetch errors (ie ACCESS to server blocked)
    .catch(err => {
      console.error(err);
      const tr = document.createElement("tr");
      const td = document.createElement("td");
      td.innerHTML = err;
      tr.appendChild(td);
      resultContainer.appendChild(tr);
    });
  }

  function create_user(){
    //Validate Password (must be 6-20 characters in len)
    //verifyPassword("click");
    const body = {
        date: document.getElementById("date").value,
        product: document.getElementById("product").value,
        cost: document.getElementById("cost").value,
        stock: document.getElementById("stock").value
    };
    const requestOptions = {
        method: 'POST',
        body: JSON.stringify(body),
        headers: {
            "content-type": "application/json",
            'Authorization': 'Bearer my-token',
        },
    };

    // URL for Create API
    // Fetch API call to the database to create a new user
    fetch(create_fetch, requestOptions)
      .then(response => {
        // trap error response from Web API
        if (response.status !== 200) {
          const errorMsg = 'Database create error: ' + response.status;
          console.log(errorMsg);
          const tr = document.createElement("tr");
          const td = document.createElement("td");
          td.innerHTML = errorMsg;
          tr.appendChild(td);
          resultContainer.appendChild(tr);
          return;
        }
        // response contains valid result
        response.json().then(data => {
            console.log(data);
            //add a table row for the new/created userid
            add_row(data);
        })
    })
  }

  function add_row(data) {
    const tr = document.createElement("tr");
    const date = document.createElement("td");
    const product = document.createElement("td");
    const cost = document.createElement("td")
    const stock = document.createElement("td");
  

    // obtain data that is specific to the API
    date.innerHTML = data.date; 
    product.innerHTML = data.product; 
    cost.innerHTML = data.cost;
    stock.innerHTML = data.stock;  

    // add HTML to container
    tr.appendChild(date);
    tr.appendChild(product);
    tr.appendChild(cost);
    tr.appendChild(stock);

    resultContainer.appendChild(tr);
  }

  function myFunction() {
  var table = document.getElementById("table1");
  var row = table.insertRow(-1);
  var cell1 = row.insertCell(0);
  var cell2 = row.insertCell(1);
  var cell3 = row.insertCell(2);
  var cell4 = row.insertCell(3);
  var cell5 = row.insertCell(4);
  cell1.innerHTML = Date.now();
  cell2.innerHTML = document.getElementById('date').value;
  cell3.innerHTML = document.getElementById('product').value;
  cell4.innerHTML = document.getElementById('stock').value;
  cell5.innerHTML = document.getElementById('cost').value;
}

</script>