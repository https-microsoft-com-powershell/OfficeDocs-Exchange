---
title: 'Matching methods and techniques for rule packages: Exchange 2013 Help'
TOCTitle: Matching methods and techniques for rule packages
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer: 
ms.assetid: 09fe9278-d077-452c-b7e5-729b3dc70b1b
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Matching methods and techniques for rule packages in Exchange 2013

_**Applies to:** Exchange Server 2013_

This topic describes techniques for matching pattern and evidence elements within a data loss prevention (DLP) XML file that is designed to contain your own custom sensitive information type rule package. After you have created a well-formed XML file, you can import the file by using the Exchange admin center (EAC) or Exchange Management Shell in order to help create your Microsoft Exchange Server 2013 DLP solution. Before you can make use of the matching methods described here, you should already have a DLP XML file started. For more information about defining your own DLP templates and XML files, see [Define your own DLP templates and information types](define-your-own-dlp-templates-and-information-types-exchange-2013-help.md).

## The Match element

The `Match` element is used within the `Pattern` and `Evidence` elements to represents the underlying keyword, regex or function that is to be matched. The definition of the match itself is stored outside of the `Rule` element and is referenced through the `idRef` required attribute. Multiple `Match` elements can be included in a Pattern definition which can be included directly in the `Pattern` element or combined using the `Any` element to define matching semantics.

```powershell
<?xml version="1.0" encoding="utf-8"?>
<Rules packageId="...">
        ...
<Entity id="...">
  <Pattern confidenceLevel="85">
             <IdMatch idRef="FormattedSSN" />
             <Match idRef="USDate" />
             <Match idRef="USAddress" />
  </Pattern>
</Entity>
        ...
         <Keyword id="FormattedSSN "> ... </Keyword>
         <Regex id=" USDate "> ... </Regex>
         <Regex id="USAddress"> ... </Regex>
        ...
</Rules>
```

## Defining keyword-based matches

A common Rule requirement is to match based on well-known keyword string expressions. This is accomplished by using the `Keyword` element. The Keyword element has an "id" attribute that is used as a reference in the corresponding Entity or Affinity rules. A single Keyword element can be referenced in multiple Entity and Affinity rules.

The matching can be performed using either an exact match or word match based algorithms. Exact match uses a case-sensitive algorithm that searches for the text in the specified language. Word match applies a matching algorithm based on word boundaries. The terms to be matched can be included inline in the Keyword definition by using the Term sub-element.

> [!TIP]
> Use the constant based match style over regex for better efficiency and performance. Use regex matching only in cases where constant based matches are not sufficient and flexibility of regular expressions is required.

```xml
<Keyword id="Word_Example">
    <Group matchStyle="word">
       <Term>card verification</Term>
       <Term>cvn</Term>
       <Term>cid</Term>
       <Term>cvc2</Term>
       <Term>cvv2</Term>
       <Term>pin block</Term>
       <Term>security code</Term>
    </Group>
</Keyword>
...
<Keyword id="String_Example">
    <Group matchStyle="string">
       <Term>card</Term>
       <Term>pin</Term>
       <Term>security</Term>
    </Group>
</Keyword>
```

## Defining regular expression based matches

Another common method of matching is based on regular expressions. The flexibility of regular expression matching makes this a common choice for implementing matches for data such as driver's license numbers, addresses. Common regular expression syntax is used to define the regex patterns. The table here provides examples of some of the most common regular expression tokens available.

> [!TIP]
> Use the constant based match style over regex for better efficiency and performance. Use regex matching only in cases where constant based matches are not sufficient and flexibility of regular expressions is required.

<br>

****

|Symbol|Meaning|
|---|---|
|c|Match the literal character c once, unless it is one of the special characters.|
|^|Match the beginning of a line.|
|.|Match any character that isn't a new line.|
|$|Match the end of a line.|
|||Logical OR between expressions.|
|()|Group sub-expressions.|
|[]|Define a character class.|
|\*|Match the preceding expression zero or more times.|
|+|Match the preceding expression one or more times.|
|?|Match the preceding expression zero or one time.|
|{*n*}|Match the preceding expression *n* times.|
|{*n,*}|Match the preceding expression at least *n* times.|
|{*n, m*}|Match the preceding expression at least *n* times and at most *m* times.|
|\d|Match a digit.|
|\D|Match a character that is not a digit.|
|\w|Match an alpha character, including the underscore.|
|\W|Match a character that is not an alpha character.|
|\s|Match a whitespace character (any of \t, \n, \r, or \f).|
|\S|Match a non-whitespace character.|
|\t|Tab.|
|\n|New line.|
|\r|Carriage return.|
|\f|Form feed.|
|\ *m*|Escape *m*, where *m* is one of the meta characters described above: ^, ., $, \|, (), [], \*, +, ?, \, or /.|
|

