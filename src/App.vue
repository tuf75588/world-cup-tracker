<template>
  <section>
    <section class="scores">
      <h1 class="header">
        {{results.MatchTime}}
        <br />
        <small class="time">{{last_retrieved.toTimeString()}}</small>
      </h1>
      <section>
        <Team :team="home_team" :penalty_score="results.HomeTeamPenaltyScore"></Team>
        <Team :team="away_team" :penalty_score="results.AwayTeamPenaltyScore"></Team>
      </section>
    </section>
    <h1 class="round-name" v-if="results.StageName">{{results.StageName[0].Description}}</h1>
  </section>
</template>

<script>
// TODO: Find an API to get a list of matches, create a drop down to select the match to show
const API = 'https://api.fifa.com/api/v1/live/football/17/254645/275093/300331498?language=en-GB';
import Team from './components/Team';

export default {
  components: {
    Team
  },
  data: () => ({
    first: true,
    last_retrieved: new Date(),
    results: {},
    home_team: {},
    away_team: {},
  }),
  mounted() {
    this.getScores();
  },
  methods: {
    getScores() {
      fetch(API)
        .then(res => res.json())
        .then(results => {          
          if(!this.first && (results.HomeTeam.Score != this.home_team.Score
              || results.AwayTeam.Score != this.away_team.Score
              || results.HomeTeamPenaltyScore != this.results.HomeTeamPenaltyScore
              || results.AwayTeamPenaltyScore != this.results.AwayTeamPenaltyScore)) {
     
          }
          this.last_retrieved = new Date();
          this.results = results;
          this.home_team = results.HomeTeam;
          this.away_team = results.AwayTeam;
          this.first = false;
          setTimeout(() => {
            this.getScores();
          }, 5000);
        });
    }
  }
};
</script>

<style>
body {
  text-align: center;
  font-family: sans-serif;
  font-size: 3em;
}
.round-name {
  font-size: 1em;
  margin: 0px;
}
.scores {
  display: flex;
  justify-content: center;
  align-items: center;
}
.team {
  width: 30%;
  display: inline-block;
  padding: 10%;
}
.time {
  font-size: 0.15em;
}
</style>