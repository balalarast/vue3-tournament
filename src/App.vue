<template>
  <TournamentBracket
    :rounds="rounds"
    format="default"
    @onMatchClick="onMatchClick"
    @onParticipantClick="onParticipantClick"
  >
    <!-- 
    <template #match.title="{ match }">
      <div class="title">
        <span>{{ match?.title }}</span>
      </div>
    </template>
    <template #team.name="{ team }">
      <span class="name">{{ team.name }}</span>
    </template>
    <template #team.score="{ team }">
      <span class="score" v-if="team.score != undefined && team.score >= 0">{{ team.score }}</span>
    </template>
    <template #feedIn="{ feedIn }">
      <span class="name">{{ feedIn.name }}</span>
    </template>
    -->
  </TournamentBracket>
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
