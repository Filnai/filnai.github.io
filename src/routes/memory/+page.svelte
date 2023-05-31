<script>
  	import { story_id_store } from '$lib/stores.js';

    let score = 0;
    let score2 = 0;
    let points = 0;
    let points2 = 0;
    let totalPoints = 0;
    let cards = [];
    let player1 = true;
    let doubler = 1;
    let doublerCost = 1000;
    let cardsCost = 600;

//totala poängen överförs till shopen
    story_id_store.subscribe(value => {
		totalPoints = Number(value);

//korten som är med i memory 
	});
    for (let index = 0; index < 2; index++) {
      cards.push({
        id: index, // TODO: unique ids per card card
        img: "https://i0.wp.com/www.printmag.com/wp-content/uploads/2021/02/4cbe8d_f1ed2800a49649848102c68fc5a66e53mv2.gif?resize=476%2C280&ssl=1", // TODO: unique images per card card
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 2, // TODO: unique ids per card card
        img: "https://media2.giphy.com/media/CVMo4eDVQ8bOo/giphy.gif", // TODO: unique images per card card
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 4, // TODO: unique ids per card card
        img: "https://media2.giphy.com/media/D2t94jSqtrVe0/giphy.gif",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 6, // TODO: unique ids per card card
        img: "https://media3.giphy.com/media/gbn9Z5tgJiKnRLOyxd/200.gif",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 8, // TODO: unique ids per card card
        img: "https://media.giphy.com/media/yr7n0u3qzO9nG/giphy.gif",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 10, // TODO: unique ids per card card
        img: "https://brightspotcdn.byu.edu/dims4/default/980ccfb/2147483647/strip/true/crop/355x325+0+0/resize/355x325!/quality/90/?url=https%3A%2F%2Fbrigham-young-brightspot.s3.amazonaws.com%2F88%2F2e%2F8bd62b724e9f906e2e8569170b04%2Fshaq-gif.gif",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 12, // TODO: unique ids per card card
        img: "https://media2.giphy.com/media/kEKcOWl8RMLde/giphy.gif?cid=6c09b952mj1r5db1zsy6mh10wnqlppk07xh034yfg90332da&ep=v1_gifs_search&rid=giphy.gif&ct=g",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 14, // TODO: unique ids per card card
        img: "https://media.tenor.com/XWd77nNGOs4AAAAM/sushichaeng-shrek.gif",
        flipped: false,  // TODO: think
        completed: false,
      });
    }

//då vid minishopen man kan köpa att lägga till kort
    function addCards() {
      let index=cards.length;
      //man kan bara lägga till ett visst antal kort, då max är 32 index
      if (totalPoints >= cardsCost && index<32) {
        totalPoints = totalPoints - cardsCost;
        story_id_store.set(totalPoints.toString())
        //korten som läggs till, med samma img 
      cards.push({
        id: index, // TODO: unique ids per card card
        img: "https://picsum.photos/id/"+index.toString()+"/200/300",
        flipped: false,  // TODO: think
        completed: false,
      });
      cards.push({
        id: index + 1, // TODO: unique ids per card card
        img: "https://picsum.photos/id/"+index.toString()+"/200/300",
        flipped: false,  // TODO: think
        completed: false,
      });
      //korten ska alltid ändra plats
      shuffleCards(cards);
      cards = cards
    }
    //om man inte har råd får man en alert
    else {
      alert("Sorry, you don't have enough money for this!")
    }

    }

    function pay(cost) {
      let procentagePay = points/points2;
      points = procentagePay*points;
      points2 = (1-procentagePay)*points2;
      totalPoints = points + points2;
    }

  //i mini shopen kan man köpa en upgrade där allt dubblas
    function double() {
      //så länge man har tillräckligt med pengar så funkar det
      if (totalPoints >= doublerCost) {
        doubler=doubler*2;
        points*=doubler;
        points2*=doubler;
        totalPoints = totalPoints - doublerCost;
        story_id_store.set(totalPoints.toString())
    }
    //om man inte har råd får man en alert
    else {
      alert("Sorry, you don't have enough money for this!")
    }
  }
//att korten inte alltid är på samma plats men shufflas
    function shuffleCards(cards) {
    for (var i = cards.length - 1; i > 0; i--) {
        var j = Math.floor(Math.random() * (i + 1));
        var temp = cards[i];
        cards[i] = cards[j];
        cards[j] = temp;
    }
}
//reset knappen då alla vända kort vänds tillbaka och score för vardera spelare startas om
    function reset() {
      cards.forEach ((card2) => {
        card2.completed = false;
        card2.flipped = false; 
        score = 0 
        score2 = 0
      })
      
      cards = cards
    //för att nollställa poängen igen, ta bort kommentaren här under och tryck reset sen kommentera ut den igen
      //story_id_store.set("0");

    }
