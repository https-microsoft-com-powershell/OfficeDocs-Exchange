---
title: 'Customize the built-in DLP sensitive information types: Exchange 2013 Help'
TOCTitle: Customize the built-in DLP sensitive information types
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer:
ms.assetid: 3f8bf141-2e7c-4ea7-b102-dfd6c41539da
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Customize the built-in DLP sensitive information types in Exchange 2013

_**Applies to:** Exchange Server 2013_

When looking for sensitive information in email, you need to describe that information in what's called a rule. Data loss prevention (DLP) includes a pack of 51 rules for the most-common sensitive information types that you can use right away. To use these rules, you have to include them in a policy. You might find that you want to adjust these built-in rules to meet your organization's specific needs, and you can do that by creating a custom sensitive information type. This topic shows you how to customize the XML file that contains the existing rule collection to detect a wider range of potential credit-card information.

You can take this example and apply it to other built-in sensitive information types. For a list of default sensitive information types and XML definitions, see the [Sensitive information types in Exchange Server](../ExchangeServer/policy-and-compliance/data-loss-prevention/sensitive-information-types.md) topic.

This topic guides you through the following sections for XML rule customizations:

- [Export the XML file of the current rules](#export-the-xml-file-of-the-current-rules)

- [Find the rule that you want to modify in the XML](#find-the-rule-that-you-want-to-modify-in-the-xml)

- [Modify the XML and create a new sensitive information type](#modify-the-xml-and-create-a-new-sensitive-information-type)

- [Remove the corroborative evidence requirement from a sensitive information type](#remove-the-corroborative-evidence-requirement-from-a-sensitive-information-type)

- [Look for keywords that are specific to your organization](#look-for-keywords-that-are-specific-to-your-organization)

- [Upload your rule](#upload-your-rule)

To learn what the different parts of rules are and what they do, check out the [Term glossary](#term-glossary) at the end of this topic.

## Export the XML file of the current rules

To export the XML, you need to use the Exchange Management Shell. For more information, see [Exchange Management Shell](/powershell/exchange/exchange-management-shell).

1. In the Exchange Management Shell, type `Get-ClassificationRuleCollection` to display your organization's rules on screen. If you haven't created your own, you'll only see the default, built-in rules, labeled "Microsoft Rule Package."

2. Store your organization's rules in a in a variable by typing `$ruleCollections = Get-ClassificationRuleCollection`. Storing something in a variable makes it easily available later in a format that works for remote PowerShell commands.

3. Make a formatted XML file with all that data by replacing `"C:\custompath\` with a real file path and typing `Set-Content -Path "C:\custompath\exportedRules.xml" -Encoding Byte -Value $ruleCollections.SerializedClassificationRuleCollection`. (**Set-content** is the part of the cmdlet that writes the XML to the file.)

## Find the rule that you want to modify in the XML

The cmdlets above exported the entire rule collection, which includes the 51 default rules we provide. Next you'll need to look specifically for the Credit Card Number rule that you want to modify.

1. Use a text editor to open the XML file that you exported in the previous section.

2. Scroll down to the **\<Rules\>** tag, which is the start of the section that contains the DLP rules. (Because this XML file contains the information for the entire rule collection, it contains other information at the top that you need to scroll past to get to the rules.)

3. Look for **Func_credit_card** to find the Credit Card Number rule definition. (In the XML, rule names can't contain spaces, so the spaces are usually replaced with underscores, and rule names are sometimes abbreviated. An example of this is the U.S. Social Security number rule, which is abbreviated "SSN." The Credit Card Number rule XML should look like the following code sample.

   ```powershell
   <Entity id="50842eb7-edc8-4019-85dd-5a5c1f2bb085"
          patternsProximity="300" recommendedConfidence="85">
         <Pattern confidenceLevel="85">
          <IdMatch idRef="Func_credit_card" />
           <Any minMatches="1">
             <Match idRef="Keyword_cc_verification" />
             <Match idRef="Keyword_cc_name" />
             <Match idRef="Func_expiration_date" />
           </Any>
         </Pattern>
       </Entity>
   ```

Now that you have located the Credit Card Number rule definition in the XML, you can customize the rule's XML to meet your needs. (For a refresher on the XML definitions, see the [Term glossary](#term-glossary) at the end of this topic.)

## Modify the XML and create a new sensitive information type

First, you need to create a new sensitive information type because you can't directly modify the default rules. You can do a wide variety of things with custom sensitive information types, which are outlined in [Developing sensitive information rule packages](developing-sensitive-information-rule-packages-exchange-2013-help.md). For this example, we'll keep it simple and only remove corroborative evidence and add keywords to the Credit Card Number rule.

All XML rule definitions are built on the following general template. You need to copy and paste the Credit Card Number definition XML in the template, modify some values (notice the ". . ." placeholders in the following example), and then upload the modified XML as a new rule that can be used in policies.

```powershell
<?xml version="1.0" encoding="utf-8"?>
<RulePackage xmlns="http://schemas.microsoft.com/office/2011/mce">
  <RulePack id=". . .">
    <Version major="1" minor="0" build="0" revision="0" />
    <Publisher id=". . ." />
    <Details defaultLangCode=". . .">
      <LocalizedDetails langcode=" . . . ">
         <PublisherName>. . .</PublisherName>
         <Name>. . .</Name>
         <Description>. . .</Description>
      </LocalizedDetails>
    </Details>
  </RulePack>

 <Rules>
   <!-- Paste the Credit Card Number rule definition here.-->
      <LocalizedStrings>
         <Resource idRef=". . .">
           <Name default="true" langcode=" . . . ">. . .</Name>
           <Description default="true" langcode=". . ."> . . .</Description>
         </Resource>
      </LocalizedStrings>
   </Rules>
</RulePackage>
```

Now, you have something that looks similar to the following XML. Because rule packages and rules are identified by their unique GUIDs, you need to generate two GUIDs: one for the rule package and one to replace the GUID for the Credit Card Number rule. (The GUID for the entity ID in the following code sample is the one for our built-in rule definition, which you need to replace with a new one.) There are several ways to generate GUIDs, but you can do it easily in PowerShell by typing `[guid]::NewGuid()`.

```powershell
<?xml version="1.0" encoding="utf-8"?>
<RulePackage xmlns="http://schemas.microsoft.com/office/2011/mce">
  <RulePack id="8aac8390-e99f-4487-8d16-7f0cdee8defc">
    <Version major="1" minor="0" build="0" revision="0" />
    <Publisher id="8d34806e-cd65-4178-ba0e-5d7d712e5b66" />
    <Details defaultLangCode="en">
      <LocalizedDetails langcode="en">
        <PublisherName>Contoso Ltd.</PublisherName>
        <Name>Financial Information</Name>
        <Description>Modified versions of the Microsoft rule package</Description>
      </LocalizedDetails>
    </Details>
  </RulePack>

 <Rules>
    <Entity id="db80b3da-0056-436e-b0ca-1f4cf7080d1f"
       patternsProximity="300" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_credit_card" />
        <Any minMatches="1">
          <Match idRef="Keyword_cc_verification" />
          <Match idRef="Keyword_cc_name" />
          <Match idRef="Func_expiration_date" />
        </Any>
      </Pattern>
    </Entity>
      <LocalizedStrings>
         <Resource idRef="db80b3da-0056-436e-b0ca-1f4cf7080d1f">
<!-- This is the GUID for the preceding Credit Card Number entity because the following text is for that Entity. -->
           <Name default="true" langcode="en-us">Modified Credit Card Number</Name>
           <Description default="true" langcode="en-us">Credit Card Number that looks for additional keywords, and another version of Credit Card Number that doesn't require keywords (but has a lower confidence level)</Description>
         </Resource>
      </LocalizedStrings>
   </Rules>
</RulePackage>
```

## Remove the corroborative evidence requirement from a sensitive information type

Now that you have a new sensitive information type that you're able to upload to your Exchange environment, the next step is to make the rule more specific. Modify the rule so that it only looks for a 16-digit number that passes the checksum but doesn't require additional (corroborative) evidence (for example keywords). To do this, you need to remove the part of the XML that looks for corroborative evidence. Corroborative evidence is very helpful in reducing false positives because usually there are certain keywords or an expiration date near the credit card number. If you remove that evidence, you should also adjust how confident you are that you found a credit card number by lowering the **confidenceLevel**, which is 85 in the example.

```powershell
<Entity id="db80b3da-0056-436e-b0ca-1f4cf7080d1f" patternsProximity="300"
      <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_credit_card" />
      </Pattern>
    </Entity>
```

## Look for keywords that are specific to your organization

You might want to require corroborative evidence but want different or additional keywords, and perhaps you want to change where to look for that evidence. You can adjust the **patternsProximity** to expand or shrink the window for corroborative evidence around the 16-digit number. To add your own keywords, you need to define a keyword list and reference it within your rule. The following XML adds the keywords "company card" and "Contoso card" so that any message that contains those phrases within 150 characters of a credit card number will be identified as a credit card number.

```powershell
<Rules>
<! -- Modify the patternsProximity to be "150" rather than "300." -->
    <Entity id="db80b3da-0056-436e-b0ca-1f4cf7080d1f" patternsProximity="150" recommendedConfidence="85">
      <Pattern confidenceLevel="85">
        <IdMatch idRef="Func_credit_card" />
        <Any minMatches="1">
          <Match idRef="Keyword_cc_verification" />
          <Match idRef="Keyword_cc_name" />
<!-- Add the following XML, which references the keywords at the end of the XML sample. -->
          <Match idRef="My_Additional_Keywords" />
          <Match idRef="Func_expiration_date" />
        </Any>
      </Pattern>
    </Entity>
<!-- Add the following XML, and update the information inside the <Term> tags with the keywords that you want to detect. -->
    <Keyword id="My_Additional_Keywords">
      <Group matchStyle="word">
        <Term caseSensitive="false">company card</Term>
        <Term caseSensitive="false">Contoso card</Term>
      </Group>
    </Keyword>
```

## Upload your rule

To upload your rule, you need to do the following.

1. Save it as an .xml file with Unicode encoding. This is important because the rule won't work if the file is saved with a different encoding.

2. Connect to the [Exchange Management Shell](/powershell/exchange/exchange-management-shell).

3. Replace `\C:\custompath\` with a real file path and run the following command:

   ```powershell
   New-ClassificationRuleCollection -FileData (Get-Content -Path "C:\custompath\MyNewRulePack.xml " -Encoding Byte)
   ```

4. To confirm, type **Y**, and then press **Enter**.

5. Verify that your new rule was uploaded by typing Get-DataClassification, which now displays the name of your rule.

To start using the new rule to detect sensitive information, you need to add the rule to a DLP policy. To learn how to add the rule to a policy, see [Manage DLP policies](manage-dlp-policies-exchange-2013-help.md).

## Term glossary

These are the definitions for the terms you encountered during this procedure.

|**Term**|**Definition**|
|:-----|:-----|
|Entity|Entities are what we call sensitive information types, such as credit card numbers. Each entity has a unique GUID as its ID. If you copy a GUID and search for it in the XML, you'll find the XML rule definition and all the localized translations of that XML rule. You can also find this definition by locating the GUID for the translation and then searching for that GUID.|
|Functions|The XML file references **Func_credit_card**, which is a function in compiled code. Functions are used to run complex regexes and verify that checksums match for our built-in rules.) Because this happens in the code, some of the variables don't appear in the XML file.|
|IdMatch|This is the identifier that the pattern is to trying to match (for example, a credit card number). You can read more about this and about the **Match** tags in [Entity rules](developing-sensitive-information-rule-packages-exchange-2013-help.md#entity-rules).|
|Keyword lists|The XML file also references **keyword_cc_verification** and **keyword_cc_name**, which are lists of keywords from which we are looking for matches within the **patternsProximity** for the entity. These aren't currently displayed in the XML.|
|Pattern|The pattern contains the list of what the sensitive type is looking for. This includes keywords, regexes, and internal functions (that perform tasks like verifying checksums). Sensitive information types can have multiple patterns with unique confidences. This is useful when creating a sensitive information type that returns a high confidence if corroborative evidence is found and a lower confidence if little or no corroborative evidence is found.|
|Pattern confidenceLevel|This is the level of confidence that the DLP engine found a match. This level of confidence is associated with a match for the pattern if the pattern's requirements are met. This is the confidence measure you should consider when using Exchange transport rules (ETRs).|
|patternsProximity|When we find what looks like a credit card number pattern, **patternsProximity** is the proximity around that number where we'll look for corroborative evidence.|
|recommendedConfidence|This is the confidence level we recommend for this rule. The recommended confidence applies to entities and affinities. For entities, this number is never evaluated against the **confidenceLevel** for the pattern. It's merely a suggestion to help you choose a confidence level if you want to apply one. For affinities, the **confidenceLevel** of the pattern must be higher than the **recommendedConfidence** number for an ETR action to be invoked. The **recommendedConfidence** is the default confidence level used in ETRs that invokes an action. If you want, you can manually change the ETR to be invoked based off the pattern's confidence level, instead.|

## For more information

- [How DLP rules are applied to evaluate messages](dlp-rule-application-exchange-2013-help.md)

- [Create a custom DLP policy](create-custom-dlp-policy-exchange-2013-help.md)

- [Sensitive information types in Exchange Server](../ExchangeServer/policy-and-compliance/data-loss-prevention/sensitive-information-types.md)