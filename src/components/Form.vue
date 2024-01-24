<script>
export default {
  name: "Form",
  props: {
    style: {
      type: Object,
      default: () => ({
        color: "#D63057",
        opacity: 0.5,
      }),
    },
  },
  data() {
    return {
      color: this.style.color,
      opacity: this.style.opacity,
    };
  },
  watch: {
    color(newColor) {
      this.emitStyleChange();
    },
    opacity(newOpacity) {
      this.emitStyleChange();
    },
  },
  methods: {
    emitStyleChange() {
      this.$emit("change-style", {
        color: this.color,
        opacity: this.opacity,
      });
    },
  },
};
</script>

<template>
  <div>
    <h2 class="text-lg">Змінити стилі геометрії</h2>
    <div class="flex items-cente gap-x-4">
      <label for="color">Колір: </label>
      <input type="color" id="color" v-model="color" />
    </div>
    <div class="flex items-center gap-x-4">
      <label for="opacity">Прозорість: </label>
      <input
        id="opacity"
        type="range"
        min="0"
        max="1"
        step="0.01"
        v-model="opacity"
      />
      <span>{{ opacity }}</span>
    </div>
  </div>
</template>
