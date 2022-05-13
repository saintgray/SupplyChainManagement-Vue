<template>
    <form id="myForm" action=""  method="">

        <p class="Location">
            <a href="../dashboard/dashboard.do" class="btn_set home">메인으로</a> <span
                class="btn_nav bold">Sample</span> <span class="btn_nav bold">창고별 재고 현황</span> 
                <a href="../system/comnCodMgr.do" class="btn_set refresh">새로고침</a>
        </p>

        <p class="conTitle">
            <span>창고별 재고 현황</span> 
            <span class="fr">
                <select v-model="param.searchgrouptype">
                        <option value="" >전체</option>
                        <option value="0" >창고 명</option>
                        <option value="1" >제품 명</option>
                </select> 
                <input type="text" v-model="param.searchtext">
                <a class="btnType blue" id="btnSearchGrpcod" name="btn" style="cursor: pointer;"><span onclick="search()" >검  색</span></a>
                <a href= "" class="btnType blue" id="btnSearchGrpcod" name="btn" style="cursor: pointer;"><span >초기화</span></a>
                
            </span>

        </p>
        
        <div class="divComGrpCodList">
            <table class="col">
                <thead>
                    <tr>
                        <th scope="col">창고 코드</th>
                        <th scope="col">창고명</th>
                        <th scope="col">총 재고량</th>
                        <th scope="col">지역</th>
                        <th scope="col">주소</th>
                    </tr>
                </thead>
                <tbody v-if="total>0">
                    <tr v-for="item in inventories" :key="item.wh_id" @click="inventoryDetail(item.wh_id)">
                        <td>{{item.wh_id}}</td>
                        <td>{{item.wh_nm}}</td>
                        <td>{{item.st_cnt}}</td>
                        <td>{{item.wh_loc}}</td>
                        <td>{{item.addr}}</td>
                    </tr>
                    <template v-if="inventorySelected==item.wh_id">
                        <tr>
                            <StocksDetail>
                            </StocksDetail>
                        </tr>
                    </template>
                </tbody>
            </table>
        </div>
        <Pagination class="mt-4 justify-content-center"
                    :v-model="param.currentPage"
                    :page-count="param.totalPage"
                    :page-range=5
                    :margin-pages=0
                    :click-handler="clickPageCallBack"
                    :prev-text="'이전'"
                    :next-text="'다음'"
                    :prev-class="'prev'"
                    :container-class="'pagination'"
                    :page-class="'page-item'">
        </Pagination>

    </form>
</template>

<script>
    import Paginate from "vuejs-paginate-next"; 
    import StockDetail from "@/components/scm/StocksDetail.vue"
    export default{
        data:function(){
            return{
                param:{
                    searchgrouptype:'',
                    searchtext:'',
                    currentPage:1,
                    pageSize:5,
                    totalPage:1,
                    total:0,
                },
                inventories:[],
                inventorySelected:0
            }
        },
        components:{Pagination:Paginate, StocksDetail:StockDetail},
        methods:{
            getInventories:function(selectPage){
                this.param.currentPage=selectPage || 1;
                let vm= this;
                this.axios
                    .post('/scm/vue/inventories')
                    .then((resp)=>{
                        let data=resp.data;
                        this.total=data.total;
                        vm.param.currentPage=data.currentPage;
                        vm.param.totalPage=data.totalPage;
                        vm.inventories=data.inventories;
                    })
                    .catch((err)=>{
                        console.log(err);
                    })

            },
            inventoryDetail:function(idx){
                this.axios
                    .post('/scm/vue/stocks/'+idx)
                    .then((resp)=>{
                        let data=resp.data;
                        console.log(data);
                    })
            }
        },
    }

</script>


<style>
    #detailArea{
        margin:20px auto;
        padding:0;
        animation:showUp .7s ease-out;
    }
    #whlist>tr{
        cursor:pointer;
    }
    #whlist>tr.blur{
        animation : blur normal 2s infinite ease-in-out;
    }
    @keyframes blur{
        0%{
            background-color:white;
        }
        50%{
            background-color:#fafa5f;
        }
        100%{
            background-color:white;
        }
    }
    @keyframes showUp{
        0%{
            opacity:0;
            transform:translateX(-100%);
        }
        100%{
            opacity:1;
            transform:translateX(0);
        }
    }
</style>