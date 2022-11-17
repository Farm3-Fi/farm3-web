<script>
import "../app.postcss";
import {ethers} from "ethers";
import {wallet, provider} from "$lib/web3Store";

const connectWallet = async () => {
  if (typeof window.ethereum !== 'undefined') {
  $provider = new ethers.providers.Web3Provider(window.ethereum)
  await $provider.send("eth_requestAccounts", []);
  $wallet = await $provider.getSigner().getAddress();
  const chainId = await $provider.send( 'eth_chainId', []);
  const polygonChainID = '0x1';
  console.log($wallet)
  if(chainId != polygonChainID) {
    await $provider.send(
      'wallet_switchEthereumChain',
      [{ chainId: polygonChainID}],
    );
  }
  } else {
    alert("please install metamask!")
  }
}
</script>


<main class=" h-screen flex flex-col overflow-x-hidden bg-yellow-300 text-white">
  <nav class="mx-10 mt-6 flex justify-between items-center rounded-md  sticky top-0 z-10 h-14">
    <p class="col-span-5 text-3xl font-bold text-neutral-800">HoneyLemon</p>
    <div class="h-full">
      <button class="border-4 border-white px-10 h-full font-semibold rounded-full text-white">+Create Future</button>
      <button on:click={connectWallet} class="bg-white px-10 h-full font-semibold rounded-full text-yellow-300">{$wallet != null ? $wallet.substring(0,8) + "..." : "Connect Wallet"}</button>
    </div>
  </nav>
  <div class="mx-10 z-10 mt-8 text-neutral-500">
    <slot />
  </div>
  <footer class="fixed bottom-0 p-4 w-full bg-neutral-800">
    {#if $provider}
    {#await $provider.getBlockNumber()}
    <p>loading block ....</p>
    {:then block}
      <p>Block {block}</p>      
    {/await}
    {/if}
  </footer>
</main>

