<template>
    <div class="row ">
        <!-- <div class="col-md-3 sidebar"> -->
               
            <ul class="navbar-nav">
                <li class="nav-item">
                    <router-link  
                    class="nav-link text-white" 
                    :to="{'name':'blog', params:{allData:propsData}}" >
                    {{ propsData.title }}
                    </router-link>
                </li>                    
            </ul>
            
        <!-- </div> -->
    </div>
</template>

<script>
export default {
    props:['propsData']
}
</script>
