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
	<p class="loading">Loading...</p>
{:else if results.length === 0}
	<p>No results found.</p>
{:else}
	<ul>
		{#each results as result}
			<h2>
				<li>
					<a href={result.html_url}>{result.full_name}</a>
					<p class="meta">{result.description}</p>
				</li>
			</h2>
		{/each}
	</ul>
{/if}

<style>
	a {
		padding: 1em;
		display: block;
	}

	.loading {
		opacity: 0;
		animation: 0.4s 0.8s forwards fade-in;
	}

	@keyframes fade-in {
		from { opacity: 0; }
		to { opacity: 1; }
	}

	li {
		position: relative;
		padding: 0 0 0 2em;
		border-bottom: 1px solid #eee;
	}

	h2 {
		font-size: 1em;
		margin: 0.5em 0;
	}

	.meta {
		color: #999;
	}
</style>