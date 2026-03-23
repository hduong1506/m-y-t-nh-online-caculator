<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Hello Kitty Premium Calculator</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: "Comic Sans MS", "Segoe UI", cursive, sans-serif;
    }

    html {
      scroll-behavior: smooth;
    }

    body {
      min-height: 100vh;
      overflow-y: auto;
      background:
        radial-gradient(circle at 20% 20%, rgba(255,255,255,0.35), transparent 20%),
        radial-gradient(circle at 80% 30%, rgba(255,255,255,0.2), transparent 18%),
        radial-gradient(circle at 50% 80%, rgba(255,255,255,0.25), transparent 22%),
        linear-gradient(180deg, #ffd6ea, #ffb8d8, #ff9fc9);
      color: #8a1c57;
      position: relative;
    }

    body::before {
      content: "";
      position: fixed;
      inset: 0;
      pointer-events: none;
      background-image:
        radial-gradient(circle, rgba(255,255,255,0.55) 2px, transparent 2px),
        radial-gradient(circle, rgba(255,255,255,0.35) 1px, transparent 1px);
      background-size: 80px 80px, 40px 40px;
      background-position: 0 0, 20px 20px;
      opacity: 0.5;
      z-index: 0;
    }

    .floating-hearts {
      position: fixed;
      inset: 0;
      overflow: hidden;
      pointer-events: none;
      z-index: 1;
    }

    .heart {
      position: absolute;
      bottom: -50px;
      font-size: 18px;
      animation: floatUp linear infinite;
      opacity: 0.7;
      user-select: none;
    }

    @keyframes floatUp {
      0% {
        transform: translateY(0) scale(0.8) rotate(0deg);
        opacity: 0;
      }
      10% {
        opacity: 0.9;
      }
      100% {
        transform: translateY(-120vh) scale(1.4) rotate(360deg);
        opacity: 0;
      }
    }

    .page {
      position: relative;
      z-index: 2;
      width: 100%;
      max-width: 1000px;
      margin: 0 auto;
      padding: 30px 18px 80px;
    }

    .hero {
      text-align: center;
      margin-bottom: 24px;
      animation: fadeInDown 0.8s ease;
    }

    .hero h1 {
      font-size: clamp(1.8rem, 4vw, 3rem);
      color: #ff2f8b;
      text-shadow: 0 0 12px rgba(255,255,255,0.8);
      margin-bottom: 8px;
    }

    .hero p {
      font-size: 1rem;
      color: #9e2d64;
      font-weight: 700;
    }

    .kitty-badge {
      display: inline-flex;
      align-items: center;
      gap: 8px;
      background: rgba(255,255,255,0.55);
      border: 2px solid rgba(255,255,255,0.7);
      padding: 10px 18px;
      border-radius: 999px;
      margin-top: 12px;
      box-shadow: 0 10px 24px rgba(255, 85, 150, 0.18);
      backdrop-filter: blur(10px);
    }

    .main-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 24px;
    }

    @media (min-width: 900px) {
      .main-grid {
        grid-template-columns: 1.1fr 0.9fr;
        align-items: start;
      }
    }

    .card {
      background: rgba(255, 255, 255, 0.38);
      backdrop-filter: blur(14px);
      border: 2px solid rgba(255,255,255,0.55);
      border-radius: 30px;
      box-shadow: 0 18px 40px rgba(255, 71, 144, 0.22);
    }

    .calculator-card {
      padding: 22px;
      animation: fadeInUp 0.9s ease;
    }

    .calc-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      margin-bottom: 16px;
    }

    .calc-title {
      font-size: 1.2rem;
      font-weight: 900;
      color: #d81b72;
    }

    .calc-sub {
      font-size: 0.85rem;
      color: #a13667;
      font-weight: 700;
    }

    .display-wrap {
      margin-bottom: 18px;
      position: relative;
    }

    .display {
      width: 100%;
      height: 88px;
      border: none;
      outline: none;
      border-radius: 22px;
      padding: 16px 18px;
      text-align: right;
      font-size: clamp(1.6rem, 4vw, 2.4rem);
      font-weight: 900;
      color: #b5145c;
      background: rgba(255,255,255,0.8);
      box-shadow:
        inset 0 4px 10px rgba(255,255,255,0.8),
        inset 0 -4px 10px rgba(255, 160, 205, 0.35);
    }

    .display-hint {
      margin-top: 8px;
      font-size: 0.8rem;
      text-align: right;
      color: #9e2d64;
      font-weight: 700;
    }

    .buttons {
      display: grid;
      grid-template-columns: repeat(4, 1fr);
      gap: 12px;
    }

    button {
      border: none;
      outline: none;
      border-radius: 20px;
      min-height: 64px;
      cursor: pointer;
      font-size: 1.3rem;
      font-weight: 900;
      color: #b3155d;
      background: linear-gradient(180deg, #fff8fc, #ffd4e8);
      box-shadow:
        0 8px 16px rgba(255, 75, 149, 0.18),
        inset 0 2px 0 rgba(255,255,255,0.9);
      transition: transform 0.18s ease, box-shadow 0.18s ease, filter 0.18s ease;
      user-select: none;
    }

    button:hover {
      transform: translateY(-3px) scale(1.03);
      box-shadow:
        0 12px 24px rgba(255, 75, 149, 0.25),
        inset 0 2px 0 rgba(255,255,255,1);
      filter: brightness(1.03);
    }

    button:active {
      transform: translateY(1px) scale(0.98);
    }

    .operator {
      background: linear-gradient(180deg, #ffc2df, #ff9fcb);
      color: #fff;
      text-shadow: 0 1px 2px rgba(153, 0, 76, 0.35);
    }

    .special {
      background: linear-gradient(180deg, #ffe4f1, #ffc7e2);
      color: #c21868;
    }

    .equal {
      background: linear-gradient(135deg, #ff4fa0, #ff79c8);
      color: #fff;
      text-shadow: 0 1px 3px rgba(143, 0, 70, 0.4);
      box-shadow:
        0 10px 24px rgba(255, 79, 160, 0.35),
        inset 0 2px 0 rgba(255,255,255,0.35);
    }

    .zero {
      grid-column: span 2;
    }

    .side-panel {
      padding: 22px;
      animation: fadeInUp 1s ease;
    }

    .section-title {
      font-size: 1.25rem;
      color: #d81b72;
      font-weight: 900;
      margin-bottom: 14px;
    }

    .features {
      display: grid;
      gap: 12px;
      margin-bottom: 18px;
    }

    .feature {
      background: rgba(255,255,255,0.52);
      border: 2px solid rgba(255,255,255,0.6);
      border-radius: 20px;
      padding: 14px;
      font-weight: 700;
      color: #9e2d64;
      box-shadow: 0 8px 20px rgba(255, 92, 156, 0.12);
    }

    .premium-preview {
      background: linear-gradient(180deg, rgba(255,255,255,0.6), rgba(255,240,248,0.72));
      border-radius: 24px;
      padding: 18px;
      border: 2px dashed rgba(255, 87, 155, 0.35);
    }

    .premium-preview p {
      line-height: 1.6;
      font-weight: 700;
      color: #9e2d64;
    }

    .scroll-info {
      margin-top: 24px;
      padding: 20px;
      text-align: center;
      background: rgba(255,255,255,0.35);
      border-radius: 24px;
      border: 2px solid rgba(255,255,255,0.5);
      font-weight: 800;
      color: #9e2d64;
    }

    .footer-space {
      height: 500px;
      margin-top: 24px;
      border-radius: 30px;
      background:
        linear-gradient(180deg, rgba(255,255,255,0.2), rgba(255,255,255,0.08)),
        repeating-linear-gradient(
          45deg,
          rgba(255,255,255,0.18),
          rgba(255,255,255,0.18) 12px,
          rgba(255,255,255,0.06) 12px,
          rgba(255,255,255,0.06) 24px
        );
      border: 2px solid rgba(255,255,255,0.35);
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 20px;
      color: #a52763;
      font-size: 1.05rem;
      font-weight: 900;
    }

    .modal {
      position: fixed;
      inset: 0;
      display: none;
      align-items: center;
      justify-content: center;
      padding: 20px;
      background: rgba(113, 8, 53, 0.45);
      backdrop-filter: blur(8px);
      z-index: 999;
    }

    .modal.show {
      display: flex;
      animation: fadeIn 0.25s ease;
    }

    .modal-box {
      width: 100%;
      max-width: 760px;
      max-height: 90vh;
      overflow-y: auto;
      border-radius: 30px;
      padding: 24px;
      background:
        radial-gradient(circle at top right, rgba(255,255,255,0.5), transparent 35%),
        linear-gradient(180deg, #ffe3f1, #ffc6e1, #ffafd4);
      border: 3px solid rgba(255,255,255,0.7);
      box-shadow: 0 25px 60px rgba(146, 12, 72, 0.35);
      position: relative;
      animation: popIn 0.35s ease;
    }

    .close-btn {
      position: sticky;
      top: 0;
      margin-left: auto;
      display: block;
      width: 46px;
      height: 46px;
      border-radius: 50%;
      background: linear-gradient(180deg, #ff5aaa, #ff2f8b);
      color: #fff;
      font-size: 1.4rem;
      z-index: 5;
    }

    .modal-head {
      text-align: center;
      margin-bottom: 20px;
    }

    .modal-head h2 {
      font-size: clamp(1.6rem, 4vw, 2.4rem);
      color: #d40067;
      text-shadow: 0 0 10px rgba(255,255,255,0.65);
      margin-bottom: 8px;
    }

    .modal-head p {
      color: #8e2458;
      font-weight: 800;
      line-height: 1.6;
    }

    .plans {
      display: grid;
      grid-template-columns: 1fr;
      gap: 16px;
      margin: 20px 0;
    }

    @media (min-width: 720px) {
      .plans {
        grid-template-columns: repeat(3, 1fr);
      }
    }

    .plan {
      background: rgba(255,255,255,0.65);
      border: 2px solid rgba(255,255,255,0.75);
      border-radius: 26px;
      padding: 18px;
      text-align: center;
      box-shadow: 0 14px 26px rgba(255, 80, 150, 0.18);
      position: relative;
      overflow: hidden;
      transition: transform 0.2s ease, box-shadow 0.2s ease;
    }

    .plan:hover {
      transform: translateY(-4px);
      box-shadow: 0 18px 30px rgba(255, 80, 150, 0.25);
    }

    .plan.popular {
      border: 3px solid #ff3d97;
      transform: scale(1.02);
    }

    .ribbon {
      position: absolute;
      top: 12px;
      right: -32px;
      background: #ff3d97;
      color: #fff;
      font-size: 0.75rem;
      font-weight: 900;
      padding: 6px 36px;
      transform: rotate(35deg);
      box-shadow: 0 4px 10px rgba(0,0,0,0.15);
    }

    .plan h3 {
      font-size: 1.35rem;
      color: #d81b72;
      margin-bottom: 10px;
    }

    .price {
      font-size: 2rem;
      color: #ff2f8b;
      font-weight: 900;
      margin-bottom: 10px;
    }

    .price small {
      font-size: 0.9rem;
      color: #a13667;
    }

    .plan ul {
      list-style: none;
      text-align: left;
      display: grid;
      gap: 8px;
      margin: 14px 0 18px;
      color: #9b2f62;
      font-weight: 700;
      font-size: 0.95rem;
    }

    .buy-btn {
      width: 100%;
      min-height: 52px;
      border-radius: 18px;
      background: linear-gradient(135deg, #ff4fa0, #ff2f8b);
      color: #fff;
      font-size: 1rem;
      font-weight: 900;
    }

    .payment-box {
      margin-top: 22px;
      background: rgba(255,255,255,0.55);
      border-radius: 24px;
      padding: 18px;
      border: 2px solid rgba(255,255,255,0.65);
    }

    .payment-box h3 {
      color: #d81b72;
      margin-bottom: 10px;
    }

    .payment-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 12px;
      margin-top: 12px;
    }

    @media (min-width: 600px) {
      .payment-grid {
        grid-template-columns: repeat(2, 1fr);
      }
    }

    .payment-item {
      background: rgba(255,255,255,0.72);
      border-radius: 18px;
      padding: 14px;
      font-weight: 800;
      color: #9e2d64;
    }

    .success {
      display: none;
      margin-top: 18px;
      background: rgba(255,255,255,0.7);
      border: 2px solid rgba(255,255,255,0.8);
      border-radius: 22px;
      padding: 18px;
      text-align: center;
      color: #c21868;
      font-weight: 900;
      animation: pulseGlow 1s infinite alternate;
    }

    .success.show {
      display: block;
    }

    .tiny-note {
      margin-top: 14px;
      text-align: center;
      color: #8e2458;
      font-size: 0.9rem;
      font-weight: 700;
      line-height: 1.6;
    }

    @keyframes fadeInDown {
      from {
        opacity: 0;
        transform: translateY(-18px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @keyframes fadeInUp {
      from {
        opacity: 0;
        transform: translateY(22px);
      }
      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    @keyframes popIn {
      from {
        opacity: 0;
        transform: scale(0.92) translateY(18px);
      }
      to {
        opacity: 1;
        transform: scale(1) translateY(0);
      }
    }

    @keyframes pulseGlow {
      from {
        box-shadow: 0 0 0 rgba(255, 79, 160, 0);
      }
      to {
        box-shadow: 0 0 28px rgba(255, 79, 160, 0.35);
      }
    }

    .sparkle {
      position: fixed;
      font-size: 18px;
      pointer-events: none;
      z-index: 1000;
      animation: sparkleFly 1.1s ease forwards;
    }

    @keyframes sparkleFly {
      0% {
        opacity: 1;
        transform: translate(0, 0) scale(0.8);
      }
      100% {
        opacity: 0;
        transform: translate(var(--x), var(--y)) scale(1.5) rotate(180deg);
      }
    }

    /* Custom scrollbar */
    ::-webkit-scrollbar {
      width: 12px;
    }

    ::-webkit-scrollbar-track {
      background: rgba(255,255,255,0.35);
      border-radius: 20px;
    }

    ::-webkit-scrollbar-thumb {
      background: linear-gradient(180deg, #ff86c3, #ff4fa0);
      border-radius: 20px;
    }
  </style>
</head>
<body>
  <div class="floating-hearts" id="floatingHearts"></div>

  <div class="page">
    <section class="hero">
      <h1>🎀 Hello Kitty Premium Calculator 🎀</h1>
      <p>Máy tính màu hồng siêu cute, nhưng bấm dấu bằng là đòi tiền như đời thật 💸</p>
      <div class="kitty-badge">
        <span>🐱</span>
        <span>Premium Cute Mode Enabled</span>
        <span>💖</span>
      </div>
    </section>

    <section class="main-grid">
      <div class="card calculator-card">
        <div class="calc-header">
          <div>
            <div class="calc-title">Kitty Calculator</div>
            <div class="calc-sub">Bấm = để mở giao diện VIP</div>
          </div>
          <div style="font-size: 1.8rem;">🎀</div>
        </div>

        <div class="display-wrap">
          <input type="text" class="display" id="display" readonly placeholder="0" />
          <div class="display-hint">Enter trên bàn phím cũng mở Premium</div>
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
      </div>

      <div class="card side-panel">
        <div class="section-title">✨ Tính năng dễ thương</div>
        <div class="features">
          <div class="feature">💖 Giao diện Hello Kitty hồng pastel siêu nổi</div>
          <div class="feature">🎀 Nút bấm có hiệu ứng nổi + glow + hover</div>
          <div class="feature">💸 Bấm dấu bằng là hiện popup Upgrade to Premium</div>
          <div class="feature">🛍️ Có 3 gói VIP: tuần / tháng / năm</div>
          <div class="feature">🩷 Popup cuộn được, full trang cuộn mượt</div>
        </div>

        <div class="premium-preview">
          <div class="section-title" style="font-size:1.1rem; margin-bottom:10px;">Preview logic "máy tính online"</div>
          <p>
            Máy tính thì tính được... nhưng không cho ra kết quả 😌<br />
            Khi ấn dấu bằng, nó sẽ mở giao diện "Upgrade to Premium" với 3 gói:
            <strong>49K / 199K / 999K</strong>.<br /><br />
            Đúng kiểu app thời nay: cho dùng miễn phí nửa bước rồi chặn cổng thu phí.
          </p>
        </div>
      </div>
    </section>

    <div class="scroll-info">
      👇 Kéo xuống dưới vẫn còn nội dung để test scroll full trang
    </div>

    <div class="footer-space">
      <div>
        🌸 Đây là vùng nội dung kéo xuống để trang web có thể lướt lên lướt xuống full.<br /><br />
        Chủ nhân up lên GitHub Pages là chạy luôn, không cần tách file.<br />
        Con người thích làm mọi thứ phức tạp, nên tôi gộp hết vào 1 file cho đỡ phá. 💀
      </div>
    </div>
  </div>

  <!-- Modal Premium -->
  <div class="modal" id="premiumModal">
    <div class="modal-box">
      <button class="close-btn" onclick="closePremium()">✕</button>

      <div class="modal-head">
        <h2>🎀 Upgrade to Premium 🎀</h2>
        <p>
          Oops~ Tính kết quả là tính năng VIP nha 🩷<br />
          Muốn biết đáp án? Trả tiền. Đúng tinh thần internet hiện đại.
        </p>
      </div>

      <div class="plans">
        <div class="plan">
          <h3>💗 Gói Tuần</h3>
          <div class="price">49K <small>/ tuần</small></div>
          <ul>
            <li>✔ Xem kết quả phép tính</li>
            <li>✔ Giao diện hồng premium</li>
            <li>✔ Hiệu ứng tim lung tung</li>
            <li>✔ Ưu tiên... giả vờ</li>
          </ul>
          <button class="buy-btn" onclick="buyPlan('Gói Tuần - 49K')">Mua ngay 💖</button>
        </div>

        <div class="plan popular">
          <div class="ribbon">HOT</div>
          <h3>🎀 Gói Tháng</h3>
          <div class="price">199K <small>/ tháng</small></div>
          <ul>
            <li>✔ Tất cả tính năng gói tuần</li>
            <li>✔ Hiệu ứng sang chảnh hơn</li>
            <li>✔ Huy hiệu VIP Kitty</li>
            <li>✔ Trải nghiệm đốt tiền ổn định</li>
          </ul>
          <button class="buy-btn" onclick="buyPlan('Gói Tháng - 199K')">Mua ngay 🌸</button>
        </div>

        <div class="plan">
          <h3>👑 Gói Năm</h3>
          <div class="price">999K <small>/ năm</small></div>
          <ul>
            <li>✔ Full bộ tính năng VIP</li>
            <li>✔ Giao diện nữ hoàng mèo</li>
            <li>✔ Hiệu ứng premium max level</li>
            <li>✔ Cảm giác giàu hơn một chút</li>
          </ul>
          <button class="buy-btn" onclick="buyPlan('Gói Năm - 999K')">Mua ngay ✨</button>
        </div>
      </div>

      <div class="payment-box">
        <h3>💳 Phương thức thanh toán giả lập</h3>
        <div class="payment-grid">
          <div class="payment-item">🏦 MB Bank: 1234 5678 9999</div>
          <div class="payment-item">📱 Momo: 0900 123 456</div>
          <div class="payment-item">🧾 Nội dung CK: BUY KITTY VIP</div>
          <div class="payment-item">🔒 Trạng thái: Fake demo only</div>
        </div>

        <div class="success" id="successBox">
          🩷 Thanh toán thành công (giả lập)!<br />
          Chào mừng đến với Hello Kitty Premium VIP ✨
        </div>

        <div class="tiny-note">
          Đây là giao diện demo để làm web đẹp mắt.<br />
          Không có backend thật, nên bấm mua chỉ hiện hiệu ứng thôi.
        </div>
      </div>
    </div>
  </div>

  <script>
    const display = document.getElementById("display");
    const premiumModal = document.getElementById("premiumModal");
    const successBox = document.getElementById("successBox");
    const floatingHearts = document.getElementById("floatingHearts");

    function appendValue(value) {
      if (display.value === "0" && value !== ".") {
        display.value = value;
      } else {
        display.value += value;
      }
      burstSparkles();
    }

    function clearDisplay() {
      display.value = "";
      burstSparkles();
    }

    function deleteLast() {
      display.value = display.value.slice(0, -1);
      burstSparkles();
    }

    function showPremium() {
      premiumModal.classList.add("show");
      document.body.style.overflow = "hidden";
      burstSparkles(18);
    }

    function closePremium() {
      premiumModal.classList.remove("show");
      document.body.style.overflow = "auto";
      successBox.classList.remove("show");
    }

    function buyPlan(planName) {
      successBox.innerHTML = `🩷 Đã chọn <strong>${planName}</strong><br>Thanh toán thành công (giả lập)! Welcome to Premium ✨`;
      successBox.classList.add("show");
      burstSparkles(28);
    }

    function calculateResult() {
      showPremium();
    }

    document.addEventListener("keydown", function (e) {
      const key = e.key;

      if (!isNaN(key) || ["+", "-", "*", "/", ".", "%"].includes(key)) {
        appendValue(key);
      } else if (key === "Backspace") {
        deleteLast();
      } else if (key === "Escape") {
        closePremium();
      } else if (key === "Enter" || key === "=") {
        e.preventDefault();
        calculateResult();
      }
    });

    premiumModal.addEventListener("click", function (e) {
      if (e.target === premiumModal) {
        closePremium();
      }
    });

    function createFloatingHeart() {
      const heart = document.createElement("div");
      heart.className = "heart";
      heart.innerHTML = Math.random() > 0.5 ? "💖" : "🎀";
      heart.style.left = Math.random() * 100 + "vw";
      heart.style.animationDuration = (5 + Math.random() * 5) + "s";
      heart.style.fontSize = (14 + Math.random() * 18) + "px";
      heart.style.opacity = 0.4 + Math.random() * 0.6;
      floatingHearts.appendChild(heart);

      setTimeout(() => {
        heart.remove();
      }, 11000);
    }

    setInterval(createFloatingHeart, 650);

    function burstSparkles(count = 8) {
      for (let i = 0; i < count; i++) {
        const sparkle = document.createElement("div");
        sparkle.className = "sparkle";
        sparkle.textContent = Math.random() > 0.5 ? "✨" : "💖";
        sparkle.style.left = (window.innerWidth / 2 + (Math.random() * 180 - 90)) + "px";
        sparkle.style.top = (window.innerHeight / 2 + (Math.random() * 180 - 90)) + "px";
        sparkle.style.setProperty("--x", (Math.random() * 240 - 120) + "px");
        sparkle.style.setProperty("--y", (Math.random() * 240 - 120) + "px");
        document.body.appendChild(sparkle);

        setTimeout(() => sparkle.remove(), 1100);
      }
    }

    // Initial cute hearts
    for (let i = 0; i < 12; i++) {
      setTimeout(createFloatingHeart, i * 180);
    }
  </script>
</body>
</html>
