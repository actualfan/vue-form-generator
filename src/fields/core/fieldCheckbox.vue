<template lang="pug">
	checkbox(:id="getFieldID(schema)", v-model="value", :autocomplete="schema.autocomplete", :disabled="disabled", :name="schema.inputName", :class="schema.fieldClasses", :required="schema.required") {{schema.label}}
</template>

<script>
import abstractField from "../abstractField";
import checkbox from "../../utils/MaterialCheckbox/components/material-checkbox.vue";

export default {
	mixins: [abstractField],
	components: {
		checkbox
	}
};
</script>

<style lang="scss">
.vue-form-generator .field-checkbox input {
	margin-left: 12px;
}
</style>
