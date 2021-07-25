<template>
  <div>
      <b-nav tabs>
      <b-nav-item disabled>Trivia Quiz App</b-nav-item>
      <b-nav-item disabled>Question : {{ correctNum }} / {{ totalNum }}</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props : [
    'correctNum',
    'totalNum'
  ]

}
</script>

<style>

</style>