shuffleCards(cards)

    let flipcount = 0;
    function flip(card) {
      card.flipped = true
      flipcount ++
      // flip card over if two cards are not already flipped
      // TODO: and card is already not flipped
      if (card.flipped && flipcount < 3) {
        // TODO: Probably do what?
        // flip the cards over after 2s from seeing both cards
        if (flipcount == 2) {
          cards.forEach ((card2) => {
             if (card.img == card2.img && card.id != card2.id && card2.flipped) {
              card.completed = true
              card2.completed = true
              if (player1) {
                score ++
              }
              else {
                score2 ++
              }
              player1 = !player1
             }
          }
          )

          setTimeout(() => {
            // flip over cards that have not been marked as "completed"
            cards.forEach((card) => {
              card.flipped = card.completed;
            });
            flipcount = 0;
            cards = cards;
            if (score+score2==cards.length/2) {
              if (score>score2) {
                points = points + 100*doubler;
                totalPoints = totalPoints + 100*doubler;
                story_id_store.set(totalPoints.toString())

              }
              else {
                points2 = points2 + 100*doubler;
                totalPoints = totalPoints + 100*doubler;
                story_id_store.set(totalPoints.toString())

              }  
            }

          }, 2000);
          player1 = !player1;
        }
        cards = cards;
      } else {
        alert("chill");
      }

      console.log(story_id_store)

    }
   
  </script>

//detsamma som i shopen att lägga till div så man kan lägga objekten bredvid varandra 
<div class="flexbox">
  <div class="info">
    <header>Total Points:</header>
    <input class="totalPoints" type=number bind:value={totalPoints}>

    <header>Here you can play memory</header>
    <header>You can also play against someone and keep track of your score</header>
    <header>Collect points to use in the shop</header>
    <a href="http://localhost:5173/Shop"><img class="webbshopPicture" src="https://shop.textalk.se/shop/ws76/42476/files/Network/Header%20till%20webbshop.png" alt="Shop" /></a>

    <button class="reset" on:click={reset}>Reset</button>
  </div>

  <div>
    <header>Points:</header>
    <header>Player1:</header>
    <input class="points" type=number bind:value={points} on:click={reset}>
    <header>Player2:</header>
    <input class="points" type=number bind:value={points2} on:click={reset}> 
  </div>

  <div class="varor">
    <div class="shop">
      <header>Mini Shop:</header>
      <header>Upgrades</header>
      <header class="information">Doubler:</header>
      <h1 class="information">Price: 1000p</h1>
      <button class="buy" value={doubler} on:click={double}>BUY</button>

      <header class="information">Add Cards:</header>
      <h1 class="information">Price: 600p</h1>
      <button class="buy" value={cardsCost} on:click={addCards}>BUY</button>
    </div>
  </div>


</div>

  <main>
    <div class="row">
      {#each cards as card, i}
        <div
          on:click={() => {
            flip(card);
          }}
          on:keypress={() => {
            flip(card);
          }}
          class:flipped={card.flipped}
          class="card"
        >
          <img class="front" src={card.img} alt="" />
          <img class="back" src="https://media3.giphy.com/media/3CCXHZWV6F6O9VQ7FL/giphy.gif" alt="" />
        </div>
      {/each}
    </div>

    <div class="score">
      <input class="playerScore" type=number bind:value={score}>
      <button class="playerScore" class:turn={player1} on:click="{() => score += 1}">player</button>
      <input class="playerScore" type=number bind:value={score2}>
      <button class="playerScore" class:turn={!player1} on:click="{() => score2 += 1}">player 2</button>
    </div>

  </main>
  <style>

    .playerScore {
      width: 110px;
      margin-right: 400px;
    }

    .score {
      display:grid;
    }

    .information {
      font-size: small;
    }

    .buy {
      width: 70px;
      font-size: small;
      margin-bottom: 10px;
    }

    .varor {
      display: flex;
      align-items:center;
      justify-content:center;
      width: 100px;

    }

    .shop {
      border: 5px solid;
      border-color:rgb(206, 206, 183);
      width: 100px;
      display:inline;
      align-items:flex-start;
    }

    .webbshopPicture {
      width: 70%;
    }

    .info {
      width: 50%
    }

    .flexbox {
      display: flex;
      align-items:center;
      justify-content:space-between;
      width: 75%
    }

    .totalPoints {
      margin-right: 1000px;
      width: 50%;
    }

    .reset {
      margin-left: 2px;
      width: 40%;
    }

    .points {
      margin: 20px auto;
      width: 100px;
    }

    .turn {
      background-color: green;
    }

    header {
      font-size: 20px;
      font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
      color: black;
    }

    input {
      width: 9%;
      font-size: 90%;
      font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
      margin: 40px auto;
      background-color:rgb(206, 206, 183);
      border: yellow;
      color: black;
    }

    button {
      width: 10%;
      font-size: 90%;
      font-family:Cambria, Cochin, Georgia, Times, 'Times New Roman', serif;
      margin: 40px auto;
      background-color:rgb(206, 206, 183);
      border: yellow;
      color: black;
    }

    main {
      margin-top: 50px;
      display: flex;
      place-content: center;
      place-items: center;

    }
    .row {
      display:grid;
      gap: 20px;
      width: 100%;
      grid-template-columns: repeat(4, 100px);
      grid-template-rows: repeat(8, 100px);
      margin-right: 100px;
    }
    .card {
      border: 2px solid black;
      cursor: pointer;
      transition: transform 1s;
      transform-style: preserve-3d;
      width: 100%;
      height: 100%;
    }
    .card.flipped {
      transform: rotateY(180deg);
    }
    .card .back {
      transform: rotateY(0deg);
    }
    .card .front {
      transform: rotateY(180deg);
    }
    .card img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      backface-visibility: hidden;
      -webkit-backface-visibility: hidden;
      position: absolute;
    }
    img {
      width: 30%;
    }
  </style>