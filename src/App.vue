<template>
  <div class="converter p-6 max-w-md mx-auto bg-white rounded-xl shadow-md space-y-4">
    <div class="flex w-full justify-between">
      <select v-model="selectedEra" @change="calculateYear" class="w-full p-2 border border-gray-300 rounded-md">
        <option value="gregorian">西暦</option>
        <option value="kouki">皇紀</option>
        <option value="showa">昭和</option>
        <option value="heisei">平成</option>
        <option value="reiwa">令和</option>
      </select>

      <input type="number" v-model.number="inputYear" placeholder="年を入力" @input="calculateYear"
        class="w-full p-2 border border-gray-300 rounded-md" />
    </div>
    <div class="output text-lg text-blue-600 font-bold">
      <p>変換結果: {{ result }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'App',
  setup() {
    const selectedEra = ref<string>('gregorian');
    const inputYear = ref<number | null>(null);
    const result = ref<string>('');

    const calculateYear = () => {
      let year = 0;

      switch (selectedEra.value) {
        case 'showa':
          if (inputYear.value !== null) {
            year = inputYear.value + 1925;
            result.value = `昭和${inputYear.value}年は西暦${year}年です`;
          }
          break;
        case 'heisei':
          if (inputYear.value !== null) {
            year = inputYear.value + 1988;
            result.value = `平成${inputYear.value}年は西暦${year}年です`;
          }
          break;
        case 'reiwa':
          if (inputYear.value !== null) {
            year = inputYear.value + 2018;
            result.value = `令和${inputYear.value}年は西暦${year}年です`;
          }
          break;
          case 'kouki':
          if (inputYear.value !== null) {
            year = inputYear.value - 660;
            result.value = `皇紀${inputYear.value}年は西暦${year}年です`;
          }
          break;
        default:
          if (inputYear.value !== null) {
            result.value = `西暦${inputYear.value}年です`;
          }
      }
    };

    return {
      selectedEra,
      inputYear,
      result,
      calculateYear
    };
  }
});
</script>

<style scoped>
.converter {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 0.5rem;
}

.output {
  margin-top: 1rem;
}
</style>
