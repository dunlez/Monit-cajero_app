<template>
  <div id="Transaction">
    <b-container>
      <b-row>
        <b-col cols="4">Cantidad:</b-col>
        <b-col cols="8"><b-form-input style="width: 50%" type="text" placeholder="Cantidad" v-model="cantidad" /></b-col>
      </b-row>
      <b-row>
        <b-col>
          <b-button variant="dark" v-on:click="makeTransaction">Hacer transacción</b-button>
        </b-col>
      </b-row>
      <br />

      <b-row>
        <table>
          <tr>
            <th>Id de la transacción</th>
            <th>Fecha de la transacción</th>
            <th>Valor de la transacción</th>
          </tr>

          <tr v-for="t in transactions" v-bind:key="t.id_transaction">
            <td>{{ t.id_transaction }}</td>
            <td>{{ t.date }}</td>
            <td>{{ t.value }}</td>
          </tr>

        </table>
      </b-row>

      <div>
        <b-table striped hover :items="transactions"></b-table>
      </div>



    </b-container>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Transaction",
  data: function () {
    return {
      cantidad: "",
      transactions: []
    };
  },
  methods: {
    makeTransaction: function() {

      var data = {
        username: localStorage.getItem("current_username"),
        value: this.cantidad        
      }

      axios.put("http://localhost:8000/user/transaction/", data)
      .then(response => {
          alert("La transacción se realizo con exito");
          this.$router.push({name: "user_balance", params: { username: data.username } });
      })
      .catch((error) => {
        alert("ERROR Servidor");
      });
    }
  },
  created: function() {
    var current_username = localStorage.getItem("current_username");
    let self = this;
    axios.get("http://localhost:8000/user/transactions/" + current_username)
      .then((result) => {
        self.transactions = result.data;
      })
      .catch((error) => {
        alert("ERROR Servidor");
      });
  }    
}
</script>

<style>

td {
  text-align: center;
}

</style>