# atemsis
All-Seeing

Notes to myself
==
In `D:\Program Files\Epic Games\MagicTheGathering\MTGA_Data\Downloads\Data` there are two files of interest:
* data_cards_[UUID].mtga
* data_loc_[UUID].mtga

Where UUID is what looks like a UUID hex value.

data_cards
--
This has an array of the following objects:
```json
[
  {
    "grpid": 6873,
    "titleId": 2929,
    "artId": 1837,
    "isToken": false,
    "isPrimaryCard": true,
    "artSize": 0,
    "power": "8",
    "toughness": "4",
    "flavorId": 431065,
    "CollectorNumber": "210",
    "altDeckLimit": null,
    "cmc": 8,
    "rarity": 2,
    "artistCredit": "Steve White",
    "set": "MIR",
    "linkedFaceType": 0,
    "types": [
      2
    ],
    "subtypes": [
      55
    ],
    "supertypes": [],
    "cardTypeTextId": 8,
    "subtypeTextId": 738,
    "colors": [
      5
    ],
    "frameColors": [
      5
    ],
    "frameDetails": [],
    "colorIdentity": [
      5
    ],
    "abilities": [
      {
        "abilityId": 14,
        "textId": 28
      }
    ],
    "hiddenAbilities": [],
    "linkedFaces": [],
    "castingcost": "o6oGoG",
    "linkedTokens": [],
    "knownSupportedStyles": [],
    "DigitalReleaseSet": ""
  },
```
data_loc
--
This has an array of the following objects:
```json
[
  {
    "langkey": "null",
    "isoCode": "en-US",
    "keys": [
      {
        "id": 1,
        "text": ""
      },
      {
        "id": 2,
        "text": "Basic"
      },
      {
        "id": 3,
        "text": "Legendary"
      },
```
Correlation between data_cards and data_loc
--
My interpretation is that data_cards has all of the information about cards and data_loc has all of the localized strings in the application.  To get to a specific string for a card you need to know the `isoCode` and the `id`.  There are 9 `isoCode`s:
* en-US
* fr-FR
* it-IT
* de-DE
* es-ES
* ja-JP
* pt-BR
* ru-RU
* ko-KR

It looks like it's a combination of "Administrative language alpha-2" and "Alpha-2 code".  Need to research into this further.

To get utility of this you would match the various `*id` fields from data_cards to the strings (based on `isoCode`) to get the localized card text.

data_prompt
--
The file looks like this:
```json
[
  {
    "id": 0,
    "text": 1
  },
  {
    "id": 1,
    "text": 164
  },
  {
    "id": 2,
    "text": 165
  },
```
To me this is a join table which matches an sequential `id` to it's text string.

data_enums
--
Ahh some fun stuff. This is what I assume to be a lot of string enumerations used in the application:
* CardType
* Color
* CounterType
* FailureReason
* MatchState
* Phase
* ResultCode
* Step
* SubType
* SuperType

data_altPrintings
--
I assume this means "alternate printings".  Looks like it's got card `id`s with values like "SH" and "TOHO". "TOHO" stands for Toho Co., Ltd which owns the right to Godzilla so it sounds like these are alternate arts for the Ikoria cards. Likewise any card with "SH" seems to have a showcase art alternate treatment. I'm not sure if these are special alternate art outside of the parallax treatment that every card seems to get.

For some reason, Planeswalker parallax art seems to have a value of `" "`.

Lastly, Thalia, Guardian of Thraben has four codes due to her Secret Lair art.

data_altFlavorTexts
--
TBD

data_altArtCredits
--
TBD

data_abilities
--
TBD

Extracting useful information
--
To get the most interesting data from this I would pull the following:
* Name (`titleId`)
* Mana Cost (`castingcost`)
* Type Line (`types`)
  * Supertype (`subtypes`)
  * Subtype (`subtypes`)
* Expansion Symbol (`set`)
* Text Box
  * Abilities (`abilities`)
  * Flavor Text (`flavorId`)
* Artist Info (`artistCredit`)
* Collector's Number (`CollectorNumber`)
* Power/Toughness (`power`/`toughness`) note: toughness is Planeswalker loyalty
* Card Border (`frameColors`)