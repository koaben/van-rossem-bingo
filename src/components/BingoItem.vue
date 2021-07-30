<template>
  <div class="bingo-card__item" @click="toggleActive">

    <textarea v-if="editMode" v-model="quote" style="width: 98%;" rows="4" @input="quoteChanged"></textarea>
    <span v-else>
      {{ quote }}
    </span>
    <span class="bingo-card__checkbox"></span>
  </div>
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
      if(!this.editMode) {
        event.target.classList.toggle('active')
      }
    },
    quoteChanged() {
      this.$emit('quote-changed', {index: this.index, quote: this.quote});
    }
  }
}
</script>


