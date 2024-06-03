---
title: Best Practices for Comparing Strings in .NET  Microsoft Docs
feed: show
---

[https://docs.microsoft.com/en-us/dotnet/standard/base-types/best-practices-strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/best-practices-strings)

.NET provides extensive support for developing localized and globalized applications, and makes it easy to apply the conventions of either the current culture or a specific culture when performing common operations such as sorting and displaying strings. But sorting or comparing strings is not always a culture-sensitive operation. For example, strings that are used internally by an application typically should be handled identically across all cultures. When culturally independent string data, such as XML tags, HTML tags, user names, file paths, and the names of system objects, are interpreted as if they were culture-sensitive, application code can be subject to subtle bugs, poor performance, and, in some cases, security issues.

This article examines the string sorting, comparison, and casing methods in .NET, presents recommendations for selecting an appropriate string-handling method, and provides additional information about string-handling methods.

## Recommendations for string usage

When you develop with .NET, follow these simple recommendations when you compare strings:

- Use overloads that explicitly specify the string comparison rules for string operations. Typically, this involves calling a method overload that has a parameter of type [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison).
- Use [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) or [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) for comparisons as your safe default for culture-agnostic string matching.
- Use comparisons with [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) or [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) for better performance.
- Use string operations that are based on [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture) when you display output to the user.
- Use the non-linguistic [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) or [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) values instead of string operations based on [CultureInfo.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture) when the comparison is linguistically irrelevant (symbolic, for example).
- Use the [String.ToUpperInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupperinvariant) method instead of the [String.ToLowerInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.tolowerinvariant) method when you normalize strings for comparison.
- Use an overload of the [String.Equals](https://docs.microsoft.com/en-us/dotnet/api/system.string.equals) method to test whether two strings are equal.
- Use the [String.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) and [String.CompareTo](https://docs.microsoft.com/en-us/dotnet/api/system.string.compareto) methods to sort strings, not to check for equality.
- Use culture-sensitive formatting to display non-string data, such as numbers and dates, in a user interface. Use formatting with the [invariant culture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture#system-globalization-cultureinfo-invariantculture) to persist non-string data in string form.

Avoid the following practices when you compare strings:

- Do not use overloads that do not explicitly or implicitly specify the string comparison rules for string operations.
- Do not use string operations based on [StringComparison.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-invariantculture) in most cases. One of the few exceptions is when you are persisting linguistically meaningful but culturally agnostic data.
- Do not use an overload of the [String.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) or [CompareTo](https://docs.microsoft.com/en-us/dotnet/api/system.string.compareto) method and test for a return value of zero to determine whether two strings are equal.

## Specifying string comparisons explicitly

Most of the string manipulation methods in .NET are overloaded. Typically, one or more overloads accept default settings, whereas others accept no defaults and instead define the precise way in which strings are to be compared or manipulated. Most of the methods that do not rely on defaults include a parameter of type [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison), which is an enumeration that explicitly specifies rules for string comparison by culture and case. The following table describes the [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) enumeration members.

For example, the [IndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof) method, which returns the index of a substring in a [String](https://docs.microsoft.com/en-us/dotnet/api/system.string) object that matches either a character or a string, has nine overloads:

- [IndexOf(Char)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-char)), [IndexOf(Char, Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-char-system-int32)), and [IndexOf(Char, Int32, Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-char-system-int32-system-int32)), which by default perform an ordinal (case-sensitive and culture-insensitive) search for a character in the string.
- [IndexOf(String)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string)), [IndexOf(String, Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string-system-int32)), and [IndexOf(String, Int32, Int32)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string-system-int32-system-int32)), which by default perform a case-sensitive and culture-sensitive search for a substring in the string.
- [IndexOf(String, StringComparison)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string-system-stringcomparison)), [IndexOf(String, Int32, StringComparison)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string-system-int32-system-stringcomparison)), and [IndexOf(String, Int32, Int32, StringComparison)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string-system-int32-system-int32-system-stringcomparison)), which include a parameter of type [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) that allows the form of the comparison to be specified.

