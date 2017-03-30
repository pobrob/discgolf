<style lang="css">



</style>

<template lang="html">

<div class='container'>
    <form class="panel panel-default">
        <div class="panel-heading">Import/export bag:</div>
        <div class="settings toolbox panel-body">
            <row>
                <button @click="exportBagJSON(0);" class="btn btn-primary" style="float:left">Export bag as JSON</button>
                <button @click="exportBagJSON(1);" class="btn btn-primary" style="float:left">Export compressed bag</button>
            </row>

            <row>
                <button @click="importBagJSON(0);" class="btn btn-primary" style="float:left">Import bag from JSON</button> &nbsp;
                <button @click="importBagJSON(1);" class="btn btn-primary" style="float:left">Import compressed bag</button>
            </row>
            <br/>
            <br/>
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
        exportBagJSON() {

            },
            importBagJSON() {

            }
    }
}

</script>
