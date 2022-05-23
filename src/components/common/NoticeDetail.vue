<template>

    <div id='noticeInfoWrap'>
        <div style="font-size:35px">
            <h1>공지사항입니다</h1>
            <div style="font-size:15px">
                <p>{{detail.ntc_no}}</p>
                <p>{{detail.ntc_title}}</p>
                <p>{{detail.ntc_content}}</p>
            </div>
        </div>
    </div>
    
    
    
    

</template>


<script>

    export default{
        props:{'detail':Object ,'class':String, 'modal-id':String}
    }
</script>

<style scoped>

    #noticeInfoWrap{
        background-color: #fff;
        padding: 3rem;
        border-radius: 10px;
        border: 2px solid rgb(59,59,59);        
    }
</style>