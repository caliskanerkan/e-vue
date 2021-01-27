<template>
    <button :class="btnClasses" :disabled="disabled">
      <slot>
        <span class="btn--content" :style="{opacity: btnLabelOpacity}">
          <Icon v-if="icon" name="fas fa-home" />
          {{label}}
        </span>
        <span v-if="loading" class="btn--loading">
          <spinner speed="normal" :size="25" color="#fff" />
        </span>
      </slot>
    </button>
</template>
<script>
import { toRefs, computed } from 'vue'
import Spinner from './Spinner.vue'
import Icon from './Icon.vue'
export default {
  components: { Spinner, Icon },
  props: {
    block: {
      type: Boolean,
      required: false,
      default: false
    },
    color: {
      type: String,
      required: false,
      default: null,
      validator: function (value) {
        return ['primary', 'secondary', 'success', 'info', 'warning', 'error'].indexOf(value) !== -1
      }
    },
    loading: {
      type: Boolean,
      required: false,
      default: false
    },
    disabled: {
      type: Boolean,
      required: false,
      default: false
    },
    round: {
      type: Boolean,
      required: false
    },
    label: {
      type: String,
      required: false
    },
    size: {
      type: String,
      required: false,
      default: 'normal',
      validator: function (value) {
        return ['big', 'extra', 'small', 'normal'].indexOf(value) !== -1
      }
    },
    icon: {
      type: String,
      required: false
    }
  },
  setup (props) {
    const { color, size, disabled, loading, round, block, label } = toRefs(props)
    const blockBtn = computed(() => {
      return {
        'btn--flex': block.value
      }
    })
    const btnLabelOpacity = computed(() => loading.value ? 0 : 1)
    const btnClasses = computed(() => {
      return {
        btn: true,
        // eslint-disable-next-line
        [`btn__${color.value}`]: color.value,
        // eslint-disable-next-line
        [`btn__${size.value}`]: size.value,
        btn__round: round.value,
        'btn--block': block.value,
        btn__disable: disabled.value
      }
    })
    return {
      btnLabelOpacity,
      blockBtn,
      // eslint-disable-next-line
      label,
      btnClasses
    }
  }
}
</script>
