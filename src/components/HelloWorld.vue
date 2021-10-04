<template>
  <div class="marcador">
    <div>{{ nivel }}</div>
    <div>{{ puntos }}</div>
  </div>
  <div class="operacion">
    {{ textoOperacion }} <br />
    =
  </div>
  <input type="number" class="campo" v-model="respuesta" /> <br />
  <button class="btn" @click="comenzar">Nueva partida</button>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data: () => ({
    operadores: ['+', '-', '*', '/'],
    currentOperador: '+',
    textoOperacion: '',
    respuesta: 0,
    nivel: 0,
    puntos: 0,
    numero1: 0,
    numero2: 0,
  }),
  watch: {
    respuesta(value) {
      if (
        value == eval(`${this.numero1} ${this.currentOperador} ${this.numero2}`)
      ) {
        this.sumarPuntos();
        this.crearOperacion();
      }
    },
  },
  computed: {
    unidades() {
      if (this.nivel > 100) return 1000;
      if (this.nivel > 90) return 900;
      if (this.nivel > 80) return 800;
      if (this.nivel > 70) return 700;
      if (this.nivel > 60) return 600;
      if (this.nivel > 50) return 400;
      if (this.nivel > 40) return 200;
      if (this.nivel > 30) return 100;
      if (this.nivel > 20) return 50;
      if (this.nivel > 10) return 20;
      if (this.nivel >= 0) return 10;
    },
  },
  methods: {
    comenzar() {
      this.nivel = 1;
      this.puntos = 0;
      this.crearOperacion();
    },

    sumarPuntos() {
      if (this.currentOperador == '+' || this.currentOperador == '-') {
        this.puntos += Math.abs(this.numero1 * this.numero2);
      } else if (this.currentOperador == '+' || this.currentOperador == '-') {
        this.puntos += Math.abs(this.numero1 * this.numero2 * 1.5);
      }
      this.respuesta = null;
      this.nivel += 1;
    },

    crearOperacion() {
      this.currentOperador = this.operadores[Math.floor(Math.random() * 4)];
      this.numero1 = Math.floor(
        Math.random() * (Math.random() * this.unidades)
      );
      this.numero2 = Math.floor(
        Math.random() * (Math.random() * this.unidades)
      );
      if (this.numero1 == 0 || this.numero2 == 0) {
        this.crearOperacion();
      }
      if (
        eval(`${this.numero1} ${this.currentOperador} ${this.numero2}`) % 1 !=
        0
      ) {
        this.crearOperacion();
      }
      this.textoOperacion = `${this.numero1} ${this.currentOperador} ${this.numero2}`;
      if (this.currentOperador == '*')
        this.textoOperacion = `${this.numero1} x ${this.numero2}`;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.operacion {
  font-size: 10vw;
}
.btn {
  font-size: 4vw;
  padding: 3vw 5vw 3vw 5vw;
  border-radius: 5vw;
  border: solid 1px #ccc;
  cursor: pointer;
  background: #eee;
}
.btn:hover {
  background: #ccc;
}
.marcador {
  display: flex;
  justify-content: space-between;
  font-size: 4vw;
  padding: 2vw;
  padding-bottom: 5vw;
  padding-top: 0;
}
.campo {
  text-align: center;
  font-size: 6vw;
  padding: 2vw;
  margin-top: 2vw;
  margin-bottom: 8vw;
  border: none;
}
</style>
