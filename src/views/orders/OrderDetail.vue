<template>
  <div class="p-6">
    <h1 class="title">{{ model.cliente.name }}</h1>
    <h2 class="subtitle">Order de compra</h2>

    <Loading v-if="isLoading" />
    <template v-else>
      <table class="table is-fullwidth is-striped">
        <thead>
          <tr>
            <th>Producto</th>
            <th class="has-text-right" style="width: 100px">Cantidad</th>
            <th class="has-text-right" style="width: 150px">P.U</th>
            <th class="has-text-right" style="width: 150px">IVA</th>
            <th class="has-text-right" style="width: 150px">Sub Total</th>
            <th class="has-text-right" style="width: 150px">Total</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in model.items" :key="item.orderDetailId">
            <td>{{ item.product.name }}</td>
            <td>{{ item.quantity }}</td>
            <td class="has-text-right">US$ {{ item.unitPrice.toFixed(2) }}</td>
            <td class="has-text-right">US$ {{ item.iva.toFixed(2) }}</td>
            <td class="has-text-right">US$ {{ item.subTotal.toFixed(2) }}</td>
            <td class="has-text-right">US$ {{ item.total.toFixed(2) }}</td>
          </tr>
        </tbody>
        <tfoot class="has-text-weight-bold">
          <tr>
            <td colspan="5" class="has-text-right">IVA</td>
            <td class="has-text-right">US$ {{ model.iva.toFixed(2) }}</td>
          </tr>
          <tr>
            <td colspan="5" class="has-text-right">Sub Total</td>
            <td class="has-text-right">US$ {{ model.subTotal.toFixed(2) }}</td>
          </tr>
          <tr>
            <td colspan="5" class="has-text-right">Total</td>
            <td class="has-text-right">US$ {{ model.total.toFixed(2) }}</td>
          </tr>
        </tfoot>
      </table>
    </template>
  </div>
</template>

<script>
import Loading from "../../components/Loading.vue";

export default {
  name: "OrderDetail",
  components: {
    Loading,
  },
  mounted() {
    console.log("aqui");
    this.get();
  },
  data() {
    return {
      isLoading: false,
      model: {
        cliente: {
          name: ''
        },
        items: [],
        iva: 0,
        subTotal: 0,
        total: 0
      },
    };
  },
  methods: {
    get() {
      this.isLoading = true;

      this.$proxies.orderProxy
        .get(this.$route.params.id)
        .then((x) => {
          this.model = x.data;
        })
        .catch((exception) => {
          console.log(exception);
        })
        .finally(() => {
          this.isLoading = false;
        });
    },
  },
};
</script>