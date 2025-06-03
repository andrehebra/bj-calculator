<script>
    /* EVENT LISTNER FOR KEYBOARD SHORTCUT */

    import { onMount, onDestroy } from 'svelte';

    function handleKeydown(event) {
    const key = event.key.toLowerCase();

    const keyMap = {
      '1': 0,
      '2': 1,
      '3': 2,
      '4': 3,
      '5': 4,
      '6': 5,
      '7': 6,
      '8': 7,
      '9': 8,
      '0': 9,
      'a': 10,
      's': 11,
      'd': 12
    };

    if (key in keyMap) {
      removeCard(keyMap[key]);
    }
  }

    

    onMount(() => {
        window.addEventListener('keydown', handleKeydown);
    });

    

    // tracking of remaining cards
    //      ["A",2,3,4,5,6,7,8,9,10,"J","Q","K"];
    //      there will be a single array in the following order which will indicate the remaining number as an integer
    let cardArray = [];
    let numDecks = 1;
    let cardTrack = [];
    const cards = ["A",2,3,4,5,6,7,8,9,10,"J","Q","K"];
    let dealerHand = -1;
    let playerHand = [];

    function createDeck() {
        for (let i = 0; i < 13; i++) {
            cardArray.push(numDecks*4);
        }

        for (let i = 0; i < numDecks*4; i++) {
            cardTrack.push(i);
        }

        // in here to test custom deck
        // cardArray = [0,2,0,0,0,0,0,1,0,1,0,0,1]
    }

    function removeCard(index) {
        cardArray[index] = cardArray[index]-1;
        console.log(cardArray)
    }

    function addDealerCard(index) {
        removeCard(index);
        dealerHand = index+1;
        console.log(dealerHand);
    }

    function addPlayerCard(index) {
        removeCard(index);
        playerHand.push(index+1);
        playerHand = playerHand;
    }

    function resetPlayerAndDealer() {
        dealerHand = -1;
        playerHand = [];
    }

    function returnCard(index) {
        if (index == 0) {
            return "A";
        }
        if (index == 10) {
            return "J";
        }
        if (index == 11) {
            return "Q";
        }
        if (index == 12) {
            return "K";
        }
    }

    createDeck();
    console.log(cardArray);
    console.log(cardTrack);


    /* --- CALCULATE ODDS OF BUSTING --- */
    let oddsOfBusting;
    function determineBustOdds(remainingCards, playerHand) {
        let currentHandValue = 0;
        for (let i = 0; i < playerHand.length; i++) {
            if (playerHand[i] > 10) {
                currentHandValue+=10;
            } else {
                currentHandValue+=playerHand[i];
            }
        }
        console.log("Current Hand Value: " + currentHandValue);

        let numberOfRemainingCards = 0;
        for (let i = 0; i < remainingCards.length; i++) {
            numberOfRemainingCards+=remainingCards[i];
        }
        console.log("Number of remaining cards: " + numberOfRemainingCards);

        let winCount = 0;
        for (let i = 0; i < 13; i++) {
            if (i > 9) {
                if (currentHandValue + 10 <= 21) {
                    winCount += remainingCards[i];
                
                }
            } else {
                if (currentHandValue + i + 1 <= 21) {
                    winCount += remainingCards[i];
                }
            }
        }
        console.log("Win Count: " + winCount);

        oddsOfBusting = 100 - (winCount / numberOfRemainingCards * 100);
        console.log("Odds of Busting: " + oddsOfBusting);
        return oddsOfBusting;
    }



    let bestMove = "";

    function returnBestMove() {
        //let moveObject = decideHitOrStand(cardArray, playerHand, dealerHand);
        //console.log(moveObject);
        //bestMove = moveObject;
        //return bestMove;
    }

</script>

