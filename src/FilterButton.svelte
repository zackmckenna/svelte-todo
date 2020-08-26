<script>
  import { createEventDispatcher } from 'svelte';

  export let filterOption;
  export let currentFilter;

  const dispatch = createEventDispatcher();

  function updateFilter() {
    dispatch('updateFilter', {
      filterOption: filterOption
    });
  }

</script>

<style>
  button {
    font-size: 14px;
    background-color: white;
    appearance: none;
  }

  button:hover {
    background: lightseagreen;
    color: white;
  }

  button:focus {
    outline: none;
  }

  .active {
    background: lightskyblue;

  }
</style>

<button on:click={() => updateFilter()} class:active={filterOption === currentFilter}>{filterOption}</button>
