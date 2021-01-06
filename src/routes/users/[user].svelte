<!-- frontend/src/routes/users/[user].svelte -->
<script context="module">
    let contract_url = "http://167.172.126.5:18080/contracts/con_apd_v13/"; //"https://masternode-01.lamden.io/contracts/con_abuse_6/"; //
   export async function preload({ params, query }) {
      const res = await this.fetch(contract_url + `State?key=${params.user}`) // http://167.172.126.5:18080/contracts/con_apd_v2
      const data = await res.json();
      if (data.value === 'undefined') this.error(res.status, data.message);
      if (data.value === null) data.value = 0;
      return { value: data.value, user: params.user };
   }

// probably walletController in the header
</script>

<script>
    import { goto } from '@sapper/app';

    export let user;
    export let value;

   let contract_host = "http://167.172.126.5:18080/"
   let contract_url = "contracts/con_apd_v13/"; //"https://masternode-01.lamden.io/contracts/con_abuse_6/";
   let receivers = "";
   let amount = 0;
   let percentages = "";

   const percent_transfer = async () => { // change it to like percent_transfer & amount_transfer
      const percent_transaction = {
         sender: user,
         contract: 'apd_v13',
         method: 'percent_transfer',
         args: {
            receivers,
            amount,
            percentages,
         }
      }

   const refreshBalance = async () => {
      const res = await fetch(contract_host + contract_url + "State?key=" + user)
      let data = await res.json();
      value = data.value;
      }

   const clearInputs = () => {
      receivers = ""
      amount = 0
      percentages = ""
   }

   const logout = () => {
      goto(`.`);
   }

      const options = {
         method: 'POST',
         headers: {
            'Content-Type': 'application/json'
         },
         body: JSON.stringify(percent_transaction)
      }

      const res = await fetch(contract_host, options)
      const data = await res.json();
      if (data.error) {
         alert(data.error);
      } else {
         alert("You sent " + amount + " token(s) to " + receivers + "!");
         clearInputs();
         refreshBalance();
            }
   }
</script>

<style>
   p { font-size: 1.2em; }
   .shadowbox { padding: 0.5rem 20px; }
   form{
      padding: 50px;
      color: #461BC2;
      display:flex;
      flex-direction: column;
      border: none;
      box-sizing: border-box;
   }
   form > h2{
      margin: 0;
      font-weight: 600;
      line-height: 2.2;
      letter-spacing: 1px;
   }
   form > input{
      margin-bottom: 1rem;
   }
   input[type="submit"] {
      margin-right: 20px;
   }
   .buttons {
      display: flex;
      flex-direction: row;
      justify-content: flex-end;
      margin-top: 1rem;
   }
</style>

<svelte:head>
   <title>{user + "'s Tokens"}</title>
</svelte:head>

<h1>{"Hello " + user + "!"}</h1>
<h2>Token Balance: {value}</h2>

<form on:submit|preventDefault={percent_transfer}>
   <h3>Distribute Funds based on percentages (use whole numbers)</h3>
   
    <!-- make it so a person can add a certain number of investors -->
   <label for="to">Receipients</label>
   <input type="text" name="to" bind:value={receivers} required="true"/>
   <label for="amount">Total Token Amount</label>
   <input type="number" name="amount" bind:value={amount} required="true"/>
   <label for="amount">Percentages</label>
   <input type="number" name="amount" bind:value={percentages} required="true"/>
   <div class="buttons">
        <input class="button" type="submit" value="send"/>
        <button class="button" on:click={logout}>Sign Out</button>
   </div>
</form>
