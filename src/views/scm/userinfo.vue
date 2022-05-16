<template>

    <p class="Location">
        <a href="/dashboard" class="btn_set home"></a>
        <span class="btn_nav bold">기준정보</span>
        <span class="btn_nav bold">기업고객/직원정보관리</span>
        <a href="/scm/userinfo.do" class="btn_set refresh"></a>
    </p>

    <!-- SearchArea -->
    <div class="d-flex justify-content-around" id="searchArea">
       
        <select v-model="param.searchType" class="form-control h-auto">
            <option value="all">전체</option>
            <option value="comp_nm" >회사명</option>
            <option value="emp_nm" >직원명</option>
            <option value="user_nm">담당자명</option>
            <option value="task">담당업무</option>
        </select>
        <input type="text" class="form-control" style="width:50%;" v-model="param.keyword">
        <div style="line-height:36px;">
            <input type="checkbox" v-model="param.showall">
            <span class="mx-2">삭제된 정보 표시</span>
        </div>
        <button type="button" class="btn btn-primary" id="searchBtn" @click='getUserList(param.selectPage)'>검색</button>

    </div>

    <!-- UserListArea -->
    <div id="userListArea">
        <div class="bts mt30">
            <table class="col">
                <thead>
                    <tr>
                        <th scope="col">아이디</th>
                        <th scope="col">구분명</th>
                        <th scope="col">회사명/성명</th>
                        <th scope="col">담당업무</th>
                        <th scope="col">담당자명</th>
                        <th scope="col">연락처</th>
                    </tr>
                </thead>
                <tbody>
                    <template v-if='param.totalCount>0'>
                        <tr v-for="user in userlist" :key="user.loginID" @click="getUserDetail(user.loginID)">
                            <td><span>{{user.loginID}}</span></td>
                            <td>
                                <span v-if='user.user_Type=="C"'>기업고객</span>
                                <span v-else>내부직원</span>
                            </td>
                            <td>
                                <span v-if='user.user_Type=="C"'>{{user.client}}</span>
                                <span v-else>{{user.name}}</span>
                            </td>
                            <td>
                                <span v-if='user.user_Type=="A"'>SCM 관리자</span>
                                <span v-if='user.user_Type=="B"'>배송담당자</span>
                                <span v-if='user.user_Type=="D"'>구매담당자</span>
                                <span v-if='user.user_Type=="E"'>임원</span>
                            </td>
                            <td>
                                <span v-if='user.user_Type=="C"'>{{user.name}}</span>
                            </td>
                            <td>
                                <span>{{user.phone}}</span>
                            </td>
                        </tr>
                    </template>
                    <template v-else>
                        <tr><td colspan="6">일치하는 검색 결과가 없습니다</td></tr>
                    </template>
                </tbody>
            </table>
	    </div>
	
        <!-- Register And Delete Btn Area -->
        <div class="bts userInfoBtnArea" style="margin: 10px 0">
            <div class="text-right">
                <button type="button" class="btn btn-primary mx-2" @click="openRegisterForm">신규등록</button>
                <button type="button" class="btn btn-danger" id="delUserBtn">삭제</button>
            </div>
        </div>

        <PageNavi class="mt-4 justify-content-center"
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
        </PageNavi>    
    </div>



    <!-- User Form Area -->
    <form id="userRegForm">
        <UserRegForm v-if='buttonAction.actionType=="NEW"'></UserRegForm>
        <UserDetail v-if='buttonAction.actionType=="INFO"' :userInfo="userInfo"></UserDetail>
    </form>



    
</template>




<script>
import PageNavi from "vuejs-paginate-next";
import UserRegForm from "@/components/scm/UserRegisterForm.vue";
import UserDetail from "@/components/scm/UserDetail.vue";
export default{

    data:function(){
        return{
            param:{
                selectPage:1,
				rowsPerPage:5,
				totalCount:0,
                totalPage:1,
                keyword:null,
				showall:'',
				searchType:'all'
            },
            userlist:[],
            buttonAction:{
                // form or manage
                actionType:'',
                // form : NEW or INFO
                // manage : CREATE , DELETE , EDIT
                action:'',
                // user PK for manage(DELETE, EDIT)
                id:''
            },
            userInfo:{}
        }
    },
    components:{PageNavi:PageNavi, UserRegForm, UserDetail},
    created:function(){
        this.emitter.on('close',()=>{
            this.buttonAction.actionType='';
        })
        this.getUserList();
    },
    methods:{
        getUserList:function(selectPage){
            let vm=this;
            selectPage = selectPage || 1;
            vm.param.selectPage=selectPage;
            console.log(vm.param);
            this.axios
                .post('/scm/vue/users',new URLSearchParams(vm.param))
                .then((resp)=>{
                    let page=resp.data.page;
                    console.log(page);
                    vm.param.selectPage=page.selectPage;
                    vm.param.totalCount=page.totalCount;
                    vm.param.totalPage=Number(page.totalPage);
                    vm.userlist=page.userlist
                })
                .catch((err)=>{
                    console.log(err);
                })
        },
        clickPageCallBack:function(page){
            this.param.selectPage=page;
            this.getUserList(page);
        },
        getUserDetail:function(id){
            
            let vm=this;
            

            this.axios
                .post('/scm/vue/user/'+id)
                .then((resp)=>{
                    let data=resp.data;
                    console.log(data);
                    vm.userInfo=data.info;
                    vm.buttonAction.actionType="INFO";
                })
                .catch((err)=>{
                    // error 의 응답 객체는 err.response 로 받을 수 있다.
                    let status=err.response.status;
                    if(status==903){
                        alert('삭제된 회원이거나 없는 회원입니다');
                        vm.getUserList(vm.param.selectPage);
                    }
                })
            
            

        },
        openRegisterForm:function(){
            if(this.buttonAction.actionType!='NEW'){
                this.buttonAction.actionType="NEW"
            }
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
	
	#userListArea table{
		margin:0 auto;
	}
	#userinfoarea{
		padding: 10px;
		border: 2px solid rgb(190,190,190);
		margin-bottom: 50px;
		
	}
	#userinfoarea table{
		border-collapse: separate;
		border-spacing: 10px 10px;
		margin: 0 auto;
		
	}
	.userInfoBtnArea{
		margin-top: 10px;
	}
	#sb-userType{
		margin-left:0 !important;
	}
	
	#btn-close-daum{
		position:absolute;
		right:0;
	    bottom: 0;
	    z-index: 11;
	    cursor: pointer;
		
	}

</style>