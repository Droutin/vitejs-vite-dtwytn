<script lang="ts">
interface Props {
  modelValue: string | number | boolean;
  label: string;
  id?: string;
  type?: string;
}
</script>
<script setup lang="ts">
import { v4 as uuidv4 } from "uuid";
import { ref, watch } from "vue";
const props = withDefaults(defineProps<Props>(), {
  id: undefined,
  type: "text",
});
const emits = defineEmits(["update:modelValue"]);

const innerValue = ref(props.modelValue);
const innerId = props.id || "input" + uuidv4();

watch(
  () => props.modelValue,
  (val) => {
    innerValue.value = val;
  }
);

const changeEvent = (e: Event) => {
  const target = e.target as HTMLInputElement;
  emits("update:modelValue", target.value);
};
</script>

<template>
  <div class="input-wrapper">
    <input :id="innerId" v-model="innerValue" :type="type" required pattern=".*\S.*" @keyup="changeEvent" />
    <label :for="innerId">{{ label }}</label>
    <div class="input-border-space">{{ label }}</div>
    <div class="input-border" />
  </div>
</template>

<style scoped lang="scss">
.input-wrapper {
  margin-top: 6px;
  background-color: $content-color;
  position: relative;
  border-radius: 8px;
  &:hover {
    & .input-border {
      border-color: #fff;
    }
  }
}
.input-border {
  border: 2px solid $border-color;
  border-radius: 8px;
  position: absolute;
  background-color: transparent;
  z-index: 1;
  top: 0px;
  bottom: 0;
  left: 0;
  right: 0;
}
.label-wrapper {
  overflow: hidden;
}
label {
  position: absolute;
  top: 7px;
  z-index: 3;
  left: 16px;
  color: $muted;
  transition: 100ms ease-in-out;
  white-space: nowrap;
}
.input-border-space {
  position: absolute;
  z-index: 2;
  top: 0;
  left: 12px;
  height: 1px;
  opacity: 0;
  padding: 0 5px;
  background-color: $content-color;
  color: transparent;
  @include font-size(14);
}
input::file-selector-button {
  display: none;
}
input:not([type="checkbox"]):not([type="radio"]) {
  display: block;
  width: 100%;
  background-color: transparent;
  position: relative;
  z-index: 3;
  padding: 7px 16px;
  appearance: none;
  &:focus {
    & ~ label {
      color: $primary;
      font-weight: 500;
    }
    & ~ .input-border {
      border-width: 2px;
      border-color: $primary !important;
      transition: 50ms ease-in-out;
    }
  }
  &:focus,
  &:valid,
  &[type="file"] {
    & ~ label {
      top: 0;
      transform: translateY(-50%);
      @include font-size(14);
    }
    & ~ .input-border-space {
      opacity: 1;
      height: 2px;
    }
  }
  &:hover,
  &:focus,
  &:active {
    outline: none;
  }
}
</style>
