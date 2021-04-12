<template>
    <div class="header">
        <img class="logo" src="../assets/f1_logo.svg">
        <h2 class="header-title">Compare!</h2>
        <div class="header-split"></div>
        <div class="menu">
            <div @click="menuPress(menuItem.label)" v-for="menuItem in menuItems" v-bind:key="menuItem.id" class="menu-item">
                {{menuItem.label}}
            </div>
        </div>
        <div class="season-select">
            <select @change="seasonChange($event)" class="season-dropdown" id="season-select" v-model="season">
                <option v-bind:value="season.season" v-for="season in seasons" v-bind:key="season.id">{{season.season}}</option>
            </select>
            <label class="season-label" for="season-select">Season:</label>
        </div>
    </div>
</template>

<script>
    export default{
        methods: {
            menuPress(item){
                this.$emit("menu-press", item);
            },
            seasonChange(event){
                this.$emit("season-change", event.target.value);
            }
        },
        mounted(){
            //Setup dropdown in reverse year order to 2008.
            var currentYear = new Date().getFullYear();
            var countdownYear = currentYear;
            while(countdownYear != 1949){
                this.seasons.push({id: countdownYear, season: countdownYear});
                countdownYear--;
            }
            this.$emit("season-change", currentYear);
        },
        data() {
            return {
                menuItems: [
                    {id:"home", label:"Home"},
                    {id:"drivers", label:"Drivers"},
                    {id:"teams", label:"Teams"},
                    {id: "tracks", label:"Tracks"}
                ],
                seasons: [],
                season: new Date().getFullYear()
            }
        }
    };
</script>

<style>
    .header{
        margin: 0;
        background-color: var(--main-colour);
        height: 80px;
        color: white;
    }

    .logo{
        float: left;
        width: 150px;
        margin-top: 21.25px;
        margin-left: 10px;
    }

    .header-title{
        float: left;
        font-family: 'F1Bold', Fallback, sans-serif;
        margin: 0;
        height: calc(60px - 34px);
        padding-top: 34px;
    }

    .header-split{
        float: left;
        background-color: white;
        width: 2px;
        height: calc(100% - 18px);
        margin: 0;
        margin-top: 9px;
        margin-left: 20px;
    }
    
    .menu{
        float: left;
        margin-left: 20px;
    }

    .menu-item{
        float: left;
        margin: 0;
        margin-top: 30px;
        margin-left: 20px;
        font-size: 18px;
    }

    .menu-item:hover{
        text-decoration: underline;
    }

    .season-select{
        float: right;
        margin: 0;
        margin-top: 25px;
        margin-right: 20px;
        font-size: 18px;
    }

    .season-label{
        float: right;
        margin-top: 4px;
        margin-right: 8px;
    }

    .season-dropdown{
        float: right;
        border: 2px solid white;
        background-color: white;
        border-radius: 5px;
        height: 29px;
        font-size: 19px;
        font-family: 'F1';
    }

</style>