<template>
  <div>
    <h2 class="text-xl font-semibold text-indigo-500 font-display">
      Rute {{ type }} dari Kota {{ result.from.toUpperCase() }} ke Kota
      {{ result.to.toUpperCase() }}
    </h2>
    <div class="p-4">
      <ol class="list-decimal">
        <li>
          Jumlah biaya kota dan jalan:
          <span class="font-bold">{{ result.totalDistance }}</span> satuan
        </li>
        <li>
          <span class="flex flex-wrap justify-start">
            <span class="flex items-center justify-center h-6 mr-2"
              >Kota yang dilalui:</span
            >
            <span
              class="flex items-center justify-center w-6 h-6 px-2 py-1 mb-2 mr-2 text-white whitespace-normal bg-indigo-500 rounded"
              v-for="(city, index) in result.path"
              :key="index"
              >{{ city.toUpperCase() }}</span
            >
          </span>
        </li>
        <li>Jumlah biaya tiap kota: {{ result.distanceEdges }} satuan</li>
        <li>Jumlah biaya tiap jalan: {{ result.distanceNodes }} satuan</li>
        <li>
          <span class="flex flex-wrap justify-start">
            <span class="flex items-center justify-center h-6 mr-2"
              >Rincian biaya:</span
            >
            <span
              :class="[
                item.type === 'node'
                  ? 'text-white bg-indigo-500'
                  : 'border text-indigo-500 border-indigo-500'
              ]"
              class="flex items-center justify-center w-6 h-6 px-2 py-1 mb-2 mr-2 rounded"
              v-for="(item, index) in result.increment"
              :key="index"
              >{{ item.value }}</span
            >
          </span>
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
