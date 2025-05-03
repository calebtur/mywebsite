
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no" />
<title>Stock Algo Subscription</title>
<style>
  /* Reset & base */
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  html, body {
    height: 100%;
    background-color: #000;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    color: #ccc;
    overflow: hidden;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  #app {
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  .container {
    padding: 1rem 1.5rem 2rem 1.5rem;
    flex: 1;
    /* Each section will scroll if content too tall */
    overflow-y: auto;
  }
  .container::-webkit-scrollbar {
    width: 8px;
  }
  .container::-webkit-scrollbar-thumb {
    background: #1b8c3b;
    border-radius: 4px;
  }

  :root {
    --bg-black: #000000;
    --green-dark: #1b8c3b;
    --green-bright: #2fef4f;
    --red-dark: #a82323;
    --red-bright: #ff4f4f;
    --text-light: #ccc;
    --text-lighter: #aaa;
  }

  h1, h2, h3, h4 {
    font-weight: 700;
    text-shadow: 0 0 8px rgba(0,255,0,0.7);
  }
  button, .btn {
    cursor: pointer;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    transition: background-color 0.3s, box-shadow 0.3s;
    padding: 0.7em 1.5em;
    font-size: 1rem;
    user-select: none;
  }
  .btn-primary {
    background-color: var(--green-dark);
    color: #000;
    box-shadow: 0 0 6px var(--green-bright);
  }
  .btn-primary:hover, .btn-primary:focus {
    background-color: var(--green-bright);
    box-shadow: 0 0 16px var(--green-bright);
    outline: none;
  }
  .btn-secondary {
    background-color: var(--red-dark);
    color: #fff;
    box-shadow: 0 0 6px var(--red-bright);
  }
  .btn-secondary:hover, .btn-secondary:focus {
    background-color: var(--red-bright);
    color: #000;
    box-shadow: 0 0 16px var(--red-bright);
    outline: none;
  }

  #homepage, #plans, #payment, #dashboard, #adminDashboard {
    max-width: 600px;
    margin: 1.5rem auto;
    padding: 1rem 1.5rem 2rem 1.5rem;
    background: #111;
    border-radius: 14px;
    box-shadow: 0 0 20px var(--green-bright);
    display: none;
    height: calc(100vh - 4rem);
    overflow-y: auto;
    color: #ccc;
  }
  #homepage.active, #plans.active, #payment.active, #dashboard.active, #adminDashboard.active {
    display: block;
  }
  #homepage h1 {
    font-size: 2.5rem;
    color: var(--green-bright);
    margin-bottom: 1rem;
  }
  #homepage p {
    font-size: 1.2rem;
    line-height: 1.5;
    margin-bottom: 2rem;
    color: var(--text-lighter);
  }
  #getStartedBtn {
    font-size: 1.2rem;
  }
  #modal-overlay {
    position: fixed;
    inset: 0;
    background: rgba(0,0,0,0.85);
    display: none;
    justify-content: center;
    align-items: center;
    z-index: 1000;
  }
  #modal-overlay.active {
    display: flex;
  }
  #modal {
    background: #111;
    width: 320px;
    border-radius: 12px;
    padding: 2rem 2.5rem;
    color: #ccc;
    position: relative;
    box-shadow: 0 0 20px var(--green-bright);
  }
  #modal h2 {
    margin-bottom: 1rem;
    color: var(--green-bright);
    font-size: 1.8rem;
    text-align: center;
  }
  #modal form {
    display: flex;
    flex-direction: column;
  }
  #modal label {
    font-size: 0.9rem;
    color: var(--text-lighter);
    margin-bottom: 0.5rem;
  }
  #modal input[type=email], #modal input[type=password] {
    background: #222;
    border-radius: 6px;
    border: none;
    color: #eee;
    font-size: 1rem;
    padding: 0.5rem 0.7rem;
    margin-bottom: 1rem;
  }
  #modal input:focus {
    outline: 2px solid var(--green-bright);
  }
  #modal .form-switch {
    text-align: center;
    font-size: 0.9rem;
    color: #999;
    cursor: pointer;
    margin-top: 0.8rem;
    user-select: none;
  }
  #modal .error-message {
    color: var(--red-bright);
    font-size: 0.85rem;
    height: 1.2rem;
    margin-bottom: 0.5rem;
    text-align: center;
  }
  #modal button.close-btn {
    background: transparent;
    border: none;
    color: var(--red-bright);
    font-size: 1.3rem;
    position: absolute;
    top: 0.5rem;
    right: 1rem;
    cursor: pointer;
  }
  #modal button.submit-btn {
    margin-top: 0.2rem;
  }

  /* Plans styling */
  #plans h2 {
    color: var(--green-bright);
    text-align: center;
    margin-bottom: 1rem;
    text-shadow: 0 0 6px var(--green-bright);
  }
  .plan-list {
    display: flex;
    gap: 1rem;
    overflow-x: auto;
  }
  .plan {
    background: #222;
    flex: 0 0 170px;
    border-radius: 12px;
    padding: 1.2rem 1rem;
    border: 2px solid var(--green-dark);
    cursor: pointer;
    user-select: none;
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .plan:hover, .plan.selected {
    border-color: var(--green-bright);
    box-shadow: 0 0 20px var(--green-bright);
  }
  .plan h3 {
    margin-bottom: 0.5rem;
    color: var(--green-bright);
  }
  .plan .price {
    color: #8bf08b;
    font-weight: 700;
    margin-bottom: 0.8rem;
  }
  .plan ul {
    list-style: none;
    color: var(--text-lighter);
    font-size: 0.9rem;
    padding-left: 0;
  }
  .plan ul li::before {
    content: "✔";
    margin-right: 6px;
    color: var(--green-bright);
    font-weight: 700;
  }
  #buyPlanBtn {
    display: block;
    margin: 2rem auto 0 auto;
    font-weight: 700;
  }

  /* Payment styles */
  #payment h2 {
    color: var(--green-bright);
    text-align: center;
    margin-bottom: 1rem;
    text-shadow: 0 0 6px var(--green-bright);
  }
  .payment-options {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .payment-option {
    background: #222;
    border-radius: 8px;
    padding: 1rem;
    border: 2px solid transparent;
    cursor: pointer;
    font-weight: 600;
    user-select: none;
    transition: border-color 0.3s, box-shadow 0.3s;
  }
  .payment-option.selected, .payment-option:hover {
    border-color: var(--green-bright);
    box-shadow: 0 0 20px var(--green-bright);
  }
  #confirmPaymentBtn {
    width: 100%;
    padding: 0.9rem;
    font-weight: 700;
    border-radius: 10px;
    background: var(--green-bright);
    border: none;
    color: #000;
    cursor: pointer;
    user-select: none;
  }
  #confirmPaymentBtn:disabled {
    background: #333;
    cursor: not-allowed;
    color: #666;
  }

  /* Dashboard */
  #dashboard h2 {
    color: var(--green-bright);
    text-align: center;
    margin-bottom: 1rem;
    text-shadow: 0 0 8px var(--green-bright);
  }
  #dashboard .logout-btn {
    background: transparent;
    color: var(--red-bright);
    border: 2px solid var(--red-bright);
    border-radius: 8px;
    padding: 0.4rem 1rem;
    font-weight: 700;
    cursor: pointer;
    float: right;
    margin-bottom: 1rem;
    user-select: none;
    transition: background-color 0.3s;
  }
  #dashboard .logout-btn:hover {
    background-color: var(--red-bright);
    color: #000;
  }
  #stockForm {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
    margin-bottom: 1rem;
  }
  #stockForm label {
    display: flex;
    flex-direction: column;
    max-width: 140px;
    font-size: 0.9rem;
    color: var(--text-lighter);
  }
  #stockForm input {
    padding: 0.4rem 0.5rem;
    border-radius: 6px;
    border: none;
    background: #222;
    color: #ddd;
    font-size: 1rem;
    margin-top: 0.3rem;
  }
  #stockForm button {
    background: var(--green-bright);
    flex-grow: 1;
    max-width: 140px;
    border: none;
    border-radius: 6px;
    color: #000;
    font-weight: 700;
    cursor: pointer;
    user-select: none;
  }
  table {
    width: 100%;
    border-collapse: collapse;
    font-size: 0.95rem;
    margin-bottom: 2rem;
  }
  thead th {
    text-align: left;
    border-bottom: 2px solid var(--green-dark);
    padding: 0.8rem 0.5rem;
    color: var(--green-bright);
  }
  tbody tr {
    border-bottom: 1px solid #222;
  }
  tbody td {
    padding: 0.8rem 0.5rem;
  }
  .price-positive {
    color: var(--green-bright);
    font-weight: 600;
  }
  .price-negative {
    color: var(--red-bright);
    font-weight: 600;
  }

  /* Daily stock update section */
  #dailyUpdatesSection h3 {
    font-size: 1.3rem;
    color: var(--green-bright);
    margin-bottom: 0.6rem;
    text-shadow: 0 0 6px var(--green-bright);
  }

  /* Admin Dashboard styles */
  #adminDashboard h2 {
    color: var(--red-bright);
    text-align: center;
    margin-bottom: 1rem;
    text-shadow: 0 0 8px var(--red-bright);
  }
  #dailyUpdateForm {
    display: flex;
    flex-wrap: wrap;
    gap: 1rem;
    justify-content: center;
    margin-bottom: 1rem;
  }
  #dailyUpdateForm label {
    display: flex;
    flex-direction: column;
    max-width: 140px;
    font-size: 0.9rem;
    color: var(--text-lighter);
  }
  #dailyUpdateForm input {
    padding: 0.4rem 0.5rem;
    border-radius: 6px;
    border: none;
    background: #222;
    color: #ddd;
    font-size: 1rem;
    margin-top: 0.3rem;
  }
  #dailyUpdateForm button {
    background: var(--red-bright);
    flex-grow: 1;
    max-width: 140px;
    color: #000;
    border: none;
    border-radius: 6px;
    font-weight: 700;
    cursor: pointer;
    user-select: none;
  }
  #adminDailyUpdatesList button.delete-btn {
    background: transparent;
    border: none;
    color: var(--red-bright);
    font-weight: 700;
    font-size: 1.2rem;
    cursor: pointer;
    user-select: none;
  }
  #adminDailyUpdatesList button.delete-btn:hover {
    color: var(--green-bright);
  }

  /* Responsive */
  @media (max-width: 480px) {
    #stockForm, #dailyUpdateForm {
      flex-direction: column;
      gap: 1rem;
    }
    #stockForm label, #dailyUpdateForm label {
      max-width: 100%;
    }
    #stockForm button, #dailyUpdateForm button {
      max-width: 100%;
    }
    .plan-list {
      overflow-x: auto;
    }
  }

  /* 3D animated background sphere */
  #background-3d {
    position: fixed;
    top: 50%;
    left: 50%;
    width: 200px;
    height: 200px;
    margin-left: -100px;
    margin-top: -100px;
    pointer-events: none;
    z-index: 0;
    filter: drop-shadow(0 0 5px var(--green-bright));
    opacity: 0.3;
  }
  .sphere {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    perspective: 800px;
    animation: rotateSphere 45s linear infinite;
  }
  .sphere-inner {
    width: 160px;
    height: 160px;
    border-radius: 50%;
    background: radial-gradient(circle at center, var(--green-dark) 40%, transparent 70%);
    position: absolute;
    top: 20px;
    left: 20px;
    box-shadow: 0 0 30px var(--green-bright), inset 0 0 20px var(--green-bright);
    transform-style: preserve-3d;
  }
  @keyframes rotateSphere {
    from {
      transform: rotateX(0deg) rotateY(0deg);
    }
    to {
      transform: rotateX(360deg) rotateY(360deg);
    }
  }

  /* Admin toggle button */
  #adminToggleBtn {
    position: fixed;
    bottom: 1rem;
    right: 1rem;
    z-index: 1100;
    background-color: var(--red-bright);
    border: none;
    border-radius: 12px;
    color: #000;
    font-weight: 700;
    padding: 0.8rem 1.2rem;
    cursor: pointer;
    box-shadow: 0 0 10px var(--red-bright);
    user-select: none;
    font-size: 0.9rem;
    display: none;
  }
  #adminToggleBtn[aria-pressed="true"] {
    background-color: var(--green-bright);
  }
  #adminToggleBtn:focus {
    outline: 2px solid var(--green-bright);
  }
