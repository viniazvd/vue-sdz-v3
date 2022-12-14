<template>
  <div class="relative">
    <span ref="activatorSlot">
      <slot name="activator" v-bind="{on: eventListeners.on, isOpen}" />
    </span>

    <STransition>
      <div
        class="dropdown-borders dropdown-shadows dropdown-position dropdown-colors ring-1 ring-black/5 min-w-full z-20"
        :class="[anchor]"
        v-show="isOpen"
        ref="dropdownTarget">
        <slot v-bind="eventListeners" />
      </div>
    </STransition>
  </div>
</template>
<script lang="ts" setup>
import {useToggle, onClickOutside} from "@vueuse/core";
import {computed, PropType, ref} from "vue";
import { ANIMATION_LIST } from "../STransition/AnimationNames";
import STransition from "../STransition/Index.vue";

const [isOpen, setIsOpen] = useToggle(false);
const eventListeners = {
  on: {
    click: async () => {
      emit("close:dropdown")
      setIsOpen(!isOpen.value)
    }
  }
}

defineExpose({setIsOpen, isOpen})

const dropdownTarget = ref();
const activatorSlot = ref();
const anchor = computed(() => [props.anchor === "right" && "right-0"]);

onClickOutside(dropdownTarget, () => setIsOpen(false), {
  ignore: [activatorSlot]
});

const emit = defineEmits(["close:dropdown"])

const props = defineProps({
  animation: {
    type: String as PropType<keyof typeof ANIMATION_LIST>,
    default: "ZOOM_IN"
  },
  anchor: {
    type: String as PropType<"right" | "left">,
    default: "left"
  }
})

</script>
<script lang="ts">
export default {
  name: "SDropdown"
}
</script>
<style lang="postcss" scoped>
.dropdown-shadows {
  @apply shadow-xl
}

.dropdown-position {
  @apply absolute mt-1;
}

.dropdown-borders {
  @apply rounded-lg;
}

.dropdown-colors {
  @apply  bg-white dark:bg-stone-800;
}

</style>