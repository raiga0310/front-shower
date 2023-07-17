<script>
    import available from "../assets/available.png";
    import occupied from "../assets/occupied.png";
    import disabled from "../assets/disabled.png";
    export let status = { available: 10, occupied: 0, disabled: 0, total: 10 };
  
    let bgColor = "white";
  
    $: {
        // Determine background color
        if (status.available <= 0) {
            bgColor = "red";
        } else if (status.available / status.total <= 0.4) {
            bgColor = "yellow";
        } else {
            bgColor = "white";
        }
    }
  </script>
  
  <div class="shower-status" style="background-color: {bgColor}">
    <div>
      <span class="available-icon"><img src={available} alt="available"/>: {status.available}</span>
      <span class="occupied-icon"><img src={occupied} alt="occupied"/>: {status.occupied}</span>
      <span class="disabled-icon"><img src={disabled} alt="disabled"/>: {status.disabled ?? 0}</span>
    </div>
  </div>
  
  <style>
    .shower-status {
        display: flex;
        justify-content: center;
        padding: 1em;
    }
    span {
        margin: 0.5em;
    }
    .available-icon {
        color: #00ff88;
        display: inline-block;
        width: 40px;
        background-size: contain;
    }
    .occupied-icon {
        color: rgb(255, 196, 0);
        display: inline-block;
        width: 40px;
        background-size: contain;
    }
    .disabled-icon {
        color: grey;
        display: inline-block;
        width: 40px;
        background-size: contain;
    }
    img {
        width: 2.5rem;
        height: 2.5rem;
    }
  </style>
  