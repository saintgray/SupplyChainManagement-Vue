<template>
    <table class="col">
        <caption>caption</caption>
        <colgroup>
            <col width="50px">
            <col width="200px">
            <col width="60px">
            <col width="60px">										
        </colgroup>
        <thead>
            <tr>
                <th scope="col">공지 번호</th>
                <th scope="col">공지 제목</th>
                <th scope="col">공지 날짜</th>											
                <th scope="col">작성자</th>
            </tr>
        </thead>
        <tbody>
            <template v-if='notices == null'>
                <tr>
                    <td colspan="4">데이터가 존재하지 않습니다.</td>
                </tr>
            </template>
            <template v-else>
                <tr v-for='item in notices' :key='item.ntc_no' @click='getNotice(item.ntc_no)'>
                    <td>{{item.ntc_no}}</td>
                    <td>{{item.ntc_title}}</td>
                    <td>{{item.ntc_regdate}}</td>
                    <td>{{item.loginID}}</td>
                </tr>
            </template>
        </tbody>
    </table>    
</template>

<script>
    import { openModal } from "jenesius-vue-modal";
    import NoticeDetail from "@/components/common/NoticeDetail.vue";
    export default{
        props:{'notices':Array},
        methods:{
            getNotice:function(idx){
                console.log(idx);
                let vm=this;
                this.axios
                    .get('/scm/vue/notice/'+idx)
                    .then((resp)=>{
                        if(resp.status==204){
                            alert('삭제되었거나 없는 게시물입니다');
                            vm.emitter.emit('refreshList',null);
                        }else{
                            openModal(NoticeDetail,{detail : resp.data})
                        }
                        console.log(resp);
                        

                    })
            }
        },
        
    }
</script>