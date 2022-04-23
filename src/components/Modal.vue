<template>
  <div class="modal-overlay" @click="$emit('close-modal')">
    <div class="modal" @click.stop>
      <h6 v-show="!newProductModal">Edit product: {{ productData.type }}</h6>
      <div>
        <label for="product-date"
          >Best-Before-Date
          <input
            type="date"
            id="product-date"
            :value="
              new Date(productData.bestBefore).toISOString().substr(0, 10)
            "
            @input="(event) => (productData.bestBefore = event.target.value)"
            required
          />
        </label>
      </div>
      <br />
      <div>
        <label for="product-stock"
          >Stock
          <input
            type="number"
            id="product-stock"
            v-model="productData.stock"
            min="1"
            max="10000"
            required
          />
        </label>
      </div>
      <br />
      <div>
        <label for="product-sku"
          >SKU
          <input
            type="text"
            id="product-sku"
            v-model="productData.sku"
            min="1"
            max="9999"
          />
        </label>
      </div>
      <br />
      <div>
        <label for="product-type"
          >Type
          <input
            type="text"
            id="product-type"
            v-model="productData.type"
            minlength="5"
          />
        </label>
      </div>

      <br />
      <button @click="$emit('close-modal')">Save</button>
    </div>
  </div>
</template>

<script>
export default {
  props: ["productData"],
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: center;
  background-color: #000000da;
}

.modal {
  text-align: center;
  background-color: white;
  height: 500px;
  width: 500px;
  margin-top: 10%;
  padding: 60px 0;
  border-radius: 20px;
}
.close {
  margin: 10% 0 0 16px;
  cursor: pointer;
}

.close-img {
  width: 25px;
}

.check {
  width: 150px;
}

h6 {
  font-weight: 500;
  font-size: 28px;
  margin: 20px 0;
}

p {
  font-size: 16px;
  margin: 20px 0;
}

button {
  background-color: #ac003e;
  width: 150px;
  height: 40px;
  color: white;
  font-size: 14px;
  border-radius: 16px;
  margin-top: 50px;
}
</style>
