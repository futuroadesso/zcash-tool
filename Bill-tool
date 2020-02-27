#!/bin/bash
day=30
miner="" #insert your miner address here (t or zs)
bill=""  #insert your address used for paying electrical bill (t or zs)
safu=""  #insert your cold storage address (t or zs)

fee=0.0001
finalbill_eur=  #insert your eur eletrical bill here (numeric)



for today in {01..30}
do
 while [ $(date +%d) == $today ]
 do
  balanceminer=$(zcash-cli z_getbalance $miner)
 balancebill_zec=$(zcash-cli z_getbalance $bill)
 price=$(curl https://api.coinmarketcap.com/v1/ticker/zcash/?convert=EUR | awk '/"price_eur"/ {print $2 }' | cut -c2-6)
 finalbill_zec=$(echo "$finalbill_eur/ $price" | bc -l | cut -c1-10)
 dueuntiltoday=$(echo "scale=8 ;var=$finalbill_zec/ $day* $today ;if (var<1) print 0;var" | bc -l )
 topaytoday=$(echo "scale=8 ;var=$dueuntiltoday- $balancebill_zec ;if (var<0) var=0.00000000 else if (var<1) print 0;var" | bc -l)
  if [ $balanceminer != 0.00000000 ]
  then
  payenought=$(awk 'BEGIN{ print "'$balancebill_zec'"<"'$dueuntiltoday'"?1:0 }')
   if [ "$payenought" -eq 1 ]
   then
   haveenought=$(awk 'BEGIN{ print "'$balanceminer'">"'$topaytoday'"?1:0 }')
    if [ "$haveenought" -eq 1 ]
    then
     netfee=$(echo "var=$balanceminer- $fee ;if (var<1) print 0;var" | bc -l)
     netbill=$(echo "var=$netfee- $topaytoday ;if (var<1) print 0;var" | bc -l)
     opid=$(zcash-cli z_sendmany $miner '[{"address": "'$bill'" ,"amount": '$topaytoday'},{"address": "'$safu'" ,"amount": '$netbill'}]' "2" $fee)
     sleep 50s
     echo "$opid"
     result=$(zcash-cli z_getoperationstatus '["'$opid'"]')
     echo -e "$result\n"
     echo "$result" >> logbill.txt
     
    else
     netfee=$(echo "var=$balanceminer- $fee ;if (var<1) print 0;var" | bc -l)
     opid=$(zcash-cli z_sendmany $miner '[{"address": "'$bill'" ,"amount": '$netfee'}]' "2" $fee)
     sleep 50s
     echo "$opid"
     result=$(zcash-cli z_getoperationstatus '["'$opid'"]')
     echo -e "$result\n"
     echo "$result" >> logbill.txt
    fi
   else
    netfee=$(echo "var=$balanceminer- $fee ;if (var<1) print 0;var" | bc -l)
    opid=$(zcash-cli z_sendmany $miner '[{"address": "'$safu'" ,"amount": '$netfee'}]' "2" $fee)
    sleep 50s
    echo "$opid"
    result=$(zcash-cli z_getoperationstatus '["'$opid'"]')
    echo -e "$result\n"
    echo "$result" >> logbill.txt
   fi
  else
   echo "retry later..."
   sleep 3h
  fi
 done

done
