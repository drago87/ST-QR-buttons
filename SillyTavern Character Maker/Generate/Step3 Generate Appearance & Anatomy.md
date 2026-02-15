/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Appearance & Anatomy" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step3|


/setvar key=skip Update|
/ife ( stepDone == 'No') {:
	/buttons labels=["Skip", "Update"] Do you want to skip or update already generated content? You will get a question for each already done if you select Update.|
	/setvar key=skip {{pipe}}|
	/ife ( skip == ''){:
		/echo Aborting |
		/abort
	:}|
:}|

/setvar key=stepDone No|

/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let selected_btn {{noop}}|

/ife (unitType == '') {:
	/buttons labels=["Metric", "Imperial"] What type of measuring system do you want to use during these generations?|
	/setvar key=unitType {{pipe}}|
	/ife ( unitType == ''){:
		/echo Aborting |
		/abort
	:}|
:}|
/ife (unitType == 'Metric') {:
	/setvar key=unitTypeShort cm|
:}|
/else {:
	/setvar key=unitTypeShort inch|
:}|

/ife ((characterArchetype == 'Anthropomorphic') or (characterArchetype == 'Beastkin') or (characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon') or (characterArchetype == 'Digimon')) {:
	/ife (coveringType == '') {:
		/setvar key=coveringType ["Skin", "Fur", "Scale", "Feather"]|
		/buttons labels={{getvar::coveringType}} "Select the type of covering {{getvar::firstName}} should have."|
		/setvar key=coveringType {{pipe}}|
		/ife (coveringType == '') {:
			/echo Aborting |
			/abort
		:}|
	:}|
:}|

//**Features**|
/var key=do No|
/var key=variableName "appearanceFeatures"|
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
	/ife ((characterArchetype == 'Human') or (characterArchetype == 'Android')) {:
		/setvar key=genSettings index=wi_book_key "Appearance Features Humanoid"|
	:}|
	/else {:
		/setvar key=genSettings index=wi_book_key "Appearance Features Other"|
	:}|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=needOutput No|
	/setvar key=genSettings index=outputIsList Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|

/ife ((appearanceFeatures != 'None') and (appearanceFeatures != '')) {:
	//Features Type|
	/var key=do No|
	/var key=variableName "appearanceFeaturesTypes"|
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
		/setvar key=genSettings index=wi_book_key "Appearance Features Type"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/let key=tempOutputList []|
			/foreach {{getvar::appearanceFeatures}} {:
				/setvar key=feature {{var::item}}|
				/setvar key=genSettings index=buttonPrompt Is this the feature type you want for the {{getvar::feature}}?|
				/:"CMC Logic.GenerateWithPrompt"|
				/len {{var::tempOutputList}}|
				/var key=tempOutputList index={{pipe}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/foreach {{var::tempOutputList}} {:
				/addvar key={{var::variableName}} {{var::item}}|
			:}|
			/flushvar {{var::variableName}}Item|
		:}|
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
	//-----------|
	
	//Features Placement|
	
	/var key=do No|
	/var key=variableName "appearanceFeaturesPlacements"|
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
		/setvar key=genSettings index=wi_book_key "Appearance Features Placement"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/let key=tempOutputList []|
			/foreach {{getvar::appearanceFeatures}} {:
				/setvar key=feature {{var::item}}|
				/getvar key=appearanceFeaturesTypes index={{var::index}}|
				/setvar key=featureType {{pipe}}|
				/ife (featureType == 'None') {:
					/setvar key=featureType {{noop}}|
				:}|
				/else {:
					/setvar key=featureType "Let the phrasing be influenced by its {{getvar::featureType}} form."
				:}|
				/setvar key=genSettings index=buttonPrompt Is this the feature placement you want for the {{getvar::feature}}?|
				/:"CMC Logic.GenerateWithPrompt"|
				/len {{var::tempOutputList}}|
				/var key=tempOutputList index={{pipe}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
				/flushvar featureType|
				/flushvar featureDescription|
			:}|
			/foreach {{var::tempOutputList}} {:
				/addvar key={{var::variableName}} {{var::item}}|
			:}|
			/flushvar {{var::variableName}}Item|
		:}|
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
	//-----------|
	
	//Features Description|
	
	/var key=do No|
	/var key=variableName "appearanceFeaturesDescriptions"|
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
		/setvar key=genSettings index=wi_book_key "Appearance Features Description"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/let key=tempOutputList []|
			/foreach {{getvar::appearanceFeatures}} {:
				/setvar key=feature {{var::item}}|
				/getvar key=appearanceFeaturesTypes index={{var::index}}|
				/setvar key=featureType {{pipe}}|
				/ife (featureType == 'None') {:
					/setvar key=featureType {{noop}}|
				:}|
				/else {:
					/setvar key=featureType ", incorporating {{getvar::featureType}} if it adds clarity"|
				:}|
				/getvar key=appearanceFeaturesPlacements index={{var::index}}|
				/setvar key=featuresPlacement {{pipe}}|
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (featureType != '') {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- Use any descriptive terms provided about the feature’s style, texture, or form naturally in the sentence. Do not label them or repeat the feature name."|
					
				:}|
				/setvar key=genSettings index=buttonPrompt Is this the feature description you want for the {{getvar::feature}}?|
				/:"CMC Logic.GenerateWithPrompt"|
				/len {{var::tempOutputList}}|
				/var key=tempOutputList index={{pipe}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
				/flushvar logicBasedInstruction|
			:}|
			/foreach {{var::tempOutputList}} {:
				/addvar key={{var::variableName}} {{var::item}}|
			:}|
			/flushvar {{var::variableName}}Item|
		:}|
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
	//-----------|
:}|
/else {:
	/setvar key=appearanceFeaturesTypes None|
	/addvar key=dataBaseNames appearanceFeaturesTypes|
	/setvar key=appearanceFeaturesPlacements None|
	/addvar key=dataBaseNames appearanceFeaturesPlacements|
	/setvar key=appearanceFeaturesDescriptions None|
	/addvar key=dataBaseNames appearanceFeaturesDescriptions|
:}|

/setvar key=parsedAppearanceFeatures {{noop}}|
/ife ((appearanceFeatures != '') and (appearanceFeatures != 'None') and (appearanceFeatures is list)) {:
	/foreach {{getvar::appearanceFeatures}} {:
		/ife ( index > 0) {:
			/addvar key=parsedAppearanceFeatures "{{newline}}{{newline}}"|
		:}|
		/addvar key=parsedAppearanceFeatures "- Feature: {{var::item}}"|
		/getvar key=appearanceFeaturesTypes index={{var::index}}|
		/let key=aType {{pipe}}|
		/ife (aType != 'None') {:
			/addvar key=parsedAppearanceFeatures "{{newline}}  - {{var::item}} Type: {{var::aType}}"|
		:}|
		/getvar key=appearanceFeaturesPlacements index={{var::index}}|
		/addvar key=parsedAppearanceFeatures "{{newline}}  - {{var::item}} Placement: {{pipe}}"|
		/getvar key=appearanceFeaturesDescriptions index={{var::index}}|
		/addvar key=parsedAppearanceFeatures "{{newline}}  - {{var::item}} Description: {{pipe}}"|
	:}|
:}|
/else {:
	/setvar key=parsedAppearanceFeatures None|
	/addvar key=dataBaseNames parsedAppearanceFeatures|
:}|


/ife ( ((characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon') or (characterArchetype == 'Digimon') or (characterArchetype == 'Tauric')) and (lenghtHeight == '')) {:
	/buttons labels=["Length (Their body is horizontal or animal-like — e.g., snake, lizard, quadruped)", "Height (They stand upright like a human or humanoid)", "Both (They have both vertical and elongated traits — e.g., centaur, dragon)"] <div>How should this character's body be measured?</div><div>Choose the option that best fits their physical structure:</div>|
	/setvar key=lenghtHeight {{pipe}}|
	/ife (lenghtHeight == '') {:
		/echo Aborting |
		/abort
	:}|
	/re-replace find="/\s\(.*$/g" replace="" {{getvar::lenghtHeight}}|
	/setvar key=lenghtHeight {{pipe}}|
:}|

//**Length**|

/ife ((( lenghtHeight == 'Length') or ( lenghtHeight == 'Both')) and (((characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon') or (characterArchetype == 'Digimon') or (characterArchetype == 'Tauric')))) {:
	/var key=do No|
	/var key=variableName "length"|
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
		/setvar key=genSettings index=wi_book_key "Length"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Unit Type: {{getvar::unitType}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/else {:
			/setvar key=genSettings index=contextKey []|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
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
:}|
/else {:
	/setvar key=length None|
	/addvar key=dataBaseNames length|
:}|
//-----------|

//**Height**|
/ife ((( lenghtHeight == 'Height') or ( lenghtHeight == 'Both')) or  (((characterArchetype != 'Animalistic') and (characterArchetype != 'Pokémon') and (characterArchetype != 'Digimon')))) {:
	/var key=do No|
	/var key=variableName "height"|
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
		/setvar key=genSettings index=wi_book_key "Height"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Unit Type: {{getvar::unitType}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/else {:
			/setvar key=genSettings index=contextKey []|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
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
:}|
/else {:
	/setvar key=height None|
	/addvar key=dataBaseNames height|
:}|

//-----------|

//**Face**|
/var key=do No|
/var key=variableName "appearanceFace"|
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
	/setvar key=genSettings index=wi_book_key "Appearance Face"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=genSettings index=useAnatomy Yes|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	/ife ((coveringType == 'Fur') or (coveringType == 'Scale') or (coveringType == 'Feather')) {:
	/setvar key=faceTaskColor " , and color"|
	/setvar key=faceRulesColor "{{newline}}- Include visible **color** and specify which parts are each color if multiple shades are present."|
	:}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|

//**Hair**|
/var key=do No|
/var key=variableName "appearanceHair"|
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
	
	/ife ((hairLenght == '') or (skip == 'Update')) {:
		/buttons labels=["Short", "Medium", "Long", "Extra Long"] What lenght of hair to you want for {{getvar::firstName}}?|
		/setvar key=hairLenght {{pipe}}|
		/ife (hairLenght == '') {:
	        /echo Aborting |
	        /abort
	    :}|
	:}|
	
	/ife ((hairStyleSelected == '') or (skip == 'Update')) {:
		/buttons labels=["Yes", "No"] Do you want to set a hairstyle?|
		/var key=do {{pipe}}|
		/ife (do == '') {:
	        /echo Aborting |
	        /abort
	    :}|
	    /elseif (do == 'Yes') {:
		    /findentry field=comment file="CMC Variables" "Hairstyles"|
		    /let key=wi_uid {{pipe}}|
		    /getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
		    /setvar key=hairStyles "Manual Input---{{pipe}}"|
		    /split find="---" {{getvar::hairStyles}}|
		    /setvar key=hairStyles {{pipe}}|
		    /buttons labels={{getvar::hairStyles}} Select the hairstyle you want for {{getvar::firstName}}|
		    /setvar key=hairStyleSelected {{pipe}}|
		    /ife (hairStyleSelected == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		    /elseif (hairStyleSelected == 'Manual Input') {:
			    /input Write the hairstyle you want to use for {{getvar::firstName}}|
			    /setvar key=hairStyleSelected {{pipe}}|
		    :}|
	    :}|
	    /else {:
		    /setvar key=hairStyleSelected {{noop}}|
	    :}| 
	:}|
	
	/ife (hairStyleSelected != '') {:
		/setvar key=hairStyle "{{getvar::hairLengt}} length hair, styled in a {{getvar::hairStyleSelected}} hairdo"|
	:}|
	/else {:
		/setvar key=hairStyle "{{getvar::hairLengt}} length hair"|
	:}|
	
	/setvar key=genSettings {}|
	/setvar key=genSettings index=wi_book_key "Appearance Hair"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=genSettings index=useAnatomy Yes|
	/setvar key=extra []|
	/addvar key=extra "- Face Description: {{getvar::appearanceFace}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|

//**Eyes**|
/var key=do No|
/var key=variableName "appearanceEyes"|
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
	/setvar key=genSettings index=wi_book_key "Appearance Eyes"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=genSettings index=useAnatomy Yes|
	/setvar key=extra []|
	/addvar key=extra "- Face Description: {{getvar::appearanceFace}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|

/ife ((characterArchetype == 'Anthropomorphic') or (characterArchetype == 'Beastkin') or (characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon') or (characterArchetype == 'Digimon')) {:
	
	/var key=do No|
	/var key=variableName "coveringFront"|
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
		/setvar key=genSettings index=wi_book_key "Covering Front"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (variable == 'content') {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
			
		:}|
		/else {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
		:}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
	
		//setvar key=genSettings index=buttonPrompt CHANGE_THIS_PROMPT|
	
		//setvar key=genSettings index=guidencePrompt CHANGE_THIS_PROMPT|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
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
	
	
	/var key=do No|
	/var key=variableName "coveringSidesBack"|
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
		/setvar key=genSettings index=wi_book_key "Covering Sides and Back"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (variable == 'content') {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
			
		:}|
		/else {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
		:}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
	
		//setvar key=genSettings index=buttonPrompt CHANGE_THIS_PROMPT|
	
		//setvar key=genSettings index=guidencePrompt CHANGE_THIS_PROMPT|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
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
	
	
	/var key=do No|
	/var key=variableName "coveringArmsLegs"|
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
		/setvar key=genSettings index=wi_book_key "Covering Arms and Legs"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (variable == 'content') {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
			
		:}|
		/else {:
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
		:}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
	
		//setvar key=genSettings index=buttonPrompt CHANGE_THIS_PROMPT|
	
		//setvar key=genSettings index=guidencePrompt CHANGE_THIS_PROMPT|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
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
:}|
/else {:
	/setvar key=coveringFront None|
	/addvar key=dataBaseNames coveringFront|
	/setvar key=coveringSidesBack None|
	/addvar key=dataBaseNames coveringSidesBack|
	/setvar key=coveringArmsLegs None|
	/addvar key=dataBaseNames coveringArmsLegs|
:}|

/setvar key=parsedCovering {{noop}}|
/ife (( coveringType != 'None') and ( coveringType != '')) {:
	/setvar key=parsedCovering "- {{getvar::coveringType}} Pattern:{{newline}} - Front (Chest, Stomach, and Inner Thighs): {{getvar::coveringFront}}{{newline}} - Sides and Back: {{getvar::coveringSidesBack}}{{newline}} - Arms and Legs:  {{getvar::coveringArmsLegs}}"
:}|
/else {:
	/setvar key=parsedCovering None|
	/addvar key=dataBaseNames parsedCovering|
:}|

//**Body**|

/var key=do No|
/var key=variableName "appearanceBody"|
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
	
	/ife (gender == 'Female') {:
		/ife ((buttSize == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Butt Size"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=size {{pipe}}|
			/split find="---" {{var::needed}}|
			/var key=size {{pipe}}|
			/buttons labels={{var::size}} What size is {{getvar::firstName}}'s butt?|
			/setvar key=buttSize {{pipe}}|
			/ife (buttSize == '') {:
				/echo Aborting |
		        /abort
			:}|
		:}|
		/ife ((buttShape == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Butt Shape"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=shape {{pipe}}|
			/split find="---" {{var::needed}}|
			/var key=shape {{pipe}}|
			/buttons labels={{var::shape}} What shape is {{getvar::firstName}}'s butt?|
			/setvar key=buttShape {{pipe}}|
			/ife (buttShape == '') {:
				/echo Aborting |
		        /abort
			:}|
		:}|
		/ife ((buttFirmness == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Butt Firmness"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=firmness {{pipe}}|
			/split find="---" {{var::needed}}|
			/var key=firmness {{pipe}}|
			/buttons labels={{var::firmness}} What firmness is {{getvar::firstName}}'s butt?|
			/setvar key=buttFirmness {{pipe}}|
			/ife (buttFirmness == '') {:
				/echo Aborting |
		        /abort
			:}|
		:}|
		
		/ife ((hipsSize == '') or (skip == 'Update')) {:
				/findentry field=comment file="CMC Variables" "Hip Size"|
				/let key=wi_uid {{pipe}}|
				/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
				/let key=list {{pipe}}|
				/split find="---" {{var::list}}|
				/var key=list {{pipe}}|
				/buttons labels={{var::list}} What size is {{getvar::firstName}}'s hips?|
				/setvar key=hipsSize {{pipe}}|
				/ife (hipsSize == '') {:
			        /echo Aborting |
			        /abort
			    :}|
			:}|
		
		/ife ((thighsSize == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Thigh Size"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What size is {{getvar::firstName}}'s thighs?|
			/setvar key=thighsSize {{pipe}}|
			/ife (thighsSize == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
	:}|
	
	/setvar key=genSettings {}|
	/setvar key=genSettings index=wi_book_key "Appearance Body"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext No|
	/setvar key=genSettings index=useAnatomy Yes|
	
	/setvar key=extra []|
	/ife (appearanceFeatures != 'None') {:
		/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
	:}|
	/ife (gender == 'Female') {:
		/ife (buttSize != 'Skip') {:
			/addvar key=extra "Butt Size: {{getvar::buttSize}}"|
		:}|
		/ife (buttShape != 'Skip') {:
			/addvar key=extra "Butt Shape: {{getvar::buttShape}}"|
		:}|
		/ife (buttFirmness != 'Skip') {:
			/addvar key=extra "Butt Firmness: {{getvar::buttFirmness}}"|
		:}|
		/ife (thighsSize != 'Skip') {:
			/addvar key=extra "- Thigh Size: {{getvar::thighsSize}}"|
		:}|
		/ife (hipsSize != 'Skip') {:
			/addvar key=extra "- Hip Size: {{getvar::hipsSize}}"|
		:}|
	:}|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/ife (gender == 'Female') {:
		/setvar key=femaleBodyShapeRule "{{newline}}- Include visible lower-body proportions such as butt size, thighs size, and hips size when describing the body."|
	:}|
	/else {:
		/setvar key=femaleBodyShapeRule {{noop}}|
	:}|

	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	/ife (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Do not include tails, wings, hooves, digitigrade legs, animalistic limbs, or non-human anatomy. Use human structure only."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Describe only human anatomical structure — posture should be upright and bipedal, with no tails or animalistic features. Focus on general proportions and center of gravity if relevant."|
		
	:}|
	/elseif (( characterArchetype != 'Human') and ( characterArchetype != 'Android') and (appearanceFeatures != 'None')) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- If '{{getvar::appearanceFeatures}}' includes non-human traits (e.g., tail, claws, digitigrade stance), describe how they affect posture, balance, or silhouette."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Incorporate any relevant non-human traits (e.g., tails, hooves, digitigrade legs) from '{{getvar::appearanceFeatures}}' into descriptions of balance, movement, or posture."|
	:}|
	/elseif (( characterArchetype != 'Human') and ( characterArchetype != 'Android') and (appearanceFeatures == 'None')) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use a species-appropriate body structure, but do not describe any extra non-human features unless clearly implied by species type."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Do not invent tails, claws, wings, or non-human limbs unless strongly implied by species type. Focus on body shape, stance, or balance based on general archetype alone."|		
	:}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|

/ife (gender == 'Female') {:
	//**Breasts**|
	/var key=do No|
	/var key=variableName "appearanceBreasts"|
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
		
		/ife ((breastSize == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Breast Size"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=size {{pipe}}|
			/split find="---" {{var::size}}|
			/var key=size {{pipe}}|
			/buttons labels={{var::size}} What size is {{getvar::firstName}}'s Breasts?|
			/setvar key=breastSize {{pipe}}|
			/ife (breastSize == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/ife ((breastShape == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Breast Shape"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=shape {{pipe}}|
			/split find="---" {{var::shape}}|
			/var key=shape {{pipe}}|
			/buttons labels={{var::shape}} What shape is {{getvar::firstName}}'s Breasts?|
			/setvar key=breastShape {{pipe}}|
			/ife (breastShape == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/ife ((breastFirmness == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Breast Firmness"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=size {{pipe}}|
			/split find="---" {{var::size}}|
			/var key=size {{pipe}}|
			/buttons labels={{var::size}} What firmness is {{getvar::firstName}}'s Breasts?|
			/setvar key=breastFirmness {{pipe}}|
			/ife (breastFirmness == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/setvar key=genSettings {}|
		/setvar key=genSettings index=wi_book_key "Appearance Breasts"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=genSettings index=useAnatomy Yes|
		/setvar key=extra []|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/ife (breastSize != 'Skip') {:
			/addvar key=extra "- Breast Size: {{getvar::breastSize}}"|
		:}|
		/ife (breastShape != 'Skip') {:
			/addvar key=extra "- Breast Shape: {{getvar::breastShape}}"|
		:}|
		
		/ife (breastFirmness != 'Skip') {:
			/addvar key=extra "- Breast Firmness: {{getvar::breastFirmness}}"|
		:}|
		/ife ((appearanceFeatures != 'None') and ('Breast' in parsedAppearanceFeatures) or ('breast' in parsedAppearanceFeatures)) {:
			/addvar key=extra "{{newline}}{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (characterArchetype == 'Tauric') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Breasts are located on the humanoid torso — do not describe them as part of the lower animal body."|
			
		:}|
		/elseif ((characterArchetype == 'Animalistic') and (speciesGroup != 'Fantasy')) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Do not describe breasts — most realistic Animalistic species lack humanoid chest anatomy. Focus on muscular bulk or species-appropriate chest traits."|
			
		:}|
		/elseif ((characterArchetype == 'Animalistic') and ((speciesGroup == 'Fantasy') or (speciesGroup == 'Alien') or (speciesGroup == 'Demonic') or (speciesGroup == 'Mammal'))) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Breasts may be described only if the species is explicitly humanoid or mammalian in design. Use anatomical framing consistent with fantasy physiology."|
			
		:}|
		
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
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

//-----------|

//**Nipples**|
	/var key=do No|
	/var key=variableName "appearanceNipples"|
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
	
		/ife ((nippleType == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Nipple Type"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=type {{pipe}}|
			/split find="---" {{var::type}}|
			/var key=type {{pipe}}|
			/buttons labels={{var::type}} What is {{getvar::firstName}}'s nipple type?|
			/setvar key=nippleType {{pipe}}|
			/ife (nippleType == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((nippleProtrusion == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Nipple Protrusion"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=protrusion {{pipe}}|
			/split find="---" {{var::protrusion}}|
			/var key=protrusion {{pipe}}|
			/buttons labels={{var::protrusion}} What is {{getvar::firstName}}'s nipple protrusion?|
			/setvar key=nippleProtrusion {{pipe}}|
			/ife (nippleProtrusion == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((areolaSize == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Areola Size"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=size {{pipe}}|
			/split find="---" {{var::size}}|
			/var key=size {{pipe}}|
			/buttons labels={{var::size}} What is {{getvar::firstName}}'s areola size?|
			/setvar key=areolaSize {{pipe}}|
			/ife (areolaSize == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((areolaShape == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Areola Shape"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=shape {{pipe}}|
			/split find="---" {{var::shape}}|
			/var key=shape {{pipe}}|
			/buttons labels={{var::shape}} What is {{getvar::firstName}}'s areola shape?|
			/setvar key=areolaShape {{pipe}}|
			/ife (areolaShape == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		
		/setvar key=genSettings {}|
		/setvar key=genSettings index=wi_book_key "Appearance Nipples"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=genSettings index=useAnatomy Yes|
		/setvar key=extra []|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
		/ife (nippleType != 'Skip') {:
			/addvar key=extra "- Nipple Type: {{getvar::nippleType}}"|
		:}|
		/ife (nippleProtrusion != 'Skip') {:
			/addvar key=extra "- Nipple Protrusion: {{getvar::nippleProtrusion}}"|
		:}|
		/ife (areolaSize != 'Skip') {:
			/addvar key=extra "- Areola Size: {{getvar::areolaSize}}"|
		:}|
		/ife (areolaShape != 'Skip') {:
			/addvar key=extra "- Areola Shape: {{getvar::areolaShape}}"|
		:}|
		/ife ((appearanceFeatures != 'None') and ('nipple' in parsedAppearanceFeatures) or ('Nipple' in parsedAppearanceFeatures)) {:
			/addvar key=extra "{{newline}}{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (characterArchetype == 'Tauric') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Nipples are placed on the humanoid upper torso. Do not describe any features on the lower animal body."|
			
		:}|
		/ife (( 'fur' in logicBasedInstruction) or ( 'Fur' in logicBasedInstruction)) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Areolae may be partially obscured by fur or lightly visible beneath it. Use fur-based skin logic for color and contrast."|
			
		:}|
		/elseif (( 'scales' in logicBasedInstruction) or ( 'Scales' in logicBasedInstruction)) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Areolae may appear slightly raised or differently textured than surrounding scales. Coloration and contrast should match scaled coverage."|
			
		:}|
		/elseif (( 'fur' not in logicBasedInstruction) and ( 'Fur' not in logicBasedInstruction) and ( 'scales' not in logicBasedInstruction) and ( 'Scales' not in logicBasedInstruction)) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Use skin-based detail — focus on tone, contour, and texture. Keep all surface description rooted in visible, uncovered human-like anatomy."|
			
		:}|
		
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
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
//-----------|
:}|
/else {:
	/setvar key=appearanceBreasts None|
	/setvar key=appearanceNipples None|
	/addvar key=dataBaseNames appearanceBreasts|
	/addvar key=dataBaseNames appearanceNipples|
:}|

//**Pussy**|
/ife ((gender == 'Female') or (futanari == 'Yes')) {:
	//Pussy Depth|
	/var key=do No|
	/var key=variableName "pussyDepth"|
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
		/setvar key=genSettings index=wi_book_key "Pussy Depth"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
	
		/setvar key=genSettings index=buttonPrompt What is the depth of {{getvar::firstName}}'s pussy? (From the entrence to the entrance of the cervix)|
		
		//[[Generate with Prompt]]|
		
		/:"CMC Logic.GenerateWithPrompt"|
		/ife (unitType == 'Metric') {:
			/setvar key={{var::variableName}} {{getvar::output}} cm|
		:}|
		/else {:
			/setvar key={{var::variableName}} {{getvar::output}} inch|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookAppearance index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookAppearance index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookAppearance index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookAppearance index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	
	//Pussy Accomendation|
	/var key=do No|
	/var key=variableName "pussyStretch"|
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
		/setvar key=genSettings index=wi_book_key "Pussy Stretch"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
	
		/setvar key=genSettings index=buttonPrompt What is the stretch of {{getvar::firstName}}'s pussy? (How mutch can the pussy Stretch before it starts to hurt)|
		
		//[[Generate with Prompt]]|
		
		/:"CMC Logic.GenerateWithPrompt"|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookAppearance index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookAppearance index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookAppearance index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookAppearance index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	
	//Pussy Apperance|
	/var key=do No|
	/var key=variableName "appearancePussy"|
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
		/ife ((clitVisibility == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Clit Visibility"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is {{getvar::firstName}}'s clit's visibility?|
			/setvar key=clitVisibility {{pipe}}|
			/ife (clitVisibility == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((labiaMajoraFullness == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Majora Fullness"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} How would you describe the natural shape or fullness of {{getvar::firstName}}’s labia majora?|
			/setvar key=labiaMajoraFullness {{pipe}}|
			/ife (labiaMajoraFullness == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((labiaMajoraOutsideColor == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Majora Outside Color"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the color of {{getvar::firstName}}'s Labia Majora's Outside?|
			/setvar key=labiaMajoraOutsideColor {{pipe}}|
			/ife (labiaMajoraOutsideColor == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		    /elseif (labiaMajoraOutsideColor == 'Own Color') {:
			    /input Type the color your want {{getvar::firstName}}'s Labia Majora's Outside Color to be.|
			    /setvar key=labiaMajoraOutsideColor {{pipe}}|
				/ife (labiaMajoraOutsideColor == '') {:
			        /echo Aborting |
			        /abort
			    :}|
		    :}|
		:}|
		
		/ife ((labiaMajoraInsideColor == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Majora Inside Color"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the color of {{getvar::firstName}}'s Labia Majora's Inside?|
			/setvar key=labiaMajoraInsideColor {{pipe}}|
			/ife (labiaMajoraInsideColor == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		    /elseif (labiaMajoraInsideColor == 'Own Color') {:
			    /input Type the color your want {{getvar::firstName}}'s Labia Majora's Inside Color to be.|
			    /setvar key=labiaMajoraInsideColor {{pipe}}|
				/ife (labiaMajoraInsideColor == '') {:
			        /echo Aborting |
			        /abort
			    :}|
		    :}|
		:}|
		
		/ife ((labiaMinoraVisibility == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Minora Visibility"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} Is {{getvar::firstName}}'s Labia Minora Visible?|
			/setvar key=labiaMinoraVisibility {{pipe}}|
			/ife (labiaMinoraVisibility == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/ife ((labiaMinoraOutsideColor == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Minora Outside Color"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the color of {{getvar::firstName}}'s Labia Minora's Outside?|
			/setvar key=labiaMinoraOutsideColor {{pipe}}|
			/ife (labiaMinoraOutsideColor == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		    /elseif (labiaMinoraOutsideColor == 'Own Color') {:
			    /input Type the color your want {{getvar::firstName}}'s Labia Minora's Outside Color to be.|
			    /setvar key=labiaMinoraOutsideColor {{pipe}}|
				/ife (labiaMinoraOutsideColor == '') {:
			        /echo Aborting |
			        /abort
			    :}|
		    :}|
		:}|
		
		/ife ((labiaMinoraInsideColor == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Labia Minora Inside Color"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the color of {{getvar::firstName}}'s Labia Minora Inside?|
			/setvar key=labiaMinoraInsideColor {{pipe}}|
			/ife (labiaMinoraInsideColor == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		    /elseif (labiaMinoraInsideColor == 'Own Color') {:
			    /input Type the color your want {{getvar::firstName}}'s Labia Minora's Inside Color to be.|
			    /setvar key=labiaMinoraInsideColor {{pipe}}|
				/ife (labiaMinoraInsideColor == '') {:
			        /echo Aborting |
			        /abort
			    :}|
		    :}|
		:}|
		
		/ife ((externalVulvaState == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "External Vulva State"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} How would you describe the natural openness of {{getvar::firstName}}’s labia majora?|
			/setvar key=externalVulvaState {{pipe}}|
			/ife (externalVulvaState == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/ife ((pussypubicHair == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Pussy Pubic Hair Style"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the status of {{getvar::firstName}}'s pubic hair?|
			/setvar key=pussypubicHair {{pipe}}|
			/ife (pussypubicHair == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/setvar key=genSettings {}|
		/setvar key=genSettings index=wi_book_key "Appearance Pussy"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=genSettings index=useAnatomy Yes|
		/setvar key=extra []|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/ife (appearanceFeatures != 'None') {:
			/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/addvar key=extra "- Female  Genital Type: {{getvar::privatesFemale}}"|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Animal Base: {{getvar::animalBase}}"|
		/ife (clitVisibility != 'Skip') {:
			/addvar key=extra "- Clit Visibility: {{getvar::clitVisibility}}"|
		:}|
		/ife (labiaMajoraFullness != 'Skip') {:
			/addvar key=extra "- Labia Majora Fullness: {{getvar::labiaMajoraFullness}}"|
		:}|
		/ife (labiaMajoraOutsideColor != 'Skip') {:
			/addvar key=extra "- Labia Majora's Outside Color: {{getvar::labiaMajoraOutsideColor}}"|
		:}|
		/ife (labiaMajoraInsideColor != 'Skip') {:
			/addvar key=extra "- Labia Majora's Inside Color: {{getvar::labiaMajoraInsideColor}}"|
		:}|
		/ife (labiaMinoraVisibility != 'Skip') {:
			/addvar key=extra "- Labia Minora Visibility: {{getvar::labiaMinoraVisibility}}"|
		:}|
		/ife (labiaMinoraOutsideColor != 'Skip') {:
			/addvar key=extra "- Labia Minora's Outside Color: {{getvar::labiaMinoraOutsideColor}}"|
		:}|
		/ife (labiaMinoraInsideColor != 'Skip') {:
			/addvar key=extra "- Labia Minora's Inside Color: {{getvar::labiaMinoraInsideColor}}"|
		:}|
		/ife (externalVulvaState != 'Skip') {:
			/addvar key=extra "- External Vulva State: {{getvar::externalVulvaState}}"|
		:}|
		/ife (pussypubicHair != 'Skip') {:
			/addvar key=extra "- Pussy Pubic Hair Style: {{getvar::pussypubicHair}}"|
		:}|
		/ife (futanari == 'Yes') {:
			/addvar key=extra "Important: {{getvar::firstName}} is a futanari, so {{getvar::subjPronoun}} has both a pussy and a cock."|
		:}|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (futanari == 'Yes') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Only describe the vulva — do not mention or reference the cock, even by proximity."|
			
		:}|
		/ife (privatesFemale == 'Mammal') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The vulva may include outer and inner lips, a visible slit, or light pubic hair or fur. Use anatomical terms like “folds,” “cleft,” or “labia” as appropriate."|
			
		:}|
		/elseif (privatesFemale == 'Reptile') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The slit may appear smooth, scaled, or slightly ridged. Do not describe fur, hair, or soft fleshy folds. Placement may be flush with surrounding scale plates."|
			
		:}|
		/elseif (privatesFemale == 'Bird') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The vulva may be recessed or hidden beneath feathers. If the species uses a cloacal structure, describe it as smooth and integrated near the tail base. Avoid mammalian terminology."|
			
		:}|
		/elseif (privatesFemale == 'Fish') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Use phrases like “smooth slit,” “recessed fold,” or “ventral placement.” Avoid fur or lip-based descriptions. The texture should reflect aquatic biology."|
			
		:}|
		/elseif (privatesFemale == 'Amphibian') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The vulva may be soft, moist-looking, and smooth-skinned. Avoid terms like fur, hair, or lips. Structure should appear simple and subtle."|
			
		:}|
		/elseif (privatesFemale == 'Invertebrate') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Structure may appear segmented, chitinous, or soft-bodied. Use terms like “opening,” “ventral slit,” or “intersegmental ridge.” Avoid all mammalian descriptors."|
			
		:}|
		/elseif (privatesFemale == 'Cephalopod') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The vulva may appear smooth, ringed, or recessed between soft folds or tentacle bases. Avoid describing labia or lips unless the species is hybridized."|
			
		:}|
		/elseif (privatesFemale == 'Synthetic') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- If the vulva is synthetic or sculpted, describe it as smooth, artificial, or bio-mimetic. Do not reference organic function, fluids, or reproductive cues."|
			
		:}|
		/elseif (privatesFemale == 'Fantasy') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Vulva structure may include subtle magical, exotic, or hybrid traits — but keep all language anatomical and grounded. Avoid metaphor, emotion, or subjective tone."|
			
		:}|
		/elseif (privatesFemale == 'Alien') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Vulva may appear asymmetrical, oddly colored, or biologically unique. Keep tone anatomical — do not invent behavior or function unless implied by the context."|
			
		:}|
		/elseif ( (privatesFemale == 'Feline') or (privatesFemale == 'Canine') or (privatesFemale == 'Ursine') or (privatesFemale == 'Leporidae') ) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- You may describe soft fur or pubic hair around the vulva if the character’s surface features indicate fur. Do not use fur-related descriptors if none are present."|
			
		:}|
		/elseif ( (privatesFemale == 'Draconic') or (privatesFemale == 'Lacertilian') or (privatesFemale == 'Serpentine') or (privatesFemale == 'Crocodilian') ) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The vulva may be partially hidden by or integrated into scale patterns. Avoid soft or fleshy mammalian terms unless hybridized."|
			
		:}|
		/elseif ( (privatesFemale == 'Passerine') or (privatesFemale == 'Raptor') or (privatesFemale == 'Ratite') or (privatesFemale == 'Aviary') ) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Vulva may be positioned beneath feathers or integrated into a cloacal structure. Keep surface detail minimal and avoid fur, lips, or fleshy folds."|
			
		:}|
		
		
		
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
		/flushvar logicBasedInstruction|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
	:}|
	//-----------|
:}|
/else {:
	/setvar key={{var::variableName}} None|
	/addvar key=dataBaseNames {{var::variableName}}|
:}|


//**Cock**|
/var key=do No|
/var key=variableName "appearanceCock"|
/ife ((gender == 'Male') or (futanari == 'Yes' )) {:
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
	
		/ife ((cockSize == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Cock Size"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What size is {{getvar::firstName}}'s Cock?|
			/setvar key=cockSize {{pipe}}|
			/ife (cockSize == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/ife ((testicularPosition == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Testicular Position"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} Where is {{getvar::firstName}}'s testicles positioned?|
			/setvar key=testicularPosition {{pipe}}|
			/ife (testicularPosition == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/ife ((cockpubicHair == '') or (skip == 'Update')) {:
			/findentry field=comment file="CMC Variables" "Cock Pubic Hair Style"|
			/let key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
			/let key=list {{pipe}}|
			/split find="---" {{var::list}}|
			/var key=list {{pipe}}|
			/buttons labels={{var::list}} What is the status of {{getvar::firstName}}'s pubic hair?|
			/setvar key=cockpubicHair {{pipe}}|
			/ife (cockpubicHair == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		/setvar key=genSettings {}|
		/setvar key=genSettings index=wi_book_key "Appearance Cock"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=genSettings index=useAnatomy Yes|
		/setvar key=extra []|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/ife (appearanceFeatures != 'None') {:
			/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/ife (futanari == 'Yes') {:
			/addvar key=extra "- Pussy Appearance: {{getvar::appearancePussy}}"|
		:}|
		/addvar key=extra "- Male Genital Type: {{getvar::privatesMale}}"|
		/ife (cockSize != 'Skip') {:
			/addvar key=extra "- Cock Size: {{getvar::cockSize}}"|
		:}|
		/ife (testicularPosition != 'Skip') {:
			/addvar key=extra "- Testicular Position: {{getvar::testicularPosition}}"|
		:}|
		/ife (cockpubicHair != 'Skip') {:
			/addvar key=extra "- Cock Pubic Hair Style: {{getvar::cockpubicHair}}"|
		:}|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Animal Base: {{getvar::animalBase}}"|
		/ife (futanari == 'Yes') {:
			/addvar key=extra "Important: {{getvar::firstName}} is a futanari, so she has both a pussy and a cock."|
		:}|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		/ife (futanari == 'Yes') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Rule"|
			
		:}|
		/ife (privatesMale == 'Mammal') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The shaft may include a foreskin or remain uncovered. Use standard anatomical terms for glans, shaft, and scrotum. Testicles may be furred or bare depending on features."|
			
		:}|
		/elseif (privatesMale == 'Canine') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The shaft may emerge from a sheath and taper to a point. Include a prominent knot at the base if erect. The scrotum may be fur-covered and sit close to the base."|
			
		:}|
		/elseif (privatesMale == 'Feline') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The shaft may feature fine ridges or barbs. When erect, describe a tapered form with slight texture variation. The testicles may be small and soft-furred."|
			
		:}|
		/elseif (privatesMale == 'Equine') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The penis is long, thick, and partially sheathed when flaccid. When erect, it may feature a flared head and medial ring. The testicles are large, pendulous, and smooth-skinned."|
			
		:}|
		/elseif ((privatesMale == 'Reptile') or (privatesMale == 'Draconic')) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The penis may be scaled, muscular, or ridged. It can be tucked into a slit or retractable area. When erect, emphasize texture, internal pressure, or unusual shape without exaggeration."|
			
		:}|
		/elseif ((privatesMale == 'Insectoid') or (privatesMale == 'Arachnid') or (privatesMale == 'Cephalopod')) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The shaft may be segmented, chitinous, or soft-bodied. Avoid human terminology. Use terms like “appendage,” “ventral organ,” or “spiral sheath” depending on structure."|
			
		:}|
		/elseif (privatesMale == 'Synthetic') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The shaft may be artificial, sculpted, or bioengineered. Describe design features such as texture, joint seams, or material finish. Avoid organic function or reproductive cues."|
			
		:}|
		/elseif (privatesMale == 'Alien') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- The penis may be asymmetrical, multi-textured, or unusually shaped. Describe with anatomical clarity, not metaphor. You may include multiple elements if the form justifies it."|
			
		:}|
		/elseif (privatesMale == 'Fantasy') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- If the anatomy blends known and exotic traits, describe it with neutral detail. Do not invent abilities or hybrid function — structure only."|
			
		:}|
		
		/ife (('fur' in appearanceFeatures) or ('Fur' in appearanceFeatures)) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- If the character has fur, you may describe fur around the sheath or scrotum. Keep texture consistent with body coverage."|
			
		:}|
		/elseif (('scales' in appearanceFeatures) or ('Scales' in appearanceFeatures)) {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- If the character has scales, the shaft may appear ridged or recessed, with the scrotum partially armored or integrated into surrounding body texture."|
			
		:}|
		
		
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
		/flushvar logicBasedInstruction|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
	:}|
	//-----------|
:}|
/else {:
	/setvar key={{var::variableName}} None|
	/addvar key=dataBaseNames {{var::variableName}}|
:}|

//**Privates Sync**|
/var key=do No|
/var key=variableName "appearanceGenitals"|
/ife (futanari == 'Yes') {:
	
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
		/setvar key=genSettings index=wi_book_key "Appearance Genitals Sync"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/ife (appearanceFeatures != 'None') {:
			/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/addvar key=extra "- Pussy Appearance: {{getvar::appearancePussy}}"|
		/addvar key=extra "- Female Genital Type:: {{getvar::privatesFemale}}"|
		/addvar key=extra "- Cock Appearance: {{getvar::appearanceCock}}"|
		/addvar key=extra "- Male Genital Type:: {{getvar::privatesMale}}"|
		
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
		/flushvar logicBasedInstruction|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
	:}|
	//-----------|
:}|
/else {:
	/setvar key={{var::variableName}} None|
	/addvar key=dataBaseNames {{var::variableName}}|
:}|


//**Anus**|
/var key=do No|
/var key=variableName "appearanceAnus"|
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
	/setvar key=genSettings index=wi_book_key "Appearance Anus"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=genSettings index=useAnatomy Yes|
	/setvar key=extra []|
	/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
	/ife (appearanceFeatures != 'None') {:
		/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
	:}|
	/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	
	/setvar key=logicBasedInstruction {{noop}}|
	/ife (futanari == 'Yes') {:
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/setvar key=logicBasedInstruction "- {{getvar::firstName}} is a futanari. The anus should be described neutrally and anatomically, with no reference to the cock or pussy."|
	:}|
	/ife ('Tail' in appearanceFeatures) {:
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/setvar key=logicBasedInstruction "- {{getvar::firstName}} has a tail. Describe the anus in relation to the tail's base if visible."|
		
	:}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=outputIsList {{pipe}}|
	
	
	/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/setvar key={{var::variableName}} {{getvar::output}}|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
	/flushvar logicBasedInstruction|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//-----------|

/setvar key=parsedApperance {{noop}}|
/addvar key=parsedApperance ### Appearance|
/ife (length != 'None') {:
	/addvar key=parsedApperance "{{newline}} - Length: {{getvar::length}}"|
:}|
/ife (height != 'None') {:
	/addvar key=parsedApperance "{{newline}} - Height: {{getvar::height}}"|
:}|
/ife (appearanceHair != 'None') {:
	/addvar key=parsedApperance "{{newline}} - Hair: {{getvar::appearanceHair}}"|
:}|
/ife (appearanceEyes != 'None') {:
	/addvar key=parsedApperance "{{newline}} - Eyes: {{getvar::appearanceEyes}}"|
:}|
/ife (appearanceFace != 'None') {:
	/addvar key=parsedApperance "{{newline}} - Face: {{getvar::appearanceFace}}"|
:}|
/addvar key=parsedApperance "{{newline}}### Body{{newline}}- Body:"|
/ife (appearanceBody != 'None') {:
	/addvar key=parsedApperance "{{newline}}  - Body: {{getvar::appearanceBody}}"|
:}|
/ife (appearanceBreasts != 'None') {:
	/addvar key=parsedApperance "{{newline}}  - Breasts: {{getvar::appearanceBreasts}}"|
:}|
/ife (appearanceNipples != 'None') {:
	/addvar key=parsedApperance "{{newline}}  - Nipples: {{getvar::appearanceNipples}}"|
:}|
/ife (appearanceGenitals != 'None') {:
	/addvar key=parsedApperance "{{newline}}  - Genitals: {{getvar::appearanceGenitals}}"|
:}|
/ife ((appearanceGenitals == 'None') and (appearancePussy != 'None')) {:
	/addvar key=parsedApperance "{{newline}}  - Pussy: {{getvar::appearancePussy}}"|
:}|
/ife ((appearanceGenitals == 'None') and (appearanceCock != 'None')) {:
	/addvar key=parsedApperance "{{newline}}  - Cock: {{getvar::appearanceCock}}"|
:}|
/ife (appearanceAnus != 'None') {:
	/addvar key=parsedApperance "{{newline}}  - Anus: {{getvar::appearanceAnus}}"|
:}|
/addvar key=parsedApperance "{{newline}}"|
/addvar key=dataBaseNames parsedApperance|


//**Appearance Traits**|
/var key=do No|
/var key=variableName "appearanceTraits"|
/buttons labels=["Yes", "No"] Do you want {{getvar::firstName}} to have any Appearance Traits?|
/var selected_btn {{pipe}}|
/ife ( selected_btn == 'Yes') {:
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
		/setvar key=appearanceTraitsDetails {{noop}}|
		/setvar key=appearanceTraitsEffect {{noop}}|
		/setvar key=genSettings index=wi_book_key "Appearance Trait Type"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList Yes|
		/setvar key=genSettings index=useContext Yes|
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
		
		
		/ife ((inputIsList== 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
		/flushvar logicBasedInstruction|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
	:}|
:}|
/else {:
	/setvar key=appearanceTraits None|
	/addvar key=dataBaseNames appearanceTraits|
:}|

/ife ((appearanceTraits != '') and (appearanceTraits != 'None')) {:
	//Descriptions|
	/var key=do No|
	/var key=variableName "appearanceTraitsDetails"|
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
		/setvar key=genSettings index=wi_book_key "Appearance Trait Details"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/foreach {{getvar::appearanceTraits}} {:
				/setvar key=appearanceTrait {{var::item}}|
				/:"CMC Logic.GenerateWithPrompt"|
				/addvar key={{var::variableName}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/flushvar appearanceTrait|
		:}|
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
	
	//|
	/var key=do No|
	/var key=variableName "appearanceTraitsEffect"|
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
		/setvar key=genSettings index=wi_book_key "Appearance Trait Effect"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/foreach {{getvar::appearanceTraits}} {:
				/setvar key=appearanceTrait {{var::item}}|
				/getvar key=appearanceTraitsDetails index={{var::index}}|
				/setvar key=appearanceTraitDetails {{pipe}}|
				/:"CMC Logic.GenerateWithPrompt"|
				/addvar key={{var::variableName}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/flushvar appearanceTrait|
			/flushvar appearanceTraitDetails|
		:}|
		/else {:
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
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
	//-----------|
:}|
/else {:
	/setvar key=appearanceTraitsDetails None|
	/setvar key=appearanceTraitsEffect None|
	/addvar key=dataBaseNames appearanceTraitsDetails|
	/addvar key=dataBaseNames appearanceTraitsEffect|
:}|
/setvar key=parsedAppearanceTraits {{noop}}|
/ife (appearanceTraits != 'None') {:
	/setvar key=parsedAppearanceTraits |
	/foreach {{getvar::appearanceTraits}} {:
		/ife (index > 0) {:
			/addvar key=parsedAppearanceTraits "{{newline}}{{newline}}"|
		:}|
		/getvar key=appearanceTraitsDetails index={{var::index}}|
		/let key=details {{pipe}}|
		/getvar key=appearanceTraitsEffect index={{var::index}}|
		/let key=effect {{pipe}}|
		/addvar key=parsedAppearanceTraits "- Appearance Trait: {{var::item}}
  - Details: {{var::details}}
  - Effect: {{var::effect}}"
	:}|
:}|
/else {:
	/setvar key=parsedAppearanceTraits None|
:}|
/addvar key=dataBaseNames parsedAppearanceTraits|


/ife (makeLoreBook == 'Yes') {:
	/setvar key=appearance_obj {}|
	/setvar key=appearance_obj index=unitType {{getvar::unitType}}|
	
	/ife ((appearanceFeatures != '') and (appearanceFeatures != 'none')) {:
		/setvar key=features_arr []|
		/let key=appearance_obj_features {}|
		/foreach {{getvar::appearanceFeatures}} {:
			/var key=appearance_obj_features index=name {{var::item}}|
			
			/getvar key=appearanceFeaturesTypes index={{var::index}}|
			/var key=appearance_obj_features index=type {{pipe}}|
			
			/getvar key=appearanceFeaturesPlacements index={{var::index}}|
			/var key=appearance_obj_features index=placement {{pipe}}|
			
			/getvar key=appearanceFeaturesDescriptions index={{var::index}}|
			/var key=appearance_obj_features index=description {{pipe}}|
			/addvar key=features_arr {{var::appearance_obj_features}}|
			/var key=appearance_obj_features {}|
		:}|
		/setvar key=appearance_obj index=features {{getvar::features_arr}}|
		/flushvar features_arr|
	:}|
	
	/ife ((lenghtHeight != '') and (lenghtHeight != 'none')) {:
		/let key=lenghtHeight_obj {}|
		/ife ((length != '') and (length != 'none')) {:
			/var key=lenghtHeight_obj index=length {{getvar::length}}|
			
		:}|
		/ife ((height != '') and (height != 'none')) {:
			/var key=lenghtHeight_obj index=height {{getvar::height}}|
			
		:}|
		/setvar key=appearance_obj index=size {{var::lenghtHeight_obj}}|
	:}|
	
	/ife ((coveringType != '') and (coveringType != 'none')) {:
		/let key=covering_obj {}|
		/var key=covering_obj index=coveringType {{getvar::coveringType}}|
		/var key=covering_obj index=coveringFront {{getvar::coveringFront}}|
		/var key=covering_obj index=coveringSidesBack {{getvar::coveringSidesBack}}|
		/var key=covering_obj index=coveringArmsLegs {{getvar::coveringArmsLegs}}|
		
		/setvar key=appearance_obj index=covering {{getvar::covering_obj}}|
	:}|
	
	/setvar key=appearance_obj index=face {{getvar::appearanceFace}}|
	
	/let key=hair_obj {}|
	/var key=hair_obj index=length {{getvar::hairLenght}}|
	/var key=hair_obj index=style {{getvar::hairStyleSelected}}|
	/var key=hair_obj index=description {{getvar::appearanceHair}}|
	/setvar key=appearance_obj index=hair {{var::hair_obj}}|
	
	/setvar key=appearance_obj index=eyes {{getvar::appearanceEyes}}|
	
	/ife (gender == 'Female') {:
		/let key=butt {}|
		/var key=butt index=buttSize {{getvar::buttSize}}|
		/var key=butt index=buttShape {{getvar::buttShape}}|
		/var key=butt index=buttFirmness {{getvar::buttFirmness}}|
		/setvar key=appearance_obj index=butt {{var::butt}}|
		
		/let key=pelvis {}|
		/var key=pelvis index=hipsSize {{getvar::hipsSize}}|
		/var key=pelvis index=thighsSize {{getvar::thighsSize}}|
		/setvar key=appearance_obj index=pelvis {{var::pelvis}}|
		
		/let key=breasts {}|
		/var key=breasts index=breastSize {{getvar::breastSize}}|
		/var key=breasts index=breastShape {{getvar::breastShape}}|
		/var key=breasts index=breastFirmness {{getvar::breastFirmness}}|
		/var key=breasts index=description {{getvar::appearanceBreasts}}|
		
		/let key=nipples {}|
		/var key=nipples index=nippleType {{getvar::nippleType}}|
		/var key=nipples index=nippleProtrusion {{getvar::nippleProtrusion}}|
		/var key=nipples index=areolaSize {{getvar::areolaSize}}|
		/var key=nipples index=areolaShape {{getvar::areolaShape}}|
		/var key=nipples index=description {{getvar::appearanceNipples}}|
		/var key=breasts index=nipples {{var::nipples}}|
		
		/setvar key=appearance_obj index=breasts {{var::breasts}}|
	:}|
	/ife ((gender == 'Female') or (futanari == 'Yes')) {:
		/let key=pussy {}|
		/var key=pussy index=pussyDepth {{getvar::pussyDepth}}|
		/var key=pussy index=pussyStretch {{getvar::pussyStretch}}|
		/var key=pussy index=clitVisibility {{getvar::clitVisibility}}|
		/var key=pussy index=labiaMajoraFullness {{var::labiaMajoraFullness}}|
		/var key=pussy index=labiaMajoraOutsideColor {{getvar::labiaMajoraOutsideColor}}|
		/var key=pussy index=labiaMajoraInsideColor {{getvar::labiaMajoraInsideColor}}|
		/var key=pussy index=labiaMinoraVisibility {{getvar::labiaMinoraVisibility}}|
		/var key=pussy index=labiaMinoraOutsideColor {{getvar::labiaMinoraOutsideColor}}|
		/var key=pussy index=labiaMinoraInsideColor {{getvar::labiaMinoraInsideColor}}|
		/var key=pussy index=externalVulvaState {{getvar::externalVulvaState}}|
		/var key=pussy index=pubicHair {{getvar::pussypubicHair}}|
		/var key=pussy index=description {{getvar::appearancePussy}}|
		
		/setvar key=appearance_obj index=pussy {{var::pussy}}|
	:}|
	/ife ((gender == 'Male') or (futanari == 'Yes')) {:
		/let key=cock {}|
		/var key=cock index=size {{getvar::cockSize}}|
		/var key=cock index=testicularPosition {{getvar::testicularPosition}}|
		/var key=cock index=pubicHair {{getvar::cockpubicHair}}|
		/var key=cock index=description {{getvar::appearanceCock}}|
		
		/setvar key=appearance_obj index=cock {{var::cock}}|
	:}|
	/ife (futanari == 'Yes') {:
		/setvar key=appearance_obj index=combined_description {{getvar::appearanceGenitals}}|
	:}|
	
	/setvar key=appearance_obj index=body_description {{getvar::appearanceBody}}|
	
	/setvar key=appearance_obj index=anus_description {{getvar::appearanceAnus}}|
	
	/ife ((appearanceTraits != 'none') and (appearanceTraits != '')) {:
		/setvar key=traits_obj []
		/let key=trait_obj {}|
		/foreach {{getvar::appearanceTraits}} {:
			/var key=trait_obj index=name {{var::item}}|
			/getvar key=appearanceTraitsDetails index={{var::index}}|
			/var key=trait_obj index=details {{pipe}}|
			/getvar key=appearanceTraitsEffect index={{var::index}}|
			/var key=trait_obj index=effect {{pipe}}|
			
			/addvar key=traits_obj {{var::trait_obj}}|
			/var key=trait_obj {}|
		:}|
		/setvar key=appearance_obj index=traits {{getvar::traits_obj}}|
		/fluschvar traits_obj|
	:}|
:}|


/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Outfit" {{pipe}}|
/forcesave|