<template>

    <td colspan="5">
        <div id="detailArea">
            <table class="col">
                <thead>
                    <tr>
                        <th scope="col">분류</th>
                        <th scope="col">상품명</th>
                        <th scope="col">재고량</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(item,index) in detail" :key="index">
                        <td>{{item.sales_type}}</td>
                        <td>{{item.sales_nm}}</td>
                        <td>{{item.st_cnt}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </td>


</template>


<script>
    export default{
        props:{detail:Array},
        data:function(){
            return{
                stockDetail:{}
            }
        },
        methods:{
            
        }
    }


</script>