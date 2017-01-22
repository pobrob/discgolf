<template lang="html">
  <div>
    <b>Import/export bag:</b><br/>
    <div class="settings toolbox">
        <button @click="exportBagJSON(0);">Export bag as JSON</button> &nbsp;
        <button @click="exportBagJSON(1);">Export compressed bag</button> &nbsp; | &nbsp;
        <button @click="importBagJSON(0);">Import bag from JSON</button> &nbsp;
        <button @click="importBagJSON(1);">Import compressed bag</button>
        <br/><br/>
        <textarea style="width:100%;margin-right:8px;" id="bag-raw-data" rows="10"></textarea>
        <br/>
        <span id="debug-area"></span>
    </div>
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
