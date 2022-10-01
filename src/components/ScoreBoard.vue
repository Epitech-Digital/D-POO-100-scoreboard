<template>
  <div style="margin: auto">
    <LightSaber :key="team" v-for="team in teams" v-bind:color='team.color' v-bind:team="team.name" v-bind:score="team.score"></LightSaber>
  </div>
</template>

<script>
import LightSaber from './LightSaber.vue'
import {PublicGoogleSheetsParser} from "@/Services/PublicGoogleSheetParser";


export default {
  name: "ScoreBoard",
  data() {
    return {
      teams: []
    }
  },
  beforeMount() {
    let i = 1;
    this.teams = []
    while (process.env['VUE_APP_TEAM_' + i + '_NAME']) {
      this.teams
          .push({color: process.env['VUE_APP_TEAM_' + i + '_COLOR'], name: process.env['VUE_APP_TEAM_' + i + '_NAME'], score:0})
      i++
    }
  },
  mounted() {
    this.updateScores();
    if (process.env.VUE_APP_REFRESH_RATE >= 1)
      setInterval(this.updateScores, process.env.VUE_APP_REFRESH_RATE * 60000) //VUE_APP_REFRESH_RATE must be in minutes
    else
      console.warn('Refresh rate (VUE_APP_REFRESH_RATE) is too low or badly formatted. Scores will not automatically refresh')
  },
  methods:{
    updateScores: function () {
      const spreadsheetId = process.env.VUE_APP_SHEETID
      const parser = new PublicGoogleSheetsParser(spreadsheetId, process.env.VUE_APP_SHEET_SCORES_NAME)
      parser.parse().then(items => {
        items.forEach( item => {
          let team = this.teams.find(team => {
            return team.name.toLowerCase() === item.Equipe.toLowerCase()
          });
          if (team)
              team.score = item.Point
        })
      })}
  },
  components: {
    LightSaber
  }
}

</script>

<style scoped>

</style>