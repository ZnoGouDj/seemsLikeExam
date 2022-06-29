<template>
  <div class="btn-container">
    <button v-for="image in images" :key="image.src" @click="onChange(image.alt)">
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
        { src: html, alt: 'html/css' },
        { src: js, alt: 'javascript' },
        { src: react, alt: 'react' },
        { src: redux, alt: 'redux' },
        { src: ts, alt: 'typescript' },
      ],
    };
  },
  name: 'select-buttons',
  methods: {
    onChange(name) {
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
  width: 84px;
  height: 84px;
  background: black;

  &:hover {
    transform: scale(1.1);
  }
}

img {
  width: 80px;
  height: 80px;
  background: white;
  border-radius: 8px;
}
</style>
