<!-- frontend/src/routes/index.svelte -->
<script>
    import { goto } from '@sapper/app';
    let user = "";
    const login = () => { goto('/users/' + user); }
</script>

<style>
   *{
      text-align: center;
   }
</style>

<svelte:head>
   <title>Blockchain Accounting</title>
</svelte:head>

<h1>Blockchain Accounting</h1>

<p>Welcome to an ever-growing suite of tools designed to help you with accounting and other financial activities utilizing blockchain technology.</p>

<br><br>
<form on:submit|preventDefault={login}>
   <h2>Sign in to check your token balance.</h2>
   <input type="text" bind:value={user} required="true"/>
   <input class="button" type="submit" value="SIGN IN" />
</form>

<footer>
 <br><br><br>
   <a href="https://lamden.io"> <img alt='Powered by Lamden' src='lamden-tau-logo.png' style="width:85%; height:85%;"> </a>
    <h3><i>Powered by Lamden</i></h3>
    <br><br><br>
</footer>