---
author: Teemu Harju
comments: true
date: 2015-07-20 08:57:11+00:00
layout: post
slug: validating-google-play-store-receipts-using-elixir
title: Validating Google Play Store Receipts Using Elixir
wordpress_id: 622
categories:
- Elixir
- Erlang
tags:
- Elixir
- Erlang
---

Since I'm writing game servers and our company is making money from games, this topic has lately become of specific interest to me. It was not as straightforward process I'd hoped it to be so I decided to write down the solution in case someone else is doing the same thing. I'll describe here the technique for validating the integrity of the receipt using the given signature. You might want to perform few more checks to the content of the receipt, in case someone would be sending a valid receipt and signature pair, but for a purchase that has already been processed.

When user completes a purchase in the _Play Store_ you should get a receipt and a signature. The receipt looks something like this...

{% highlight json %}
{"orderId":"...","packageName":"...","productId":"my_product.1","purchaseTime":1437212304221,"purchaseState":0,"developerPayload":"...","purchaseToken":"..."}
{% endhighlight %}

The signature is a base64 encoded string.

Now, you should verify that the signature is valid using the RSA public key that you get from _Google Play Developer Console_ under the _"Services & APIs"_ section. The public key is also base64 encoded.

Ok, let's look at the code on how to achieve that. Here's the code, I'll explain it below...

{% highlight elixir %}
def valid_purchase?(receipt, signature, public_key) do
  # decode the base64 encoded RSA public key and the signature
  public_key_bytes = Base.decode64!(public_key)
  signature_bytes = Base.decode64!(signature)

  # decode the public key in a format that the Erlang public_key module understands
  {_, _, {0, key_der}} = :public_key.der_decode(:'SubjectPublicKeyInfo', public_key_bytes)
  public_key = :public_key.der_decode(:'RSAPublicKey', key_der)

  # verify the signature
  :public_key.verify(receipt, :sha, signature_bytes, public_key)
end
{% endhighlight %}

As you see the process is rather simple. You just need to figure out which functions from Erlang's `:public_key` module to use. First we base64 decode the give public key and signature strings. Then we need to decode the public key so that we can use it with `:public_key.verify/4`. And that's it. Remember that the receipt string must remain unchanged so in case you parse it before validating the signature remember to use the original JSON string. Just extra newline somewhere will make the signature fail.

I tend to like this way of validating purchases more than the one with _Apple's AppStore_ where I need to call Apple's service for validating the purchase receipt. In that case the system becomes lot more complex since you need to handle the case where the validation service is down. Maybe a similar solution would be possible in Apple's case as well.
