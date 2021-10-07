<template>
  <b-field :label="fieldLabel">
    <b-select
      :name="selectName"
      placeholder="Seçiniz"
      :id="selectName"
      :value="value"
      @input="$emit('input', $event)"
    >
      <option
        v-for="option in options"
        :value="option.optionValue"
        :key="option.id"
      >
        {{ option.optionText }}
      </option>
    </b-select>
  </b-field>
</template>

<script>
export default {
  name: "Select",
  props: {
    selectName: {
      type: String,
      required: true,
    },
    fieldLabel: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      required: true,
    },
    value: {
      required: true,
    },
  },
};
</script>
