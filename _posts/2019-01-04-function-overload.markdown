---
layout: post
title:  "F# - Function overload"
subtitle: "Little win of Friday"
date:   2019-01-04 11:54:00
categories: [fsharp]
tags: [fsharp, aws, lambda]
---

I recently updated one of our Lambda functions from .NET Core version from 2.0 to 2.1 and suddenly got this error message in the following code snippet:

{% highlight fsharp %}
let tryParseInt =
    Int32.TryParse >> function
    | true, v -> Some v
    | false, _ -> None
{% endhighlight %}

"A unique overload for method 'TryParse' could not be determined based on type information prior to this program point. A type annotation may be needed. Candidates: Int32.TryParse(s: ReadOnlySpan<char>, result: byref<int>) : bool, Int32.TryParse(s: string, result: byref<int>) : bool"

Mhhh, interesting...

After a short research I found out that I can directly specify which overload I want to use of the given function.

This did the trick
{% highlight fsharp %}
let tryParseInt =
    (Int32.TryParse : string -> bool * int) >> function
    | true, v -> Some v
    | false, _ -> None
{% endhighlight %}