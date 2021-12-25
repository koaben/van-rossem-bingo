<template>
  <div id="app">
    <div style="position: relative; display: flex; justify-content: center; margin-top: 50px">
      <img
          src="https://i.redd.it/4qn19gaf72m21.png"
          class="no-print"
          style="height: 100px; width: 100px; position: absolute; border: 2px solid #fff; border-radius: 5px;">
    </div>
    <div class="main-content">
      <div class="title">
        <span>
        Maarten van Rossem Bingo
        </span>
      </div>
      <div class="sub-title no-print">
        <span>
          Klik om de vakjes op ze af te strepen of <a href="#print" @click="print">download de printversie</a>.
        </span>
      </div>
      <div class="bingo-card" style="justify-content: center;">
        <BingoItem :key="item" v-for="(item, index) in items" :msg="item" :editMode="editMode" :index="index" v-on:quote-changed="quoteChanged"/>
      </div>

      <div style="display: flex; flex-direction: column; justify-content: center; align-items: center; width: 100%; color: rgb(211 243 255); font-size: 14px; margin-bottom: 15px; margin-top: 5px; ">

        <div style="display: inline-flex; align-items: center; cursor: pointer;" @click="editMode = true; editSaved = false" v-if="!editMode">
          <!-- pencil svg -->
          <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor" style="width: 15px; margin-right: 6px;">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
          </svg>
          Bingokaart bewerken
        </div>

        <div style="display: inline-flex; align-items: center; cursor: pointer; color: #5cff5c" @click="editMode = false; editSaved = true;" v-if="editMode">
          <!-- save svg -->
          <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" style="width: 15px; margin-right: 6px;">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7H5a2 2 0 00-2 2v9a2 2 0 002 2h14a2 2 0 002-2V9a2 2 0 00-2-2h-3m-1 4l-3 3m0 0l-3-3m3 3V4" />
          </svg>
          Wijzigingen opslaan en delen
        </div>


        <div style="display: inline-flex; flex-direction: column; align-items: center; cursor: pointer; background: #62de4c; padding: 5px 10px; width: 80%; border-radius: 5px; color: rgb(33 76 19); margin-top: 10px;" v-if="editSaved && showSaveMessage">
           De wijzigingen zijn opgeslagen! Je kunt je bingokaart met de volgende link delen:

          <input type="text" :value="currentUrl" style="margin-top: 10px; width: 90%">
        </div>
      </div>

    </div>

    <div style="position: relative; display: flex; justify-content: center; margin-top: 50px" class="no-print">
      <a target="_blank" href="https://podcasts.apple.com/nl/podcast/maarten-van-rossem-de-podcast/id1513807137">
        <img src="/van-rossem-bingo/apple.svg">
      </a>
      <div style="width: 10px"></div>
      <a target="_blank" href="https://open.spotify.com/show/1RGUJ8uKoJTTMOJjIHoe8n">
        <img src="/van-rossem-bingo/spotify.svg">
      </a>
    </div>

    <div style="text-align: center; margin-top: 35px; margin-bottom: 50px;" class="no-print">
      <small style="color: #555c7b">
        Copied from <a href="https://thijskuilman.github.io/van-rossem-bingo" > https://thijskuilman.github.io/van-rossem-bingo</a>

        Afbeelding is eigendom van <a href="https://www.maartendepodcast.nl/" style="color: #555c7b">maartendepodcast.nl</a>.
      </small>
    </div>
  </div>
</template>

<script>
import BingoItem from './components/BingoItem.vue'

export default {
  name: 'App',
  mounted() {
    const CSVToJSON = (data, delimiter = '|') => {
      const titles = data.slice(0, data.indexOf('\n')).split(delimiter);
      return data
          .slice(data.indexOf('\n') + 1)
          .split('\n')
          .map(v => {
            const values = v.split(delimiter);
            return titles.reduce(
                (obj, title, index) => ((obj[title] = values[index]), obj),
                {}
            );
          });
    };

    var url = new URL(window.location.href);
    if (url.searchParams.get('custom')) {
      var quotesAsString = decodeURIComponent(atob(url.searchParams.get('custom')));
      // this.items = CSVToJSON(quotesAsString)[0];
      this.items = Object.values(CSVToJSON(quotesAsString)[0]);
    }
    this.items = (this.items).sort(() => 0.5 - Math.random());
  },
  methods:
      {
        print() {
          window.print()
        },
        quoteChanged(quote) {
          this.items[quote.index] = quote.quote;
          var searchParams = new URLSearchParams(window.location.search)
          searchParams.set('custom', btoa(encodeURIComponent(Object.values(this.items).join("|"))));
          var newRelativePathQuery = window.location.pathname + '?' + searchParams.toString();
          history.pushState(null, '', newRelativePathQuery);
          this.currentUrl = window.location.href;
          this.showSaveMessage = true;
        }
  },
  data: function () {
    return {
      showSaveMessage: false,
      currentUrl: '',
      editSaved: false,
      editMode: false,
      items: [
        "In allerlei opzichten",
        "Lampje van de camera",
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

a {
  color: #fff;
}

* {
  box-sizing: border-box;
}

body {
  background: #f8f8f8;
  font-family: "Nunito", sans-serif;
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
  color: #fff;
  padding: 30px 10px;
  padding-bottom: 10px;
  padding-top: 65px;
  grid-column: span 4;
  text-align: center;
  font-size: 28px;
}

.title span {
  display: none;
}

.title span:nth-child(1) {
  display: block;
}

.sub-title {
  color: #fff;
  padding: 5px 5px;
  margin-bottom: 10px;
  grid-column: span 4;
  text-align: center;
  font-size: 12px;
}


.bingo-card {
  padding: 10px;
  display: grid;
  grid-gap: 3px;
  grid-template-rows: repeat(4, 110px);
  grid-template-columns: repeat(4, 25%);
  text-transform: uppercase;
}

.bingo-card__item {
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  text-align: center;
  justify-content: center;
  position: relative;
  cursor: pointer;
  font-size: 12px;
  line-height: 1.35;
  user-select: none;
  color: #d3f3ff;
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
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
  /* Non-prefixed version, currently
                                   supported by Chrome, Edge, Opera and Firefox */
}

.bingo-card__item.active:after {
  height: 100%;
  opacity: 0.7;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
  /* Non-prefixed version, currently
                                   supported by Chrome, Edge, Opera and Firefox */
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
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
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

.main-content {
  margin-bottom: 50px;
  border-radius: 10px;
  box-shadow: rgba(0, 0, 0, 0.4) 0px 7px 29px 0px;
  background-color: #5f6eba;
  background-image: url("https://www.transparenttextures.com/patterns/dark-dotted-2.png");
  /* This is mostly intended for prototyping; please download the pattern and re-host for production environments. Thank you! */
}

body {
  background: #121731;
  background-image: url("/van-rossem-bingo/dark-dotted-2.png");
}

html,
body {
  min-height: 100%;
}

@media print
{
  .no-print, .no-print *
  {
    display: none !important;
  }
}

</style>

