<template>
  <div class="stock-div">
    <h3>Sweets stock management system</h3>
    <p>Stock Value = £{{ total() }}</p>
    <table class="main-table">
      <tr>
        <td class="data">Product ID</td>
        <td class="data">Product Name</td>
        <td class="data">Product Quantity</td>
        <td class="data">Product Price</td>
        <td class="data">Product Currency</td>
        <td class="data">Product Description</td>
        <td class="data"></td>
      </tr>
      <tr class="data-rows" v-for="row in products" v-bind:key="row.id">
        <td class="data">{{ row.id }}</td>
        <td class="data">{{ row.name }}</td>
        <td class="data">{{ row.quantity }}</td>
        <td class="data">£{{ row.price }}</td>
        <td class="data">{{ row.currency }}</td>
        <td class="data">{{ row.description }}</td>
        <td>
          <button type="button" v-on:click="deleteRow(row.id)">Delete</button>
        </td>
      </tr>
    </table>
    <div class="add-item">
      <form class="add-form">
        <input
          class="input-forms"
          type="text"
          id="name"
          v-model="name"
          placeholder="Product Name"
          required
        /><br />
        <input
          class="input-forms"
          type="number"
          id="quantity"
          v-model="quantity"
          placeholder="Product Quantity"
          required
        /><br />
        <input
          class="input-forms"
          type="text"
          id="price"
          v-model="price"
          placeholder="Product price"
          required
        /><br />
        <input
          class="input-forms"
          type="text"
          id="currency"
          v-model="currency"
          placeholder="Product Currencty (GBP)"
          required
        /><br />
        <input
          class="input-forms"
          type="text"
          id="desc"
          v-model="desc"
          placeholder="Product Description"
          required
        /><br />
        <button class="input-btn" type="button" @click="add">
          Submit product
        </button>
      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      products: [{}],
      id: "",
      name: "",
      quantity: "",
      price: "",
      currency: "",
      desc: "",
    };
  },
  methods: {
    stockData() {
      axios
        .get("http://localhost:5000/sweets")
        .then((response) => {
          this.products = response.data;
        })
        .catch((error) => {
          if (error.response) {
            console.log(error.response.status);
          } else if (error.request) {
            console.log(error.request);
          } else {
            console.log(error.message);
          }
        });
    },
    total() {
      return this.products.reduce(function (total, product) {
        return total + product.quantity * product.price;
      }, 0);
    },
    deleteRow(id) {
      axios
        .delete("http://localhost:5000/sweets/delete/" + id)
        .then((response) => {
          this.stockData();
          alert("success");
        });
    },
    add() {
      axios
        .post(`http://localhost:5000/sweets/add`, {
          name: this.name,
          quantity: this.quantity,
          price: this.price,
          currency: this.currency,
          desc: this.desc,
        })
        .then((response) => {
          console.log(response);
          this.stockData();
          alert('Success');
        })
        .catch((error) => {
          if (error.response) {
            console.log(error.response.status);
          } else if (error.request) {
            console.log(error.request);
          } else {
            console.log(error.message);
          }
        });
        this.id = "";
        this.name = "";
        this.quantity = "";
        this.price = "";
        this.currency = "";
        this.desc = "";
    },
  },
  mounted: function () {
    this.stockData();
  },
};
</script>


<style scoped>
.main-table {
  width: 80%;
  margin-left: auto;
  margin-right: auto;
  border: 0.1rem solid black;
  font-family: "Poppins", sans-serif;
}

.data {
  border: 0.1rem solid black;
}

.add-form {
  border: 0.2rem solid black;
  border-radius: 1rem;
  width: 15%;
  margin-top: 2rem;
  margin-left: auto;
  margin-right: auto;
}

.input-forms {
  height: 2rem;
  width: 80%;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}

.input-btn {
  margin-bottom: 0.5rem;
}
</style>