---
title: XML Normalization
author: Xiphoseer
---

It seems like a lot of people despise XML and consider it obsolete. But
I would argue that is not the case and _beautiful_ XML is very much possible
if we apply the learnings from [Database normalization].

Recall the following definitions:

- A database is in **first normal form** (1NF), if and only if each attribute contains a single value.
- A database is in **second normal form** (2NF), if and only if each attribute that is not part of the
  primary key depends on all parts of the primary key.
- A database is in **third normal form** (3NF), if and only if all all non-prime attributes depend only on
  candidate keys and do not have a transitive dependency on another key.

You can think of an XML Element as a row in a relational schema. The attributes on
the element are the attributes of the relation i.e. the columns of the table. Nesting
of elements implies multiple relations, where the inner element is a relation with
a foreign key to the parent element.

### Example 1

Consider the following example from the game LEGO&reg; Universe:

```xml
<behavior id="5678" templateID="2" effectID="1439">
    <parameter key="action">5679</parameter>
    <parameter key="angle">90</parameter>
    <parameter key="angle_weight">0</parameter>
    <parameter key="blocked action">5685</parameter>
    <parameter key="check_env">1</parameter>
    <parameter key="clear_provided_target">0</parameter>
    <parameter key="distance_weight">0</parameter>
    <parameter key="height">2.2</parameter>
    <parameter key="lower_bound">0.5</parameter>
    <parameter key="max range">9</parameter>
    <parameter key="max targets">1</parameter>
    <parameter key="method">1</parameter>
    <parameter key="min range">0</parameter>
    <parameter key="target_enemy">1</parameter>
    <parameter key="target_friend">0</parameter>
    <parameter key="target_self">0</parameter>
    <parameter key="upper_bound">5</parameter>
    <parameter key="use_attack_priority">1</parameter>
    <parameter key="use_picked_target">0</parameter>
</behavior>
```

In the original database there are two relations for this:

- One table [`BehaviorTemplate`] with primary key `behaviorID` and non-prime attributes
  `templateID` (foreign key to another table), `effectID`, and `effectHandle`.
- One table [`BehaviorParameter`] with a composite primary key consisting of
  `behaviorID` and `parameterID` and a single non-prime attribute `value`.

I'm being intentionally mechanistic here to achive the result that one
element in the XML maps to one row in the database. It would be possible
to define the parameters like

```xml
<upper_bound>5</upper_bound>
```

but that would: 

- introduce structural ambiguity, i.e. when parsing, it's difficult to
  detect which child elements are parameters
- theoretically mapping to a two-column relation `BehaviorUpperBound`
- limit flexibility provided by the current schema to add new parameters
- require some sort of escaping for some of the parameter names

One first rule we may derive is

> For a relation R, if there is exactly one non-prime attribute in
> R, that attribute SHOULD be used as the text content of the XML element.

Another is

> For a relation R, all parts of the primary key that are not implied
> by parent elements MUST be encoded as an XML attribute.

### Example 2

Consider the following XML:

```xml
<mission id="1855" isMission="true" definedType="Location" definedSubtype="Avant Gardens" uiSortOrder="240" gateVersion="nexustower">
    <name xml:lang="en-US">Monumental Repairs</name>
    <offerObjectID>6011</offerObjectID>
    <targetObjectID>6011</targetObjectID>
    <legoScore>20</legoScore>
    <rewardCurrency>250</rewardCurrency>
    <prereqMissionID>1854</prereqMissionID>
    <missionText gateVersion="freetrial">
        <cinematicAcceptedLeadin>0.1</cinematicAcceptedLeadin>
        <turnInIconId>3511</turnInIconId>
        <acceptChatBubble xml:lang="en-US">Need to get this place back on track!</acceptChatBubble>
        <chat state="1" xml:lang="en-US">Blast this Maelstrom, why's it have ta be so chaotic?</chat>
        <chat state="2" xml:lang="en-US">Check alla the paths up the Monument - it gets tricky up there!</chat>
        <chat state="3" xml:lang="en-US">Looks good from here, I'd say.</chat>
        <completionSucceedTip xml:lang="en-US">Return to Rusty Steele at the base of the Monument.</completionSucceedTip>
        <inProgress xml:lang="en-US"><![CDATA[<font color='#FF7F00'>Quick Build 1 button, 1 bouncer, and 1 moving platform</font> on the Monument.]]></inProgress>
        <offer xml:lang="en-US"><![CDATA[There’s some assembling that needs to be done! <font color='#FF7F00'>Quick Build 1 button, 1 bouncer, and 1 moving platform</font> up there!]]></offer>
        <readyToComplete xml:lang="en-US">You’re pretty handy with them bricks, pal! The Assembly Faction would be lucky to have you!</readytoComplete>
    </missionText>
    <missionTask uid="2626" taskType="2" gateVersion="freetrial">
        <description xml:lang="en-US">Quick Build 1 button on the Monument.</description>
        <iconId>4407</iconId>
        <largeTaskIconId>4407</largeTaskIconId>
        <target>123</target>
        <targetValue>1</targetValue>
    </missionTask>
    <missionTask uid="2627" taskType="2" gateVersion="freetrial">
        <description xml:lang="en-US">Quick Build 1 bouncer on the Monument.</description>
        <iconId>4406</iconId>
        <largeTaskIconId>4406</largeTaskIconId>
        <target>124</target>
        <targetValue>1</targetValue>
    </missionTask>
    <missionTask uid="2628" taskType="2" gateVersion="freetrial">
        <description xml:lang="en-US">Quick Build 1 moving platform on the Monument.</description>
        <iconId>4408</iconId>
        <largeTaskIconId>4408</largeTaskIconId>
        <target>125</target>
        <targetValue>1</targetValue>
    </missionTask>
</mission>
```

