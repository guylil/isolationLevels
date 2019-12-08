<template>
  <div>
      <h1>Home assignment:</h1>
      <h2>The most isolated country:</h2>
      <div>
        <span class="bold">{{sortedIsoCountries[0].country}}</span> with isolation level of <span class="bold">{{this.sortedIsoCountries[0].isoLevel}}</span>
      </div>

        <table class="table">
            <thead class="head">
                <tr>
                    <th>Agent ID</th>
                    <th>Country</th>
                    <th>Address</th>
                    <th>Date</th>
                </tr>
            </thead>
            <tbody class="body">
                <tr v-for="(mission, idx) in missionsSortedByDates" :key="idx">
                    <td>{{mission.agent}}</td>
                    <td>{{mission.country}}</td>
                    <td>{{mission.address}}</td>
                    <td>{{mission.date}}</td>
                </tr>
            </tbody>
            <tfoot>
                <tr>
                    <td colspan="4" class="bottomRow">{{missionsSortedByDates.length}} missions</td>
                </tr>
            </tfoot>
        </table>
  </div>
</template>

<script>
    import missions from '../assets/data.js'

    export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: () => ({
    missions,
  }),
  mounted() {
  },
  methods: {
      isolationLevel(country) {
          return this.isolatedAgents.reduce((acc, cur) => cur.country === country ? ++acc : acc, 0)
      }
  },
  computed: {
    agentsIsolationStat() {
        let agentsStats = {};
        this.missions.forEach(m =>{
                if (!agentsStats[m.agent]) {agentsStats[m.agent] = [m.country]}
                else {agentsStats[m.agent].push(m.country)}
        });
        return agentsStats
    },
    isolatedAgents() {
        const isolatedAgents = [];
        for (const a in this.agentsIsolationStat) {
            if (this.agentsIsolationStat[a].length === 1)
                isolatedAgents.push({agent:a, country:this.agentsIsolationStat[a][0]});
        }
        return isolatedAgents
    },
      sortedIsoCountries() {
        const res = [];
        this.countries.forEach(country => {
            res.push({country, isoLevel: this.isolationLevel(country)})
        });
        return res.sort((a,b) => b.isoLevel - a.isoLevel)
      },
      countries() {return [...new Set(this.missions.map(mission => mission.country))]},
      missionsSortedByDates() {
          let res = JSON.parse(JSON.stringify(this.missions));
        return res.sort((a,b) => new Date(a.date) - new Date(b.date));
      },
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.bold {
    font-weight: bold;
    font-size: large;
}
.table {
    margin-top: 15px;
    border: silver solid 1px;
    border-collapse: collapse;
}
.head {
    text-align: left;
    background-color: whitesmoke;
    border-bottom: lightgrey solid 4px;
}
tr { }
th {
    padding: 10px;
    border-left: lightgrey solid 1px;

}
td {
    padding: 8px 10px;
    border-top: lightgrey double 2px;
}
hr {
    background-color: red;
    color: blue;
}
.bottomRow {
    border-top: lightgrey solid 4px;
    text-align: end;
    background-color: whitesmoke;
}
</style>
