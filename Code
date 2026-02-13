<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>RNG Meme Spinner</title>
<style>
  body { font-family: Arial, sans-serif; background: #111; color: #fff; text-align: center; }
  #spinner { margin: 50px auto; width: 300px; height: 80px; overflow: hidden; border: 2px solid #fff; border-radius: 10px; position: relative; }
  .item-strip { display: flex; position: absolute; top: 0; left: 0; }
  .item { width: 80px; height: 80px; line-height: 80px; font-size: 16px; border-right: 1px solid #555; background: #222; }
  #result { margin-top: 20px; }
  #history { margin-top: 30px; max-width: 90%; margin-left: auto; margin-right: auto; text-align: left; }
  #skipBtn { margin-top: 20px; padding: 10px 20px; font-size: 16px; cursor: pointer; }
  img { max-width: 100px; margin-top: 10px; border: 2px solid #fff; border-radius: 5px; }
</style>
</head>
<body>

<h1>RNG Meme Spinner</h1>

<div id="spinner">
  <div class="item-strip" id="itemStrip"></div>
</div>

<button id="spinBtn">Spin</button>
<button id="skipBtn">Skip Roll</button>

<div id="result"></div>
<div id="history"><h3>History:</h3><ul id="historyList"></ul></div>

<script>
const items = [
  {name: "Meme A", image: "https://via.placeholder.com/100?text=Meme+A", rarity: 0.5},  // 50%
  {name: "Meme B", image: "https://via.placeholder.com/100?text=Meme+B", rarity: 0.3},  // 30%
  {name: "Meme C", image: "https://via.placeholder.com/100?text=Meme+C", rarity: 0.1},  // 10%
  {name: "Meme D", image: "https://via.placeholder.com/100?text=Meme+D", rarity: 0.09}, // 9%
  {name: "Meme E", image: "https://via.placeholder.com/100?text=Meme+E", rarity: 0.01}  // 1%
];

let history = [];
let spinning = false;
let animationFrame;
let stripPos = 0;

// Generate the spinning strip with multiple copies of items for animation
function generateStrip() {
  const strip = document.getElementById("itemStrip");
  strip.innerHTML = "";
  for (let i = 0; i < 50; i++) { // repeat to simulate spin
    const item = items[Math.floor(Math.random()*items.length)];
    const div = document.createElement("div");
    div.className = "item";
    div.textContent = item.name;
    strip.appendChild(div);
  }
}

// RNG function based on rarity
function rollItem() {
  const rand = Math.random();
  let cumulative = 0;
  for (const item of items) {
    cumulative += item.rarity;
    if (rand <= cumulative) return item;
  }
  return items[0]; // fallback
}

// Animate the spin
function animateSpin(targetIndex, duration = 3000) {
  const strip = document.getElementById("itemStrip");
  const totalItems = strip.children.length;
  const itemWidth = strip.children[0].offsetWidth;
  const finalPos = -(targetIndex * itemWidth);

  const start = performance.now();
  spinning = true;

  function step(time) {
    const elapsed = time - start;
    const progress = Math.min(elapsed / duration, 1);
    // ease out effect
    const eased = 1 - Math.pow(1 - progress, 3);
    strip.style.left = stripPos + (finalPos - stripPos) * eased + "px";

    if (progress < 1 && spinning) {
      animationFrame = requestAnimationFrame(step);
    } else {
      spinning = false;
      stripPos = parseFloat(strip.style.left) || 0;
      showResult(targetIndex);
    }
  }

  animationFrame = requestAnimationFrame(step);
}

// Show the result and update history
function showResult(targetIndex) {
  const strip = document.getElementById("itemStrip");
  const itemName = strip.children[targetIndex].textContent;
  const itemData = items.find(i => i.name === itemName);

  document.getElementById("result").innerHTML = `
    <h2>You got: ${itemName}</h2>
    <img src="${itemData.image}" alt="${itemName}">
  `;

  history.push(itemData);
  // sort by rarity descending
  const sorted = [...history].sort((a,b)=> b.rarity - a.rarity);
  const historyList = document.getElementById("historyList");
  historyList.innerHTML = "";
  sorted.forEach(i => {
    const li = document.createElement("li");
    li.textContent = i.name;
    historyList.appendChild(li);
  });
}

// Spin button
document.getElementById("spinBtn").addEventListener("click", () => {
  if (spinning) return;
  generateStrip();
  const resultItem = rollItem();

  // pick a random position of the item in the strip
  const strip = document.getElementById("itemStrip");
  let targetIndex = Array.from(strip.children).findIndex(c => c.textContent === resultItem.name);
  if (targetIndex < 0) targetIndex = 0;

  animateSpin(targetIndex);
});

// Skip roll button
document.getElementById("skipBtn").addEventListener("click", () => {
  if (!spinning) return;
  spinning = false;
  cancelAnimationFrame(animationFrame);
  const strip = document.getElementById("itemStrip");
  // jump to a final roll
  const resultItem = rollItem();
  const targetIndex = Array.from(strip.children).findIndex(c => c.textContent === resultItem.name);
  strip.style.left = -(targetIndex * strip.children[0].offsetWidth) + "px";
  stripPos = parseFloat(strip.style.left) || 0;
  showResult(targetIndex);
});
</script>

</body>
</html>
