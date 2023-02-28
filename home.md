<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale = 1.0">
        <meta http-equiv="X-UA-Compatible" content = "ie=edge">
        <title>DESMOS Marketplace</Title>
        <style>
        </style>
    </head>
    <body>
        <h3>Sell Products:</h3>
        <form>
            <div class = "formBox">
                <label for="product">Product</label>
                <input type="text" id="product" placeholder="Product Description"/>
            </div>
            <div class = "formBox">
                <label for="cost">Cost</label>
                <input type="text" id="cost" placeholder="xxxx.xx (in $)"/>
            </div>
            <div class = "formBox">
                <label for="stock">Stock</label>
                <input type="number" id="stock" placeholder="# of product"/>
            </div>
            <div class = "formBox">
                <button id="btn">Add Product</button>
            </div>
            <div id = "msg">
                <pre></pre>
            </div>
        </form>
        <h3>Purchase Products:</h3>
        <form>
            <div class = "formBox">
                <label for="bproduct">Product</label>
                <input type="text" id="bproduct" placeholder="Product Name as Displayed Below"/>
            </div> 
            <div class = "formBox">
                <label for="bstock">Quantity</label>
                <input type="number" id="bstock" placeholder="# of products"/>
            </div> 
            <div class = "formBox">
                <button id="buybtn">Purchase Product</button>
            </div>
        </form>
        <script>
            let products = [];

            const addProduct = (ev)=>{
                ev.preventDefault();
                let product = {
                    id: Date.now(),
                    product: document.getElementById('product').value,
                    cost: document.getElementById('cost').value,
                    stock: document.getElementById('stock').value,
                }
            
                products.push(product);
                document.forms[0].reset();

                console.warn('added' , {products} );
                let pre = document.querySelector('#msg pre');
                pre.textContent = '\n' + JSON.stringify(products, '\t', 2);

                localStorage.setItem('MyProductList', JSON.stringify(products));
            }

            const buyProduct = (ev)=>{
                ev.preventDefault();
                let product = {
                    product: document.getElementById('bproduct').value,
                    revenue: document.getElementById('cost').value,
                    stock: document.getElementById('stock').value - document.getElementById('bstock').value,
                }
            
                products.push(product);
                document.forms[0].reset();

                console.warn('added' , {products} );
                let pre = document.querySelector('#msg pre');
                pre.textContent = '\n' + JSON.stringify(products, '\t', 2);

            }

            document.addEventListener('DOMContentLoaded', ()=>{
                document.getElementById('btn').addEventListener('click', addProduct);
            });
            document.addEventListener('DOMContentLoaded', ()=>{
                document.getElementById('buybtn').addEventListener('click', buyProduct);
            });
        </script>
    </body>
</html>