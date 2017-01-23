<template lang="html">
  <div class="container">
    <form class = "panel panel-default">
    <div class="panel-heading">Import/export bag:</div>
    <div class="settings toolbox panel-body">
        <button @click="exportBagJSON(0);" class="btn btn-primary">Export bag as JSON</button> &nbsp;
        <button @click="exportBagJSON(1);" class="btn btn-primary">Export compressed bag</button> &nbsp; | &nbsp;
        <button @click="importBagJSON(0);" class="btn btn-primary">Import bag from JSON</button> &nbsp;
        <button @click="importBagJSON(1);" class="btn btn-primary">Import compressed bag</button>
        <br/><br/>
        <textarea style="width:100%;margin-right:8px;" id="bag-raw-data" rows="10" class="form-text"></textarea>
        <br/>
        <span id="debug-area"></span>
    </div>
  </form>
</div>
</template>

<script>
export default {
  methods: {
    exportBagJSON(){

    },
    importBagJSON(){

    }
  }
}
</script>

<style lang="css">
</style>
