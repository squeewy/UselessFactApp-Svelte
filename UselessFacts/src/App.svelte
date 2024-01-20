<script>
// @ts-nocheck

  import { onMount } from 'svelte';
  import Fact from './Fact.svelte';

  let fact = '';

  const fetchNewFact = async () => {
    try{
      const response = await fetch('https://useless-facts.sameerkumar.website/api');
      if (!response.ok){
        throw new Error('Failed to fetch: ${response.statusText');
      }
      const data = await response.json();
      fact = data.text;
      console.log('Fact fetched:', fact)
    } catch (error) {
      console.error('Error fetching fact:', error);
    }
  };

  onMount(() => {
    fetchNewFact();
  });
</script>

<main>
  <h1>Useless Fact App</h1>
  <button on:click={fetchNewFact}>Get New Fact</button>
  <Fact text={fact} />
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
  }

  button {
    margin-top: 10px;
    padding: 8px 16px;
    font-size: 16px;
    cursor: pointer;
  }
</style>
