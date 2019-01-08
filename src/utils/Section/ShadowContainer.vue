<template>
  <div class="with-shadow">
    <slot/>
  </div>
</template>

<style lang="scss" scoped>
.with-shadow {
	min-width: 330px;
	box-shadow: 0px 15px 20px rgba(0, 0, 1, 0.15);
	border: none;
	padding: 50px 40px;
}
</style>