<h1>Card Calculator</h1>
<h2>Hand</h2>
<h3>Dealer</h3>
<div class="suitRow">
    {#if dealerHand != -1}
        <div class="card">{dealerHand}</div>
    {/if}
</div>
<div class="suitRow">
    <button onclick={() => addDealerCard(0)} class="card">A</button>
    <button onclick={() => addDealerCard(1)} class="card">2</button>
    <button onclick={() => addDealerCard(2)} class="card">3</button>
    <button onclick={() => addDealerCard(3)} class="card">4</button>
    <button onclick={() => addDealerCard(4)} class="card">5</button>
    <button onclick={() => addDealerCard(5)} class="card">6</button>
    <button onclick={() => addDealerCard(6)} class="card">7</button>
    <button onclick={() => addDealerCard(7)} class="card">8</button>
    <button onclick={() => addDealerCard(8)} class="card">9</button>
    <button onclick={() => addDealerCard(9)} class="card">10</button>
    <button onclick={() => addDealerCard(10)} class="card">J</button>
    <button onclick={() => addDealerCard(11)} class="card">Q</button>
    <button onclick={() => addDealerCard(12)} class="card">K</button>
</div>
<h3>Player</h3>
<div class="suitRow">
    {#each playerHand as playerCard}
    <div class="card">{playerCard}</div>
    {/each}
</div>
<div class="suitRow">
    <button onclick={() => addPlayerCard(0)} class="card">A</button>
    <button onclick={() => addPlayerCard(1)} class="card">2</button>
    <button onclick={() => addPlayerCard(2)} class="card">3</button>
    <button onclick={() => addPlayerCard(3)} class="card">4</button>
    <button onclick={() => addPlayerCard(4)} class="card">5</button>
    <button onclick={() => addPlayerCard(5)} class="card">6</button>
    <button onclick={() => addPlayerCard(6)} class="card">7</button>
    <button onclick={() => addPlayerCard(7)} class="card">8</button>
    <button onclick={() => addPlayerCard(8)} class="card">9</button>
    <button onclick={() => addPlayerCard(9)} class="card">10</button>
    <button onclick={() => addPlayerCard(10)} class="card">J</button>
    <button onclick={() => addPlayerCard(11)} class="card">Q</button>
    <button onclick={() => addPlayerCard(12)} class="card">K</button>
</div>

<div class="buttonRow">
    <button onclick={() => determineBustOdds(cardArray, playerHand)}>Calculate Odds</button>
    <button onclick={resetPlayerAndDealer}>Reset Hands</button>
</div>
<div class="results">
    <p>{returnBestMove()}</p>
</div>

<div class="oddsPage">
    {#if oddsOfBusting != null}
        <p>Odds of Busting: {oddsOfBusting}%</p>
    {/if}
</div>


<h2>Remaining Cards</h2>

<div class="suitRow">
    <button onclick={() => removeCard(0)} class="card">A</button>
    <button onclick={() => removeCard(1)} class="card">2</button>
    <button onclick={() => removeCard(2)} class="card">3</button>
    <button onclick={() => removeCard(3)} class="card">4</button>
    <button onclick={() => removeCard(4)} class="card">5</button>
    <button onclick={() => removeCard(5)} class="card">6</button>
    <button onclick={() => removeCard(6)} class="card">7</button>
    <button onclick={() => removeCard(7)} class="card">8</button>
    <button onclick={() => removeCard(8)} class="card">9</button>
    <button onclick={() => removeCard(9)} class="card">10</button>
    <button onclick={() => removeCard(10)} class="card">J</button>
    <button onclick={() => removeCard(11)} class="card">Q</button>
    <button onclick={() => removeCard(12)} class="card">K</button>
</div>

<div class="cardGrid">
    {#each cardTrack as cardNumber}
            <div class="suitRow">
                {#if cardNumber < cardArray[0]}
                    <div class="card">A</div>
                {:else}
                    <div class="card used">A</div>
                {/if}
                {#if cardNumber < cardArray[1]}
                    <div class="card">2</div>
                {:else}
                    <div class="card used">2</div>
                {/if}
                {#if cardNumber < cardArray[2]}
                    <div class="card">3</div>
                {:else}
                    <div class="card used">3</div>
                {/if}
                {#if cardNumber < cardArray[3]}
                    <div class="card">4</div>
                {:else}
                    <div class="card used">4</div>
                {/if}
                {#if cardNumber < cardArray[4]}
                    <div class="card">5</div>
                {:else}
                    <div class="card used">5</div>
                {/if}
                {#if cardNumber < cardArray[5]}
                    <div class="card">6</div>
                {:else}
                    <div class="card used">6</div>
                {/if}
                {#if cardNumber < cardArray[6]}
                    <div class="card">7</div>
                {:else}
                    <div class="card used">7</div>
                {/if}
                {#if cardNumber < cardArray[7]}
                    <div class="card">8</div>
                {:else}
                    <div class="card used">8</div>
                {/if}
                {#if cardNumber < cardArray[8]}
                    <div class="card">9</div>
                {:else}
                    <div class="card used">9</div>
                {/if}
                {#if cardNumber < cardArray[9]}
                    <div class="card">10</div>
                {:else}
                    <div class="card used">10</div>
                {/if}
                {#if cardNumber < cardArray[10]}
                    <div class="card">J</div>
                {:else}
                    <div class="card used">J</div>
                {/if}
                {#if cardNumber < cardArray[11]}
                    <div class="card">Q</div>
                {:else}
                    <div class="card used">Q</div>
                {/if}
                {#if cardNumber < cardArray[12]}
                    <div class="card">K</div>
                {:else}
                    <div class="card used">K</div>
                {/if}
            </div>
    {/each}
</div>

<style>
    .cardGrid {
        display: flex;
        flex-direction: column;
        gap: 2px;
    }

    .suitRow {
        display: flex;
        gap: 2px;
    }

    .card {
        border: 1px solid black;
        border-radius: 3px;
        width: 20px;
        height: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .used {
        background-color: red;
    }
</style>