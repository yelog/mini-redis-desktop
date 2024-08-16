<script lang="ts">
  import { fade } from "svelte/transition";
  let showModal = false;

  let redisMode = "standalone"; // 默认为单机模式

  let redisConfig = {
    name: "",
    host: "",
    port: "",
    password: "",
    clusterNodes: [""],
  };
  function openModal() {
    showModal = true;
  }

  function closeModal() {
    showModal = false;
  }

  function addClusterNode() {
    redisConfig.clusterNodes.push("");
  }

  function removeClusterNode(index) {
    redisConfig.clusterNodes.splice(index, 1);
  }

  function submitConfig() {
    console.log("Submitted Redis Config:", redisConfig);
    closeModal();
  }
</script>

<div class="flex flex-row h-full">
  <div class="w-1/3 bg-blue-100 h-full">
    <div class="h-12 bg-gray-200 p-1">
      <button
        on:click={() => (showModal = true)}
        class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded-lg shadow-lg focus:outline-none focus:ring-2 focus:ring-blue-400 focus:ring-opacity-75"
      >
        Add redis
      </button>
    </div>
    <div class="h-12 bg-amber-100 flex felx-col justify-between items-center">
      <div class="p-2">lssc-uat</div>
      <div class="p-2">lssc-uat</div>
    </div>
  </div>
  <div class="flex-1 bg-red-100">2</div>
</div>
{#if showModal}
  <div
    class="fixed inset-0 flex items-center justify-center bg-black bg-opacity-50"
  >
    <div
      class="bg-white rounded-lg shadow-lg transform transition-transform duration-300 ease-out scale-90"
      transition:fade={{ duration: 300 }}
      style="animation: scaleUp 0.3s ease-out forwards;"
    >
      <div class="px-8 py-6">
        <h2 class="text-2xl font-bold text-gray-800">Redis Configuration</h2>

        <div class="mt-4">
          <label class="block text-gray-700">Redis Mode</label>
          <select bind:value={redisMode} class="mt-2 p-2 border rounded">
            <option value="standalone">Standalone</option>
            <option value="cluster">Cluster</option>
          </select>
        </div>

        {#if redisMode === "standalone"}
          <!-- 单机模式配置 -->
          <div class="mt-4">
            <label class="block text-gray-700">Host</label>
            <input
              type="text"
              bind:value={redisConfig.host}
              class="mt-2 p-2 border rounded w-full"
              placeholder="127.0.0.1"
            />
          </div>
          <div class="mt-4">
            <label class="block text-gray-700">Port</label>
            <input
              type="text"
              bind:value={redisConfig.port}
              class="mt-2 p-2 border rounded w-full"
              placeholder="6379"
            />
          </div>
          <div class="mt-4">
            <label class="block text-gray-700">Password</label>
            <input
              type="password"
              bind:value={redisConfig.password}
              class="mt-2 p-2 border rounded w-full"
            />
          </div>
        {/if}

        {#if redisMode === "cluster"}
          <!-- 集群模式配置 -->
          <div class="mt-4">
            {#each redisConfig.clusterNodes as node, index}
              <div class="flex items-center mt-2">
                <input
                  type="text"
                  bind:value={redisConfig.clusterNodes[index]}
                  class="p-2 border rounded w-full mr-2"
                />
                <button
                  on:click={() => removeClusterNode(index)}
                  class="bg-red-500 hover:bg-red-600 text-white font-bold py-1 px-2 rounded"
                >
                  Remove
                </button>
              </div>
            {/each}
            <button
              on:click={addClusterNode}
              class="mt-4 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-4 rounded"
            >
              Add Node
            </button>
          </div>
        {/if}
      </div>
      <div class="px-8 py-4 bg-gray-200 text-right">
        <button
          class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded mr-2"
          on:click={closeModal}
        >
          Cancel
        </button>
        <button
          class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-4 rounded"
          on:click={submitConfig}
        >
          Save
        </button>
      </div>
    </div>
  </div>
{/if}

<style>
  :root {
    font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
    font-size: 16px;
    line-height: 24px;
    font-weight: 400;

    color: #0f0f0f;
    background-color: #f6f6f6;

    font-synthesis: none;
    text-rendering: optimizeLegibility;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    -webkit-text-size-adjust: 100%;
    height: 100%;
  }
  @keyframes scaleUp {
    from {
      transform: scale(0.9);
    }
    to {
      transform: scale(1);
    }
  }
</style>
