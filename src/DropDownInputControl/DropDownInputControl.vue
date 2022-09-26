<script setup lang="ts">
import type * as Rete from "rete";
import type { EventsTypes } from "rete/types/events";
import { defineComponent } from "vue";

export interface Props {
  ikey: string;
  options?: { text: string; value: string }[];
  reteEmitter?: Rete.Emitter<EventsTypes> | undefined;
  reteGetData?: (ikey: string) => number;
  retePutData?: (ikey: string, value: number) => void;
}

const props = withDefaults(defineProps<Props>(), {
  options: () => [
    { text: "x_dimension", value: "x_dimension" },
    { text: "y_dimension", value: "y_dimension" },
    { text: "z_dimension", value: "z_dimension" },
  ],
  reteGetData: (ikey: string) => 0,
  retePutData: (ikey: string, value: number) => {
    return;
  },
  reteEmitter: undefined,
});

const emits = defineEmits([]);
</script>

<script lang="ts">
export default defineComponent({
  data() {
    return {
      currentValue: undefined,
    };
  },
  methods: {
    change(e: Event) {
      this.update();
    },
    update() {
      if (this.ikey) {
        this.retePutData?.(this.ikey, this.currentValue);
      }
      this.reteEmitter?.trigger("process");
    },
    mounted() {
      this.currentValue = this.options[0];
      if (this.ikey && this.reteGetData)
        this.currentValue = this.reteGetData(this.ikey);
    },
  },
});
</script>

<template>
  <select v-model="currentValue" @input="change($event)">
    <option v-for="option in options" :key="option.value" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</template>
