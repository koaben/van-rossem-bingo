<template>
  <div class="bingo-card__item" @click="toggleActive">
    {{ msg }}
    <span class="bingo-card__checkbox"></span>
  </div>
</template>

<script>
export default {
  name: 'BingoItem',
  props: {
    msg: String
  },
  methods: {
    toggleActive(event) {
       event.target.classList.toggle('active')
    }
  }
}
</script>


