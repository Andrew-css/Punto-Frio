<template>
  <div id="body">
    <div id="p1">
      <h1>🅿unto F 10</h1>
      <img src="./img/images.png" alt="Producto" class="product-image" @mouseover="showCart">
    </div>
    <div id="p2">
      <img src="./img/images2.jpg" alt="Negocio" id="img2">
    </div>
    <div id="p3">
      <h1 id="p3t">Venta al detal y al por mayor</h1>
      <h1 id="p3t2">Haz tu pedido ➡️ 3103370459</h1>
    </div>
    <div id="p4">
      <h1 id="p4t">Productos</h1>
    </div>
    <div id="tarjetas">
      <div id="productos" v-for="(dato, index) in producto" :key="index">
        <img :src="dato.url" alt="Producto" id="imgtar">
        <h1>{{ dato.nombre }}</h1>
        <h1>Cantidad: {{ dato.cantidad }}</h1>
        <h1>Uds: {{ dato.unidad }}</h1>
        <h1>${{ (dato.precio).toLocaleString("es-ES", {
          style: "currency",
          currency: "COP",
          minimumFractionDigits: 0,
          maximumFractionDigits: 2,
        }) }}</h1>
        <button id="buton" @click="agregarAlCarrito(dato)">Añadir al carrito</button>
      </div>
    </div>
  </div>
  <div id="carrito" :class="{ 'visible': isCartVisible }" @mouseleave="hideCart">
    <div id="carritoc">
      <h1 id="p4t" style="font-size: 40px;">Carrito de Compras</h1>
    </div>
    <table>
      <thead>
        <tr>
          <th>Imagen</th>
          <th>Producto</th>
          <th>Cantidad</th>
          <th>Unidad</th>
          <th>Precio unitario</th>
          <th>Total</th>
          <th>Opciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(producto, index) in carrito" :key="index">
          <td><img :src="producto.url" alt="" class="product-image"></td>
          <td>{{ producto.nombre }}</td>
          <td>{{ producto.cantidadEnCarrito }}</td>
          <td>{{ producto.unidad }}</td>
          <td>${{ (producto.precio).toLocaleString("es-ES", {
            style: "currency",
            currency: "COP",
            minimumFractionDigits: 0,
            maximumFractionDigits: 2,
          }) }}</td>
          <td>${{ (producto.precio * producto.cantidadEnCarrito ).toLocaleString("es-ES", {
          style: "currency",
          currency: "COP",
          minimumFractionDigits: 0,
          maximumFractionDigits: 2,
        }) }}</td>  
          <td><button @click="eliminar(index)">❌</button></td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <td></td>
          <td></td>
          <td></td>
          <td><button @click="vaciarCarrito()" id="vaciar">Vaciar carrito</button></td>
          <td style="font-weight: bolder; font-size: 20px;">Total a pagar:</td>
          <td>${{ (totalCarrito).toLocaleString("es-ES", {
            style: "currency",
            currency: "COP",
            minimumFractionDigits: 0,
            maximumFractionDigits: 2,
          }) }}</td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
let isCartVisible = ref(false);

let producto = ref([
  { id: 1, nombre: "Águila Normal", cantidad: "1LT", unidad: "1x13", precio: 56000, url: './src/assets/AguilaLitro.jpg', cantidadEnCarrito: 1 },
  { id: 2, nombre: "Aguila Light", cantidad: "1LT", unidad: "1x13", precio: 60000, url: './src/assets/LightLitro.jpg', cantidadEnCarrito: 1 },
  { id: 3, nombre: "Aguila Normal", cantidad: "330ml", unidad: "1x30", precio: 58000, url: './src/assets/AguilaPersonal.jpg', cantidadEnCarrito: 1 },
  { id: 4, nombre: "Póker", cantidad: "1LT", unidad: "1x13", precio: 54000, url: './src/assets/PokerLitro.jpg', cantidadEnCarrito: 1 },
  { id: 5, nombre: "Póker", cantidad: "330ml", unidad: "1x30", precio: 58000, url: './src/assets/PokerPersonal.jpg', cantidadEnCarrito: 1 },
  { id: 6, nombre: "SixPack Coronita", cantidad: "210ml", unidad: "1x6", precio: 16000, url: './src/assets/Coronita.jpg', cantidadEnCarrito: 1 },
  { id: 7, nombre: "Club Colombia Dorada", cantidad: "330ml", unidad: "1x30", precio: 60000, url: './src/assets/ClubPersonal.jpg', cantidadEnCarrito: 1 },
  { id: 8, nombre: "Postobon Vdrio", cantidad: "1LT", unidad: "1x12", precio: 24000, url: './src/assets/GaseosaPostobonLitro.jpg', cantidadEnCarrito: 1 },
  { id: 9, nombre: "Postobon Personal Vdrio", cantidad: "350ml", unidad: "1x30", precio: 25000, url: './src/assets/GaseosaPersonal.jpg', cantidadEnCarrito: 1 },
  { id: 10, nombre: "Coca Cola Vdrio", cantidad: "1LT", unidad: "1x12", precio: 30000, url: './src/assets/CocaColaLitro.jpg', cantidadEnCarrito: 1 },
  { id: 11, nombre: "Coca Cola Vdrio", cantidad: "350ml", unidad: "1x30", precio: 50000, url: './src/assets/CocaCola250.jpg', cantidadEnCarrito: 1 },
  { id: 12, nombre: "Speed Max Lta", cantidad: "269ml", unidad: "1x32", precio: 42000, url: './src/assets/speed.jpg', cantidadEnCarrito: 1 },
])
let carrito = ref([])

