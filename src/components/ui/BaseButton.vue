<template lang="pug">
button
  slot
</template>

<style scoped lang="scss">
button {
  padding: 15px 20px;
  font-size: 20px;
  border-radius: 50px;
  background-color: #60b8d1;
  border: none;
  cursor: pointer;

  &:hover {
    background-color: #279cc6;
    color: white;
  }
}
</style>
