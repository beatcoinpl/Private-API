coinbe.net private API v1.0.0.1 

* API key - your individual API key, you can find it in Settings. 
* API pin - individual password, you can find it in Settings.
* Methods are bolded and listed below in All methods.
* API URL is https://coinbe.net/api 
* API method is POST. 
* Remember to use SHA512!
* API #GET methods are limited to 1request / 200ms
* API #SET methods are limited to 1request / 2s
* if you see error 429 - please slow down ;-) It means, that your script is using too many requests!
PHP configuration example:
```
<?php
 
 
function Coinbe_Api($method, $params = array())
{
    $key = "123";
    $secret = "321";
 
    $params["method"] = $method;
    $params["time"] = time();
 
    $post = http_build_query($params, "", "&");
    $sign = hash_hmac("sha512", $post, $secret);
    $headers = array(
        "key: " . $key,
        "hash: " . $sign,
    );
    $curl = curl_init();
    curl_setopt($curl, CURLOPT_RETURNTRANSFER, true);
    curl_setopt($curl, CURLOPT_URL, "https://coinbe.net/api/");
    curl_setopt($curl, CURLOPT_POST, true);
    curl_setopt($curl, CURLOPT_POSTFIELDS, $post);
    curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
    $result = curl_exec($curl);
    $result = json_decode($result);
 
    if(!empty($result->error)){
        // Error code: $result->error
        // Error message: $result->errorMsg
    }
 
    return $result;
}

$info = Coinbe_Api("info");
var_dump($info->balance);
?>


```
PYTHON 3.6 configuration example:
```
import time
import hmac
import hashlib
import requests
import json


key = 'apiKey';
secret = b'secret';
timeError = 0; #change this parameter, if you got 504 error


def Coinbe_Api( method, params ):
	params['method']=method
	params['time']=str(int(time.time())+timeError)
	tosign = "&".join( [i + '=' + params[i] for i in params] ).encode('utf-8')
	sign = hmac.new(secret, tosign, hashlib.sha512)
	headers = {'hash': sign.hexdigest(), 'key': key }
	result = requests.post("https://coinbe.net/api", data=params, headers=headers).json()
	if "error" in result:
		# Error code: result["error"]
		# Error message: result["errorMsg"]
		print(result)
		exit()
	return result


info = Coinbe_Api("info",{})

print(info["balance"])
```


# Private API methods


## All methods
```
info - get info about your account #SET
trade - use Trade method to make a new offer on market #SET
trades - see your active orders in specify markets #GET
orders -  see all active in specify markets #GET
cancel - put id of action you want to cancel  #SET
history - history of user orders #GET
markethistory - history of market orders #GET
time - check time server #GET
```



## Info

Get info about your account. 


output
```

(
    [balance] => stdClass Object
        (
            [PLN] => 3000.05
            [EUR] => 0.00
            [USD] => 0.00
            [BTC] => 10.70798333
            [LTC] => 10000.00000000
            [DOGE] => 10000.00000000
            [DASH] => 10000.00000000
            [XMR] => 10000.00000000
            [ETH] => 10000.00000000
            [ZEC] => 0.00000000
            [BCN] => 1.00000000
            [ETC] => 0.00000000
        )
```

Info about your available balance that you can withdraw or exchange.
```
[BTC] => stdClass Object
                (
                    [transaction] => Array
                        (
                           )
                                (
                                    [amount] => 1.00000000
                                    [price_amount] => 10000.00000000
                                    [id_action] => 1
                                )

                        )

                    [withdrawals] => 1.00000000
                )
		
```
```
[withdrawals] - waiting for withdraw
[amount] - what amount you want to trade
[price_amount] - exchange rate
[total] = amount * price_amount 
[id_action] 1 - standard buy/sell
[id_action] 2 - quick buy/sell
[id_action] 3 - Bot buy/sell
```
wallet - your private, unique addresses to send cryptos on Coinbe.


```  
[wallet] => stdClass Object
        (
            [btc] => 158y1oivbEkde2fu9zpw4G9JYyrL9PC97E
            [ltc] => LaVKJY8rE4Hex63LpnxCcNdr6EyMiB4m2w
            [doge] => DARdK59aTWUaU5vBJWtNUU38Y7rKaQsEEP
            [dash] => XoGbqyi16MkRzWuaQfgDngXZSQaxQXX3UW
            [xmr] => 4JGe3aErQ7FdKHnZDYcf7Q1nqQA2hzYfUC7qp2Hja29CKWJ2ed9Pqw2XhNSaQuvU5L9dTqu9hcE87KhuwzkjPs6zCUKkBWboXdeBguKS1G
            [eth] => 0x564ae0d3bc676ad8549eb8ba904e15d3f2154761
            [etc] => 0xbb79bb584decd41b862745587e58bf6433ca36de
            [zec] => t1VTwKYBCZEanVeQMB7ok8PsRQcimmWtFub
            [bcn] => 25FKrrkkub8DUVpyVryhQh8iVWMrBwLSHccYNEBAg79wFv217GnpBgS3LcdE6wvsXmQnGzx5XDuQSjhtgwcXLTCC6sESMNq
        )

)

```



---


## Trade

Use Trade methods to make a new offer on market. 

