<!-- frontend/src/routes/index.svelte -->
<script>
    import { goto } from '@sapper/app';
    let user = "";
    // const login = () => { goto('/users/' + user); }
    const detail = JSON.stringify({
        appName: 'Blockchain Accounting',
        version: '1.0.0',
        logo: 'https://iili.io/K40fKF.png',
        contractName: 'con_apd_v15',
        networkType: 'testnet', // other option is 'mainnet'
    });
</script>
<script>
    function lamdenConnect() {
        document.dispatchEvent(new CustomEvent('lamdenWalletConnect', {detail} ));
       // goto('/users/' + user);
        };
    
   // $('signIn').on('submit', lamdenConnect);
    //const login = lamdenConnect() => { goto('/users/' + user); }
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
<!-- <form on:submit|preventDefault={login}> -->
<form onsubmit="lamdenConnect()">
  <!-- <h2>Sign in with your Wallet Address below.</h2> -->
  <!-- <input type="text" bind:value={user} placeholder="Lamden Wallet Address" required="true"/> -->
   <input class="button" value="SIGN IN" />
</form>


<footer>
 <br><br><br>
   <a href="https://lamden.io"> <img alt='Powered by Lamden' src='lamden-tau-logo.png' width=10%; height=10%;> </a>
    <h3><i>Powered by Lamden</i></h3>
    <br><br><br>
</footer>