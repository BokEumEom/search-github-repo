<script>
  import { onMount } from 'svelte'
  import Search from './Search.svelte'

  let results = []
  let isLoading = false
  
  async function search(query) {
    isLoading = true
    const response = await fetch(`https://api.github.com/search/repositories?q=${query}`)
    const data = await response.json()
    results = data.items
    isLoading = false
  }
  
  onMount(() => {
    // initialize with a default search query
    search('svelte')
  })
</script>

<Search onSearch={search} />

{#if isLoading}
  <p>Loading...</p>
{:else if results.length === 0}
  <p>No results found.</p>
{:else}
  <ul>
    {#each results as result}
      <li><a href={result.html_url}>{result.full_name}</a></li>
    {/each}
  </ul>
{/if}
