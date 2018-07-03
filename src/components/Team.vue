<template>
  <section class="team">
    <h1 v-if="team.TeamName">{{team.TeamName[0].Description}}</h1>
    <img :src="getImage" />
    <p>{{team.Score}} - ({{penalty_score}})</p>
    <div>
      <p v-for="goal in team.Goals" class="goal" :key="goal.IdPlayer">
        {{goal.Minute}}
        <br />
        <img class="player-img" :src="'https://api.fifa.com/api/v1/picture/players/2018fwc/' + goal.IdPlayer + '_sq-300_jpg?allowDefaultPicture=true'" />
        <br />
        <span class="player-name">{{getPlayerName(goal.IdPlayer)}}</span>
      </p>
    </div>
  </section>
</template>

<script>
export default {
  props: ['team', 'penalty_score'],
  computed: {
    getImage() {
      return 'https://api.fifa.com/api/v1/picture/flags-fwc2018-4/' + this.team.IdCountry;
    }
  },
  methods: {
    getPlayerName(id) {
      const player = this.team.Players.find(player => player.IdPlayer == id);        
      return player ? player.PlayerName[0].Description : '';
    }
  }
};
</script>

<style>
.goal {
  font-size: 0.5em;
}
.player-img {
  height: 50px;
}
.player-name {
  font-size: 0.75em;
}
</style>