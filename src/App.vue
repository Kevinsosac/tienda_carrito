<template>
  <div class="caja">
    <div class="barra">
      <h1>ACCESORIOS Y CALZADO WARRIORS 🏀</h1>
      <a href="#" class="bx bxs-cart-alt" @click="mostrarModal = true">
        <span class="carrito-cantidad">{{ carrito.length }}</span>
      </a>
    </div>
    <div class="titulo">
      <img src="./assets/fondo.jpg" alt="">
    </div>
    <div class="port"></div>
    <div class="productos">
      <div class="box-contaier">
        <div v-for="(producto, index) in listaProductos" :key="index" class="box">
          <div class="imagen">
            <img :src="producto.imagenSrc" alt="" />
          </div>
          <div class="content">
            <div class="strellas">
              <i class="bx bxs-star" v-for="star in producto.estrellas" :key="star"></i>
              <i class="bx bxs-star-half"></i>
            </div>
            <h3>{{ producto.nombre }}</h3>
            <p>{{ producto.descripcion }}</p>
            <span>{{ formatoPesoColombiano(producto.precio) }}</span>
          </div>
          <button class="btn-pe" @click="agregarAlCarrito(producto)">
              Agregar
            </button>
        </div>
      </div>
    </div>
    <div v-if="mostrarModal" class="modal">
      <div class="modal-content">
        <h2 class="carrito-titulo">Carrito de Compras</h2>
        <div class="carrito-lista">
          <div v-for="(item, index) in carrito" :key="index" class="carrito-item">
            <span><img :src="item.producto.imagenSrc" /><span class="nombre">{{
              item.producto.nombre
            }}</span>
              <span class="cantidad">{{ item.cantidad }}</span>
              <span class="precio">${{ item.producto.precio }}</span></span>
              
              <button class="eliminar" @click="eliminarDelCarrito(index)">❌</button>
          </div>
        </div>
        <p class="carrito-total">Total compra: {{ formatoPesoColombiano(calcularTotal()) }}</p>
        <div class="bot">
          <button class="cerrar-modal" @click="mostrarModal = false">Cerrar</button>
          <button class="vaciar" @click="vaciarCarrito">
            Vaciar carrito
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import "boxicons/css/boxicons.min.css";

let carrito = ref([]);
let mostrarModal = ref(false);

let listaProductos = ref([
{
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQBnHvmWZCqzuqJYsnKSai11xn7Wq4Q5RmpOg&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Balon ADIDAS",
    descripcion: "Balon original de adidas coontramarcado",
    precio: 120000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRO7iyMr7jlVj4X2woCcR3actTdOMhV4EH28Q&usqp=CAU",
    estrellas: [1, 2, 3],
    nombre: "Balon NIKE",
    descripcion: "Balon original de adidas coontramarcado",
    precio: 200000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQ5aXC3R_AbfBVzJn5a2KeWRgKiKCbiS3XfuQ&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "balon SPALDING",
    descripcion: "Balon original de spaldin perfecto para canchas de madera",
    precio: 200000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQCQWAv-5Auyyi5cC3ONCTkJnxQIGG4WAW78A&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Mini aro",
    descripcion: "Una perfecta distracion en casa para nuestros hijos",
    precio: 250000,
  },
  {
    imagenSrc: "https://http2.mlstatic.com/D_NQ_NP_776584-MCO69726442790_052023-O.webp",
    estrellas: [1, 2, 3, 4],
    nombre: "Protector de codo",
    descripcion: "Mantiene caliente el antebrazo y protege muy bien el codo",
    precio: 50000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRoMJvDIdLEoIHstTsLH1TcohFuPY-TSIFk1w&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Rodilleras",
    descripcion: "Protegen muy bien las rodillas y permiten un movimiento normal",
    precio: 80000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTZFBUzrwd6yrju0T7rj9ATgr4Bbkn3g28nPw&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Zapatillas curry 3",
    descripcion: "zapatillas originales de under armour en colaboracion con sthepen curry",
    precio: 300000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTQXWIS9NwzU0E1-LrFt6sHDcUrvhexG2TOJQ&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Zapatillas ADIDAS 4",
    descripcion: "Muy comodas, con muy buena precion en el tobillo",
    precio: 150000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSmBaAFPkHcrF2I68VDvojFEcM59xtAIiGXhw&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Zapatillas curry 2",
    descripcion: "zapatillas originales de under armour en colaboracion con sthepen curry",
    precio: 400000,
  },
  {
    imagenSrc: "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRfgaDDiBpu8pHVwzvj3qEDO6XGuX98ud5B-_7pCqHwme5oisc0nTBbrD0QuuRR7FcYpgU&usqp=CAU",
    estrellas: [1, 2, 3, 4],
    nombre: "Zapatillas UNDER ARMUR",
    descripcion: "zapatillas originales de under armour, muy comodas y muy buena proteccion de el tobillo",
    precio: 350000,
  },
  {
    imagenSrc: "data:image/jpeg;base64,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",
    estrellas: [1, 2, 3, 4],
    nombre: "Lamelo ball 2",
    descripcion: "Zapatillas originales, con muy beun agarre en la suela",
    precio: 750000,
  },
  {
    imagenSrc: "data:image/jpeg;base64,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",
    estrellas: [1, 2, 3, 4],
    nombre: "Lamello ball 3",
    descripcion: "Zapatillas originales, con muy beun agarre en la suela y muy buena proteccion de el tobillo",
    precio: 850000,
  },
])

const formatoPesoColombiano = (valor) => {
  return valor.toLocaleString('es-CO', {
    style: 'currency',
    currency: 'COP'
  });
}

