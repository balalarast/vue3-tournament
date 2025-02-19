<template>
  <div class="vt-item-teams">
    <div>
      <div class="title">
        <span>{{ bracketNode.match?.title }}</span>
      </div>
      <div
        :class="[
          'vt-team',
          'vt-team-top',
          getPlayerClass(bracketNode.match!.team1!),
        ]"
        @mouseover="highlightTeam(bracketNode.match!.team1!.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick(bracketNode.match!.team1!)"
      >
        <span class="name">{{ bracketNode.match!.team1!.name }}</span>
        <span class="score" v-if="bracketNode.match!.team1!.score != undefined && bracketNode.match!.team1!.score >= 0">{{ bracketNode.match!.team1!.score }}</span>
      </div>

      <div
        :class="[
          'vt-team',
          'vt-team-bot',
          getPlayerClass(bracketNode.match!.team2!),
        ]"
        @mouseover="highlightTeam(bracketNode.match!.team2!.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick(bracketNode.match!.team2!)"
      >
        <span class="name">{{ bracketNode.match!.team2!.name }}</span>
        <span class="score" v-if="bracketNode.match!.team2!.score != undefined && bracketNode.match!.team2!.score >= 0">{{ bracketNode.match!.team2!.score }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import ITeam from "../interface/ITeam";
import IBracketNode from "../interface/IBracketNode";

export default defineComponent({
  name: 'GameMatch',
  props: {
    bracketNode: {
      type: Object as PropType<IBracketNode>,
      default: () => ({}),
    },
    highlightedTeamId: {
      type: String,
      default: undefined,
    },
  },
  setup(props, { emit }) {
    const getPlayerClass = (team: ITeam): string => {
      let clazz = "";
      if (
        props.bracketNode.match?.winner !== undefined &&
        team.id === props.bracketNode.match?.winner
      ) {
        clazz = "winner";
      }

      if (props.highlightedTeamId === team.id && !team.disabled) {
        clazz += " highlight";
      }
      
      if(team.disabled) {
        clazz += " disabled";
      }

      return clazz;
    };

    const onClick = (team: ITeam): void => {
      emit("onMatchClick", props.bracketNode?.match?.id);
      emit("onParticipantClick", team, props.bracketNode?.match);
    };

    const highlightTeam = (playerId: string | number): void => {
      emit("onSelectedTeam", playerId);
    };

    const unhighlightTeam = (): void => {
      emit("onDeselectedTeam");
    };

    return {
      getPlayerClass,
      onClick,
      highlightTeam,
      unhighlightTeam,
    };
  },
});
</script>

<style>
/* استایل‌های شما */
</style>
