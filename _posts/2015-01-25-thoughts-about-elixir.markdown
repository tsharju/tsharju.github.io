---
author: Teemu Harju
comments: true
date: 2015-01-25 09:15:43+00:00
layout: post
slug: thoughts-about-elixir
title: Thoughts About Elixir
wordpress_id: 549
categories:
- Elixir
- Erlang
- Programming
tags:
- Elixir
- Erlang
- Programming
---

I've been programming [Erlang](http://www.erlang.org) for several years now and have always liked it. Erlang syntax has it's quirks, but I've always considered OTP so cool that I don't mind all the braces, colons, semicolons and stuff. Then I heard about [Elixir](http://elixir-lang.org) that runs on Erlang VM, has a Rubyish syntax and can take advantage of all the cool stuff that the Erlang OTP offers. It took a while for Elixir to reach version 1.0 and become really a viable option for developing server software, but I really think it's now there and I though that I should write about why I think so, and why I'm probably going to do more Elixir programming than Erlang programming in the near future.



### Syntax



Elixir syntax is pretty close to Ruby. You don't have to add braces to function calls if you don't feel like it and all that stuff. Personally, this is what I don't like about Ruby. I'm more of a Python guy anyways, forgive me. _[The Zen of Python](https://www.python.org/dev/peps/pep-0020/)_ states that...



<blockquote>There should be one-- and preferably only one --obvious way to do it.</blockquote>



I kind of agree with that. Although, shamed to admit, but Elixir's Rubyish syntax is growing on me and I'm liking it more and more.

The one thing where Erlang's syntax is at it's best is writing recursive code using pattern matching. It combines the different parts of the recursive function nicely using semicolon. Elixir doesn't make this distinction basically at all. Pattern matched functions all look the same and are somewhat more difficult to spot from the code. For example in Erlang a simple recursive function looks like this...

{% highlight erlang %}
loop([], Acc) ->;
  Acc;
loop([Head|Tail], Acc) ->;
  loop(Tail, Acc + Head).
{% endhighlight %}

And in Elixir...

{% highlight elixir %}
def loop([], acc) do
  acc
end

def loop([head|tail], acc) do
  loop(tail, acc + head)
end
{% endhighlight %}

Note that both examples only have one function, which is maybe not that visible from Elixir code. Not a big deal, but makes code somewhat less readable when, for example, you have `gen_fsm` module with a lot of callbacks. The Elixir syntax does not group the def clauses in any way, but the compiler politely warns you if you happen to have your function definitions spread all over.



### Handling Nested Data Structures



One area where Elixir particularly shines over Erlang is handling nested data structures. It is quite common to have some entity's state represented by a nested map data structure and you usually all your application does is modify that state. Due to the fact that Erlang (and Elixir) data structures are immutable, modifying these nested data structures gets rather cumbersome. Elixir has some neat utilities to help you deal with these.



#### Keyword Lists vs Proplists



Proplists are common way of passing on configuration data in Erlang applications. Proplist is a list of two element tuples, where the first element is an key atom and second element is the value which can be any Erlang term. Data in proplists is accessed using the `proplists` module. So you'll end up doing a lot of this...

{% highlight erlang %}
Opts = [{key, "value"}].
Value = proplists:get_value(key, Opts).
{% endhighlight %}

Or if you happen to have nested proplist...

{% highlight erlang %}
Opts = [{key1, [{key2, "value"}]}].
Value = proplists:get_value(
                    key2,
                    proplists:get_value(key1, Opts)).
{% endhighlight %}

Elixir has a concept of keyword lists. Which are essentially the same as proplists in Erlang. Well, proplists can contain also atoms which are just a shorthand for `{Atom, true}`. But basically they are the same. Elixir syntax just has a lot more convenient way of accessing the data. In Elixir you can...

{% highlight elixir %}
opts = [key: "value"]
value = opts[:key]
{% endhighlight %}

Or with nested...

{% highlight elixir %}
opts = [key1: [key2: "value"]]
value = opts[:key1][:key2]
{% endhighlight %}



#### Updating Nested Maps



Due to the fact that the data structures are immutable you will end up doing something like this when you need to modify a map. Let's say we have a player state where we have player's "wallet" containing all the soft/hard currency the player has and we want to reduce the amount of diamonds by ten. We'd do it like this...

{% highlight elixir %}
state = %{username: "Player1", wallet: %{diamonds: 100, oil: 100, gold: 100}}
wallet = Dict.put(state.wallet, :diamonds, state.wallet.diamonds - 10)
state = Dict.put(state, :wallet, wallet)
{% endhighlight %}

Looks unnecessarily complex doesn't it? Luckily Elixir is all about making things easier for you so it has `Kernel.put_in` and `Kernel.get_in` just for this. So here's how we would use those...

{% highlight elixir %}
state = %{username: "Player1", wallet: %{diamonds: 100, oil: 100, gold: 100}}
state = put_in(state.wallet.diamonds, state.wallet.diamonds - 10)
{% endhighlight %}



#### Pipe Operator



Simple, yet one of the most useful syntax feature in my opinion is the Elixir _pipe operator_. Similar to that found from F# language, pipe operator passes on the output of the previous function as the first parameter of the next function. Let's compare to Erlang once again. Coding in Erlang you easily end up doing plenty of this...

{% highlight erlang %}
List = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9].
%% take out all odd numbers
List1 = lists:filter(fun(X) -> X rem 2 == 0 end, List).
%% power of two
List2 = lists:map(fun(X) -> X * X end, List1).
%% sum of all
Result = lists:foldl(fun(X, Acc) -> X + Acc end, 0, List2).
{% endhighlight %}

And the same using Elixir pipe operator...

{% highlight elixir %}
list = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
result = list
|> Enum.filter(fn(x) -> rem(x, 2) == 0 end)
|> Enum.map(fn(x) -> x * x end)
|> Enum.reduce(0, fn(x, acc) -> x + acc end)
{% endhighlight %}

I just gathered here pretty randomly the syntax related stuff that came to my mind. I really like how it is very clear that Elixir designers have really focused on making a functional language that is easy and pleasant to write and produces easily readable code.



### Tooling



It's common knowledge that Erlang's build/release tools are quite horrible. There are 3rd party tools like [rebar](https://github.com/basho/rebar), [Erlang.mk](https://github.com/ninenines/erlang.mk) and [relx](https://github.com/erlware/relx), which are pretty neat but are not included in Erlang by default and don't help the person writing his first Erlang software at all.

Luckily Elixir has really nice tools included. The standard distribution contains a tool called `mix` that is one of the best I've seen in any language. It lets you bootstrap your project, package it, run tests, install dependencies etc. Really nice. One thing it does not do is create a release like Erlang `reltool` or `relx`. There is a plugin for `mix` that handles this called `[exrm](https://github.com/bitwalker/exrm)`.



### Documentation



One more thing where Elixir beats Erlang is documentation. Everything is well documented and documentation is easily accessible either through the website or even more easily through the Elixir shell. For example, try this in `iex`.

{% highlight elixir %}
iex(1)> h Enum.map
{% endhighlight %}



### Interfacing Erlang Code



The thing that finally got me convinced that Elixir is to be taken seriously is the way it interacts with Erlang code. Well, since Elixir compiles to Erlang it is quite straightforward. You just call Erlang modules and for the most part it just works. I can call Erlang like this...

{% highlight elixir %}
iex(1)> :erlang.now
{1422, 88172, 596308}
{% endhighlight %}

Pretty much the only thing that can lead to problems is that since Elixir strings are Erlang binaries it is good to keep in mind that if some Erlang module takes string as parameter you need to give it Elixir's character list which is the single quoted string in Elixir syntax.



### All in all



I've been extremely happy working with Elixir. I quite recently started as a _Lead Server Programmer_ in a mobile gaming company called [Ministry of Games](http://ministryofgames.io), based in Helsinki, Finland. We're  building our entire application server using Elixir. What is cool is that, while we're using Elixir, we have the whole Erlang ecosystem at our disposal as well. E.g., we're using [ranch](https://github.com/ninenines/ranch) and [locker](https://github.com/wooga/locker) which both are awesome pieces or Erlang software that do some of the most difficult parts of writing distributed servers extremely well, and we can write our application code on top of those using Elixir. A match made in heaven. :)

Ok, now... go give [Elixir](http://elixir-lang.org) a try!
