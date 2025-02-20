<template>
  <div class="vt-wrapper" v-if="recursiveBracket" :style="cssVars">
    <BracketNode
      :bracket-node="recursiveBracket"
      :highlighted-team-id="highlightedTeamId"
      @onSelectedTeam="highlightTeam"
      @onDeselectedTeam="unhighlightTeam"
      @onMatchClick="onMatchClick"
      @onParticipantClick="onParticipantClick"
    >
      <template #match.title="props">
        <slot name="match.title" v-bind="props"></slot>
      </template>
      <template #team="props">
        <slot name="team" v-bind="props"></slot>
      </template>
      <template #feedIn="props">
        <slot name="feedIn" v-bind="props"></slot>
      </template>
    </BracketNode>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, ref, PropType } from 'vue';
import IRound from './interface/IRound';
import IBracketNode from './interface/IBracketNode';
import ITeam from "./interface/ITeam";
import IFeedIn from "./interface/IFeedIn";
import IMatch from './interface/IMatch';
import BracketNode from './components/BracketNode.vue';
// import { courthiveAdaptor } from './adaptor/courthiveAdaptor';
import { transform as transformBracket } from './recursiveBracket';

export default defineComponent({
  components: {
    BracketNode,
  },
  props: {
    rounds: {
      type: Array as PropType<IRound[]>,
      default: () => [],
    },
    format: {
      type: String as PropType<'default' | 'courthive'>,
      default: 'default',
    },
    textColor: {
      type: String,
      default: '#ffffff',
    },
    titleColor: {
      type: String,
      default: '#9d999d',
    },
    teamBackgroundColor: {
      type: String,
      default: '#59595e',
    },
    highlightTeamBackgroundColor: {
      type: String,
      default: '#222222',
    },
    scoreBackgroundColor: {
      type: String,
      default: '#787a7f',
    },
    winnerScoreBackgroundColor: {
      type: String,
      default: '#ee7b3c',
    },
  },
  setup(props, { emit }) {
    const highlightedTeamId = ref<string | undefined>(undefined);

    const onMatchClick = (matchId: string | number) => {
      emit('onMatchClick', matchId);
    };

    const onParticipantClick = (participant: ITeam | IFeedIn, match: IMatch): void => {
      emit('onParticipantClick', participant, match);
    };

    const highlightTeam = (teamId: string) => {
      highlightedTeamId.value = teamId;
    };

    const unhighlightTeam = () => {
      highlightedTeamId.value = undefined;
    };

    const recursiveBracket = computed<IBracketNode|undefined>(() => {
      let copyRound = props.rounds;
      // if (props.format === 'courthive') {
      //   copyRound = courthiveAdaptor(props.rounds);
      // }
      return transformBracket(copyRound);
    });

    const cssVars = computed(() => ({
      '--text-color': props.textColor,
      '--title-color': props.titleColor,
      '--team-background-color': props.teamBackgroundColor,
      '--highlight-team-background-color': props.highlightTeamBackgroundColor,
      '--score-background-color': props.scoreBackgroundColor,
      '--winner-score-background-color': props.winnerScoreBackgroundColor,
    }));

    return {
      highlightedTeamId,
      onMatchClick,
      onParticipantClick,
      highlightTeam,
      unhighlightTeam,
      recursiveBracket,
      cssVars,
    };
  },
});
</script>

<style>
.vt-wrapper {
  display: flex;
}

.vt-item-teams,
.vt-item-feedIn {
  color: var(--text-color);
}

.vt-item-teams .title,
.vt-item-feedIn .title {
  color: var(--title-color);
}

.vt-item-teams .vt-team,
.vt-item-feedIn .vt-feedIn {
  background-color: var(--team-background-color);
}

.vt-item-teams .vt-team .score {
  background-color: var(--score-background-color);
}

.vt-item-teams .vt-team.winner .score {
  background-color: var(--winner-score-background-color);
}

.vt-item-teams .vt-team.highlight,
.vt-item-feedIn .vt-feedIn.highlight {
  background-color: var(--highlight-team-background-color);
}

.vt-item-teams .vt-team.disabled,
.vt-item-feedIn .vt-feedIn.disabled {
  opacity: 0.5;
  cursor: default !important;
  pointer-events: none;
}
.vt-item-teams .vt-team.disabled .score,
.vt-item-feedIn .vt-feedIn.disabled .score {
  opacity: 0.7;
}
</style>