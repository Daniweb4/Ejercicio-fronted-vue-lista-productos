<!-- components/Historial.vue -->
<template>
  <div class="modal-overlay" @click.self="$emit('cerrar')">
    <div class="modal-content">
      <h2>🧾 Historial de Compras</h2>
      <div v-if="historial.length">
        <div v-for="(compra, i) in historial" :key="i" class="compra">
          <p><strong>Fecha:</strong> {{ compra.fecha }}</p>
          <ul>
            <li v-for="(prod, j) in compra.productos" :key="j">
              {{ prod.nombre }} - ${{ prod.precio }}
            </li>
          </ul>
          <p><strong>Total con IVA:</strong> ${{ compra.totalConIVA }}</p>
          <hr />
        </div>
      </div>
      <div v-else>
        <p>No hay compras aún.</p>
      </div>
      <button class="btn-cerrar" @click="$emit('cerrar')">Cerrar</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      historial: JSON.parse(localStorage.getItem('historialCompras')) || []
    };
  }
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 10px;
  max-height: 80vh;
  overflow-y: auto;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
}ul{
    list-style: none;
}

.btn-cerrar {
  margin-top: 1rem;
  padding: 10px;
  background-color: #e74c3c;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.btn-cerrar:hover {
  background-color: #c0392b;
}
</style>
