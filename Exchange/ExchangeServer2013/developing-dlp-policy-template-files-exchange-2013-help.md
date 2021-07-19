---
title: 'Developing DLP policy template files: Exchange 2013 Help'
TOCTitle: Developing DLP policy template files
ms.author: dmaguire
author: msdmaguire
manager: serdars
ms.reviewer: 
ms.assetid: 4b263547-aef4-4ee8-aa4f-fa64a5863189
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Developing DLP policy template files in Exchange 2013

_**Applies to:** Exchange Server 2013_

This overview explains the components of an XML schema definition for data loss prevention (DLP) policy template files and also provides an XML-format example policy file. It will be helpful to understand the overall DLP architecture and rule-development process before you begin. For more information, see [Data loss prevention](data-loss-prevention-exchange-2013-help.md) and [Define your own DLP templates and information types](define-your-own-dlp-templates-and-information-types-exchange-2013-help.md).

In order to make data loss prevention solutions easy to adopt and manage, a conceptual model known as DLP policies and policy templates is introduced in Microsoft Exchange Server 2013. DLP policy templates provide a preliminary design for your intended DLP policy. In order to be valuable, a DLP policy template must encapsulate all the directives and data objects that are required to meet a specific policy objective, such as a regulation or business need. The template is not environment-specific. It is simply a definition or model of a policy that can be provided as part of the product configuration or supplied by independent software vendors and partners. DLP policies on the other hand, are run-time instantiations of the templates that are specific to the deployment environment. Your existing messaging policy framework can incorporate DLP policies through the use of transport rules. Transport rules provide great flexibility in adapting and expressing the richness of your DLP solutions.

## Policy template sources and structure

DLP policy templates are typically influenced from multiple sources such as server-based processing directives, client computer policies, or other policy constructs as shown in the following image:

![Factors that influence policy templates](images/ITPro_MRM_DlpPolicy_TempInputs.gif)

Simple management operations are available for DLP policy templates though both the Exchange Management Shell and Internet-based interfaces, such as the Exchange admin center, which include Import, Export, Deletion and Query capabilities. A DLP policy is created by referencing a DLP policy template as part of the creation process. These referenced DLP policy templates may be references to ones installed in the system, which are stored in active directory domain services, or be provided as input directly from externally supplied policies.

DLP policy templates are represented as XML documents. A single XML schema is used for policies provided within Exchange and externally also. The conceptual structure of the XML document is represented in the table below, which shows the major elements. The set of policy component definitions help you achieve a specific policy objective such as a regulation or business need.

|**Structural Element**|**Meaning or Example**|
|:-----|:-----|
|Publisher|Microsoft or Partner|
|Version|15.0.1.0|
|Policy Name|PCI-DSS|
|Description|The PCI-DSS DLP policy helps detect the presence of information subject to PCI Data Security Standard ‎(PCI DSS)‎, including information like credit card or debit card numbers. Use of this policy does not ensure compliance with any regulation. After your testing is complete, make the necessary configuration changes in Exchange so the transmission of information complies with your organization's policies. Examples include configuring TLS with known business partners or adding more restrictive transport rule actions, such as adding rights protection to messages that contain this type of data.|
|Metadata|Tags to describe the local regulation, country or region, keywords and more.|
|Set of policy constructs| Transport rule definitions, such as conditions and actions.  <br/>  Email client behavior definitions that control client experience through interactive notifications.  <br/>  Optionally, configuration references that need to be coordinated with client environment-specific settings.|
|Set of data classifications| Specifies classification entities or affinities.  <br/>  Entities have count and confidence; affinities only have a confidence.  <br/>  Comes with its own set of properties and classification schema.|

## Policy template format definition

DLP Policy templates are expressed as XML documents which adhere to the following schema. Note that the XML is case-sensitive. For instance, `dlpPolicyTemplates` will work, but `DlpPolicyTemplates` won't work.

