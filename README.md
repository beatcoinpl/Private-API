API key - your individual API key, to find in Settings.
API pin - individual password, to find in Settings.


### Private methods

----
## Info

Get info about your account. 


example response
'''
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
'''

info about your exchange values aviable to withdraw or exchange.

'''
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
		
'''

[withdrawals] - waiting for withdraw
[amount] - how much amount are you selling
[price_amount] - price of 1 unit of selling
[total] = amount * price_amount 
[id_action] 1 - standard buy/sell
[id_action] 2 - quick buy/sell
[id_action] 3 - Bot buy/sell

wallet - your private, unique addresses to send cryptos on Beatcoin.


'''    
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

'''



---


## Trade

Use Trade method to make new offer on market. 


[crypto] -> give which one crypto you want to trade
[currency] -> give which one currency want to trade (include BTC in markets BTC -> other cryptos)
[amount]  -> amount in crypto, which you want to buy or sell
[price]  -> price in currency, which you want to buy, or sell
[type]   -> type of action, buy, or sell


output:

'''
(
    [error] => 509
    [errorMsg] => Invalid value of the crypto parameter.
)
'''

Action cant be done, check your input.

'''
(
    [error] => 515
    [errorMsg] => You do not have enough funds.
)
'''

you try to trade more than you have

'''
(
    [0] => success
)
'''
Transaction done. 




------

## Cancel

ut id of action you want to cancel

Output:
'''
(
    [error] => 512
    [errorMsg] => Invalid value of the ID parameter.
)
'''

If you dont have transaction with this ID. 


'''
(
    [balance] => 9.70798333
    [currency] => BTC
)
'''


If cancel is succed you will see your balance. 

-----

##Orders

See your active orders in specify markets

inputs:
[crypto] - > cryptocurrency
[currency] -> fiat currency
[type]     -> kind of operation (buy, or sell)
 

Output:
'''
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
'''


[start] -> number of first transaction (we starting it on 0, 0 is the first your offer)
[limit] -> your limit of number of transactions to see
[total] -> count of your transactions
[id]    -> id of your offer
[type]  -> kind of your offer
[who]   -> user, bot, or express - its kind of your listed transaction.
[cryptoAmount] -> amount of crypto you offer
[mainAmount]   -> amount of currency fiat you offer
[mainCurrency] -> fiat currency (PLN, EUR, USD, or BTC)
[total]        -> its total value of offer
totalCurrency]-> currency of total (belong of market you used)
[datatime] 	   -> data, and the time 



-----

## Trades 
offers actually in the market

Inputs:


'''
[crypto] - > cryptocurrency
[currency] -> fiat currency
[type]     -> kind of operation (buy, or sell)

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

'''

[id]    -> id of offer
[type]  -> kind of offer
[who]   -> user, bot, or express - its kind of listed transaction. You can see only user transactions if it is not your transaction.
[cryptoAmount] -> amount of crypto offered on market
[mainAmount]   -> amount of currency fiat offered
[mainCurrency] -> fiat currency (PLN, EUR, USD, or BTC)
[total]        -> its total value of offer
[totalCurrency]-> currency of total (belong of market you used)
[datatime] 	   -> data, and the time 


-----
## History 

- temporary unaviable. 




