<template>
  <div id="app">
   
<div class="main-content">
  <div class="title"> 


    <span>

<!-- 
<img src="https://d3t3ozftmdmh3i.cloudfront.net/production/podcast_uploaded_nologo/5567033/5567033-1591538095363-bfa84457378f7.jpg" style="width: px;"> -->

    Van Rossem Bingo  
    </span></div>
  <div class="bingo-card">

    <BingoItem :key="item" v-for="item in items" :msg="item"/>

    </div>
  </div>
</div>
</template>

<script>
import BingoItem from './components/BingoItem.vue'

export default {
  name: 'App',
  mounted() {
this.items = (this.items).sort(() => 0.5 - Math.random());
  },
 data: function () {
    return {
      items: [
       "In allerlei opzichten",
       "Een stelletje halve garen",
       "Niet goed snik",
       "In feite",
       "Volslagen idioten",
       "Maar nu begin ik af te dwalen",
       "Betrekkelijk",
       "Dat zou verboden moeten worden",
       "Als ik het wel heb",
       "Nietwaar?",
       "Ik neem even een slokje water",
       "Lollig",
       "Stelletje halvegaren",
       "Als ik het me goed herinner",
       "Dat wil zeggen",
       "Lees eens iets"
      ]
    }
  },
  components: {
    BingoItem
  }
}
</script>

<style>
@charset "UTF-8";
* {
  box-sizing: border-box;
}

body {
  background: #f8f8f8;
  font-family: "Nunito", sans-serif;
   user-select: none; /* supported by Chrome and Opera */
   -webkit-user-select: none; /* Safari */
   -khtml-user-select: none; /* Konqueror HTML */
   -moz-user-select: none; /* Firefox */
   -ms-user-select: none; /* Internet Explorer/Edge */
}

aside.context {
  text-align: center;
  color: #333;
  line-height: 1.7;
}
aside.context a {
  text-decoration: none;
  color: #333;
  padding: 3px 0;
  border-bottom: 1px dashed;
}
aside.context a:hover {
  border-bottom: 1px solid;
}
aside.context .explanation {
  max-width: 700px;
  margin: 4em auto 0;
}

footer {
  text-align: center;
  margin: 3em auto;
  width: 100%;
}
footer a {
  text-decoration: none;
  display: inline-block;
  width: 45px;
  height: 45px;
  border-radius: 50%;
  background: transparent;
  border: 1px dashed #333;
  color: #333;
  margin: 5px;
}
footer a:hover {
  background: rgba(255, 255, 255, 0.1);
}
footer a .icons {
  margin-top: 12px;
  display: inline-block;
  font-size: 20px;
}

.main-content {
  max-width: 600px;
  width: 100%;
  margin: 4em auto 0;
  overflow: hidden;
}

.title {
  background: #fdb90b;
  color: #fff;
  padding: 30px 10px;
  grid-column: span 4;
  text-align: center;
  font: 72px/0.9 "Amatic SC", cursive;
}
.title span {
  display: none;
}
.title span:nth-child(1) {
  display: block;
}


.bingo-card {
  background: #f7d75c;
  padding: 10px;
  display: grid;
  grid-gap: 3px;
  grid-template-rows: repeat(4, 110px);
  grid-template-columns: repeat(4, 1fr);
  text-transform: uppercase;
}
.bingo-card__item {
  background: #f9f59e;
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  position: relative;
  cursor: pointer;
  font-size: 12px;
  line-height: 1.35;
  user-select: none;
}
.bingo-card__item:after {
  content: "-";
  position: absolute;
  top: -28%;
  left: -30%;
  color: #fbda7d;
  width: 100%;
  opacity: 0;
  transition: 0.1s ease;
  height: 0;
  pointer-events: none;
  font: 280px/0.5 "Caveat Brush", cursive;
  text-align: center;
  transform: rotate(-45deg);
}
.bingo-card__item.active:after {
  height: 100%;
  opacity: 0.7;
}

.bingo-card__item {
  padding: 15px;
}
.bingo-card__item.active .bingo-card__checkbox:before {
  content: "✔";
  position: absolute;
  color: black;
  left: 0;
  top: -19px;
  color: #fdb90b;
  font: 30px "Caveat Brush", cursive;
}
.bingo-card__checkbox {
  display: none;
}

.clear-button {
  margin: 2em 0 0;
  font: 700 16px "Nunito", sans-serif;
  text-transform: uppercase;
  cursor: pointer;
  display: inline-block;
  border: 2px dotted;
  color: #fdb90b;
  padding: 8px 10px;
}
.clear-button:hover {
  color: #f2ae00;
}

</style>
