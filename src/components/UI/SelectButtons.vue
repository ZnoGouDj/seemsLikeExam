<template>
  <div class="btn-container">
    <button
      v-for="image in images"
      :key="image.src"
      @click.stop="onChange(image.alt)"
      :class="{ active: image.isActive, transparent: image.isActive === null }"
    >
      <img :src="image.src" :alt="image.alt" />
    </button>
  </div>
</template>

<script>
import common from '@/assets/COMMON.png';
import html from '@/assets/HTML&CSS.png';
import js from '@/assets/JS.png';
import react from '@/assets/REACT.png';
import redux from '@/assets/REDUX.png';
import ts from '@/assets/TYPESCRIPT.png';

export default {
  data() {
    return {
      images: [
        { src: common, alt: 'COMMON', isActive: null },
        { src: html, alt: 'HTML/CSS', isActive: null },
        { src: js, alt: 'JAVASCRIPT', isActive: null },
        { src: react, alt: 'REACT', isActive: null },
        { src: redux, alt: 'REDUX', isActive: null },
        { src: ts, alt: 'TYPESCRIPT', isActive: null },
      ],
      isTopicSelected: false,
    };
  },
  name: 'select-buttons',
  methods: {
    onChange(name) {
      let current = this.images.find(el => el.alt === name);
      this.images.forEach(el => (el.isActive = false));
      current.isActive = true;
      this.$emit('onChange', name);
      this.isTopicSelected = true;
    },
  },
  props: {
    isSelected: {
      type: Boolean,
    },
  },
  watch: {
    isSelected(value) {
      if (!value) {
        this.images.forEach(el => (el.isActive = null));
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.btn-container {
  display: flex;
}

.transparent {
  opacity: 1;
}

button {
  border-radius: 8px;
  transition: 0.5s all;
  width: 74px;
  height: 74px;
  background: black;
  margin: 0 10px;
  opacity: 0.5;
  cursor: pointer;

  &:hover {
    transform: scale(1.1);
    opacity: 1;
  }

  &.active {
    transform: scale(1.2);
    box-shadow: 0px 0px 8px white;
    opacity: 1;
  }
}

img {
  width: 70px;
  height: 70px;
  background: white;
  border-radius: 8px;
}

.transparentBlock {
  position: absolute;
  margin-top: 75px;
  width: 500px;
  height: 330px;
  opacity: 0;
  z-index: 999;
}
</style>
