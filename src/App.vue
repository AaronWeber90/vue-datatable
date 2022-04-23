<script>
import Modal from "./components/Modal.vue";
import NewProductModal from "./components/NewProductModal.vue";

export default {
  components: {
    Modal,
    NewProductModal,
  },
  data() {
    return {
      // change something here?
      productToEdit: {
        bestBefore: new Date(),
      },
      productData: [
        {
          sku: "1234",
          type: "Carrot",
          bestBefore: "2021-10-03",
          stock: 240,
        },
        {
          sku: "4567",
          type: "Potatoes",
          bestBefore: "2021-10-20",
          stock: 140,
        },
        {
          sku: "4321",
          type: "Corn",
          bestBefore: "2021-08-20",
          stock: 40,
        },
        {
          sku: "2416",
          type: "Cucumber",
          bestBefore: "2021-07-16",
          stock: 80,
        },
      ],
      showModal: false,
      searchQuery: "",
      isNewProduct: false,
      newProduct: {
        sku: "",
        type: "",
        bestBefore: "2021-07-16",
        stock: 1,
      },
      isNewProductModal: false,
    };
  },
  methods: {
    addNewProduct() {
      if (this.newProduct.type.trim() && this.newProduct.sku) {
        this.productData = [...this.productData, this.newProduct];
        this.closeModal();
        this.newProduct = {
          sku: "",
          type: "",
          bestBefore: "2021-07-16",
          stock: 1,
        };
      } else {
        alert("Please fill out all fields!");
      }
    },
    editNewProduct() {
      this.isNewProductModal = true;
    },
    removeProduct(product) {
      this.productData = this.productData.filter((t) => t !== product);
    },
    editProduct(product) {
      this.showModal = true;
      this.productToEdit = product;
    },
    closeModal() {
      this.showModal = false;
      this.isNewProductModal = false;
      this.productToEdit = {
        bestBefore: new Date(),
      };
    },
  },
  computed: {
    filterProducts() {
      return this.productData.filter(
        (item) =>
          item.type.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          item.sku.includes(this.searchQuery)
      );
    },
  },
};
// https://www.smashingmagazine.com/2020/01/data-components-vue-js/
</script>

<template>
  <div>
    <div class="search-container">
      <input v-model="searchQuery" placeholder="e.g. Carrot" />
      <button @click="editNewProduct">Add product</button>
    </div>
    <table class="product-table">
      <tr class="product-table__header">
        <th>SKU</th>
        <th>Name</th>
        <th>Best-Before</th>
        <th>Stock</th>
        <th></th>
      </tr>
      <tr
        v-for="product in filterProducts"
        :key="product.sku"
        class="product-table__row"
      >
        <td>{{ product.sku }}</td>
        <td>{{ product.type }}</td>
        <td>{{ product.bestBefore }}</td>
        <td>{{ product.stock }}</td>
        <td>
          <button @click="editProduct(product)">edit</button
          ><button @click="removeProduct(product)">X</button>
        </td>
      </tr>
    </table>
    <Modal
      v-show="showModal"
      @close-modal="closeModal"
      :productData="productToEdit"
      @edit-product="editNewProduct"
    />
    <NewProductModal
      v-show="isNewProductModal"
      @close-modal="closeModal"
      @add-product="addNewProduct"
      :newProduct="newProduct"
      :newProductModal="newProductModal"
    />
    <p class="no-search-result" v-show="!filterProducts.length">
      Nothing found!
    </p>
  </div>
</template>

<style>
@import "./assets/base.css";

.search-container {
  display: flex;
  justify-content: space-between;
  padding: 1em 0;
}

.product-table {
  border-collapse: collapse;
  width: 100%;
}

.product-table__header {
  background: grey;
  color: #fff;
}

.product-table th {
  font-weight: bold;
}

.product-table th,
.product-table td {
  padding: 0.7em;
}

.product-table__row + .product-table__row {
  border-top: 1px solid #000 !important;
}

#app {
  max-width: 1280px;
  margin: 0 auto;
  padding: 4rem 2rem;
  font-weight: normal;
}

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

.no-search-result {
  text-align: center;
  padding: 2em;
}

@media (min-width: 1024px) {
  body {
    display: flex;
    place-items: center;
  }

  #app {
    padding: 0 2rem;
  }

  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }

  .logo {
    margin: 0 2rem 0 0;
  }
}
</style>
