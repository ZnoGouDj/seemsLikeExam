<template>
  <button class="btn"><slot></slot></button>
</template>

<script>
export default {
  name: 'my-button',
};
</script>

<style lang="scss" scoped>
.btn {
  padding: 20px 10px;
  font-weight: 900;
  background-color: #e3ce0f;
  color: black;
  border-radius: 10px;
}
.btn:hover {
  background-color: yellowgreen;
}
</style>
