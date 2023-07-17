<script>
    import { createEventDispatcher } from 'svelte';
  
  const dispatch = createEventDispatcher();

  export let filters = { gender: '', building: '', floor: '' };

  $: {
      if(filters.gender === '') {
          filters.building = '';
          filters.floor = '';
      }
      if(filters.building === '') {
          filters.floor = '';
      }
      dispatch('filterChange', filters);
      //console.log("filters: ", filters);
  }
</script>

<div class="select-container">
    <label>
        Gender:
        <select bind:value={filters.gender}>
          <option value="">All</option>
          <option value="male">Male</option>
          <option value="female">Female</option>
        </select>
      </label>
      
      <label>
        Building:
        <select bind:value={filters.building}>
          <option value="">All</option>
          <option value="A">A</option>
          <option value="B">B</option>
          <option value="C">C</option>
        </select>
      </label>
      
      <label>
        Floor:
        <select bind:value={filters.floor}>
          <option value="">All</option>
          <option value="1">1</option>
          <option value="2">2</option>
          <option value="3">3</option>
          <option value="4">4</option>
        </select>
      </label>
</div>

<style>
    .select-container {
        display: flex;
        justify-content: space-evenly;
        margin-top: 20px;
        margin-bottom: 20px;
    }
    select {
    font-size: 16px;
    padding: 8px 10px;
    border: none;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.15);
    appearance: none; 
    -webkit-appearance: none; 
    -moz-appearance: none; 
    background: #fff;
    outline: none;
    }

    label {
    display: block;
    margin-bottom: 10px;
    }
</style>
