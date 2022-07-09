<style scoped>

  .modal__window {
    position: fixed;
    top: 0;
    left: 0;
    bottom: 0;
    width: 100vw;
    height: 1;
    line-height: 100vh;
    background-color: #999999;
    opacity: 0.9;
  }
  .kanji {
    display: block;
    width: 100%;
    font-size: 80px;
    text-align: center;
  }
</style>

<template>
  <div class="modal__window" @click="$emit('close')">
    <span class="kanji">{{ kanji }}</span>
  </div>
</template>

<script>
  export default {
    name: 'Modal',
    props: ['kanji'],
  }
</script>