The Regex element has an "id" attribute that is used as a reference in the corresponding Entity or Affinity rules. A single Regex element can be referenced in multiple Entity and Affinity rules. The Regex expression is defined as the value of the Regex element.

```xml
<Regex id="CCRegex">
     \bcc\#\s|\bcc\#\:\s
</Regex>
...
<Regex id="ItinFormatted">
    (?:^|[\s\,\:])(?:9\d{2})[- ](?:[78]\d[-
     ]\d{4})(?:$|[\s\,]|\.\s)
</Regex>
...
<Regex id="NorthCarolinaDriversLicenseNumber">
    (^|\s|\:)(\d{1,8})($|\s|\.\s)
</Regex>
```

## Combining multiple match elements

A common technique to increase the matching confidence is to define semantics between multiple Match elements, for example that one or more of the matches need to occur. The Any element allows the definition of the logic based between multiple matches. The Any element can be used as sub-element in the Pattern element. It contains one or more Match children elements and defines the logic of matches between them. See below for XML code examples for the Any elements with all matches, "not" logic, and exact match count.

Optional minMatches attribute can be used (default = 1) to define the minimum number of Match elements that must be met to satisfy a match. Similarly, optional maxMatches attribute can be used (default = number of children Match elements) to define the maximum number of Match elements that must be met to satisfy a match. These conditions are combined using logical operators based on the usage of the minMatches and maxMatches attributes, allowing following semantics:

- Matching all children Match elements

    Not matching any children Match elements

    Matching an exact subset of any children Match elements

```xml
<Any minMatches="3" maxMatches="3">
    <Match idRef="USDate" />
    <Match idRef="USAddress" />
    <Match idRef="Name" />
</Any>
```

```xml
<Any maxMatches="0">
    <Match idRef="USDate" />
    <Match idRef="USAddress" />
    <Match idRef="Name" />
</Any>
```

```xml
<Any minMatches="1" maxMatches="1">
    <Match idRef="USDate" />
    <Match idRef="USAddress" />
    <Match idRef="Name" />
</Any>
```

### Increasing confidence level with more evidence

For entity base rules, another option of increasing confidence is to define multiple Pattern elements, each with increasing number of corroborative evidence. This is achieved by using minMatches and maxMatches for Any element to create independent patterns with increasing confidence level based on increasing number of corroborative evidence. For example:

- if one piece of corroborative evidence is found: confidence level is 65%
- if two pieces: confidence level is 75%
- if three pieces: confidence level is 85%

```xml
<Entity id="..." patternsProximity="300" >
    <Pattern confidenceLevel="65">
        <IdMatch idRef="UnformattedSSN" />
        <Any maxMatches="1">
            <Match idRef="USDate" />
            <Match idRef="USAddress" />
            <Match idRef="Name" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="75">
        <IdMatch idRef="UnformattedSSN" />
        <Any minMatches="2" maxMatches="2">
            <Match idRef="USDate" />
            <Match idRef="USAddress" />
            <Match idRef="Name" />
        </Any>
    </Pattern>
    <Pattern confidenceLevel="85">
        <IdMatch idRef="UnformattedSSN" />
        <Any minMatches="3">
            <Match idRef="USDate" />
            <Match idRef="USAddress" />
            <Match idRef="Name" />
        </Any>
    </Pattern>
</Entity>
```

## Example: US Social Security rule

This section includes an introduction description for authoring of a rule matching a US Social Security number. First, start with a description of how we identify content that contains social security number. A Social Security Number is found if:

1. Regex matches a formatted SSN (and it's in the valid SSN range)
2. Corroborative Evidence one of the following must occur nearby:
   1. Keyword match {Social Security, Soc Sec, SSN, SSNS, SSN#, SS#, SSID}
   2. Text representing a US address
   3. Text representing a date
   4. Text representing a name

Next, translate the description into the Rule schema representation:

```xml
<Entity id="a44669fe-0d48-453d-a9b1-2cc83f2cba77"
         patternsProximity="300" RecommendedConfidence="85">
    <Pattern confidenceLevel="85">
      <IdMatch idRef="FormattedSSN" />
      <Any minMatches="1">
          <Match idRef="SSNKeywords" />
          <Match idRef="USDate" />
          <Match idRef="USAddress" />
          <Match idRef="Name" />
      </Any>
    </Pattern>
</Entity>
```

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)

[Define your own DLP templates and information types](define-your-own-dlp-templates-and-information-types-exchange-2013-help.md)

[Import a custom DLP policy template from a file](import-a-custom-dlp-policy-template-from-a-file-exchange-2013-help.md)
