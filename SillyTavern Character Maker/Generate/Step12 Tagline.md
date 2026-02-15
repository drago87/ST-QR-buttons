/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Tagline" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step12|
/setvar key=stepDone No|

/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let selected_btn {{noop}}|
/let key=len {{noop}}|

/let key=find "Tagline Template"|
/findentry field=comment file="CMC Templates" "{{var::find}}"|
/let key=wi_uid {{pipe}}|
/getentryfield field=content file="CMC Templates" {{var::wi_uid}}|
/setvar key=taglineSheet {{pipe}}|
/ife (timePeriod != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--TimePeriod--/g" replace="{{getvar::timePeriod}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (seasons != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Season--/g" replace="{{getvar::seasons}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ( seasons == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Season--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (worldDetails != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--WorldDetails--/g" replace="{{getvar::worldDetails}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedExtraCharacters != '') and (parsedExtraCharacters != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--ExtraCharacters--/g" replace="{{newline}}{{getvar::parsedExtraCharacters}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedExtraCharacters == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--ExtraCharacters--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((lore != '') and ( lore != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Lore--/g" replace="## LORE{{newline}}{{getvar::lore}}{{newline}}{{newline}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ( lore == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Lore--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (scenarioOverview != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--ScenarioOverview--/g" replace="{{getvar::scenarioOverview}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (characterOverview != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--CharacterOverview--/g" replace="{{getvar::characterOverview}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedMedia != '') and (parsedMedia != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Real--/g" replace="**Media Origin**: {{getvar::parsedMedia}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedMedia != 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Real--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (lastName != '') and (lastName != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--LastName--/g" replace="{{getvar::lastName}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (lastName == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--LastName--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (alias != '') and (alias != 'None')){:
	/getvar key=taglineSheet|
	/re-replace find="/--Alias1--/g" replace=", Alias" {{pipe}}|
	/re-replace find="/--Alias--/g" replace=", {{getvar::alias}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (alias == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Alias1--/g" replace="" {{pipe}}|
	/re-replace find="/--Alias--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedSpecies != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Species--/g" replace="{{getvar::parsedSpecies}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedOrigin != '') and (parsedOrigin != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Origin--/g" replace="{{newline}}- Origin: {{getvar::parsedOrigin}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif  ((parsedOrigin == 'None') or ((nationality == 'None') and (ethnicity == 'None'))){:
	/getvar key=taglineSheet|
	/re-replace find="/--Origin--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (gender != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Gender--/g" replace="{{getvar::gender}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((futanari != '') and (futanari == 'Yes')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Futanari--/g" replace=" Futanari" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif  (futanari == 'No'){:
	/getvar key=taglineSheet|
	/re-replace find="/--Futanari--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((length != '') and (length != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Length--/g" replace="- Lenght: {{getvar::length}}{{newline}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (length == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Length--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((height != '') and (height != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Height--/g" replace="- Height: {{getvar::height}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (height == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Height--/g" replace="{{getvar::height}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (age != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Age--/g" replace="{{getvar::parcedAge}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceHair != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Hair--/g" replace="{{getvar::appearanceHair}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceEyes != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Eyes--/g" replace="{{getvar::appearanceEyes}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceFace != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Face--/g" replace="{{getvar::appearanceFace}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceBody != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Body--/g" replace="{{newline}} - Body: {{getvar::appearanceBody}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((tanlines != '') and (tanlines != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Tanlines--/g" replace="{{newline}} - Tanlines: {{getvar::tanlines}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (tanlines == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Tanlines--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (appearanceBreasts != '') and (appearanceBreasts != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Breasts--/g" replace="{{newline}} - Breast: {{getvar::appearanceBreasts}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (appearanceBreasts == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Breasts--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((appearanceNipples != '') and (appearanceNipples != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Nipples--/g" replace="{{newline}} - Nipple: {{getvar::appearanceNipples}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (appearanceNipples == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Nipples--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((appearanceGenitals != '') and (appearanceGenitals != 'None') and (futanari == 'yes')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Genitals--/g" replace="{{newline}} - Genitals: {{getvar::appearanceGenitals}}" {{pipe}}|
	/re-replace find="/--Pussy--/g" replace="" {{pipe}}|
	/re-replace find="/--Cock--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ((appearancePussy != '') and (appearancePussy != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Pussy--/g" replace="{{newline}} - Pussy: {{getvar::appearancePussy}}" {{pipe}}|
	/re-replace find="/--Cock--/g" replace="" {{pipe}}|
	/re-replace find="/--Genitals--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ((appearanceCock != '') and (appearanceCock != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Cock--/g" replace="{{newline}} - Cock: {{getvar::appearanceCock}}" {{pipe}}|
	/re-replace find="/--Pussy--/g" replace="" {{pipe}}|
	/re-replace find="/--Genitals--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceAnus != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Anus--/g" replace="{{getvar::appearanceAnus}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedAppearanceFeatures != '') and (parsedAppearanceFeatures != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Features--/g" replace="{{newline}}{{newline}}{{getvar::parsedAppearanceFeatures}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedAppearanceFeatures != 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Features--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedAppearanceTraits != '') and (parsedAppearanceTraits != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--ApperanceTraits--/g" replace="{{newline}}{{newline}}### APPEARANCE TRAITS{{newline}}{{getvar::parsedAppearanceTraits}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedAppearanceTraits == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--ApperanceTraits--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (outfitHeadDescription != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitHead--/g" replace="{{getvar::outfitHeadDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedAccessories != '') and (parsedAccessories == 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitAccessories--/g" replace="{{getvar::parsedAccessories}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ((parsedAccessories != '') and (parsedAccessories != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitAccessories--/g" replace="{{newline}}{{getvar::parsedAccessories}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedMakeup != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitMakeup--/g" replace="{{getvar::parsedMakeup}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (outfitNeckDescription != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitNeck--/g" replace="{{getvar::outfitNeckDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((outfitMainwearDescription != '') and (outfitMainwearDescription != 'Skip')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitMainwear--/g" replace="- Mainwear: {{getvar::outfitMainwearDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (outfitMainwearDescription == 'Skip') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitMainwear--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((outfitTopDescription != '') and (outfitTopDescription != 'Skip')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitTop--/g" replace="- Top: {{getvar::outfitTopDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (outfitTopDescription == 'Skip') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitTop--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((outfitBottomDescription != '') and (outfitBottomDescription != 'Skip')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitBottom--/g" replace="{{newline}}- Bottom: {{getvar::outfitBottomDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (outfitBottomDescription == 'Skip') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitBottom--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (outfitLegwearDescription != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitLegwear--/g" replace="{{getvar::outfitLegwearDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (outfitShoesDescription != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitShoes--/g" replace="{{getvar::outfitShoesDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (outfitUnderwearTopDescription != '') and (outfitUnderwearTopDescription != 'Skip')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitUnderwearTop--/g" replace="{{newline}}- Underwear (Top):  {{getvar::outfitUnderwearTopDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (outfitUnderwearTopDescription == 'Skip') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitUnderwearTop--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (outfitUnderwearBottomDescription != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--OutfitUnderwearBottom--/g" replace="{{getvar::outfitUnderwearBottomDescription}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (appearanceQA != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--AppearanceQA--/g" replace="{{newline}}{{newline}}</Q&A>{{newline}}{{getvar::appearanceQA}}{{newline}}</Q&A>" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (backstory != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Backstory--/g" replace="{{getvar::backstory}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (residence != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Residence--/g" replace="{{getvar::residence}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedOccupation != '') and (parsedOccupation != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Occupation--/g" replace="{{newline}}{{newline}}### OCCUPATION{{newline}}{{getvar::parsedOccupation}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedOccupation == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Occupation--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (connections != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Connections--/g" replace="{{getvar::connections}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSecret != '') and (parsedSecret != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Secret--/g" replace="{{newline}}{{newline}}{{getvar::parsedSecret}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSecret == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Secret--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedItems != '') and (parsedItems != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Items--/g" replace="{{newline}}{{newline}}### INVENTORY{{newline}}{{getvar::parsedItems}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedItems == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Items--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (parsedAbilities != '') and (parsedAbilities != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Abilities--/g" replace="{{newline}}{{newline}}### ABILITIES{{newline}}{{getvar::parsedAbilities}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedAbilities == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Abilities--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedArchetype != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Archetype--/g" replace="{{getvar::parsedArchetype}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (parsedAlignment != '') and  (parsedAlignment != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Alignment--/g" replace="{{newline}}{{newline}}{{getvar::parsedAlignment}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedAlignment == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Alignment--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (personalityTags != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--PersonalityTags--/g" replace="- Personality Tags: {{getvar::personalityTags}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( personalityIntelligenceLevel != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--IntelligenceLevel--/g" replace="- Intelligence Level: {{getvar::personalityIntelligenceLevel}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (personalitycognitiveAbilities != '') and (personalitycognitiveAbilities != 'None') ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--CognitiveAbilities--/g" replace="{{newline}}{{newline}}- Cognitive Abilities: {{getvar::personalitycognitiveAbilities}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (personalitycognitiveAbilities == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--CognitiveAbilities--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSentientLevel != '') and (parsedSentientLevel != 'None')) {:
/getvar key=taglineSheet|
	/re-replace find="/--SentientLevel--/g" replace="{{newline}}### ANIMALISTIC COGNITION{{newline}}{{getvar::parsedSentientLevel}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSentientLevel == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SentientLevel--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( personalitySocialBehavior != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SocialBehavior--/g" replace="- Social Behavior: {{getvar::personalitySocialBehavior}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (personalitySocialSkills != '') and (personalitySocialSkills != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SocialSkills--/g" replace="{{newline}}{{newline}}- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (personalitySocialSkills == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SocialSkills--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedAspiration != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--MainAspiration--/g" replace="{{getvar::mainAspiration}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedTraits != '') and (parsedTraits != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--UniqueTraits--/g" replace="{{getvar::parsedTraits}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedTraits == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--UniqueTraits--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (personalityQA != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--PersonalityQA--/g" replace="{{newline}}{{newline}}<Q&A>{{newline}}{{getvar::personalityQA}}{{newline}}</Q&A>" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedBehaviorNotes != '') and (parsedBehaviorNotes != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--BehaviorNotes--/g" replace="{{newline}}{{newline}}- - -

## [BEHAVIOR_NOTES]
[IMPORTANT NOTE FOR AI: This section governs how --FirstName-- behaves moment to moment. In all interactions—especially intimate or emotionally charged scenes—adhere closely to the personality, social behavior, sexual role, and emotional boundaries established in this profile. **Do not** deviate from {{char}}’s defined orientation, role, or behavioral patterns unless a clear, in-character transformation is justified.]{{newline}}{{newline}}{{getvar::parsedBehaviorNotes}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedBehaviorNotes == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--BehaviorNotes--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedSexualOrientation != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualOrientation--/g" replace="{{getvar::parsedSexualOrientation}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (sexualLibido != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Libido--/g" replace="- Libido: {{getvar::sexualLibido}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSexualItems != '') and (parsedSexualItems != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualItems--/g" replace="{{newline}}{{newline}}### SEXUAL INVENTORY{{newline}}{{getvar::parsedSexualItems}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSexualItems == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualItems--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (parsedSexualAbilities != '') and (parsedSexualAbilities != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualAbilities--/g" replace="{{newline}}{{newline}}### SEXUAL ABILITIES{{newline}}{{getvar::parsedSexualAbilities}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSexualAbilities == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualAbilities--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSexualKinks != '') and (parsedSexualKinks != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Kinks--/g" replace="{{newline}}{{newline}}### Kinks{{newline}}{{getvar::parsedSexualKinks}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif  (parsedSexualKinks == 'None'){:
	/getvar key=taglineSheet|
	/re-replace find="/--Kinks--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((sexualityQA != '') and (sexualityQA != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualityQA--/g" replace="{{newline}}{{newline}}<Q&A>{{newline}}{{getvar::sexualityQA}}{{newline}}</Q&A>" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (sexualityQA != 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualityQA--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSexualityNotes != '') and (parsedSexualityNotes != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualNotes--/g" replace="{{getvar::parsedSexualityNotes}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSexualityNotes == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SexualNotes--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (speechStyle != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SpeechStyle--/g" replace="{{getvar::speechStyle}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (speechQuirks != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SpeechQuirks--/g" replace="{{getvar::speechQuirks}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (speechTics != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SpeechTics--/g" replace="{{getvar::speechTics}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (speechExampleString != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--SpeechExamples--/g" replace="{{newline}}{{newline}}## Speech EXAMPLES AND OPINIONS
[IMPORTANT NOTE FOR AI: This section provides --FirstName--'s speech examples, memories, thoughts, and --FirstName--'s real opinions on subjects. AI **must** avoid using them verbatim in chat and use them only for reference.]{{newline}}{{newline}}<speech_examples>{{newline}}{{getvar::speechExampleString}}{{newline}}</speech_examples>" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedSynonyms != '') and (parsedSynonyms != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Synonyms--/g" replace="{{newline}}{{newline}}- - -{{newline}}{{newline}}## SYNONYMS{{newline}}[IMPORTANT NOTE FOR AI: This section lists synonymous phrases to substitute the character's name or pronouns to avoid repetition.]{{newline}}{{getvar::parsedSynonyms}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedSynonyms == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Synonyms--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ( (parsedStoryPlan != '') and (parsedStoryPlan != 'None') ){:
	/getvar key=taglineSheet|
	/re-replace find="/--StoryPlan--/g" replace="{{newline}}{{newline}}- - -{{newline}}{{newline}}## PREMADE STORY PLAN{{newline}}{{getvar::parsedStoryPlan}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif (parsedStoryPlan == 'None' ) {:
	/getvar key=taglineSheet|
	/re-replace find="/--StoryPlan--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (previously != '') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Previously--/g" replace="{{getvar::previously}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedNotes != '') and (parsedNotes != 'None')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--Notes--/g" replace="{{getvar::parsedNotes}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife (parsedNotes == 'None') {:
	/getvar key=taglineSheet|
	/re-replace find="/--Notes--/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((user != '') and (user == 'Yes')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--User1--/g" replace="--User--, " {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/elseif ((user != '') and (user != 'Yes')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--User1--,\s/g" replace="" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((parsedWritingInstruct != '') and (parsedWritingInstruct != 'Nope')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--WritingInstuctions--/g" replace="{{getvar::parsedWritingInstruct}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|
/ife ((firstName != '') and (firstName != 'Nope')) {:
	/getvar key=taglineSheet|
	/re-replace find="/--FirstName--/g" replace="{{getvar::firstName}}" {{pipe}}|
	/setvar key=taglineSheet {{pipe}}|
:}|

/var key=do No|
/var key=variableName "tagline"|
/ife ({{var::variableName}} == '') {:
    /var key=do Yes|
:}|
/elseif (skip == 'Update') {:
    /getvar key={{var::variableName}}|
    /buttons labels=["Yes", "No"] Do you want to set or redo {{var::variableName}} (current value: {{pipe}})?|
    /var key=do {{pipe}}|
    /ife (do == '') {:
        /echo Aborting |
        /abort
    :}|
:}|
/ife ( do == 'Yes' ) {:
	/setvar key=genSettings {}|
	/setvar key=genSettings index=wi_book_key "Tagline"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	/buttons labels=["Yes", "No"] Do you want to include the First Message as context for the generation of the Tagline?|
	/let key=selected_btn {{pipe}}|
	/ife (selected_btn == 'Yes') {:
		/setvar key=optionalFirstMessageGuidance "{{newline}}{{newline}}## **CHARACTER FIRST MESSAGE:**{{newline}}```{{newline}}{{getvar::firstMessage}}{{newline}}```"
	:}|
	/else {:
		/setvar key=optionalFirstMessageGuidance {{noop}}|
	:}|

	/setvar key=genSettings index=buttonPrompt Is this diplayed name and Character Description the one you want to display?|

	//[[Generate with Prompt]]|
	
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
	/addvar key={{var::variableName}} "{{newline}}{{newline}}In-Chat Name: {{getvar::firstName}}"|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|

/ife (user == 'No') {:
	/re-replace find="/Tagline:\s/g" replace="Tagline: (NarratorPOV)" {{getvar::tagline}}|
	/setvar key=tagline {{pipe}}|
:}|
/elseif (userGender == 'Male') {:
	/re-replace find="/Tagline:\s/g" replace="Tagline: (MalePOV)" {{getvar::tagline}}|
	/setvar key=tagline {{pipe}}|
:}|
/elseif (userGender == 'Female') {:
	/re-replace find="/Tagline:\s/g" replace="Tagline: (FemalePOV)" {{getvar::tagline}}|
	/setvar key=tagline {{pipe}}|
:}|
/elseif ((userGender == 'Gender Neutral') or (userGender == 'Anything')) {:
	/re-replace find="/Tagline:\s/g" replace="Tagline: (AnyPOV)" {{getvar::tagline}}|
	/setvar key=tagline {{pipe}}|
:}|

/messages names=off {{getvar::taglineID}}|
/let key=mess {{pipe}}|
/ife (mess == '') {:
	/sendas name={{char}} {{getvar::tagline}}
:}|
/else {:
	/message-edit message={{getvar::taglineID}} await=true {{getvar::tagline}}|
:}|