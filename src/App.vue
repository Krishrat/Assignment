<template>
<div class="main">
    <div class="logodiv">
      <img class="logo" src="./assets/logo.svg" alt="Rupeek Logo">
    </div>
   
    <div class="search">
        <select name="start" id="sort" v-model="sort">
            <option disabled value="Select a Sort">Select a Sort</option>
            <option value="Sort By">Recommended</option>
            <option value="orginalPrice">Price: Low to High</option>
            <option value="discountPercentage">Discount: High to Low</option>
            <option value="rating">Rating</option>
        </select>
        <div style="background: none;" class="APIC">
            <h1 style="color:white;">API Hits: {{ apihitcount }}
            </h1>
        </div>
        <input placeholder="Search...." class="search-input" type="text" v-model="searchTerm" />
    </div>

    <br>
    <div class="loaderclass" v-if="loader">
        <img class="loader" src="./assets/loader.svg" alt="Loader">
    </div>
    <!-- Code is displaying details after serch bar is pressed or revoked -->
    <div v-for="item in filtered" v-bind:key="item.id">
        <ItemCard :id="item.id" :name="findName(item.name)" :MRP="Math.round(item.orginalPrice)" :discount="item.discountPercentage" :discountedPrice="Math.round(calculateSP(item.orginalPrice,item.discountPercentage))" :image="item.image" :rat="item.rating" />
    </div>
</div>
</template>

<script>
import axios from 'axios'
import ItemCard from './components/ItemCard.vue'

export default {
    name: 'App',
    components: {
        ItemCard
    },
    data() {
        return {
            sort: 'Sort By',
            feched: false,
            searchTerm: '',
            list: [],
            filtered: [],
            loader: true,
            apihitcount: 0,
        }
    },
    mounted() {
        axios.get('https://run.mocky.io/v3/531b65a6-0355-4892-b54e-70903a79075a')
            .then((res) => {
                this.list = [...res.data];
                console.log(typeof this.list[0].orginalPrice)
                console.log(Math.round (this.list[2].orginalPrice))
                this.filtered = [...res.data];
                this.loader = false
                if(localStorage.getItem('apihitcount'))
                {
                  this.apihitcount=localStorage.getItem('apihitcount');
                }
                this.apihitcount++;
                localStorage.setItem('apihitcount',this.apihitcount);
            })
    },
    methods: {
        calculateSP(original, discount) {
            return original - (original * discount / 100)
        },
        findName(name) {
            var n = name.indexOf('(');
            var str = name.substring(0, n);
            return str;
        }
    },
    watch: {
        searchTerm(currentValue) {
            this.filtered = this.filtered.filter((item) => item.name.includes(currentValue.toUpperCase()));
            this.fetched = true
            this.home = false
        },
        sort(currentValue) {
            if (currentValue == 'orginalPrice') {
                this.filtered = this.filtered.sort((a, b) => {
                    return this.calculateSP(a.orginalPrice, a.discountPercentage) -
                        this.calculateSP(b.orginalPrice, b.discountPercentage);
                })
            } else if (currentValue == 'discountPercentage') {
                this.filtered = this.filtered.sort((a, b) => {
                    return b.discountPercentage - a.discountPercentage;
                })
            } else if (currentValue == 'rating') {
                this.filtered = this.filtered.sort((a, b) => {
                    return b.rating - a.rating;
                })
            } else if (currentValue == 'Sort By') {
                this.filtered = [...this.list];
            }
        }
    }
}
</script>

<style>
body {
    background-image: url('./assets/bg.jpg');
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center;
    width: 100vw;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
}
.logodiv{
  height: 40%;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 10px;
  margin-top: 30px;
}
.logo {
    width: 10%;
}
.search-input {
    background-color: white;
    background-image: none;
    border-color: burlywood;
    width: 300px;
    height: 25px;
    margin: 40px;
    padding: 5px;
    justify-content: left;
}
.search {
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    width: 100vw;
    justify-content: space-around;
}
#sort {
    height: 30px;
    width: 150px;
    background-color: white;
    background-image: none;
    border-color: burlywood;
    width: 300px;
    height: 40px;
    margin: 40px;
}
.loaderclass {
    background-image: none;
}
.loader {
    width: 1000px;
    height: 500px;
}
</style>
