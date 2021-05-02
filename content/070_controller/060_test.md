+++
title = "Testing the APIs"
date = 2020-07-08T14:37:59+03:00
weight = 60
+++

Our APIs are published, lets verify all works as expected.  

1. Check that the authentication is failing when not providing an API key

```
curl -k# https://$microhost/v1/stock/ticker/all
```

{{< output >}}
<html>
<head><title>401 Authorization Required</title></head>
<body>
<center><h1>401 Authorization Required</h1></center>
<hr><center>nginx</center>
</body>
</html>
{{< /output >}}  

2. Try again but this time including the API key and the test-client value

```
curl -k#  https://$microhost/v1/stock/ticker/all --header "apikey: 0123456789"
```

{{< output >}}
{"btc":{"ticker":{"symbol":"tBTCUSD","bid":53783,"bidSize":9.98263374,"ask":53789,"askSize":8.07079989,"dailyChange":-1057,"dailyChangePerc":-0.0193,"lastPrice":53789,"volume":6229.6409748,"high":56432,"low":53441}},"eth":{"ticker":{"symbol":"tETHUSD","bid":2767.1,"bidSize":334.64948837000003,"ask":2767.9,"askSize":198.96622354000002,"dailyChange":55.8,"dailyChangePerc":0.0206,"lastPrice":2767.3,"volume":72050.81102921,"high":2796.1,"low":2661.5}},"ltc":{"ticker":{"symbol":"tLTCUSD","bid":256.64,"bidSize":632.84153383,"ask":256.82,"askSize":1257.55131368,"dailyChange":0.23,"dailyChangePerc":0.0009,"lastPrice":256.72,"volume":36688.87741606,"high":262.73,"low":250.29}}}
{{< /output >}}  

3. The configuration is rate limiting to 5 request per 1 minute sliding window. Repeat the above command 6 times. The last request will be blocked due to reaching the rate limit.  

{{< output >}}
<head><title>429 Too Many Requests</title></head>
<body>
<center><h1>429 Too Many Requests</h1></center>
<hr><center>nginx</center>
</body>
</html>
{{< /output >}}  