const agregarAlCarrito = (producto) => {
  let itemEnCarrito = carrito.value.find(
    (item) => item.producto === producto
  );
  if (itemEnCarrito) {
    itemEnCarrito.cantidad++;
  } else {
    carrito.value.push({ producto: producto, cantidad: 1 });
  }
}

const eliminarDelCarrito = (index) => {
  carrito.value.splice(index, 1);
}

const vaciarCarrito = () => {
  carrito.value = [];
}

const calcularTotal = () => {
  return carrito.value.reduce(
    (total, item) => total + item.producto.precio * item.cantidad,
    0
  );
}

</script> 

<style>
* {
  margin: 0%;
  padding: 0%;
  box-sizing: border-box;
  font-family: 'Permanent Marker', cursive;
  outline: none;
  border: none;
  text-decoration: none;
  transition: all 0.2s linear;
}

body {
  background-image: url("./assets/balon.jpg");
  object-fit: cover;
  position: relative;
  text-align: center;
  background-size: cover;
  height: 400px;
}

h1 {
  margin-top: 80px;
}

.cantidad {
  margin-left: 85px;
}

img {
  object-fit: cover;
  border-radius: 5rem;
}

a {
  text-decoration: none;
}

.precio {
  margin-left: 80px;
}

.nombre {
  margin-left: 10px;
}

.barra {
  text-align: center;
  background-color: rgba(255, 255, 255, 0.767);
  border-radius: 2rem;
  margin-right: 23%;
  margin-left: 28.5%;
  margin-top: 80px;
  z-index: 1;
  position: absolute;
}

.titulo {
  position: relative;
  margin-top: 15px;
}

.titulo img {
  height: 450px;
  background-size: cover;
  width: 95%;
}

.barra h1 {
  margin-top: 3rem;
  color: #020100;
  font-size: 3rem;
  font-style: italic;
  font-weight: bold;
}

.barra i,
.barra a {
  cursor: pointer;
  height: 4.5rem;
  line-height: 4.5rem;
  width: 4.5rem;
  text-align: center;
  font-size: 1.7rem;
  color: #ff0000;
  border-radius: 50%;
  background: #eee;
}

.productos {
  margin: 5rem;
}

.box-contaier {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(28rem, 1fr));
  gap: 1.5rem;
}

.box-contaier .box {
  padding: 2.5rem;
  background: #c7c4c4c0;
  border-radius: 0.5rem;
  border: 0.1rem solid rgba(255, 0, 0, 0.233);
  box-shadow: var(--box-shadow);
  position: relative;
  overflow: hidden;
  text-align: center;
}

.box-contaier .box img {
  justify-content: center;
  height: 17rem;
}

.box-contaier .box h3 {
  color: #000000;
  font-weight: bold;
  font-size: 2rem;
  margin: 0.5rem;
}

.box-contaier .box .strellas {
  padding: 0.8rem;
}

.box-contaier .box .strellas i {
  font-size: 1.2rem;
  color: #fbff00a4;
}

.btn-pe {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.7rem;
  color: #000000;
  background: #858586a6;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 2rem;
}

.box-contaier .box span {
  color: #000000;
  font-weight: bolder;
  font-size: 2.5rem;
}

.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(129, 128, 128, 0.363);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 100;
}

.modal-content {
  background-color: rgba(211, 208, 208, 0.836);
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(66, 65, 65, 0.521);
  width: 70%;
  max-width: 600px;
  overflow-y: auto;
}

.detalle-producto {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #ccc;
}

.detalle-producto img {
  max-width: 60px;
  margin-right: 10px;
}

.detalle-info h3 {
  margin: 0;
}

.detalle-info p {
  margin: 0;
}

.carrito-titulo {
  padding-bottom: 10px;
  margin-bottom: 10px;
  font-style: italic;
  color: #000000;
  text-align: center;
  font-size: 2rem;
  font-weight: bold;
}

.modal-content .carrito-item span img {
  width: 40px;
  height: 40px;
  object-fit: cover;
  border-radius: 7px;
}

.carrito-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #000000;
  padding: 10px;
}

.carrito-producto {
  display: flex;
  align-items: center;
}

.vaciar {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.3rem;
  color: #131212;
  background-color: #007bff;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 1rem;
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  margin-left: 200px;


}

.carrito-cantidad {
  flex: 1;
  text-align: center;
}

.carrito-eliminar {
  margin-left: 10px;
}

.carrito-total {
  font-weight: bold;
  margin-top: 10px;
}

.eliminar {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1rem;
  color: #1a1918;
  background: #0000003f;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.5rem .98rem;
}

.cerrar-modal {
  margin-top: 1rem;
  display: inline-block;
  font-size: 1.3rem;
  color: #131212;
  background: #3b3b3a3f;
  border-radius: 0.5rem;
  cursor: pointer;
  padding: 0.8rem 2rem;
}

/* Estilos para el modal */
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  max-width: 600px;
  width: 100%;
}

.carrito-lista {
  display: flex;
  flex-direction: column;
}

.carrito-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 10px;
}

.producto-imagen {
  width: 50px;
  height: 50px;
  object-fit: cover;
  margin-right: 303px;
}

.producto-detalle {
  flex: 1;
  margin-left: -14px;
}

.carrito-total {
  text-align: right;
  font-weight: bold;
  margin-top: 10px;
}

/* Otros estilos */
.carrito-titulo {
  font-size: 24px;
  margin-bottom: 10px;
}

.cerrar-modal {
  background-color: #007bff;
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
}

.eliminar {
  background-color: #ff403657;
  color: white;
  border: none;
  padding: 5px 10px;
  border-radius: 50%;
  cursor: pointer;
}


.carrito-lista {
  max-height: 300px;
  overflow-y: auto;
}
</style>