<template>
  <div class="btn-container">
    <button
      v-for="image in images"
      :key="image.src"
      @click="onChange(image.alt)"
      :class="{ active: image.isActive }"
    >
      <img :src="image.src" :alt="image.alt" />
    </button>
  </div>
</template>

<script>
import html from '@/assets/HTML&CSS.png';
import js from '@/assets/JS.png';
import react from '@/assets/REACT.png';
import redux from '@/assets/REDUX.png';
import ts from '@/assets/TYPESCRIPT.png';

export default {
  data() {
    return {
      images: [
        { src: html, alt: 'html/css', isActive: false },
        { src: js, alt: 'javascript', isActive: false },
        { src: react, alt: 'react', isActive: false },
        { src: redux, alt: 'redux', isActive: false },
        { src: ts, alt: 'typescript', isActive: false },
      ],
    };
  },
  name: 'select-buttons',
  methods: {
    onChange(name) {
      let current = this.images.find(el => el.alt === name);
      this.images.forEach(el => (el.isActive = false));
      current.isActive = true;
      this.$emit('onChange', name);
    },
  },
};
</script>

<style lang="scss" scoped>
.btn-container {
  display: flex;
}

button {
  border-radius: 8px;
  transition: 0.5s all;
  width: 74px;
  height: 74px;
  background: black;
  margin: 0 10px;

  &:hover {
    transform: scale(1.1);
  }

  &.active {
    transform: scale(1.5);
    box-shadow: 0px 0px 8px white;
  }
}

img {
  width: 70px;
  height: 70px;
  background: white;
  border-radius: 8px;
}
</style>
