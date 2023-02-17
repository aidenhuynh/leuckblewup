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

<h1>Project Marketplace</h1>

<table id="myTable">
  <thead>
  <tr>
    <th>Date</th>
    <th>Product</th>
    <th>Cost</th>
    <th>Stock</th>
  </tr>
  </thead>
  <tbody id="inventory-table">
    <!-- javascript generated data -->
  </tbody>
</table>

<h2>Sell:</h2>

    <form action="javascript:create_market()">
        <p><label>
            Date:
            <input type="text" name="date" id="date" placeholder="MM/DD/YYYY" pattern="[0-9]{2}/[0-9]{2}/[0-9]{4}"required>
        </label></p>
        <p><label>
            Product:
            <input type="text" name="product" id="product" placeholder="Name" required>
        </label></p>
        <p><label>
            Cost:
            <input type="text" name="cost" id="cost" placeholder="xxxx.xx (in $)" pattern="[0-9]{4}.[0-9]{2}" required>
        </label></p>
        <p><label>
            Stock:
            <input type="text" name="stock" id="stock" pattern="[0-9]{0-9}" required>
        </label></p>
        <p>
            <button id="Create-Product-Button">Create Product</button>
        </p>
    </form>

  <h2>Buy:</h2>
    <form action="javascript:create_market()">
        <p><label>
            Date:
            <input type="text" name="buy date" id=" buy date" placeholder="MM/DD/YYYY" pattern="[0-9]{2}/[0-9]{2}/[0-9]{4}">
        </label></p>
        <p><label>
            Product:
            <input type="text" name="buy product" id="buy product" placeholder="Name" required>
        </label></p>
        <p><label>
            Cost:
            <input type="text" name="buy cost" id="buy cost" placeholder="xxxx.xx (in $)" pattern="[0-9]{4}.[0-9]{2}">
        </label></p>
        <p><label>
          Quantity:
          <input type="text" name="quantity" id="quantity" pattern="[0-9]{0-9}" required>
        </label></p>
        <p>
            <button>Buy Product</button>
        </p>
      </form>

  <script>
      // prepare HTML result container for new output
      const resultContainer = document.getElementById("result");
      // prepare URL's to allow easy switch from deployment and localhost
      //const url = "http://localhost:8086/api/users"
      const url = "https://github.com/aidenhuynh/blewupflask/blob/main/api/market.py"
      const create_fetch = url + '/create';
      const read_fetch = url + '/';
    
      // Load users on page entry
      read_user();
    
    
      // Display User Table, data is fetched from Backend Database
      function read_user() {
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
        const cost = document.createElement("td");
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

        var button = document.getElementById("Create-Product-Button");
        button.innerHTML = "Create Product";

        button.addEventListener ("click", function() {
          alert("Product Successfully Uploaded to DESMOS Marketplace!");
          var table = document.getElementById("myTable");
          var row = table.insertRow(0);
          var cell1 = row.insertCell(0);
          var cell2 = row.insertCell(1);
          var cell3 = row.insertCell(2);
          var cell4 = row.insertCell(3);
          cell1.innerHTML = "12/09/22";
          cell2.innerHTML = "jonathan";
          cell3.innerHTML = "1246.82";
          cell4.innerHTML = "8";
}); 
    </script>
</form>