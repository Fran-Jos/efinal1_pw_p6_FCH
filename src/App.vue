<template>
  <div class="container" v-if="juego">
    <div class="valores">
      <p>Puntaje:{{puntaje}}</p>
      <p>Intento: {{intento}}</p>
    </div>

    <div class="cuerpo">
      <cuadrado :imagen="img1" :texto="text1" />
      <cuadrado :imagen="img2" :texto="text2" />
      <cuadrado :imagen="img3" :texto="text3" />
    </div>

    <div class="boton">
      <button @:click="jugar">JUGAR</button>
    </div>
  </div>

  <div v-if="perdedor">
    <h1>Haz utlizado tus 5 intentos</h1>
    <h1>el juego ha terminado, intentalo nuevamente</h1>
    <button @:click="reiniciar">Nuevo Juego</button>
  </div>
    <div v-if="ganador">
    <h1>Puntaje:{{puntaje}}</h1>
    <h1>Felicidades has ganado un premio de $ 10.000,00</h1>
    <button @:click="reiniciar">Nuevo Juego</button>
  </div>
</template>

<script>
import cuadrado from "./components/Cuadrado.vue";

export default {
  name: "App",
  components: {
    cuadrado,
  },

  data() {
    return {
      img1: "https://via.placeholder.com/250",
      text1: "XXXXXXXXXXXXX",
      img2: "https://via.placeholder.com/250",
      text2: "XXXXXXXXXXXXX",
      img3: "https://via.placeholder.com/250",
      text3: "XXXXXXXXXXXXX",
      lista: [],
      puntaje: 0,
      intento: 0,
      juego:true,
      ganador:false,
      perdedor:false
    };
  },

  methods: {
    async consumirApi(n) {
      const data = await fetch("https://pokeapi.co/api/v2/pokemon/" + n).then(
        (response) => response.json()
      );
      const { name, sprites } = data;
      const image = sprites.front_default;
      return { name, image };
    },

    async cargarLista() {
      for (let i = 0; i < 3; i++) {
        const data = await this.consumirApi(i + 1);
        this.lista.push(data);
      }
    },

    async jugar() {
      this.intento++;
      await this.cargarLista();

      const random = Math.floor(Math.random() * this.lista.length);
      this.img1 = this.lista[random].image;
      this.text1 = this.lista[random].name;

      const random2 = Math.floor(Math.random() * this.lista.length);
      this.img2 = this.lista[random2].image;
      this.text2 = this.lista[random2].name;

      const random3 = Math.floor(Math.random() * this.lista.length);
      this.img3 = this.lista[random3].image;
      this.text3 = this.lista[random3].name;

      if (
        this.text1 === this.text2 &&
        this.text1 === this.text3 &&
        this.text2 === this.text3
      ) {
        this.puntaje += 5;
      }
      if (
        this.text1 === this.text2 ||
        this.text1 === this.text3 ||
        this.text2 === this.text3
      ) {
        this.puntaje += 2;
      }



      if(this.intento === 5 ){

        if(this.puntaje >= 10){
          this.ganador = true;
          this.juego = false;
          return;
        }
        this.juego = false;
        this.perdedor = true;
      }


    },

  
    reiniciar(){
      img1: "https://via.placeholder.com/250";
      text1: "XXXXXXXXXXXXX";
      img2: "https://via.placeholder.com/250";
      text2: "XXXXXXXXXXXXX";
      img3: "https://via.placeholder.com/250";
      text3: "XXXXXXXXXXXXX"
      lista: [];
      puntaje: 0;
      intento: 0;
      juego:true;
      ganador:false;
      perdedor:false;
    }
  },
};
</script>

<style>
.container {
  border: 2px solid black;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.cuerpo {
  display: flex;
  justify-content: space-around;
  width: 50%;
}
.valores {
  display: flex;
  justify-content: space-around;
  width: 25%;
}
</style>
