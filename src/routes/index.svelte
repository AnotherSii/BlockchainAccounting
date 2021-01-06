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
<h3><i>Powered by <a href="https://lamden.io">Lamden</a></i></h3>

<p>Welcome to an ever-growing suite of tools designed to help you with accounting and other financial activities utilizing blockchain technology.</p>

<form on:submit|preventDefault={login}>
   <p>Sign in and check your token balance.</p>
   <input type="text" bind:value={user} required="true"/>
   <input class="button" type="submit" value="Sign In" />
</form>
