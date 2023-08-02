<script>
  import {onMount} from 'svelte';
  let peopleList;
  let query = '';
  let isError;
  $: filteredList = !query 
                  ? peopleList 
                    : peopleList?.filter((people) => (new RegExp  (query, 'i')).test(people.name))

  onMount(async () => {
    try {
      const response = await fetch('https://swapi.dev/api/people')
      const data = await response.json();
      peopleList = data.results;
    } catch {
      isError = true
    }
  })

</script>
<header>
  <h1>People Finder</h1>
</header>

<main>
<div class="searchbar">
  <label for="search">Search person</label>
  <input type="text" bind:value={query}>
</div>

<div>
{#if peopleList === undefined}
<p>Fetching list...</p>
{:else if isError}
<p>Error occured, try again later!</p>
{:else}
{#each filteredList as people (people)}
<div class="card">
<h3>{people.name}</h3>
<p>Gender: {people.gender}</p>
</div>
{/each}
{/if}
</div>

</main>

<style>
.searchbar {
  display: flex;
  gap:0.5em;
  flex-direction: column;
}
.card {
  border: 1px solid white;
  margin-top: 1em;
  padding: 1em;
  border-radius: 5px;}
</style>