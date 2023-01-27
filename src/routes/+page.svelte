 <script>
   import AudioPlayer, { stopAll } from './AudioPlayer.svelte'

let audioTracks = [
  'https://sveltejs.github.io/assets/music/satie.mp3'
]
  import "@picocss/pico/css/pico.min.css";
  let clicks = 0;
  let multiplier = 1;
  let workers = [];
  let worker_multiplier = 1;

  let upgrades = [
    { cost: 15, name: "Doubler", multiplier: 2, worker: 0 },
    { cost: 10, name: "Worker", multiplier: 0, worker: 1 },
  ];

  function increment() {
    clicks = clicks + multiplier;
    console.log("click" + clicks);
  }
</script>

<h1>Välkommen till Filippas hemsida!</h1>
<h1>Här kan du spela sunflower seed collector </h1>

{#each audioTracks as src}
  <AudioPlayer {src} />
{/each}

<article>
  genom att klicka ett antal gånger får du en boost
  <progress value={clicks} max="5000"></progress>
  <header>
    klicka på uppgraderingarna för att samla mer poäng, men glöm inte att klicka själv i solrosen!
    <div class="grid">
      {#each upgrades as upgrade}
        <button
          class="upgrade"
          on:click={() => {
            if (clicks >= upgrade.cost) {
              if (upgrade.multiplier) {
                multiplier = multiplier + upgrade.multiplier;
                clicks -= upgrade.cost;
                upgrade.cost *= 2
              }
              if (upgrade.worker && clicks) {
                worker_multiplier = worker_multiplier * 2;
                workers = [upgrade.name, ...workers];
                /* start "clicking" every 1000 ms */
                /*ändrade till 2000*/
                setInterval(increment, 2000);
                clicks -= upgrade.cost;
                upgrade.cost *= 2
              }
            } else {
              alert("Click some more first!");
            }
          }}
        >
          <span>{upgrade.name}</span>
          <span>{upgrade.cost}</span>
        </button>
      {/each}
    </div>
  </header>
  <div class="game">
    <button on:click={increment} class="clicker">
      <span class="clicks">{clicks}</span>
      <span class="pointtext">PPC: {multiplier}</span>
    </button>
  </div>
  <footer>
    <div class="panelright">
      <div>
        <span>Workers: {workers.length}</span>
        <hr />
        <div class="shop">
          {#each workers as worker}
            <div class="worker">{worker}</div>
          {/each}
        </div>
      </div>
      <hr />
    </div>
  </footer>
</article>

<style>

progress{
  background-color:rgb(187, 216, 161);
  scrollbar-color: black;
}

progress[value]::-webkit-progress-value {
  background-image:
     -webkit-linear-gradient(-45deg, 
                             transparent 33%, rgba(189, 202, 91, 0.1) 33%, 
                             rgba(224, 184, 212, 0.1) 66%, transparent 66%),
     -webkit-linear-gradient(top, 
                             rgba(197, 209, 122, 0.25), 
                             rgba(211, 189, 122, 0.25)),
     -webkit-linear-gradient(left, rgb(153, 169, 65), rgb(210, 226, 142));

    border-radius: 2px; 
    background-size: 35px 20px, 100% 100%, 100% 100%;
}


  .shop {
    display: grid;
    grid-auto-flow: row; /* default */
    gap: 8px;
    grid-template-rows: repeat(3, 1fr);
    grid-template-columns: repeat(3, 1fr);
  }

  .upgrade {
    width: 100%;
    height: 100%;
    border: 8px solid rgb(190, 206, 81);
    background-color: rgb(152, 149, 8);
    background-size: cover;
    background-image: url("https://www.color-meanings.com/wp-content/uploads/Yellow-and-green-mixed.jpeg");
    place-items: center;
    place-content: center;
    display: flex;
    flex-direction: column;
  }

  .worker {
    width: 100%;
    height: 100%;
    border: 5px solid rgb(165, 200, 83);
    background-color: rgb(213, 219, 143);
    place-items: center;
    place-content: center;
    display: flex;
  }

  .game {
    height: 50vh;
    display: flex;
    flex-direction: column;
    place-items: center;
    place-content: center;
    background-color:rgb(225, 219, 124)
  }
  .clicker {
    clip-path: circle();
    display: flex;
    height: 100%;
    width: 50%;
    flex-direction: column;
    place-items: center;
    place-content: center;
    background-image: url("https://media.istockphoto.com/id/174648035/sv/foto/sunflower-isolated.jpg?s=612x612&w=0&k=20&c=eap8JN5NYGlTUc5IjUdWi6mb5kEupvyWoagzREqOePA=");
    background-size: cover;
    background-position: 0px -100px;
  }

  .panelright {
    height: 100%;
    display: flex;
    flex-direction: column;
    gap: 16px;
  }

  .clicks {
    font-size: 100px;
  }

  .pointtext {
    color: rgb(236, 220, 98);
    font-size: 25px;
    font-weight: bold;
  }
</style>