We recommend that you select an overload that does not use default values, for the following reasons:

- Some overloads with default parameters (those that search for a [Char](https://docs.microsoft.com/en-us/dotnet/api/system.char) in the string instance) perform an ordinal comparison, whereas others (those that search for a string in the string instance) are culture-sensitive. It is difficult to remember which method uses which default value, and easy to confuse the overloads.
- The intent of the code that relies on default values for method calls is not clear. In the following example, which relies on defaults, it is difficult to know whether the developer actually intended an ordinal or a linguistic comparison of two strings, or whether a case difference between `protocol` and "http" might cause the test for equality to return `false`.

    ```Plain
    string protocol = GetProtocol(url);
    if (String.Equals(protocol, "http")) {
       // ...Code to handle HTTP protocol.
    }
    else {
       throw new InvalidOperationException();
    }
    ```

In general, we recommend that you call a method that does not rely on defaults, because it makes the intent of the code unambiguous. This, in turn, makes the code more readable and easier to debug and maintain. The following example addresses the questions raised about the previous example. It makes it clear that ordinal comparison is used and that differences in case are ignored.

```Plain
string protocol = GetProtocol(url);
if (String.Equals(protocol, "http", StringComparison.OrdinalIgnoreCase)) {
   // ...Code to handle HTTP protocol.
}
else {
   throw new InvalidOperationException();
}
```

## The details of string comparison

String comparison is the heart of many string-related operations, particularly sorting and testing for equality. Strings sort in a determined order: If "my" appears before "string" in a sorted list of strings, "my" must compare less than or equal to "string". Additionally, comparison implicitly defines equality. The comparison operation returns zero for strings it deems equal. A good interpretation is that neither string is less than the other. Most meaningful operations involving strings include one or both of these procedures: comparing with another string, and executing a well-defined sort operation.

Note

You can download the [Sorting Weight Tables](https://www.microsoft.com/download/details.aspx?id=10921), a set of text files that contain information on the character weights used in sorting and comparison operations for Windows operating systems, and the [Default Unicode Collation Element Table](https://www.unicode.org/Public/UCA/latest/allkeys.txt), the latest version of the sort weight table for Linux and macOS. The specific version of the sort weight table on Linux and macOS depends on the version of the [International Components for Unicode](http://site.icu-project.org/) libraries installed on the system. For information on ICU versions and the Unicode versions that they implement, see [Downloading ICU](http://site.icu-project.org/download).

However, evaluating two strings for equality or sort order does not yield a single, correct result; the outcome depends on the criteria used to compare the strings. In particular, string comparisons that are ordinal or that are based on the casing and sorting conventions of the current culture or the [invariant culture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture#system-globalization-cultureinfo-invariantculture) (a locale-agnostic culture based on the English language) may produce different results.

In addition, string comparisons using different versions of .NET or using .NET on different operating systems or operating system versions may return different results. For more information, see [Strings and the Unicode Standard](https://docs.microsoft.com/en-us/dotnet/api/system.string#Unicode).

### String comparisons that use the current culture

One criterion involves using the conventions of the current culture when comparing strings. Comparisons that are based on the current culture use the thread's current culture or locale. If the culture is not set by the user, it defaults to the setting in the **Regional Options** window in Control Panel. You should always use comparisons that are based on the current culture when data is linguistically relevant, and when it reflects culture-sensitive user interaction.

However, comparison and casing behavior in .NET changes when the culture changes. This happens when an application executes on a computer that has a different culture than the computer on which the application was developed, or when the executing thread changes its culture. This behavior is intentional, but it remains non-obvious to many developers. The following example illustrates differences in sort order between the U.S. English ("en-US") and Swedish ("sv-SE") cultures. Note that the words "ångström", "Windows", and "Visual Studio" appear in different positions in the sorted string arrays.

```Plain
using System;
using System.Globalization;
using System.Threading;

public class Example
{
   public static void Main()
   {
      string[] values= { "able", "ångström", "apple", "Æble",
                         "Windows", "Visual Studio" };
      Array.Sort(values);
      DisplayArray(values);

      // Change culture to Swedish (Sweden).
      string originalCulture = CultureInfo.CurrentCulture.Name;
      Thread.CurrentThread.CurrentCulture = new CultureInfo("sv-SE");
      Array.Sort(values);
      DisplayArray(values);

      // Restore the original culture.
      Thread.CurrentThread.CurrentCulture = new CultureInfo(originalCulture);
    }

    private static void DisplayArray(string[] values)
    {
      Console.WriteLine("Sorting using the {0} culture:",
                        CultureInfo.CurrentCulture.Name);
      foreach (string value in values)
         Console.WriteLine("   {0}", value);

      Console.WriteLine();
    }
}
// The example displays the following output:
//       Sorting using the en-US culture:
//          able
//          Æble
//          ångström
//          apple
//          Visual Studio
//          Windows
//
//       Sorting using the sv-SE culture:
//          able
//          Æble
//          apple
//          Windows
//          Visual Studio
//          ångström
```

Case-insensitive comparisons that use the current culture are the same as culture-sensitive comparisons, except that they ignore case as dictated by the thread's current culture. This behavior may manifest itself in sort orders as well.

Comparisons that use current culture semantics are the default for the following methods:

- [String.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) overloads that do not include a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter.
- [String.CompareTo](https://docs.microsoft.com/en-us/dotnet/api/system.string.compareto) overloads.
- The default [String.StartsWith(String)](https://docs.microsoft.com/en-us/dotnet/api/system.string.startswith#system-string-startswith(system-string)) method, and the [String.StartsWith(String, Boolean, CultureInfo)](https://docs.microsoft.com/en-us/dotnet/api/system.string.startswith#system-string-startswith(system-string-system-boolean-system-globalization-cultureinfo)) method with a `null`[CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo) parameter.
- [String.IndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof) overloads that accept a [String](https://docs.microsoft.com/en-us/dotnet/api/system.string) as a search parameter and that do not have a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter.
- [String.LastIndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.lastindexof) overloads that accept a [String](https://docs.microsoft.com/en-us/dotnet/api/system.string) as a search parameter and that do not have a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter.

In any case, we recommend that you call an overload that has a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter to make the intent of the method call clear.

Subtle and not so subtle bugs can emerge when non-linguistic string data is interpreted linguistically, or when string data from a particular culture is interpreted using the conventions of another culture. The canonical example is the Turkish-I problem.

For nearly all Latin alphabets, including U.S. English, the character "i" (\u0069) is the lowercase version of the character "I" (\u0049). This casing rule quickly becomes the default for someone programming in such a culture. However, the Turkish ("tr-TR") alphabet includes an "I with a dot" character "İ" (\u0130), which is the capital version of "i". Turkish also includes a lowercase "i without a dot" character, "ı" (\u0131), which capitalizes to "I". This behavior occurs in the Azerbaijani ("az") culture as well.

Therefore, assumptions made about capitalizing "i" or lowercasing "I" are not valid among all cultures. If you use the default overloads for string comparison routines, they will be subject to variance between cultures. If the data to be compared is non-linguistic, using the default overloads can produce undesirable results, as the following attempt to perform a case-insensitive comparison of the strings "file" and "FILE" illustrates.

```Plain
using System;
using System.Globalization;
using System.Threading;

public class Example
{
   public static void Main()
   {
      string fileUrl = "file";
      Thread.CurrentThread.CurrentCulture = new CultureInfo("en-US");
      Console.WriteLine("Culture = {0}",
                        Thread.CurrentThread.CurrentCulture.DisplayName);
      Console.WriteLine("(file == FILE) = {0}",
                       fileUrl.StartsWith("FILE", true, null));
      Console.WriteLine();

      Thread.CurrentThread.CurrentCulture = new CultureInfo("tr-TR");
      Console.WriteLine("Culture = {0}",
                        Thread.CurrentThread.CurrentCulture.DisplayName);
      Console.WriteLine("(file == FILE) = {0}",
                        fileUrl.StartsWith("FILE", true, null));
   }
}
// The example displays the following output:
//       Culture = English (United States)
//       (file == FILE) = True
//
//       Culture = Turkish (Turkey)
//       (file == FILE) = False
```

This comparison could cause significant problems if the culture is inadvertently used in security-sensitive settings, as in the following example. A method call such as `IsFileURI("file:")` returns `true` if the current culture is U.S. English, but `false` if the current culture is Turkish. Thus, on Turkish systems, someone could circumvent security measures that block access to case-insensitive URIs that begin with "FILE:".

```Plain
public static bool IsFileURI(String path)
{
   return path.StartsWith("FILE:", true, null);
}
```

In this case, because "file:" is meant to be interpreted as a non-linguistic, culture-insensitive identifier, the code should instead be written as shown in the following example:

```Plain
public static bool IsFileURI(string path)
{
   return path.StartsWith("FILE:", StringComparison.OrdinalIgnoreCase);
}
```

### Ordinal string operations

Specifying the [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) or [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) value in a method call signifies a non-linguistic comparison in which the features of natural languages are ignored. Methods that are invoked with these [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) values base string operation decisions on simple byte comparisons instead of casing or equivalence tables that are parameterized by culture. In most cases, this approach best fits the intended interpretation of strings while making code faster and more reliable.

Ordinal comparisons are string comparisons in which each byte of each string is compared without linguistic interpretation; for example, "windows" does not match "Windows". This is essentially a call to the C runtime `strcmp` function. Use this comparison when the context dictates that strings should be matched exactly or demands conservative matching policy. Additionally, ordinal comparison is the fastest comparison operation because it applies no linguistic rules when determining a result.

Strings in .NET can contain embedded null characters. One of the clearest differences between ordinal and culture-sensitive comparison (including comparisons that use the invariant culture) concerns the handling of embedded null characters in a string. These characters are ignored when you use the [String.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) and [String.Equals](https://docs.microsoft.com/en-us/dotnet/api/system.string.equals) methods to perform culture-sensitive comparisons (including comparisons that use the invariant culture). As a result, in culture-sensitive comparisons, strings that contain embedded null characters can be considered equal to strings that do not.

The following example performs a culture-sensitive comparison of the string "Aa" with a similar string that contains several embedded null characters between "A" and "a", and shows how the two strings are considered equal:

```Plain
using System;

public class Example
{
   public static void Main()
   {
      string str1 = "Aa";
      string str2 = "A" + new String('\u0000', 3) + "a";
      Console.WriteLine("Comparing '{0}' ({1}) and '{2}' ({3}):",
                        str1, ShowBytes(str1), str2, ShowBytes(str2));
      Console.WriteLine("   With String.Compare:");
      Console.WriteLine("      Current Culture: {0}",
                        String.Compare(str1, str2, StringComparison.CurrentCulture));
      Console.WriteLine("      Invariant Culture: {0}",
                        String.Compare(str1, str2, StringComparison.InvariantCulture));

      Console.WriteLine("   With String.Equals:");
      Console.WriteLine("      Current Culture: {0}",
                        String.Equals(str1, str2, StringComparison.CurrentCulture));
      Console.WriteLine("      Invariant Culture: {0}",
                        String.Equals(str1, str2, StringComparison.InvariantCulture));
   }

   private static string ShowBytes(string str)
   {
      string hexString = String.Empty;
      for (int ctr = 0; ctr < str.Length; ctr++)
      {
         string result = String.Empty;
         result = Convert.ToInt32(str[ctr]).ToString("X4");
         result = " " + result.Substring(0,2) + " " + result.Substring(2, 2);
         hexString += result;
      }
      return hexString.Trim();
   }
}
// The example displays the following output:
//    Comparing 'Aa' (00 41 00 61) and 'A   a' (00 41 00 00 00 00 00 00 00 61):
//       With String.Compare:
//          Current Culture: 0
//          Invariant Culture: 0
//       With String.Equals:
//          Current Culture: True
//          Invariant Culture: True
```

However, the strings are not considered equal when you use ordinal comparison, as the following example shows:

```Plain
Console.WriteLine("Comparing '{0}' ({1}) and '{2}' ({3}):",
                  str1, ShowBytes(str1), str2, ShowBytes(str2));
Console.WriteLine("   With String.Compare:");
Console.WriteLine("      Ordinal: {0}",
                  String.Compare(str1, str2, StringComparison.Ordinal));

Console.WriteLine("   With String.Equals:");
Console.WriteLine("      Ordinal: {0}",
                  String.Equals(str1, str2, StringComparison.Ordinal));
// The example displays the following output:
//    Comparing 'Aa' (00 41 00 61) and 'A   a' (00 41 00 00 00 00 00 00 00 61):
//       With String.Compare:
//          Ordinal: 97
//       With String.Equals:
//          Ordinal: False
```

Case-insensitive ordinal comparisons are the next most conservative approach. These comparisons ignore most casing; for example, "windows" matches "Windows". When dealing with ASCII characters, this policy is equivalent to [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal), except that it ignores the usual ASCII casing. Therefore, any character in [A,Z] (\u0041-\u005A) matches the corresponding character in [a,z] (\u0061-\007A). Casing outside the ASCII range uses the invariant culture's tables. Therefore, the following comparison:

```Plain
String.Compare(strA, strB, StringComparison.OrdinalIgnoreCase);
```

is equivalent to (but faster than) this comparison:

```Plain
String.Compare(strA.ToUpperInvariant(), strB.ToUpperInvariant(),
               StringComparison.Ordinal);
```

These comparisons are still very fast.

Both [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) and [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) use the binary values directly, and are best suited for matching. When you are not sure about your comparison settings, use one of these two values. However, because they perform a byte-by-byte comparison, they do not sort by a linguistic sort order (like an English dictionary) but by a binary sort order. The results may look odd in most contexts if displayed to users.

Ordinal semantics are the default for [String.Equals](https://docs.microsoft.com/en-us/dotnet/api/system.string.equals) overloads that do not include a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) argument (including the equality operator). In any case, we recommend that you call an overload that has a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter.

### String operations that use the invariant culture

Comparisons with the invariant culture use the [CompareInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.compareinfo) property returned by the static [CultureInfo.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture) property. This behavior is the same on all systems; it translates any characters outside its range into what it believes are equivalent invariant characters. This policy can be useful for maintaining one set of string behavior across cultures, but it often provides unexpected results.

Case-insensitive comparisons with the invariant culture use the static [CompareInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.compareinfo) property returned by the static [CultureInfo.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.invariantculture) property for comparison information as well. Any case differences among these translated characters are ignored.

Comparisons that use [StringComparison.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-invariantculture) and [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) work identically on ASCII strings. However, [StringComparison.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-invariantculture) makes linguistic decisions that might not be appropriate for strings that have to be interpreted as a set of bytes. The `CultureInfo.InvariantCulture.CompareInfo` object makes the [Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) method interpret certain sets of characters as equivalent. For example, the following equivalence is valid under the invariant culture:

InvariantCulture: a + ̊ = å

The LATIN SMALL LETTER A character "a" (\u0061), when it is next to the COMBINING RING ABOVE character "+ " ̊" (\u030a), is interpreted as the LATIN SMALL LETTER A WITH RING ABOVE character "å" (\u00e5). As the following example shows, this behavior differs from ordinal comparison.

```Plain
string separated = "\u0061\u030a";
string combined = "\u00e5";

Console.WriteLine("Equal sort weight of {0} and {1} using InvariantCulture: {2}",
                  separated, combined,
                  String.Compare(separated, combined,
                                 StringComparison.InvariantCulture) == 0);

Console.WriteLine("Equal sort weight of {0} and {1} using Ordinal: {2}",
                  separated, combined,
                  String.Compare(separated, combined,
                                 StringComparison.Ordinal) == 0);
// The example displays the following output:
//    Equal sort weight of a° and å using InvariantCulture: True
//    Equal sort weight of a° and å using Ordinal: False
```

When interpreting file names, cookies, or anything else where a combination such as "å" can appear, ordinal comparisons still offer the most transparent and fitting behavior.

On balance, the invariant culture has very few properties that make it useful for comparison. It does comparison in a linguistically relevant manner, which prevents it from guaranteeing full symbolic equivalence, but it is not the choice for display in any culture. One of the few reasons to use [StringComparison.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-invariantculture) for comparison is to persist ordered data for a cross-culturally identical display. For example, if a large data file that contains a list of sorted identifiers for display accompanies an application, adding to this list would require an insertion with invariant-style sorting.

## Choosing a StringComparison member for your method call

The following table outlines the mapping from semantic string context to a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) enumeration member:

|Case-sensitive internal identifiers. Case-sensitive identifiers in standards such as XML and HTTP. Case-sensitive security-related settings.|A non-linguistic identifier, where bytes match exactly.|Ordinal|
|---|---|---|
|[[Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive Case-insensitive...]]|A non-linguistic identifier, where case is irrelevant; especially data stored in most Windows system services.|[OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase)|

## Common string comparison methods in .NET

The following sections describe the methods that are most commonly used for string comparison.

### String.Compare

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

As the operation most central to string interpretation, all instances of these method calls should be examined to determine whether strings should be interpreted according to the current culture, or dissociated from the culture (symbolically). Typically, it is the latter, and a [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal) comparison should be used instead.

The [System.Globalization.CompareInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.compareinfo) class, which is returned by the [CultureInfo.CompareInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo.compareinfo) property, also includes a [Compare](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.compareinfo.compare) method that provides a large number of matching options (ordinal, ignoring white space, ignoring kana type, and so on) by means of the [CompareOptions](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.compareoptions) flag enumeration.

### String.CompareTo

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

This method does not currently offer an overload that specifies a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) type. It is usually possible to convert this method to the recommended [String.Compare(String, String, StringComparison)](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare#system-string-compare(system-string-system-string-system-stringcomparison)) form.

Types that implement the [IComparable](https://docs.microsoft.com/en-us/dotnet/api/system.icomparable) and [IComparable< T >](https://docs.microsoft.com/en-us/dotnet/api/system.icomparable-1) interfaces implement this method. Because it does not offer the option of a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) parameter, implementing types often let the user specify a [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) in their constructor. The following example defines a `FileName` class whose class constructor includes a [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) parameter. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is then used in the `FileName.CompareTo` method.

```Plain
using System;

public class FileName : IComparable
{
   string fname;
   StringComparer comparer;

   public FileName(string name, StringComparer comparer)
   {
      if (String.IsNullOrEmpty(name))
         throw new ArgumentNullException("name");

      this.fname = name;

      if (comparer != null)
         this.comparer = comparer;
      else
         this.comparer = StringComparer.OrdinalIgnoreCase;
   }

   public string Name
   {
      get { return fname; }
   }

   public int CompareTo(object obj)
   {
      if (obj == null) return 1;

      if (! (obj is FileName))
         return comparer.Compare(this.fname, obj.ToString());
      else
         return comparer.Compare(this.fname, ((FileName) obj).Name);
   }
}
```

### String.Equals

Default interpretation: [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal).

The [String](https://docs.microsoft.com/en-us/dotnet/api/system.string) class lets you test for equality by calling either the static or instance [Equals](https://docs.microsoft.com/en-us/dotnet/api/system.string.equals) method overloads, or by using the static equality operator. The overloads and operator use ordinal comparison by default. However, we still recommend that you call an overload that explicitly specifies the [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) type even if you want to perform an ordinal comparison; this makes it easier to search code for a certain string interpretation.

### String.ToUpper and String.ToLower

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

Be careful when you use the [String.ToUpper()](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupper#system-string-toupper) and [String.ToLower()](https://docs.microsoft.com/en-us/dotnet/api/system.string.tolower#system-string-tolower) methods, because forcing a string to a uppercase or lowercase is often used as a small normalization for comparing strings regardless of case. If so, consider using a case-insensitive comparison.

The [String.ToUpperInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupperinvariant) and [String.ToLowerInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.tolowerinvariant) methods are also available. [ToUpperInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupperinvariant) is the standard way to normalize case. Comparisons made using [StringComparison.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinalignorecase) are behaviorally the composition of two calls: calling [ToUpperInvariant](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupperinvariant) on both string arguments, and doing a comparison using [StringComparison.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-ordinal).

Overloads are also available for converting to uppercase and lowercase in a specific culture, by passing a [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo) object that represents that culture to the method.

### Char.ToUpper and Char.ToLower

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

The [Char.ToUpper(Char)](https://docs.microsoft.com/en-us/dotnet/api/system.char.toupper#system-char-toupper(system-char)) and [Char.ToLower(Char)](https://docs.microsoft.com/en-us/dotnet/api/system.char.tolower#system-char-tolower(system-char)) methods work similarly to the [String.ToUpper()](https://docs.microsoft.com/en-us/dotnet/api/system.string.toupper#system-string-toupper) and [String.ToLower()](https://docs.microsoft.com/en-us/dotnet/api/system.string.tolower#system-string-tolower) methods described in the previous section.

### String.StartsWith and String.EndsWith

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

By default, both of these methods perform a culture-sensitive comparison.

### String.IndexOf and String.LastIndexOf

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

There is a lack of consistency in how the default overloads of these methods perform comparisons. All [String.IndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof) and [String.LastIndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.lastindexof) methods that include a [Char](https://docs.microsoft.com/en-us/dotnet/api/system.char) parameter perform an ordinal comparison, but the default [String.IndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof) and [String.LastIndexOf](https://docs.microsoft.com/en-us/dotnet/api/system.string.lastindexof) methods that include a [String](https://docs.microsoft.com/en-us/dotnet/api/system.string) parameter perform a culture-sensitive comparison.

If you call the [String.IndexOf(String)](https://docs.microsoft.com/en-us/dotnet/api/system.string.indexof#system-string-indexof(system-string)) or [String.LastIndexOf(String)](https://docs.microsoft.com/en-us/dotnet/api/system.string.lastindexof#system-string-lastindexof(system-string)) method and pass it a string to locate in the current instance, we recommend that you call an overload that explicitly specifies the [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) type. The overloads that include a [Char](https://docs.microsoft.com/en-us/dotnet/api/system.char) argument do not allow you to specify a [StringComparison](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison) type.

## Methods that perform string comparison indirectly

Some non-string methods that have string comparison as a central operation use the [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) type. The [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) class includes six static properties that return [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) instances whose [StringComparer.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.compare) methods perform the following types of string comparisons:

- Culture-sensitive string comparisons using the current culture. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.currentculture) property.
- Case-insensitive comparisons using the current culture. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.CurrentCultureIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.currentcultureignorecase) property.
- Culture-insensitive comparisons using the word comparison rules of the invariant culture. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.invariantculture) property.
- Case-insensitive and culture-insensitive comparisons using the word comparison rules of the invariant culture. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.InvariantCultureIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.invariantcultureignorecase) property.
- Ordinal comparison. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.Ordinal](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.ordinal) property.
- Case-insensitive ordinal comparison. This [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object is returned by the [StringComparer.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.ordinalignorecase) property.

### Array.Sort and Array.BinarySearch

Default interpretation: [StringComparison.CurrentCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-currentculture).

When you store any data in a collection, or read persisted data from a file or database into a collection, switching the current culture can invalidate the invariants in the collection. The [Array.BinarySearch](https://docs.microsoft.com/en-us/dotnet/api/system.array.binarysearch) method assumes that the elements in the array to be searched are already sorted. To sort any string element in the array, the [Array.Sort](https://docs.microsoft.com/en-us/dotnet/api/system.array.sort) method calls the [String.Compare](https://docs.microsoft.com/en-us/dotnet/api/system.string.compare) method to order individual elements. Using a culture-sensitive comparer can be dangerous if the culture changes between the time that the array is sorted and its contents are searched. For example, in the following code, storage and retrieval operate on the comparer that is provided implicitly by the `Thread.CurrentThread.CurrentCulture` property. If the culture can change between the calls to `StoreNames` and `DoesNameExist`, and especially if the array contents are persisted somewhere between the two method calls, the binary search may fail.

```Plain
// Incorrect.
string []storedNames;

public void StoreNames(string [] names)
{
   int index = 0;
   storedNames = new string[names.Length];

   foreach (string name in names)
   {
      this.storedNames[index++] = name;
   }

   Array.Sort(names); // Line A.
}

public bool DoesNameExist(string name)
{
   return (Array.BinarySearch(this.storedNames, name) >= 0);  // Line B.
}
```

A recommended variation appears in the following example, which uses the same ordinal (culture-insensitive) comparison method both to sort and to search the array. The change code is reflected in the lines labeled `Line A` and `Line B` in the two examples.

```Plain
// Correct.
string []storedNames;

public void StoreNames(string [] names)
{
   int index = 0;
   storedNames = new string[names.Length];

   foreach (string name in names)
   {
      this.storedNames[index++] = name;
   }

   Array.Sort(names, StringComparer.Ordinal);  // Line A.
}

public bool DoesNameExist(string name)
{
   return (Array.BinarySearch(this.storedNames, name, StringComparer.Ordinal) >= 0);  // Line B.
}
```

If this data is persisted and moved across cultures, and sorting is used to present this data to the user, you might consider using [StringComparison.InvariantCulture](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparison#system-stringcomparison-invariantculture), which operates linguistically for better user output but is unaffected by changes in culture. The following example modifies the two previous examples to use the invariant culture for sorting and searching the array.

```Plain
// Correct.
string []storedNames;

public void StoreNames(string [] names)
{
   int index = 0;
   storedNames = new string[names.Length];

   foreach (string name in names)
   {
      this.storedNames[index++] = name;
   }

   Array.Sort(names, StringComparer.InvariantCulture);  // Line A.
}

public bool DoesNameExist(string name)
{
   return (Array.BinarySearch(this.storedNames, name, StringComparer.InvariantCulture) >= 0);  // Line B.
}
```

### Collections example: Hashtable constructor

Hashing strings provides a second example of an operation that is affected by the way in which strings are compared.

The following example instantiates a [Hashtable](https://docs.microsoft.com/en-us/dotnet/api/system.collections.hashtable) object by passing it the [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) object that is returned by the [StringComparer.OrdinalIgnoreCase](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer.ordinalignorecase) property. Because a class [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) that is derived from [StringComparer](https://docs.microsoft.com/en-us/dotnet/api/system.stringcomparer) implements the [IEqualityComparer](https://docs.microsoft.com/en-us/dotnet/api/system.collections.iequalitycomparer) interface, its [GetHashCode](https://docs.microsoft.com/en-us/dotnet/api/system.collections.iequalitycomparer.gethashcode) method is used to compute the hash code of strings in the hash table.

```Plain
const int initialTableCapacity = 100;
Hashtable h;

public void PopulateFileTable(string directory)
{
   h = new Hashtable(initialTableCapacity,
                     StringComparer.OrdinalIgnoreCase);

   foreach (string file in Directory.GetFiles(directory))
         h.Add(file, File.GetCreationTime(file));
}

public void PrintCreationTime(string targetFile)
{
   Object dt = h[targetFile];
   if (dt != null)
   {
      Console.WriteLine("File {0} was created at time {1}.",
         targetFile,
         (DateTime) dt);
   }
   else
   {
      Console.WriteLine("File {0} does not exist.", targetFile);
   }
}
```

## See also

- [Globalization in .NET apps](https://docs.microsoft.com/en-us/dotnet/core/extensions/globalization)

## Recommended content

## Feedback
