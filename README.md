<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hello Kitty Premium Calculator</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: "Comic Sans MS", "Trebuchet MS", sans-serif;
      user-select: none;
    }

    body {
      min-height: 100vh;
      overflow: hidden;
      background:
        radial-gradient(circle at 20% 20%, rgba(255,255,255,0.5), transparent 25%),
        radial-gradient(circle at 80% 30%, rgba(255,255,255,0.4), transparent 25%),
        radial-gradient(circle at 40% 80%, rgba(255,255,255,0.35), transparent 25%),
        linear-gradient(135deg, #ffd6ec, #ffc0e1, #ffb3da, #ff9fcf);
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 20px;
      position: relative;
    }

    .floating-hearts,
    .floating-stars {
      position: absolute;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
      z-index: 0;
    }

    .heart,
    .star {
      position: absolute;
      opacity: 0.35;
      animation: floatUp linear infinite;
      font-size: 18px;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(100vh) scale(0.8) rotate(0deg);
        opacity: 0;
      }
      10% { opacity: 0.35; }
      90% { opacity: 0.35; }
      100% {
        transform: translateY(-20vh) scale(1.2) rotate(360deg);
        opacity: 0;
      }
    }

    .calculator-wrapper {
      position: relative;
      z-index: 2;
      width: 100%;
      max-width: 420px;
      animation: popIn 0.7s ease;
    }

    @keyframes popIn {
      from {
        transform: scale(0.85) translateY(30px);
        opacity: 0;
      }
      to {
        transform: scale(1) translateY(0);
        opacity: 1;
      }
    }

    .kitty-ears {
      display: flex;
      justify-content: space-between;
      padding: 0 35px;
      margin-bottom: -10px;
      z-index: 3;
      position: relative;
    }

    .ear {
      width: 70px;
      height: 70px;
      background: linear-gradient(145deg, #fff4fb, #ffd4eb);
      border: 4px solid #ff8fc5;
      transform: rotate(45deg);
      border-radius: 12px;
      box-shadow: 0 8px 18px rgba(255, 105, 180, 0.2);
      position: relative;
    }

    .ear::after {
      content: "";
      position: absolute;
      inset: 14px;
      background: #ffb5d8;
      border-radius: 10px;
    }

    .calculator {
      background: rgba(255, 245, 251, 0.92);
      backdrop-filter: blur(10px);
      border: 4px solid #ff8fc5;
      border-radius: 35px;
      padding: 20px;
      box-shadow:
        0 20px 45px rgba(255, 20, 147, 0.22),
        inset 0 0 0 4px rgba(255,255,255,0.45);
      position: relative;
      overflow: hidden;
    }

    .calculator::before {
      content: "";
      position: absolute;
      top: -80px;
      right: -80px;
      width: 180px;
      height: 180px;
      background: radial-gradient(circle, rgba(255,255,255,0.55), transparent 70%);
      border-radius: 50%;
      pointer-events: none;
    }

    .kitty-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 16px;
      gap: 10px;
    }

    .kitty-title {
      font-size: 1.2rem;
      font-weight: 800;
      color: #ff3f9d;
      text-shadow: 1px 1px 0 white;
      letter-spacing: 0.5px;
    }

    .kitty-face {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .kitty-bow {
      width: 26px;
      height: 20px;
      background: #ff4ea2;
      border-radius: 10px;
      position: relative;
      animation: wiggle 2s ease-in-out infinite;
    }

    .kitty-bow::before,
    .kitty-bow::after {
      content: "";
      position: absolute;
      width: 14px;
      height: 14px;
      background: #ff73b8;
      border-radius: 50%;
      top: 3px;
    }

    .kitty-bow::before { left: -6px; }
    .kitty-bow::after { right: -6px; }

    @keyframes wiggle {
      0%, 100% { transform: rotate(0deg); }
      25% { transform: rotate(-8deg); }
      75% { transform: rotate(8deg); }
    }

    .screen-box {
      background: linear-gradient(180deg, #fff, #ffe9f5);
      border: 3px solid #ff9fcd;
      border-radius: 22px;
      padding: 16px;
      margin-bottom: 18px;
      box-shadow: inset 0 6px 14px rgba(255, 182, 193, 0.35);
    }

    .mini-status {
      display: flex;
      justify-content: space-between;
      font-size: 0.8rem;
      color: #ff68ae;
      margin-bottom: 8px;
      font-weight: 700;
    }

    .display {
      width: 100%;
      border: none;
      outline: none;
      background: transparent;
      text-align: right;
      font-size: 2.1rem;
      font-weight: 800;
      color: #ff2f8c;
      min-height: 50px;
      overflow-x: auto;
      white-space: nowrap;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    button {
      border: none;
      border-radius: 20px;
      padding: 16px 10px;
      font-size: 1.25rem;
      font-weight: 800;
      cursor: pointer;
      transition: all 0.18s ease;
      background: linear-gradient(180deg, #fff, #ffe7f4);
      color: #ff2f8c;
      box-shadow:
        0 8px 16px rgba(255, 105, 180, 0.18),
        inset 0 -3px 0 rgba(255, 192, 203, 0.35);
    }

    button:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow:
        0 12px 20px rgba(255, 105, 180, 0.25),
        inset 0 -3px 0 rgba(255, 192, 203, 0.35);
    }

    button:active {
      transform: scale(0.96);
    }

    .operator {
      background: linear-gradient(180deg, #ffd0e8, #ffb0d6);
      color: #fff;
    }

    .special {
      background: linear-gradient(180deg, #ffe7f4, #ffd1e8);
      color: #ff3b95;
    }

    .equal {
      background: linear-gradient(135deg, #ff4fa0, #ff79c8);
      color: #fff;
      font-size: 1.6rem;
      box-shadow:
        0 10px 22px rgba(255, 79, 160, 0.35),
        inset 0 -4px 0 rgba(255,255,255,0.15);
    }

    .zero {
      grid-column: span 2;
    }

    .kitty-footer {
      margin-top: 16px;
      text-align: center;
      color: #ff67ae;
      font-size: 0.9rem;
      font-weight: 700;
    }

    .premium-overlay {
      position: fixed;
      inset: 0;
      background: rgba(255, 105, 180, 0.18);
      backdrop-filter: blur(8px);
      display: none;
      align-items: center;
      justify-content: center;
      padding: 20px;
      z-index: 999;
      animation: fadeIn 0.25s ease;
    }

    .premium-overlay.active {
      display: flex;
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .premium-modal {
      width: 100%;
      max-width: 760px;
      background: linear-gradient(180deg, #fff7fc, #ffe8f5);
      border: 4px solid #ff8fc5;
      border-radius: 32px;
      padding: 24px;
      box-shadow: 0 30px 60px rgba(255, 20, 147, 0.25);
      position: relative;
      animation: modalPop 0.35s ease;
      overflow: hidden;
    }

    @keyframes modalPop {
      from {
        transform: scale(0.9) translateY(20px);
        opacity: 0;
      }
      to {
        transform: scale(1) translateY(0);
        opacity: 1;
      }
    }

    .premium-modal::before {
      content: "🎀 ✨ 💖 🎀 ✨ 💖 🎀 ✨ 💖";
      position: absolute;
      top: 8px;
      left: 18px;
      right: 18px;
      text-align: center;
      font-size: 1rem;
      opacity: 0.6;
      pointer-events: none;
    }

    .close-btn {
      position: absolute;
      top: 14px;
      right: 14px;
      width: 42px;
      height: 42px;
      border-radius: 50%;
      font-size: 1.4rem;
      background: linear-gradient(180deg, #ff9fd0, #ff6eb8);
      color: white;
      box-shadow: 0 8px 18px rgba(255, 105, 180, 0.25);
    }

    .premium-head {
      text-align: center;
      padding-top: 20px;
      margin-bottom: 18px;
    }

    .premium-head h2 {
      color: #ff2f8c;
      font-size: 2rem;
      margin-bottom: 8px;
    }

    .premium-head p {
      color: #ff5fa9;
      font-weight: 700;
      font-size: 1rem;
    }

    .premium-badge {
      display: inline-block;
      margin-top: 10px;
      padding: 8px 16px;
      border-radius: 999px;
      background: linear-gradient(135deg, #ff5aa8, #ff86ca);
      color: white;
      font-weight: 800;
      box-shadow: 0 8px 18px rgba(255, 105, 180, 0.22);
      animation: pulse 1.8s infinite;
    }

    @keyframes pulse {
      0%, 100% { transform: scale(1); }
      50% { transform: scale(1.05); }
    }

    .plans {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 18px;
      margin-top: 20px;
    }

    .plan {
      background: linear-gradient(180deg, #fff, #fff0f8);
      border: 3px solid #ffc0de;
      border-radius: 26px;
      padding: 20px;
      text-align: center;
      position: relative;
      transition: all 0.2s ease;
      box-shadow: 0 12px 24px rgba(255, 182, 193, 0.2);
      cursor: pointer;
    }

    .plan:hover {
      transform: translateY(-6px) scale(1.02);
      border-color: #ff7fbe;
      box-shadow: 0 18px 30px rgba(255, 105, 180, 0.24);
    }

    .plan.selected {
      border-color: #ff3f9d;
      transform: translateY(-4px) scale(1.02);
      box-shadow: 0 20px 34px rgba(255, 20, 147, 0.25);
      background: linear-gradient(180deg, #fff6fb, #ffdff0);
    }

    .plan.popular::before {
      content: "HOT";
      position: absolute;
      top: -12px;
      right: 14px;
      background: linear-gradient(135deg, #ff4fa0, #ff79c8);
      color: white;
      padding: 6px 12px;
      border-radius: 999px;
      font-size: 0.8rem;
      font-weight: 800;
      box-shadow: 0 8px 18px rgba(255, 105, 180, 0.25);
    }

    .plan .emoji {
      font-size: 2rem;
      margin-bottom: 8px;
    }

    .plan h3 {
      color: #ff2f8c;
      margin-bottom: 6px;
      font-size: 1.2rem;
    }

    .plan .price {
      font-size: 2rem;
      font-weight: 900;
      color: #ff3b95;
      margin: 10px 0;
    }

    .plan .sub {
      color: #ff6eb3;
      font-size: 0.9rem;
      font-weight: 700;
      margin-bottom: 12px;
    }

    .plan ul {
      list-style: none;
      text-align: left;
      color: #ff5fa9;
      font-weight: 700;
      font-size: 0.9rem;
      display: grid;
      gap: 8px;
    }

    .plan li::before {
      content: "✔ ";
      color: #ff2f8c;
    }

    .payment-box {
      margin-top: 20px;
      background: rgba(255,255,255,0.7);
      border: 2px dashed #ff9fcd;
      border-radius: 22px;
      padding: 18px;
    }

    .payment-box h4 {
      color: #ff2f8c;
      margin-bottom: 10px;
      font-size: 1.1rem;
    }

    .payment-methods {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-bottom: 14px;
    }

    .method {
      padding: 10px 14px;
      border-radius: 999px;
      background: #fff;
      border: 2px solid #ffd0e8;
      color: #ff4fa0;
      font-weight: 800;
      cursor: pointer;
      transition: 0.2s;
    }

    .method.active {
      border-color: #ff4fa0;
      background: #ffe8f5;
    }

    .buy-btn {
      width: 100%;
      padding: 16px;
      border-radius: 18px;
      background: linear-gradient(135deg, #ff4fa0, #ff7ec6);
      color: white;
      font-size: 1.15rem;
      font-weight: 900;
      box-shadow: 0 12px 24px rgba(255, 79, 160, 0.28);
    }

    .loading-area {
      margin-top: 14px;
      display: none;
      text-align: center;
    }

    .loading-area.active {
      display: block;
    }

    .loading-text {
      color: #ff4fa0;
      font-weight: 800;
      margin-bottom: 10px;
    }

    .progress-bar {
      width: 100%;
      height: 14px;
      background: #ffe2f1;
      border-radius: 999px;
      overflow: hidden;
      border: 2px solid #ffc0de;
    }

    .progress-fill {
      width: 0%;
      height: 100%;
      background: linear-gradient(90deg, #ff5aa8, #ff9fd0);
      border-radius: 999px;
      transition: width 0.2s linear;
    }

    .success-box {
      display: none;
      margin-top: 16px;
      text-align: center;
      background: linear-gradient(180deg, #fff, #fff1f8);
      border: 2px solid #ffb0d6;
      border-radius: 20px;
      padding: 16px;
      animation: popIn 0.4s ease;
    }

    .success-box.active {
      display: block;
    }

    .success-box h4 {
      color: #ff2f8c;
      margin-bottom: 8px;
      font-size: 1.2rem;
    }

    .success-box p {
      color: #ff5fa9;
      font-weight: 700;
      line-height: 1.5;
    }

    .sparkles {
      position: absolute;
      inset: 0;
      pointer-events: none;
      overflow: hidden;
    }

    .sparkle {
      position: absolute;
      font-size: 16px;
      animation: sparkleFly 1.2s ease forwards;
    }

    @keyframes sparkleFly {
      0% {
        opacity: 0;
        transform: translate(0,0) scale(0.5);
      }
      20% {
        opacity: 1;
      }
      100% {
        opacity: 0;
        transform: translate(var(--x), var(--y)) scale(1.4) rotate(220deg);
      }
    }

    @media (max-width: 820px) {
      .plans {
        grid-template-columns: 1fr;
      }

      .premium-modal {
        max-height: 90vh;
        overflow-y: auto;
      }
    }

    @media (max-width: 480px) {
      .calculator {
        padding: 16px;
      }

      button {
        padding: 14px 8px;
        font-size: 1.1rem;
      }

      .display {
        font-size: 1.7rem;
      }

      .premium-head h2 {
        font-size: 1.6rem;
      }
    }
  </style>
</head>
<body>
  <div class="floating-hearts" id="hearts"></div>
  <div class="floating-stars" id="stars"></div>

  <div class="calculator-wrapper">
    <div class="kitty-ears">
      <div class="ear"></div>
      <div class="ear"></div>
    </div>

    <div class="calculator">
      <div class="kitty-header">
        <div class="kitty-title">Hello Kitty Calculator 🎀</div>
        <div class="kitty-face">
          <div class="kitty-bow"></div>
          <span style="font-size: 1.4rem;">🐱</span>
        </div>
      </div>

      <div class="screen-box">
        <div class="mini-status">
          <span>Premium Locked</span>
          <span id="clock">19:10</span>
        </div>
        <input type="text" id="display" class="display" readonly placeholder="0" />
      </div>

      <div class="buttons">
        <button onclick="clearDisplay()" class="special">C</button>
        <button onclick="deleteLast()" class="special">⌫</button>
        <button onclick="appendValue('%')" class="operator">%</button>
        <button onclick="appendValue('/')" class="operator">÷</button>

        <button onclick="appendValue('7')">7</button>
        <button onclick="appendValue('8')">8</button>
        <button onclick="appendValue('9')">9</button>
        <button onclick="appendValue('*')" class="operator">×</button>

        <button onclick="appendValue('4')">4</button>
        <button onclick="appendValue('5')">5</button>
        <button onclick="appendValue('6')">6</button>
        <button onclick="appendValue('-')" class="operator">−</button>

        <button onclick="appendValue('1')">1</button>
        <button onclick="appendValue('2')">2</button>
        <button onclick="appendValue('3')">3</button>
        <button onclick="appendValue('+')" class="operator">+</button>

        <button onclick="appendValue('0')" class="zero">0</button>
        <button onclick="appendValue('.')">.</button>
        <button onclick="showPremium()" class="equal">=</button>
      </div>

      <div class="kitty-footer">✨ Bấm = để mở phép màu... à nhầm, trả tiền ✨</div>
    </div>
  </div>

  <div class="premium-overlay" id="premiumOverlay">
    <div class="premium-modal">
      <button class="close-btn" onclick="closePremium()">✕</button>

      <div class="premium-head">
        <h2>Upgrade to Premium 🎀</h2>
        <p>Mở khóa phép tính bằng cách nạp tiền như một công dân gương mẫu.</p>
        <div class="premium-badge">✨ HELLO KITTY VIP ACCESS ✨</div>
      </div>

      <div class="plans">
        <div class="plan" data-plan="week" data-price="49.000" onclick="selectPlan(this)">
          <div class="emoji">💗</div>
          <h3>Gói Tuần</h3>
          <div class="price">49K</div>
          <div class="sub">7 ngày đáng yêu</div>
          <ul>
            <li>Mở khóa dấu bằng</li>
            <li>Hiệu ứng tim bay</li>
            <li>Giao diện siêu hồng</li>
            <li>Calculator VIP</li>
          </ul>
        </div>

        <div class="plan popular selected" data-plan="month" data-price="199.000" onclick="selectPlan(this)">
          <div class="emoji">🎀</div>
          <h3>Gói Tháng</h3>
          <div class="price">199K</div>
          <div class="sub">30 ngày premium</div>
          <ul>
            <li>Tất cả tính năng tuần</li>
            <li>Ưu tiên xử lý giả vờ</li>
            <li>Animation xịn hơn</li>
            <li>Best value</li>
          </ul>
        </div>

        <div class="plan" data-plan="year" data-price="999.000" onclick="selectPlan(this)">
          <div class="emoji">👑</div>
          <h3>Gói Năm</h3>
          <div class="price">999K</div>
          <div class="sub">365 ngày hoàng gia</div>
          <ul>
            <li>Tất cả tính năng tháng</li>
            <li>VIP huyền thoại</li>
            <li>Glow hồng tối đa</li>
            <li>Đẳng cấp không ai hỏi</li>
          </ul>
        </div>
      </div>

      <div class="payment-box">
        <h4>Chọn phương thức thanh toán 💳</h4>
        <div class="payment-methods">
          <div class="method active" onclick="selectMethod(this)">Momo</div>
          <div class="method" onclick="selectMethod(this)">ZaloPay</div>
          <div class="method" onclick="selectMethod(this)">Banking</div>
          <div class="method" onclick="selectMethod(this)">Visa</div>
        </div>

        <button class="buy-btn" onclick="startPurchase()">
          Mua Premium ngay - <span id="buyPrice">199.000đ</span>
        </button>

        <div class="loading-area" id="loadingArea">
          <div class="loading-text" id="loadingText">Đang xử lý Premium...</div>
          <div class="progress-bar">
            <div class="progress-fill" id="progressFill"></div>
          </div>
        </div>

        <div class="success-box" id="successBox">
          <h4>Thanh toán thành công 🎉</h4>
          <p id="successText">
            Premium đã được kích hoạt. Nhưng vì đây là trò đùa xinh đẹp nên máy tính vẫn không tính đâu 💖
          </p>
        </div>
      </div>

      <div class="sparkles" id="sparkles"></div>
    </div>
  </div>

  <script>
    const display = document.getElementById("display");
    const premiumOverlay = document.getElementById("premiumOverlay");
    const buyPrice = document.getElementById("buyPrice");
    const loadingArea = document.getElementById("loadingArea");
    const progressFill = document.getElementById("progressFill");
    const loadingText = document.getElementById("loadingText");
    const successBox = document.getElementById("successBox");
    const successText = document.getElementById("successText");
    const sparkles = document.getElementById("sparkles");

    let selectedPlan = "month";
    let selectedPrice = "199.000";
    let selectedMethod = "Momo";
    let purchaseInterval = null;

    function appendValue(value) {
      if (display.value === "0" && value !== ".") {
        display.value = value;
      } else {
        display.value += value;
      }
      buttonBurst();
    }

    function clearDisplay() {
      display.value = "";
      buttonBurst();
    }

    function deleteLast() {
      display.value = display.value.slice(0, -1);
      buttonBurst();
    }

    function showPremium() {
      premiumOverlay.classList.add("active");
      createSparkleBurst();
      resetPurchaseUI();
    }

    function closePremium() {
      premiumOverlay.classList.remove("active");
      resetPurchaseUI();
    }

    function selectPlan(planEl) {
      document.querySelectorAll(".plan").forEach(plan => plan.classList.remove("selected"));
      planEl.classList.add("selected");

      selectedPlan = planEl.dataset.plan;
      selectedPrice = planEl.dataset.price;
      buyPrice.textContent = selectedPrice + "đ";

      createSparkleBurst();
    }

    function selectMethod(methodEl) {
      document.querySelectorAll(".method").forEach(method => method.classList.remove("active"));
      methodEl.classList.add("active");
      selectedMethod = methodEl.textContent;
    }

    function resetPurchaseUI() {
      if (purchaseInterval) {
        clearInterval(purchaseInterval);
        purchaseInterval = null;
      }
      loadingArea.classList.remove("active");
      successBox.classList.remove("active");
      progressFill.style.width = "0%";
      loadingText.textContent = "Đang xử lý Premium...";
    }

    function startPurchase() {
      resetPurchaseUI();
      loadingArea.classList.add("active");

      let progress = 0;
      const messages = [
        "Đang kết nối cổng thanh toán...",
        "Đang xác minh độ đáng yêu...",
        "Đang cấp quyền Hello Kitty VIP...",
        "Đang mở khóa dấu bằng...",
        "Đang hoàn tất phép màu..."
      ];

      purchaseInterval = setInterval(() => {
        progress += Math.floor(Math.random() * 12) + 8;
        if (progress > 100) progress = 100;

        progressFill.style.width = progress + "%";

        const msgIndex = Math.min(
          Math.floor((progress / 100) * messages.length),
          messages.length - 1
        );
        loadingText.textContent = messages[msgIndex];

        if (progress >= 100) {
          clearInterval(purchaseInterval);
          purchaseInterval = null;

          setTimeout(() => {
            loadingArea.classList.remove("active");
            successBox.classList.add("active");

            const planLabel =
              selectedPlan === "week" ? "Gói Tuần" :
              selectedPlan === "month" ? "Gói Tháng" :
              "Gói Năm";

            successText.innerHTML = `
              Bạn đã mua <b>${planLabel}</b> qua <b>${selectedMethod}</b> với giá <b>${selectedPrice}đ</b> 🎀<br>
              Premium đã bật thành công. Nhưng vì đây là máy tính lừa đảo đáng yêu nên dấu <b>=</b> vẫn chỉ mở popup thôi 💖
            `;

            createMegaBurst();
          }, 400);
        }
      }, 180);
    }

    function buttonBurst() {
      const calc = document.querySelector(".calculator");
      calc.animate(
        [
          { transform: "scale(1)" },
          { transform: "scale(1.01)" },
          { transform: "scale(1)" }
        ],
        {
          duration: 120,
          easing: "ease-out"
        }
      );
    }

    function createSparkleBurst() {
      const icons = ["✨", "💖", "🎀", "💗", "🌸"];
      for (let i = 0; i < 14; i++) {
        const s = document.createElement("div");
        s.className = "sparkle";
        s.textContent = icons[Math.floor(Math.random() * icons.length)];
        s.style.left = Math.random() * 100 + "%";
        s.style.top = Math.random() * 100 + "%";
        s.style.setProperty("--x", (Math.random() * 180 - 90) + "px");
        s.style.setProperty("--y", (Math.random() * 180 - 90) + "px");
        sparkles.appendChild(s);

        setTimeout(() => s.remove(), 1200);
      }
    }

    function createMegaBurst() {
      createSparkleBurst();
      setTimeout(createSparkleBurst, 180);
      setTimeout(createSparkleBurst, 360);
    }

    function spawnFloatingDecor() {
      const hearts = document.getElementById("hearts");
      const stars = document.getElementById("stars");
      const heartIcons = ["💖", "💗", "💕", "🌸"];
      const starIcons = ["✨", "⭐", "🎀"];

      setInterval(() => {
        const h = document.createElement("div");
        h.className = "heart";
        h.textContent = heartIcons[Math.floor(Math.random() * heartIcons.length)];
        h.style.left = Math.random() * 100 + "%";
        h.style.animationDuration = (6 + Math.random() * 5) + "s";
        h.style.fontSize = (14 + Math.random() * 12) + "px";
        hearts.appendChild(h);

        setTimeout(() => h.remove(), 12000);
      }, 500);

      setInterval(() => {
        const s = document.createElement("div");
        s.className = "star";
        s.textContent = starIcons[Math.floor(Math.random() * starIcons.length)];
        s.style.left = Math.random() * 100 + "%";
        s.style.animationDuration = (7 + Math.random() * 5) + "s";
        s.style.fontSize = (12 + Math.random() * 10) + "px";
        stars.appendChild(s);

        setTimeout(() => s.remove(), 12000);
      }, 700);
    }

    function updateClock() {
      const clock = document.getElementById("clock");
      const now = new Date();
      const h = String(now.getHours()).padStart(2, "0");
      const m = String(now.getMinutes()).padStart(2, "0");
      clock.textContent = `${h}:${m}`;
    }

    document.addEventListener("keydown", (e) => {
      const allowed = "0123456789+-*/.%";

      if (allowed.includes(e.key)) {
        appendValue(e.key);
      } else if (e.key === "Backspace") {
        deleteLast();
      } else if (e.key === "Escape") {
        clearDisplay();
      } else if (e.key === "Enter" || e.key === "=") {
        e.preventDefault();
        showPremium();
      }
    });

    premiumOverlay.addEventListener("click", (e) => {
      if (e.target === premiumOverlay) {
        closePremium();
      }
    });

    updateClock();
    setInterval(updateClock, 1000);
    spawnFloatingDecor();
  </script>
</body>
</html>
