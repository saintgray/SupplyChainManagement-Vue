<template>

    <div id='noticeInfoWrap'>
        <div id='ntc_wrap'>
            <div class='ntc_inner_wrap'>
                <h1 class='n_title'>
                    <strong>
                        {{detail.ntc_title}}
                    </strong>
                </h1>
                <div class='n_content'>
                    <span>{{detail.ntc_regdate}}</span>
                    {{detail.ntc_content}}
                </div>
            </div>
        </div>
    </div>
    
</template>


<script>
    //import { BDropdown } from 'bootstrap-vue'
   // import { defineAsyncComponent } from '@vue/runtime-core'
    export default{
        props:{'detail':Object ,'class':String, 'modal-id':String},
       // components:{'b-dropdown':defineAsyncComponent(()=>import(BDropdown))}
    }
</script>

<style scoped>
    #noticeInfoWrap{
        background-color: #fff;
        padding: 3rem;
        border-radius: 10px;
        border: 2px solid rgb(59,59,59);
        min-width: 50%;
        min-height: 85%;
    }
    #ntc_wrap{
        padding:15px;
    }
    .n_title{
        background-color:rgb(235,235,235);
        text-align: center;
        padding: 5px;
    }
    .ntc_inner_wrap{
        border: 1px solid rgb(59,59,59);
    }
    .n_content{
        position: relative;
        border-top: 1px solid rgb(59,59,59);
        padding: 20px 10px;
        min-height: 500px;
    }
    .n_content:first-child{
        position: absolute;
        top: 3px;
        right: 5px;
    }
</style>