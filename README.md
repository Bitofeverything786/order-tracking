<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Bit of Everything - Track Your Order</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-image: url('https://cdn.prod.website-files.com/660e658d0cfb31720d8934d0/670946512c37ec199fe9368a_66bc68f879ce322f7885ca0a_shipment-tracking.webp');
      background-size: cover;
      background-position: center;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      color: #fff;
      flex-direction: column;
    }
    .tracking-box {
      background: rgba(255, 255, 255, 0.85);
      padding: 2rem;
      border-radius: 12px;
      max-width: 600px;
      width: 100%;
      box-shadow: 0 4px 10px rgba(0,0,0,0.2);
      text-align: center;
    }
    h1 {
      color: #333;
      font-size: 2rem;
      margin-bottom: 0.5rem;
      display: inline-block;
      margin-left: 15px;
    }
    h2 {
      color: #555;
      font-size: 1.5rem;
      margin-bottom: 2rem;
    }
    input {
      width: 80%;
      padding: 0.8rem;
      font-size: 1.2rem;
      margin-bottom: 1.5rem;
      border: 1px solid #ccc;
      border-radius: 8px;
      margin-top: 1rem;
    }
    button {
      padding: 0.8rem 1.5rem;
      font-size: 1.1rem;
      background-color: #007bff;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      width: 80%;
      transition: background-color 0.3s ease;
      margin-bottom: 1rem;
    }
    button:hover {
      background-color: #0056b3;
    }
    .result {
      margin-top: 1rem;
      font-size: 1.1rem;
      font-weight: bold;
      color: #333;
      text-align: left;
    }
    .error {
      color: red;
      font-weight: bold;
    }
    .success {
      color: green;
    }
    .developer-credit {
      font-size: 0.9rem;
      color: #888;
      text-align: center;
      margin-top: 2rem;
    }
    .logo {
      width: 120px;
      vertical-align: middle;
    }
    .header-container {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 2rem;
      flex-direction: column;
    }
    .icons {
      display: flex;
      justify-content: space-around;
      margin-top: 2rem;
    }
    .icons img {
      width: 40px;
      cursor: pointer;
    }
    .label {
      color: #007bff;
    }
    .no-underline {
      text-decoration: none;
      color: #007bff;
    }

    /* Mobile Optimizations */
    @media (max-width: 600px) {
      h1 {
        font-size: 1.6rem;
      }
      h2 {
        font-size: 1.2rem;
      }
      .tracking-box {
        padding: 1.5rem;
      }
      input, button {
        width: 90%;
      }
      .icons {
        display: block;
        text-align: center;
        margin-top: 1rem;
      }
      .icons img {
        margin: 0 10px;
        width: 35px;
      }
    }
  </style>
</head>
<body>

<div class="tracking-box">
  <div class="header-container">
    <a href="https://boe.com.pk/" target="_blank">
      <img src="https://boe.com.pk/cdn/shop/files/277800761_349678410512734_6677683408089721473_n.jpg?v=1729752325&width=535" alt="Bit of Everything Logo" class="logo">
    </a>
    <h1>Bit of Everything</h1>
  </div>

  <h2>Want to Know Your Order Status?</h2>
  
  <input type="text" id="orderInput" placeholder="Enter your order number">
  <button onclick="trackOrder()">Track Order</button>

  <div class="result" id="resultBox"></div>

  <div class="icons">
    <a href="https://www.facebook.com/bitofeverythingofficial" target="_blank">
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Facebook_f_logo_%282019%29.svg/512px-Facebook_f_logo_%282019%29.svg.png" alt="Facebook">
    </a>
    <a href="https://www.instagram.com/bitofeverythingofficial/?hl=en" target="_blank">
      <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Instagram_logo_2022.svg/512px-Instagram_logo_2022.svg.png" alt="Instagram">
    </a>
    <a href="https://www.youtube.com/@bitofeverything4190" target="_blank">
      <img src="https://upload.wikimedia.org/wikipedia/commons/4/42/YouTube_icon_%282013-2017%29.png" alt="YouTube">
    </a>
  </div>

  <div class="developer-credit">
    <p>Developed by Muhammad Adeel</p>
  </div>
</div>

<script>
// Fetch tracking data from JSON file
fetch('trackingData.json')
  .then(response => response.json())
  .then(data => {
    window.trackingData = data;
  })
  .catch(error => console.error('Error loading tracking data:', error));

// Track order by order number (__17)
function trackOrder() {
  const orderNumber = document.getElementById("orderInput").value.trim();
  const resultBox = document.getElementById("resultBox");

  if (orderNumber === "") {
    resultBox.innerHTML = "<p class='error'>Please enter an order number.</p>";
    return;
  }

  const order = window.trackingData.find(item => String(item["__17"]) === orderNumber);

  if (order) {
    const trackingNumber = order[""];
    const courierName = "Daewoo FastEx";
    const courierLink = "https://fastex.pk/";

    resultBox.innerHTML = `
      <p><span class="label">Tracking Number:</span> ${trackingNumber}</p>
      <p><span class="label">Courier:</span> <a href="${courierLink}" class="no-underline" target="_blank">${courierName}</a></p>
      <p><span class="label">Order Status:</span> ${order["__7"]}</p>
      <p><span class="label">Customer Name:</span> ${order["__18"]}</p>
      <p><span class="label">COD Amount:</span> ${order["__14"]}</p>
      <p><span class="label">Destination:</span> ${order["__10"]}</p>
      <p><span class="label">Customer Address:</span> ${order["__20"]}</p>
    `;
    resultBox.className = 'result';
  } else {
    resultBox.innerHTML = "<p class='error'>Order not found.</p>";
    resultBox.className = 'error';
  }
}
</script>

</body>
</html>
