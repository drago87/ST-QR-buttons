/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Outfit" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step4|

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
/setvar key=outfitsDone Yes|
/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let key=selected_btn {{noop}}|



/ife (overallOutfit == '') {:
	/ife ( characterArchetype != 'Animalistic') {:
		/buttons labels=["Yes", "No"] <div>Do you want to set a overall outfit look? This can be changed for each outfit part</div><div>Example1: School Uniform</div><div>Example2: School Swimsuit</div><div>Example3: Office Outfit</div>|
		/var selected_btn {{pipe}}|
	:}|
	/else {:
		/buttons labels=["Yes", "No", "No Outfit"] <div>Do you want to set a overall outfit look? This can be changed for each outfit part</div><div>Example1: School Uniform</div><div>Example2: School Swimsuit</div><div>Example3: Office Outfit</div>|
		/var selected_btn {{pipe}}|
	:}|
	/ife ( selected_btn == ''){:
		/echo Aborting |
		/abort
	:}|
	/elseif ( selected_btn == 'Yes') {:
		/input <div>Example1: School Uniform</div><div>Example2: School Swimsuit</div><div>Example3: Office Outfit</div>|
		/setvar key=overallOutfit {{pipe}}|
		/ife ( overallOutfit == ''){:
			/echo Aborting |
			/abort
		:}|
	:}|
	/else {:
		/setvar key=overallOutfit {{var::selected_btn}}|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=overallOutfit|
			/setvar key=tempLore {{pipe}}|
			/addvar key=tempLore {{getvar::overallOutfit}}|
			/setvar key=loreBookOutfit index=overallOutfit {{getvar::tempLore}}|
		:}|
	:}|
:}|

