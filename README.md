# zcash-tool
Some useful tools you can run with zcashd

With this tool you can set up&running your payment-processor.

It is useful to be always sure you got enought money for the electrical bill,
indipendently from the zcash/eur changes.
You can also shield your miner balance in a single zs-address if you don't care
about price changes. 
DON'T KEEP FUNDS OVER T-ADDRESS...shield it! :-)
Now you have no more problems with z_sendmany command

It must be configured adding your addresses between doublequotes "",
The fee (i've set it up as 0.0001 by default)
The final cost of bill in euros in finalbill_eur 
And modifying the sleep time at the end, that changes the daily frequency 

It requires a zcash fullnode up&running to use this.
For the zcash fullnode visit https://z.cash/
