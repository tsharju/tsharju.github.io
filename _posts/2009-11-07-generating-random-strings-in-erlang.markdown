---
author: teemuharjublog
comments: true
date: 2009-11-07 05:44:20+00:00
layout: post
slug: generating-random-strings-in-erlang
title: Generating random strings in Erlang
wordpress_id: 287
categories:
- Erlang
- Programming
tags:
- Erlang
- Programming
---

I could not find any decent examples from the web on how to generate a random string with a certain set of characters and length in Erlang. The basic idea for such a method is to take a string of allowed characters and loop N times where the N is the length of the resulting string. Then at each loop we take some random character from the string that contains the required set of characters. Sounds relatively simple, right? Next we have to write this in Erlang. This is what I came up with...

[code language="erlang"]
get_random_string(Length, AllowedChars) ->
    lists:foldl(fun(_, Acc) ->
                        [lists:nth(random:uniform(length(AllowedChars)),
                                   AllowedChars)]
                            ++ Acc
                end, [], lists:seq(1, Length)).
[/code]

Ok, Erlang is not the most readable language in the world and a simple thing such as generating a random string can look pretty tedious. No worries. I'll go through the method line by line.

I'm using the `lists:foldl` method here. What it does is that it goes through a list (from left to right) and calls a function that has as it's parameter a value from that list and the result form the previous iteration. The result of the method is the result of the last call to the function. The list I give as a parameter to `lists:foldl` is a sequence of numbers from one to the length of the resulting random string. For that I use the `lists:seq` method. This is how we define how many times we loop.

I'll explain the `fun()` that is the first parameter of `lists:foldl`. Here is what it looks like separate from the whole code.

[code language="erlang"]
fun(_, Acc) ->
     [lists:nth(random:uniform(length(AllowedChars)), AllowedChars)]
          ++ Acc
end
[/code]

The first parameter of the function is the value from the given list (`[1, 2, 3, 4,..., N]`) and we don't use it (hence the underscore). The second parameter `Acc` is called the accumulator that is the result from the previous iteration. To achieve our goal of producing random strings we use `lists:nth` and `random:uniform` method calls to pick a random character from the `AllowedChars` string. Note that the `lists:nth` returns the integer value of that character so that is why the method call is wrapped in square brackets making the result a string (in Erlang strings are lists of integers). What we do then is that we add the` Acc` (the result of the previous iteration) to the result and this way build our random string.

There is also a third parameter for the `lists:foldl` method that you probably have guessed already. Naturally, you also have to give the value of the accumulator for the first iteration, which in this case is empty list `[]` or empty string since strings in Erlang are actually lists.

Here is an example of the result that the method produces.

[code language="erlang"]
test:get_random_string(32, "qwertyQWERTY1234567890").     
"8qttW01wQET1qRTt1r4tr2T392QY94Re"
[/code]
