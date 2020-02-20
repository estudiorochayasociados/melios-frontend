<template>
  <div class="container">
    <MessageComponent v-bind:text="messageText" v-bind:status="messageStatus" />
    <h4 class="mt-20">Total de Productos: {{ products.length }}</h4>
    <table class="table is-fullwidth mt-20">
      <thead>
        <th>Producto</th>
      </thead>
      <tbody>
        <tr v-for="product in products">
          <td>{{ product.title }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
import MessageComponent from "../components/MessageComponent";
export default {
  data() {
    return {
      products: [],
      messageText: "",
      messageStatus: ""
    };
  },
  components: {
    MessageComponent
  },
  async mounted() {
    const products = await axios.get(
      process.env.apiUrl + "/product",
      this.$cookies.get("header-token")
    );
    this.products = products.data;
    this.products.forEach(element => {
      var gold_pro = element.mercadolibre.findIndex(x => x.type === "gold_pro");
      var gold_special = element.mercadolibre.findIndex(
        x => x.type === "gold_special"
      );
      const meli = [];
      if (element.mercadolibre[gold_pro] != undefined) {
        meli.push(element.mercadolibre[gold_pro]);
      }
      if (element.mercadolibre[gold_special] != undefined) {
        meli.push(element.mercadolibre[gold_special]);
      }
      console.log(meli);
    });
  },
  methods: {
    viewDiv: function(id) {
      var element = document.getElementById(id);
      var element = document.getElementById(id);
      element.classList.add("d-block");
    }
  }
};
</script> 