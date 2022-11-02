import axios from "axios"

axios
  .get('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin&amp;vs_currencies=usd&amp;include_market_cap=true&amp;include_24hr_vol=true&amp;include_last_updated_at=true')
  .then((res) => {
          console.log(res.data)
  })
  .catch((error) => {
          console.log(error);
  });