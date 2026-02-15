/setvar key=createdLorebook {"entries":{|

/setvar key=lorebookEntryTemplate \"--uid--":{"uid":--uid--,"key":--primKey--,"keysecondary":--secKey--,"comment":"--comment--","content":"--content--","constant":false,"vectorized":false,"selective":true,"selectiveLogic":--sLogic--,"addMemo":false,"order":100,"position":--pos--,"disable":false,"ignoreBudget":false,"excludeRecursion":false,"preventRecursion":false,"matchPersonaDescription":false,"matchCharacterDescription":false,"matchCharacterPersonality":false,"matchCharacterDepthPrompt":false,"matchScenario":false,"matchCreatorNotes":false,"delayUntilRecursion":false,"probability":100,"useProbability":true,"depth":--depth--,"outletName":"--outletName--","group":"","groupOverride":false,"groupWeight":100,"scanDepth":--scanDepth--,"caseSensitive":null,"matchWholeWords":null,"useGroupScoring":null,"automationId":"","role":--role--,"sticky":0,"cooldown":0,"delay":0,"triggers":[],"displayIndex":--uid--,"characterFilter":{"isExclude":false,"names":[],"tags":[]}}|

/let key=lorebookIndex 0|

/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
//Covering: coveringType, coveringFront, coveringSidesBack, coveringArmsLegs|
//Outlet: No|

/addvar key=createdLorebook {{getvar::templateCopy}}|

/ife ((appearanceGenitals == 'None') or (appearanceGenitals == '')) {:
	/ife ((appearanceBreasts != 'None') and (appearanceBreasts != '')) {:
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Breasts: breastSize, appearanceNipples, appearanceBreasts|
		//Outlet: No|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
	
	/ife ((appearanceCock != 'None') and (appearanceCock != '')) {:
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Cock: cockSize, testicularPosition, pubicHair, appearanceCock|
		//Outlet: No|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
	
	/ife ((appearancePussy != 'None') and (appearancePussy != '')) {:
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Pussy: clitVisibility, labiaMajoraFullness, labiaMajoraOutsideColor, labiaMajoraInsideColor, labiaMinoraVisibility, labiaMinoraOutsideColor, labiaMinoraInsideColor, externalVulvaState, pubicHair, appearancePussy|
		//Outlet: No|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
:}|
/else {:
	/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
	/ife ((appearanceBreasts != 'None') and (appearanceBreasts != '')) {:
		//Breasts: breastSize, appearanceNipples, appearanceBreasts|
		//Outlet: No|
	:}|
	
	/ife ((appearanceCock != 'None') and (appearanceCock != '')) {:
		//Cock: cockSize, testicularPosition, pubicHair, appearanceCock|
		//Outlet: No|
	:}|
	
	/ife ((appearancePussy != 'None') and (appearancePussy != '')) {:
		//Pussy: clitVisibility, labiaMajoraFullness, labiaMajoraOutsideColor, labiaMajoraInsideColor, labiaMinoraVisibility, labiaMinoraOutsideColor, labiaMinoraInsideColor, externalVulvaState, pubicHair, appearancePussy|
		//Outlet: No|
	:}|
	/addvar key=createdLorebook {{getvar::templateCopy}}|
:}|

/ife ((appearanceAnus != 'None') and (appearanceAnus != '')) {:
	/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
	//Anus: appearanceAnus|
	//Outlet: No|
	
	/addvar key=createdLorebook {{getvar::templateCopy}}|
:}|

/ife ((appearanceTraits != 'None') and (appearanceTraits != '')) {:
	/keys {{getvar::appearanceTraits}}|
	/let key=keys1 {{pipe}}|
	/foreach {{var::keys1}} {:
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Appearance Traits: appearanceTraits, appearanceTraitsDetails, appearanceTraitsEffect|
		//Outlet: Maybe|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
:}|

/ife ((appearanceFeatures != 'None') and (appearanceFeatures != '')) {:
	/foreach {{getvar::appearanceFeatures}} {:
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Appearance Features: appearanceFeatures, appearanceFeaturesDescriptions, appearanceFeaturesPlacements, appearanceFeaturesTypes|
		//Outlet: Maybe|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
:}|

/ife ((outfitHead != 'None') and (outfitHead != '')) {:
	/foreach {{getvar::outfits}} {:
		/getvar 
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		//Appearance Features: appearanceFeatures, appearanceFeaturesDescriptions, appearanceFeaturesPlacements, appearanceFeaturesTypes|
		//Outlet: Maybe|
		
		/addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|
:}|


//selectiveLogic 0:AND ANY, 1:NOT ALL, 2:NOT ANY, 3:AND ALL|
//position: 0:↑Char, 1:↓Char, 2:↑AN, 3:↓AN, 4:@D, 5:↑EM, 6:↓EM, 7:Outlet|
//role: only for position 4 all else 'null': 0: System, 1: User, 2: Char|
//↑: Before, ↓: After|
//Char: Character Definitions, EM: Example Messages, AN: Autor's Note, @D role: At Depth|
//scanDepth: null or number