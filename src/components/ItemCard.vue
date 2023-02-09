<!-- import AwesomeVueStarRating from 'awesome-vue-star-rating' -->

<script>
import StarRating from 'vue-star-rating'
export default{
    name:'ItemCard',
    props:{
        id:Number,
        name:String,
        discount:Number,
        MRP:Number,
        image:undefined,
        rat:Number,
        discountedPrice:Number
    },
    components:{
        StarRating
    }
}
</script>

<template class="main">
    <div class="card-container">
        <div class="card-body">
            
            <img class="card-photo" :src="image" alt="Shoes Image">
            <p class="name">{{ name }}</p>
            <div class="comb">
                <s class="op">{{ MRP }}</s>
                <span style="margin-left:2px" class="SP">{{ discountedPrice }} </span>   
                <span style="margin-left:5px" class="discount" v-if="discount">  {{ discount }}% Off</span>
            </div>
            <div id="rating">
                <StarRating :rating="rat" v-bind:increment="0.5"
                v-bind:max-rating="5"
                v-bind:star-size="20"
                :show-rating="false"
                :read-only="true"	/>
            </div>
            
        </div>
    </div>
</template>

<style>
*{
    margin: 0px;
    padding: 0px;

}
.discount{
    padding: 5px;
    border: 1px solid grey;
    border-radius: 10px;
    font-size: 12px;
    background-image:none;
}
.op{
    font-weight: 100;
    font-size: 12px;
    background-image: none;
}
.SP{
    font-weight: 900;
    font-size: 18px;
    background-image: none;
}
.card-container{
    height: 400px;
    width: 400px;
    display: flex;
    flex-flow: column;
    justify-content: space-between;
    border-radius: 20px;
    float: left;
    background-color: white;
    align-items: center;
    margin-left: 60px;
    margin-bottom: 30px;
}
.card-photo{
    width: 60%;
    height: 60%;
    background-color: white;
    padding: 10px;
}
.card-body{
    height: 500px;
    width: 65%;
    display: flex;
    flex-direction: column;
    justify-content: space-evenly;
    align-items: center;

}
.comb{
    background-image: none;
}
.name{
    background-image: none;
    text-align: center;
}

#rating{
    background-image: none;
    background-color: white;
    margin: 5%;
    background-size: 1000cm;
}
.main
{
    background-image: none;
}

</style>