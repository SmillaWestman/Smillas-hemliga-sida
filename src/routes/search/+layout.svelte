<script>
     
  // Importera goto-funktionen för att navigera mellan sidor
  import { goto } from '$app/navigation';
  
  // Funktion som hanterar formulärets submit-händelse
  function handleSubmit(e) {
    // Förhindra att sidan laddas om (standardbeteende för formulär)
    e.preventDefault();
    
    // Skapa ett FormData-objekt från formuläret
    const formData = new FormData(e.target);
    
    // Hämta värdet från input-fältet med name="search"
    const search = formData.get('search');
    
    // Navigera till den dynamiska routen /search/[pokemon]
    goto('/search/' + search);
  }


</script>

<main>
    <slot>

    </slot>

     
<form onsubmit={handleSubmit}>
  <input class="search-input" type="text" name="search" placeholder="Sök upp en pokemon" />
</form>
                 
</main>

<div class= "backdrop"> 

</div>

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
</style>
