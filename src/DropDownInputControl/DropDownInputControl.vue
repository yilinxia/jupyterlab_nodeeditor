<script setup lang="ts">
import type * as Rete from "rete";
import type { EventsTypes } from "rete/types/events";
import { defineComponent } from "vue";

export interface Props {
  initialValue: string;
  ikey: string;
  reteEmitter?: Rete.Emitter<EventsTypes> | undefined;
  reteGetData?: (ikey: string) => number;
  retePutData?: (ikey: string, value: number) => void;
}

const props = withDefaults(defineProps<Props>(), {
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
      currentValue: "x_dimension",
      dimensions: [
        { text: "i_dimension", value: "i_dimension" },
        { text: "j_dimension", value: "j_dimension" },
        { text: "k_dimension", value: "k_dimension" },
      ],
    };
  },
  methods: {
    change(e: Event) {
      this.currentValue = (e.target as HTMLSelectElement).value;
      this.update();
    },
    update() {
      if (this.ikey) {
        this.retePutData?.(this.ikey, this.currentValue);
      }
      this.reteEmitter?.trigger("process");
    },
    mounted() {
      this.currentValue = "x_dimension";
      if (this.ikey && this.reteGetData)
        this.currentValue = this.reteGetData(this.ikey);
    },
  },
});
</script>

<template>
  <select v-model="currentValue" @input="change($event)">
    <option v-for="option in dimensions" :value="option.value">
      {{ option.text }}
    </option>
  </select>
</template>
