<template>
<div class="title">
  <h1>Lista de productos</h1>
  <button class="btnh" @click="mostrarHistorial = !mostrarHistorial">
  {{ mostrarHistorial ? 'Ocultar' : 'Ver' }} Historial 🧾
</button>

<Historial v-if="mostrarHistorial" @cerrar="mostrarHistorial = false" />




   <button @click="mostrarCarrito = !mostrarCarrito">
    {{ mostrarCarrito ? 'Ocultar' : 'Ver' }} Carrito 🛒
  </button>
</div>
  <div class="container">
    
    <Producto v-for = "(p, i) in productos"
    :producto="p"
    :key="i"
 
    @agregar="agregarAlCarrito"
    
    />
  
  
  </div>

    
  <!-- MODAL DEL CARRITO -->
  <div v-if="mostrarCarrito" class="modal-overlay" @click.self="cerrarModal">
    <div class="modal-content">
      <h2>🛒 Carrito</h2>
      <ul>
        <li v-for="(item, index) in carrito" :key="index" class="carrito-item">
          {{ item.nombre }} - ${{ item.precio }}
        <button @click="eliminarDelCarrito(index)" class="btn-eliminar">❌</button>
        </li>
      </ul>
      <p><strong>Total:</strong> ${{ totalSinIVA }}</p>
      <p><strong>Total con IVA (15%):</strong> ${{ totalConIVA }}</p>

     <div class="btn-group">
      <button @click="cerrarModal" class="btn-cerrar">Cerrar</button>
      <button @click="cancelarCompra" class="btn-cancelar">Cancelar compra</button>
      <button @click="realizarCompra" class="btn-comprar">Realizar compra</button>
    </div>
    </div>
  </div>
</template>
<script>
import HistorialVue from './components/Historial.vue';

import Producto from './components/Producto.vue';
import Historial from './components/Historial.vue';
export default {
  components: {Producto, Historial},
  data(){
    return{
      productos:[
        {nombre:'Camisa', precio:30},
        {nombre:'Pantalon', precio:35},
        {nombre:'Abrigo', precio:15},
        {nombre:'Medias', precio:2},
        {nombre:'Blusas', precio:5},
        {nombre:'Faldas', precio:10},
        {nombre:'Boxer',precio:3},
        {nombre:'Camisetas',precio:15}
      ],
      carrito:[],
      mostrarCarrito: false,
      mostrarHistorial: false //
      
    };
 

  },
  methods:{
    agregarAlCarrito(producto){
      this.carrito.push(producto);
    
    },
      cerrarModal() {
      this.mostrarCarrito = false;
    },
    cancelarCompra() {
      this.carrito = [];
      this.cerrarModal();
    }, 
    eliminarDelCarrito(index){
      this.carrito.splice(index,1);


    },

    realizarCompra(){
      if (this.carrito.length === 0) {
        alert('El carrito esta vacio ');
        return; 
      }
      const total = this.totalConIVA;
       // 🔽 Crear objeto de compra
      const compra = {
          fecha: new Date().toLocaleString(),
          productos: this.carrito,
          totalConIVA: total
      };
      // 🔽 Obtener historial anterior
      const historial = JSON.parse(localStorage.getItem('historialCompras')) || [];

  // 🔽 Agregar esta compra al historial
      historial.push(compra);

  // 🔽 Guardar de nuevo en localStorage
      localStorage.setItem('historialCompras', JSON.stringify(historial));
      localStorage.setItem('carrito', JSON.stringify(this.carrito));


  // ✅ Confirmación al usuario
      alert(`¡Gracias por tu compra! Total con IVA: $${total}`);

  // 🔽 Limpiar y cerrar
      this.carrito = [];
      this.cerrarModal();

    }

  },
   computed: {
    totalSinIVA() {
      return this.carrito.reduce((sum, item) => sum + item.precio, 0);
    },
    totalConIVA() {
      return (this.totalSinIVA * 1.15).toFixed(2);
    }
  }
}
</script>
<style scoped>
.title{
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: -20px;

  

 
}@media (max-width: 600px) {
  .title {

    flex-direction: column;
    text-align: center;
   
  
    
  }.title button{
    position:relative;
    top: 5px;
    margin: auto;
    width: 200px;
  }.title .btnh{
    position: relative;
    top: -5px;
    background-color: aquamarine;
  }
}
.container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}.btnh{
  cursor: pointer;
  background-color: #364d97;
  position: relative;
  top: -3px;
}.btnh:hover{
  background-color: #3498db;
}
button {
  position: relative;
  top: 4px;width: 250px;
  height: 40px;
  background-color: #90964f;
  color: white;
  border: none;
  
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #36976d;
}

/* Modal */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  list-style: none;
  justify-content: center;
  align-items: center;
  z-index: 999;
}
.modal-content {
  background-color: white;
  padding: 55px;
  border-radius: 15px;
  max-width: 400px;
  width: 90%;
  box-shadow: 0 2px 10px rgba(0,0,0,0.3);


}.modal-content ul{
  list-style: none;
  padding: 0, 20px;
}
.btn-group {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 15px;
}

.btn-cerrar,
.btn-cancelar {
  flex: 1;
  padding: 10px;
  border-radius: 5px;
  color: white;
  border: none;
  cursor: pointer;
}

.btn-cerrar {
  background-color: #3498db;
}

.btn-cerrar:hover {
  background-color: #2980b9;
}

.btn-cancelar {
  background-color: #e74c3c;
}

.btn-cancelar:hover {
  background-color: #c0392b;
}
@media (max-width: 400px) {
  .modal-content {
    padding: 55px;
  }

  .btn-group {
    flex-direction: column;
    gap: 10px;
  }
}.carrito-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 6px 0;
  border-bottom: 1px solid #eee;
}

.btn-eliminar {
  background-color: transparent;
  border: none;
  width: 50px;
  text-align: center;
  font-size: 18px;
  cursor: pointer;
  margin-left: 10px;
  transition: transform 0.2s ease;
}

.btn-eliminar:hover {
  background-color: transparent;
  transform: scale(1.2);
}


</style>