<template>
  <div class="title-main">{{text}}</div>
</template>


<script>
export default {
	props: ["text"]
};
</script>

<style lang="scss" scoped>
.title-main {
	font-size: 47px;
	line-height: 60px;
	font-family: "Latino", sans-serif;
	color: #232322;
	font-weight: bold;
}
</style>