function agregarAlCarrito(producto) {
  // Verificar si el producto ya está en el carrito
  const productoExistente = carrito.value.find(p => p.id === producto.id);

  if (productoExistente) {
    // Si el producto ya está en el carrito, aumentar su cantidad
    productoExistente.cantidadEnCarrito++;
  } else {
    // Si el producto no está en el carrito, añadirlo al carrito
    carrito.value.push({ ...producto });
  }
}

const totalCarrito = computed(() => {
  return carrito.value.reduce((total, producto) => total + producto.precio * producto.cantidadEnCarrito, 0);
});

function eliminar(index) {
  carrito.value.splice(index, 1)
}

function vaciarCarrito(){
  carrito.value=[]
}

function showCart() {
  isCartVisible.value = true;
}

function hideCart() {
  isCartVisible.value = false;
}

</script>


<style scoped>
#p1 {
  display: flex;
  position: fixed;
  width: 100%;
  justify-content: center;
  gap: 700px;
  background-color: rgb(246, 250, 44);
  font-size: 20px;
  z-index: 1;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  color: rgb(58, 85, 84);
}

.product-image {
  width: 40px;
  height: 40px;
  margin-top: 35px;
  z-index: 0;
}

#p2 {
  width: 100%;
  position: absolute;
  z-index: 0;
}

#img2 {
  position: absolute;
  top: 105px;
  width: 100%;
  height: 600px;
  z-index: 0;
}

#p3 {
  width: 100%;
  position: absolute;
  top: 700px;
  background-color: rgb(235, 46, 46);
  height: 100px;
  color: white;
  text-align: center;
  z-index: 0;
}

#p3t {
  width: 200px;
  font-size: 30px;
  margin-left: 550px;
  margin-top: 15px;
  z-index: 0;
}

#p3t2 {
  width: 400px;
  font-size: 30px;
  margin-left: 600px;
  position: relative;
  bottom: 80px;
  left: 400px;
  z-index: 0;
}

#p4 {
  width: 100%;
  position: absolute;
  top: 800px;
  height: 100px;
  color: rgb(7, 7, 7);
  text-align: center;
  z-index: 0;
  background-color: rgb(144, 185, 224);
}

#tarjetas {
  display: flex;
  flex-wrap: wrap;
  gap: 35px;
  position: relative;
  top: 900px;
  justify-content: center;
  padding: 0px 300px;
  background-color: rgb(144, 185, 224);
}

#productos {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border: 10px solid rgb(0, 0, 0);
  border-radius: 10px;
  padding: 15px;
  gap: 15px;
  text-align: center;
  width: 300px;
  font-size: 14.99px;
  background-color: rgb(244, 244, 245);
}

#p4t {
  font-size: 50px;
  z-index: 0;
}

#imgtar {
  border-radius: 10px;
  border: 7px solid black;
  width: 285px;
  height: 200px;
}

#buton {
  width: 200px;
  margin-left: 50px;
  font-size: 20px;
  padding: 10px;
  font-weight: bolder;
  border-radius: 10px;
}

#buton:hover {
  background-color: rgb(246, 250, 15);
}

#tabla {
  position: relative;
  top: 200px;
  left: 1300px;
  width: 500px;
  z-index: 2;
  background-color: rgb(0, 0, 0);
}

table {
  border-collapse: collapse;
  width: 100%;
  margin-left: 20px;
}

th {
  border: 1px solid rgb(68, 67, 67);
  padding: 8px;
  text-align: left;
  position: sticky;
  top: 0;
  background-color: rgb(46, 145, 211);
}

td {
  padding: 5px
}

tfoot tr:last-child td {
  margin-bottom: -1px;
}

#carrito {
  width: 630px;
  position: fixed;
  /* Cambia 'absolute' a 'fixed' */
  z-index: 5;
  top: 110px;
  /* Ajusta el valor para posicionarlo adecuadamente */
  right: 20px;
  /* Ajusta el valor para posicionarlo adecuadamente */
  background-color: rgb(36, 200, 250);
  max-height: calc(100vh - 150px);
  /* Asegura que el carrito no se extienda más allá de la pantalla */
  overflow-y: auto;
  /* Agrega scroll vertical si el contenido es más largo que la pantalla */
  scrollbar-width: thin;
  /* Grosor del scrollbar */
  border: 5px solid black;
  border-radius: 20px;
  max-height: calc(100vh - 150px);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  display: none;
  padding: 20px;
}

#carrito.visible {
  display: flex;
}

#carrito::-webkit-scrollbar {
  width: 15px;
  /* Ancho del scrollbar */
}

#carrito::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0.4);
  /* Color del pulgar (barra móvil) */
  border-radius: 40px;
  /* Borde redondeado */
}

#carrito::-webkit-scrollbar-track {
  background-color: transparent;
  /* Color de fondo del scrollbar */
}

#carritoc {
  display: flex;
  justify-content: center;
}

#carrito td img.product-image {
  width: 70px;
  /* Ajusta el valor según tus preferencias */
  height: 70px;
  /* Ajusta el valor según tus preferencias */
  border-radius: 5px;
}

#vaciar{
  border-radius: 5px;
  font-size: 15px;
  background-color: gold;
  color: black;
  font-weight: bolder;
  padding: 8px;
  cursor: pointer;
}

#vaciar:hover{
  color: white;
  background-color: red;
}
</style>