In this case, I intentionally deviated from the rule that one row in the database
is one element. That is because of the [billion dollar mistake]: If we disallow
NULL values, then 1NF implies that optional fields are table-valued (there is
a relation or there isn't, not two different values of an attribute). Thus, we
represent only the columns that are filled with data, but we do so as child elements.

This has the benefit of collapsing the entire `rewardX` columns into a single
`rewardCurrency` element in the case of this particular mission.

This isn't really practical if the data storage is a set of CSV files, but once
you start talking about columnar data storage, the distinction quickly disappears.
Thus:

> If any column is optional, it MUST be represented as a child element.

Another recommendation would be that

> If a column serves as a discriminator for subtyping, it MUST be represented as an attribute

In the example above, this applies to the `taskType` attribute. Task Type is an
enumeration that indicates the meaning of the `target` properties. It is generally
easier to implement deserialization of these formats if the type tag precedes the
content and is not embedded somewhere after its value would be needed for the first
time.

The next recommendation relates to the use of `xml:lang`:

> The standard `xml:lang` attribute should be used whenever there is text content
> int the element and its children and it is guaranteed to be text in one language.
> In that case, the most precise language tag should be used.

In the case above, it's always `xml:lang="en-US"` but depending on context a simple
language subtag such as `en` may also be appropriate.

Finally, I want to point out that there is HTML embedded in some of the `missionText`s.
In both cases I used a CDATA section to preserve the original content without
introducing additional tags to the outer document. On that note:

> CDATA sections MAY be used to represent character data within elements but they MUST
> be processed as if their content was represented as regular character data.

### Example 3

I want to take a closer look at a more complex rewards case:

```xml
<mission id="477">
    <rewardCurrency>250</rewardCurrency>
    <rewardItem count="3">2198</rewardItem>
    <rewardItem>14592</rewardItem>
    ...
</mission>
```

For this mission there are two reward items: 3x "Notion Potion" and 1x "Maelstrom Vacuum".
In the original schema, the reward items are 8 columns, 4 for the Item IDs and 4 for the
associated counts. This violates 3NF in a way because each count only refers to a single
item ID. Thus, we can think of them as a separate relation, limited to 4 rows per mission
with a column `count`. This column can be an attribute because, while optional, it has
a meaningful default value (1 item). Thus:

> Required columns that have a default value MUST be represented as attributes and MAY be
> omitted when serializing a row with the column set to the default value.

Another thing to note: XML is an ordered semi-structured representation. The reward items
thus have a defined order in this representation. In the database relation, there should
be an attribute `index` as part of the primary key that ensures that the rows are ordered
and addressable without a separate unique ID.

[Database normalization]: https://en.wikipedia.org/wiki/Database_normalization
[`BehaviorTemplate`]: https://docs.lu-dev.net/en/latest/database/BehaviorTemplate.html
[`BehaviorParameter`]: https://docs.lu-dev.net/en/latest/database/BehaviorParameter.html
[billion dollar mistake]: https://en.wikipedia.org/wiki/Tony_Hoare#Research_and_career