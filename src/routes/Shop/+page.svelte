<script>
    let slagga = 5000;
    let klubba = 5100;
    let varukorg = [];
    

    import { story_id_store } from '$lib/stores.js';
    import { each, validate_each_argument } from 'svelte/internal';

//poängen överförs från memory till shopen
    let totalPoints = 0;
    story_id_store.subscribe(value => {
		totalPoints = value;
	});

//för popup vid checkout
    import { writable } from 'svelte/store';
	import Modal from 'svelte-simple-modal';
    import { setContext } from 'svelte'
    import popup from './popup.svelte';


  const modal = writable(null);
  let sum = writable(0);
  setContext('my-var', sum); 

  function showModal(){ 
    varukorg.forEach((value) => {$sum = value + $sum })

    console.log(sum)
    modal.set(popup);
}

//vid checkout försvinner summan pengar man handlat för
//får en alert när man inte har tillräckligt med pengar för köpet
function pay() {
    if(totalPoints >= $sum) {
        totalPoints = totalPoints - $sum
    }
    else {
        alert("You do not have enough money to buy this product, keep playing and try another time!");
        }
}

</script>

//Här är de totala poängen som har överförts från memory till shopen 
    <header>Here are your points you can shop with</header>
    <input class="totalPoints" type=number bind:value={totalPoints}>

//första div (flexbox) som tar upp hela första ytan som sedan har flera div i sig 
//första div i flexbox tar upp första delen av ytan och är linken till memory 
    <div class="flexbox">
        <div class="photoLink">
            <header>Here you can get to the game to collect points to shop</header>
            <header>Click the picture to play</header>
            <a href="http://localhost:5173/memory"><img  src="image.png" alt="Memory" class="photoLink"/></a>
        </div>

//andra div i flexbox gör så att objekten läggs bredvid varandra med hjälp av style detsamma (display: flex;)
//dock vid plasering av div så la jag istället ihop de två varorna tillsammans i en div så plaseringen av objekten blev bättre
//där vi har en div som innehåller det första man kan köpa 
        <div class="varor">
            <div class="photos">
                <img class="varaimg" src="slägga.png" alt="slägga"/>
                <header class="information">Antons slägga:</header>
                <header class="information">Flyg högt min broder</header>
            //gav knappen ett värde av priset av produkten, sedan en funktion direkt i kod snutten
            //då varukorg listan lägger till varan när knappen klickas
                <button class="buy" value={slagga} on:click={()=>{varukorg = [slagga,...varukorg]}} >BUY</button>
                <header class="information">Pris: 5000p</header>
                
//inom samma div som varan innan, alltså classen varor så plaseringen blev rätt 
            </div>
            <div class="photos">
                <img class="varaimg" src="innebandyklubba.webp" alt="innebandyklubba"/>
                <header class="information">Alice innebandyklubba:</header>
                <header class="information">Sniper</header>
            //gav knappen ett värde av priset av produkten, sedan en funktion direkt i kod snutten
            //då varukorg listan lägger till varan när knappen klickas
                <button class="buy" value={klubba} on:click={()=>{varukorg = [klubba,...varukorg]}}>BUY</button>
                <header class="information">Pris: 5100p</header>
            </div>

        </div>
    </div>

//en ny div helt och hållet då de inte ska placeras bredvid varandra men under
    <div class="notice">
        <div>
            <p>Notice...</p>
            <header>There are soon arriving more things to the shop for you to buy, but don't be scared to buy things multiple times in both our mini shop and our regular shop. Have a nice day now! </header>
        </div>
    </div>

//då knappen till varorna trycks läggs summan pengar till och sedan sumeras till den sammanlagda kostnaden
    {#each varukorg as vara}
    <header class="checkOut">{vara}</header>
    {/each}
    <header class="checkOut">Amount to pay^</header>
//modulen som är en popup kommer upp när knappen trycks med information i en annan fil
        <Modal show={$modal}>
            <button on:click={showModal} on:click={pay} >Checkout</button>
          </Modal>

<style>
    
    .checkOut {
        border: 2px solid;
        background-color: rgb(98, 41, 41);
        border-color: rgb(233, 235, 196);
        color:rgb(233, 235, 196);
    }

    .notice {
        border: 5px solid;
        border-color:rgb(233, 235, 196);
        background-color:antiquewhite;
    }

    .buy {
        width: 100px;
    }

    .varaimg {
        width: 200px;
    }

    .varor {
        display: flex;
        width: 200%;
        margin-left: 5%;
    }

    .varukorg {
        width: 15%;
        background-color:antiquewhite;
        color:rgb(98, 41, 41);
    }

    .information {
        font-size:medium;
        font-family:'Trebuchet MS', 'Lucida Sans Unicode', 'Lucida Grande', 'Lucida Sans', Arial, sans-serif;
    }
    button {
        width: 20%;
        background-color:rgb(98, 41, 41);
        color:antiquewhite;
        
    }

    img {
        border:5px solid;
        border-color:rgb(98, 41, 41);
        background-color:white;
        
    }

    .photos {
        display: flex;
        align-items:start;
        justify-content:start;
        flex-direction: column;
        width: 100%;
    }

    .flexbox {
        display: flex;
        align-items:center;
        justify-content:flex-start;
        background-color:rgb(233, 235, 196);
        border: 5px solid;
        border-color:blanchedalmond;
        
    }
    
    .photoLink {
        width: 1000px;
    }
    img {
        width: 30%;
    }
    header {
        font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
    }
</style>