<script>
  import GUN from 'gun';
  import Login from "./Login.svelte";
  import ChatMessage from "./ChatMessage.svelte";
  import { username, user } from "../user/user";
  import { onMount } from 'svelte';

  const db = GUN();
  let newMessage;
  let messages = [];

  onMount(() => {

    // Get Messages
    db.get("chat")
      .map()
      .once(async (data, id) =>{
        if(data) {
          // Key for end-to-end encryption
          const key = '#foo';
          var message = { 
            // transform the data
            who: await db.user(data).get('alias'), // a user might lie who they are! So let the user system detect whose data it is.
            what: ( await SEA.decrypt(data.what, key)) + '', // force decrypt as text.
            when: GUN.state.is(data,'what'), // get the internal timestamp for the what property.
          };

          if(message.what) {
            messages = [...messages.slice(-100), message];
          }
        }
      })
  });
  const sendMessage = async() => {
    const secret = await SEA.encrypt(newMessage, "#foo");
    const message = user.get('all').set({ what: secret });
    const index = new Date().toISOString();
    db.get("chat").get(index).put(message);
    newMessage = "";
  }
</script>

<div class="container">
  {#if $username}
    <main>
      {#each messages as message (message.when)}
        <ChatMessage {message} sender={$username} />
      {/each}
    </main>

    <form on:submit|preventDefault={sendMessage}>

    </form>
  {:else}
    <main>
      <Login />
    </main>
  {/if}
</div>