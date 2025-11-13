<script>

    import { users_store } from "$lib/user";
    import { onMount } from "svelte";

    let namn =""
    let lösenord=""
    let users = []

    function handleSubmit(){
        alert(`Välkommen ${namn}!\nDu är nu inloggad`);

    }

    onMount(() => {
        if ($users_store.length > 2) {
            users = JSON.parse($users_store);
        }
    });

    function handleLogin() {
        // Filtrera ut användaren baserat på namn
        const foundUser = users.filter(u => u.username === namn && u.password === lösenord);

        if (foundUser.length > 0) {
            alert(`Välkommen tillbaka ${namn}!`);
        } else {
            alert("Fel användarnamn eller lösenord!");
        }
    }
</script>


<main>

    <div class ="container">

        <h1>Inloggning</h1>

        <form on:submit|preventDefault={handleLogin}>
            <label for="namn">Namn:</label>
            <input type="text" id="namn" bind:value={namn} >

            <label for ="lösenord">Lösenord</label>
            <input type="text" id="lösenord" bind:value={lösenord}>

            <input type="submit" value="Logga in">

            <p>Har du inget konto? <a href="/register">Registrera dig</a></p>

         </form>
        
    </div>

</main>

<style>
 main{
        background-image: url("https://t4.ftcdn.net/jpg/05/21/65/59/360_F_521655929_N80d5GaCQJ2VP073PfTXJTe9mkvsNtHE.jpg");
        background-size: cover;
        width: 100%;
        height: 100%;
        min-height: 500px;
        padding: 5%;
    }

    
.container{
        border: solid 5px rgb(255, 255, 255);
        border-radius: 10px;
        width: 25%;
        height: 70%;
        min-width: 300px;
        min-height: 400px;
        background-color: #c6a5c1;
        margin: auto;

        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: space-evenly;

}     

form {
    display: flex;
    flex-direction: column;
    gap: 1rem; /* mellanrum mellan label/input */
}

label {
    font-weight: bold;
}


input, select {
    padding: 0.5rem;
    border-radius: 5px;
    border: none;
    outline: none;
    font-size: 1rem;
}

        
</style>