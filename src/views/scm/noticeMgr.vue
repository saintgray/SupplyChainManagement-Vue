<template>

    <p class="Location">
        <a @click.prevent='$router.push({path:"/dashboard/home"})' class="btn_set home">메인으로</a>    
        <span class="btn_nav bold">기준정보</span> 
        <span class="btn_nav bold">공지사항</span> 
        <a @click.prevent='$router.go(0)' class="btn_set refresh">새로고침</a>
    </p>    
    <div id="searchArea">
        <div class="d-flex justify-content-around">
            <select class="form-control h-auto" v-model="param.oname" style='width:70px;'>
                <option value="all">전체</option>
                <option value="title" >제목</option>
                <option value="content" >내용</option>
            </select>

            <DatePicker id="datePick" v-model='dateRange' 
                        :range='true' 
                        :multiCalendars='true' 
                        :enableTimePicker='false' 
                        :maxDate='today'
                        :format='"yyyy-MM-dd"'>
            </DatePicker>   

            <input type="text" class="form-control" style="width:50%;" v-model="param.sname"  @keydown.enter="getNotices(param.currentPage)">
            <button type="button" class="btn btn-primary" @click='getNotices(param.currentPage)'>검색</button>
        </div>
    </div>

    <div id="notices">
        <List :notices='param.notices' class="my-5">

        </List>
        <PageNavi class="mt-4 justify-content-center"
                :v-model="param.selectPage"
                :page-count="param.totalPage"
                :page-range=5
                :margin-pages=0
                :click-handler="getNotices"
                :prev-text="'이전'"
                :next-text="'다음'"
                :prev-class="'prev'"
                :container-class="'pagination'"
                :page-class="'page-item'">
        </PageNavi>
    </div>

        
    
    
</template>


<script>
    import Paginate from "vuejs-paginate-next";
    import Datepicker from '@vuepic/vue-datepicker';
    import '@vuepic/vue-datepicker/dist/main.css'
    import NoticeList from "@/components/common/NoticeList.vue";

    
    
    // offset : 몇 ms 차이가 나는지를 반환
    const offset=new Date().getTimezoneOffset()*60*1000;
    const today= new Date(Date.now()-offset);

    export default{
        data:function(){
            return{
                today:'',
                dateRange:[],
                param:{
                    currentPage:1,
                    pageSize:5,
                    total:0,
                    totalPage:1,
                    sname:'',
                    from_date:'',
                    to_date:'',
                    oname:'all',
                    notices:[]
                }
            }
        },
        components:{PageNavi : Paginate, DatePicker : Datepicker, List : NoticeList},
        watch:{
            dateRange:function(changed){
                // console.log(changed);
                // console.log(new Date(changed[0]));
                // console.log(new Date(changed[1]));
                this.param.from_date=new Date(changed[0]).toISOString().substr(0,10);
                this.param.to_date=new Date(changed[1]).toISOString().substr(0,10);
                console.log('finished setting from_date and to_date')
            }
        },
        created:function(){
            this.today=today;

            let initDateRangeArr=[];
            initDateRangeArr.push(new Date(today.getFullYear(), today.getMonth()-2, today.getDate()));
            initDateRangeArr.push(today);
            this.dateRange=initDateRangeArr;

            console.log('processing after finised setting from_date and to_date');

            // set refresh event
            this.emitter.on('refreshNotice',()=>{
                this.getNotices(this.param.currentPage);
            });
            
            this.getNotices();
        },
        methods:{
            getNotices:function(selectPage){
                let vm=this;
                this.param.currentPage=selectPage || 1;
                this.axios
                    .get('/scm/vue/notices?'+new URLSearchParams(vm.param).toString())
                    .then((resp)=>{
                        console.log(resp);
                        console.log(vm.param);
                        this.param=resp.data;
                        console.log(vm.param);
                    })
                    .catch((err)=>{
                        console.log(err.response);
                    })
            },
        }


    }
</script>

<style>

    #searchArea{
		margin-top: 35px;
	    padding: 50px 0;
	    border: 2px solid rgb(190,190,190);
	}
    .dp__input_icon_pad{
        padding-left: 40px!important;
        width:250px;
    }
</style>