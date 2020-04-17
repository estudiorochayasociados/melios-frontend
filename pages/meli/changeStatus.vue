<template>
  <div class="container">
    <h2>TOTAL DE PRODUCTOS A AGREGAR: {{ products.length }}</h2>
    <button
      @click="changeStatus('active')"
      class="btn btn-success btn-block"
    >Modificar productos a Mercadolibre</button>
    <br />
    <br />
    <hr />
    Resultado ({{ total }})
    <br />
    <br />
    <div class="row">
      <div class="col-md-12">
        <table class="table table-bordered table-hover">
          <thead>
            <th>Estado</th>
            <th>Producto</th>
            <th>Mensaje</th>
          </thead>
          <tbody>
            <tr v-for="response_ in response">
              <td>{{ response_ }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      products: [],
      response: [],
      total: 0
    };
  },
  async mounted() {
    const products = await axios.get(
      process.env.apiUrl + "/product",
      this.$cookies.get("header-token")
    );
    this.products = products.data;
    console.log(this.products);
  },
  methods: {
    changeStatus: async function(status) {
      await this.products.forEach(async product => {
        await product.mercadolibre.forEach(async meli => {
          const meliPostItem = await axios.post(
            process.env.apiUrl + "/mercadolibre/change-status/" + meli.code,
            {
              status: status
            },
            this.$cookies.get("header-token")
          );
          console.log(meliPostItem);
          if (meliPostItem.data.status === 200) {
            this.total += 1;
          }
          await this.response.push(meliPostItem.data);
        });
      });
    }
  }
};
</script> 