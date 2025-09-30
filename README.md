# events-on3<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Customized Event Selection</title>
  <style>
    body { font-family: Arial, sans-serif; background: #f8f8fc; color: #222; margin: 0; padding: 0; }
    .container { max-width: 750px; margin: 35px auto; background: #fff; padding: 30px 25px 25px; border-radius: 12px; box-shadow: 0 2px 10px #0001; }
    h1 { text-align: center; margin-bottom: 35px; }
    .step { margin-bottom: 28px; }
    .label { display: block; margin-bottom: 10px; font-weight: bold; }
    .flex-row { display: flex; gap: 22px; }
    .btn, .option-btn { background: #eee; border-radius: 6px; border: none; padding: 14px 18px; font-size: 1em; cursor: pointer; margin: 8px 9px 8px 0; }
    .btn.selected, .option-btn.selected { background: #3155b7; color: #fff; }
    .input { padding: 9px 11px; border-radius: 5px; border: 1px solid #bbb; width: 180px; margin-left: 10px;}
    .summary-box { background: #e3e7fa; padding: 25px; border-radius: 8px; margin-top: 12px; }
    .section-title { font-size: 1.08em; margin-top: 23px; text-decoration: underline; color: #333;}
    ul { padding-left: 20px; }
    @media (max-width: 600px) {
      .flex-row { flex-direction: column; gap: 0; }
      .input { width: 98%; }
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Customized Event Selection</h1>
    
    <div class="step" id="serviceStep">
      <span class="label">1. Choose Service:</span>
      <button class="btn" onclick="chooseService('Decoration')">Decoration</button>
      <button class="btn" onclick="chooseService('Catering')">Catering</button>
    </div>
    
    <form id="eventForm" style="display:none;">
      <div class="step" id="locationStep">
        <span class="label">2. Location:</span>
        <input type="text" class="input" id="locationInput" required placeholder="Enter location" />
      </div>
      
      <div class="step" id="budgetStep">
        <span class="label">3. Budget:</span>
        <input type="number" class="input" id="budgetInput" min="0" required placeholder="e.g., 100000" />
      </div>
      
      <div class="step" id="foodTypeStep" style="display:none;">
        <span class="label">4. Food Type:</span>
        <button class="option-btn" type="button" onclick="chooseFoodType('Veg')">Veg</button>
        <button class="option-btn" type="button" onclick="chooseFoodType('Non Veg')">Non Veg</button>
        <button class="option-btn" type="button" onclick="chooseFoodType('Mixed')">Mixed</button>
      </div>
      
      <div class="step" id="plateRateStep" style="display:none;">
        <span class="label">5. Plate Rate:</span>
        <button class="option-btn" type="button" onclick="chooseRate(120)">₹120</button>
        <button class="option-btn" type="button" onclick="chooseRate(150)">₹150</button>
        <button class="option-btn" type="button" onclick="chooseRate(200)">₹200</button>
        <button class="option-btn" type="button" onclick="chooseRate('Other')">Other</button>
        <input type="number" class="input" id="customRateInput" min="0" style="display:none; margin-left:12px;" placeholder="Enter rate..." />
      </div>
      
      <div class="step" id="itemSelectStep" style="display:none;">
        <span class="label">6. Select Menu Items:</span>
        <div class="flex-row" style="flex-wrap:wrap;">
          <label><input type="checkbox" name="menuItem" value="Welcome Drink"> Welcome Drink</label>
          <label><input type="checkbox" name="menuItem" value="Sweet"> Sweet</label>
          <label><input type="checkbox" name="menuItem" value="Rice"> Rice</label>
          <label><input type="checkbox" name="menuItem" value="Biryani"> Biryani</label>
          <!-- Add more items here -->
        </div>
      </div>
      
      <button type="button" class="btn" style="margin-top:23px;" onclick="showSummary()">Show Summary</button>
    </form>
    
    <div class="summary-box" id="summaryBox" style="display:none;">
      <div class="section-title">Summary:</div>
      <div id="summaryContent"></div>
    </div>
  </div>
  
<script>
  let chosenService = null, chosenFoodType = null, chosenRate = null;
  function chooseService(service) {
    chosenService = service;
    document.querySelectorAll('.btn').forEach(b => b.classList.remove('selected'));
    event.target.classList.add('selected');
    document.getElementById('eventForm').style.display = 'block';
    document.getElementById('locationStep').style.display = '';
    document.getElementById('budgetStep').style.display = '';
    if(service === 'Catering') {
      document.getElementById('foodTypeStep').style.display = '';
      document.getElementById('plateRateStep').style.display = '';
      document.getElementById('itemSelectStep').style.display = '';
    } else {
      document.getElementById('foodTypeStep').style.display = 'none';
      document.getElementById('plateRateStep').style.display = 'none';
      document.getElementById('itemSelectStep').style.display = 'none';
    }
    document.getElementById('summaryBox').style.display = 'none';
    resetFoodInputs();
  }

  function chooseFoodType(type) {
    chosenFoodType = type;
    document.querySelectorAll('#foodTypeStep .option-btn').forEach(b => b.classList.remove('selected'));
    Array.from(document.querySelectorAll('#foodTypeStep .option-btn')).find(b => b.textContent === type).classList.add('selected');
  }

  function chooseRate(rate) {
    chosenRate = rate;
    document.querySelectorAll('#plateRateStep .option-btn').forEach(b => b.classList.remove('selected'));
    let btn = Array.from(document.querySelectorAll('#plateRateStep .option-btn')).find(b => b.textContent.includes(rate === 'Other' ? 'Other' : rate));
    if (btn) btn.classList.add('selected');
    if(rate === 'Other') {
      document.getElementById('customRateInput').style.display = '';
      document.getElementById('customRateInput').value = '';
    } else {
      document.getElementById('customRateInput').style.display = 'none';
    }
  }
  function resetFoodInputs() {
    chosenFoodType = null;
    chosenRate = null;
    document.querySelectorAll('#foodTypeStep .option-btn').forEach(b => b.classList.remove('selected'));
    document.querySelectorAll('#plateRateStep .option-btn').forEach(b => b.classList.remove('selected'));
    document.getElementById('customRateInput').value = '';
    document.getElementById('customRateInput').style.display = 'none';
    let checks = document.querySelectorAll("input[name='menuItem']");
    checks.forEach(chk => chk.checked = false);
  }

  function showSummary() {
    let location = document.getElementById('locationInput').value.trim();
    let budget = document.getElementById('budgetInput').value.trim();
    let html = `<ul>`;
    html += `<li><b>Service:</b> ${chosenService || ''}</li>`;
    html += `<li><b>Location:</b> ${location}</li>`;
    html += `<li><b>Budget:</b> ${budget && Number(budget) > 0 ? '₹' + budget : ''}</li>`;
    if(chosenService === "Catering") {
      html += `<li><b>Food Type:</b> ${chosenFoodType || ''}</li>`;
      let plateRate = chosenRate === 'Other' ? document.getElementById('customRateInput').value : chosenRate;
      html += `<li><b>Plate Rate:</b> ${plateRate ? '₹' + plateRate : ''}</li>`;
      let menuItems = Array.from(document.querySelectorAll("input[name='menuItem']:checked")).map(chk => chk.value);
      html += `<li><b>Menu Items:</b> ${menuItems.length ? menuItems.join(', ') : ''}</li>`;
    }
    html += `</ul>`;
    document.getElementById('summaryContent').innerHTML = html;
    document.getElementById('summaryBox').style.display = '';
    window.scrollTo({top: document.body.scrollHeight, behavior: "smooth"});
  }
</script>
</body>
</html>
