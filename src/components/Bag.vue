<template lang="html">
  <div class="panel panel-default">
    <div class="panel-body" >
<slot></slot>
    <div  v-for="disc in discs">{{disc.disc}}</div>
  </div>
</div>
</template>

<script>

export default {
    props: ['discs']
}
</script>

<style lang="css">

</style>
