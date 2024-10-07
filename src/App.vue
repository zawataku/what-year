<template>
  <div class="flex flex-col items-center justify-center min-h-screen p-6 bg-gray-200">
    <div class="w-full max-w-2xl p-8 bg-white shadow-lg rounded-xl flex relative flex-col items-start gap-2">
      <h1 class="font-bold text-2xl mx-auto pt-2 pb-3">今は何年？ 計算ツール</h1>
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
      
      <div class="mt-4 text-lg font-bold">
        <table class="table-auto w-full text-blue-600">
          <tbody>
            <tr>
              <td class="px-4 py-2 border">西暦</td>
              <td class="px-4 py-2 border">{{ result.gregorian }}年</td>
            </tr>
            <tr>
              <td class="px-4 py-2 border">昭和</td>
              <td class="px-4 py-2 border">{{ result.showa }}</td>
            </tr>
            <tr>
              <td class="px-4 py-2 border">平成</td>
              <td class="px-4 py-2 border">{{ result.heisei }}</td>
            </tr>
            <tr>
              <td class="px-4 py-2 border">令和</td>
              <td class="px-4 py-2 border">{{ result.reiwa }}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';

export default defineComponent({
  name: 'App',
  setup() {
    const selectedEra = ref<string>('gregorian');
    const inputYear = ref<number | null>(null);
    const result = ref({
      gregorian: '',
      showa: '',
      heisei: '',
      reiwa: ''
    });

    const calculateYear = () => {
      if (inputYear.value === null) {
        result.value = { gregorian: '', showa: '', heisei: '', reiwa: '' };
        return;
      }

      let gregorianYear;

      switch (selectedEra.value) {
        case 'showa':
          gregorianYear = inputYear.value + 1925;
          result.value = {
            gregorian: gregorianYear.toString(),
            showa: `昭和${inputYear.value}年`,
            heisei: gregorianYear >= 1989 ? `平成${gregorianYear - 1988}年` : '-',
            reiwa: gregorianYear >= 2019 ? `令和${gregorianYear - 2018}年` : '-'
          };
          break;
        case 'heisei':
          gregorianYear = inputYear.value + 1988;
          result.value = {
            gregorian: gregorianYear.toString(),
            showa: gregorianYear >= 1926 && gregorianYear < 1989 ? `昭和${gregorianYear - 1925}年` : '-',
            heisei: `平成${inputYear.value}年`,
            reiwa: gregorianYear >= 2019 ? `令和${gregorianYear - 2018}年` : '-'
          };
          break;
        case 'reiwa':
          gregorianYear = inputYear.value + 2018;
          result.value = {
            gregorian: gregorianYear.toString(),
            showa: gregorianYear >= 1926 && gregorianYear < 1989 ? `昭和${gregorianYear - 1925}年` : '-',
            heisei: gregorianYear >= 1989 && gregorianYear < 2019 ? `平成${gregorianYear - 1988}年` : '-',
            reiwa: `令和${inputYear.value}年`
          };
          break;
        case 'kouki':
          gregorianYear = inputYear.value - 660;
          result.value = {
            gregorian: gregorianYear.toString(),
            showa: gregorianYear >= 1926 && gregorianYear < 1989 ? `昭和${gregorianYear - 1925}年` : '-',
            heisei: gregorianYear >= 1989 && gregorianYear < 2019 ? `平成${gregorianYear - 1988}年` : '-',
            reiwa: gregorianYear >= 2019 ? `令和${gregorianYear - 2018}年` : '-'
          };
          break;
        default:
          gregorianYear = inputYear.value;
          result.value = {
            gregorian: gregorianYear.toString(),
            showa: gregorianYear >= 1926 && gregorianYear < 1989 ? `昭和${gregorianYear - 1925}年` : '-',
            heisei: gregorianYear >= 1989 && gregorianYear < 2019 ? `平成${gregorianYear - 1988}年` : '-',
            reiwa: gregorianYear >= 2019 ? `令和${gregorianYear - 2018}年` : '-'
          };
      }
    };

    watch([selectedEra, inputYear], calculateYear);

    return {
      selectedEra,
      inputYear,
      result
    };
  }
});
</script>