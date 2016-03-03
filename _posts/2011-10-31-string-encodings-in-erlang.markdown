---
author: teemuharjublog
comments: true
date: 2011-10-31 08:44:53+00:00
layout: post
slug: string-encodings-in-erlang
title: String encodings in Erlang
wordpress_id: 469
categories:
- Erlang
- Programming
---

Erlang is famous for the way it deals with strings. Being that strings are "just a list of integers". Sounds easy, doesn't it? I've been having some issues with Erlang and UTF-8 strings lately and I thought I would write down some of my findings here.

I'm playing with the Erlang shell here so first I need to figure out which encoding my shell is using. This is how I can figure that out.

[code language="erlang"]
1> lists:keyfind(encoding, 1, io:getopts()).
{encoding, unicode}
[/code]

According to the [Erlang manual](http://www.erlang.org/doc/apps/stdlib/unicode_usage.html#id186711), the shell should be able to read and write UTF-8 strings if your environment has been configured properly. Now we can see that our Erlang shell supports Unicode, so we're happy. Let's start playing around with some strings.

Let's start with string "`äiti`" which is a Finnish word for mom (I find myself missing my mom often when dealing with strings in Erlang). By default, Erlang strings use Latin-1 encoding. The list of integers representing this string looks like this.

[code language="erlang"]
1> String = [228, 105, 116, 105].
"äiti"
[/code]

As you can see, Erlang is "clever" enough to show the string represented by this list of integers. These integers represent code points for the characters and since the default encoding in Erlang is Latin-1 the code point for character "`ä`" is `228`. For Latin-1, these code points are integers from 0 to 255. So you can represent 256 different characters with Latin-1. If you're using ASCII encoding your code points are between 0 and 127 (ASCII uses seven bits). Since ASCII is a subset of Latin-1 the characters, "`i`" and "`t`" have same code points in ASCII and Latin-1.

Ok, pretty easy so far. But what happens when 256 characters just is not enough. Say hello to Unicode. With unicode, one can represent basically any number of characters. The code points are not limited to just one byte anymore. Let's stick with the word "`äiti`" still for a while. We know that ASCII is a subset of Latin-1 and actually Latin-1 is also a subset of Unicode. Makes sense, ha? So Latin-1 string "`äiti`" looks in terms of code points exactly the same as Unicode string "`äiti`".

[code language="erlang"]
1> Latin1String = [228, 105, 116, 105].
"äiti"
2> UnicodeString = [228, 105, 116, 105].
"äiti"
[/code]

Now this is just convenient, since the character "`ä`" has the same code point in Latin-1 and Unicode and it can be represented using only one byte. Ok, this looks pretty easy. Nothing can go wrong here since the Latin-1 and Unicode strings look exactly the same. Well, not quite. Since Unicode can represent way more characters than Latin-1 we need to agree on how the Unicode strings are represented on the byte level. You cannot represent the Unicode character "snow man" (☃) with one byte since it's Unicode code point is `9731`. Here we need UTF-8 encoding. It is very commonly used and it is the encoding you need to use nowadays. For example, popular data serialization format JSON assumes that the JSON string is encoded in UTF-8. Ok, let's look at how the string "`äiti`" looks like in UTF-8. The integers here no longer represent the code point in Unicode, but the byte of the UTF-8 string. As you can see we are using Erlang binaries here to represent the string.

[code language="erlang"]
1> Utf8String = <<195, 164, 105, 116, 105>>.
<<"Ã¤iti">>
[/code]

Now Erlang shows the string a bit messed up, since it tries to convert the binary to string using one byte per character and as you can see UTF-8 uses two bytes to represent the character "`ä`" and the string looks to be messed up. How we can deal with this in Erlang? We need to use the `unicode` module.

[code language="erlang"]
2> unicode:characters_to_list(Utf8String, utf8).
"äiti"
[/code]

A very common way to mess up things here is to use `erlang:binary_to_list/1`. The same thing happens as with the shell print out. `binary_to_list/1` converts binary to list byte by byte and now your string has five characters instead of four. This can easily lead to "exploding" strings if you write this string to database and read it from there and decode it again with `binary_to_list/1`.

Like I already mentioned UTF-8 is the "de facto" encoding nowadays. So here are few pointers about how to deal with UTF-8 strings in Erlang.


### Creating strings


If you create strings the usual way (`String = "äiti".`) remember that it is Latin-1 encoded.

If you are reading strings from somewhere as a byte stream, you should use `unicode:characters_to_list/2` and give the function the proper encoding e.g.,

[code language="erlang"]
1> unicode:characters_to_list(<<226,152,131>>, utf8).
[9731]
[/code]

Here we have the UTF-8 encoded binary representation of the "snow man" (☃) character. The output is list with Unicode code point integers. This Unicode string you can use with e.g., functions from the `string` module.


### Convert Unicode strings to binaries


Some functions like `crypto:sha_mac/2` requires the input to be an `iolist()`, which a list that does not care about the encodings but must be just a list of bytes. This cannot take Unicode string as a parameter since those might have integers in them that need more than one byte. So if you're handling strings as Unicode, you will need to convert them to UTF-8 binaries to functions like `crypto:sha_mac/2`. Like this:

[code language="erlang"]
1> UnicodeString = [9731].
[9731]
2> Utf8Bin = unicode:characters_to_binary(UnicodeString, unicode, utf8).
<<226,152,131>>
3> crypto:sha_mac(<<"key">>, Utf8Bin).
[/code]

As a conclusion. Everything should go well if you stick with Unicode strings and remember to encode/decode them to/from UTF-8 every now and then.
