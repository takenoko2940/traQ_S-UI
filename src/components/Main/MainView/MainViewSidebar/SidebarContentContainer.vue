<template>
  <div
    :data-is-large-padding="largePadding"
    :data-is-clickable="clickable"
    :class="$style.container"
    @click.self="onContainerClick"
  >
    <div v-if="title" :class="$style.header" @click="onTitleClick">
      <h2 :class="$style.headerTitle">{{ title }}</h2>
      <slot name="header-control"></slot>
    </div>
    <slot />
  </div>
</template>

<script lang="ts">
import { defineComponent } from '@vue/composition-api'

export default defineComponent({
  name: 'SidebarContentContainer',
  props: {
    title: String,
    largePadding: {
      type: Boolean,
      default: false
    },
    clickable: {
      type: Boolean,
      default: false
    }
  },
  setup(props, context) {
    const onTitleClick = () => {
      context.emit('click')
    }
    const onContainerClick = () => {
      if (props.clickable) {
        context.emit('click')
      }
    }
    return { onTitleClick, onContainerClick }
  }
})
</script>

<style lang="scss" module>
.container {
  @include background-primary;
  @include color-ui-secondary;
  display: flex;
  flex-direction: column;
  width: 100%;
  padding: 12px;
  border-radius: 4px;
  flex-shrink: 0;
  &[data-is-clickable] {
    cursor: pointer;
  }
  &[data-is-large-padding] {
    padding: 16px;
  }
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8px;
  cursor: pointer;
  &:last-child {
    // 折り畳み時の見た目調整
    margin-bottom: 0;
  }
}

.headerTitle {
  @include size-body1;
  font-weight: bold;
}
</style>
