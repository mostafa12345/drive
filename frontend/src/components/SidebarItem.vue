<template>
  <button
    class="flex h-9 w-full cursor-pointer items-center rounded-lg duration-300 ease-in-out px-2 text-sm font-medium transition-all"
    :class="[
      isActive
        ? 'bg-[#b31313] text-white border-l-4 border-[#7e1010]'
        : 'hover:bg-[#faebeb] text-gray-800 border-l-4 border-transparent',
      'focus:outline-none focus:ring-2 focus:ring-[#b31313]'
    ]"
    @click="handleClick"
  >
    <div class="flex w-full items-center justify-between duration-300 ease-in-out">
      <div class="flex items-center">
        <Tooltip
          :text="label"
          placement="right"
          arrow-class="fill-gray-900"
          :disabled="!isCollapsed"
        >
          <slot name="icon">
            <span class="grid h-4.5 w-4.5 flex-shrink-0 place-items-center">
              <FeatherIcon
                v-if="typeof icon == 'string'"
                :name="icon"
                :class="isActive ? 'text-white' : 'text-[#b31313]'"
              />
              <component :is="icon" v-else :class="isActive ? 'text-white' : 'text-[#b31313]'" />
            </span>
          </slot>
        </Tooltip>
        <span
          class="flex-1 flex-shrink-0 text-sm duration-300 ease-in-out"
          :class="
            isCollapsed
              ? 'ml-0 w-0 overflow-hidden opacity-0'
              : 'ml-2 w-auto opacity-100'
          "
        >
          {{ label }}
        </span>
      </div>
      <slot name="right" />
    </div>
  </button>
</template>

<script setup>
import { Tooltip, FeatherIcon } from "frappe-ui"
import { computed } from "vue"
import { useStore } from "vuex"
import { useRouter } from "vue-router"

const router = useRouter()
const store = useStore()

const props = defineProps({
  icon: {
    type: [String, Object],
    default: null,
  },
  label: {
    type: String,
    default: "",
  },
  to: {
    type: [Object, String],
    default: "",
  },
  isCollapsed: {
    type: Boolean,
    default: false,
  },
})

function handleClick() {
  router.push({ path: props.to })
}

let isActive = computed(() => {
  return store.state.currentBreadcrumbs[0].label === props.label
})
</script>
