---
layout: default
title: Randomizing authors
---

Recently we had a an author list to 

I've pulled these from [a random random names generator I found](http://www.namesgenerator.org/100-random-names/)

{% highlight python %}
authors = [
    "Staffan Krammerer", "Rosalinda Stavoua", "Tahsin Tomms", 
    "Saikhantuya Stronge", "Bronte Movileanu", "Mykel Eletti", 
    "Brendan Hamzová", "Utaka Heldt", "Alekzander Ogiemwonyi", 
    "Jaimi Neutz", "Jakobien Waclawik", "Olegas General",
    "Ishtar Nikulshin", "Ajo Lebedinsky", "Aril Blackfeather", 
    "Chrissa Kipoin", "Luciene Tomrley", "Shinobu Looney", 
    "Kalmi Saites", "Vienna Hyer", "Chritian Crosara", 
    "Jemski Grieder", "Giwayen Antlers", "Caila Wixted", 
    "Donara Polec", "Ricki Plockmeyer", "Merdan Werbeer", 
    "Rosemar Weglinsky", "Soline Sagimoto", "Letha Kampras"
]
{% endhighlight %}

[Requests](http://docs.python-requests.org/en/latest/) is my favourite Python HTTP library. If you're not using it now then you should be. 

{% highlight python %}
response = requests.put('https://api.random.org/json-rpc/1/invoke',
                        data=json_request)
{% endhighlight %}

Requests packages everything up nicely into a [`Response`](http://docs.python-requests.org/en/latest/api/#requests.Response) object. We really only care about the `Response.status_code` and `Response.content` attributes, which contain the response code from the server, and the JSON-encoded content. Using a handy-dandy `simplejson.JSONDecoder` we can decode the JSON into a `dict` making it much easier to deal with.

{% highlight python %}
if response.status_code == requests.codes.ok:
    # Decode response from the server
    decoder = simplejson.JSONDecoder()
    order = decoder.decode(response.content)
{% endhighlight %}

Having got a random permutation, we can then generate the author list

{% highlight python %}
print ', '.join(map(lambda i: authors[i],
                    order['result']['random']['data']))
{% endhighlight %}

Assuming everything has run ok, you should see something like the following output (permuted in some different fashion, of course).

```
Utaka Heldt, Olegas General, Kalmi Saites, Donara Polec, Staffan Krammerer, Jemski Grieder, Jaimi Neutz, Caila Wixted, Tahsin Tomms, Brendan Hamzová, Merdan Werbeer, Mykel Eletti, Ricki Plockmeyer, Ajo Lebedinsky, Luciene Tomrley, Saikhantuya Stronge, Chrissa Kipoin, Giwayen Antlers, Ishtar Nikulshin, Shinobu Looney, Alekzander Ogiemwonyi, Chritian Crosara, Bronte Movileanu, Rosalinda Stavoua, Rosemar Weglinsky, Letha Kampras, Soline Sagimoto, Jakobien Waclawik, Aril Blackfeather, Vienna Hyer
```

If you want this code in a nice copy-and-pastable form, [here's a gist](https://gist.github.com/jesserobertson/2d00f4c20b3e7c8c3262).