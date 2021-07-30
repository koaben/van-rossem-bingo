<template>
  <main>
    <div class="bingo-card__item" style="height:100%; width: 100%" @click="toggleActive" v-if="!editMode">
      {{ quote }}
      <span class="bingo-card__checkbox"></span>
    </div>

    <div class="bingo-card__item" style="height:100%; width: 100%" @click="toggleActive" v-if="editMode">
      <textarea v-model="quote" style="width: 98%;" rows="4" @input="quoteChanged"></textarea>
    </div>
  </main>
</template>

<script>
export default {
  name: 'BingoItem',
  props: {
    msg: String,
    editMode: Boolean,
    index: Number,
  },
  data() {
    return {
      quote: this.msg,
    }
  },
  methods: {
    toggleActive(event) {
      if (!this.editMode) {
        event.target.classList.toggle('active')
      }
    },
    quoteChanged() {
      this.$emit('quote-changed', {index: this.index, quote: this.quote});
    }
  }
}
</script>


