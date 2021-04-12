<template>
    <div class="race-breakdown">
            <h3 class="race-breakdown-title">{{this.season}}'s Races</h3>
            <div class="race-switcher">
                <div @click="raceChange('prev')" class="race-sw-left">&lt;</div>
                <div class="names">
                    <h4 class="race-name">{{this.raceBasicInfo[0].raceName}}</h4>
                    <p class="circuit-name">{{this.raceBasicInfo[0].circuitName}}</p>
                </div>
                <div @click="raceChange('next')" class="race-sw-right">&gt;</div>
            </div>
            <div class="race-timings-content">
                <div class="race-winners-container">
                    <h4 class="race-winners-title">Race Winners</h4>
                    <table class="race-winners-table">
                        <tr>
                            <td style="color: darkorange" class="sector-heading">P1</td>
                            <td>{{this.raceBasicInfo[0].results.p1.name}}</td>
                        </tr>
                        <tr>
                            <td style="color: darkgray" class="sector-heading">P2</td>
                            <td>{{this.raceBasicInfo[0].results.p2.name}}</td>
                        </tr>
                        <tr>
                            <td style="color: brown" class="sector-heading">P3</td>
                            <td>{{this.raceBasicInfo[0].results.p3.name}}</td>
                        </tr>
                    </table>
                </div>
                <div class="table-splitter"></div>
                <div class="lap-table-container">
                    <h4 class="laps-title">Fastest Lap</h4>
                    <table class="lap-table">
                        <tr>
                            <td>{{this.fastestDriver.name}}</td>
                            <td>{{this.fastestTime}}</td>
                        </tr>
                    </table>
                </div>
            </div>
        </div>
</template>

<script>
export default {
    props: [
        'season',
        'raceBasicInfo',
        'fastestDriver',
        'fastestTime'
    ],
    methods: {
        raceChange(dir){
            this.$emit("race-change", dir);
        }
    }
}
</script>

<style>
    .race-breakdown{
        float: left;
        width: calc(100% - 560px);
        height: 299px;
        border-top: 10px solid var(--main-colour);
        border-right: 10px solid var(--main-colour);
        border-bottom: 10px solid var(--main-colour);
        border-bottom-right-radius: 20px;
        border-top-right-radius: 20px;
        margin-top: 30px;
        margin-left: 40px;
    }

    .race-breakdown-title{
        margin: 0px;
        background-color: white;
        width: 155px;
        margin-top: -15px;
        margin-bottom: 10px;
    }

    .race-switcher{
        border-bottom: solid 1px #dfdfdf;
        height: 40px;
        font-size: 18px;
    }

    .race-sw-left, .race-sw-right{
        float: left;
        width: calc(80px - 5px);
        height: 25x;
        text-align: center;
        font-size: 27px;
        margin-left: 5px;
        background-color: var(--main-colour);
        border-radius: 5px;
        color: white;
    }

    .race-sw-right{
        float: right;
        margin: 0;
        margin-right: 5px;
    }

    .names{
        float: left;
        text-align: center;
        width: calc(100% - 160px);
        margin: 0;
    }

    .race-name{
        margin: 0;
        margin-top: -10px;
    }

    .circuit-name{
        margin: 0;
        font-size: 16px;
        margin-top: 5px;
    }

    .race-sw-left:hover, .race-sw-right:hover{
        text-decoration: underline;
    }

    .race-timings-content{
        width: 250px;
        height: calc(100% - 59px);
        margin-top: 20px;
    }

    .laps-title, .race-winners-title{
        float: left;
        margin: 0;
        margin-bottom: 5px;
    }

    .lap-table-container, .race-winners-container{
        float: left;
        width: 100%;
        
    }

    .lap-table-container{
        margin-top: 20px;
    }

    .lap-table, .race-winners-table{
        border-collapse: collapse; 
        height: 100%;
        width: 100%;
    }

    .lap-table td, .race-winners-table td{
        font-family: 'Titillium Web', sans-serif;
        font-size: 16px;
        text-align: center;
        padding-top: 7px;
        padding-bottom: 7px;
    }

    .sector-heading{
        font-weight: bold;
        width: 30px;
    }

    .lap-table tr:nth-child(odd), .race-winners-table tr:nth-child(odd){
        background-color: rgb(223, 223, 223);
    }
</style>