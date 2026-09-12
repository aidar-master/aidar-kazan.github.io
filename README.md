<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Aidar | Калькуляторы под заказ</title>
  <style>
    :root { --primary: #007bff; --danger: #dc3545; --bg: #f8f9fa; --border: #dee2e6; }
    body { font-family: Arial, sans-serif; max-width: 560px; margin: 0 auto; padding: 12px; background: #fff; color: #333; }
    h1 { text-align: center; margin-bottom: 8px; }
    .subtitle { text-align: center; color: #666; margin-bottom: 24px; }

    /* Табы */
    .tabs { display: flex; gap: 4px; margin-bottom: 16px; }
    .tab-btn { flex: 1; padding: 10px; border: 1px solid var(--border); background: #fff; cursor: pointer; border-radius: 6px; text-align: center; }
    .tab-btn.active { background: var(--primary); color: #fff; border-color: var(--primary); }

    /* Контейнеры калькуляторов */
    .calc-container { display: none; border: 1px solid var(--border); border-radius: 8px; padding: 16px; background: var(--bg); }
    .calc-container.active { display: block; }

    .section { margin-bottom: 16px; }
    .field { margin-bottom: 10px; }
    label { display: block; margin-bottom: 4px; font-weight: 600; font-size: 0.95em; }
    input { width: 100%; padding: 8px; box-sizing: border-box; border: 1px solid var(--border); border-radius: 4px; font-size: 15px; }
    button.calc-btn { width: 100%; padding: 14px; background: var(--primary); color: #fff; border: none; border-radius: 6px; font-size: 16px; cursor: pointer; margin-top: 8px; }
    button.calc-btn:hover { background: #0056b3; }

    #result { margin-top: 18px; padding: 14px; background: #fff; border: 1px dashed var(--border); border-radius: 6px; display: none; }
    .row { display: flex; justify-content: space-between; margin-bottom: 6px; }
    .total { font-weight: bold; font-size: 1.15em; color: #28a745; }

    /* Водяной знак */
    .watermark { position: fixed; top: 0; left: 0; width: 100%; height: 100%; display: flex; justify-content: center; align-items: center; z-index: 9998; pointer-events: none; color: rgba(220, 53, 69, 0.4); font-size: 3rem; font-weight: bold; text-transform: uppercase; letter-spacing: 2px; transform: rotate(-25deg); user-select: none; }

    /* Форма заявки */
    .form-block { margin-top: 24px; padding: 16px; border: 1px solid var(--border); border-radius: 8px; background: #fff; }
    .form-field { margin-bottom: 10px; }
    button.submit-btn { width: 100%; padding: 14px; background: #28a745; color: #fff; border: none; border-radius: 6px; font-size: 16px; cursor: pointer; }
    button.submit-btn:hover { background: #1e7e37; }

    .legal { margin-top: 16px; font-size: 0.85em; color: #666; line-height: 1.4; }
    .note-demo { margin-top: 16px; padding: 12px; background: #ffebee; border: 1px dashed #d32f2f; color: #c62828; border-radius: 6px; font-size: 0.9em; }
  </style>
</head>
<body>

  <div class="watermark">ДЕМО. НЕ ДЛЯ ИСПОЛЬЗОВАНИЯ</div>

  <h1>Aidar — калькуляторы под ваш бизнес</h1>
  <p class="subtitle">ИП, Казань. Грузоперевозки + ИТ‑услуги. Делаю калькуляторы, чтобы клиент сразу видел цену и не задавал одни и те же вопросы.</p>

  <!-- Переключатель вкладок -->
  <div class="tabs">
    <button class="tab-btn active" onclick="switchTab('gruz')">🚚 Грузоперевозки</button>
    <button class="tab-btn" onclick="switchTab('it')">💻 ИТ‑услуги</button>
  </div>

  <!-- Калькулятор грузоперевозок -->
  <div id="gruz" class="calc-container active">
    <h3>Калькулятор грузоперевозок</h3>
    <div class="section">
      <h4>Ваши тарифы</h4>
      <div class="field"><label>Ставка за час по городу (₽)</label><input type="number" id="ratePerHourGruz" value="900" min="0" step="50"></div>
      <div class="field"><label>Межгород: тариф за км (₽)</label><input type="number" id="ratePerKmGruz" value="25" min="0" step="0.5"></div>
      <div class="field"><label>Грузчики: ставка за час/чел (₽)</label><input type="number" id="loaderRateGruz" value="300" min="0" step="50"></div>
      <div class="field"><label>Подъём без лифта: за этаж за предмет (₽)</label><input type="number" id="floorRateGruz" value="100" min="0" step="10"></div>
    </div>
    <div class="section">
      <h4>Параметры заявки</h4>
      <div class="field"><label>Часы работы по городу</label><input type="number" id="hoursGruz" placeholder="Например: 2" min="0" step="0.5"></div>
      <div class="field"><label>Межгород: расстояние в км (туда)</label><input type="number" id="distanceGruz" placeholder="Например: 60" min="0"></div>
      <div class="field"><label>Количество грузчиков</label><input type="number" id="loadersCountGruz" placeholder="Например: 2"></div>
      <div class="field"><label>Часы работы грузчиков</label><input type="number" id="loaderHoursGruz" placeholder="Например: 2"></div>
      <div class="field"><label>Подъём без лифта: этажей (всего)</label><input type="number" id="floorsGruz" placeholder="Например: 4"></div>
    </div>
    <button class="calc-btn" onclick="calcGruz()">Рассчитать стоимость</button>
    <div id="resultGruz">
      <div class="row"><span>Транспорт (по часам):</span><span id="transportCostGruz">0 ₽</span></div>
      <div class="row"><span>Межгород (км в обе стороны):</span><span id="intercityCostGruz">0 ₽</span></div>
      <div class="row"><span>Грузчики:</span><span id="loaderCostGruz">0 ₽</span></div>
      <div class="row"><span>Подъём без лифта:</span><span id="floorCostGruz">0 ₽</span></div>
      <hr style="margin: 10px 0; border: 0; border-top: 1px solid #ccc;">
      <div class="row total"><span>Итого:</span><span id="totalCostGruz">0 ₽</span></div>
    </div>
  </div>

  <!-- Калькулятор ИТ‑услуг -->
  <div id="it" class="calc-container">
    <h3>Калькулятор ИТ‑услуг</h3>
    <div class="section">
      <h4>Ваши тарифы</h4>
      <div class="field"><label>Ставка за час работы (₽)</label><input type="number" id="ratePerHourIt" value="1500" min="0"></div>
      <div class="field"><label>Выезд (фиксированно, ₽)</label><input type="number" id="deliveryFeeIt" value="500" min="0"></div>
      <div class="field"><label>Расходные материалы (среднее, ₽)</label><input type="number" id="materialsFeeIt" value="300" min="0"></div>
    </div>
    <div class="section">
      <h4>Параметры заявки</h4>
      <div class="field"><label>Диагностика (мин)</label><input type="number" id="diagMinIt" placeholder="Например: 30"></div>
      <div class="field"><label>Установка Windows и драйверов (мин)</label><input type="number" id="winSetupMinIt" placeholder="Например: 60"></div>
      <div class="field"><label>Дополнительные работы (мин)</label><input type="number" id="extraMinIt" placeholder="Например: 15"></div>
    </div>
    <button class="calc-btn" onclick="calcIt()">Рассчитать стоимость</button>
    <div id="resultIt">
      <div class="row"><span>Работа (по часам):</span><span id="workCostIt">0 ₽</span></div>
      <div class="row"><span>Выезд:</span><span id="deliveryCostIt">0 ₽</span></div>
      <div class="row"><span>Материалы:</span><span id="materialsCostIt">0 ₽</span></div>
      <hr style="margin: 10px 0; border: 0; border-top: 1px solid #ccc;">
      <div class="row total"><span>Итого:</span><span id="totalCostIt">0 ₽</span></div>
    </div>
  </div>

  <!-- Форма заявки -->
  <div class="form-block">
    <h3>Заказать доработку калькулятора</h3>
    <div class="form-field"><label>Ваше имя</label><input type="text" id="name" placeholder="Иван"></div>
    <div class="form-field"><label>Телефон</label><input type="tel" id="phone" placeholder="+7 (___) ___-__-__"></div>
    <button class="submit-btn" onclick="alert('Спасибо! Я свяжусь с вами в ближайшее время.')">Отправить заявку</button>
  </div>

  <div class="note-demo">
    Это демонстрационная версия. Не предназначена для самостоятельного использования.
  </div>

  <div class="legal">
    Работаю как ИП (УСН). Чек пробиваю через ККТ. Старт работ — после активации кассы (январь 2027). Принимаю заявки сейчас. Предоплата 30% после пробития чека.
  </div>

  <script>
    function switchTab(tabName) {
      document.querySelectorAll('.calc-container').forEach(el => el.classList.remove('active'));
      document.querySelectorAll('.tab-btn').forEach(el => el.classList.remove('active'));
      document.getElementById(tabName).classList.add('active');
      event.target.classList.add('active');
    }

    function calcGruz() {
      const ratePerHour = parseFloat(document.getElementById('ratePerHourGruz').value) || 0;
      const ratePerKm = parseFloat(document.getElementById('ratePerKmGruz').value) || 0;
      const loaderRate = parseFloat(document.getElementById('loaderRateGruz').value) || 0;
      const floorRate = parseFloat(document.getElementById('floorRateGruz').value) || 0;

      const hours = parseFloat(document.getElementById('hoursGruz').value) || 0;
      const distance = parseFloat(document.getElementById('distanceGruz').value) || 0;
      const loadersCount = parseInt(document.getElementById('loadersCountGruz').value) || 0;
      const loaderHours = parseFloat(document.getElementById('loaderHoursGruz').value) || 0;
      const floors = parseInt(document.getElementById('floorsGruz').value) || 0;

      const effectiveHours = Math.max(hours, hours > 0 ? 2 : 0);
      const transportCost = effectiveHours * ratePerHour;
      const intercityCost = distance * 2 * ratePerKm;
      const loaderCost = loadersCount * loaderHours * loaderRate;
      const floorCost = floors * floorRate;
      const total = transportCost + intercityCost + loaderCost + floorCost;

      document.getElementById('transportCostGruz').textContent = transportCost.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('intercityCostGruz').textContent = intercityCost.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('loaderCostGruz').textContent = loaderCost.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('floorCostGruz').textContent = floorCost.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('totalCostGruz').textContent = total.toLocaleString('ru-RU') + ' ₽';

      document.getElementById('resultGruz').style.display = 'block';
    }

    function calcIt() {
      const ratePerHour = parseFloat(document.getElementById('ratePerHourIt').value) || 0;
      const deliveryFee = parseFloat(document.getElementById('deliveryFeeIt').value) || 0;
      const materialsFee = parseFloat(document.getElementById('materialsFeeIt').value) || 0;

      const diagMin = parseFloat(document.getElementById('diagMinIt').value) || 0;
      const winSetupMin = parseFloat(document.getElementById('winSetupMinIt').value) || 0;
      const extraMin = parseFloat(document.getElementById('extraMinIt').value) || 0;

      const totalMinutes = diagMin + winSetupMin + extraMin;
      const workHours = totalMinutes / 60;
      const workCost = workHours * ratePerHour;
      const total = workCost + deliveryFee + materialsFee;

      document.getElementById('workCostIt').textContent = workCost.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('deliveryCostIt').textContent = deliveryFee.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('materialsCostIt').textContent = materialsFee.toLocaleString('ru-RU') + ' ₽';
      document.getElementById('totalCostIt').textContent = total.toLocaleString('ru-RU') + ' ₽';

      document.getElementById('resultIt').style.display = 'block';
    }
  </script>
</body>
</html>
