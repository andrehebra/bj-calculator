<script>
    // tracking of remaining cards
    //      ["A",2,3,4,5,6,7,8,9,10,"J","Q","K"];
    //      there will be a single array in the following order which will indicate the remaining number as an integer
    let cardArray = [];
    let numDecks = 8;
    let cardTrack = [];
    const cards = ["A",2,3,4,5,6,7,8,9,10,"J","Q","K"];

    function createDeck() {
        for (let i = 0; i < 13; i++) {
            cardArray.push(numDecks*4);
        }

        for (let i = 0; i < numDecks*4; i++) {
            cardTrack.push(i);
        }
    }

    function removeCard(index) {
        cardArray[index] = cardArray[index]-1;
        console.log(cardArray)
    }

    createDeck();
    console.log(cardArray);
    console.log(cardTrack);
</script>

<h1>Card Calculator</h1>

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

<button onclick={() => removeCard(0)}>Reset</button>

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