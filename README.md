<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simplified ROI Calculator</title>
  <style>
    :root {
      --accent: #FF5F4E;
      --text-color: #444;
      --bg-light: #F9F6F2;
      --edge: #ECE8E3;
      --font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      --shadow: 0 3px 14px rgba(0,0,0,.05);
      --radius: 1rem;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: var(--font-family);
      color: var(--text-color);
      background-color: transparent;
      padding: 20px;
    }
    
    .calculator-container {
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem;
      background-color: var(--bg-light);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
    }
    
    h2 {
      font-size: 1.25rem;
      font-weight: 600;
      margin-bottom: 1rem;
      color: var(--text-color);
      border-bottom: 1px solid var(--edge);
      padding-bottom: 0.5rem;
    }
    
    .calculation-section {
      margin-top: 1.5rem;
      margin-bottom: 1.5rem;
    }
    
    .grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 1.5rem;
      margin-bottom: 1.5rem;
    }
    
    .grid-item {
      display: flex;
      flex-direction: column;
    }
    
    label {
      font-weight: 500;
      margin-bottom: 0.5rem;
    }
    
    select, input, button {
      padding: 0.75rem;
      border: 1px solid var(--edge);
      border-radius: var(--radius);
      font-family: var(--font-family);
      color: var(--text-color);
      background-color: white;
      margin-bottom: 10px;
    }
    
    button {
      background-color: var(--accent);
      color: white;
      font-weight: bold;
      cursor: pointer;
      border: none;
    }
    
    .result-box {
      padding: 0.75rem;
      background-color: var(--edge);
      border-radius: var(--radius);
    }
    
    .final-result {
      border-top: 1px solid var(--edge);
      padding-top: 1.5rem;
      margin-top: 0.5rem;
    }
    
    .roi-box {
      padding: 1rem;
      background-color: var(--accent);
      color: white;
      border-radius: var(--radius);
      text-align: center;
      font-size: 1.5rem;
      font-weight: 700;
      margin-top: 0.5rem;
    }
    
    .result-label {
      font-size: 1.25rem;
      font-weight: 700;
    }
    
    .font-semibold {
      font-weight: 600;
    }
    
    /* Mobile responsiveness */
    @media (max-width: 768px) {
      .grid {
        grid-template-columns: 1fr;
      }
    }
  </style>
</head>
<body>
  <div class="calculator-container">    
    <div class="grid">
      <div class="grid-item">
        <label for="closeRate">Enter your average close rate (%)</label>
        <input type="number" id="closeRate" min="0" max="100" placeholder="e.g., 25" value="25">
      </div>
      
      <div class="grid-item">
        <label for="dealSize">Enter your average deal size ($)</label>
        <input type="number" id="dealSize" min="0" placeholder="e.g., 35000" value="35000">
      </div>
    </div>
    
    <div class="grid">
      <div class="grid-item">
        <label for="accountType">Pick your plan:</label>
        <select id="accountType">
          <option value="Impact">Impact</option>
          <option value="Scale">Scale</option>
        </select>
      </div>
      
      <div class="grid-item">
        <label for="paymentFrequency">Pick your payment option:</label>
        <select id="paymentFrequency">
          <option value="Monthly">Monthly</option>
          <option value="Annually">Annually</option>
        </select>
      </div>
    </div>
    
    <button onclick="calculate()">Calculate ROI</button>
    
    <div class="calculation-section">
      <h2>ROI Calculator - Using Minimum Conversation Guarantee</h2>
      <div class="grid">
        <div class="grid-item">
          <label>Top of Funnel Total Accounts:</label>
          <div id="topOfFunnel" class="result-box">250</div>
        </div>
        
        <div class="grid-item">
          <label>Express Interest (12%):</label>
          <div id="expressInterest" class="result-box">30</div>
        </div>
        
        <div class="grid-item">
          <label>Move to Meeting Rate (85%):</label>
          <div id="moveToMeeting" class="result-box">26</div>
        </div>
        
        <div class="grid-item">
          <label>Total Conversations:</label>
          <div id="totalConversations" class="result-box">26</div>
        </div>
      </div>
    </div>
    
    <div class="grid">
      <div class="grid-item">
        <label>Expected Revenue:</label>
        <div id="expectedRevenue" class="result-box font-semibold">$52,000</div>
      </div>
      
      <div class="grid-item">
        <label>Subscription Cost:</label>
        <div id="subscriptionCost" class="result-box font-semibold">$42,000</div>
      </div>
    </div>
    
    <div class="final-result">
      <label class="result-label">Return on Investment (ROI):</label>
      <div id="roi" class="roi-box">1.24x</div>
    </div>
  </div>
  
  <script>
    // Initial calculation on page load
    calculate();
    
    // Main calculation function
    function calculate() {
      // Get input values
      var accountType = document.getElementById('accountType').value;
      var paymentFrequency = document.getElementById('paymentFrequency').value;
      var closeRate = parseFloat(document.getElementById('closeRate').value) || 0;
      var dealSize = parseFloat(document.getElementById('dealSize').value) || 0;
      
      // Calculate top of funnel accounts
      var topOfFunnel = 250;
      if (accountType === 'Scale') {
        topOfFunnel = 500;
      }
      
      // Calculate express interest (12% of top of funnel)
      var expressInterest = Math.round(topOfFunnel * 0.12);
      
      // Calculate move to meeting rate (85% of express interest)
      var moveToMeeting = Math.round(expressInterest * 0.85);
      
      // Set total conversations
      var totalConversations = moveToMeeting;
      
      // Calculate expected revenue
      var expectedRevenue = Math.round((totalConversations * closeRate / 100) * dealSize);
      
      // Determine subscription cost based on selections
      var subscriptionCost = 42000; // Default: Impact Monthly
      
      if (accountType === 'Impact' && paymentFrequency === 'Annually') {
        subscriptionCost = 35000;
      } else if (accountType === 'Scale' && paymentFrequency === 'Monthly') {
        subscriptionCost = 66000;
      } else if (accountType === 'Scale' && paymentFrequency === 'Annually') {
        subscriptionCost = 50000;
      }
      
      // Calculate ROI
      var roi = expectedRevenue / subscriptionCost;
      
      // Update DOM elements
      document.getElementById('topOfFunnel').textContent = topOfFunnel;
      document.getElementById('expressInterest').textContent = expressInterest;
      document.getElementById('moveToMeeting').textContent = moveToMeeting;
      document.getElementById('totalConversations').textContent = totalConversations;
      document.getElementById('expectedRevenue').textContent = '$' + expectedRevenue.toLocaleString();
      document.getElementById('subscriptionCost').textContent = '$' + subscriptionCost.toLocaleString();
      document.getElementById('roi').textContent = roi.toFixed(2) + 'x';
    }
  </script>
</body>
</html>
