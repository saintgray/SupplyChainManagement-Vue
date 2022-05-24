<template>
    <div id='ntc_regForm' @focusout='warn'>
        <input type='text' class='form-control' placeholder='제목' v-model='data.title'>
        <QuillEditor :options='options' v-model='data.content' ref='editor'> </QuillEditor>
        <div class='mt-2 text-right'>
            <button class="mx-2 btn btn-primary" @click='noticeRegister'>등록</button>
            <button class="mx-2 btn btn-light" @click='checkModalClose'>취소</button>
        </div>
        
    </div>
</template>



<script>
    import { QuillEditor } from '@vueup/vue-quill';
    import {onBeforeModalClose, closeModal} from "jenesius-vue-modal"
    import '@vueup/vue-quill/dist/vue-quill.snow.css';
    export default{
        setup(){
            onBeforeModalClose(function(){
                return this.isRegistered;
            })
        },
        data:function(){
            return{
                props:{'modal-id':String,'class':String},
                // editor init options    
                options:{
                    // debug:'info',
                    modules:{
                        toolbar:[
                            ['bold', 'italic', 'underline', 'strike'],
                            ['blockquote', 'code-block'],

                            [{ 'header': 1 }, { 'header': 2 }],
                            [{ 'list': 'ordered'}, { 'list': 'bullet' }],
                            [{ 'script': 'sub'}, { 'script': 'super' }],
                            [{ 'indent': '-1'}, { 'indent': '+1' }],
                            [{ 'direction': 'rtl' }],

                            [{ 'size': ['small', false, 'large', 'huge'] }],
                            [{ 'header': [1, 2, 3, 4, 5, 6, false] }],

                            [{ 'color': [] }, { 'background': [] }],
                            [{ 'font': [] }],
                            [{ 'align': [] }],
                            [ 'link', 'image', 'video', 'formula' ],
                            
                            ['clean']     
                        ],
                    },
                    placeholder:'내용을 입력하세요',
                    theme:'snow'
                },
                data:{
                    title:'',
                    content:''
                },
                isRegistered:false,
            }
        },
        components:{QuillEditor:QuillEditor},
        methods:{
            noticeRegister:function(){
                
                let vm= this;
                this.axios
                    .post('/scm/vue/notice',vm.data,{headers:{'Content-Type':'application/json'}})
                    .then((resp)=>{
                        console.log(resp);
                        let status = resp.state;
                        if(status==201){
                            alert('정상적으로 등록되었습니다');
                            this.isRegistered=true;
                            closeModal(vm);
                        }
                    })
                    .catch((err)=>{
                        let resp=err.response
                        console.log(resp);
                        if(resp.status>=400){
                            alert('오류가 발생했습니다. 잠시 후 다시 시도해주세요');
                        }
                    })
            },
            checkModalClose:function(){
                this.isRegistered=confirm('작성을 취소하시겠습니까?');
                closeModal(this);
            }
        }
    }


</script>


<style scoped>
    
    #ntc_regForm >>> .ql-editor{
        min-height: 500px;
        max-height: 500px;
        overflow-y: scroll;
    }
    
    
    #ntc_regForm{
        padding:30px;
        background-color: #fff;
        border: 2px solid rgb(59,59,59);
        border-radius: 20px;
    }
</style>