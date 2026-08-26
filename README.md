<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="theme-color" content="#090a0c">
  <meta name="description" content="RIDER - rutas para motociclistas">
  <title>RIDER</title>

  <style>
    * {
      box-sizing: border-box;
    }

    body {
      margin: 0;
      background: #090a0c;
      color: #fff;
      font-family: Arial, sans-serif;
    }

    header {
      height: 65px;
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 20px;
      background: #090a0c;
    }

    .logo {
      font-size: 30px;
      font-weight: 900;
      letter-spacing: 5px;
    }

    #gps {
      border: 0;
      background: #191b20;
      color: #fff;
      width: 45px;
      height: 45px;
      border-radius: 50%;
      font-size: 20px;
    }

    #map {
      height: 42vh;
      min-height: 300px;
      background:
        linear-gradient(135deg, #17201c, #11161a);
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 30px;
    }

    .map-content {
      color: #a0a6ad;
    }

    .bike {
      font-size: 65px;
      margin-bottom: 15px;
    }

    .panel {
      padding: 22px;
    }

    h1 {
      font-size: 25px;
      margin: 0 0 6px;
    }

    .subtitle {
      color: #858b94;
      line-height: 21px;
      margin-bottom: 25px;
    }

    label {
      display: block;
      color: #a0a6ae;
      font-size: 12px;
      font-weight: bold;
      margin-top: 17px;
      margin-bottom: 7px;
    }

    input[type="range"] {
      width: 100%;
      accent-color: #fff;
    }

    select {
      width: 100%;
      background: #181a1e;
      color: #fff;
      border: 0;
      border-radius: 12px;
      padding: 15px;
      font-size: 16px;
    }

    .switch {
      background: #15171b;
      padding: 15px;
      border-radius: 14px;
      margin-top: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    .route {
      width: 100%;
      border: 0;
      border-radius: 14px;
      padding: 18px;
      margin-top: 20px;
      background: #fff;
      color: #090a0c;
      font-size: 15px;
      font-weight: 900;
    }

    #result {
      margin-top: 18px;
      padding: 18px;
      border-radius: 18px;
      background: #171a1f;
      display: none;
      line-height: 1.6;
    }

    .gps-ok {
      color: #fff;
      font-weight: bold;
    }
  </style>
</head>

<body>

<header>
  <div class="logo">RIDER</div>

  <button id="gps" aria-label="Mi ubicación">
    📍
  </button>
</header>

<section id="map">

  <div class="map-content">

    <div class="bike">🏍️</div>

    <strong>RIDER</strong>

    <p>
      Pulsa 📍 para obtener<br>
      tu ubicación GPS
    </p>

  </div>

</section>

<section class="panel">

  <h1>¿Dónde quieres rodar?</h1>

  <p class="subtitle">
    Diseña tu ruta según el tipo de conducción que buscas.
  </p>

  <label>🔥 CURVAS</label>

  <input
    id="curves"
    type="range"
    min="0"
    max="10"
    value="8"
  >

  <label>⛰️ MONTAÑA</label>

  <input
    id="mountain"
    type="range"
    min="0"
    max="10"
    value="7"
  >

  <label>🌲 CARRETERAS SECUNDARIAS</label>

  <input
    id="secondary"
    type="range"
    min="0"
    max="10"
    value="9"
  >

  <label>🌄 PAISAJE</label>

  <input
    id="scenery"
    type="range"
    min="0"
    max="10"
    value="7"
  >

  <label>📏 DISTANCIA</label>

  <select id="distance">

    <option>100 km</option>
    <option>200 km</option>
    <option selected>250 km</option>
    <option>350 km</option>
    <option>500 km</option>

  </select>

  <div class="switch">

    <span>🛣️ Evitar autopistas</span>

    <input
      id="highways"
      type="checkbox"
      checked
    >

  </div>

  <button
    class="route"
    onclick="createRoute()"
  >
    🏍️ CREAR RUTA
  </button>

  <div id="result"></div>

</section>

<script>

let position = null;

const gpsButton =
  document.getElementById("gps");

const map =
  document.getElementById("map");

gpsButton.addEventListener(
  "click",
  getLocation
);

function getLocation() {

  if (!navigator.geolocation) {

    alert(
      "Tu navegador no permite obtener la ubicación GPS."
    );

    return;
  }

  map.innerHTML = `
    <div class="map-content">
      <div class="bike">📡</div>
      <strong>BUSCANDO TU UBICACIÓN...</strong>
      <p>Espera unos segundos</p>
    </div>
  `;

  navigator.geolocation.getCurrentPosition(

    function(pos) {

      position = pos.coords;

      map.innerHTML = `
        <div class="map-content">

          <div class="bike">🏍️</div>

          <strong class="gps-ok">
            📍 TU UBICACIÓN
          </strong>

          <p>
            Latitud:
            ${position.latitude.toFixed(5)}
            <br>
            Longitud:
            ${position.longitude.toFixed(5)}
          </p>

        </div>
      `;

      gpsButton.textContent = "✅";

    },

    function(error) {

      map.innerHTML = `
        <div class="map-content">

          <div class="bike">⚠️</div>

          <strong>
            NO SE HA PODIDO OBTENER EL GPS
          </strong>

          <p>
            Comprueba que Chrome tenga permiso
            para acceder a tu ubicación.
          </p>

        </div>
      `;

    },

    {
      enableHighAccuracy: true,
      timeout: 15000,
      maximumAge: 0
    }

  );

}

function createRoute() {

  const distance =
    document.getElementById("distance").value;

  const curves =
    document.getElementById("curves").value;

  const mountain =
    document.getElementById("mountain").value;

  const secondary =
    document.getElementById("secondary").value;

  const scenery =
    document.getElementById("scenery").value;

  const highways =
    document.getElementById("highways").checked;

  const result =
    document.getElementById("result");

  result.style.display = "block";

  result.innerHTML = `

    <h2>🏍️ Ruta RIDER</h2>

    <p>
      <strong>Distancia:</strong>
      ${distance}
    </p>

    <p>
      🔥 Curvas: ${curves}/10<br>
      ⛰️ Montaña: ${mountain}/10<br>
      🌲 Secundarias: ${secondary}/10<br>
      🌄 Paisaje: ${scenery}/10
    </p>

    <p>
      🛣️ Autopistas:
      ${highways ? "evitadas" : "permitidas"}
    </p>

    <p>
      ${
        position
        ? "📍 Salida desde tu ubicación GPS."
        : "📍 Primero activa el GPS."
      }
    </p>

    <p>
      <strong>
        Configuración de ruta preparada.
      </strong>
    </p>

  `;

}

</script>

</body>
</html>
