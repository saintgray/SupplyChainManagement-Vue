<template>
 
	<p class="Location">
		<a @click.prevent='$router.push({path:"/dashboard/home"})' class="btn_set home">메인으로</a>
	
		<span class="btn_nav bold">기준정보</span> 
		<span class="btn_nav bold">제품 정보 관리</span> 
	
		<a @click.prevent='$router.go(0)' class="btn_set refresh">새로고침</a>
	</p>
	<!-- SearchArea -->
	<searchArea></searchArea>

	<div id="salesListWrap">
		<salesList></salesList>
	</div>
	
	
	<salesForm></salesForm>
	

</template>

<script>
    import salesList from "@/components/scm/SalesList.vue";
    import searchArea from "@/components/scm/SearchForm.vue";
	import salesForm from "@/components/scm/SalesForm.vue";
    
    export default{
        components:{
        	salesList, searchArea, salesForm
        },
		mounted:function(){
			console.log('initiate productMng');
		}
    }
</script>



<style scoped>

	#st-btn{
		width:51px;
		height:51px;
		position: fixed;
		bottom: 30px;
	    right: 30px;
	    cursor: pointer;
	    background-image: url(/images/mainicons.png);
	    background-size: 457px 434px;
	    background-position: -148px -210px;
	    background-repeat: no-repeat;
	}
</style>