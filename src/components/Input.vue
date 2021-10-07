<template>
  <b-field :label="fieldLabel">
    <b-input
      :value="value"
      @input="$emit('input', $event)"
      :type="inputType"
      :name="inputName"
      :id="inputName"
      :maxlength="inputType == 'textarea' ? 200 : null"
    ></b-input>
  </b-field>
</template>

<script>
export default {
  name: "Input",
  props: {
    inputType: {
      type: String,
      required: true,
      default: "text",
    },
    inputName: {
      type: String,
      required: true,
    },
    fieldLabel: {
      type: String,
      required: true,
    },
    value: {
      required: true,
    },
  },
};
</script>
