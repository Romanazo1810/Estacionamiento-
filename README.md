<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Cobro de Estacionamiento</title>
</head>
<body>

  <h1>Cobro de Estacionamiento</h1>

  <!-- Sección para ingresar la patente -->
  <h2>Registrar entrada:</h2>
  <form id="entradaForm">
    <label for="patenteEntrada">Patente del auto:</label>
    <input type="text" id="patenteEntrada" required>
    <button type="button" onclick="registrarEntrada()">Registrar Entrada</button>
    <p id="horaEntrada"></p>
  </form>

  <!-- Sección para seleccionar la patente al salir -->
  <h2>Cobro al salir:</h2>
  <form id="salidaForm">
    <label for="patenteSalida">Seleccionar patente:</label>
    <select id="patenteSalida" required>
      <!-- Aquí se insertarán las patentes registradas dinámicamente -->
    </select>
    <button type="button" onclick="calcularCobro()">Calcular Cobro</button>
    <p id="cobroResultado"></p>
    <label for="descuento">Aplicar Descuento:</label>
    <select id="descuento">
      <option value="0">Sin Descuento</option>
      <option value="1500">Descuento Compra (1500 pesos)</option>
      <option value="750">Descuento Pagos (750 pesos)</option>
    </select>
    <button type="button" onclick="aplicarDescuento()">Aplicar Descuento</button>
  </form>

  <!-- Pestaña para ver todos los autos ingresados -->
  <h2>Autos Ingresados:</h2>
  <button type="button" onclick="mostrarAutosIngresados()">Ver Autos Ingresados</button>
  <ul id="listaAutos"></ul>

  <script>
    // Objeto para almacenar las patentes, los tiempos de entrada y los descuentos
    const patentesEntrada = {};
    const descuentos = {};

    function registrarEntrada() {
      const patente = document.getElementById('patenteEntrada').value;
      const tiempoEntrada = new Date();

      patentesEntrada[patente] = {
        tiempo: tiempoEntrada,
        descuento: 0
      };

      // Mostrar la hora exacta de entrada
      document.getElementById('horaEntrada').innerText = `Hora de entrada: ${tiempoEntrada.toLocaleTimeString()}`;

      // Agregar la patente al select de salidas
      const selectSalida = document.getElementById('patenteSalida');
      const option = document.createElement('option');
      option.value = patente;
      option.text = patente;
      selectSalida.add(option);

      // Limpiar el campo de entrada
      document.getElementById('patenteEntrada').value = '';
    }

    function calcularCobro() {
      const patenteSeleccionada = document.getElementById('patenteSalida').value;
      const descuentoAplicado = descuentos[patenteSeleccionada] || 0;

      if (patentesEntrada.hasOwnProperty(patenteSeleccionada)) {
        const tiempoEntrada = patentesEntrada[patenteSeleccionada].tiempo;
        const tiempoSalida = new Date();
        const tiempoEstacionado = Math.floor((tiempoSalida - tiempoEntrada) / (1000 * 60)); // minutos

        let cobro = 500; // Tarifa mínima

        if (tiempoEstacionado > 15) {
          cobro += (tiempoEstacionado - 15) * 25;
        }

        // Aplicar descuento
        cobro -= descuentoAplicado;

        // Mostrar el resultado del cobro
        document.getElementById('cobroResultado').innerText = `Cobro total: $${cobro}`;

      } else {
        alert('La patente seleccionada no tiene un registro de entrada.');
      }
    }

    function aplicarDescuento() {
      const patenteSeleccionada = document.getElementById('patenteSalida').value;
      const descuentoSeleccionado = parseInt(document.getElementById('descuento').value);

      if (patentesEntrada.hasOwnProperty(patenteSeleccionada)) {
        // Aplicar descuento y mostrar mensaje
        descuentos[patenteSeleccionada] = descuentoSeleccionado;
        alert(`Descuento aplicado: $${descuentoSeleccionado}`);
      } else {
        alert('La patente seleccionada no tiene un registro de entrada.');
      }
    }

    function mostrarAutosIngresados() {
      const listaAutos = document.getElementById('listaAutos');
      listaAutos.innerHTML = '';

      for (const patente in patentesEntrada) {
        const tiempoEntrada = patentesEntrada[patente].tiempo;
        const listItem = document.createElement('li');
        listItem.innerText = `${patente} - Hora de entrada: ${tiempoEntrada.toLocaleTimeString()}`;
        listaAutos.appendChild(listItem);
      }
    }
  </script>

</body>
</html>
