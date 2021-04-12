<template>
  <div id="app">
    <Header @menu-press="changeView" @season-change="updateSeason"></Header>
    <Home  @race-change="updateRaceData" v-bind:season="this.season" v-bind:driversStandings="this.driversStandings" v-bind:constructorsStandings="this.constructorsStandings" v-bind:raceBasicInfo="this.raceBasicInfo" v-bind:fastestDriver="this.fastestDriver" v-bind:fastestTime="this.fastestTime" v-if="content == 'Home'"></Home>
    <Drivers v-if="content == 'Drivers'"></Drivers>
    <Teams v-if="content == 'Teams'"></Teams>
  </div>
</template>

<script>
//Component Imports
import Header from './components/Header.vue';
import Home from './components/Home.vue';
import Drivers from './components/Drivers.vue';
import Teams from './components/Teams.vue';

export default {
  name: 'App',
  components: {
    Header,
    Home,
    Drivers,
    Teams
  },
  data(){
    return{
      content: "Home",
      season: 0,
      driversStandings: [],
      constructorsStandings: [],
      raceBasicInfo: [],
      round: 0,
      totalRounds: 0,
      fastestDriver: {},
      fastestTime: ""
    }
  },
  methods: {
    changeView(item){
      console.log("menu-press", item);
      this.content = item;
    },
    updateSeason(season){
      console.log("season-change", season);
      this.season = season;
      this.getSeasonData(season);
      
    },
    updateRaceData(dir){


      if(dir == "next"){
        this.round++;
      }else if(dir == "prev"){
        this.round--;
      }

      if(this.round > this.totalRounds){
        this.round--;
      }else if(this.round < 1){
        this.round++;
      }

      
      console.log("round:",this.round);

      this.getRaceData();
    },
    getRaceData(){
      //Get current race
      var newRaceData = []
      this.$http.get("https://ergast.com/api/f1/"+this.season+"/"+this.round+"/results.json").then((data) => {
        var seasonsRaces = data.data.MRData.RaceTable.Races;
        newRaceData.push({
          roundNumber: seasonsRaces[0].round,
          raceName: seasonsRaces[0].raceName,
          circuitName:  seasonsRaces[0].Circuit.circuitName,
          results: {
            p1: {
              name: seasonsRaces[0].Results[0].Driver.givenName + " " +seasonsRaces[0].Results[0].Driver.familyName,
            },
            p2: {
              name: seasonsRaces[0].Results[1].Driver.givenName + " " + seasonsRaces[0].Results[1].Driver.familyName,
            },
            p3: {
              name: seasonsRaces[0].Results[2].Driver.givenName + " " +seasonsRaces[0].Results[2].Driver.familyName,
            }
          }
        });
      });
      this.raceBasicInfo = newRaceData;
      console.log(newRaceData);

      if(this.season >=2004){
        //Get the fastest lap driver of the race
        var fastestDriverCode = "";
        var newFastestDriver = {};
        this.$http.get("https://ergast.com/api/f1/"+this.season+"/"+this.round+"/fastest/1/drivers.json").then((data) => {
          fastestDriverCode = data.data.MRData.DriverTable.Drivers[0].driverId;
          newFastestDriver = {
            name: data.data.MRData.DriverTable.Drivers[0].givenName + " " + data.data.MRData.DriverTable.Drivers[0].familyName,
            code: fastestDriverCode
          };
          this.getFastestLapData(newFastestDriver.code);
          this.fastestDriver = newFastestDriver;
        });
      }else{
        //Catch out pre-2004 missing data.
        newFastestDriver = {
            name: "N/A"
        };
        this.fastestDriver = newFastestDriver;
        this.fastestTime = "-"
      }
      
    },
    getFastestLapData(driverID){
      //Filter through laps of the race to find fastest of the driver we know was fastest
      this.$http.get("https://ergast.com/api/f1/"+this.season+"/"+this.round+"/laps.json?limit=15000").then((data) => {
        var raceLaps = data.data.MRData.RaceTable.Races[0].Laps;        
        var laps = [];
        var lap_times = {};
        for(var i=0;i<raceLaps.length;i++){
          for(var k=0; k<raceLaps[i].Timings.length; k++){
            if(raceLaps[i].Timings[k].driverId == driverID){
              var lap = raceLaps[i].Timings[k].time.replace(":","");
              laps.push(lap);
            }
          }
        }

        laps = laps.map(function(lapTime){
          lap = Number(lapTime.replace(":",""));
          lap_times[lap] = lapTime;
          return lap;
        });

        var uncleanTime = lap_times[Math.min(...laps)];
        var cleanTime = uncleanTime.charAt(0)+":"+uncleanTime.substr(1,uncleanTime.length);
        console.log(cleanTime);
        this.fastestTime = cleanTime;
        
      });
    },
    getSeasonData(season){

      var newDriverStandingsData = [];
      var newConstructorsStandingsData = [];
      
      //Get full season standings (wdc)
      this.$http.get("https://ergast.com/api/f1/"+season+"/driverStandings.json").then((data) => {
        
        var uncleanChampionships = data.data.MRData.StandingsTable.StandingsLists[0].DriverStandings;
        var rounds = data.data.MRData.StandingsTable.StandingsLists[0].round;
        this.totalRounds = rounds;
        for(var i=0; i<uncleanChampionships.length; i++){
          var driver = uncleanChampionships[i].Driver;
          var constructor = uncleanChampionships[i].Constructors[0];
          newDriverStandingsData.push({
            position: uncleanChampionships[i].position,
            firstName: driver.givenName,
            lastName: driver.familyName,
            points: uncleanChampionships[i].points,
            team: constructor.name,
            ppr: uncleanChampionships[i].points / rounds
          });
        }
      });

      //Get full seasons standings (wcc)
      this.$http.get("https://ergast.com/api/f1/"+season+"/constructorStandings.json").then((data) => {
        var uncleanChampionships = data.data.MRData.StandingsTable.StandingsLists[0].ConstructorStandings;
        var rounds = data.data.MRData.StandingsTable.StandingsLists[0].round;
        console.log(uncleanChampionships);
        for(var i=0; i<uncleanChampionships.length; i++){
          var constructor = uncleanChampionships[i].Constructor;
          newConstructorsStandingsData.push({
            position: uncleanChampionships[i].position,
            name: constructor.name,
            points: uncleanChampionships[i].points,
            ppr: uncleanChampionships[i].points / rounds
          });
        }
      });

      this.driversStandings = newDriverStandingsData;
      this.constructorsStandings = newConstructorsStandingsData;
      this.round = 1;
      this.getRaceData();

    }
  }
}
</script>

<style>
  #app{
    margin: 0;
    height: 100%;
  }
</style>