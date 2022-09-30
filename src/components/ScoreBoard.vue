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
      teams: [
        {color: '#FEBF51',name: 'jedi',score:0},
        {color: '#E97077',name: 'sith',score:0},
        {color: '#62C5EB',name: 'rebels',score:0}
      ]
    }
  },
  mounted() {
    this.updateScores();
    setInterval(this.updateScores, process.env.VUE_APP_REFRESH_RATE * 60000) //VUE_APP_REFRESH_RATE must be in minutes
  },
  methods:{
    updateScores: function () {
      console.log(process.env.VUE_APP_SHEETID)
      const spreadsheetId = process.env.VUE_APP_SHEETID
      const parser = new PublicGoogleSheetsParser(spreadsheetId, process.env.VUE_APP_SHEET_SCORES_NAME)
      parser.parse().then(items => {
        items.forEach( item => {
          this.teams.find(team => {
            return team.name.toLowerCase() === item.Equipe.toLowerCase()
          }).score = item.Point
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