<template>
  <div>
    <div
      class="select-input"
      :style="{width: inputWidth}"
      @click="showMenu = !showMenu"
      :class="error? 'error' : ''"
    >
      {{displayValue(value)}}
      <div :class="showMenu ? 'opened-arrow' : 'arrow'"></div>
    </div>
    <div class="border-bot"></div>
    <div class="options-container" v-show="showMenu" @click="showMenu = false"></div>
    <ShadowContainer class="options" v-show="showMenu" :style="{width: inputWidth}">
      <div
        class="option"
        v-for="option in options"
        :key="option[vKey] || option"
        @click="change(option[vKey] || option)"
      >{{option[vLabel] || option}}</div>
    </ShadowContainer>
  </div>
</template>

<script>
import ShadowContainer from "../Section/ShadowContainer.vue";
import Row from "../Row/Row.vue";
export default {
	components: { ShadowContainer, Row },
	props: ["options", "value", "inputWidth", "placeholder", "error", "identifier", "label"],
	data() {
		return {
			showMenu: false
		};
	},
	computed: {
		vKey() {
			return this.identifier ? this.identifier : "value";
		},
		vLabel() {
			return this.label ? this.label : "label";
		}
	},
	methods: {
		change(value) {
			this.showMenu = false;
			this.$emit("input", value);
		},
		displayValue(value) {
			if (value === "" || value === null || value === undefined) {
				return this.placeholder;
			}
			if (this.options[0] && this.options[0][this.vKey]) {
				const curOption = this.options.find((option) => option[this.vKey] === value);
				if (curOption) {
					return curOption[this.vLabel];
				} else {
					return this.placeholder;
				}
			}
			return value;
		}
	}
};
</script>

<style lang="scss" scoped>
.select-input {
	outline: none;
	font-size: 16px;
	color: #232322;
	font-weight: 300;
	// border-bottom: 2px solid #dcdcdc;
	padding: 8px 2px;
	margin: 12px 0;
	line-height: 20px;
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	cursor: pointer;
	.arrow {
		color: rgba(0, 0, 0, 0);
		width: 15px;
		height: 15px;
		border-right: 3px solid black;
		border-bottom: 3px solid black;
		transform: rotate(45deg);
		border-radius: 3px;
		-webkit-transition: 0.3s ease-in-out;
		-moz-transition: 0.3s ease-in-out;
		-o-transition: 0.3s ease-in-out;
		transition: 0.3s ease-in-out;
        margin-left: 8px;
	}
	.opened-arrow {
		color: rgba(0, 0, 0, 0);
		width: 15px;
		height: 15px;
		border-right: 3px solid black;
		border-bottom: 3px solid black;
		transform: rotate(-135deg);
		border-radius: 3px;
		-webkit-transition: 0.3s ease-in-out;
		-moz-transition: 0.3s ease-in-out;
		-o-transition: 0.3s ease-in-out;
		transition: 0.3s ease-in-out;
        margin-left: 8px;
	}
}

.border-bot {
	border-top: 2px solid #dcdcdc;
}

.options {
	width: 100%;
	padding: 0 !important;
	position: absolute;
	z-index: 2;
	background-color: #fff;
	cursor: pointer;
	.option {
		font-size: 16px;
		padding: 20px 2px;
		line-height: 20px;
		font-weight: 300;
		&:hover {
			background-color: #0081c8;
			color: #fff;
		}
	}
}

.options-container {
	width: 100%;
	height: 100vh;
	position: fixed;
	top: 0;
	left: 0;
	z-index: 1;
}

.error {
	background-color: rgba(230, 0, 126, 0.2);
}
</style>
