<template>
  <Transition
    name="custom-class"
    :enter-active-class="`animate__animated animate__faster ${enterClass}`"
    :leave-active-class="`animate__animated animate__faster ${outClass}`"
    :mode="props.mode"
    appear>
    <slot />
  </Transition>
</template>
<script lang="ts" setup>
import {computed, PropType} from "vue";
import {ANIMATION_LIST} from "./AnimationNames";

const enterClass = computed(() => ANIMATION_LIST[props.name].ENTER)
const outClass = computed(() => ANIMATION_LIST[props.name].LEAVE)

const ANIMATION_MODES = {
  IN_OUT: "in-out",
  OUT_IN: "out-in"
} as const;

const props = defineProps({
  name: {
    type: String as PropType<keyof typeof ANIMATION_LIST>,
    default: "FADE_IN"
  },
  mode: {
    type: String as PropType<typeof ANIMATION_MODES[keyof typeof ANIMATION_MODES]>,
    default: "out-in"
  }
})

</script>
<script lang="ts">
export default {
  name: "STransition"
}
</script>
<style>
:root {
  --animate-duration: .30s;
}
</style>