/ife (overallOutfit != 'No Outfit') {:
	//Outfit Head|
	/var key=do No|
	/var key=variableName "outfitHead"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Head"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 20|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
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
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Head Description|
	/ife (outfitHead != 'None') {:
		/var key=do No|
		/var key=variableName "outfitHeadDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Head Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/addvar key=extra "- Hair: {{getvar::appearanceHair}}"|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			
			/setvar key=logicBasedInstruction {{noop}}|
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If {{getvar::parsedSpecies}} includes visible head features (ears, horns, fins, wings, etc), consider how they affect fit or placement of the headwear."|
			:}|
			/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If any of the Features includes scars, prosthetics, cybernetics, piercings, or other physical modifications, consider how they visually contrast with or influence the item’s appearance or placement."|
			:}|
			
			/getvar key=genSettings index=inputIsList|
			/let key=inputIsList {{pipe}}|
			/getvar key=genSettings index=inputIsList|
			/let key=outputIsList {{pipe}}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitHeadDescription None|
		/addvar key=dataBaseNames outfitHeadDescription|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitHeadDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitHeadDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitHeadDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Accessories|
	/var key=do No|
	/var key=variableName "outfitAccessories"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Accessories"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 10|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList Yes|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Accessories Description|
	/ife (outfitAccessories != 'None') {:
		/var key=do No|
		/var key=variableName "outfitAccessoriesDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Accessories Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList Yes|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			
			/setvar key=logicBasedInstruction {{noop}}|
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If {{getvar::parsedSpecies}} includes tails, horns, paws, wings, or other non-human limbs, consider how this affects where or how the accessory is worn."|
			:}|
			/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
				/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes prosthetics, piercings, cybernetics, or scars, consider how the accessory interacts with or highlights these features."|
			:}|
			
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
				/foreach {{getvar::outfitAccessories}} {:
					/setvar key={{var::variableName}}Item {{var::item}}|
					/:"CMC Logic.GenerateWithPrompt"|
					/addvar key={{var::variableName}} {{getvar::output}}|
					/flushvar output|
					/flushvar guidance|
				:}|
				/flushvar {{var::variableName}}Item|
			:}|
			/else {:
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
			:}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitAccessoriesDescription None|
		/addvar key=dataBaseNames outfitAccessoriesDescription|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitAccessoriesDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitAccessoriesDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitAccessoriesDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	/wait {{getvar::wait}}|
	/setvar key=parsedAccessories {{noop}}|
	/ife (outfitAccessoriesDescription == 'None') {:
		/addvar key=parsedAccessories "{{getvar::outfitAccessoriesDescription}}"|
	:}|
	/elseif (outfitAccessoriesDescription is list) {:
		/foreach {{getvar::outfitAccessoriesDescription}} {:
			/ife (index > 0) {:
				/addvar key=parsedAccessories {{newline}}|
			:}|
			/addvar key=parsedAccessories "  - {{var::item}}"|
		:}|
	:}|
	/addvar key=dataBaseNames parsedAccessories|
	
	//Outfit Makeup|
	/var key=do No|
	/var key=variableName "outfitMakeup"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Makeup"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 10|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Makeup Description|
	/ife (outfitMakeup != 'None') {:
		/var key=do No|
		/var key=variableName "outfitMakeupDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Makeup Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList Yes|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			
			/setvar key=logicBasedInstruction {{noop}}|
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, feathers, or non-human markings, ensure the makeup is applied in visible or exposed areas — such as facial skin patches, ridges, horns, or ceremonial markings — and accounts for species texture."|
			:}|
			/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes prosthetics, cybernetics, scars, piercings, or skin conditions, consider how the makeup highlights or contrasts with these features visually."|
			:}|
			
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
				/foreach {{getvar::outfitMakeup}} {:
					/setvar key={{var::variableName}}Item {{var::item}}|
					/:"CMC Logic.GenerateWithPrompt"|
					/addvar key={{var::variableName}} {{getvar::output}}|
					/flushvar output|
					/flushvar guidance|
				:}|
				/flushvar {{var::variableName}}Item|
			:}|
			/else {:
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
			:}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitMakeupDescription None|
		/addvar key=dataBaseNames outfitMakeupDescription|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitMakeupDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitMakeupDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitMakeupDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	/wait {{getvar::wait}}|
	/setvar key=parsedMakeup {{noop}}|
	/ife (outfitMakeupDescription == 'None') {:
		/addvar key=parsedMakeup " {{getvar::outfitMakeupDescription}}"|
	:}|
	/elseif (outfitMakeupDescription is list) {:
		/foreach {{getvar::outfitMakeupDescription}} {:
			/addvar key=parsedMakeup "{{newline}}  - {{var::item}}"|
		:}|
	:}|
	/addvar key=dataBaseNames parsedMakeup|
	
	//Outfit Neck|
	/var key=do No|
	/var key=variableName "outfitNeck"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Neck"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 10|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Neck Description|
	/ife (outfitNeck != 'None') {:
		/var key=do No|
		/var key=variableName "outfitNeckDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Neck Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			
			/setvar key=logicBasedInstruction {{noop}}|
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, manes, ruffs, or neck-based traits (e.g., gills, fins, feathers), describe how the neckwear fits, wraps around, or contrasts with those features."|
			:}|
			/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes scars, prosthetics, piercings, or visible augmentations around the neck or upper torso, reflect how the neckwear interacts with or complements these features."|
			:}|
			
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
			
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitNeckDescription None|
		/addvar key=dataBaseNames outfitNeckDescription|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitNeckDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitNeckDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitNeckDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	/ife (mainwearType == '') {:
		/buttons labels=["One-Piece", "Two-Piece"] Do you want to have a One-Piece outfit or a Two-Piece outfit?|
		/var key=selected_btn {{pipe}}|
		/ife (selected_btn == '') {:
			/echo Aborting |
			/abort
		:}|
		/setvar key=mainwearType {{var::selected_btn}}|
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=mainwearType|
			/setvar key=tempLore {{pipe}}|
			/addvar key=tempLore {{getvar::mainwearType}}|
			/setvar key=loreBookOutfit index=mainwearType {{getvar::tempLore}}|
		:}|
	:}|
	
	/ife ( mainwearType == 'One-Piece') {:
		//Outfit One-Piece|
		/var key=do No|
		/var key=variableName "outfitMainwear"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Mainwear"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 8|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				
				/getvar key=loreBookOutfit index=outfitTop|
				/setvar key=tempLore {{pipe}}|
				/getvar key=outfitTop|
				/addvar key=tempLore {{noop}}|
				/setvar key=loreBookOutfit index=outfitTop {{getvar::tempLore}}|
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
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
		
		//Outfit One-Piece Description|
		/ife (outfitMainwear != 'None') {:
			/var key=do No|
			/var key=variableName "outfitMainwearDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Mainwear Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
				/ife ( appearanceBreasts != 'None') {:
					/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes wings, tail bases, dorsal fins, fur crests, or unusual body shapes, describe how the garment is shaped or opened to accommodate those features."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical gear, prosthetics, or cybernetics on the torso or pelvis, describe how the garment adjusts to fit, support, or conceal them."|
				:}|
				
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
					
					/getvar key=loreBookOutfit index=outfitTopDescription|
					/setvar key=tempLore {{pipe}}|
					/addvar key=tempLore {{noop}}|
					/setvar key=loreBookOutfit index=outfitTopDescription {{getvar::tempLore}}|
				:}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
				/flushvar guidance|
				/flushvar genOrder|
				/flushvar genContent|
				/flushvar genSettings|
			:}|
			/else {:
				/addvar key=dataBaseNames {{var::variableName}}|
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				:}|
			:}|
		:}|
		/else {:
			/setvar key=outfitMainwearDescription None|
			/addvar key=dataBaseNames outfitMainwearDescription|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index=outfitMainwearDescription|
				/setvar key=tempLore {{pipe}}|
				/getvar key=outfitMainwearDescription|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index=outfitMainwearDescription {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
	:}|
	/else {:
		/setvar key=outfitMainwear Skip|
		/addvar key=dataBaseNames outfitMainwear|
		/setvar key=outfitMainwearDescription Skip|
		/addvar key=dataBaseNames outfitMainwearDescription|
	:}|
	
	/ife ( mainwearType == 'Two-Piece') {:
		//Outfit Top|
		/var key=do No|
		/var key=variableName "outfitTop"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Top"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				
				/getvar key=loreBookOutfit index=outfitMainwear|
				/setvar key=tempLore {{pipe}}|
				/addvar key=tempLore {{noop}}|
				/setvar key=loreBookOutfit index=outfitMainwear {{getvar::tempLore}}|
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
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
		
		//Outfit Top Description|
		/ife (outfitTop != 'None') {:
			/var key=do No|
			/var key=variableName "outfitTopDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Top Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
				/ife ( appearanceBreasts != 'None') {:
					/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
	
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes wings, tail bases, dorsal fins, fur crests, or unusual body shapes, describe how the top is shaped or adapted to fit those features comfortably or functionally."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical supports, prosthetics, or cybernetics on the torso or arms, describe how the top accommodates or interacts with them."|
				:}|
				
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
					
					/getvar key=loreBookOutfit index=outfitMainwearDescription|
					/setvar key=tempLore {{pipe}}|
					/addvar key=tempLore {{noop}}|
					/setvar key=loreBookOutfit index=outfitMainwearDescription {{getvar::tempLore}}|
				:}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
				/flushvar guidance|
				/flushvar genOrder|
				/flushvar genContent|
				/flushvar genSettings|
			:}|
			/else {:
				/addvar key=dataBaseNames {{var::variableName}}|
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				:}|
			:}|
		:}|
		/else {:
			/setvar key=outfitTopDescription None|
			/addvar key=dataBaseNames outfitTopDescription|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index=outfitTopDescription|
				/setvar key=tempLore {{pipe}}|
				/getvar key=outfitTopDescription|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index=outfitTopDescription {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
		
		//Outfit Bottom|
		/var key=do No|
		/var key=variableName "outfitBottom"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Bottom"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
		//--------|
		
		//Outfit Bottom Description|
		/ife (outfitTop != 'None') {:
			/var key=do No|
			/var key=variableName "outfitBottomDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Bottom Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
	
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes tails, fur, multiple legs, or digitigrade limbs, describe how {{getvar::outfitBottom}} adapts to the species-specific lower anatomy."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes braces, prosthetics, or assistive gear on the lower body, describe how {{getvar::outfitBottom}} fits or adjusts to accommodate them."|
				:}|
				
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				:}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
				/flushvar guidance|
				/flushvar genOrder|
				/flushvar genContent|
				/flushvar genSettings|
			:}|
			/else {:
				/addvar key=dataBaseNames {{var::variableName}}|
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
		/else {:
			/setvar key=outfitBottomDescription None|
			/addvar key=dataBaseNames outfitBottomDescription|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index=outfitBottomDescription|
				/setvar key=tempLore {{pipe}}|
				/getvar key=outfitBottomDescription|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index=outfitBottomDescription {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
	:}|
	/else {:
		/setvar key=outfitTop Skip|
		/addvar key=dataBaseNames outfitTop|
		/setvar key=outfitTopDescription Skip|
		/addvar key=dataBaseNames outfitTopDescription|
		/setvar key=outfitBottom Skip|
		/addvar key=dataBaseNames outfitBottom|
		/setvar key=outfitBottomDescription Skip|
		/addvar key=dataBaseNames outfitBottomDescription|
			
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitTop|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitTop|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitTop {{getvar::tempLore}}|
			
			/getvar key=loreBookOutfit index=outfitTopDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitTopDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitTopDescription {{getvar::tempLore}}|
			
			/getvar key=loreBookOutfit index=outfitBottom|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitBottom|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitBottom {{getvar::tempLore}}|
			
			/getvar key=loreBookOutfit index=outfitBottomDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitBottomDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitBottomDescription {{getvar::tempLore}}|
		:}|
	:}|
	
	//Outfit Legs|
	/var key=do No|
	/var key=variableName "outfitLegwear"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Legwear"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 10|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/ife (appearanceFeatures != 'None') {:
			/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
		:}|
		/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
		/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
		/addvar key=extra "- Bottom Outfit: {{getvar::outfitBottom}}"|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Legs Description|
	/ife (outfitLegwear != 'None') {:
		/var key=do No|
		/var key=variableName "outfitLegwearDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Legwear Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife (appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
			/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
			/addvar key=extra "- Bottom Outfit: {{getvar::outfitBottom}}"|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			
			/setvar key=logicBasedInstruction {{noop}}|
			
			/ife ((('tail' in appearanceFeatures ) or ('Tail' in appearanceFeatures )) and (('wings' in appearanceFeatures ) or ('Wings' in appearanceFeatures ))) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If both tail and wings are present, the outfit should account for both anatomical openings or contours without compromising garment structure. Prioritize ergonomic design over decorative cutouts."|
				
			:}|
			/elseif (('wings' in appearanceFeatures ) or ('Wings' in appearanceFeatures )) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If the character has wings, the outfit must either leave the upper back open, include structured wing slots, or have tailored cutouts to avoid restricting motion. Do not ignore wing presence in the garment fit."|
				
			:}|
			/elseif (('tail' in appearanceFeatures ) or ('Tail' in appearanceFeatures )) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If the character has a tail, ensure the garment includes a slit, flap, stretch opening, or contour that accommodates tail placement and movement. Placement should appear intentional and integrated."|
				
			:}|
			/elseif ((('tail' not in appearanceFeatures ) and ('Tail' not in appearanceFeatures )) and (('wings' not in appearanceFeatures ) or ('Wings' not in appearanceFeatures ))) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- Do not include any garment openings or anatomical adaptations for tails or wings, as none are present."|
				
			:}|
			
			
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
			
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			
			/addvar key=dataBaseNames {{var::variableName}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitLegwearDescription None|
		/addvar key=dataBaseNames outfitLegwearDescription|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitLegwearDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitLegwearDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitLegwearDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Socks|
		/var key=do No|
		/var key=variableName "outfitSocks"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Socks"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Socks Description|
		/ife (outfitSocks != 'None') {:
			/var key=do No|
			/var key=variableName "outfitSocksDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Socks Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has paws, claws, hooves, or digitigrade limbs, describe how the socks are shaped, opened, or adjusted to fit their structure."|
					
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has prosthetics, braces, or other medical features, describe how the socks provide support or structural compatibility."|
					
				:}|
				
				
				/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitsocksDescription None|
			/addvar key=dataBaseNames outfitsocksDescription|
		:}|
		//--------|
	
	//Outfit Shoes|
	/var key=do No|
	/var key=variableName "outfitShoes"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Shoes"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 10|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Shoes Description|
	/ife (outfitShoes != 'None') {:
		/var key=do No|
		/var key=variableName "outfitShoesDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Shoes Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If the character has paws, claws, hooves, or digitigrade limbs, describe how the shoes are shaped, opened, or adjusted to fit their structure."|
				
			:}|
			/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If the character has prosthetics, braces, or other medical features, describe how the shoes provide support or structural compatibility."|
				
			:}|
			
			
			/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			
			/addvar key=dataBaseNames {{var::variableName}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		/setvar key=outfitShoesDescription None|
		/addvar key=dataBaseNames outfitShoesDescription|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitShoesDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitShoesDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitShoesDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	/ife (appearanceBreasts != 'None') {:
		//Outfit Underwear (Top)|
		/var key=do No|
		/var key=variableName "outfitUnderwearTop"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Underwear Top"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 5|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
		
		//Outfit Underwear (Top) Description|
		/ife (outfitUnderwearTop != 'None') {:
			/var key=do No|
			/var key=variableName "outfitUnderwearTopDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Underwear Top Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
				/ife ( appearanceBreasts != 'None') {:
					/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, extra limbs, or an unusual torso shape, describe how {{getvar::outfitUnderwearTop}} adapts to or works with these features."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical gear, scars, or prosthetics, note how {{getvar::outfitUnderwearTop}} provides comfort or coverage in those areas."|
				:}|
				
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				:}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
				/flushvar guidance|
				/flushvar genOrder|
				/flushvar genContent|
				/flushvar genSettings|
			:}|
			/else {:
				/addvar key=dataBaseNames {{var::variableName}}|
				/ife (makeLoreBook == 'Yes') {:
					/getvar key=loreBookOutfit index={{var::variableName}}|
					/setvar key=tempLore {{pipe}}|
					/getvar key={{var::variableName}}|
					/addvar key=tempLore {{pipe}}|
					/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
				:}|
			:}|
		:}|
		/else {:
			/setvar key=outfitUnderwearTopDescription None|
			/addvar key=dataBaseNames outfitUnderwearTopDescription|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index=outfitUnderwearTopDescription|
				/setvar key=tempLore {{pipe}}|
				/getvar key=outfitUnderwearTopDescription|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index=outfitUnderwearTopDescription {{getvar::tempLore}}|
			:}|
		:}|
		//--------|
	:}|
	/else {:
		/setvar key=outfitUnderwearTop Skip|
		/setvar key=outfitUnderwearTopDescription Skip|
		/addvar key=dataBaseNames outfitUnderwearTop|
		/addvar key=dataBaseNames outfitUnderwearTopDescription|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitUnderwearTop|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitUnderwearTop|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitUnderwearTop {{getvar::tempLore}}|
			
			/getvar key=loreBookOutfit index=outfitUnderwearTopDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitUnderwearTopDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitUnderwearTopDescription {{getvar::tempLore}}|
		:}|
	:}|
	
	//Outfit Underwear (Bottom)|
	/var key=do No|
	/var key=variableName "outfitUnderwearBottom"|
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
		/setvar key=genSettings index=wi_book_key "Outfit Underwear Bottom"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=genAmount 5|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence No|
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
		
		/ife (overallOutfit != 'No') {:
			/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
		:}|
		
		/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookOutfit index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
	
	//Outfit Underwear (Bottom) Description|
	/ife (outfitUnderwearBottom != 'None') {:
		/var key=do No|
		/var key=variableName "outfitUnderwearBottomDescription"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Underwear Bottom Description"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife ( appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
			
			/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes tails, fur, digitigrade legs, extra limbs, or tauric anatomy, mention how {{getvar::outfitUnderwearBottom}} accommodates or wraps around them."|
				
			:}|
			/elseif ((( characterArchetype == 'Human') or ( characterArchetype == 'Android')) and (appearanceFeatures != 'None')) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes scars, braces, or prosthetics around the hips or legs, describe how {{getvar::outfitUnderwearBottom}} adjusts or provides comfort."|
				
			:}|
			/ife (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
				
				/ife ( logicBasedInstruction != '') {:
					/addvar key=logicBasedInstruction {{newline}}|
				:}|
				/addvar key=logicBasedInstruction "- Do not mention tails, fur, digitigrade joints, claws, wings, extra limbs, or species-based adaptations. Describe only human anatomy and fit."|
				
			:}|
			
			
			/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
			
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
			
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar logicBasedInstruction|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
			/ife (makeLoreBook == 'Yes') {:
				/getvar key=loreBookOutfit index={{var::variableName}}|
				/setvar key=tempLore {{pipe}}|
				/getvar key={{var::variableName}}|
				/addvar key=tempLore {{pipe}}|
				/setvar key=loreBookOutfit index={{var::variableName}} {{getvar::tempLore}}|
			:}|
		:}|
	:}|
	/else {:
		
		/addvar key=dataBaseNames outfitUnderwearBottomDescription|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookOutfit index=outfitUnderwearBottomDescription|
			/setvar key=tempLore {{pipe}}|
			/getvar key=outfitUnderwearBottomDescription|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookOutfit index=outfitUnderwearBottomDescription {{getvar::tempLore}}|
		:}|
	:}|
	//--------|
