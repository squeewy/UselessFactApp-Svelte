<script>
// @ts-nocheck

  import { onMount } from 'svelte';
  import Fact from './Fact.svelte';
  import Cat from './Cat.svelte';
  

  let fact = '';
  let catImage ='';

  const fetchNewFact = async () => {
    try{
      const response = await fetch('https://uselessfacts.jsph.pl/api/v2/facts/random');
      if (!response.ok){
        throw new Error('Failed to fetch: ${response.statusText');
      }
      const data = await response.json();
      console.log(data);
      fact = data.text;
      console.log(fact);
      console.log('Fact fetched:', fact)
    } catch (error) {
      console.error('Error fetching fact:', error);
    }
  };

  const fetchCatImage = async() => {
    try{
      const response = await fetch('https://api.thecatapi.com/v1/images/search');
      if (!response.ok){
        throw new Error(`Failed to fetch cat image: ${response.status}`);
      }   
      const data = await response.json();
      catImage = data[0].url;
      console.log('Cat image fetched:',catImage);
    } catch (error){
      console.error('Error fetching cat image: ', error);
    }
  };

  onMount(() => {
    fetchNewFact();
  });
  const getCatImage = async () => {
    await fetchCatImage();
  };

</script>

<main>
  <h1>Useless Facts And Cat Pictures</h1>
  <button on:click={fetchNewFact}>Get New Fact</button>
  <Fact {fact} />
  <button on:click={getCatImage}>Or get a cat picture</button>
  {#if catImage}
    <div>
      <Cat src = {catImage}/>
    </div>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 20px;
  }

  button {
    margin-top: 10px;
    padding: 8px 16px;
    font-size: 20px;
    cursor: pointer;
  }
  
</style>
