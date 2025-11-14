<script>

    import {users_store} from "$lib/user";
    import { base } from '$app/paths';

    let color = "black"
    let namn =""
    let email =""
    let lösenord=""

    let färg = [
        {namn: "Blå", value: "blue"}, 
        {namn: "Röd", value: "red"}, 
        {namn: "Grön", value: "green"},
        {namn: "Lila", value: "purple"},
        {namn: "Gul", value: "Yellow"},
        {namn: "Rosa", value: "pink"},
        {namn: "Orange", value: "orange"},
    ]

    let users = [];

    function handleSubmit(){
        let new_user = {
            username: namn, 
            password: lösenord, 
            email: email, 
            color: color
        };

        const existingUser = users.filter(u => u.username === namn);

        if (existingUser.length > 0) {
        alert("Användaren finns redan! Välj ett annat användarnamn.");
        return; // stoppa funktionen här
    }


        users = [...users, new_user];
        $users_store = JSON.stringify(users);

        alert(`Välkommen ${namn}!\nE-post: ${email}\nLösenord: ${lösenord}`);

    }

    
import { onMount } from 'svelte';
onMount(() => {
    /*Check if has more then 2 characters*/
    if($users_store.length > 2){
        users = JSON.parse($users_store);
    }
});

</script>


<main>

    <div class ="container">

        <h1>Registrering</h1>

        <div
            style="width: 100px; height: 100px; border-radius: 50%; overflow:hidden; background-color:{color};">
        </div>

        <form on:submit|preventDefault={handleSubmit}>
            <label for="namn">Namn:</label>
            <input type="text" id="namn" bind:value={namn} >

            <label for="email">E-postadress:</label>
            <input type="text" id="email" bind:value={email}>

            <label for ="lösenord">Lösenord:</label>
            <input type="text" id="lösenord" bind:value={lösenord}>

            <label for="färg">Favoritfärg:</label>
            <select id="färg" bind:value={color}>
                {#each färg as c}
                    <option value={c.value}>{c.namn}</option>
                {/each}
            
            </select>

            <input type="submit" value="Registrera">

            <p>Har du redan ett konto? <a href="{base}/login">Logga in</a></p>
            


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
        width: 35%;
        height: 80%;
        min-width: 300px;
        min-height: 500px;
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
