<script>
  import { user } from "../user/user";

  let username;
  let password;

  const login = () =>{
    user.auth(username, password, ({ err }) => err && alert(err));
  }
  const signUp = () =>{
    user.create(username, password, ({ err }) =>{
      if(err) {
        alert(err)
      }else{
        login();
      }
    })
  }
</script>

<label for="username">Usuário</label>
<input name="username" type="text" bind:value={username} minlength="3" maxlength="16" />

<label for="password">Senha</label>
<input name="password" type="password" bind:value={password} />

<button class="login" on:click={login}>Login</button>
<button class="login" on:click={signUp}>Cadastrar</button>