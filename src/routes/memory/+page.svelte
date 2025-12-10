
<script>

    let images = [
        "https://i.pinimg.com/236x/44/7c/ad/447cad1e160456a95a0e41fe60a794ae.jpg", "https://i.pinimg.com/236x/44/7c/ad/447cad1e160456a95a0e41fe60a794ae.jpg",
        "https://i.pinimg.com/736x/de/be/19/debe193887eea9e9c030bb67fbea9f50.jpg", "https://i.pinimg.com/736x/de/be/19/debe193887eea9e9c030bb67fbea9f50.jpg",
        "https://i.pinimg.com/236x/5b/b7/aa/5bb7aae0fad3f22eddfd104bea62e51d.jpg", "https://i.pinimg.com/236x/5b/b7/aa/5bb7aae0fad3f22eddfd104bea62e51d.jpg",
        "https://i.pinimg.com/236x/fa/b7/b1/fab7b107a5598c19095eb0862f4a4561.jpg", "https://i.pinimg.com/236x/fa/b7/b1/fab7b107a5598c19095eb0862f4a4561.jpg",
        "https://i.pinimg.com/236x/2f/46/25/2f4625e0e0c83aec00739228b4870d97.jpg", "https://i.pinimg.com/236x/2f/46/25/2f4625e0e0c83aec00739228b4870d97.jpg",
        "https://i.pinimg.com/474x/fe/ad/81/fead81ea21bc096a44b354ca69b284ca--lady-bugs-art-google.jpg", "https://i.pinimg.com/474x/fe/ad/81/fead81ea21bc096a44b354ca69b284ca--lady-bugs-art-google.jpg"
    ]

images = images.sort(() => Math.random() - 0.5)

let cards = $state(images.map(img => ({
    image: img,
    flipped: false,
    matched: false
})));


let bluePoints = $state(0);
let redPoints =$state(0) ;
let blueTurn = $state(true);

let flipCount = 0
let firstCard = null
let secondCard = null

let showGameOver = $state(false)
let winner = $state("")

function flip(card) {
    if (flipCount === 2 )return;

    if(card.flipped || card.matched) return;

    card.flipped = true;
    flipCount ++;
    cards = cards;

    if(flipCount === 1) {
        firstCard = card;
        return;
    }

    if(flipCount === 2) {
        secondCard = card;
    }

     if (firstCard.image === secondCard.image) {
                firstCard.matched = true;
                secondCard.matched = true;
                cards = cards;

                if (blueTurn) bluePoints++;
                else redPoints++;

                flipCount = 0;
                firstCard = null;
                secondCard = null;

                gameOver();
                return;
            }

            setTimeout(() => {
                firstCard.flipped = false;
                secondCard.flipped = false;
                cards = cards;

                firstCard = null;
                secondCard = null;
                flipCount = 0;

                blueTurn = !blueTurn;
            }, 1000);
}

function restartGame() {
    images = images.sort(() => Math.random() - 0.5);

    cards = images.map(img => ({
        image: img,
        flipped: false,
        matched: false
    }));

    bluePoints = 0;
    redPoints = 0;
    blueTurn = true;

    flipCount = 0;
    firstCard = null;
    secondCard = null;
    showGameOver = false
}
function gameOver() {
    if (cards.every(card=> card.matched)) {
    showGameOver = true}

    if (bluePoints > redPoints) winner = "Blå spelare vann";
    else if(bluePoints < redPoints) winner = "Rosa spelare vann";
    else winner = "Oavgjort"
}


</script>

<h1>Memory</h1>

<button onclick={restartGame}>Starta om spelet</button>

{#if showGameOver}
<div class="gameover">
    <h2>Game Over! </h2>
    <p> {winner}</p>
    <button onclick={restartGame}>Spela igen</button>
</div>
{/if}


<main>
    {#each cards as card, i}
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <!-- svelte-ignore a11y_click_events_have_key_events -->
        <div class="card" class:flipped= { card.flipped } onclick={() => flip(card)}>
            <img src={card.image} alt="framsida" class = "front" />
            <img src= {"https://img.freepik.com/premium-vector/geometric-seamless-pattern_809705-5737.jpg"} alt= "baksida" class = "back" >
        </div>
    {/each}
</main>

<aside class="blue">
    <p> {bluePoints} </p>
</aside>

<aside>
 <p> {redPoints}</p>
</aside>

<aside class="turn" class:blue= { blueTurn }> </aside>

<style>
    h1 {
        text-align: center;
        font-size: 45px;
    }

    main {
        display: grid;
        grid-template-columns: repeat(3, 130px);
        grid-template-rows: repeat(4, 130px);
        justify-content: center;
        gap: 10px;
    }

    .card {
    width: 100%;
    height: 100%;
    border: 1px solid black;
    position: relative;
    transform-style: preserve-3d;
    transition: transform 0.5s;
}
.card:not(.flipped):hover {
    transform: scale(0.9);
}
.card img {
    width: 100%;
    height: 100%;
    position: absolute;
}

  aside{
    width: 120px;
    height: 120px;
    position: fixed;
    bottom: 10px;
    right: 10px;
    background-color: rgb(255, 130, 199);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  p{
    font-size: 40px;
  }
  
  .blue {
    background-color: rgb(0, 234, 255);
    left:10px;
  }

  .turn {
    box-shadow: 0 0 10px 10px rgb(0, 0, 0);
    z-index: -1;
  }

.front,
.back {
    width: 100%;
    height: 100%;
    position: absolute;
    backface-visibility: hidden;
}

 .front {
    transform: rotateY(180deg);
}

.card.flipped {
    transform: rotateY(180deg);
}

@media (min-height: 500px){
  main{
    grid-template-columns: repeat(3, 130px);
    grid-template-rows: repeat(4, 130px);
  }
}
@media (max-height: 500px){
  main{
    grid-template-columns: repeat(6, 100px);
    grid-template-rows: repeat(2, 100px);
  }
}

button {
    background-color: rgb(255, 199, 90);
    border: none;
    border-radius: 10px;
}

button:hover {
    transform: scale(1.1);
    background-color: rgb(255, 179, 50);
}

.gameover {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;

    background: rgba(0, 0, 0, 0.6); 
    backdrop-filter: blur(5px);       
    display: flex;
    justify-content: center;
    align-items: center;

    z-index: 1000;       
}


.gameover h2 {
    font-size: 40px;
    margin-bottom: 10px;
    gap: 10px;
}

.gameover p {
    font-size: 35px;
    margin-bottom: 10px;
}

.gameover button {
    font-size: 20px;
    padding: 10px 15px;
    border: none;
    border-radius: 10px;
    background-color: rgb(255, 199, 90);
}

.gameover button:hover {
    transform: scale(1.1);
    background-color: rgb(255, 179, 50);
}

</style>

