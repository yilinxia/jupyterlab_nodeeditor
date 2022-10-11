<script setup lang="ts">
import type * as Rete from "rete";
import type { EventsTypes } from "rete/types/events";
import { defineComponent } from "vue";

export interface Props {
  initialValue: number;
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
      currentValue: this.initialValue === undefined ? 0 : this.initialValue,
    };
  },
  methods: {
    change(e: Event) {
      this.currentValue = +(e.target as HTMLInputElement).value;
      this.update();
    },
    update() {
      if (this.ikey) {
        this.retePutData?.(this.ikey, this.currentValue);
      }
      this.reteEmitter?.trigger("process");
    },
    mounted() {
      this.currentValue = 0;
      if (this.ikey && this.reteGetData)
        this.currentValue = this.reteGetData(this.ikey);
    },
  },
});
</script>
<template>
  <input
    type="number"
    @input="change($event)"
    @mousedown.stop
    v-model="currentValue"
  />
</template>
