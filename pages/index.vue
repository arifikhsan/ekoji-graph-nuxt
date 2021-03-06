<template>
  <div
    class="flex flex-col max-w-3xl px-4 pt-4 pb-20 mx-auto space-y-6 leading-loose font-body"
  >
    <div class="py-4 text-center">
      <h1 class="text-4xl font-bold text-indigo-500 lg:text-5xl font-display">
        Ekoji Graph Challenge #3
      </h1>
    </div>
    <div>
      <h2 class="text-3xl font-bold text-indigo-500 lg:text-4xl font-display">
        Data Graph
      </h2>
      <div class="mt-4">
        <img src="~/assets/images/graph.png" />
      </div>
    </div>
    <div>
      <h2 class="text-3xl font-bold text-indigo-500 lg:text-4xl font-display">
        Temukan Rute Dari Dua Kota
      </h2>
      <form @submit.prevent="calculateDistance" class="max-w-xs mt-4">
        <div>
          <span class="text-gray-700">Pilih kota asal</span>
          <select
            v-model="startCity"
            class="block w-full mt-1 form-select"
            required
          >
            <option v-for="(value, key) in nodes" :key="key">{{ key }}</option>
          </select>
        </div>
        <div class="mt-3">
          <span class="text-gray-700">Pilih kota tujuan</span>
          <select
            v-model="endCity"
            class="block w-full mt-1 form-select"
            required
          >
            <option v-for="(value, key) in nodes" :key="key">{{ key }}</option>
          </select>
        </div>
        <div class="mt-4">
          <button
            class="px-4 py-2 font-semibold text-white transition duration-500 bg-indigo-500 rounded hover:bg-indigo-700"
          >
            Cari Rute Terdekat
          </button>
        </div>
      </form>
    </div>
    <div>
      <div>
        <h2 class="text-3xl font-bold text-indigo-500 lg:text-4xl font-display">
          Rute terdekat
        </h2>
        <div>
          <div class="flex flex-row items-center justify-start">
            <div class="w-6 h-6 bg-indigo-500 rounded"></div>
            <div class="ml-2">= Kota</div>
          </div>
          <div class="flex flex-row items-center justify-start">
            <div class="w-6 h-6 border border-indigo-500 rounded"></div>
            <div class="ml-2">= Jalan</div>
          </div>
        </div>
      </div>
      <div class="mt-4">
        <div v-if="done">
          <result-route :result="combineResult" type="pulang pergi" />
          <result-route class="mt-8" :result="startResult" type="berangkat" />
          <result-route class="mt-8" :result="endResult" type="pulang" />
        </div>
        <div v-else>
          Pilih kota asal dan kota tujuan, kemudian klik tombol
          <span class="text-indigo-500">Cari rute terdekat</span>!
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import ResultRoute from "~/components/result-route";
export default {
  data() {
    return {
      nodes: {
        a: 5,
        b: 7,
        c: 4,
        d: 8,
        e: 5,
        f: 4,
        g: 4,
        h: 3,
        i: 6,
        j: 7,
        k: 6,
        l: 9,
        m: 5,
        n: 7,
        o: 6
      },
      edges: {
        a: { b: 7, c: 7, h: 5 },
        b: { a: 7, c: 3, g: 2, d: 4, f: 5 },
        c: { a: 7, b: 3, i: 10, h: 9 },
        d: { b: 4, f: 7, e: 3, g: 11 },
        e: { j: 4, i: 8, g: 6, d: 3, f: 3, k: 8, l: 7 },
        f: { k: 9, e: 3, d: 7, b: 5 },
        g: { e: 6, i: 5, b: 2, d: 11 },
        h: { a: 5, c: 9, i: 8 },
        i: { h: 8, c: 10, g: 5, e: 8, j: 11, n: 2 },
        j: { e: 4, l: 5, m: 1, n: 4, i: 11 },
        k: { o: 2, l: 3, e: 8, f: 9 },
        l: { m: 2, j: 5, e: 7, k: 3, o: 5 },
        m: { n: 8, j: 1, l: 2, o: 3 },
        n: { i: 2, j: 4, m: 8 },
        o: { m: 3, l: 5, k: 2 }
      },
      startGraph: {},
      endGraph: {},
      startResult: {},
      endResult: {},
      combineResult: {},
      startCity: "a",
      endCity: "j",
      done: false
    };
  },
  components: {
    ResultRoute
  },
  created() {
    this.startGraph = { ...this.edges };
    this.endGraph = { ...this.edges };
  },
  methods: {
    reset() {
      this.startGraph = { ...this.edges };
      this.endGraph = { ...this.edges };
      this.startResult = {};
      this.endResult = {};
      this.combineResult = {};
      this.done = false;
    },
    shortestDistanceNode(distances, visited) {
      let shortest = null;
      for (let node in distances) {
        let currentIsShortest =
          shortest === null ||
          distances[node] < distances[shortest] + this.nodes[node];
        if (currentIsShortest && !visited.includes(node)) {
          shortest = node;
        }
      }
      return shortest;
    },
    findShortestPath(graph, startNode, endNode) {
      let distances = {};
      distances[endNode] = Infinity;
      distances = Object.assign(distances, graph[startNode]);

      let parents = { endNode: null };

      for (const child in graph[startNode]) {
        parents[child] = startNode;
      }

      let visited = [];
      let node = this.shortestDistanceNode(distances, visited);

      const increment = [];

      while (node) {
        let distance = distances[node];
        let children = graph[node];

        for (let child in children) {
          if (String(child) === String(startNode)) {
            continue;
          } else {
            let newDistance = distance + children[child];
            let newDistanceWithNode =
              distance + children[child] + this.nodes[child];

            if (!distances[child] || distances[child] > newDistanceWithNode) {
              distances[child] = newDistance;
              parents[child] = node;
            }
          }
        }

        visited.push(node);
        node = this.shortestDistanceNode(distances, visited);
      }

      let shortestPath = [endNode];
      let parent = parents[endNode];

      while (parent) {
        shortestPath.push(parent);
        parent = parents[parent];
      }

      shortestPath.reverse();

      let distanceNodes = 0;
      shortestPath.forEach(path => {
        distanceNodes += this.nodes[path];
      });

      for (let i = 0; i < shortestPath.length; i++) {
        increment.push({ type: "node", value: this.nodes[shortestPath[i]] });
        const edge = this.edges[shortestPath[i]][shortestPath[i + 1]];

        if (typeof edge === "number") {
          increment.push({ type: "edge", value: edge });
        }
      }

      let incrementCount = 0;
      increment.map(e => (incrementCount += e.value));

      let results = {
        distanceEdges: distances[endNode],
        distanceNodes,
        increment,
        incrementCount,
        totalDistance: distanceNodes + distances[endNode],
        path: shortestPath,
        from: shortestPath[0],
        to: shortestPath[shortestPath.length - 1]
      };

      return results;
    },
    calculateDistance() {
      this.reset();
      this.done = true;
      this.startResult = this.findShortestPath(
        this.startGraph,
        this.startCity,
        this.endCity
      );

      const visitedCities = [...this.startResult.path];
      visitedCities.pop();
      visitedCities.shift();

      visitedCities.forEach(usedNode => {
        delete this.endGraph[usedNode];
      });
      this.endResult = this.findShortestPath(
        this.endGraph,
        this.endCity,
        this.startCity
      );

      const combineResultPath = [
        ...this.startResult.path,
        ...this.endResult.path
      ];

      const combineResultIncrement = [
        ...this.startResult.increment,
        ...this.endResult.increment
      ];

      console.log("combineResultPath: ", combineResultPath);

      const combineDistance =
        this.startResult.totalDistance + this.endResult.totalDistance;

      this.combineResult = {
        distanceEdges:
          this.startResult.distanceEdges + this.endResult.distanceEdges,
        distanceNodes:
          this.startResult.distanceNodes + this.endResult.distanceNodes,
        increment: combineResultIncrement,
        incrementCount: combineDistance,
        totalDistance: combineDistance,
        path: combineResultPath,
        from: combineResultPath[0],
        to: combineResultPath[this.startResult.path.length - 1]
      };
    }
  },
  head: {
    title: "Ekoji Graph",
    meta: [
      {
        hid: "description",
        name: "description",
        content: "Ekoji Challenge 3"
      }
    ]
  }
};
</script>
