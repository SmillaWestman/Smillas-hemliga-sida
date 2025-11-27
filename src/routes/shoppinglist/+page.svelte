<script>

    import { fade } from 'svelte/transition'
    
    let varor = $state([{ name: "Mjölk", kopt: false, priorety: 1}]);

    let nyVara = $state("")

    function laggTillVara(){
        if (nyVara.trim() === "") return;
        
        varor.push({
            name: nyVara,
            kopt: false,
            priorety: 1
        })
        
        nyVara = ""

    }

    function taBortVara(index){
        varor.splice(index, 1)

    }

    function bytKategori(index){
        varor[index].kopt=!varor[index].kopt

    }

    $effect(()=> {
        varor.sort((a,b)=> a.priorety - b.priorety)
    })

</script>

<main class="container">
    <h1> Shoppinglist</h1>

    <div class="categories_container">
        <section>
            <h2>Varor att köpa</h2>
            <ol>
                {#each varor as vara, i}
                {#if !vara.kopt}
                    <li transition:fade >{ vara.name } 

                        <input
                            type="number"
                            min = "1"
                            max = "5"
                            bind:value={vara.priorety}
                            class = "prio-input"
                        >

                        <button onclick={() => bytKategori(i)}>Köp</button>
                        <button onclick={() => taBortVara(i)}>Ta bort</button>

                    </li> 

                    

                    {/if}
                {/each}
            </ol>
        </section>
        <section>
            <h2>Köpta varor</h2>
            <ul>
                {#each varor as vara, i}
                {#if vara.kopt}
                <li transition:fade> { vara.name } 

                    <input
                        type="number"
                        min = "1"
                        max = "5"
                        bind:value={vara.priorety}
                        class = "prio-input"
                    >

                    <button onclick={() => bytKategori(i)}>Ångra</button>
                    <button onclick={() => taBortVara(i)}>Ta bort</button>

                </li>
                
                {/if}
                {/each}
            </ul>
        </section>
        
    </div>

    
    <form onsubmit={laggTillVara}>
    <input type="text" bind:value={nyVara} placeholder="Lägg till ny vara">

    <button type="submit">Lägg till vara</button>

    </form>
    
</main>

<style>
    
.container{
    background-color: rgb(244, 202, 222);
    width: 70vw;
    height: 80vh;
    border-radius: 20px;

    display: grid;
    grid-template-rows: 1fr 9fr 1fr;
    margin: auto;
    
   
}

.container h1{
    background-color: rgba(250, 158, 192, 0.7);
    border-radius: 20px;

    justify-self: center;
    align-self: center;
    padding: 8px;

}

.categories_container{
    height: 100%;
    background-color: rgb(247, 186, 215);

    display: grid;
    grid-template-columns: repeat(2,1fr);
    gap: 10px;

}

.categories_container section{
    border-radius: 20px;
}

 .categories_container section:first-child{
    background-color: rgba(246, 155, 191, 0.8) 
  }
 
  .categories_container section:nth-child(2){
    background-color: rgb(245, 106, 161, 0.9) 
  }

.categories_container section h2{
    background-color: rgb(255, 255, 255, 0.6);
    font-size: 1.5rem;
    text-align: center;
    padding: 10px;
}

ol{
    list-style-type: circle;
}

ul{
    list-style-type: disc;
}

form {
    display: flex;
    flex-direction: column;
    width: 45%;
    padding: 4%;
    margin: auto;
   
}

li {
    border-bottom: 1px solid white;
    padding: 5px;
}

li:last-child {
    border-bottom: none;

}

.prio-input {
    width: 30px;
    margin-left: 10px;

}

</style>