```
[crypto] -> type crypto you want to trade
[currency] -> type currency you want to trade (includes BTC in markets BTC -> other cryptos)
[amount]  -> amount of crypto that you want to buy or sell
[price]  -> price of "currency" which you want to buy or sell
[type]   -> type of action: buy or sell
[action] -> standard (its mandatory and prepared for trade methods) 
```

output:
```
(
    [error] => 509
    [errorMsg] => Invalid value of the crypto parameter.
)
```

Action can't be done, check your input.

```
(
    [error] => 515
    [errorMsg] => You do not have enough funds.
)
```

You are trying trade more than you have!

```
(
    [status] => success
    [info] =>  Array
        (
	    [transactions] => Array with details of offers, which interaction has begun. 
            [market] => Information about your interaction with market, or "false" if nothing happened
        )
)
```
Transaction has been placed successfully 






## Cancel

Put id of action you want to cancel

Output:
```
(
    [error] => 512
    [errorMsg] => Invalid value of the ID parameter.
)
```

You don't have transaction with this ID. 


```
(
    [balance] => 9.70798333
    [currency] => BTC
)
```


If you see your balance, the order was successfully canceled



## Orders

See all active orders in specify markets

inputs:
```
[crypto] - > cryptocurrency
[currency] -> fiat currency
[type]     -> type of operation (buy, or sell)
 ```

Output:
```
(
    [start] => 0
    [limit] => 50
    [total] => 1
    [result] => Array
        (
            [0] => stdClass Object
                (
                    [id] => 62
                    [type] => sell
                    [who] => user
                    [cryptoAmount] => 1.00000000
                    [cryptoCurrency] => BTC
                    [mainAmount] => 10000.00000000
                    [mainCurrency] => PLN
                    [total] => 10000.00000000
                    [totalCurrency] => PLN
                    [datetime] => 2017-07-02 21:04:37
                )

        )

)
```

```
[start] -> number of first transaction (we start with 0, 0 is your first offer)
[limit] -> limit of transaction that you can maximally see
[total] -> counter of your transactions
[id]    -> your offer ID
[type]  -> type of your offer
[who]   -> user, bot, or express - it is type of your listed transaction.
[cryptoAmount] -> amount of crypto you offer
[mainAmount]   -> amount of fiat currency you offer
[mainCurrency] -> fiat currency (PLN, EUR, USD, or BTC)
[total]        -> it is total value of your offer
[totalCurrency]-> amount of total (concerns the market you have chosen)
[datatime] 	   -> data, and the time 
```




## Trades 
All active offers on the exchange

Inputs:


```
[crypto] - > cryptocurrency
[currency] -> fiat currency
[type]     -> type of operation (buy, or sell)

(
    [start] => 0
    [limit] => 100
    [total] => 1
    [result] => Array
        (
            [0] => stdClass Object
                (
                    [id] => 27
                    [type] => sell
                    [who] => user
                    [cryptoAmount] => 1.25966667
                    [cryptoCurrency] => BTC
                    [mainAmount] => 3000.00000000
                    [mainCurrency] => PLN
                    [total] => 3779.00001000
                    [totalCurrency] => PLN
                    [datetime] => 2017-06-30 22:24:07
                )

            [1] => stdClass Object
                (
                    [id] => 64
                    [type] => sell
                    [who] => user
                    [cryptoAmount] => 1.00000000
                    [cryptoCurrency] => BTC
                    [mainAmount] => 10000.00000000
                    [mainCurrency] => PLN
                    [total] => 10000.00000000
                    [totalCurrency] => PLN
                    [datetime] => 2017-07-02 21:12:58
                )
		}	
}

```
```
[id]    -> offer ID
[type]  -> type of offer
[who]   -> user, bot, or express - it is a type of listed transaction. You can see only 'user' transactions if it is not your transaction.
[cryptoAmount] -> amount of crypto offered on the exchange
[mainAmount]   -> amount of currency fiat offered on the exchange
[mainCurrency] -> fiat currency (PLN, EUR, USD, or BTC)
[total]        -> it is a total value of offer
[totalCurrency]-> amount of total (concerns the market you have chosen)
[datatime] 	   -> data, and the time 
```


## History 

use "/history" 

```
input:
 
    currency - main currency (PLN/BTC/EUR/USD/ETH* )
    crypto - second currency 
    limit - max 800, minimum 2
    type - sell, or buy
    since - tid of transaction it starts with transaction you gave +1 
```
You can input only one currency, but you dont have to. (for example only PLN, or only BTC) 
 
 ```
output:
  
 
        datetime - time of transaction
        cryptoAmount - amount of transaction
        cryptoCurrency - cryptocurrency
        mainAmount - price
        mainCurrency - currency of price
        total - summary


*ETH will be available soon! 
```

## Market History 

use "/markethistory" 
```input:
 
    currency - main currency (PLN/BTC/EUR/USD/ETH* )
    crypto - second currency 
    limit - max 800, minimum 2
    since - tid of transaction it starts with transaction you gave +1 
 ```
You can input only one currency, but you dont have to. (for example only )

 ```
output:
 
   Result: 
 
        datetime - time of transaction
        cryptoAmount - amount of transaction
        cryptoCurrency - cryptocurrency
        mainAmount - price
        mainCurrency - currency of price
        total - summary


```


## Time

use "/time" 

Check server time

 ```
output:
 
   Result: 
 
        status - success
        timestamp - UNIX Server Time
        datetime - human readable time format Y-m-d H:i:s
        tolerance - diffrence in time, which is acceptable by our API (+/-)


```
