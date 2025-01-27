<template>
  <div class="container">
    <h1>Товары</h1>
    <button class="btn-dev" @click="addProduct">Добавить новый товар</button>
    <h2 v-if="!products.length">Товаров ещё не добавили!</h2>
    <div class="card-block">
      <div class="card" v-for="product in products" >
        <div class="card-int">
          <input type="checkbox" v-model="product.completed">
          <h2 :class="(product.completed) ? 'active' : ''" class="card-title">{{ product.name }}</h2>
        </div>
        <p class="card-price">Цена: {{ product.price }} ₽</p>
        <div class="btn-cl">
          <button class="btn-dev" @click="decreaseCount(product)">-</button>
          <p>{{ product.count }}</p>
          <button class="btn-dev" @click="increaseCount(product)">+</button>
        </div>
        <div class="btn-in" @click="editProduct(product)">Изменить</div>

      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: JSON.parse(localStorage.getItem("products")) || [] 
    };
  },
  methods: {
    addProduct() {
      const newProduct = {
        id: this.products.length + 1,
        name: prompt("Введите название товара:"),
        price: parseFloat(prompt("Введите цену товара:")),
        count: 1,
        completed: false
      };

      if (newProduct.name && (newProduct.price)) { 
        this.products.push(newProduct);
        this.saveStore(); 
      } else {
        alert("Название и цена товара не могут быть пустыми, а цена должна быть числом!");
      }
    },
    increaseCount(product) {
      product.count++;
      this.saveStore();
    },
    decreaseCount(product) {
      if (product.count > 1) {
        product.count--;
      } else {
        const index = this.products.indexOf(product);
        if (index !== -1) {
          this.products.splice(index, 1);
        }
      }
      this.saveStore(); 
    },
    editProduct(product) {
      const newName = prompt("Введите новое название товара:", product.name);
      const newPrice = parseFloat(prompt("Введите новую цену товара:", product.price));

      if (newName && !isNaN(newPrice)) {
        product.name = newName;
        product.price = newPrice;
        this.saveStore(); 
      } else {
        alert("Название и цена товара не могут быть пустыми, а цена должна быть числом!");
      }
    },
    saveStore() {
      localStorage.setItem("products", JSON.stringify(this.products));
    }
  }
};
</script>

<style scoped>
.container {
  max-width: 100%;
  margin: 0 auto;
  padding: 20px;
}
.btn-in {
  padding: 8px 76px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 5px;
}
.btn-dev {
  padding: 10px 20px;
  background-color: #42b983;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  margin: 5px;
}
.btn-in:hover {
  background-color: #3aa876;
}
.btn-dev:hover {
  background-color: #3aa876;
}

.card-block {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 50px;
  margin-top: 20px;
}
.card-int{
  display: flex;
  gap: 10px;
  align-items: center;
}

.card {
  display: flex ;
  flex-direction: column;
  align-items: center;
  width: 300px;
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 20px;
  text-align: center;
  margin-bottom: 20px;
}
.btn-cl{
  display: flex;
  gap: 10px;
}
.card-title {
  font-size: 24px;
}

.card-price {
  font-size: 18px;
  color: #333;
  margin-bottom: 15px;
}
.active{
  text-decoration: line-through;
  color: #3aa876;
}
</style>