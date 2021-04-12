<template>
    <div class="standings">
            <h3 class="standings-title">{{this.season}} World Drivers Championship</h3>
            <table class="standings-table">
                <tr>
                    <th>Position</th>
                    <th>Driver</th>
                    <th>Team</th>
                    <th>Points</th>
                </tr>
                <tr v-for="driver in this.driversStandings" v-bind:key="driver.id">
                    <td>{{driver.position}}</td>
                    <td>{{driver.firstName+" "+driver.lastName}}</td>
                    <td>{{driver.team}}</td>
                    <td>{{driver.points}}</td>
                </tr>  
            </table>
        </div>
</template>

<script>
export default {
    props: [
        'season',
        'driversStandings'
    ]
}
</script>

<style>
    .standings{
        float: left;
        width: 500px;
        border-top: 10px solid var(--main-colour);
        border-right: 10px solid var(--main-colour);
        border-bottom: 10px solid var(--main-colour);
        border-bottom-right-radius: 20px;
        border-top-right-radius: 20px;
        margin-top: 30px;
        min-height: calc(100% - 103px);
    }
    
    .standings-title{
        margin: 0px;
        background-color: white;
        width: 370px;
        margin-top: -15px;
        margin-bottom: 10px;
    }

    .standings-table{
        border-collapse: collapse; 
        height: 100%;
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