</style>
</head>
<body>
<div id="app" role="main" aria-live="polite" aria-atomic="true">
  <!-- 3D Background Sphere -->
  <div id="background-3d" aria-hidden="true">
    <div class="sphere">
      <div class="sphere-inner"></div>
    </div>
  </div>

  <!-- Homepage -->
  <section id="homepage" class="active" aria-label="Homepage" role="region">
    <h1>Stock Algo</h1>
    <p>
      Welcome to Stock Algo — a powerful subscription-based platform to manage and analyze your stock investments effortlessly.
      Track buying prices, set target prices, and stop losses for your stocks all in one place.
      Begin your journey towards smarter trading today.
    </p>
    <button id="getStartedBtn" class="btn btn-primary" aria-haspopup="dialog" aria-controls="modal-overlay">Get Started</button>
  </section>

  <!-- Login/Register Modal -->
  <div id="modal-overlay" role="dialog" aria-modal="true" aria-labelledby="modalTitle" tabindex="-1" aria-hidden="true">
    <div id="modal" role="document">
      <button class="close-btn" id="modalCloseBtn" aria-label="Close">&times;</button>
      <h2 id="modalTitle">Login</h2>
      <form id="authForm" novalidate>
        <div class="error-message" id="authErrorMsg" aria-live="assertive"></div>
        <label for="emailInput">Email</label>
        <input type="email" id="emailInput" name="email" required autocomplete="username" aria-required="true" placeholder="you@example.com" />
        <label for="passwordInput">Password</label>
        <input type="password" id="passwordInput" name="password" required autocomplete="current-password" aria-required="true" placeholder="Your password" />
        <button type="submit" class="btn btn-primary submit-btn" id="authSubmitBtn">Login</button>
      </form>
      <div class="form-switch" id="switchAuth" tabindex="0" role="button" aria-pressed="false">Don't have an account? Sign up here</div>
    </div>
  </div>

  <!-- Plans -->
  <section id="plans" aria-label="Subscription Plans" role="region" tabindex="0">
    <h2>Select a Plan</h2>
    <div class="plan-list" role="list">
      <div class="plan" role="listitem" tabindex="0" data-plan="basic" aria-label="Basic Plan">
        <h3>Basic</h3>
        <div class="price">$10 / month</div>
        <ul>
          <li>Track up to 5 stocks</li>
          <li>Basic alerts</li>
          <li>Email support</li>
        </ul>
      </div>
      <div class="plan" role="listitem" tabindex="0" data-plan="pro" aria-label="Pro Plan">
        <h3>Pro</h3>
        <div class="price">$25 / month</div>
        <ul>
          <li>Track up to 20 stocks</li>
          <li>Advanced alerts</li>
          <li>Priority email support</li>
        </ul>
      </div>
      <div class="plan" role="listitem" tabindex="0" data-plan="premium" aria-label="Premium Plan">
        <h3>Premium</h3>
        <div class="price">$50 / month</div>
        <ul>
          <li>Unlimited stocks</li>
          <li>Real-time alerts</li>
          <li>24/7 premium support</li>
        </ul>
      </div>
    </div>
    <button id="buyPlanBtn" class="btn btn-primary" disabled>Proceed to Payment</button>
  </section>

  <!-- Payment -->
  <section id="payment" aria-label="Payment Options" role="region" tabindex="0">
    <h2>Choose Payment Method</h2>
    <div class="payment-options" role="list">
      <div class="payment-option" role="listitem" tabindex="0" data-method="card" aria-label="Credit or Debit Card">
        Credit / Debit Card
      </div>
      <div class="payment-option" role="listitem" tabindex="0" data-method="paypal" aria-label="PayPal">
        PayPal
      </div>
      <div class="payment-option" role="listitem" tabindex="0" data-method="crypto" aria-label="Cryptocurrency">
        Cryptocurrency
      </div>
    </div>
    <button id="confirmPaymentBtn" class="btn btn-primary" disabled>Confirm Payment</button>
  </section>

  <!-- User Dashboard -->
  <section id="dashboard" aria-label="User Dashboard" role="region" tabindex="0">
    <button id="logoutBtn" class="logout-btn" aria-label="Logout">Logout</button>
    <h2>Your Stock Dashboard</h2>
    <form id="stockForm" aria-label="Add Stock Form">
      <label for="stockName">Stock Name
        <input type="text" id="stockName" name="stockName" placeholder="e.g. AAPL" required maxlength="10" aria-required="true" autocomplete="off" />
      </label>
      <label for="buyPrice">Buying Price
        <input type="number" id="buyPrice" name="buyPrice" min="0" step="0.01" placeholder="e.g. 150.50" required aria-required="true" />
      </label>
      <label for="targetPrice">Target Price
        <input type="number" id="targetPrice" name="targetPrice" min="0" step="0.01" placeholder="e.g. 170.00" required aria-required="true" />
      </label>
      <label for="stopLoss">Stop Loss
        <input type="number" id="stopLoss" name="stopLoss" min="0" step="0.01" placeholder="e.g. 140.00" required aria-required="true" />
      </label>
      <button type="submit" class="btn btn-primary">Add Stock</button>
    </form>

    <table aria-live="polite" aria-label="List of your tracked stocks">
      <thead>
        <tr>
          <th>Stock</th>
          <th>Buying Price</th>
          <th>Target Price</th>
          <th>Stop Loss</th>
          <th>Trend</th>
        </tr>
      </thead>
      <tbody id="stockList">
        <!-- Stocks added will appear here -->
      </tbody>
    </table>

    <section id="dailyUpdatesSection" aria-label="Daily Stock Updates">
      <h3>Daily Stock Updates</h3>
      <table id="dailyUpdatesTable" aria-live="polite" aria-label="Daily updated stocks list">
        <thead>
          <tr>
            <th>Stock</th>
            <th>Price</th>
            <th>Change</th>
          </tr>
        </thead>
        <tbody id="dailyUpdatesList">
          <tr><td colspan="3" style="color:#666;text-align:center;padding:1rem;">No daily updates available.</td></tr>
        </tbody>
      </table>
    </section>
  </section>

  <!-- Admin Dashboard -->
  <section id="adminDashboard" aria-label="Admin Dashboard" role="region" tabindex="0">
    <h2>Admin Dashboard - Daily Stock Updates</h2>
    <form id="dailyUpdateForm" aria-label="Add Daily Stock Update Form">
      <label for="dailyStockName">Stock Name
        <input type="text" id="dailyStockName" name="dailyStockName" placeholder="e.g. AAPL" required maxlength="10" aria-required="true" autocomplete="off" />
      </label>
      <label for="dailyPrice">Price
        <input type="number" id="dailyPrice" name="dailyPrice" min="0" step="0.01" placeholder="e.g. 172.00" required aria-required="true" />
      </label>
      <label for="dailyChange">Change
        <input type="number" id="dailyChange" name="dailyChange" step="0.01" placeholder="e.g. +1.50 or -0.50" required aria-required="true" />
      </label>
      <button type="submit" class="btn btn-secondary">Add / Update</button>
    </form>

    <table aria-live="polite" aria-label="List of daily stock updates">
      <thead>
        <tr>
          <th>Stock</th>
          <th>Price</th>
          <th>Change</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody id="adminDailyUpdatesList">
        <!-- Daily updates entries here -->
      </tbody>
    </table>
  </section>

  <!-- Admin toggle button -->
  <button id="adminToggleBtn" aria-pressed="false" aria-label="Toggle Admin Dashboard">Admin</button>