```powershell
<?xml version="1.0" encoding="UTF-8"?>
<dlpPolicyTemplates>
  <dlpPolicyTemplate id="F7C29AEC-A52D-4502-9670-141424A83FAB" mode="Audit" state="Enabled" version="15.0.2.0">
    <contentVersion>4</contentVersion>
    <publisherName>Microsoft</publisherName>
    <name>
      <localizedString lang="en">PCI-DSS</localizedString>
    </name>
    <description>
      <localizedString lang="en">Detects the presence of information subject to Payment Card Industry Data Security Standard (PCI-DSS) compliance requirements.</localizedString>
    </description>
    <keywords></keywords>
    <ruleParameters></ruleParameters>
    <ruleParameters/>
    <policyCommands>
      <!-- The contents below are applied/executed as rules directly in PS - -->
      <commandBlock>
        <![CDATA[ new-transportRule "PCI-DSS: Monitor Payment Card Information Sent To Outside" -DlpPolicy "%%DlpPolicyName%%" -SentToScope NotInOrganization -SetAuditSeverity High -MessageContainsDataClassifications @{Name="Credit Card Number"; MinCount="1" } -Comments "Monitors payment card information sent to outside the organization as part of the PCI-DSS DLP Policy."]]>
      </commandBlock>
      <commandBlock>
        <![CDATA[ new-transportRule "PCI-DSS: Monitor Payment Card Information Sent To Within" -DlpPolicy "%%DlpPolicyName%%" -Comments "Monitors payment card information sent inside the organization as part of the PCI-DSS DLP Policy." -SentToScope InOrganization -SetAuditSeverity Low -MessageContainsDataClassifications @{Name="Credit Card Number"; MinCount="1" } ]]>
      </commandBlock>
    </policyCommands>
    <policyCommandsResources></policyCommandsResources>
  </dlpPolicyTemplate>
</dlpPolicyTemplates>
```

If a parameter you include in your XML file for any element includes a space, the parameter has to be surrounded by double quotes or it will not work properly. In the example below, the parameter that follows `-SentToScope` is acceptable and does not include double quotes because it is one continuous string of characters without a space. However, the parameter provided for - `Comments` will not appear in the Exchange admin center because there are no double quotes and it includes spaces.

```powershell
<CommandBlock><![CDATA[ new-transportRule "PCI-DSS: Monitor Payment Card Information Sent To Within" -DlpPolicy "PCI-DSS" -Comments Monitors payment card information sent inside the organization -SentToScope InOrganization -SetAuditSeverity Low -MessageContainsDataClassifications @{Name="Credit Card Number"; MinCount="1" } ]]> </CommandBlock>
```

### localizedString Element

The template format offers the capability to localize strings in the template which may be presented to the end-user, for example as part of selecting which DLP policy templates are installed. The localizedString element can be used to supply multiple definitions for the Name and Description fields.

### ruleParameters Node

This is an optional set of parameters that need to be supplied during the template instantiation phase when creating a DLP policy to map to deployment specific objects. For example an actual distribution group that is available in the deployment.

### dlpPolicyTemplate Element

This is the root element for the DLP policy template and is required for every template. Available attributes are shown in the following table:

|**Attribute Name**|**Required?**|**Description**|
|:-----|:-----|:-----|
|Version|Yes|The version number used in this DLP policy template document.|
|State|No|Optional default configuration for the state of the policy.|
|Mode|No|Optional default configuration for the mode of the policy.|
|Id|No|A GUID to uniquely identify this DLP policy template definition in the following format:"A29C69BF-4F98-47F1-9A99-5771DFD2C27F".|

Child elements include the following sequence of elements.

|**Child Element**|**(minimum, maximum)**|**Description**|
|:-----|:-----|:-----|
|PublisherName|(1, 1)|Meta data for the template's publisher|
|Name|(1, 1)|Localizable name for this template.|
|Description|(1, 1)|Localizable description for this template.|
|Keywords|(1, 1)|List of keywords that applies to this template. A template may have an empty list of keywords.|
|RuleParameters|(0, 1)|List of template parameters that are used in the policy definition.|
|PolicyCommands|(0, 1)|List of Transport rule definitions for this policy. This is an optional element.|

### DLP Policy Template: PolicyCommands

This part of the policy template contains the list of the Exchange Management Shell commands that are used to instantiate the policy definition. The import process will execute each of the commands as part of the instantiation process. Sample policy commands are provided here.

