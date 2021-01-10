<script context="module">
    import WalletController from './js/lamden_wallet_controller/walletController.js';
    let contract_url = "https://testnet-master-1.lamden.io/contracts/con_apd_v15/";
   export async function preload({ params, query }) {
      //const res = await this.fetch(contract_url + `State?key=${params.user}`)
       // const res = await this.fetch(contract_url + `/State?key=${params.user}`) 
      // no need to get the url... should just refresh the page, and with a notification
      const res = await document.dispatchEvent(new CustomEvent('lamdenWalletGetInfo'));
      const data = await res.json(); // 
      alert(data.value, data);
      if (data.value === 'undefined') this.error(res.status, data.message);
      if (data.value === null) data.value = 0;
      
      return { value: data.value, user: params.user }; // THIS IS URL ISSUE = don't need a get
   }

</script>

<script>
    var contract_version = 'con_apd_v15'
    var network_version = 'testnet' // other option is mainnet
    
    import { goto } from '@sapper/app';
   // import WalletController from 'lamden_wallet_controller';
    
       const connectionRequest = {
        appName: 'Blockchain Accounting',
        version: '1.0.0',
        logo: 'https://iili.io/K40fKF.png',
        contractName: contract_version,
        networkType: network_version, // other option is 'mainnet'
    };

const handleResults = (txResults) => console.log(txResults) // probably delete this
const lwc = new WalletController(connectionRequest)



const handleWalletInfo = (walletInfo) => console.log(walletInfo)
const handleTxResults = (txInfo) => console.log(txInfo)

// Approval Transaction

const approve_detail = JSON.stringify({ 
   contractName: 'currency',
    methodName: 'approve',
    networkType: network_version, // might only be mainnet
    kwargs: {
        amount: 100, // variable
        to: contract_version,
        }
    
});  // do I need to send tokens to the contract before a user can redistribute the funds?
    // might need to link this to transfer_from

// document.dispatchEvent(new CustomEvent('lamdenWalletSendTx', {detail}));
// lwc.sendTransaction(transaction, handleResults) // callback is optional - need lwc
  
    
    export let user;
    export let value;

    let contract_host = "https://testnet-master-1.lamden.io/";
    let contract_url = "contracts/" + contract_version; //"https://masternode-01.lamden.io/contracts/con_abuse_6/";
    let receivers = "";
    let amount = 0;
    let percentages = "";
    let total = 0;
    let amounts = 0;
    


    // PERCENT TRANSFER FUNCTION \\
   const percent_transfer = async () => { // change it to like percent_transfer & amount_transfer
 //     const percent_transaction = {
 //        sender: user,
 //        contract: contract_version,
 //        method: 'percent_transfer',
 //        args: {
 //           receivers,
 //           amount,
 //           percentages,
 //        }
 //     }
      
      const txInfo = {
        networkType: network_version,
        methodName: 'percent_transfer', //maybe a variable? percent_transfer or amount_transfer
        kwargs: {
            total: amount, 
            receivers: [ [receivers] ], //an Array of Arrays - needs to get form data
        }, 
        stampLimit: 100
    };


   const refreshBalance = async () => {
      const res = await fetch(contract_host + contract_url) //+ "State?key=" + user) - needs to be lwc something
      let data = await res.json();
      value = data.value;
      }

   const clearInputs = () => {
      receivers = ""
      amount = 0
      percentages = ""
   }
   


//      const options = {
//         method: 'POST',
//         headers: {
//            'Content-Type': 'application/json'
//         },
//         body: JSON.stringify(percent_transaction)
//      }
      

    //  const res = await fetch(contract_host, options)
      
      const data = await res.json();
      if (data.error) {
         alert(data.error);
      } else {
         alert("You sent " + amount + " token(s) to " + receivers + "!");
         clearInputs();
         refreshBalance();
            }
}      
      
   
   // AMOUNT TRANSFER FUNCTION \\
   const amount_transfer = async () => { // change it to like percent_transfer & amount_transfer
      const amount_transaction = {
         sender: user,
         contract: contract_version,
         method: 'amount_transfer',
         args: {
            receivers,
            total,
            amounts,
         }
      }


   const refreshBalance = async () => {
      const res = await fetch(contract_host + contract_url) //+ "State?key=" + user)
      let data = await res.json();
      value = data.value;
      }

   const clearInputs = () => {
      receivers = ""
      total = 0
      amounts = ""
   }
   

      const options = {
         method: 'POST',
         headers: {
            'Content-Type': 'application/json'
         },
         body: JSON.stringify(amount_transaction)
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
      padding: 5px;
      color: #001eb0;
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
   <title>Tokens</title>
</svelte:head>

<h2>Token Balance: {value}</h2>
<p><i>Address: {user}</i></p>

<form on:submit|preventDefault={percent_transfer}>
   <h3>Distribute Funds based on percentages (use whole numbers)</h3>
   
    <!-- make it so a person can add a certain number of investors -->
   <label for="to">Receipients</label>
   <textarea name="to" required="true", placeholder="(address, integer)"> <!-- bind:value="{receivers}" -->
    </textarea>
<!--   <input type="text" name="to" bind:value={receivers} required="true"/> -->
   <label for="total">Total Amount</label>
   <input type="number" name="total" bind:value={amount} required="true"/>
 <!--  <label for="percent">Percentages</label>
   <input type="text" name="percent" bind:value={percentages} required="true"/> -->

</form>



<form on:submit|preventDefault={amount_transfer}>
   <h3>Distribute Funds based on actual amounts (use whole numbers)</h3>
   
    <!-- make it so a person can add a certain number of investors -->
   <label for="to">Receipients</label>
   <input type="text" name="to" bind:value={receivers} required="true"/>
   <label for="total">Total Amount</label>
   <input type="number" name="total" bind:value={total} required="true"/>
   <label for="amounts">Individual Amounts</label>
   <input type="text" name="amounts" bind:value={amounts} required="true"/>

</form>