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

<button on:click={() => updateFilter()} class:active={filterOption === currentFilter}>{filterOption}</button>
