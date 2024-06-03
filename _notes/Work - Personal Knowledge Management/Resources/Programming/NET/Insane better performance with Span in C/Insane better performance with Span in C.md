---
title: Insane better performance with Span in C
feed: show
---

[https://medium.com/@johnklaumann/insane-better-performance-with-span-in-c-e485ef176bb](https://medium.com/@johnklaumann/insane-better-performance-with-span-in-c-e485ef176bb)

![[assets/0xYPhkLVhdKEugCy_.gif]]

Howdy performance lovers, it is time to optimize your code and have the faster application of the wild wild west.

![[assets/0xYPhkLVhdKEugCy_ 1.gif]]

This article will help you to increase your expertise on performance, so that you will be able to create a Story on your backlog with the name “Improve Performance on Service XXX” and do your MAGIC and reduce not only memory but also that precious golden milliseconds on the response of your endpoints from your application.

Reducing memory and time equals reduce money on infra structure that makes all managers happy.

![[0gN_LZRBAQjIyt7iI.gif]]

## SPAN

Let’s meet [Span](https://learn.microsoft.com/en-us/dotnet/api/system.readonlyspan-1?view=net-7.0)!

Ok, so, what is Span?

“Oh, I know I know!!! Is that Html tag used for phrasing content on FrontEnd!”.

![[assets/1wqEp1ro439wWTlOb_NcL0w.png]]

A Span<> is **an allocation-free representation of contiguous regions of arbitrary memory**.

Nice, what does it mean in human language? FREE ALLOCATION!!!

That means that using Span will not allocate any memory and will sent Garbage Collector on vacation for a few days.

Let’s try it out and see if works.

For example, on your application you have to deal with a logic that uses ‘SubString’ to be able to manage strings positions.

Something like this:

```Plain
namespace Span
{
 class Program
 {
 private static readonly string someString = "01 02 03";

 static void Main(string[] args)
 {
 var numbers = ExtractNumbers();
 Console.WriteLine(numbers);
 }

 public static (int pos1, int pos2, int pos3) ExtractNumbers()
 {
 var strNumber1 = someString.Substring(0, 2);
 var strNumber2 = someString.Substring(2, 4);
 var strNumber3 = someString.Substring(6);

 return (int.Parse(strNumber1), int.Parse(strNumber2), int.Parse(strNumber3));

 }
 }
}
```

So what we have here is extracting numbers from a string and then writing to the console and the result is equal this:

![[assets/141vew_wfvjfbrRKh4QBDgQ.png]]

Very well, now let’s try the new Span approach with the same logic and see the results.

```Plain
namespace Span
{
 class Program
 {
 private static readonly string someString = "01 02 03";

 static void Main(string[] args)
 {
 var numbers = ExtractNumbersWithSpan();
 Console.WriteLine(numbers);
 }

 public static (int pos1, int pos2, int pos3) ExtractNumbersWithSpan()
 {
 ReadOnlySpan<char> spanString = someString;
 var strNumber1 = spanString.Slice(0, 2);
 var strNumber2 = spanString.Slice(2, 4);
 var strNumber3 = spanString.Slice(6);

 return (int.Parse(strNumber1), int.Parse(strNumber2), int.Parse(strNumber3));
 }
 }
}
```

Using Span and changing ‘SubString’ to ‘Slice’ we will have the same result:

Pretty simple right?

Yeah, ok, but how this is more performative than ‘Substring’??

Let’s check it out, IT IS TIME FOR BENCHMARK!!!

![[assets/0He_WLmWD_J-TavZi.gif]]

First install BenchmarkDotNet and don’t forget to put your solution to Release mode to Run BenchMark.

```Plain
using BenchmarkDotNet.Attributes;
using BenchmarkDotNet.Running;

namespace Span
{
 class Program
 {
 static void Main(string[] args)
 {
 BenchmarkRunner.Run<Bench>();
 }
 }


 [MemoryDiagnoser]
 public class Bench
 {
 public static readonly string someString = "01 02 03";

 [Benchmark]
 public (int pos1, int pos2, int pos3) ExtractNumbers()
 {
 var strNumber1 = someString.Substring(0, 2);
 var strNumber2 = someString.Substring(2, 4);
 var strNumber3 = someString.Substring(6);

 return (int.Parse(strNumber1), int.Parse(strNumber2), int.Parse(strNumber3));
 }

 [Benchmark]
 public (int pos1, int pos2, int pos3) ExtractNumbersWithSpan()
 {
 ReadOnlySpan<char> spanString = someString;
 var strNumber1 = spanString.Slice(0, 2);
 var strNumber2 = spanString.Slice(2, 4);
 var strNumber3 = spanString.Slice(6);

 return (int.Parse(strNumber1), int.Parse(strNumber2), int.Parse(strNumber3));
 }
 }
}
```

As you can see now we have a new class called “Bench” and the two methods with “BenchMark” attribute and MemoryDiagnoser on the Bench Class, the rest remains the same and when running this we will have…

Time for the truth!!

![[assets/1wnpHkCHaTrIhmc8f4IPnYA.png]]

![[assets/0peCUTHk0O4Oq1ITn.gif]]

Freaking lord of the dragons with his insane magic, what is thisss????

How we can see guys, Span is almost two times faster and does not allocate memory at all!!!

It’s time to put that on your applications and make your company save some dollars.

Span works with different types, not only string, you can use for byte for example or other use case that you have on your application.

What are you waiting for, are you still going to use that expensive substring on your code???

Guys, hope that you learn something and had fun on the way!!!

See you on the next article :)
