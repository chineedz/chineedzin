<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ChineeDZ Store</title>

<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<style>

body{
font-family:Arial;
background:#fff;
}

.product-card{
border:1px solid #eee;
border-radius:12px;
padding:8px;
transition:0.3s;
}

.product-card:hover{
transform:translateY(-6px);
box-shadow:0 10px 20px rgba(0,0,0,0.08);
}

.btn{
background:black;
color:white;
padding:14px;
border-radius:8px;
font-weight:bold;
width:100%;
}

</style>

</head>
<body class="max-w-lg mx-auto p-4">

<h1 class="text-2xl font-black mb-6 text-center">ChineeDZ</h1>

<input id="searchBar" placeholder="Search..." class="border p-3 w-full rounded-lg mb-6">

<div id="products" class="grid grid-cols-2 gap-4"></div>

<hr class="my-8">

<h2 class="font-bold mb-4">Shopping Cart</h2>

<div id="cart"></div>

<p class="mt-4 font-bold">Total: <span id="total">0</span> DA</p>

<hr class="my-8">

<h2 class="font-bold mb-4">Order Info</h2>

<input id="name" placeholder="Full Name" class="border p-3 w-full rounded mb-3">

<input id="phone" placeholder="Phone" class="border p-3 w-full rounded mb-3">

<select id="delivery" class="border p-3 w-full rounded mb-3">
<option value="600">Office Delivery (600 DA)</option>
<option value="800">Home Delivery (800 DA)</option>
</select>

<button onclick="sendOrder()" class="btn">Confirm Order</button>

<script>

const products = [

{id:1,name:"Ensemble ALO",price:5500,img:"https://i.postimg.cc/KYryjMWw/IMG_20260210_225601_843.jpg"},
{id:2,name:"Ensemble Polo",price:5900,img:"https://i.postimg.cc/rwBs656R/IMG_20260210_222331_180.jpg"},
{id:3,name:"Baggy Streetwear",price:2900,img:"https://i.postimg.cc/qMW56KDm/IMG_20260215_200828_193.jpg"},
{id:4,name:"Veste Zipper Grey",price:4500,img:"https://i.postimg.cc/kXLpRSYQ/IMG_20260215_200825_775.jpg"},
{id:5,name:"Classic Black T-shirt",price:1800,img:"https://i.postimg.cc/kGTGLZCL/IMG_20260210_222158_000.jpg"}

];

let cart=[];

function render(list=products){

const container=document.getElementById("products");

container.innerHTML="";

list.forEach(p=>{

container.innerHTML+=`

<div class="product-card">

<img src="${p.img}" class="rounded mb-2">

<h3 class="font-bold text-sm">${p.name}</h3>

<p class="text-sm font-black mb-2">${p.price} DA</p>

<button onclick="addToCart(${p.id})" class="btn text-sm">Add</button>

</div>

`;

});

}

function addToCart(id){

const prod=products.find(p=>p.id===id);

cart.push(prod);

updateCart();

}

function updateCart(){

const cartDiv=document.getElementById("cart");

cartDiv.innerHTML="";

let total=0;

cart.forEach((item,index)=>{

total+=item.price;

cartDiv.innerHTML+=`

<div class="flex justify-between items-center border p-2 rounded mb-2">

<span class="text-sm">${item.name}</span>

<div class="flex gap-3 items-center">

<span class="font-bold">${item.price}</span>

<button onclick="removeItem(${index})">

<i class="fa fa-trash text-red-500"></i>

</button>

</div>

</div>

`;

});

document.getElementById("total").innerText=total;

}

function removeItem(i){

cart.splice(i,1);

updateCart();

}

document.getElementById("searchBar").addEventListener("input",function(){

const val=this.value.toLowerCase();

const filtered=products.filter(p=>p.name.toLowerCase().includes(val));

render(filtered);

});

async function sendOrder(){

if(cart.length===0){

alert("Cart empty");

return;

}

const name=document.getElementById("name").value;

const phone=document.getElementById("phone").value;

const delivery=document.getElementById("delivery").value;

let total=cart.reduce((sum,i)=>sum+i.price,0);

total+=Number(delivery);

let list="";

cart.forEach(p=>{

list+=`• ${p.name} - ${p.price} DA\n`;

});

const message=

`NEW ORDER

Products:
${list}

Total: ${total} DA

Name: ${name}
Phone: ${phone}
Delivery: ${delivery} DA`;

await fetch("https://api.telegram.org/botYOUR_BOT_TOKEN/sendMessage",{

method:"POST",

headers:{"Content-Type":"application/json"},

body:JSON.stringify({

chat_id:"YOUR_CHAT_ID",

text:message

})

});

alert("Order Sent!");

cart=[];

updateCart();

}

render();

</script>

</body>
</html>
