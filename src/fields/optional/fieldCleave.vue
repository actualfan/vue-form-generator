<template lang="pug">
	div.flex-view
		input.form-control(type="text", :value="value", :autocomplete="schema.autocomplete", :disabled="disabled", :placeholder="schema.placeholder", :readonly="schema.readonly", :name="schema.inputName", :id="getFieldID(schema)")
		span.helper(v-if="schema.unit") {{ schema.unit }}
</template>

<script>
import abstractField from "../abstractField";
import { defaults } from "lodash";

export default {
	mixins: [abstractField],

	data() {
		return {
			cleave: null,
			inputEle: null
		};
	},

	mounted() {
		this.$nextTick(function() {
			this.inputEle = this.$el.querySelector("input");
			if (window.Cleave) {
				this.cleave = new window.Cleave(
					this.inputEle,
					defaults(this.schema.cleaveOptions || {}, {
						// Credit Card
						creditCard: false,
						// onCreditCardTypeChanged: onCreditCardTypeChanged.bind(this),
						// Phone
						phone: false,
						phoneRegionCode: "AU",
						// Date
						date: false,
						datePattern: ["d", "m", "Y"],
						// Numerals
						numeral: false,
						numeralThousandsGroupStyle: "thousand",
						numeralDecimalScale: 2,
						numeralDecimalMark: ".",
						// General
						blocks: [],
						delimiter: " ",
						prefix: null,
						numericOnly: false,
						uppercase: false,
						lowercase: false,
						maxLength: 0
					})
				);

				if (this.cleave.properties && this.cleave.properties.hasOwnProperty("result")) {
					this.$watch("cleave.properties.result", () => {
						this.value = this.cleave.properties.result;
					});
				} else {
					this.inputEle.addEventListener("input", this.inputChange);
				}
			} else {
				console.warn("Cleave is missing. Please download from https://github.com/nosir/cleave.js/ and load the script in the HTML head section!");
			}
		});
	},

	methods: {
		inputChange() {
			this.value = this.inputEle.value;
		}
	},

	beforeDestroy() {
		if (this.cleave) {
			this.cleave.destroy();
			this.inputEle.removeEventListener("input", this.inputChange);
		}
	}
};
</script>

<style lang="scss">
.vue-form-generator .field-cleave {
	.flex-view {
		display: flex;
		align-items: center;
	}
	.helper {
		margin-left: 3px;
	}
}
</style>
