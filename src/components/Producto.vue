<template>
    
    <div class="card">
        <h3>{{producto.nombre}}</h3>
        <p>Precio: ${{producto.precio}}</p>
        <button @click="agregarAlCarrito">+ Carrito</button>
    </div>
</template>
<script>
export default {
    props:['producto'],
    methods:{
        agregarAlCarrito(){
            this.$emit('agregar', this.producto)
        }
    }
}
</script>

<style scoped>


.card {
    display: flex;
    flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 10px;
  padding: 16px;
  margin: 12px;
  width: 200px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  text-align: center;
  transition: 0.3s ease;
}
.card:hover {
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
}
button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 5px;
  cursor: pointer;
}
button:hover {
  background-color: #36976d;
}
</style>