```powershell
<PolicyCommands>
    <!-- The contents below are applied/executed as rules directly in PS - -->
      <CommandBlock> <![CDATA[ new-transportRule "PCI-DSS: Monitor Payment Card Information Sent To Outside" -DlpPolicy "PCI-DSS" -SentToScope NotInOrganization -SetAuditSeverity High -MessageContainsDataClassifications @{Name="Credit Card Number"; MinCount="1" } -Comments "Monitors payment card information sent to outside the organization as part of the PCI-DSS DLP policy."]]></CommandBlock>
      <CommandBlock><![CDATA[ new-transportRule "PCI-DSS: Monitor Payment Card Information Sent To Within" -DlpPolicy "PCI-DSS" -Comments "Monitors payment card information sent inside the organization as part of the PCI-DSS DLP policy." -SentToScope InOrganization -SetAuditSeverity Low -MessageContainsDataClassifications @{Name="Credit Card Number"; MinCount="1" } ]]> </CommandBlock>
  </PolicyCommands>

```

The format of the cmdlets is the standard Exchange Management Shell cmdlet syntax for the cmdlets used. The commands are executed in sequence. It is possible for each of the command nodes to contain a script block which would be composed of multiple commands. Below is example that illustrates how to embed classification rule pack inside of a dlp policy template, and installing the rule pack as part of the policy creation process. The classification rule pack is embedded in the policy template, and then passed as a parameter to the cmdlet in the template:

```powershell
<CommandBlock>
  <![CDATA[
$rulePack = [system.Text.Encoding]::Unicode.GetBytes('<?xml version="1.0" encoding="utf-8"?>
<rulePackage xmlns="http://schemas.microsoft.com/office/2011/mce">
  <RulePack id="c3f021a3-c265-4dc2-b3a7-41a1800bf518">
    <Version major="1" minor="0" build="0" revision="0"/>
    <Publisher id="e17451d3-9648-4117-a0b1-493a6d5c73ad"/>
    <Details defaultLangCode="en-us">
      <LocalizedDetails langcode="en-us">
        <PublisherName>Contoso</PublisherName>
        <Name>Contoso Sample Rule Pack</Name>
        <Description>This is a sample rule package</Description>
      </LocalizedDetails>
    </Details>
  </RulePack>
  <Rules>
    <Entity id="7cc35258-6b35-4415-baff-a76d1a018980" patternsProximity="300" recommendedConfidence="85" workload="Exchange">
      <Pattern confidenceLevel="85">
        <IdMatch idRef="Regex_Contoso" />
        <Any minMatches="1">
          <Match idRef="Regex_conf" />
        </Any>
      </Pattern>
    </Entity>
    <Regex id="Regex_Contoso">(?i)(\bContoso\b)</Regex>
    <Regex id="Regex_conf">(?i)(\bConfidential\b)</Regex>
    <LocalizedStrings>
      <Resource idRef="7cc35258-6b35-4415-baff-a76d1a018980">
        <Name default="true" langcode="en-us">
          Confidential Information Rule
        </Name>
        <Description default="true" langcode="en-us">
          Sample rule pack - Detects Contoso confidential information
        </Description>
      </Resource>
    </LocalizedStrings>
  </Rules>
</RulePackage>
')
New-ClassificationRuleCollection -FileData $rulePack
New-TransportRule -name "customEntity" -DlpPolicy "%%DlpPolicyName%%" -SentToScope NotInOrganization -MessageContainsDataClassifications @{Name="Confidential Information Rule"} -SetAuditSeverity High]]>
</CommandBlock>

```

Child elements include the following ordered sequence of elements.

|**Child Element**|**(Minimum, Maximum)**|**Description**|
|:-----|:-----|:-----|
|CommandBlock|(1, n)|A command block that is executed in the PowerShell. The command blocks are each executed in sequence.|

## For more information

[Data loss prevention](data-loss-prevention-exchange-2013-help.md)

[Define your own DLP templates and information types](define-your-own-dlp-templates-and-information-types-exchange-2013-help.md)

[Import a custom DLP policy template from a file](import-a-custom-dlp-policy-template-from-a-file-exchange-2013-help.md)
