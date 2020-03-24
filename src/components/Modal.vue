<template>
  <div v-if="showing" class="fixed inset-0 w-full h-screen flex items-center justify-center bg-semi-75">
    <div class="relative w-full max-w-2xl bg-white shadow-lg rounded-lg p-8">
      <button aria-label="close" class="absolute top-0 right-0 text-xl text-gray-500 my-2 mx-4"  @click.prevent="close">
        ×
      </button>
      <slot />
    </div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: [
    'showing'
  ],
  methods: {
    close() {
      this.$emit('close');
    }
  }
};
</script>
<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: all 0.4s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
