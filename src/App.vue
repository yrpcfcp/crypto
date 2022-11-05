

<script setup>
import { RouterLink, RouterView } from 'vue-router'

</script>
<template>
  <header>
    <h1>CRYPTO PRICES</h1>
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
    <h2>ESCOLHA A DATA PARA VER O PREÇO HISTÓRICO </h2><br><br>
    <p>CRYPTOS SUPORTADAS: terra-luna, bitcoin, dacxi, ethereum e bitcoin-atom</p>
     

    <div class="coin-list">

           
    <input v-model="coin"  type="text" id="coin" name="coin" placeholder="insira o nome da crypto"> 
    <input v-model="date"  type="date" id="date" name="date">
    <button @click="getOldPrice()">PESQUISAR PREÇOS</button>
    
    
 
  </div>
  <h2 class="preco-historico">&nbsp;&nbsp;&nbsp;O preço em &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{{date}} era R$&nbsp;&nbsp;{{coinHistoricValue}}</h2>

  
  </article>
  <footer>Data provided by CoinGecko</footer>
</template>


<script>
import axios from 'axios'


export default {
  
 el:'html',
 mounted:function(){
        this.getPrice() 
  },
  data() {
    return {
      
      date: '',
      coin: '',
      coinHistoricValue: '',
      atomPrice: 0,
      bitCoinPrice: 0,
      daxciCoinPrice: 0,
      ethereumPrice: 0,
      lunaPrice: 0,
      results:0,
      resultsAtom: 0,
      resultsDaxci: 0,
      resultsEthereum: 0,
      resultsLuna: 0,
      selectedResult: null,
      coins:['bitcoin-atom', 'bitcoin', 'dacxi', 'ethereum', 'terra-luna'],
      
    }
  }, methods: {
    
    selectCoin(){
      selectedCoin.push(coin)
      console.log(selectedCoin)
    },

   
   
  getPrice:  async function(){
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
setInterval( () =>{ 
this.getPrice()
}, 90000); 
  },



getOldPrice: async function(){
  
  

  let apiPath = 'https://api.coingecko.com/api/v3/coins/'
  let apiPath2 = '/history?date='
  let apiPath3 = '&localization=false`'
  let coin = this.coin
  let date = this.date

  date = date.match(/\d+/g)
  date = `${date[2]}-${date[1]}-${date[0]}`
  
  
 

  var url = `https://api.coingecko.com/api/v3/coins/${coin}/history?date=${date}&localization=false`

 
 
  
  try{
    let res = await axios.get(url);
    console.log(res)
    
    let coinHistoricValue = res.data.market_data.current_price.brl
    this.coinHistoricValue = coinHistoricValue
    
   
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

.coin-list{
  display:flex;
  flex-direction: row;
  align-items: center;
  justify-content:space-between;
}


#date{
  all:unset;
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
  margin: 70px 0px 0px 0px;
  text-align: center;
}

h3 {
  color: aqua;
  margin: 100px 0px 0px 0px;
  text-align: center;
}

p{
  text-align:center;
  margin: -25px 0 60px 0;
}

.preco-historico{
  margin: 30px 0 0 0;
}

article {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: -50px 0 0 0;   
}

input{
  all:unset;
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
  margin: 130px 0px 0px 0px;
}
</style>


<!-- getOldPriceAtom: async function(){
  try{
    const res = await axios.get('https://api.coingecko.com/api/v3/coins/bitcoin-atom/history?date=30-12-2021&localization=false');
    console.log(res)
    this.resultsAtom = res.data.market_data.current_price.brl
   
    
    
  } catch (error) {
    console.error(error);
  }
  
  },
  getOldPriceDacxi: async function(){
  try{
    const res = await axios.get('https://api.coingecko.com/api/v3/coins/dacxi/history?date=30-12-2017&localization=false');
    console.log(res)
    this.resultsDaxci = res.data.market_data.current_price.brl
   
    
    
  } catch (error) {
    console.error(error);
  }
  
  },
  getOldPriceEthereum: async function(){
  try{
    const res = await axios.get('https://api.coingecko.com/api/v3/coins/ethereum/history?date=30-12-2017&localization=false');
    console.log(res)
    this.resultsEthereum = res.data.market_data.current_price.brl
    
    
    
  } catch (error) {
    console.error(error);
  }
  
  },

getOldPriceLuna: async function(){
  try{
    const res = await axios.get('https://api.coingecko.com/api/v3/coins/ethereum/history?date=30-12-2017&localization=false');
    console.log(res)
    this.resultsLuna = res.data.market_data.current_price.brl
    
    
    
  } catch (error) {
    console.error(error);
  }
} -->