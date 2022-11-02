

<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>
<template>
  <header>
    <h1>PRICE GENERATOR</h1>
  </header><br>
  <main>
    <ul>
      <li>
        ATOM<br><div>R$ {{ atomPrice }}</div>
      </li>
      <li>
        BITCOIN<br><div>R$ {{ bitCoinPrice }}.00</div>
      </li>
      <li>
        DAXCI<br><div>R$ {{ daxciCoinPrice }}</div>
      </li>
      <li>
        ETHEREUM<br><div>R$ {{ ethereumPrice }}</div>
      </li>
      <li>
        LUNA(Terra Luna Classic)<br><div>R$ {{ lunaPrice }}</div>
      </li>
    </ul>
  </main>
  <article>
    <label for="data">
      <h1>ESCOLHA A DATA PARA VER O PREÇO HISTÓRICO</h1>
    </label>
    <br><br>
    <input type="date" id="date" name="date">
    <h4>preço em {{date}}: (preço)</h4>
    <button v-on:click="getUser()">get prices</button>

  
  </article>
  <footer>Data provided by CoinGecko</footer>
</template>


<script>
import axios from 'axios'
export default {
  data() {
    return {

      atomPrice: 0,
      bitCoinPrice: 0,
      daxciCoinPrice: 0,
      ethereumPrice: 0,
      lunaPrice: 0
      
    }
  }, methods: {
  getUser:  async function() {
  try {
    const res = await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin%2Cbitcoin-atom%2Cdacxi%2Cethereum%2Cterra-luna&vs_currencies=brl&include_market_cap=false&include_24hr_vol=false&include_24hr_change=false&include_last_updated_at=false');
    console.log(res)
    let atomPrice = res.data['bitcoin-atom'].brl
    let bitCoinPrice = res.data.bitcoin.brl
    let daxciCoinPrice = res.data.dacxi.brl
    let ethereumPrice = res.data.ethereum.brl
    let lunaPrice = res.data['terra-luna'].brl
    
    this.atomPrice = atomPrice;
    this.bitCoinPrice = bitCoinPrice;
    this.daxciCoinPrice = daxciCoinPrice;
    this.ethereumPrice = ethereumPrice;
    this.lunaPrice = lunaPrice;

  } catch (error) {
    console.error(error);
  }
setInterval(()=>{ 
this.getUser()
}, 90000);
}
  }
}


</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Rubik:ital,wght@0,500;0,700;1,400&display=swap');

body {
  background-color: black;
  color: aqua;
  font-family: 'Rubik', sans-serif;
}

header {
  color: black;
  padding: 35px;
}

header h1 {
  color: aqua;
  text-align: center;
  margin: 5px 0px 0px 0px;
  font-size: 40px;
}

h2 {
  color: aqua;
  margin: 100px 0px 0px 0px;
  text-align: center;
}

h3 {
  color: aqua;
  margin: 100px 0px 0px 0px;
  text-align: center;
}



article {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 100px;
}

input {
  
  width: 15%;
}

ul {
  display: flex;
  justify-content: space-evenly;
  list-style-type: none;
}

footer{
  display:flex;
  font-size: 12px;
  justify-content:center;
  margin: 70px 0px 0px 0px;
}
</style>