</div>

<script>
  (function(){
    // Elements
    const homepage = document.getElementById('homepage'),
          modalOverlay = document.getElementById('modal-overlay'),
          modalCloseBtn = document.getElementById('modalCloseBtn'),
          authForm = document.getElementById('authForm'),
          emailInput = document.getElementById('emailInput'),
          passwordInput = document.getElementById('passwordInput'),
          authErrorMsg = document.getElementById('authErrorMsg'),
          switchAuth = document.getElementById('switchAuth'),
          modalTitle = document.getElementById('modalTitle'),
          authSubmitBtn = document.getElementById('authSubmitBtn'),
          getStartedBtn = document.getElementById('getStartedBtn'),
          plansSection = document.getElementById('plans'),
          planElements = plansSection.querySelectorAll('.plan'),
          buyPlanBtn = document.getElementById('buyPlanBtn'),
          paymentSection = document.getElementById('payment'),
          paymentOptions = paymentSection.querySelectorAll('.payment-option'),
          confirmPaymentBtn = document.getElementById('confirmPaymentBtn'),
          dashboardSection = document.getElementById('dashboard'),
          stockForm = document.getElementById('stockForm'),
          stockList = document.getElementById('stockList'),
          logoutBtn = document.getElementById('logoutBtn'),
          dailyUpdatesList = document.getElementById('dailyUpdatesList'),
          adminToggleBtn = document.getElementById('adminToggleBtn'),
          adminDashboard = document.getElementById('adminDashboard'),
          dailyUpdateForm = document.getElementById('dailyUpdateForm'),
          adminDailyUpdatesList = document.getElementById('adminDailyUpdatesList'),
          dailyStockNameInput = document.getElementById('dailyStockName'),
          dailyPriceInput = document.getElementById('dailyPrice'),
          dailyChangeInput = document.getElementById('dailyChange');

    // State
    let isLoginMode = true,
        selectedPlan = null,
        selectedPaymentMethod = null;

    // Storage keys
    const usersDBKey = 'stockAlgoUsers',
          userKey = 'stockAlgoUser',
          dailyUpdatesKey = 'stockAlgoDailyUpdates';

    // Helpers
    function saveUser(user) {
      localStorage.setItem(userKey, JSON.stringify(user));
    }
    function getUser() {
      try { return JSON.parse(localStorage.getItem(userKey)); }
      catch { return null; }
    }
    function clearUser() {
      localStorage.removeItem(userKey);
    }
    function getStocksKey(email) { return 'stockAlgoStocks_' + email; }
    function saveStocks(email, stocks) {
      localStorage.setItem(getStocksKey(email), JSON.stringify(stocks));
    }
    function getStocks(email) {
      try { return JSON.parse(localStorage.getItem(getStocksKey(email))) || []; }
      catch { return []; }
    }
    function getUsersDB() {
      try { return JSON.parse(localStorage.getItem(usersDBKey)) || {}; }
      catch { return {}; }
    }
    function saveUsersDB(db) {
      localStorage.setItem(usersDBKey, JSON.stringify(db));
    }
    function saveDailyUpdates(updates) {
      localStorage.setItem(dailyUpdatesKey, JSON.stringify(updates));
    }
    function getDailyUpdates() {
      try { return JSON.parse(localStorage.getItem(dailyUpdatesKey)) || []; }
      catch { return []; }
    }
    function showSection(showElem) {
      [homepage, plansSection, paymentSection, dashboardSection, adminDashboard].forEach(elem => {
        elem.classList.remove('active');
      });
      showElem.classList.add('active');
    }
    function validateEmail(email) {
      const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      return re.test(email.toLowerCase());
    }

    // Modal controls
    function openModal(){
      modalOverlay.classList.add('active');
      modalOverlay.setAttribute('aria-hidden', 'false');
      emailInput.focus();
    }
    function closeModal() {
      modalOverlay.classList.remove('active');
      modalOverlay.setAttribute('aria-hidden', 'true');
      authErrorMsg.textContent = '';
      authForm.reset();
      isLoginMode = true;
      updateModalView();
    }
    function updateModalView() {
      if(isLoginMode){
        modalTitle.textContent = 'Login';
        authSubmitBtn.textContent = 'Login';
        switchAuth.textContent = "Don't have an account? Sign up here";
        switchAuth.setAttribute('aria-pressed', 'false');
      } else {
        modalTitle.textContent = 'Sign Up';
        authSubmitBtn.textContent = 'Sign Up';
        switchAuth.textContent = "Already have an account? Login here";
        switchAuth.setAttribute('aria-pressed', 'true');
      }
      authErrorMsg.textContent = '';
      authForm.reset();
      emailInput.focus();
    }

    // Event listeners
    getStartedBtn.addEventListener('click', openModal);
    modalCloseBtn.addEventListener('click', closeModal);
    modalOverlay.addEventListener('click', e=>{
      if(e.target === modalOverlay) closeModal();
    });
    switchAuth.addEventListener('click', () => {
      isLoginMode = !isLoginMode;
      updateModalView();
    });
    switchAuth.addEventListener('keydown', e => {
      if(e.key==='Enter' || e.key===' ') {
        e.preventDefault();
        isLoginMode = !isLoginMode;
        updateModalView();
      }
    });

    authForm.addEventListener('submit', e=>{
      e.preventDefault();
      const email = emailInput.value.trim().toLowerCase();
      const password = passwordInput.value.trim();
      if(!validateEmail(email)){
        authErrorMsg.textContent = 'Please enter a valid email.';
        emailInput.focus();
        return;
      }
      if(password.length < 6){
        authErrorMsg.textContent = 'Password must be at least 6 characters.';
        passwordInput.focus();
        return;
      }
      const usersDB = getUsersDB();
      if(isLoginMode){
        if(usersDB[email] && usersDB[email].password === password){
          saveUser({email, subscription: usersDB[email].subscription || null});
          closeModal();
          if(usersDB[email].subscription){
            initDashboard();
          } else {
            initPlans();
          }
          showAdminToggleIfAdmin(email);
        } else {
          authErrorMsg.textContent = 'Invalid email or password.';
        }
      } else {
        if(usersDB[email]){
          authErrorMsg.textContent = 'Email already registered. Please login.';
        } else {
          usersDB[email] = {password, subscription: null};
          saveUsersDB(usersDB);
          saveUser({email, subscription: null});
          closeModal();
          initPlans();
          showAdminToggleIfAdmin(email);
        }
      }
    });

    planElements.forEach(planEl=>{
      planEl.addEventListener('click', () => {
        planElements.forEach(p=>p.classList.remove('selected'));
        planEl.classList.add('selected');
        selectedPlan = planEl.getAttribute('data-plan');
        buyPlanBtn.disabled = false;
      });
      planEl.addEventListener('keydown', e=>{
        if(e.key==='Enter' || e.key===' '){
          e.preventDefault();
          planEl.click();
        }
      });
    });
    buyPlanBtn.addEventListener('click', () => {
      if(selectedPlan) {
        initPayment();
      }
    });

    paymentOptions.forEach(opt=>{
      opt.addEventListener('click', ()=>{
        paymentOptions.forEach(o => o.classList.remove('selected'));
        opt.classList.add('selected');
        selectedPaymentMethod = opt.getAttribute('data-method');
        confirmPaymentBtn.disabled = false;
      });
      opt.addEventListener('keydown', e=>{
        if(e.key==='Enter' || e.key===' '){
          e.preventDefault();
          opt.click();
        }
      });
    });
    confirmPaymentBtn.addEventListener('click', () => {
      if(!selectedPaymentMethod) return;
      confirmPaymentBtn.disabled = true;
      confirmPaymentBtn.textContent = 'Processing...';
      setTimeout(() => {
        confirmPaymentBtn.textContent = 'Confirm Payment';
        const user = getUser();
        if(!user) {
          alert('User not logged in.');
          initHomepage();
          return;
        }
        const usersDB = getUsersDB();
        usersDB[user.email].subscription = selectedPlan;
        saveUsersDB(usersDB);
        user.subscription = selectedPlan;
        saveUser(user);
        alert(`Payment completed for the ${selectedPlan.charAt(0).toUpperCase() + selectedPlan.slice(1)} Plan.`);
        initDashboard();
        showAdminToggleIfAdmin(user.email);
      }, 1500);
    });

    stockForm.addEventListener('submit', e=>{
      e.preventDefault();
      const name = stockForm.stockName.value.trim();
      const buyPrice = parseFloat(stockForm.buyPrice.value);
      const targetPrice = parseFloat(stockForm.targetPrice.value);
      const stopLoss = parseFloat(stockForm.stopLoss.value);
      if(!name || isNaN(buyPrice) || isNaN(targetPrice) || isNaN(stopLoss)){
        alert('Please fill all fields correctly.');
        return;
      }
      if(buyPrice <=0 || targetPrice <= 0 || stopLoss <= 0){
        alert('Prices must be positive numbers.');
        return;
      }
      const user = getUser();
      if(!user) {
        alert('User not logged in.');
        initHomepage();
        return;
      }
      const planLimits = {basic:5, pro:20, premium: 1e9};
      const limit = planLimits[user.subscription];
      const stocks = getStocks(user.email);
      if(stocks.length >= limit){
        alert(`Your ${user.subscription} plan limit reached. Upgrade to add more stocks.`);
        return;
      }
      stocks.push({name, buyPrice, targetPrice, stopLoss});
      saveStocks(user.email, stocks);
      renderStocks(stocks);
      stockForm.reset();
      stockForm.stockName.focus();
    });

    logoutBtn.addEventListener('click', () => {
      clearUser();
      hideAdminToggle();
      initHomepage();
    });

    adminToggleBtn.addEventListener('click', () => {
      if(adminDashboard.classList.contains('active')){
        hideAdminDashboard();
        const user = getUser();
        if(user && user.subscription){
          initDashboard();
        } else if(user) {
          initPlans();
        } else {
          initHomepage();
        }
      } else {
        const user = getUser();
        if(user && user.email === 'admin@stockalgo.com'){
          showAdminDashboard();
        } else {
          alert('Access denied. Admin only.');
        }
      }
    });

    dailyUpdateForm.addEventListener('submit', e=>{
      e.preventDefault();
      const name = dailyStockNameInput.value.trim().toUpperCase();
      const price = parseFloat(dailyPriceInput.value);
      const change = parseFloat(dailyChangeInput.value);
      if(!name || isNaN(price) || isNaN(change)){
        alert('Fill all fields with valid data.');
        return;
      }
      if(price < 0){
        alert('Price must be positive.');
        return;
      }
      let updates = getDailyUpdates();
      const i = updates.findIndex(u => u.name === name);
      if(i >=0) {
        updates[i] = {name, price, change};
      } else {
        updates.push({name, price, change});
      }
      saveDailyUpdates(updates);
      renderAdminDaily(updates);
      dailyUpdateForm.reset();
      dailyStockNameInput.focus();
    });

    adminDailyUpdatesList.addEventListener('click', e => {
      if(e.target.classList.contains('delete-btn')){
        const tr = e.target.closest('tr');
        if(!tr) return;
        const name = tr.dataset.stockName;
        let updates = getDailyUpdates().filter(u => u.name !== name);
        saveDailyUpdates(updates);
        renderAdminDaily(updates);
        renderDailyUpdates(updates);
      }
    });

    function renderStocks(stocks) {
      stockList.innerHTML = '';
      if(stocks.length === 0){
        stockList.innerHTML = '<tr><td colspan="5" style="color:#666; text-align:center; padding:1rem;">No stocks added yet.</td></tr>';
        return;
      }
      stocks.forEach(({name, buyPrice, targetPrice, stopLoss}) => {
        let trend = 'Neutral';
        let trendClass = '';
        if(targetPrice > buyPrice){
          trend = 'Uptrend';
          trendClass = 'price-positive';
        } else if(stopLoss < buyPrice){
          trend = 'Downtrend';
          trendClass = 'price-negative';
        }
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${name.toUpperCase()}</td>
          <td>$${buyPrice.toFixed(2)}</td>
          <td class="${targetPrice >= buyPrice ? 'price-positive': 'price-negative'}">$${targetPrice.toFixed(2)}</td>
          <td class="${stopLoss <= buyPrice ? 'price-negative' : 'price-positive'}">$${stopLoss.toFixed(2)}</td>
          <td class="${trendClass}">${trend}</td>
        `;
        stockList.appendChild(tr);
      });
    }

    function renderDailyUpdates(updates) {
      dailyUpdatesList.innerHTML = '';
      if(updates.length === 0){
        dailyUpdatesList.innerHTML = '<tr><td colspan="3" style="color:#666; text-align:center; padding:1rem;">No daily updates available.</td></tr>';
        return;
      }
      updates.forEach(({name, price, change}) => {
        const changeClass = change >=0 ? 'price-positive' : 'price-negative';
        const changeText = (change > 0 ? '+' : '') + change.toFixed(2);
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td>${name.toUpperCase()}</td>
          <td>$${price.toFixed(2)}</td>
          <td class="${changeClass}">${changeText}</td>
        `;
        dailyUpdatesList.appendChild(tr);
      });
    }

    function renderAdminDaily(updates) {
      adminDailyUpdatesList.innerHTML = '';
      if(updates.length === 0){
        adminDailyUpdatesList.innerHTML = '<tr><td colspan="4" style="color:#666; text-align:center; padding:1rem;">No daily updates added.</td></tr>';
        return;
      }
      updates.forEach(({name, price, change}) => {
        const changeClass = change >=0 ? 'price-positive' : 'price-negative';
        const changeText = (change > 0 ? '+' : '') + change.toFixed(2);
        const tr = document.createElement('tr');
        tr.dataset.stockName = name;
        tr.innerHTML = `
          <td>${name}</td>
          <td>$${price.toFixed(2)}</td>
          <td class="${changeClass}">${changeText}</td>
          <td><button class="delete-btn" aria-label="Delete daily update for ${name}">×</button></td>
        `;
        adminDailyUpdatesList.appendChild(tr);
      });
    }

    function initHomepage() {
      showSection(homepage);
      modalOverlay.classList.remove('active');
      modalOverlay.setAttribute('aria-hidden', 'true');
    }
    function initPlans() {
      showSection(plansSection);
      selectedPlan = null;
      buyPlanBtn.disabled = true;
      planElements.forEach(p => p.classList.remove('selected'));
      selectedPaymentMethod = null;
      confirmPaymentBtn.disabled = true;
      paymentOptions.forEach(o => o.classList.remove('selected'));
    }
    function initPayment() {
      showSection(paymentSection);
    }
    function initDashboard(){
      showSection(dashboardSection);
      const user = getUser();
      if(!user) return;
      const stocks = getStocks(user.email);
      renderStocks(stocks);
      renderDailyUpdates(getDailyUpdates());
    }
    function showAdminDashboard() {
      showSection(adminDashboard);
      renderAdminDaily(getDailyUpdates());
      adminToggleBtn.setAttribute('aria-pressed', 'true');
    }
    function hideAdminDashboard() {
      adminToggleBtn.setAttribute('aria-pressed', 'false');
    }

    function showAdminToggleIfAdmin(email) {
      if(email === 'admin@stockalgo.com'){
        adminToggleBtn.style.display = 'block';
      } else {
        adminToggleBtn.style.display = 'none';
      }
    }
    function hideAdminToggle() {
      adminToggleBtn.style.display = 'none';
      hideAdminDashboard();
    }

    // Initialization on page load
    (() => {
      const user = getUser();
      if(user){
        showAdminToggleIfAdmin(user.email);
        if(user.subscription){
          initDashboard();
        } else {
          initPlans();
        }
      } else {
        initHomepage();
      }
    })();

    // Accessibility focus trap and keyboard handling for modal
    modalOverlay.addEventListener('keydown', e => {
      if(e.key === 'Escape'){
        closeModal();
      }
      if(e.key === 'Tab'){
        const focusableEls = modalOverlay.querySelectorAll('input, button, .form-switch');
        const first = focusableEls[0];
        const last = focusableEls[focusableEls.length-1];
        if(e.shiftKey){
          if(document.activeElement === first){
            last.focus();
            e.preventDefault();
          }
        } else {
          if(document.activeElement === last){
            first.focus();
            e.preventDefault();
          }
        }
      }
    });

  })();
</script>
</body>
</html>
```
