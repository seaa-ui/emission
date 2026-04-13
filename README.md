<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vehicle-Based Carbon Emissions Estimation System</title>
  <style>
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: Arial, Helvetica, sans-serif;
    }

    body {
      min-height: 100vh;
      background: linear-gradient(135deg, #efe5d8, #d9c4ad);
      padding: 40px 18px;
      color: #2f2017;
    }

    .wrapper {
      width: 100%;
      max-width: 1180px;
      margin: 0 auto;
      background: rgba(255, 250, 245, 0.92);
      border-radius: 32px;
      padding: 46px 36px 30px;
      box-shadow: 0 25px 60px rgba(60, 40, 25, 0.15);
      border: 1px solid rgba(120, 90, 60, 0.06);
      backdrop-filter: blur(8px);
    }

    .title {
      text-align: center;
      font-size: 3rem;
      font-weight: 800;
      color: #2b1d14;
      margin-bottom: 10px;
      letter-spacing: 0.2px;
    }

    .subtitle {
      text-align: center;
      font-size: 1rem;
      color: #7a6250;
      margin-bottom: 30px;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 16px;
    }

    .form-group {
      width: 100%;
    }

    label {
      display: block;
      margin-bottom: 9px;
      font-size: 0.95rem;
      font-weight: 700;
      color: #6a5241;
    }

    select,
    input {
      width: 100%;
      height: 66px;
      padding: 0 18px;
      border: 1.5px solid #c9b39e;
      border-radius: 16px;
      background: #ffffff;
      color: #322319;
      font-size: 1.04rem;
      outline: none;
      transition: all 0.25s ease;
      box-shadow: 0 4px 12px rgba(80, 55, 35, 0.03);
    }

    select:focus,
    input:focus {
      border-color: #c47a3a;
      box-shadow: 0 0 0 5px rgba(196, 122, 58, 0.15);
    }

    input::placeholder {
      color: #8b7869;
      opacity: 1;
    }

    select.placeholder {
      color: #8b7869;
    }

    select.has-value {
      color: #322319;
    }

    select:disabled {
      background: #f6f2ee;
      color: #a39285;
      cursor: not-allowed;
    }

    button {
      width: 100%;
      height: 62px;
      border: none;
      border-radius: 16px;
      background: linear-gradient(135deg, #c47a3a, #a8642f);
      color: white;
      font-size: 1.05rem;
      font-weight: 700;
      cursor: pointer;
      margin-top: 8px;
      box-shadow: 0 10px 25px rgba(168, 100, 47, 0.35);
      transition: all 0.22s ease;
    }

    button:hover {
      background: linear-gradient(135deg, #b36d35, #915627);
      transform: translateY(-2px);
    }

    .error {
      display: none;
      margin-top: 16px;
      padding: 14px 16px;
      border-radius: 14px;
      background: #ffe4df;
      color: #922f22;
      font-weight: 700;
      font-size: 0.96rem;
    }

    .result-box {
      display: none;
      margin-top: 24px;
      background: rgba(255, 255, 255, 0.68);
      border-radius: 24px;
      padding: 22px 20px 16px;
      box-shadow: 0 10px 30px rgba(80, 55, 35, 0.1);
      border: 1px solid rgba(130, 100, 70, 0.05);
    }

    .result-title {
      font-size: 2rem;
      font-weight: 900;
      color: #332319;
      margin-bottom: 18px;
      line-height: 1.35;
    }

    .cards {
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 14px;
      margin-bottom: 14px;
    }

    .card {
      border-radius: 18px;
      padding: 20px 16px;
      text-align: center;
      min-height: 114px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      box-shadow: 0 8px 20px rgba(0, 0, 0, 0.08);
      transition: 0.2s ease;
    }

    .card:hover {
      transform: translateY(-3px);
    }

    .card h3 {
      font-size: 1.95rem;
      color: #4a3121;
      margin-bottom: 8px;
      font-weight: 800;
    }

    .card p {
      font-size: 1.16rem;
      font-weight: 700;
      color: #322319;
    }

    .co2 {
      background: #f0d6c6;
    }

    .ch4 {
      background: #f4e4b8;
    }

    .n2o {
      background: #f0cfc6;
    }

    .details {
      background: #f2e9e2;
      border-radius: 18px;
      padding: 16px 18px;
      font-size: 1.05rem;
      line-height: 1.9;
      color: #3b2a1e;
      box-shadow: inset 0 0 0 1px rgba(120, 90, 60, 0.04);
    }

    @media (max-width: 900px) {
      .wrapper {
        padding: 30px 18px 24px;
      }

      .title {
        font-size: 2.2rem;
      }

      .subtitle {
        font-size: 0.95rem;
      }

      .result-title {
        font-size: 1.3rem;
      }

      .cards {
        grid-template-columns: 1fr;
      }

      .card h3 {
        font-size: 1.7rem;
      }

      .card p,
      .details {
        font-size: 1rem;
      }

      select,
      input,
      button {
        height: 58px;
      }
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <h1 class="title">Vehicle-Based Carbon Emissions Estimation System</h1>
    <p class="subtitle">Estimate daily, monthly, yearly, and per-person transport emissions</p>

    <div class="form-grid">
      <div class="form-group">
        <label for="vehicle">Vehicle</label>
        <select id="vehicle" class="placeholder">
          <option value="" selected disabled hidden>Select Vehicle</option>
        </select>
      </div>

      <div class="form-group">
        <label for="subtype">Vehicle Type / Engine Size</label>
        <select id="subtype" class="placeholder" disabled>
          <option value="" selected disabled hidden>Select Type / Engine Size</option>
        </select>
      </div>

      <div class="form-group">
        <label for="fuel">Fuel Type</label>
        <select id="fuel" class="placeholder" disabled>
          <option value="" selected disabled hidden>Select Fuel Type</option>
        </select>
      </div>

      <div class="form-group">
        <label for="distance">Distance per day (km)</label>
        <input type="number" id="distance" placeholder="Distance per day (km)" min="1" />
      </div>

      <div class="form-group">
        <label for="persons"># of Person on Vehicle</label>
        <input type="number" id="persons" placeholder="# of Person on Vehicle (default = 1)" min="1" value="1" />
      </div>

      <button onclick="calculateEmission()">Calculate</button>
    </div>

    <div class="error" id="errorBox"></div>

    <div class="result-box" id="resultBox">
      <div class="result-title" id="resultTitle"></div>

      <div class="cards">
        <div class="card co2">
          <h3>CO₂</h3>
          <p id="co2Value">0</p>
        </div>
        <div class="card ch4">
          <h3>CH₄</h3>
          <p id="ch4Value">0</p>
        </div>
        <div class="card n2o">
          <h3>N₂O</h3>
          <p id="n2oValue">0</p>
        </div>
      </div>

      <div class="details" id="detailsText"></div>
    </div>
  </div>

  <script>

/* ================= DATA ================= */

const motorbike = {
  small: { CO2: 0.081, CH4: 0.0624, N2O: 0.0019 },
  medium: { CO2: 0.098, CH4: 0.0816, N2O: 0.0020 },
  large: { CO2: 0.131, CH4: 0.0452, N2O: 0.0020 },
  fuel: ["petrol", "diesel"]
};

const carData = {
  small: {
    petrol: { CO2: 0.140, CH4: 0.0128, N2O: 0.0012 },
    diesel: { CO2: 0.138, CH4: 0.00017, N2O: 0.0063 },
    hybrid: { CO2: 0.100, CH4: 0.0084, N2O: 0.0029 }
  },
  medium: {
    petrol: { CO2: 0.178, CH4: 0.0128, N2O: 0.0012 },
    diesel: { CO2: 0.165, CH4: 0.00017, N2O: 0.0063 },
    hybrid: { CO2: 0.108, CH4: 0.0060, N2O: 0.0039 },
    cng: { CO2: 0.154, CH4: 0.0632, N2O: 0.0014 },
    lpg: { CO2: 0.176, CH4: 0.0020, N2O: 0.0014 }
  },
  large: {
    petrol: { CO2: 0.272, CH4: 0.0128, N2O: 0.0012 },
    diesel: { CO2: 0.207, CH4: 0.00017, N2O: 0.0063 },
    hybrid: { CO2: 0.151, CH4: 0.0036, N2O: 0.0050 },
    cng: { CO2: 0.236, CH4: 0.0632, N2O: 0.0014 },
    lpg: { CO2: 0.269, CH4: 0.0020, N2O: 0.0014 }
  },
  average: {
    petrol: { CO2: 0.163, CH4: 0.0128, N2O: 0.0012 },
    diesel: { CO2: 0.168, CH4: 0.00017, N2O: 0.0063 },
    hybrid: { CO2: 0.118, CH4: 0.0068, N2O: 0.0037 },
    cng: { CO2: 0.173, CH4: 0.0632, N2O: 0.0014 },
    lpg: { CO2: 0.197, CH4: 0.0020, N2O: 0.0016 }
  }
};

const vanData = {
  "Class I": { petrol: { CO2: 0.181, CH4: 0.0096, N2O: 0.0016 } },
  "Class II": { petrol: { CO2: 0.195, CH4: 0.0096, N2O: 0.00164 } },
  "Class III": { petrol: { CO2: 0.314, CH4: 0.0096, N2O: 0.00164 } },
  "Average": {
    petrol: { CO2: 0.201, CH4: 0.0096, N2O: 0.0016 },
    diesel: { CO2: 0.230, CH4: 0.0100, N2O: 0.0062 },
    cng: { CO2: 0.230, CH4: 0.0472, N2O: 0.0019 },
    lpg: { CO2: 0.255, CH4: 0.0016, N2O: 0.0019 }
  }
};

const hgvData = {
  "Rigid 3.5-7.5t": {
    "0": { diesel: { CO2: 0.447, CH4: 0.004, N2O: 0.0201 } },
    "50": { diesel: { CO2: 0.486, CH4: 0.004, N2O: 0.0201 } },
    "100": { diesel: { CO2: 0.524, CH4: 0.004, N2O: 0.0201 } },
    "avg": { diesel: { CO2: 0.480, CH4: 0.004, N2O: 0.0201 } }
  },
  "Rigid 7.5-17t": {
    "0": { diesel: { CO2: 0.380, CH4: 0.0040, N2O: 0.018 } },
    "50": { diesel: { CO2: 0.460, CH4: 0.0045, N2O: 0.021 } },
    "100": { diesel: { CO2: 0.534, CH4: 0.0048, N2O: 0.0245 } },
    "avg": { diesel: { CO2: 0.460, CH4: 0.0045, N2O: 0.021 } }
  },
  "Rigid >17t": {
    "0": { diesel: { CO2: 0.550, CH4: 0.0060, N2O: 0.030 } },
    "50": { diesel: { CO2: 0.650, CH4: 0.0070, N2O: 0.035 } },
    "100": { diesel: { CO2: 0.736, CH4: 0.0080, N2O: 0.040 } },
    "avg": { diesel: { CO2: 0.650, CH4: 0.0070, N2O: 0.035 } }
  },
  "Articulated 3.5-33t": {
    "0": { diesel: { CO2: 0.900, CH4: 0.0007, N2O: 0.015 } },
    "50": { diesel: { CO2: 1.050, CH4: 0.0008, N2O: 0.017 } },
    "100": { diesel: { CO2: 1.200, CH4: 0.0008, N2O: 0.018 } },
    "avg": { diesel: { CO2: 1.050, CH4: 0.0008, N2O: 0.017 } }
  },
  "Articulated >33t": {
    "0": { diesel: { CO2: 1.100, CH4: 0.0009, N2O: 0.018 } },
    "50": { diesel: { CO2: 1.300, CH4: 0.0010, N2O: 0.019 } },
    "100": { diesel: { CO2: 1.500, CH4: 0.0010, N2O: 0.020 } },
    "avg": { diesel: { CO2: 1.300, CH4: 0.0010, N2O: 0.019 } }
  }
};

/* ================= HISTORY ================= */
let resultHistory = [];

/* ================= ELEMENTS ================= */
const vehicleType = document.getElementById("vehicleType");
const size = document.getElementById("size");
const load = document.getElementById("load");
const fuel = document.getElementById("fuel");
const passengers = document.getElementById("passengers");
const distance = document.getElementById("distance");

/* ================= FUNCTIONS ================= */

function updateSize() {
  const type = vehicleType.value;

  size.innerHTML = "";
  load.style.display = "none";

  if (type === "motorbike") {
    size.innerHTML = `
      <option value="small">Small ≤125cc</option>
      <option value="medium">Medium 125–500cc</option>
      <option value="large">Large >500cc</option>`;
  }

  if (type === "car") {
    size.innerHTML = `
      <option value="small">Small Car, <1.4 litre</option>
      <option value="medium">Medium Car, 1.4 - 2.0 litres</option> 
      <option value="large">Large Car, >2.0 litres</option> 
      <option value="average">Average Car</option>`;
  }

  if (type === "van") {
    size.innerHTML = `
      <option value="Class I">Class I, ≤1.305 tonnes</option>
      <option value="Class II">Class II, >1.305 to ≤1.74 tonnes</option>
      <option value="Class III">Class III, >1.74 to ≤3.5 tonnes</option>
      <option value="Average">Average (up to 3.5 tonnes)</option>`;
  }

  if (type === "hgv") {
    size.innerHTML = Object.keys(hgvData)
      .map(k => `<option value="${k}">${k}</option>`)
      .join("");

    load.style.display = "block";
    load.innerHTML = `
      <option value="0">0% weight laden</option>
      <option value="50">50% weight laden</option>
      <option value="100">100% weight laden</option>
      <option value="avg">avg weight laden</option>`;
  }

  updateFuel();
}

function updateFuel() {
  const type = vehicleType.value;
  const s = size.value;

  fuel.innerHTML = "";

  if (type === "motorbike") {
    motorbike.fuel.forEach(f => fuel.innerHTML += `<option>${f}</option>`);
  }

  if (type === "car") {
    Object.keys(carData[s]).forEach(f => fuel.innerHTML += `<option>${f}</option>`);
  }

  if (type === "van") {
    Object.keys(vanData[s]).forEach(f => fuel.innerHTML += `<option>${f}</option>`);
  }

  if (type === "hgv") {
    fuel.innerHTML = `<option>diesel</option>`;
  }
}

function calculate() {
  const type = vehicleType.value;
  const s = size.value;
  const sText = size.options[size.selectedIndex].text;

  const f = fuel.value;
  const fText = fuel.options[fuel.selectedIndex].text;

  const d = parseFloat(distance.value);
  const p = parseFloat(passengers.value || 1);
  const l = load.value || "avg";

  if (!type || !s || !f || isNaN(d)) {
    alert("Please complete inputs");
    return;
  }

  let base =
    type === "hgv"
      ? hgvData[s][l][f]
      : type === "motorbike"
      ? motorbike[s]
      : type === "car"
      ? carData[s][f]
      : vanData[s][f];

  if (!base) {
    alert("Invalid selection");
    return;
  }

  const co2 = base.CO2 * d;
  const ch4 = base.CH4 * d;
  const n2o = base.N2O * d;

  const newResult = `
    <div class="card">
      <b>${type.toUpperCase()} — ${sText} — ${fText} — ${d} km</b>

      <div class="grid">
        <div class="box co2">CO₂<br>${co2.toFixed(3)} kg</div>
        <div class="box ch4">CH₄<br>${ch4.toFixed(4)} g</div>
        <div class="box n2o">N₂O<br>${n2o.toFixed(4)} g</div>
      </div>

      <div class="summary">
        CO₂ Daily: ${co2.toFixed(3)} kg | Monthly: ${(co2*30).toFixed(3)} | Yearly: ${(co2*365).toFixed(3)} | Per Person: ${(co2/p).toFixed(3)}<br>
        CH₄ Daily: ${ch4.toFixed(4)} g | Monthly: ${(ch4*30).toFixed(4)} | Yearly: ${(ch4*365).toFixed(4)} | Per Person: ${(ch4/p).toFixed(4)}<br>
        N₂O Daily: ${n2o.toFixed(4)} g | Monthly: ${(n2o*30).toFixed(4)} | Yearly: ${(n2o*365).toFixed(4)} | Per Person: ${(n2o/p).toFixed(4)}
      </div>
    </div>
  `;

  resultHistory.unshift(newResult);

  if (resultHistory.length > 3) {
    resultHistory.pop();
  }

  document.getElementById("result").innerHTML = resultHistory.join("");
}
  
</script>

</body>
</html>
