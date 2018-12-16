<template>
  <div class="checkbox-container" :class="[classes]">
    <div
      class="checkbox-group"
      :style="checkboxState ? color && `background-color: ${color}; border-color: ${color};` : null"
      @click.stop="toggle()"
    >
      <div class="checkbox-ripple-container" v-ripple @click.stop="toggle()">
        <input
          type="checkbox"
          :id="id || uniqueId"
          :name="name"
          :value="value"
          :disabled="disabled"
          :required="required"
          :color="color"
          :checked="checkboxState"
        >
      </div>
    </div>
    <label class="checkbox_label">
      <slot/>
    </label>
  </div>
</template>

<script>
import ripple from "./ripple";

export default {
	directives: {
		ripple
	},
	model: {
		prop: "model",
		event: "change"
	},
	props: {
		id: {
			type: String,
			default: undefined
		},
		model: {
			type: String | Array,
			default: undefined
		},
		checked: Boolean,
		value: String,
		name: String,
		required: Boolean,
		disabled: Boolean,
		color: String,
		blackMode: Boolean,
		error: Boolean
	},
	data() {
		return {
			uniqueId: ""
		};
	},
	computed: {
		checkboxState() {
			if (this.model === undefined) return this.checked;

			if (Array.isArray(this.model)) return this.model.indexOf(this.value) !== -1;

			return this.model;
		},
		classes() {
			return {
				"checkbox-disabled": this.disabled,
				"checkbox-active": this.checkboxState,
				"black-mode": this.blackMode,
				error: this.error
			};
		}
	},
	methods: {
		toggle() {
			if (this.disabled) return;

			let value = this.model;

			if (Array.isArray(value)) {
				value = value.slice();
				const i = value.indexOf(this.value);

				if (i === -1) value.push(this.value);
				else value.splice(i, 1);
			} else value = !this.checkboxState;

			this.$emit("change", value);
		},

		genId() {
			if (this.id === undefined || typeof String)
				this.uniqueId = `m-checkbox--${Math.random()
					.toString(36)
					.substring(2, 10)}`;
			else this.uniqueId = this.id;
		}
	},
	watch: {
		checked(newVal) {
			if (newVal !== this.checkboxState) this.toggle();
		}
	},
	mounted() {
		this.genId();

		if (this.checked && !this.checkboxState) {
			this.toggle();
		}
	}
};
</script>

<style lang="scss">
$base: #0081c8;
$err: #e6007e;
$size: 20px;
$default-check-color: #fff;
$border-color: #dcdcdc;

$blackModeBack: #232322;

.__ripple {
	&__container {
		color: inherit;
		border-radius: inherit;
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		overflow: hidden;
		z-index: 0;
		pointer-events: none;
		contain: strict;
	}
	&__animation {
		color: inherit;
		position: absolute;
		top: 0;
		left: 0;
		border-radius: 50%;
		background: currentColor;
		opacity: 0;
		transition: 0.4s cubic-bezier(0, 0, 0.2, 1);
		pointer-events: none;
		overflow: hidden;
		will-change: transform, opacity;
		&--enter {
			transition: none;
		}
		&--visible {
			opacity: 0.15;
		}
	}
}
.checkbox-ripple-container {
	box-sizing: border-box;
	z-index: 1;
	position: absolute;
	width: 48px;
	height: 48px;
	top: 50%;
	left: 50%;
	transform: translate(-50%, -50%);
	border-radius: 50%;
}
.checkbox-container {
	box-sizing: border-box;
	display: inline-flex;
	align-items: center;
	position: relative;
	margin: 0.5rem 0;
	width: 100%;
	line-height: $size;
	cursor: pointer;
	.checkbox_label {
		position: relative;
		cursor: pointer;
		color: #232322;
		display: flex;
		align-items: center;
		font-size: 11pt;
		margin: 0;
	}
	i {
		color: $border-color;
		font-size: 11pt;
	}
	.checkbox-group {
		box-sizing: border-box;
		position: relative;
		border-radius: 2px;
		border: 2px solid rgba(0, 0, 0, 0.54);
		border: 2px solid $border-color;
		height: $size;
		width: $size;
		min-width: $size;
		margin-right: 10px;
		transition: 0.4s cubic-bezier(0.25, 0.8, 0.25, 1);
		input[type="checkbox"] {
			position: absolute;
			-webkit-appearance: none;
			appearance: none;
			left: -999rem;
		}
		&:after {
			box-sizing: inherit;
			content: "";
			position: absolute;
			transition: transform 0.25s ease;
			width: 6px;
			height: 13px;
			top: 0;
			left: 5px;
			z-index: 6;
			border: 2px solid $default-check-color;
			border-top: 0;
			border-left: 0;
			opacity: 0;
			transform: rotate(45deg) scale3D(0.1, 0.1, 0.1);
		}
	}
	&:hover {
		.checkbox-group {
			border-color: $base;
		}
		i {
			color: $base;
		}
	}

	&.black-mode {
		&:hover {
			.checkbox-group {
				border-color: $blackModeBack;
			}
		}
	}
}
.checkbox-container.checkbox-active {
	.checkbox-group {
		background-color: $base;
		border-color: $base;
		&:after {
			opacity: 1;
			transform: rotate(45deg) scale3d(1, 1, 1);
		}
	}

	&.black-mode {
		.checkbox-group {
			background-color: $blackModeBack;
			border-color: $blackModeBack;
		}

		.checkbox_label,
		i {
			color: $blackModeBack;
			span {
				background-color: transparent;
				color: $blackModeBack;
				font-weight: 600;
			}
		}
	}

	&.error {
		.checkbox-group {
			background-color: $err;
			border-color: $err;
		}

		.checkbox_label {
			color: $err;
			span {
				background-color: transparent;
				color: $err;
				font-weight: 600;
			}
		}
	}
}
.checkbox-disabled {
	cursor: not-allowed;
	.checkbox_label {
		opacity: 0.24;
		cursor: not-allowed;
	}
}
</style>
