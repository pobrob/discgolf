<template lang="html">
  <div class="container">
    <div class="panel panel-default">
      <div class="panel-heading">
        <slot></slot>
      </div>
      <div class="panel-body">
        <ul class="list-group">
            <li  v-for="disc in discs" class="list-group-item">{{disc.disc}}</li>
          </ul>
      </div>
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
