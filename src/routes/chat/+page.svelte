<script>
    import ElizaBot from 'elizabot';
    import {writable} from 'svelte/store';
    export const chat_store = writable("[]");
    import { onMount } from 'svelte';


    const eliza = new ElizaBot();

    let chat = []

    import { enhance } from "$app/forms";

    onMount(() => {
    const stored = localStorage.getItem('chat_store');
    if (stored && stored.length > 2) {
        chat = JSON.parse(stored);
        chat_store.set(stored); // uppdatera store
    } else {
        chat = [{
            user: 'Eliza',
            message: eliza.getInitial(),
            time: new Date().toLocaleTimeString("sv-SE", { hour: "2-digit", minute: "2-digit" })
        }];
        const initial = JSON.stringify(chat);
        localStorage.setItem('chat_store', initial);
        chat_store.set(initial);
    }
});

    
 async function write(message) {
    //TODO: Add the new message to the chat

    const time = new Date().toLocaleTimeString("sv-SE", {
    hour: "2-digit",
    minute: "2-digit"
});


    chat = [
        ...chat, 
        {user: "User", message, time}
    ]
    const json= JSON.stringify(chat);
    $chat_store = json;
    localStorage.setItem('chat_store', json);

    
//Hämta HTML-elementet med id:et visible
var element = document.getElementById("visible");
//Ändrar elementets CSS-egenskap display till default
element.style.display = "flex"; // Visa elementet

            


    // random delay for Eliza's response time
    await new Promise((r) => setTimeout(r, 1000 + Math.random() * 1000));
    element.style.display = "none";

    const reply = eliza.transform(message);
    
    const replyTime = new Date().toLocaleTimeString("sv-SE", {
    hour: "2-digit",
    minute: "2-digit"
});


    //TODO: Add Eliza's response to the chat
    chat = [
        ...chat,
        {user: "Eliza", message: reply, time: replyTime}
    ]
    const json2 = JSON.stringify(chat);
    $chat_store = json2;
    localStorage.setItem('chat_store', json2);



  }

  function clearChat() {
    chat = [];
    $chat_store = "[]"; // tömmer localStorage
    localStorage.removeItem('chat_store');
}

                
</script>

<main>

    <section class = "message">
        {#each chat as msg}
        <article class = "bubble {msg.user}" data-user = {msg.user}  data-time= {msg.time}>
            <p>{msg.message}</p>
        </article>
        {/each}
        <article class = text id= "visible">
            
            <span class = "circle"></span>
            <span class = "circle"></span>
            <span class = "circle"></span>
        </article>

    </section>

    <button class="clear" onclick = {clearChat}>
        Rensa chat
    </button>

    
<form  method="post"
    use:enhance={({ formElement, formData, action, cancel }) => {
      cancel(); //don't post anything to server
      const text = formData.get("text"); // what does "text" refer to?
      write(text);
      formElement.reset()

      // TODO: reset the form using _____.reset() - what do we want to reset? the element or the data?

      }}>
                
        <input name="text" type="text" placeholder="Skriv meddelande här...">
        
    </form>

</main>

<style>

    main {
        position: relative;
        width: 60vw;
        height: 70vh;
        margin: auto;
        margin-top: 10vh;
        background-color: rgb(221, 215, 246);

        padding: 10px;
        display: grid;
        grid-template-rows: 9fr 1fr;
        border-radius: 10px;

    }

form {
    background-color: rgb(255, 255, 255);
    width: 90%;
    border-radius: 10px;
}

.message {
    overflow-y: scroll;
    display: flex;
    flex-direction: column;
    gap: 10px;
    
}


input {
    margin: 10px;
    padding: 10px;
    border-radius: 10px;

    background-color: rgb(255, 255, 255);
    width: 90%;
}

.bubble {
    margin: 10px;
    padding: 10px;
    border-radius: 10px;

    background-color: white;
    width: 50%;
}

.text {
    height: 60px;
    width: 100px;
    display: none;

    justify-content: center;
    align-items: center;
}

.circle {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    animation: text 1000ms ease-in-out infinite;
    background-color: rgb(176, 160, 231);
}

 
.min-animation {
  animation-name: text;
  animation-duration: 3s; /* Längd på animationen (till exempel 3 sekunder) */
   /* Funktion som styr tidsförloppet (till exempel "ease-in-out") */
  animation-iteration-count: infinite;
}
               

@keyframes text {
    0% {transform: scale(1);}
    50% {transform: scale(1.4);}
    100% {transform: scale(1);}

}
            /* CSS-stilar för .circle med index 1 (den första cirkeln) */
            .circle:nth-child(1) {
                animation-delay: 0ms; /* Ingen fördröjning */
            }
            /* CSS-stilar för .circle med index 2 (den andra cirkeln) */
            .circle:nth-child(2) {
                animation-delay: 333ms; /* Starta animationen efter 333 millisekunder (ms) */
            }
            /* CSS-stilar för .circle med index 3 (den tredje cirkeln) */
            .circle:nth-child(3) {
                animation-delay: 666ms; /* Starta animationen efter 666 ms */
            }

.User {
    background-color: rgb(144, 117, 224);
    margin-left: auto;
    text-align: right;
}

.Eliza {
    background-color: rgb(197, 184, 248);
    margin-right: auto;
    text-align: left;
}


.bubble::before {
  content: attr(data-user); /* Innehåll som läggs till */
  color: rgb(255, 255, 255);
  font-weight: bold;
}
                  
.bubble::after {
  content: attr(data-time);
  font-size: 0.8em;
  color: rgb(255, 255, 255);
}

.bubble:hover {
    filter: brightness(1.1);
    cursor: pointer;
}

input:focus {
    outline: 2px solid rgb(85, 0, 255);
    box-shadow: 0 0 6px rgba(85, 0, 255, 0.6);
}


.Eliza:first-of-type {
    background-color: rgb(109, 82, 228);
}

.clear {
    position: absolute;
    bottom: 10px;
    right: 20px;

    width: 60px;
    height: 60px;
    
    border-radius: 50%;
    border: none;

    background-color: rgb(255, 255, 255);
    color: rgb(0, 0, 0);
    font-size: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
    transition: transform 0.2s ease;
    
}

.clear:hover {
    transform: scale(1.1);
    cursor: pointer;
}

.clear:active {
    transform: scale(0.95);
}

</style>