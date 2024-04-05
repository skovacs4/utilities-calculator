<script>
  import { onMount } from "svelte";

  let mc3 = 0;
  let selectedUtility = "water";
  let totalCost = 0;

  function calculateCost() {
    const prices = {
      water: {
        hot: 10.5, // lei/mc
        cold: 7.66, // lei/mc
      },
      electricity: [
        { maxKWh: 100, price: 0.68 }, // lei/kWh
        { maxKWh: 255, price: 0.8 }, // lei/kWh
        { maxKWh: 300, price: 1.3 }, // lei/kWh (maximum price)
      ],
      gas: 3.3, // lei/mc
    };

    let pricePerMc = 0;
    switch (selectedUtility) {
      case "water":
        // @ts-ignore
        pricePerMc =
          // @ts-ignore
          document.getElementById("waterType").value === "hot"
            ? prices.water.hot
            : prices.water.cold;
        break;
      case "electricity":
        const kWh = mc3; // initialize the kwh var
        for (const tier of prices.electricity) {
          if (kWh <= tier.maxKWh) {
            pricePerMc = tier.price;
            break;
          }
          else {
            pricePerMc = 1.3;
          }
        }
        break;
      case "gas":
        pricePerMc = prices.gas;
        break;
      default:
        break;
    }

    const vatPercentage = 0.19; // VAT percentage
    const totalCostWithoutVat = mc3 * pricePerMc;
    totalCost = totalCostWithoutVat * (1 + vatPercentage);
  }

  // Initialize calculation when component mounts
  onMount(() => {
    calculateCost();
  });
</script>

<main>
  <div class="bg-gray-100 min-h-screen flex justify-center items-center">
    <div
      class="w-full md:w-1/2 lg:w-1/3 xl:w-1/2 bg-white p-6 rounded-lg shadow-md flex flex-col "
    >
      <div class="items-center mx-auto text-center mb-4">
        <img src="src/lib/assets/logo-light.png" alt="Logo" width="250" height="250">
      </div>
      

      <h1 class="text-xl md:text-2xl lg:text-3xl font-bold text-center mb-4">
        
      </h1>
      <p class="text-xl md:text-xl lg:text-xl text-left mb-4" >
        Buna ziua, aici va puteti calcula costurile utilitatilor dumneavoastra.
      </p>

      <label for="utility" class="block mb-2">Selectati utilitatea:</label>
      <select
        id="utility"
        class="block w-full py-2 px-3 border border-gray-300 rounded-md mb-4"
        bind:value={selectedUtility}
      >
        <option value="water">Apa</option>
        <option value="electricity">Electricitate</option>
        <option value="gas">Gaz</option>
      </select>

      {#if selectedUtility === "water"}
        <label for="waterType" class="block mb-2">Selectati tipul de apa:</label
        >
        <select
          id="waterType"
          class="block w-full py-2 px-3 border border-gray-300 rounded-md mb-4"
        >
          <option value="cold">Apa rece</option>
          <option value="hot">Apa calda</option>
        </select>
      {/if}

      <label for="mc3" class="block mb-2"
        >Introduceti numarul de unitati utilizate:</label
      >
      <input
        type="number"
        id="mc3"
        class="block w-full py-2 px-3 border border-gray-300 rounded-md mb-4"
        bind:value={mc3}
      />

      <button
        on:click={calculateCost}
        class="py-2 px-4 bg-blue-500 text-white rounded hover:bg-blue-600 focus:outline-none focus:ring focus:ring-blue-500 focus:ring-opacity-50 mb-4"
        >Calculeaza costul</button
      >

      {#if totalCost > 0}
        <p class="text-gray-800">
          Costul total este: {totalCost.toFixed(2)} lei.
        </p>
      {/if}

      <!-- Disclaimer -->
      <p class="text-sm text-gray-600 mt-4 text-center">
        Rata de conversie va varia în funcție de utilitate. Această unealtă este doar în scopuri
        informative și nu este destinată să ofere sfaturi de la Nomadic Digital.
      </p>
    </div>
  </div>
</main>
