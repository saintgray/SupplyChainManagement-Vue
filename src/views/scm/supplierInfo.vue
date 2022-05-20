<template>

<p class="Location">
    <a href="../dashboard/dashboard.do" class="btn_set home">메인으로</a>    
    <span class="btn_nav bold">기준정보</span> 
    <span class="btn_nav bold">납품업체 관리</span> 
    <a @click.prevent='$router.go(0)' class="btn_set refresh">새로고침</a>
</p>
<!-- SearchArea -->
<div id="searchArea">
    <!-- <table style="margin-top: 10px" width="100%" cellpadding="5" cellsapcing="0" border="1">
        <tr style="border: 0px; border-color: blue">
            <td width="80" height="25" style="font-size: 120%;">&nbsp;&nbsp;</td>
            <td width="50" height="25" style="font-size: 100%; text-align:left; padding-right:25px;"> -->

    <div class="d-flex justify-content-around">
        <select class="form-control h-auto" v-model="param.searchType">
            <option value="all">전체</option>
            <option value="comp_nm" >회사명</option>
            <option value="sales_nm" >제품명</option>
        </select>
        <input type="text" class="form-control" style="width: 450px;" v-model="param.keyword"> 
        <button type="button" class="btn btn-primary" id="searchBtn">검색</button>
    </div>
    
                
            <!-- </td> 
        </tr>
    </table> -->
</div>
<!-- SUPPLY LIST WRAP -->
<div id="supplyListWrap">

    <PageNavi class="mt-4 justify-content-center"
                :v-model="param.selectPage"
                :page-count="param.totalPage"
                :page-range=5
                :margin-pages=0
                :click-handler="getSuppliers"
                :prev-text="'이전'"
                :next-text="'다음'"
                :prev-class="'prev'"
                :container-class="'pagination'"
                :page-class="'page-item'">
    </PageNavi>    

    
</div>

<!-- IMP SALES LIST WRAP -->
<div id="impSalesWrap"></div>



</template>




<script>
    import PageNavi from "vuejs-paginate-next";

    export default{
        data:function(){
            return{
                param:{
                    selectPage:1,
                    rowsPerPage:5,
                    searchType:'all',
                    keyword:'',
                    totalCount:0,
                    totalPage:1,
                    suppliers:[],
                }
            }
        },
        components:{PageNavi:PageNavi},
        methods:{
            getSuppliers:function(selectPage){
                this.param.selectPage=selectPage || 1;
                this.axios
                    .get('/scm/vue/suppliers')
                    .then((resp)=>{
                        let data = resp.data;
                        console.log(data);
                    })
                    .catch((err)=>{
                        console.log(err.response);
                    })

            }
        }
        
        
        
    }

</script>

<style>
#searchArea{
    margin-top: 35px;
    padding: 50px 0;
    border: 2px solid rgb(190,190,190);
}

</style>