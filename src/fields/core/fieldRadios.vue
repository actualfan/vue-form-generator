<template lang="pug">
	.radio-list(:disabled="disabled", v-attributes="'wrapper'")
		radio(v-for="item in items", :key="id + getItemValue(item)", v-model="value", :name="id", :text="getItemName(item)", :label="getItemValue(item)", :disabled="disabled", :required="schema.required", :class="schema.fieldClasses")

</template>

<script>
import { isObject } from "lodash";
import abstractField from "../abstractField";
import Radio from "../../utils/Radio/Radio.vue";

export default {
	mixins: [abstractField],

	components: {
		Radio
	},

	computed: {
		items() {
			let values = this.schema.values;
			if (typeof values == "function") {
				return values.apply(this, [this.model, this.schema]);
			} else {
				return values;
			}
		},
		id() {
			return this.schema.model;
		}
	},

	methods: {
		getItemValue(item) {
			if (isObject(item)) {
				if (typeof this.schema["radiosOptions"] !== "undefined" && typeof this.schema["radiosOptions"]["value"] !== "undefined") {
					return item[this.schema.radiosOptions.value];
				} else {
					if (typeof item["value"] !== "undefined") {
						return item.value;
					} else {
						throw "`value` is not defined. If you want to use another key name, add a `value` property under `radiosOptions` in the schema. https://icebob.gitbooks.io/vueformgenerator/content/fields/radios.html#radios-field-with-object-values";
					}
				}
			} else {
				return item;
			}
		},
		getItemName(item) {
			if (isObject(item)) {
				if (typeof this.schema["radiosOptions"] !== "undefined" && typeof this.schema["radiosOptions"]["name"] !== "undefined") {
					return item[this.schema.radiosOptions.name];
				} else {
					if (typeof item["name"] !== "undefined") {
						return item.name;
					} else {
						throw "`name` is not defined. If you want to use another key name, add a `name` property under `radiosOptions` in the schema. https://icebob.gitbooks.io/vueformgenerator/content/fields/radios.html#radios-field-with-object-values";
					}
				}
			} else {
				return item;
			}
		},
		onSelection(item) {
			this.value = this.getItemValue(item);
		},
		isItemChecked(item) {
			let currentValue = this.getItemValue(item);
			return currentValue === this.value;
		}
	}
};
</script>

<style lang="scss">
.vue-form-generator .field-radios {
	.radio-list {
		label {
			display: block;
			input[type="radio"] {
				margin-right: 5px;
			}
		}
	}
}
</style>
