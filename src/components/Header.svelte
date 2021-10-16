<script>
  import { username, user } from "../user/user";

  const signOut = () => {
    user.leave();
    username.set('')
  }

</script>

<header>
  <h1>🔫 💬 </h1>
  {#if $username }
    <div class="user-bio">
      <span> Hello <strong>{$username}</strong></span>
      <img src={`https://avatars.dicebear.com/api/initials/${$username}.svg`} alt="avatar" />
    </div>
    <button class="signout-button" on:click={signOut}>
      Sign Out
    </button>
  {:else}
    <h3>Gun.js chat</h3>
  {/if}
</header>