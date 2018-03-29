<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js  & Firebase</h1>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Add New Product</h3>
      </div>
      <div class="panel-body">
         <form id="form" class="form-inline" >
          <div class="form-group">
            <label for="productName">Name:</label>
            <input type="text" id="productName" class="form-control" v-model="newProduct.name">
          </div>
          <div class="form-group">
            <label for="productSKU">SKU:</label>
            <input type="text" id="productSKU" class="form-control" v-model="newProduct.sku">
          </div>
          <div class="form-group">
            <label for="productPrice">Price:</label>
            <input type="text" id="productPrice" class="form-control" v-model="newProduct.price">
          </div>
          <div class="form-group">
            <label for="productWebPrice">Web Price:</label>
            <input type="text" id="productWebPrice" class="form-control" v-model="newProduct.webprice">
          </div>
        
        
          <button type="submit" class="btn btn-primary" v-if="!newProduct['.key']" v-on:click="addProduct">Add Product</button>
           <button type="button" class="btn btn-primary" v-if="newProduct['.key']" v-on:click="updateProduct(newProduct)">Update</button>
        </form>
      </div>
    </div>
    <div class="panel panel-default">
      <div class="panel-heading">
        <h3 class="panel-title">Products List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Title</th>
              <th>SKU</th>
              <th>Price</th>
              <th>Webprice</th>
       
              <th></th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="product in products"  :key="product['.key']">
           
              <td>{{product.name}}</td>
              <td>{{product.sku}}</td>
              <td>{{product.price }}</td>
              <td>{{product.webprice}}</td>
     
              <td><button v-on:click="removeProduct(product)" type="button" class="btn btn-default btn-sm"><span class="glyphicon glyphicon-trash" aria-hidden="true" ></span></button></td>
              <td><button v-on:click="editProduct(product)" type="button" class="btn btn-danger btn-sm"><span class="glyphicon glyphicon-pencil" aria-hidden="true" ></span></button></td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
       
    
           
  </div>
</template>

<script>

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
    apiKey: "AIzaSyBK4M24Q6mnvzWqGpYTd9x0XtW94Xv5xcw",
    authDomain: "bigsale-aba4b.firebaseapp.com",
    databaseURL: "https://bigsale-aba4b.firebaseio.com",
    projectId: "bigsale-aba4b",
    storageBucket: "bigsale-aba4b.appspot.com",
    messagingSenderId: "133387858024"

  };
  
let app = Firebase.initializeApp(config)
let db = app.database()

let productRef = db.ref('products')

export default {
  name: 'app',

  firebase: {
    products: productRef
  },
  
  data () {
    return {
      newProduct: {
         description:'',
          imageurl:'',
          name:'',
          price:'',
          quantity:'',
          sku:'',
          webprice:'' 
      }
    }
  },
  
   methods: {
      addProduct: function () {
        productRef.push(this.newProduct);
        this.newProduct.description='';
        this.newProduct.imageurl='';
        this.newProduct.name='';
        this.newProduct.price='';
        this.newProduct.quantity='';
        this.newProduct.sku='';
        this.newProduct.webprice='' 
       
      },
      removeProduct: function (product) {
        productRef.child(product['.key']).remove()
        toastr.success('Product removed successfully')
      },
      editProduct: function (product) {
        this.newProduct = product
     
      },
        updateProduct: function(product) {
            const childKey = product['.key'];
            delete product['.key'];
            this.$firebaseRefs.products.child(childKey).set(product)
            this.newProduct = ""
        }, 
    },


 
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 20px;
}

</style>