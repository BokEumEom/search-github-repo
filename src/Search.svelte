<script>
  export let onSearch
  let query = ''
  
  function handleSubmit(event) {
    event.preventDefault()
    onSearch(query)
  }
</script>

<form on:submit={handleSubmit}>
  <label for="query">Search:</label>
  <input type="text" id="query" bind:value={query}>
  <button type="submit">Submit</button>
</form>
