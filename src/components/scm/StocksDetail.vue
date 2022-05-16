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
                    <tr>                        
                        <td  v-for="(value,name) in detail" :key="name">{{value}}</td>                        
                    </tr>
                </tbody>
            </table>
        </div>
    </td>


</template>


<script>
    export default{
        props:{detail:Object},
        data:function(){
            return{
                stockDetail:{}
            }
        },
        methods:{
            
        }
    }


</script>