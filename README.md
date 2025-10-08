# Catalogo-La-Meleria.
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Catálogo de Golosinas</title>
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@700&family=Roboto:wght@400;500&display=swap" rel="stylesheet">

<style>
body {
    font-family: 'Roboto', sans-serif;
    background-color: #FFF8F5;
    margin: 0;
    padding: 0;
}
h1 {
    font-family: 'Playfair Display', serif;
    text-align: center;
    color: #FF6F91;
    margin: 25px 0;
    font-size: 2.5em;
}
.container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 25px;
    padding: 20px;
}
.product {
    background: linear-gradient(145deg, #FFD3E2, #FFF0F5);
    border-radius: 20px;
    padding: 15px;
    width: 220px;
    text-align: center;
    box-shadow: 0 8px 15px rgba(0,0,0,0.1);
    transition: transform 0.2s;
}
.product:hover {
    transform: translateY(-5px);
}
.product img {
    width: 100%;
    border-radius: 15px;
    margin-bottom: 10px;
}
.product h2 {
    font-family: 'Playfair Display', serif;
    font-size: 1.3em;
    color: #FF6F91;
    margin: 10px 0 5px 0;
}
.product p {
    font-weight: 500;
    color: #333;
    margin: 5px 0;
}
.quantity input {
    width: 50px;
    text-align: center;
    border-radius: 8px;
    border: 1px solid #ccc;
    padding: 5px;
    font-size: 1em;
}
button.add {
    background-color: #FFB347;
    border: none;
    border-radius: 12px;
    padding: 10px 15px;
    color: white;
    cursor: pointer;
    font-weight: bold;
    margin-top: 10px;
    transition: background 0.2s;
}
button.add:hover {
    background-color: #FF9A4D;
}
.cart {
    position: fixed;
    top: 20px;
    right: 20px;
    background-color: #C1F0F6;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 8px 15px rgba(0,0,0,0.15);
    max-width: 300px;
    width: 90%;
}
.cart h3 {
    margin-top: 0;
    color: #FF6F91;
    font-family: 'Playfair Display', serif;
}
.cart ul {
    list-style: none;
    padding: 0;
    margin: 0;
    max-height: 200px;
    overflow-y: auto;
}
.cart li {
    margin: 8px 0;
    font-weight: 500;
    color: #333;
}
.cart button {
    background-color: #FF6F91;
    border: none;
    border-radius: 12px;
    padding: 12px;
    color: white;
    font-weight: bold;
    cursor: pointer;
    width: 100%;
    margin-top: 15px;
    transition: background 0.2s;
}
.cart button:hover {
    background-color: #FF497A;
}
</style>
</head>
<body>

<h1>🍬 Catálogo de Golosinas 🍬</h1>

<div class="container" id="product-list"></div>

<div class="cart">
    <h3>Carrito</h3>
    <ul id="cart-items"></ul>
    <p id="total">Total: $0</p>
    <button onclick="sendWhatsApp()">Enviar pedido por WhatsApp</button>
</div>

<script>
let cart = [];
let products = JSON.parse(localStorage.getItem('productos')) || [];

function renderProducts() {
    const list = document.getElementById('product-list');
    list.innerHTML = '';
    if (products.length === 0) {
        list.innerHTML = '<p>No hay productos disponibles.</p>';
        return;
    }
    products.forEach((p, i) => {
        list.innerHTML += `
        <div class="product">
            <img src="${p.imagen}" alt="${p.nombre}">
            <h2>${p.nombre}</h2>
            <p>$${p.precio} / 100g</p>
            <div class="quantity">
                <input type="number" id="qty${i}" value="1" min="1">
            </div>
            <button class="add" onclick="addToCart('${p.nombre}', ${p.precio}, document.getElementById('qty${i}').value)">Agregar al carrito</button>
        </div>`;
    });
}

function addToCart(name, price, qty) {
    qty = parseInt(qty);
    if(qty <= 0) return;
    const existing = cart.find(item => item.name === name);
    if(existing){
        existing.qty += qty;
    } else {
        cart.push({name, price, qty});
    }
    updateCart();
}

function updateCart() {
    const cartItems = document.getElementById('cart-items');
    cartItems.innerHTML = '';
    let total = 0;
    cart.forEach(item => {
        cartItems.innerHTML += `<li>${item.name} x ${item.qty} = $${item.price * item.qty}</li>`;
        total += item.price * item.qty;
    });
    document.getElementById('total').innerText = `Total: $${total}`;
}

function sendWhatsApp() {
    if(cart.length === 0){
        alert('El carrito está vacío');
        return;
    }
    let message = '¡Hola! Quiero realizar el siguiente pedido:\n';
    cart.forEach(item => {
        message += `- ${item.name} x ${item.qty} = $${item.price * item.qty}\n`;
    });
    let total = cart.reduce((sum, item) => sum + item.price * item.qty, 0);
    message += `Total: $${total}`;
    let phone = '54911XXXXXXX'; // Cambiar por tu número
    let url = `https://api.whatsapp.com/send?phone=${phone}&text=${encodeURIComponent(message)}`;
    window.open(url, '_blank');
}

renderProducts();
</script>
</body>
</html>
