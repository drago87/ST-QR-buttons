/ife (timePeriod != '') {:
	/messages names=off 1|
	/re-replace find="/--TimePeriod--/g" replace="{{getvar::timePeriod}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (seasons != '') {:
	/messages names=off 1|
	/re-replace find="/--Season--/g" replace="{{getvar::seasons}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif ( seasons == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Season--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (worldDetails != '') {:
	/messages names=off 1|
	/re-replace find="/--WorldDetails--/g" replace="{{getvar::worldDetails}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedExtraCharacters != '') and (parsedExtraCharacters != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--ExtraCharacters--/g" replace="{{newline}}{{getvar::parsedExtraCharacters}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedExtraCharacters == 'None') {:
	/messages names=off 1|
	/re-replace find="/--ExtraCharacters--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((lore != '') and ( lore != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Lore--/g" replace="## LORE{{newline}}{{getvar::lore}}{{newline}}{{newline}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif ( lore == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Lore--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (scenarioOverview != '') {:
	/messages names=off 0|
	/re-replace find="/--ScenarioOverview--/g" replace="{{getvar::scenarioOverview}}" {{pipe}}|
	/message-edit message=0 await=true {{pipe}}|
:}|
/ife (characterOverview != '') {:
	/messages names=off 1|
	/re-replace find="/--CharacterOverview--/g" replace="{{getvar::characterOverview}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedMedia != '') and (parsedMedia != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Real--/g" replace="**Media Origin**: {{getvar::parsedMedia}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedMedia != 'None') {:
	/messages names=off 1|
	/re-replace find="/--Real--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (lastName != '') and (lastName != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--LastName--/g" replace="{{getvar::lastName}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (lastName == 'None') {:
	/messages names=off 1|
	/re-replace find="/--LastName--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (alias != '') and (alias != 'None')){:
	/messages names=off 1|
	/re-replace find="/--Alias1--/g" replace=", Alias" {{pipe}}|
	/re-replace find="/--Alias--/g" replace=", {{getvar::alias}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (alias == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Alias1--/g" replace="" {{pipe}}|
	/re-replace find="/--Alias--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (parsedSpecies != '') {:
	/messages names=off 1|
	/re-replace find="/--Species--/g" replace="{{getvar::parsedSpecies}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedOrigin != '') and (parsedOrigin != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Origin--/g" replace="{{newline}}- Origin: {{getvar::parsedOrigin}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif  ((parsedOrigin == 'None') or ((nationality == 'None') and (ethnicity == 'None'))){:
	/messages names=off 1|
	/re-replace find="/--Origin--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (gender != '') {:
	/messages names=off 1|
	/re-replace find="/--Gender--/g" replace="{{getvar::gender}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((futanari != '') and (futanari == 'Yes')) {:
	/messages names=off 1|
	/re-replace find="/--Futanari--/g" replace=" Futanari" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif  (futanari == 'No'){:
	/messages names=off 1|
	/re-replace find="/--Futanari--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((length != '') and (length != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Length--/g" replace="- Lenght: {{getvar::length}}{{newline}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (length == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Length--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((height != '') and (height != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Height--/g" replace="- Height: {{getvar::height}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (height == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Height--/g" replace="{{getvar::height}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (age != '') {:
	/messages names=off 1|
	/re-replace find="/--Age--/g" replace="{{getvar::parcedAge}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (appearanceHair != '') {:
	/messages names=off 1|
	/re-replace find="/--Hair--/g" replace="{{getvar::appearanceHair}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (appearanceEyes != '') {:
	/messages names=off 1|
	/re-replace find="/--Eyes--/g" replace="{{getvar::appearanceEyes}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (appearanceFace != '') {:
	/messages names=off 1|
	/re-replace find="/--Face--/g" replace="{{getvar::appearanceFace}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedCovering != '') and (parsedCovering != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Covering--/g" replace="{{getvar::parsedCovering}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedCovering == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Covering--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (appearanceBody != '') {:
	/messages names=off 1|
	/re-replace find="/--Body--/g" replace="{{newline}} - Body: {{getvar::appearanceBody}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((tanlines != '') and (tanlines != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Tanlines--/g" replace="{{newline}} - Tanlines: {{getvar::tanlines}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (tanlines == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Tanlines--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (appearanceBreasts != '') and (appearanceBreasts != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Breasts--/g" replace="{{newline}} - Breast: {{getvar::appearanceBreasts}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (appearanceBreasts == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Breasts--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((appearanceNipples != '') and (appearanceNipples != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Nipples--/g" replace="{{newline}} - Nipple: {{getvar::appearanceNipples}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (appearanceNipples == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Nipples--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((appearanceGenitals != '') and (appearanceGenitals != 'None') and (futanari == 'yes')) {:
	/messages names=off 1|
	/re-replace find="/--Genitals--/g" replace="{{newline}} - Genitals: {{getvar::appearanceGenitals}}" {{pipe}}|
	/re-replace find="/--Pussy--/g" replace="" {{pipe}}|
	/re-replace find="/--Cock--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif ((appearancePussy != '') and (appearancePussy != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Pussy--/g" replace="{{newline}} - Pussy: {{getvar::appearancePussy}}" {{pipe}}|
	/re-replace find="/--Cock--/g" replace="" {{pipe}}|
	/re-replace find="/--Genitals--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif ((appearanceCock != '') and (appearanceCock != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Cock--/g" replace="{{newline}} - Cock: {{getvar::appearanceCock}}" {{pipe}}|
	/re-replace find="/--Pussy--/g" replace="" {{pipe}}|
	/re-replace find="/--Genitals--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (appearanceAnus != '') {:
	/messages names=off 1|
	/re-replace find="/--Anus--/g" replace="{{getvar::appearanceAnus}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedAppearanceFeatures != '') and (parsedAppearanceFeatures != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Features--/g" replace="{{newline}}{{newline}}{{getvar::parsedAppearanceFeatures}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedAppearanceFeatures == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Features--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedAppearanceTraits != '') and (parsedAppearanceTraits != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--ApperanceTraits--/g" replace="{{newline}}{{newline}}### APPEARANCE TRAITS{{newline}}{{getvar::parsedAppearanceTraits}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedAppearanceTraits == 'None') {:
	/messages names=off 1|
	/re-replace find="/--ApperanceTraits--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (makeLoreBook != 'Yes') {:
	/ife (outfitHeadDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitHead--/g" replace="{{getvar::outfitHeadDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((parsedAccessories != '') and (parsedAccessories == 'None')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitAccessories--/g" replace="{{getvar::parsedAccessories}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif ((parsedAccessories != '') and (parsedAccessories != 'None')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitAccessories--/g" replace="{{newline}}{{getvar::parsedAccessories}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (parsedMakeup != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitMakeup--/g" replace="{{getvar::parsedMakeup}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitNeckDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitNeck--/g" replace="{{getvar::outfitNeckDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitMainwearDescription != '') and (outfitMainwearDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitMainwear--/g" replace="- Mainwear: {{getvar::outfitMainwearDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitMainwearDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitMainwear--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitTopDescription != '') and (outfitTopDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitTop--/g" replace="- Top: {{getvar::outfitTopDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitTopDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitTop--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitBottomDescription != '') and (outfitBottomDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitBottom--/g" replace="{{newline}}- Bottom: {{getvar::outfitBottomDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitBottomDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitBottom--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitLegwearDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitLegwear--/g" replace="{{getvar::outfitLegwearDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitSocksDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitSocks--/g" replace="{{getvar::outfitSocksDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitShoesDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitShoes--/g" replace="{{getvar::outfitShoesDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ( (outfitUnderwearTopDescription != '') and (outfitUnderwearTopDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearTop--/g" replace="{{newline}}- Underwear (Top):  {{getvar::outfitUnderwearTopDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitUnderwearTopDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearTop--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitUnderwearBottomDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearBottom--/g" replace="{{getvar::outfitUnderwearBottomDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
:}|
/else {:
	 /ife (outfitHeadDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitHead--/g" replace="{\{outlet::outfitHeadDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((parsedAccessories != '') and (parsedAccessories == 'None')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitAccessories--/g" replace="{{getvar::parsedAccessories}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif ((parsedAccessories != '') and (parsedAccessories != 'None')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitAccessories--/g" replace="{{newline}}{{getvar::parsedAccessories}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (parsedMakeup != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitMakeup--/g" replace="{{getvar::parsedMakeup}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitNeckDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitNeck--/g" replace="{{getvar::outfitNeckDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitMainwearDescription != '') and (outfitMainwearDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitMainwear--/g" replace="- Mainwear: {{getvar::outfitMainwearDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitMainwearDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitMainwear--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitTopDescription != '') and (outfitTopDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitTop--/g" replace="- Top: {{getvar::outfitTopDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitTopDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitTop--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ((outfitBottomDescription != '') and (outfitBottomDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitBottom--/g" replace="{{newline}}- Bottom: {{getvar::outfitBottomDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitBottomDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitBottom--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitLegwearDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitLegwear--/g" replace="{{getvar::outfitLegwearDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitSocksDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitSocks--/g" replace="{{getvar::outfitSocksDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitShoesDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitShoes--/g" replace="{{getvar::outfitShoesDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife ( (outfitUnderwearTopDescription != '') and (outfitUnderwearTopDescription != 'Skip')) {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearTop--/g" replace="{{newline}}- Underwear (Top):  {{getvar::outfitUnderwearTopDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/elseif (outfitUnderwearTopDescription == 'Skip') {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearTop--/g" replace="" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
	/ife (outfitUnderwearBottomDescription != '') {:
		/messages names=off 1|
		/re-replace find="/--OutfitUnderwearBottom--/g" replace="{{getvar::outfitUnderwearBottomDescription}}" {{pipe}}|
		/message-edit message=1 await=true {{pipe}}|
	:}|
:}|
/ife (appearanceQA != '') {:
	/messages names=off 1|
	/re-replace find="/--AppearanceQA--/g" replace="{{newline}}{{newline}}</Q&A>{{newline}}{{getvar::appearanceQA}}{{newline}}</Q&A>" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (backstory != '') {:
	/messages names=off 1|
	/re-replace find="/--Backstory--/g" replace="{{getvar::backstory}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (residence != '') {:
	/messages names=off 1|
	/re-replace find="/--Residence--/g" replace="{{getvar::residence}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedOccupation != '') and (parsedOccupation != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Occupation--/g" replace="{{newline}}{{newline}}### OCCUPATION{{newline}}{{getvar::parsedOccupation}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedOccupation == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Occupation--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (connections != '') {:
	/messages names=off 1|
	/re-replace find="/--Connections--/g" replace="{{getvar::connections}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSecret != '') and (parsedSecret != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Secret--/g" replace="{{newline}}{{newline}}{{getvar::parsedSecret}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSecret == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Secret--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedItems != '') and (parsedItems != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Items--/g" replace="{{newline}}{{newline}}### INVENTORY{{newline}}{{getvar::parsedItems}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedItems == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Items--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (parsedAbilities != '') and (parsedAbilities != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Abilities--/g" replace="{{newline}}{{newline}}### ABILITIES{{newline}}{{getvar::parsedAbilities}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedAbilities == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Abilities--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (parsedArchetype != '') {:
	/messages names=off 1|
	/re-replace find="/--Archetype--/g" replace="{{getvar::parsedArchetype}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (parsedAlignment != '') and  (parsedAlignment != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Alignment--/g" replace="{{newline}}{{newline}}{{getvar::parsedAlignment}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedAlignment == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--Alignment--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (personalityTags != '') {:
	/messages names=off 1|
	/re-replace find="/--PersonalityTags--/g" replace="- Personality Tags: {{getvar::personalityTags}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( personalityIntelligenceLevel != '') {:
	/messages names=off 1|
	/re-replace find="/--IntelligenceLevel--/g" replace="- Intelligence Level: {{getvar::personalityIntelligenceLevel}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (personalitycognitiveAbilities != '') and (personalitycognitiveAbilities != 'None') ) {:
	/messages names=off 1|
	/re-replace find="/--CognitiveAbilities--/g" replace="{{newline}}{{newline}}- Cognitive Abilities: {{getvar::personalitycognitiveAbilities}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (personalitycognitiveAbilities == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--CognitiveAbilities--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSentientLevel != '') and (parsedSentientLevel != 'None')) {:
/messages names=off 1|
	/re-replace find="/--SentientLevel--/g" replace="{{newline}}### ANIMALISTIC COGNITION{{newline}}{{getvar::parsedSentientLevel}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSentientLevel == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--SentientLevel--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( personalitySocialBehavior != '') {:
	/messages names=off 1|
	/re-replace find="/--SocialBehavior--/g" replace="- Social Behavior: {{getvar::personalitySocialBehavior}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (personalitySocialSkills != '') and (personalitySocialSkills != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--SocialSkills--/g" replace="{{newline}}{{newline}}- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (personalitySocialSkills == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--SocialSkills--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (parsedAspiration != '') {:
	/messages names=off 1|
	/re-replace find="/--MainAspiration--/g" replace="{{getvar::mainAspiration}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedTraits != '') and (parsedTraits != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--UniqueTraits--/g" replace="{{getvar::parsedTraits}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedTraits == 'None') {:
	/messages names=off 1|
	/re-replace find="/--UniqueTraits--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (personalityQA != '') {:
	/messages names=off 1|
	/re-replace find="/--PersonalityQA--/g" replace="{{newline}}{{newline}}<Q&A>{{newline}}{{getvar::personalityQA}}{{newline}}</Q&A>" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedBehaviorNotes != '') and (parsedBehaviorNotes != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--BehaviorNotes--/g" replace="{{newline}}{{newline}}- - -

## [BEHAVIOR_NOTES]
[IMPORTANT NOTE FOR AI: This section governs how --FirstName-- behaves moment to moment. In all interactions—especially intimate or emotionally charged scenes—adhere closely to the personality, social behavior, sexual role, and emotional boundaries established in this profile. **Do not** deviate from {{char}}’s defined orientation, role, or behavioral patterns unless a clear, in-character transformation is justified.]{{newline}}{{newline}}{{getvar::parsedBehaviorNotes}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedBehaviorNotes == 'None') {:
	/messages names=off 1|
	/re-replace find="/--BehaviorNotes--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (parsedSexualOrientation != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualOrientation--/g" replace="{{getvar::parsedSexualOrientation}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualLibido != '') {:
	/messages names=off 1|
	/re-replace find="/--Libido--/g" replace="- Libido: {{getvar::sexualLibido}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSexualItems != '') and (parsedSexualItems != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--SexualItems--/g" replace="{{newline}}{{newline}}### SEXUAL INVENTORY{{newline}}{{getvar::parsedSexualItems}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSexualItems == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--SexualItems--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (parsedSexualAbilities != '') and (parsedSexualAbilities != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--SexualAbilities--/g" replace="{{newline}}{{newline}}### SEXUAL ABILITIES{{newline}}{{getvar::parsedSexualAbilities}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSexualAbilities == 'None') {:
	/messages names=off 1|
	/re-replace find="/--SexualAbilities--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSexualKinks != '') and (parsedSexualKinks != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Kinks--/g" replace="{{newline}}{{newline}}### Kinks{{newline}}{{getvar::parsedSexualKinks}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif  (parsedSexualKinks == 'None'){:
	/messages names=off 1|
	/re-replace find="/--Kinks--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActKissing != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActKissing--/g" replace="{{getvar::sexualFamiliarityActKissing}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActOralG != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActOralG--/g" replace="{{getvar::sexualFamiliarityActOralG}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActOralR != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActOralR--/g" replace="{{getvar::sexualFamiliarityActOralR}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActVaginal != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActVaginal--/g" replace="{{getvar::sexualFamiliarityActVaginal}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActAnal != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActAnal--/g" replace="{{getvar::sexualFamiliarityActAnal}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActGroupSex != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActGroupSex--/g" replace="{{getvar::sexualFamiliarityActGroupSex}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamiliarityActToys != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActToys--/g" replace="{{getvar::sexualFamiliarityActToys}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((sexualityQA != '') and (sexualityQA != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--SexualityQA--/g" replace="{{newline}}{{newline}}<Q&A>{{newline}}{{getvar::sexualityQA}}{{newline}}</Q&A>" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (sexualityQA != 'None') {:
	/messages names=off 1|
	/re-replace find="/--SexualityQA--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSexualityNotes != '') and (parsedSexualityNotes != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--SexualNotes--/g" replace="{{getvar::parsedSexualityNotes}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSexualityNotes == 'None') {:
	/messages names=off 1|
	/re-replace find="/--SexualNotes--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualExperienceLevel != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualExperienceLevel--/g" replace="{{getvar::sexualExperienceLevel}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualKnowlageLevel != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualKnowlageLevel--/g" replace="{{getvar::sexualKnowlageLevel}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualExposure != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualExposure--/g" replace="{{getvar::sexualExposure}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualPartners != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualPartners--/g" replace="{{getvar::sexualPartners}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualSelfExploration != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualSelfExploration--/g" replace="{{getvar::sexualSelfExploration}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActKissing != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActKissing--/g" replace="{{getvar::sexualFamilitaryActKissing}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActOralR != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActOralR--/g" replace="{{getvar::sexualFamilitaryActOralR}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActOralG != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActOralG--/g" replace="{{getvar::sexualFamilitaryActOralG}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActVaginal != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActVaginal--/g" replace="{{getvar::sexualFamilitaryActVaginal}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActAnal != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActAnal--/g" replace="{{getvar::sexualFamilitaryActAnal}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActGroupSex != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActGroupSex--/g" replace="{{getvar::sexualFamilitaryActGroupSex}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFamilitaryActToys != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFamilitaryActToys--/g" replace="{{getvar::sexualFamilitaryActToys}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualFraming != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualFraming--/g" replace="{{getvar::sexualFraming}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualAttitude != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualAttitude--/g" replace="{{getvar::sexualAttitude}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (sexualBehavior != '') {:
	/messages names=off 1|
	/re-replace find="/--SexualBehavior--/g" replace="{{getvar::sexualBehavior}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (speechStyle != '') {:
	/messages names=off 1|
	/re-replace find="/--SpeechStyle--/g" replace="{{getvar::speechStyle}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (speechQuirks != '') {:
	/messages names=off 1|
	/re-replace find="/--SpeechQuirks--/g" replace="{{getvar::speechQuirks}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (speechTics != '') {:
	/messages names=off 1|
	/re-replace find="/--SpeechTics--/g" replace="{{getvar::speechTics}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (speechExampleString != '') {:
	/messages names=off 1|
	/re-replace find="/--SpeechExamples--/g" replace="{{newline}}{{newline}}## Speech EXAMPLES AND OPINIONS
[IMPORTANT NOTE FOR AI: This section provides --FirstName--'s speech examples, memories, thoughts, and --FirstName--'s real opinions on subjects. AI **must** avoid using them verbatim in chat and use them only for reference.]{{newline}}{{newline}}<speech_examples>{{newline}}{{getvar::speechExampleString}}{{newline}}</speech_examples>" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedSynonyms != '') and (parsedSynonyms != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Synonyms--/g" replace="{{newline}}{{newline}}- - -{{newline}}{{newline}}## SYNONYMS{{newline}}[IMPORTANT NOTE FOR AI: This section lists synonymous phrases to substitute the character's name or pronouns to avoid repetition.]{{newline}}{{getvar::parsedSynonyms}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedSynonyms == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Synonyms--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ( (parsedStoryPlan != '') and (parsedStoryPlan != 'None') ){:
	/messages names=off 1|
	/re-replace find="/--StoryPlan--/g" replace="{{newline}}{{newline}}- - -{{newline}}{{newline}}## PREMADE STORY PLAN{{newline}}{{getvar::parsedStoryPlan}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif (parsedStoryPlan == 'None' ) {:
	/messages names=off 1|
	/re-replace find="/--StoryPlan--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (previously != '') {:
	/messages names=off 1|
	/re-replace find="/--Previously--/g" replace="{{getvar::previously}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedNotes != '') and (parsedNotes != 'None')) {:
	/messages names=off 1|
	/re-replace find="/--Notes--/g" replace="{{getvar::parsedNotes}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife (parsedNotes == 'None') {:
	/messages names=off 1|
	/re-replace find="/--Notes--/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((user != '') and (user == 'Yes')) {:
	/messages names=off 1|
	/re-replace find="/--User1--/g" replace="--User--, " {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/elseif ((user != '') and (user != 'Yes')) {:
	/messages names=off 1|
	/re-replace find="/--User1--,\s/g" replace="" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|
/ife ((parsedWritingInstruct != '') and (parsedWritingInstruct != 'Nope')) {:
	/messages names=off 1|
	/re-replace find="/--WritingInstuctions--/g" replace="{{getvar::parsedWritingInstruct}}" {{pipe}}|
	/message-edit message=1 await=true {{pipe}}|
:}|