<script>
    import ShowerStatus from './ShowerStatus.svelte';
    import ShowerRoomFilter from './ShowerRoomFilter.svelte';
    
    let eventSource;
    let showerrooms = [];
    let filters = {gender: '', building: '', floor: ''};

    const setupEventSource = () => {
    if(eventSource) {
        eventSource.close();
    }

    eventSource = new EventSource("http://localhost:8081/events");

    eventSource.onmessage = (event) => {
        console.log(event.data);

        const [eventGender, eventBuilding, eventFloor] = event.data.split("/");

        const genderMatches = filters.gender === "" || filters.gender === eventGender;
        const buildingMatches = filters.building === "" || filters.building === eventBuilding;
        const floorMatches = filters.floor === "" || filters.floor === eventFloor;

        if (genderMatches && buildingMatches && floorMatches) {
            fetchShowerrooms(filters);
        }
    };

    }

    const formatShowerrooms = (rawShowerrooms) => {
    // Initialize the 2D array
        let formatted = Array(4).fill().map(() => Array(3).fill().map(() => ({male: null, female: null})));

        for(let showerroom of rawShowerrooms) {
            const buildingIndex = ['A', 'B', 'C'].indexOf(showerroom.building);
            const floorIndex = parseInt(showerroom.floor) - 1;
            formatted[floorIndex][buildingIndex][showerroom.gender] = showerroom;
        }
    
        return formatted;
    }

  
    const fetchShowerrooms = async (filters) => {
        console.log("filters: ", filters);
        let url = 'http://localhost:8081';
        if (filters.gender) url += `/${filters.gender}`;
        if (filters.building) url += `/${filters.building}`;
        if (filters.floor) url += `/${filters.floor}`;
        url += '/showerrooms';
        console.log(url);

        const response = await fetch(url);
        console.log(response);
        if (response.ok) {
            let rawShowerrooms = await response.json();
            showerrooms = formatShowerrooms(rawShowerrooms);
            console.log(showerrooms);
        } else {
            console.log('HTTP error, status = ' + response.status);
        }
    };

    const handleFilterChange = (newFilters) => {
    // Check if the new filters are different from the current filters
        if (JSON.stringify(newFilters.detail) !== JSON.stringify(filters)) {
            filters = {...newFilters.detail};
            console.log("Received new filters:", filters);
            fetchShowerrooms(filters);
        }
    };

    // Initial fetch
    $: {
        fetchShowerrooms(filters);
        setupEventSource();
    }
</script>

<div>
    <ShowerRoomFilter on:filterChange={handleFilterChange} />
    <table>
        <thead>
            <tr>
                <th></th>
                <th>A</th>
                <th>B</th>
                <th>C</th>
            </tr>
        </thead>
        <tbody>
            {#each showerrooms as row, i}
            <tr>
                <th>{i + 1}</th>
                {#each row as cell}
                <td>
                    {#if cell.male}
                    <div class="showerroom">
                        <h4>Male</h4>
                        <ShowerStatus status={{available: cell.male.available, occupied: cell.male.occupied, disabled_rooms: cell.male.disabled, total: cell.male.total}} />
                    </div>
                    {/if}
                    {#if cell.female}
                    <div class="showerroom">
                        <h4>Female</h4>
                        <ShowerStatus status={{available: cell.female.available, occupied: cell.female.occupied, disabled_rooms: cell.female.disabled, total: cell.female.total}} />
                    </div>
                    {/if}
                </td>
                {/each}
            </tr>
            {/each}
        </tbody>
    </table>
</div>

<style>
    table {
        width: 100%;
        border-collapse: collapse;
    }
    
    th, td {
        border: 1px solid #ddd;
        text-align: center;
        padding: 8px;
    }
    
    .showerroom {
        margin: 8px 0;
    }
    </style>
