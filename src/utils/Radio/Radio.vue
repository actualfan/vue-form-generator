<template>
  <div :style="{width: inputWidth}">
    <label class="container" :for="name + label">
      <input
        type="radio"
        :name="name"
        :id="name + label "
        :value="label"
        @change="change"
        :checked="label == value"
      >
      <span class="checkmark"></span>
      <span class="checkmark-active"></span>
      <span class="check-label">{{text || label}}</span>
    </label>
  </div>
</template>

<script>
export default {
	props: ["value", "label", "text", "inputWidth", "required", "name"],
	methods: {
		change(e) {
			console.log(e);
			this.$emit("input", e.target.value);
		}
	},
	computed: {
		checked() {
			return this.label === this.value;
		}
	}
};
</script>

<style lang="scss" scoped>
.container {
	margin-left: unset;
	position: relative;
	padding-left: 35px;
	cursor: pointer;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	font-size: 18px;
	color: #232322;
	font-weight: 300;
	.check-label {
		position: relative;
		top: -2px;
		left: -5px;
		@media (max-width: 960px) {
			top: -17px;
		}
	}
	display: block;
	input {
		position: absolute;
		opacity: 0;
		cursor: pointer;
		height: 0;
		width: 0;
	}
	.checkmark {
		position: absolute;
		top: 0;
		left: 0;
		width: 20px;
		height: 20px;
		border-radius: 10px;
		background-color: #ffffff;
		border: 2px solid #dcdcdc;
		&:hover {
			border: 2px solid #0081c8;
		}
	}

	input:checked ~ .checkmark-active {
		position: absolute;
		top: 5px;
		left: 5px;
		width: 10px;
		height: 10px;
		border-radius: 5px;
		background-color: #0081c8;
	}
	input:checked ~ .checkmark {
		border: 2px solid #0081c8;
	}
	.check-label ~ input:checked {
		font-weight: bold;
	}
}
</style>

