

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
    <br><br>

  </main>
  
  <article>
    <h2>ESCOLHA A DATA PARA VER O PREÇO HISTÓRICO</h2>
    <label for="data">
      
    </label>
    <br><br>
    <div id="data-input">
  <select ID="select" name="select">
  <option value="valor1">ATOM</option>
  <option value="valor2" selected>BITCOIN</option>
  <option value="valor3">DACXI</option>
  <option value="valor4">ETHEREUM</option>
  <option value="valor5">LUNA</option>
</select>&nbsp;&nbsp;


    <input type="date" id="date" name="date">&nbsp;&nbsp;&nbsp;&nbsp;<button v-on:click="getPrice()">GET PRICES</button>
    </div>

    <h4>preço em {{date}}: (preço)</h4>
    

  
  </article>
  <footer>Data provided by CoinGecko</footer>
</template>


<script>
import axios from 'axios'
import Multiselect from 'vue-multiselect';

export default {
 el:'html',
 mounted:function(){
        this.getPrice() 
  },
  data() {
    return {

      atomPrice: 0,
      bitCoinPrice: 0,
      daxciCoinPrice: 0,
      ethereumPrice: 0,
      lunaPrice: 0,
      priceHistory: []
      
    }
  }, methods: {
   
  getPrice:  async function() {
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
}, 30000);
}, 
getOldPrice: async function(){
  try{
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

  }

}  
}




</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Electrolize&display=swap');
body {
  background-color: black;
  color: aqua;
  font-family: 'Electrolize', sans-serif;
}

article{
  margin: 400px 0 0 0;
}

button{
  background-color:aqua;
  border: 2px transparent;
  border-radius: 15px;
  font-weight: bold;
  padding: 6px;
}

.caption-button{
  text-align:center;
  margin: 10px auto;
}

#app > main > button{
  display:flex;
  justify-content:center;
  margin: 20px auto;
}

header {
  color: black;
  padding: 25px;
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
  margin: 30px 0 0 0;
  
}


.select{
  all:unset;
  
}

.date {
  display:flex;
  flex-direction: row;
  justify-content: space-evenly;  
  width: 45%;
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
