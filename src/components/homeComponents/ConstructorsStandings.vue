<template>
    <div class="constructors-standings">
        <h3 class="constructors-standings-title">{{this.season}} World Constructors Championship</h3>
        <table class="standings-table" id="constructors-table">
                <tr>
                    <th>Position</th>
                    <th>Constructor</th>
                    <th>Points</th>
                </tr>
                <tr v-for="constructor in this.constructorsStandings" v-bind:key="constructor.name">
                    <td>{{constructor.position}}</td>
                    <td>{{constructor.name}}</td>
                    <td>{{constructor.points}}</td>
                </tr>  
            </table>
    </div>
</template>

<script>
export default {
    props:[
        'season',
        'constructorsStandings'
    ]
}
</script>

<style>
    .constructors-standings{
        float: left;
        width: calc(100% - 560px);
        border-top: 10px solid var(--main-colour);
        border-right: 10px solid var(--main-colour);
        border-bottom: 10px solid var(--main-colour);
        border-bottom-right-radius: 20px;
        border-top-right-radius: 20px;
        margin-top: 30px;
        margin-left: 40px;
    }

    .constructors-standings-title{
        margin: 0px;
        background-color: white;
        width: 435px;
        margin-top: -15px;
        margin-bottom: 10px;
    }

    .standings-table{
        border-collapse: collapse; 
        height: calc(100% - 80px);
        width: 100%;
        margin-bottom: 10px;
    }

    .standings-table tr:nth-child(even){
        background-color: rgb(223, 223, 223);
    }

    .standings-table th{
        padding-bottom: 15px;
        font-size: 18px;
        font-weight: 500;
    }

    .standings-table td{
        font-family: 'Titillium Web', sans-serif;
        padding-top: 7px;
        padding-bottom: 7px;
        font-size: 16px;
        text-align: center;
    }

</style>