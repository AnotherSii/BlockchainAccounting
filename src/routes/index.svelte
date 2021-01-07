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


<p>Welcome to an ever-growing suite of tools designed to help you with accounting and other financial activities utilizing blockchain technology.</p>

<form on:submit|preventDefault={login}>
   <p>Sign in to check your token balance.</p>
   <input type="text" bind:value={user} required="true"/>
   <input class="button" type="submit" value="SIGN IN" />
</form>

<footer>
 <br><br><br>
   <a href="https://lamden.io"> <img alt='Powered by Lamden' src='lamden_original.png' style="width:65%; height:65%;"> </a>
    <h3><i>Powered by Lamden</i></h3>
    <br><br><br>
</footer>