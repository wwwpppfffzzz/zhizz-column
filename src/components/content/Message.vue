<template>
  <teleport to="#message">
    <div class="messageInfo alert" :class="classObject">
      <span>{{ message }}</span>
      <button type="button" class="btn-close" aria-label="Close" @click.prevent="hide"></button>
    </div>
  </teleport>
</template>

<script lang="ts">
import {defineComponent, PropType, ref} from 'vue';
import {useDOMCreate} from '@/hooks/useDOMCreate';

export  type MessageType = 'success' | 'error' | 'default'
export default defineComponent({
  name: 'Message',
  props: {
    message: {
      type: String,
      default: ''
    },
    type: {
      type: String as PropType<MessageType>,
      default: 'default'
    }
  },
  emits: ['close-message'],
  setup(props, context) {
    useDOMCreate('message');
    const isVisible = ref(false);
    const classObject = {
      'alert-success': props.type === 'success',
      'alert-danger': props.type === 'error',
      'alert-primary': props.type === 'default'
    };
    const hide = () => {
      isVisible.value = true;
      context.emit('close-message', false);
    };
    return {classObject, hide};
  }
});
</script>

<style lang="scss" scoped>
.messageInfo {
  display: flex;
  justify-content: space-between;
  position: fixed;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 50%;
  margin: 10px auto;
  text-align: center;
  z-index: 1002;
  opacity: 0.7;
  overflow: hidden;
}
</style>