:}|
/else {:
	/setvar key=outfitHeadDescription None|
	/addvar key=dataBaseNames outfitHeadDescription|
	/setvar key=parsedAccessories None|
	/addvar key=dataBaseNames parsedAccessories|
	/setvar key=parsedMakeup None|
	/addvar key=dataBaseNames parsedMakeup|
	/setvar key=outfitNeckDescription None|
	/addvar key=dataBaseNames outfitNeckDescription|
	/setvar key=outfitMainwearDescription Skip|
	/addvar key=dataBaseNames outfitTopDescription|
	/setvar key=outfitTopDescription Skip|
	/addvar key=dataBaseNames outfitTopDescription|
	/setvar key=outfitBottomDescription Skip|
	/addvar key=dataBaseNames outfitBottomDescription|
	/setvar key=outfitLegwearDescription None|
	/addvar key=dataBaseNames outfitLegwearDescription|
	/setvar key=outfitShoesDescription None|
	/addvar key=dataBaseNames outfitShoesDescription|
	/setvar key=outfitUnderwearTopDescription None|
	/addvar key=dataBaseNames outfitUnderwearTopDescription|
	/setvar key=outfitUnderwearBottomDescription None|
	/addvar key=dataBaseNames outfitUnderwearBottomDescription|
:}|

