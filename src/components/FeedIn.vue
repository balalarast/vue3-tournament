<template>
  <div class="vt-item-feedIn">
    <div>
      <slot name="match.title" v-bind="{ match: bracketNode?.match }">
        <div class="title">
          <span>{{ bracketNode?.match?.title }}</span>
        </div>
      </slot>
      <div
        :class="['vt-feedIn', getPlayerClass(bracketNode.match!.feedIn!)]"
        @mouseover="highlightTeam(bracketNode.match!.feedIn!.id)"
        @mouseleave="unhighlightTeam"
        @click="onClick(bracketNode.match!.feedIn!)"
      >
        <slot name="feedIn" v-bind="{ feedIn: bracketNode.match?.feedIn, match: bracketNode.match }">
          <span class="name">{{ bracketNode.match?.feedIn?.name }}</span>
        </slot>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import IBracketNode from "../interface/IBracketNode";
import IFeedIn from "../interface/IFeedIn";

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
    const getPlayerClass = (feedIn: IFeedIn): string => {
      let clazz = "";
      if (props.highlightedTeamId === feedIn.id && !feedIn.disabled) {
        clazz += " highlight";
      }
      if(feedIn.disabled) {
        clazz += " disabled";
      }
      return clazz;
    };

    const onClick = (iFeedIn: IFeedIn): void => {
      emit("onMatchClick", props.bracketNode?.match?.id);
      emit("onParticipantClick", iFeedIn, props.bracketNode?.match);
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
