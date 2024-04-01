<script>
    import { onMount } from 'svelte';
  
    let mc3 = 0;
    let selectedUtility = 'water';
    let totalCost = 0;
  
    function calculateCost() {
      const prices = {
        water: {
          hot: 10.50, // lei/mc
          cold: 7.66 // lei/mc
        },
        electricity: [
          { maxKWh: 100, price: 0.68 }, // lei/kWh
          { maxKWh: 255, price: 0.8 }, // lei/kWh
          { maxKWh: 300, price: 1.3 } // lei/kWh (maximum price)
        ],
        gas: 3.3 // lei/mc
      };
  
      let pricePerMc = 0;
      switch (selectedUtility) {
        case 'water':
          pricePerMc = document.getElementById('waterType').value === 'hot' ? prices.water.hot : prices.water.cold;
          break;
        case 'electricity':
          const kWh = mc3; // initialize the kwh var
          for (const tier of prices.electricity) {
            if (kWh <= tier.maxKWh) {
              pricePerMc = tier.price;
              break;
            }
          }
          break;
        case 'gas':
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
  
  <h1>Buna ziua, aici va puteti calcula costurile utilitatilor dumneavoastra in functie de zona.</h1>
  
  <label for="utility">Selectati utilitatea:</label>
  <select id="utility" bind:value={selectedUtility}>
    <option value="water">Apa</option>
    <option value="electricity">Electricitate</option>
    <option value="gas">Gaz</option>
  </select>
  
  {#if selectedUtility === 'water'}
    <label for="waterType">Selectati tipul de apa:</label>
    <select id="waterType">
      <option value="cold">Apa rece</option>
      <option value="hot">Apa calda</option>
    </select>
  {/if}
  
  <label for="mc3">Introduceti numarul de unitati utilizate:</label>
  <input type="number" id="mc3" bind:value={mc3}>
  
  <button on:click={calculateCost}>Calculeaza costul</button>
  
  {#if totalCost > 0}
    <p>Costul total este: {totalCost.toFixed(2)} lei.</p>
  {/if}
  