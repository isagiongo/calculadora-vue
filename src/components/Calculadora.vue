<template>
  <div class="calculadora">
    <div class="display">{{valorCorrente || '0'}}</div>
    <div v-on:click="limpar" class="botao"><span>C</span></div>
    <div v-on:click="sinal" class="botao"><span>+/-</span></div>
    <div v-on:click="porcentagem" class="botao"><span>%</span></div>
    <div v-on:click="dividir" class="botao operadores"><span>/</span></div>
    <div v-on:click="juntarNumeros('7')" class="botao"><span>7</span></div>
    <div v-on:click="juntarNumeros('8')" class="botao"><span>8</span></div>
    <div v-on:click="juntarNumeros('9')" class="botao"><span>9</span></div>
    <div v-on:click="multiplicar" class="botao operadores"><span>x</span></div>
    <div v-on:click="juntarNumeros('4')" class="botao"><span>4</span></div>
    <div v-on:click="juntarNumeros('5')" class="botao"><span>5</span></div>
    <div v-on:click="juntarNumeros('6')" class="botao"><span>6</span></div>
    <div v-on:click="subtrair" class="botao operadores"><span>-</span></div>
    <div v-on:click="juntarNumeros('1')" class="botao"><span>1</span></div>
    <div v-on:click="juntarNumeros('2')" class="botao"><span>2</span></div>
    <div v-on:click="juntarNumeros('3')" class="botao"><span>3</span></div>
    <div v-on:click="somar" class="botao operadores"><span>+</span></div>
    <div v-on:click="juntarNumeros('0')" class="botao zero"><span>0</span></div>
    <div v-on:click="ponto" class="botao"><span>.</span></div>
    <div v-on:click="resultado" class="botao operadores"><span>=</span></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      valorCorrente: '',
      numeroAnterior: null,
      operador: null,
      operadorClicado: false,
    };
  },
  methods: {
    limpar() {
      this.valorCorrente = '';
    },
    sinal() {
      this.valorCorrente = this.valorCorrente.charAt(0) === '-'
        ? this.valorCorrente.slice(1)
        : `-${this.valorCorrente}`;
    },
    porcentagem() {
      this.valorCorrente = `${parseFloat(this.valorCorrente) / 100}`;
    },
    juntarNumeros(numero) {
      if (this.operadorClicado) {
        this.valorCorrente = '';
        this.operadorClicado = false;
      }
      this.valorCorrente = `${this.valorCorrente}${numero}`;
    },
    ponto() {
      if (this.valorCorrente.indexOf('.') === -1) {
        this.juntarNumeros('.');
      }
    },
    setarValor() {
      this.numeroAnterior = this.valorCorrente;
      this.operadorClicado = true;
    },
    dividir() {
      this.operador = (num1, num2) => num1 / num2;
      this.setarValor();
    },
    multiplicar() {
      this.operador = (num1, num2) => num1 * num2;
      this.setarValor();
    },
    somar() {
      this.operador = (num1, num2) => num1 + num2;
      this.setarValor();
    },
    subtrair() {
      this.operador = (num1, num2) => num1 - num2;
      this.setarValor();
    },
    resultado() {
      this.valorCorrente = `${this.operador(
        parseFloat(this.numeroAnterior),
        parseFloat(this.valorCorrente),
      )}`;
      this.numeroAnterior = null;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.calculadora {
  margin: 0 auto;
  width: 350px;
  display: grid;
  grid-template-columns: repeat(4,1fr);
  grid-auto-rows: minmax(50px, auto);
  font-size: 40px;
}
.display {
  grid-column: 1 / 5;
  background-color: #333;
  color: white;
}
.zero {
  grid-column: 1 / 3;
}
.botao {
  background-color: #f2f2f2;
  border: 1px solid #999;
  transition: all 0.5s;
  cursor: pointer;
}

.botao span {
  cursor: pointer;
  display: inline-block;
  position: relative;
  transition: 0.5s;
}

.botao span:after {
  content: '\00bb';
  position: absolute;
  opacity: 0;
  top: 0;
  right: -20px;
  transition: 0.5s;
}

.botao:hover span {
  padding-right: 25px;
}

.botao:hover span:after {
  opacity: 1;
  right: 0;
}

.operadores {
  background-color: orange;
  color: white;
}
</style>
