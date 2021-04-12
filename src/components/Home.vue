<template>
    <div class="content">
        <DriversStandings v-bind:season="this.season" v-bind:driversStandings="this.driversStandings"></DriversStandings>
        <RaceBreakdown @race-change="raceChange" v-bind:season="this.season" v-bind:raceBasicInfo="this.raceBasicInfo" v-bind:fastestDriver="this.fastestDriver" v-bind:fastestTime="this.fastestTime"></RaceBreakdown>
        <ConstructorsStandings v-bind:season="this.season"  v-bind:constructorsStandings="this.constructorsStandings"></ConstructorsStandings>
    </div>
</template>

<script>
import DriversStandings from './homeComponents/DriversStandings.vue';
import ConstructorsStandings from './homeComponents/ConstructorsStandings.vue';
import RaceBreakdown from './homeComponents/RaceBreakdown.vue';

export default {
    components: {
        DriversStandings,
        RaceBreakdown,
        ConstructorsStandings
    },
    props: [
        'season',
        'driversStandings',
        'constructorsStandings',
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
