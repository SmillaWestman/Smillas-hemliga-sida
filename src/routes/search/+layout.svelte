<script>
     
  // Importera goto-funktionen för att navigera mellan sidor
  import { goto } from '$app/navigation';
  import {onMount} from 'svelte'
  import { base } from '$app/paths';


  let recentSearches = []

  onMount(() => {
  const stored = sessionStorage.getItem('recent-searches');

  if (stored) {
    recentSearches = JSON.parse(stored);
  }
});

function saveSearch(search) {
    search = search.toLowerCase()

    recentSearches= recentSearches.filter(
        s => s !== search
    )

    recentSearches.unshift(search)

    recentSearches = recentSearches.slice (0, 5)

    sessionStorage.setItem(
        'recent-searches', 
        JSON.stringify(recentSearches)
    )
}

function handleSubmit(e) {
    // Förhindra att sidan laddas om (standardbeteende för formulär)
    e.preventDefault();
    
    // Skapa ett FormData-objekt från formuläret
    const formData = new FormData(e.target);
    
    // Hämta värdet från input-fältet med name="search"
    const search = formData.get('search');

    saveSearch(search)
    
    // Navigera till den dynamiska routen /search/[pokemon]
    goto('/search/[pokemon]');
  }

  function getTypeClass(name) {
    const types = {
      pikachu: 'yellow',
      butterfree: 'purple',
      venusaur: 'green',
      squirtle: 'blue',
      charmander: 'orange'

    }

    return types [name] ?? 'default'
    }


</script>

<main>
    <slot>

    </slot>
        
<form onsubmit={handleSubmit}>
  <input class="search-input" type="text" name="search" placeholder="Sök upp en pokemon" />
</form>
                 
</main>

<div class= "backdrop"> </div>

<footer>
  {#each recentSearches as search}
    <a
      href={`${base}/search/${search}`}
      class={`recent-link ${getTypeClass(search)}`}
      
    >
      {search}
    </a>
  {/each}
</footer>

<style>
   
main {
    width: 80vw;
    height: 100vh;
    background-color: rgba(197, 197, 197, 0.8);
    margin: 5vh auto;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    border: 3px solid rgb(255, 255, 255);
    }


    .backdrop {
        width: 100vw;
        height: 100vh;
        background-size: cover;
        background-image: url("https://imageio.forbes.com/specials-images/imageserve/604202ff091b6539cb90fcbc/The-backs-of-a-number-of-Pok-mon-cards-/960x0.jpg?format=jpg&width=960");
        position: fixed;
        top: 0;
        left: 0;
        z-index: -1;
        filter: blur(5px);
    }

    .search-input {
        width: 300px;
        padding: 10px 20px;
        border-radius: 12px;
        border: 4px solid rgb(93, 93, 93);
        font-size: 1rem;
    
    }

    footer {
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100vw;
      height: 60px;
      background-color: rgb(205, 205, 205);
      display: flex;
      justify-content: center;
      align-items: center;

    }

    .recent-link {
      padding: 8px 14px;
      border-radius: 20px;
      color: white;
      text-transform: capitalize; 

    }

    .yellow {
      background-color: #facc15;
      color: black;
    }

    .purple {
      background-color: rgb(231, 206, 255);
      color: black;
    }

    .green {
      background-color: rgb(216, 255, 167);
      color: black;
    }

    .blue {
      background-color: rgb(171, 255, 255);
      color: black;
    }

    .orange {
      background-color: #ff914d;
      color: black;
    }

    .default {
      background-color: rgb(180, 180, 180);
      color: black;
    }

</style>