/setvar key=parsedOutfit {{noop}}|
/ife ((outfitMainwearDescription != 'Skip') and (outfitMainwearDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Mainwear: {{getvar::outfitMainwearDescription}}"|
:}|
/ife ((outfitTopDescription != 'Skip') and (outfitTopDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Top: {{getvar::outfitTopDescription}}"|
:}|
/ife ((outfitBottomDescription != 'Skip') and (outfitBottomDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Bottom: {{getvar::outfitBottomDescription}}"|
:}|
/ife ((outfitLegwearDescription != '') and (outfitLegwearDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Legs: {{getvar::outfitLegwearDescription}}"|
:}|
/ife ((outfitShoesDescription != '') and (outfitShoesDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Shoes: {{getvar::outfitShoesDescription}}"|
:}|
/ife ((outfitUnderwearTopDescription != 'Skip') and (outfitUnderwearTopDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Underwear (Top): {{getvar::outfitUnderwearTopDescription}}"|
:}|
/ife ((outfitUnderwearBottomDescription != '') and (outfitUnderwearBottomDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Underwear (Bottom): {{getvar::outfitUnderwearBottomDescription}}"|
:}|
/ife (parsedOutfit != '') {:
	/setvar key=parsedOutfit "###OUTFIT{{newline}}{{getvar::parsedOutfit}}"|
	/setvar key=parsedOutfitTan {{getvar::parsedOutfit}}|
:}|

/ife (((characterArchetype == 'Human') or (characterArchetype == 'Beastkin')) and (genTan == '')) {:
	/buttons labels=["Yes", "No"] Do yo want {{getvar::firstName}} to have some sort of tan?|
	/setvar key=genTan {{pipe}}|
	/ife (genTan == '') {:
        /echo Aborting |
        /abort
    :}|
:}|
/ife ( ( (characterArchetype == 'Human') or (characterArchetype == 'Beastkin') ) and (genTan == 'Yes') ) {:
	/let key=outTan {{noop}}|
	/ife (parsedOutfit == '') {:
		/var key=outTan ["Full Body Tan", "No Tan", "New Outfit"]|
	:}|
	/else {:
		/var key=outTan ["Full Body Tan", "No Tan", "New Outfit", "Current Outfit"]|
	:}|
	/buttons labels={{var::outTan}} Do you want to have a full body tan, no tan, current outfit or select a new outfit that to mold the tan after?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting |
		/abort
	:}|
	/elseif ( selected_btn == 'Full Body Tan') {:
		/setvar key=parsedOutfitTan "Full-body exposure"|
	:}|
	/elseif ( selected_btn == 'No Tan') {:
		/setvar key=parsedOutfitTan "No visible sun exposure"|
	:}|
	/elseif ( selected_btn == 'New Outfit') {:
		/input default="One-Piece Swimsuit" What is the name of the outfit that caused the tan?|
		/setvar key=parsedOutfitTan "Visible tanlines from a {{pipe}}"
	:}|
	/elseif ( selected_btn == 'Current Outfit') {:
		/input default="One-Piece Swimsuit" What is the name of the outfit that caused the tan?|
		/setvar key=parsedOutfitTan "Visible tanlines from a {{getvar::overallOutfit}}"
	:}|
:}|

//Tanlines|
/ife (((characterArchetype == 'Human') or (characterArchetype == 'Beastkin')) and (genTan != 'No')) {:
	/var key=do No|
	/var key=variableName "tanlines"|
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
		/setvar key=genSettings index=wi_book_key "Tanlines"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext No|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
	
		/setvar key=genSettings index=buttonPrompt Is this desctiption of {{getvar::firstName}}'s tanlines good?|
	
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
	/setvar key=tanlines None|
	/addvar key=dataBaseNames tanlines|
:}|
//---------|

/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Mental Traits & Personality" {{pipe}}|
/forcesave|