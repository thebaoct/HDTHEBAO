<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tool In Hóa Đơn - THE BÁO V3.9 (New ID Format)</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        /* --- CẤU HÌNH CƠ BẢN --- */
        body, input, select, button, table, h1, h2, h3, p, span, div, textarea {
            font-family: Arial, sans-serif !important;
        }
        body { background-color: #e9ecef; margin: 0; padding: 20px; display: flex; gap: 20px; justify-content: center; align-items: flex-start; flex-wrap: wrap; }
        
        /* Bảng điều khiển */
        .control-panel { background: white; padding: 25px; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); width: 480px; }
        .control-panel h2 { margin-top: 0; color: #333; font-size: 18px; border-bottom: 2px solid #eee; padding-bottom: 10px; margin-bottom: 15px; }
        
        .form-row { display: flex; gap: 10px; }
        .form-group { margin-bottom: 10px; flex: 1; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: bold; font-size: 13px; color: #444; }
        .form-group input, .form-group select { width: 100%; padding: 8px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box; font-size: 14px; }
        .form-group input:focus { border-color: #007bff; outline: none; }

        .price-preview { color: #d35400; font-weight: bold; font-size: 13px; margin-top: 5px; display: block; text-align: right; }

        /* Input động */
        #dynamicInputs { background: #f1f8ff; border: 1px solid #cce5ff; padding: 10px; border-radius: 5px; margin-bottom: 10px; max-height: 250px; overflow-y: auto; }
        .code-entry-row { display: flex; gap: 5px; margin-bottom: 5px; align-items: center; }
        .code-entry-row span { font-size: 12px; font-weight: bold; width: 20px; }
        .code-entry-row input { font-size: 13px; padding: 6px; }

        /* Paste nhanh */
        .bulk-import-box { background: #fff3cd; border: 1px dashed #eba312; padding: 10px; border-radius: 5px; margin-bottom: 15px; }
        .bulk-import-box textarea { width: 100%; height: 60px; border: 1px solid #ddd; font-size: 12px; resize: vertical; box-sizing: border-box; }
        .bulk-btn { background: #856404; color: white; border: none; padding: 5px 10px; border-radius: 3px; cursor: pointer; font-size: 12px; margin-top: 5px; }

        /* Buttons */
        .btn { width: 100%; padding: 10px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold; color: white; transition: 0.2s; font-size: 13px; margin-top: 5px; }
        .btn-primary { background-color: #007bff; }
        .btn-warning { background-color: #f39c12; color: #fff; }
        .btn-success { background-color: #27ae60; }
        .btn-share { background-color: #6f42c1; }
        .btn-info { background-color: #17a2b8; }
        .btn-danger { background-color: #e74c3c; padding: 4px 8px; font-size: 12px; width: auto; margin-top: 0;}
        .btn-secondary { background-color: #6c757d; }
        .btn-excel { background-color: #217346; color: white; font-size: 12px; width: auto; padding: 5px 10px; margin-top: 0; }
        .btn:hover { opacity: 0.9; }
        .btn-group { display: flex; gap: 5px; margin-top: 15px; flex-wrap: wrap; }

        /* Payment Buttons */
        .payment-options { display: flex; gap: 10px; margin-bottom: 15px; }
        .pay-btn { flex: 1; padding: 8px; border: 1px solid #ddd; background: #f8f9fa; cursor: pointer; border-radius: 5px; font-size: 13px; font-weight: bold; }
        .pay-btn.active { background: #007bff; color: white; border-color: #007bff; }
        
        /* Revenue & History */
        .revenue-box { margin-top: 20px; background: #2c3e50; color: white; padding: 15px; border-radius: 8px; text-align: center; }
        .revenue-box h3 { margin: 0; font-size: 14px; font-weight: normal; color: #ccc; }
        .revenue-box .money { font-size: 24px; font-weight: bold; color: #2ecc71; display: block; margin: 5px 0; }
        
        .temp-list { margin-top: 15px; background: #f8f9fa; padding: 10px; border-radius: 8px; border: 1px dashed #ccc; }
        .temp-item { display: flex; justify-content: space-between; align-items: center; background: #fff; padding: 5px 10px; margin-bottom: 5px; border-radius: 4px; border: 1px solid #eee; font-size: 13px;}

        /* --- LỊCH SỬ NÂNG CAO --- */
        .history-section { margin-top: 20px; border-top: 2px solid #eee; padding-top: 15px; }
        .history-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 10px; flex-wrap: wrap; gap: 10px; }
        .history-search { width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 4px; font-size: 13px; margin-bottom: 5px; box-sizing: border-box;}
        
        .history-list { max-height: 300px; overflow-y: auto; border: 1px solid #ddd; background: #fff; border-radius: 5px; }
        .history-item { padding: 8px; border-bottom: 1px solid #eee; font-size: 12px; display: flex; justify-content: space-between; align-items: center; }
        .history-item:last-child { border-bottom: none; }
        
        .history-actions { display: flex; gap: 5px; }
        .btn-mini { padding: 3px 6px; font-size: 11px; border: none; border-radius: 3px; cursor: pointer; color: white; }
        .btn-edit { background-color: #f39c12; }
        .btn-del { background-color: #e74c3c; }

        /* --- HÓA ĐƠN --- */
        .invoice-box { width: 80mm; background: white; padding: 15px; box-shadow: 0 0 15px rgba(0,0,0,0.1); color: #000; font-size: 13px; box-sizing: border-box; line-height: 1.4; position: relative; }
        .text-center { text-align: center; }
        .text-right { text-align: right; }
        .text-bold { font-weight: bold; }
        .dashed-line { border-top: 1px dashed #000; margin: 10px 0; }
        .solid-line { border-top: 2px solid #000; margin: 10px 0; }
        .info-row { display: flex; justify-content: space-between; margin: 5px 0; }
        
        /* BẢNG KẺ Ô */
        .bill-table { width: 100%; border-collapse: collapse; margin-bottom: 5px; border: 1px solid #000; }
        .bill-table th { text-align: center; border: 1px solid #000; padding: 5px 2px; font-size: 11px; font-weight: bold; background-color: #eee; }
        .bill-table td { border: 1px solid #000; padding: 5px; vertical-align: middle; font-size: 12px; }

        .prod-name { font-weight: bold; display: block; margin-bottom: 2px; font-size: 13px; }
        .card-detail-line { display: block; font-size: 11px; color: #000; margin-top: 2px; border-bottom: 1px dotted #999; padding-bottom: 2px; font-family: 'Courier New', Courier, monospace; letter-spacing: 0px; }
        .card-detail-line:last-child { border-bottom: none; }

        .provisional-hidden .card-detail-line { display: none !important; }
        .provisional-hidden .hidden-msg { display: block !important; font-style: italic; color: #555; font-size: 11px; }
        .hidden-msg { display: none; }

        .qr-section { text-align: center; margin-top: 10px; display: none; }
        .qr-section img { width: 70%; max-width: 150px; height: auto; }
        .qr-note { font-size: 11px; margin-top: 5px; }

        @media print {
            body { background: white; padding: 0; margin: 0; visibility: hidden; }
            .control-panel, .revenue-box { display: none; }
            .invoice-box { visibility: visible; position: absolute; left: 0; top: 0; width: 100%; box-shadow: none; padding: 0 5px; }
            @page { margin: 0; size: auto; }
        }
    </style>
</head>
<body>

    <div class="control-panel">
        <h2>1. Thông tin đơn hàng</h2>
        
        <div class="form-row">
            <div class="form-group">
                <label>Mã HĐ</label>
                <input type="text" id="inpInvoiceID" style="font-weight:bold; color:#d35400;" oninput="updateInvoiceHeader(); renderQR();">
            </div>
            <div class="form-group">
                <label>Ngày</label>
                <input type="date" id="inpDate" onchange="updateInvoiceHeader()">
            </div>
            <div class="form-group">
                <label>Giờ</label>
                <input type="time" id="inpTime" onchange="updateInvoiceHeader()">
            </div>
        </div>

        <div class="form-row">
            <div class="form-group" style="flex: 2;">
                <label>Tên khách hàng</label>
                <input type="text" id="inpName" value="Khách lẻ" oninput="updateInvoiceHeader()">
            </div>
            <div class="form-group" style="flex: 1.5;">
                 <label>SĐT</label>
                <input type="text" id="inpPhone" maxlength="10" oninput="this.value = this.value.replace(/[^0-9]/g, ''); updateInvoiceHeader()" placeholder="09xxxxxxxx">
            </div>
        </div>

        <div class="dashed-line" style="border-color: #eee;"></div>

        <h2>2. Nhập sản phẩm</h2>
        <div class="form-row">
            <div class="form-group">
                <label>Loại thẻ</label>
                <select id="inpCardType" onchange="handleCardChange()"></select>
            </div>
            <div class="form-group">
                <label>Mệnh giá</label>
                <select id="inpPriceSelect" onchange="calculateSellingPrice()"></select>
                <input type="number" id="inpPriceCustom" placeholder="Nhập số tiền..." style="display:none;" oninput="calculateSellingPrice()">
                <span id="sellingPricePreview" class="price-preview">Giá bán: 0 đ</span>
            </div>
        </div>
        
        <div class="bulk-import-box">
            <label style="font-size:12px; font-weight:bold; color:#856404;">📋 PASTE NHANH:</label>
            <textarea id="bulkArea" placeholder="Ví dụ: 11112222 [tab] 99998888"></textarea>
            <button type="button" class="bulk-btn" onclick="processBulkData()">⚡ Tự động điền</button>
        </div>

        <div class="form-group">
            <label>Số lượng:</label>
            <input type="number" id="inpQty" value="1" min="1" oninput="renderInputFields()">
        </div>

        <div id="dynamicInputs"></div>

        <button class="btn btn-warning" onclick="addToCart()">⬇ THÊM VÀO ĐƠN (Enter)</button>

        <div class="temp-list">
            <div style="display:flex; justify-content:space-between; margin-bottom:5px;">
                <span style="font-weight:bold; color:#555">DS Chờ in:</span>
                <span id="itemCountBadge" style="background:#007bff; color:white; padding:2px 8px; border-radius:10px; font-size:12px;">0</span>
            </div>
            <div id="cartListUI"></div>
        </div>

        <div class="form-group" style="margin-top: 15px; border-top: 1px dashed #ccc; padding-top:10px;">
            <label style="color: #e74c3c;">🏷️ Chiết khấu / Giảm giá (VNĐ):</label>
            <input type="number" id="inpDiscount" value="" placeholder="Nhập số tiền giảm..." oninput="renderAll()">
        </div>

        <div style="margin-top: 15px;">
            <label style="font-weight:bold; font-size:13px; margin-bottom:5px; display:block;">💳 Phương thức thanh toán:</label>
            <div class="payment-options">
                <button class="pay-btn active" onclick="setPayment('CASH')" id="btnPayCash">💵 Tiền mặt</button>
                <button class="pay-btn" onclick="setPayment('MB')" id="btnPayMB">🏦 MB Bank</button>
                <button class="pay-btn" onclick="setPayment('VP')" id="btnPayVP">🏦 VP Bank</button>
            </div>
        </div>

        <div class="btn-group">
            <button class="btn btn-primary" onclick="resetCart()" style="flex:1;">Làm mới</button>
            <button class="btn btn-secondary" onclick="printProvisional()" style="flex:1;">📄 IN TẠM</button>
            <button class="btn btn-secondary" onclick="shareProvisionalImage()" style="flex:1; background-color: #607d8b;">📤 SHARE TẠM</button>
        </div>
        <div class="btn-group" style="margin-top:5px;">
            <button class="btn btn-success" onclick="printOfficial()" style="flex:1.5;">🖨 IN CHÍNH THỨC</button>
            <button class="btn btn-share" onclick="shareInvoiceImage()" style="flex:1;">📤 CHIA SẺ</button>
            <button class="btn btn-info" onclick="downloadInvoiceImage()" style="flex:1;">⬇ TẢI VỀ</button>
        </div>

        <div class="revenue-box">
            <h3>Doanh thu hôm nay</h3>
            <span class="money" id="dailyTotalDisplay">0 đ</span>
            <button class="btn-danger" onclick="resetDailyRevenue()" style="width: auto; padding: 5px 10px;">Chốt ngày (Reset)</button>
        </div>

        <div class="history-section">
            <div class="history-header">
                <h3 style="font-size: 14px; margin: 0; color:#555;">🕒 Lịch sử đơn hàng (Không giới hạn)</h3>
                <button class="btn btn-excel" onclick="exportHistoryToExcel()">📥 Xuất Excel</button>
            </div>
            
            <input type="text" id="inpSearchHistory" class="history-search" placeholder="🔍 Tìm kiếm theo Mã HĐ hoặc SĐT..." oninput="loadHistory()">

            <div class="history-list" id="historyListUI">
                <p style="padding:10px; color:#999; text-align:center;">Chưa có đơn hàng nào.</p>
            </div>
        </div>
    </div>

    <div class="invoice-box" id="invoiceArea">
        <div class="text-center">
            <h3 style="margin: 5px 0 0 0; font-size: 20px; text-transform: uppercase; font-weight: 800; color: #000;">THE BÁO</h3>
            <p style="margin: 2px 0 5px 0; font-size: 13px; text-transform: uppercase; font-weight: bold;">Dịch vụ nạp game trực tuyến</p>
            <p style="margin: 2px 0;">Uy tín - Nhanh chóng - Bảo mật</p>
            <p style="margin: 2px 0; font-weight: bold;">☎ 0354.830.400</p>
        </div>

        <div class="dashed-line"></div>

        <div class="text-center">
            <h2 style="margin: 5px 0; font-size: 16px;" id="lblInvoiceTitle">HÓA ĐƠN BÁN HÀNG</h2>
            <p style="margin: 2px 0; font-weight:bold; font-size: 14px;" id="outInvoiceID">Số: TB...</p> 
            <p style="margin: 0; font-size: 12px;" id="outDate">...</p>
        </div>

        <div class="dashed-line"></div>

        <div class="info-row">
            <span>Khách hàng:</span>
            <span id="outName" class="text-bold">Khách lẻ</span>
        </div>
        <div class="info-row" id="rowPhone" style="display:none;">
            <span>SĐT:</span>
            <span id="outPhone">...</span>
        </div>

        <div class="solid-line"></div>

        <table class="bill-table">
            <thead>
                <tr>
                    <th style="width: 40%;">Tên SP</th>
                    <th class="text-center" style="width: 15%;">SL</th>
                    <th class="text-center" style="width: 20%;">Đơn giá</th>
                    <th class="text-center" style="width: 25%;">T.Tiền</th>
                </tr>
            </thead>
            <tbody id="invoiceTableBody"></tbody>
        </table>

        <div class="solid-line"></div>

        <div style="margin-top: 10px;">
            <div class="info-row" style="font-size: 13px;">
                <span>Tổng tiền hàng:</span>
                <span id="outSubTotal">0</span>
            </div>
            <div class="info-row" style="font-size: 13px; display:none;" id="rowDiscount">
                <span>Chiết khấu:</span>
                <span id="outDiscountVal" style="color:#d35400;">0</span>
            </div>
            <div class="info-row" style="font-size: 16px; margin-top: 5px;">
                <span class="text-bold">THANH TOÁN:</span>
                <span id="outFinalTotal" class="text-bold">0</span>
            </div>
            <div class="info-row" style="font-size: 13px; margin-top: 5px; font-style: italic;">
                <span>Hình thức TT:</span>
                <span id="outPaymentMethod">Tiền mặt</span>
            </div>
        </div>

        <div class="qr-section" id="qrSection">
            <div class="dashed-line"></div>
            <p class="qr-note" style="font-weight:bold; margin-bottom:5px;">Quét mã QR để thanh toán:</p>
            <img id="qrImage" src="" alt="QR Code">
            <p class="qr-note" id="qrBankName">Ngân hàng MB</p>
            <p class="qr-note" id="qrAccNum">STK: 0354830400</p>
        </div>
        
        <div class="dashed-line"></div>

        <div class="footer text-center" style="margin-top: 15px; font-style: italic; font-size: 12px;">
            <p style="margin: 3px;">Cảm ơn quý khách đã sử dụng dịch vụ!</p>
            <p style="margin: 3px;">Hỗ trợ / Khiếu nại Zalo: 0354.830.400</p>
        </div>
    </div>

    <script>
        const cardsData = {
            "Garena": [5000, 10000, 20000, 50000, 100000, 200000, 500000],
            "Gate": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000, 5000000, 10000000],
            "Oncash": [20000, 50000, 100000, 200000, 500000],
            "Zing": [20000, 50000, 100000, 200000, 500000, 1000000],
            "Vcoin": [20000, 50000, 100000, 200000, 500000, 1000000, 5000000],
            "Kul": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000],
            "Funcard": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000],
            "Coin": [20000, 50000, 100000, 200000, 500000, 1000000],
            "Appota Card": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000],
            "iSec": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000, 5000000, 10000000],
            "Gosu": [20000, 50000, 100000, 200000, 500000, 1000000],
            "9play": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000],
            "Sohacoin": [20000, 50000, 100000, 200000, 500000, 1000000, 2000000, 5000000, 10000000],
            "Viettel": [], "Vinaphone": [10000, 20000, 50000, 100000, 200000, 500000],
            "Mobifone": [10000, 20000, 50000, 100000, 200000, 500000],
            "Vietnamobile": [20000, 50000, 100000, 200000, 500000],
            "Gmobile": [50000, 100000, 200000, 500000],
            "Steam": [75000, 100000, 120000, 150000, 200000, 500000],
            "Google Play": []
        };

        let cartItems = []; 
        let currentPaymentMethod = 'CASH';
        
        const cardSelect = document.getElementById('inpCardType');
        const priceSelect = document.getElementById('inpPriceSelect');
        const priceCustom = document.getElementById('inpPriceCustom');
        const pricePreview = document.getElementById('sellingPricePreview');
        const discountInput = document.getElementById('inpDiscount');
        const formatMoney = (num) => num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
        const formatReadableCode = (code) => code ? code.replace(/\s+/g, '').replace(/(.{4})/g, '$1 ').trim() : "";

        function init() {
            for (let card in cardsData) {
                let option = document.createElement("option");
                option.value = card;
                option.text = card;
                cardSelect.appendChild(option);
            }
            const now = new Date();
            document.getElementById('inpTime').value = now.toTimeString().substring(0,5);
            document.getElementById('inpDate').valueAsDate = now;
            generateRandomInvoiceID();
            handleCardChange(); 
            updateInvoiceHeader();
            renderInputFields();
            loadDailyRevenue();
            loadHistory();
            document.addEventListener('keydown', function(event) {
                if (event.key === "Enter" && (document.activeElement.tagName === 'INPUT')) addToCart();
            });
        }

        function setPayment(method) {
            currentPaymentMethod = method;
            document.querySelectorAll('.pay-btn').forEach(b => b.classList.remove('active'));
            document.getElementById(`btnPay${method}`).classList.add('active');
            const methodText = method === 'CASH' ? 'Tiền mặt' : (method === 'MB' ? 'Chuyển khoản MB' : 'Chuyển khoản VP');
            document.getElementById('outPaymentMethod').innerText = methodText;
            renderQR();
        }

        function renderQR() {
            const qrSection = document.getElementById('qrSection');
            if (currentPaymentMethod === 'CASH') { qrSection.style.display = 'none'; return; }
            let totalBill = cartItems.reduce((s, i) => s + i.total, 0);
            let discount = parseInt(discountInput.value) || 0;
            let finalTotal = totalBill - discount;
            if (finalTotal <= 0) { qrSection.style.display = 'none'; return; }

            qrSection.style.display = 'block';
            const bankID = currentPaymentMethod === 'MB' ? 'MB' : 'VPB';
            const accNum = '0354830400';
            const invID = document.getElementById('inpInvoiceID').value;
            const content = "Thanh toan " + invID; 
            const safeContent = encodeURIComponent(content);
            document.getElementById('qrBankName').innerText = currentPaymentMethod === 'MB' ? 'Ngân hàng Quân Đội (MB)' : 'Ngân hàng VPBank';
            const qrUrl = `https://img.vietqr.io/image/${bankID}-${accNum}-compact.png?amount=${finalTotal}&addInfo=${safeContent}`;
            document.getElementById('qrImage').src = qrUrl;
        }

        function handleCardChange() {
            const selectedCard = cardSelect.value;
            const prices = cardsData[selectedCard];
            if (prices.length === 0) {
                priceSelect.style.display = "none";
                priceCustom.style.display = "block";
                priceCustom.value = "";
                priceCustom.focus();
            } else {
                priceSelect.style.display = "block";
                priceCustom.style.display = "none";
                priceSelect.innerHTML = "";
                prices.forEach(price => {
                    let option = document.createElement("option");
                    option.value = price;
                    option.text = formatMoney(price);
                    priceSelect.appendChild(option);
                });
            }
            calculateSellingPrice();
        }

        function calculateSellingPrice() {
            const cardType = cardSelect.value;
            let faceValue = 0; 
            if (cardsData[cardType].length === 0) faceValue = parseInt(priceCustom.value) || 0;
            else faceValue = parseInt(priceSelect.value) || 0;
            let sellingPrice = faceValue;
            if (cardType === "Steam") sellingPrice = faceValue + (faceValue * 0.20); 
            else if (cardType === "Google Play") sellingPrice = faceValue + (faceValue * 0.04); 
            pricePreview.innerText = `Giá bán: ${formatMoney(sellingPrice)} đ`;
            return { faceValue, sellingPrice };
        }

        function addToCart() {
            const cardType = cardSelect.value;
            const qty = parseInt(document.getElementById('inpQty').value);
            const { faceValue, sellingPrice } = calculateSellingPrice();
            if (faceValue <= 0) return alert("Vui lòng chọn hoặc nhập mệnh giá hợp lệ!");
            if (qty < 1) return alert("Số lượng phải lớn hơn 0");
            let cardList = [];
            const codeInputs = document.querySelectorAll('.sub-code');
            const serialInputs = document.querySelectorAll('.sub-serial');
            for(let i=0; i<qty; i++) {
                cardList.push({ code: codeInputs[i].value.trim(), serial: serialInputs[i].value.trim() });
            }
            cartItems.push({ cardType, faceValue, price: sellingPrice, qty, cardList, total: sellingPrice * qty });
            renderAll();
            document.getElementById('inpQty').value = 1;
            renderInputFields();
            if (priceCustom.style.display !== 'none') { priceCustom.value = ""; priceCustom.focus(); } 
            else { if(document.querySelector('.sub-code')) document.querySelector('.sub-code').focus(); }
            calculateSellingPrice(); 
        }

        function processBulkData() {
            const raw = document.getElementById('bulkArea').value;
            if(!raw.trim()) return alert("Hãy dán mã thẻ vào ô!");
            const lines = raw.trim().split(/\n/);
            document.getElementById('inpQty').value = lines.length;
            renderInputFields();
            const codeInputs = document.querySelectorAll('.sub-code');
            const serialInputs = document.querySelectorAll('.sub-serial');
            lines.forEach((line, index) => {
                if(index >= codeInputs.length) return;
                let parts = line.trim().split(/[\t|]+/); 
                if(parts.length === 1) parts = line.trim().split(/\s+/);
                if(parts.length >= 1) codeInputs[index].value = parts[0].trim();
                if(parts.length >= 2) serialInputs[index].value = parts[1].trim();
            });
            document.getElementById('bulkArea').value = "";
        }

        function renderInputFields() {
            const qty = parseInt(document.getElementById('inpQty').value) || 1;
            const container = document.getElementById('dynamicInputs');
            let currentData = [];
            document.querySelectorAll('.sub-code').forEach((el, i) => {
                currentData.push({ c: el.value, s: document.querySelectorAll('.sub-serial')[i]?.value || '' });
            });
            container.innerHTML = "";
            for(let i = 0; i < qty; i++) {
                let row = document.createElement('div');
                row.className = 'code-entry-row';
                let valC = currentData[i] ? currentData[i].c : '';
                let valS = currentData[i] ? currentData[i].s : '';
                row.innerHTML = `<span>#${i+1}</span><input type="text" class="sub-code" value="${valC}" placeholder="Mã thẻ" style="flex:1"><input type="text" class="sub-serial" value="${valS}" placeholder="Seri" style="flex:1">`;
                container.appendChild(row);
            }
        }

        // --- UPDATED ID GENERATOR ---
        function generateRandomInvoiceID() {
            const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
            let randomPart = "";
            for (let i = 0; i < 5; i++) {
                randomPart += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            document.getElementById('inpInvoiceID').value = "TB" + randomPart;
            updateInvoiceHeader();
            renderQR();
        }

        function updateInvoiceHeader() {
            const name = document.getElementById('inpName').value;
            const phone = document.getElementById('inpPhone').value;
            const dateVal = document.getElementById('inpDate').value; 
            let dateStr = dateVal ? `${dateVal.split('-')[2]}/${dateVal.split('-')[1]}/${dateVal.split('-')[0]}` : "...";
            let invoiceCode = document.getElementById('inpInvoiceID').value.toUpperCase();
            if (!invoiceCode.startsWith("TB")) invoiceCode = "TB" + invoiceCode;
            document.getElementById('outDate').innerText = `Ngày: ${dateStr} - Giờ: ${document.getElementById('inpTime').value}`;
            document.getElementById('outName').innerText = name || "Khách lẻ";
            document.getElementById('outInvoiceID').innerText = "Số: " + invoiceCode;
            const phoneRow = document.getElementById('rowPhone');
            if(phone) { document.getElementById('outPhone').innerText = phone; phoneRow.style.display = "flex"; } 
            else { phoneRow.style.display = "none"; }
        }

        function removeItem(index) { cartItems.splice(index, 1); renderAll(); }
        function resetCart() { if(confirm("Tạo đơn mới?")) { cartItems = []; generateRandomInvoiceID(); document.getElementById('inpName').value = "Khách lẻ"; document.getElementById('inpPhone').value = ""; document.getElementById('inpDiscount').value = ""; document.getElementById('inpDate').valueAsDate = new Date(); renderAll(); } }
        function loadDailyRevenue() { const saved = localStorage.getItem('gameCardRevenue'); if(saved) document.getElementById('dailyTotalDisplay').innerText = formatMoney(parseInt(saved)) + " đ"; }
        function updateDailyRevenue(amount) { let cur = parseInt(localStorage.getItem('gameCardRevenue')) || 0; cur += amount; localStorage.setItem('gameCardRevenue', cur); document.getElementById('dailyTotalDisplay').innerText = formatMoney(cur) + " đ"; }
        function resetDailyRevenue() { if(confirm("Chốt ngày và Reset về 0?")) { localStorage.setItem('gameCardRevenue', 0); document.getElementById('dailyTotalDisplay').innerText = "0 đ"; } }
        
        function printProvisional() {
            document.getElementById('invoiceArea').classList.add('provisional-hidden');
            const originalTitle = document.getElementById('lblInvoiceTitle').innerText;
            document.getElementById('lblInvoiceTitle').innerText = "HÓA ĐƠN TẠM TÍNH";
            setTimeout(() => {
                window.print();
                document.getElementById('invoiceArea').classList.remove('provisional-hidden');
                document.getElementById('lblInvoiceTitle').innerText = originalTitle;
            }, 500);
        }

        function printOfficial() {
            document.getElementById('invoiceArea').classList.remove('provisional-hidden');
            document.getElementById('lblInvoiceTitle').innerText = "HÓA ĐƠN BÁN HÀNG";
            let totalBill = cartItems.reduce((s, i) => s + i.total, 0);
            let discount = parseInt(discountInput.value) || 0;
            let finalTotal = totalBill - discount;
            if(finalTotal > 0) {
                updateDailyRevenue(finalTotal);
                saveHistory();
            }
            window.print();
        }

        function exportHistoryToExcel() {
            let history = JSON.parse(localStorage.getItem('invoiceHistory') || "[]");
            if(history.length === 0) return alert("Không có dữ liệu lịch sử để xuất!");
            let csvContent = "\uFEFF";
            csvContent += "Mã HĐ,Ngày,Giờ,Khách hàng,SĐT,Thanh toán,Chiết khấu,Tổng tiền,Chi tiết sản phẩm\n";
            history.forEach(h => {
                let details = h.cart.map(i => {
                    let displayPrice = i.faceValue >= 1000 ? (i.faceValue/1000) + 'k' : i.faceValue;
                    return `${i.cardType} ${displayPrice} (x${i.qty})`;
                }).join("; ");
                let row = [
                    h.id, h.date, h.time, h.name, "'" + h.phone, 
                    h.payment, h.discount, h.total, details
                ].map(e => `"${e}"`).join(","); 
                csvContent += row + "\n";
            });
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            if (link.download !== undefined) {
                const url = URL.createObjectURL(blob);
                link.setAttribute("href", url);
                link.setAttribute("download", "LichSu_DonHang_TheBao.csv");
                link.style.visibility = 'hidden';
                document.body.appendChild(link);
                link.click();
                document.body.removeChild(link);
            }
        }

        // --- CORE FUNCTION: SAVE / EDIT / SEARCH / DELETE ---

        function saveHistory() {
            if(cartItems.length === 0) return;
            const currentID = document.getElementById('inpInvoiceID').value;
            const historyItem = {
                id: currentID,
                date: document.getElementById('inpDate').value,
                time: document.getElementById('inpTime').value,
                name: document.getElementById('inpName').value,
                phone: document.getElementById('inpPhone').value,
                discount: document.getElementById('inpDiscount').value,
                payment: currentPaymentMethod,
                cart: JSON.parse(JSON.stringify(cartItems)), 
                total: parseInt(document.getElementById('outFinalTotal').innerText.replace(/\D/g, ''))
            };
            
            let history = JSON.parse(localStorage.getItem('invoiceHistory') || "[]");
            
            // Logic Cập nhật: Nếu ID đã tồn tại -> Ghi đè (Sửa). Nếu chưa -> Thêm mới.
            let existingIndex = history.findIndex(h => h.id === currentID);
            
            if (existingIndex !== -1) {
                history[existingIndex] = historyItem; // Cập nhật
            } else {
                history.unshift(historyItem); // Thêm mới vào đầu
            }

            // Đã xóa giới hạn 100 đơn
            localStorage.setItem('invoiceHistory', JSON.stringify(history));
            loadHistory();
        }

        function loadHistory() {
            const listUI = document.getElementById('historyListUI');
            const keyword = document.getElementById('inpSearchHistory').value.toLowerCase();
            let history = JSON.parse(localStorage.getItem('invoiceHistory') || "[]");
            
            // Lọc theo từ khóa (Mã HĐ hoặc SĐT)
            if(keyword) {
                history = history.filter(h => 
                    h.id.toLowerCase().includes(keyword) || 
                    (h.phone && h.phone.includes(keyword))
                );
            }

            if(history.length === 0) {
                listUI.innerHTML = '<p style="padding:10px; color:#999; text-align:center;">Không tìm thấy đơn hàng.</p>';
                return;
            }

            let html = '';
            history.forEach((h) => {
                html += `<div class="history-item">
                    <div style="flex:1">
                        <strong>${h.id}</strong> - ${h.name} <br>
                        <span style="color:#666">${h.date} ${h.time} | ${h.payment}</span>
                    </div>
                    <div class="text-right">
                        <strong style="color:#27ae60">${formatMoney(h.total)} đ</strong><br>
                        <div class="history-actions" style="justify-content: flex-end; margin-top:3px;">
                             <button class="btn-mini btn-edit" onclick="restoreHistory('${h.id}')">✏ Sửa</button>
                             <button class="btn-mini btn-del" onclick="deleteHistory('${h.id}')">🗑 Xóa</button>
                        </div>
                    </div>
                </div>`;
            });
            listUI.innerHTML = html;
        }

        function restoreHistory(id) {
            let history = JSON.parse(localStorage.getItem('invoiceHistory') || "[]");
            let h = history.find(item => item.id === id);
            
            if(h) {
                if(!confirm("Bạn muốn sửa đơn hàng " + id + "?\n(Dữ liệu hiện tại trên màn hình sẽ bị thay thế)")) return;
                document.getElementById('inpInvoiceID').value = h.id;
                document.getElementById('inpDate').value = h.date;
                document.getElementById('inpTime').value = h.time;
                document.getElementById('inpName').value = h.name;
                document.getElementById('inpPhone').value = h.phone;
                document.getElementById('inpDiscount').value = h.discount;
                cartItems = h.cart;
                setPayment(h.payment || 'CASH');
                renderAll();
                alert("Đã tải lại đơn hàng để sửa. \nSau khi sửa xong, nhấn 'IN CHÍNH THỨC' để cập nhật.");
            }
        }

        function deleteHistory(id) {
            if(!confirm("Bạn chắc chắn muốn xóa vĩnh viễn đơn hàng " + id + "?")) return;
            let history = JSON.parse(localStorage.getItem('invoiceHistory') || "[]");
            let newHistory = history.filter(h => h.id !== id);
            localStorage.setItem('invoiceHistory', JSON.stringify(newHistory));
            loadHistory();
        }

        function downloadInvoiceImage() {
            const invoice = document.getElementById('invoiceArea');
            const originalShadow = invoice.style.boxShadow;
            invoice.style.boxShadow = "none"; invoice.style.border = "1px solid #ccc";
            html2canvas(invoice, { scale: 2 }).then(canvas => {
                const link = document.createElement('a');
                link.download = 'HoaDon_' + document.getElementById('inpInvoiceID').value + '.png';
                link.href = canvas.toDataURL("image/png");
                link.click();
                invoice.style.boxShadow = originalShadow; invoice.style.border = "none";
            });
        }

        function shareInvoiceImage() {
            const invoice = document.getElementById('invoiceArea');
            const originalShadow = invoice.style.boxShadow;
            invoice.style.boxShadow = "none"; invoice.style.border = "1px solid #ccc";
            html2canvas(invoice, { scale: 2 }).then(canvas => {
                canvas.toBlob(blob => {
                    const file = new File([blob], "HoaDon.png", { type: "image/png" });
                    if (navigator.share && navigator.canShare && navigator.canShare({ files: [file] })) {
                        navigator.share({ files: [file], title: 'Hóa đơn', text: 'Gửi bạn hóa đơn.' })
                        .catch(err => console.log(err));
                    } else {
                        alert("Thiết bị không hỗ trợ Share. Tải ảnh về máy.");
                        const link = document.createElement('a');
                        link.download = 'HoaDon_' + document.getElementById('inpInvoiceID').value + '.png';
                        link.href = canvas.toDataURL("image/png");
                        link.click();
                    }
                });
                invoice.style.boxShadow = originalShadow; invoice.style.border = "none";
            });
        }

        function shareProvisionalImage() {
            const invoice = document.getElementById('invoiceArea');
            const originalShadow = invoice.style.boxShadow;
            const originalTitle = document.getElementById('lblInvoiceTitle').innerText;
            invoice.style.boxShadow = "none"; invoice.style.border = "1px solid #ccc";
            invoice.classList.add('provisional-hidden');
            document.getElementById('lblInvoiceTitle').innerText = "HÓA ĐƠN TẠM TÍNH";
            html2canvas(invoice, { scale: 2 }).then(canvas => {
                canvas.toBlob(blob => {
                    const file = new File([blob], "HoaDon_TamTinh.png", { type: "image/png" });
                    if (navigator.share && navigator.canShare && navigator.canShare({ files: [file] })) {
                        navigator.share({ files: [file], title: 'Hóa đơn tạm', text: 'Gửi bạn hóa đơn tạm tính.' })
                        .catch(err => console.log(err));
                    } else {
                        alert("Thiết bị không hỗ trợ Share. Tải ảnh về máy.");
                        const link = document.createElement('a');
                        link.download = 'HoaDon_TamTinh_' + document.getElementById('inpInvoiceID').value + '.png';
                        link.href = canvas.toDataURL("image/png");
                        link.click();
                    }
                });
                invoice.style.boxShadow = originalShadow; invoice.style.border = "none";
                invoice.classList.remove('provisional-hidden');
                document.getElementById('lblInvoiceTitle').innerText = originalTitle;
            });
        }

        function renderAll() {
            const listContainer = document.getElementById('cartListUI');
            const tbody = document.getElementById('invoiceTableBody');
            document.getElementById('itemCountBadge').innerText = cartItems.length;

            if (cartItems.length === 0) listContainer.innerHTML = '<p style="color:#999;font-style:italic;text-align:center;">Chưa có sản phẩm.</p>';
            else {
                let html = '';
                cartItems.forEach((item, index) => {
                    let displayPrice = item.faceValue >= 1000 ? (item.faceValue/1000) + 'k' : item.faceValue;
                    html += `<div class="temp-item"><div><strong>${index+1}. ${item.cardType} ${displayPrice}</strong> (Bán: ${formatMoney(item.price)}) x${item.qty}</div><button class="btn btn-danger" onclick="removeItem(${index})">X</button></div>`;
                });
                listContainer.innerHTML = html;
            }

            updateInvoiceHeader();
            let totalBill = 0;
            let invHtml = '';
            cartItems.forEach((item) => {
                totalBill += item.total;
                let faceLabel = item.faceValue >= 1000 ? (item.faceValue / 1000) + 'k' : item.faceValue;
                let fullName = `${item.cardType} ${faceLabel}`;
                invHtml += `<tr><td colspan="4" style="padding: 5px; background-color:#fcfcfc;"><span class="prod-name">${fullName}</span></td></tr>`;
                let detailsHtml = '';
                item.cardList.forEach(c => {
                    if(!c.code && !c.serial) return;
                    detailsHtml += `
                        <div class="card-detail-line">
                            ${c.code ? `Mã: <b>${formatReadableCode(c.code)}</b>` : ''} ${c.serial ? ` | Seri: ${c.serial}` : ''}
                        </div>
                        <span class="hidden-msg">(*Mã thẻ đã ẩn*)</span>
                    `;
                });
                invHtml += `<tr>
                    <td style="vertical-align: top;">${detailsHtml || '<span style="font-size:11px; color:#999">...</span>'}</td>
                    <td class="text-center" style="vertical-align: top;">${item.qty}</td>
                    <td class="text-right" style="vertical-align: top;">${formatMoney(item.price)}</td>
                    <td class="text-right" style="vertical-align: top;">${formatMoney(item.total)}</td>
                </tr>`;
            });
            tbody.innerHTML = invHtml;
            
            const discountVal = parseInt(discountInput.value) || 0;
            const finalTotal = totalBill - discountVal;
            document.getElementById('outSubTotal').innerText = formatMoney(totalBill) + " đ";
            if(discountVal > 0) {
                document.getElementById('rowDiscount').style.display = "flex";
                document.getElementById('outDiscountVal').innerText = "-" + formatMoney(discountVal) + " đ";
            } else {
                document.getElementById('rowDiscount').style.display = "none";
            }
            document.getElementById('outFinalTotal').innerText = formatMoney(finalTotal) + " đ";
            renderQR();
        }

        init();
    </script>
</body>
</html>
