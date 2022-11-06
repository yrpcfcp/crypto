

<script setup>

</script>
<template>
  <header>
    <h1>CRYPTO PRICES</h1>
    <div class="loading"></div>
  </header><br>
  <main>
    <ul>
      <li>
        ATOM<br><div>US$ {{ atomPrice }}</div>
      </li>
      <li>
        BITCOIN<br><div>US$ {{ bitCoinPrice }}.00</div>
      </li>
      <li>
        DAXCI<br><div>US$ {{ daxciCoinPrice }}</div>
      </li>
      <li>
        ETHEREUM<br><div>US$ {{ ethereumPrice }}</div>
      </li>
      <li>
        LUNA(Terra Luna Classic)<br><div>US$ {{ lunaPrice }}</div>
      </li>
    </ul>
    <br><br>

  </main>
  
  <article>
    <h2>CHOOSE THE DATE TO SEE THE HISTORICAL PRICE </h2><br><br>
    <p>CRYPTOS SUPPORTED: terra-luna, bitcoin, dacxi, ethereum e bitcoin-atom</p>
     
  <div class="coin-list">
    <input v-model="coin"  type="text" id="coin" name="coin" placeholder="insira o nome da crypto"> 
    <input v-model="date"  type="date" id="date" name="date">
    <button @click="getOldPrice()">GET PRICES</button>
  </div>
      <h2 class="preco-historico">&nbsp;&nbsp;&nbsp;The price on &nbsp;&nbsp;{{date}}&nbsp; was US$&nbsp;{{coinHistoricValue}}</h2>

  
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
      lunaPrice: 0 ,
    }
  }, methods: {
    
  getPrice:  async function(){
   try {
  const res = await axios.get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin%2Cbitcoin-atom%2Cdacxi%2Cethereum%2Cterra-luna&vs_currencies=brl&include_market_cap=false&include_24hr_vol=false&include_24hr_change=false&include_last_updated_at=false');
  let atomPrice = res.data['bitcoin-atom'].usd
  let bitCoinPrice = res.data.bitcoin.usd
  let daxciCoinPrice = res.data.dacxi.usd
  let ethereumPrice = res.data.ethereum.usd
  let lunaPrice = res.data['terra-luna'].usd
  
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
  
  let coin = this.coin
  let date = this.date
  date = date.match(/\d+/g)
  date = `${date[2]}-${date[1]}-${date[0]}`
  let url = `https://api.coingecko.com/api/v3/coins/${coin}/history?date=${date}&localization=false`

  try{
    let res = await axios.get(url);
    let coinHistoricValue = res.data.market_data.current_price.usd
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
  animation: unset;
  background-color:aqua;
  border: 2px transparent;
  border-radius: 15px;
  font-weight: bold;
  padding: 6px;
}

button:hover{
  background-color:black;
  color: aqua;
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



header {
  color: black;
  padding: 25px;
}




header h1 {
  color: aqua;
  text-align: center;
  margin: 5px 0px -10px 0px;
  font-size: 40px;
}

h2 {
  margin: 70px 0px 0px 0px;
  text-align: center;
}

h3 {
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

.loading{
  display:flex;
  justify-content: center;
}

.loading:after {
  content: ' ..............................................................................................................';
  animation: dots 2s steps(105, end) infinite;}
@keyframes dots {
  0%, 20% {
    color:aqua;
    text-shadow:
      .25em 0 0 rgba(0,0,0,0),
      .5em 0 0 rgba(0,0,0,0);}
  40% {
    color: white;
    text-shadow:
      .25em 0 0 rgba(0,0,0,0),
      .5em 0 0 rgba(0,0,0,0);}
  60% {
    text-shadow:
      .25em 0 0 white,
      .5em 0 0 rgba(0,0,0,0);}
  80%, 100% {
    text-shadow:
      .25em 0 0 white,
      .5em 0 0 white;}}


/* MEDIA QUERIES */

@media  (max-width: 425px){
	header{
    max-width: 50%;
    display: grid;
    place-items: center;
    margin: 0 0 0 40px;
     }
 .loading{
  display:none;
}
   ul{
    display:flex wrap;
    flex-direction:column;
    margin: -20px 0 0 16px;
    padding: 10px;
    max-width: 70%;
    text-align: center;
  }
  article{
    max-width:40%;
    margin: -80px 0 0 90px;
  }

  .coin-list{
    display:flex;
    flex-direction:column;
    align-items:center;
    padding: 15px 0 15px 0;
    margin: -30px 0 0 -10px;
    max-width: 60%;
  }

  .coin-list input{
    align-items:center;
    margin: 0 0 0 65px;
  }

  button{
    padding-top: 10px;
    margin-top: 15px;
    margin-left: 25px;
  }

  footer{
    margin: 80px 0 0 10px;
  }

  .preco-historico{
    font-size: 18px;
    margin: 10px  0 0 15px;
  }
}

</style>

