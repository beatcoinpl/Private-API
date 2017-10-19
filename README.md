Beatcoin.pl private API v1.0.0.1 

* API key - your individual API key, you can find it in Settings. 
* API pin - individual password, you can find it in Settings.
* Methods are bolded and listed below in All methods.
* API URL is https://beatcoin.pl/api 
* API method is POST. 
* Remember to use SHA512! 

PHP configuration example:
```
<?php
 
 
function BeatCoin_Api($method, $params = array())
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
    curl_setopt($curl, CURLOPT_URL, "https://beatcoin.pl/api/");
    curl_setopt($curl, CURLOPT_POST, true);
    curl_setopt($curl, CURLOPT_POSTFIELDS, $post);
    curl_setopt($curl, CURLOPT_HTTPHEADER, $headers);
    $result = curl_exec($curl);
    $result = json_decode($result);
 
    if(!empty($result->error)){
        // Error code: $result->error
        // Error message: $result->errorMsg
    }
 
    return $ret;
}
?>

```



# Private API methods


## All methods
```
info - get info about your account
trade - use Trade method to make a new offer on market
trades - see your active orders in specify markets
orders -  see all active orders in specify markets
cancel - put id of action you want to cancel 
history - history of orders
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
wallet - your private, unique addresses to send cryptos on Beatcoin.


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
    [0] => success
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

See your active orders in specify markets

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
```
input:
 
    currency - main currency (PLN/BTC/EUR/USD/ETH* )
 
    crypto - second currency 
  
 
 
output:
 
   Result: 
 
        datetime - time of transaction
        cryptoAmount - amount of transaction
        cryptoCurrency - cryptocurrency
        mainAmount - price
        mainCurrency - currency of price
        total - summary


*ETH will be available soon! 
```
