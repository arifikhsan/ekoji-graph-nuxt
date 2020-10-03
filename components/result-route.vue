<template>
  <div>
    <h2 class="font-semibold text-2xl text-indigo-500 font-display">
      Rute {{ type }} dari Kota {{ result.from.toUpperCase() }} ke Kota
      {{ result.to.toUpperCase() }}
    </h2>
    <div class="px-4">
      <ol class="list-decimal">
        <li>
          Jumlah biaya kota dan jalan:
          <span class="font-bold">{{ result.totalDistance }}</span> satuan
        </li>
        <li>
          Kota yang dilalui:
          <span
            class="text-white ml-2 px-2 bg-indigo-500 py-1 rounded"
            v-for="(city, index) in result.path"
            :key="index"
            >{{ city.toUpperCase() }}</span
          >
        </li>
        <li>Jumlah biaya tiap kota: {{ result.distanceEdges }} satuan</li>
        <li>Jumlah biaya tiap jalan: {{ result.distanceNodes }} satuan</li>
        <li>
          Rincian biaya:
          <span
            :class="[
              item.type === 'node'
                ? 'text-white bg-indigo-500'
                : 'border text-indigo-500 border-indigo-500'
            ]"
            class="ml-2 px-2 py-1 rounded"
            v-for="(item, index) in result.increment"
            :key="index"
            >{{ item.value }}</span
          >
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
export default {
  props: ["result", "type"]
};
</script>
