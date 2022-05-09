<template>
    <div id="d-api-zone">
        <VueDaumPostcode @complete='sendAPIdata'/>
    </div>
</template>

<script>

// import { defineAsyncComponent } from '@vue/runtime-core';
export default {
    data: function() {
        return {
            test:0
        }
    },   
    methods: {
        sendAPIdata: function (resp) {
            
            this.emitter.emit("daumZipResult", resp);
        },
    },
};
</script>

<style>
    #d-api-zone{
        position: fixed;
        left: 2px;
        top: 2px;
        border: 2px solid black;
        max-height: 80%;
        overflow-y: scroll;
    }

</style>