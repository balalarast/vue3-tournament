<template>
  <TournamentBracket
    :rounds="rounds"
    format="default"
    @onMatchClick="onMatchClick"
    @onParticipantClick="onParticipantClick"
  />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import IRound from './interface/IRound';
import TournamentBracket from './TournamentBracket.vue';
import bracketData from './mockData/singleElimination8WithFeedIn.json';
import ITeam from "./interface/ITeam";
import IFeedIn from "./interface/IFeedIn";
import IMatch from "./interface/IMatch";

export default defineComponent({
  components: {
    TournamentBracket,
  },
  data() {
    return {
      rounds: bracketData as IRound[],
    };
  },
  methods: {
    onMatchClick(matchId: string | number): void {
      alert(`click: ${matchId}`);
    },
    onParticipantClick(participant: ITeam | IFeedIn, match: IMatch): void {
      console.log("participant", participant)
      console.log("match", match)
    }
  },
});
</script>
