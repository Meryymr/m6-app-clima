<template>
  <div id="app">

    <header>
      <h1 class="text-center pt-4"> Probando API clima ☀️</h1>
    </header>
    <main class="container">
      <section class="infoClima">
        <button class="btn btn-info" @click="actualizarClima">Actualizar clima
          🌥️</button>
        <p class="py-3">Última consulta: {{ fechaConsulta }}</p>
        <div class="my-3 overflow-x-auto d-flex" v-if="datosClima.length">
          <table class="table table-warning">
            <thead>
              <tr>
                <th scope="col">Código 🔠</th>
                <th scope="col">Estación 🏙️</th>
                <th scope="col">Hora Actualización 🕒</th>
                <th scope="col">Temperatura 🌡️</th>
                <th scope="col">Humedad💧</th>
                <th scope="col">Estado ☀️</th>
                <th scope="col">Ícono 🏖️</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(clima, index) in datosClima" :key="index">
                <td>{{ clima.Codigo }}</td>
                <td>{{ clima.Estacion }}</td>
                <td>{{ clima.HoraUpdate }}</td>
                <td>{{ clima.Temp }}</td>
                <td>{{ clima.Humedad }}</td>
                <td>{{ clima.Estado }}</td>
                <td class="fs-2">{{ iconoClima(clima.Icono) }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <div v-else>
          <h2 class="text-center py-1">No hay información del clima</h2>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
moment.locale("es");

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      datosClima: [],
      fechaConsulta: ""
    }
  },
  methods: {
    actualizarClima: async function () {
      try {
        let response = await axios.get("https://api.gael.cloud/general/public/clima");
        if (response.status == 200) {
          this.datosClima = response.data;
          this.fechaConsulta = moment().format('LLLL');
        } else {
          throw new Error("Problemas al obtener información del clima");
        }
      } catch (error) {
        console.log(error);
        alert("Ha ocurrido un error al actualizar el clima.");
      }
    },
    iconoClima: function (nombreIcono) {
      let iconosClima = {
        "despejado.png": "🌤️",
        "parcial.png": "☁️",
        "cubierto.png": "⛅",
        "pocasnubes.png": "⛅",
        "niebla.png": "🌫️",
        "llovizna.png": "🌧️",
        "parcialalta.png": "🌥️",
        "cubiertohumo.png": "🌫️"
      }
      return iconosClima[nombreIcono];
    },
  }
}
</script>

<style>
* {
  font-family: "Abel", sans-serif;
}
</style>
