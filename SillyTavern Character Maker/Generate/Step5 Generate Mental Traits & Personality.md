/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Mental Traits & Personality" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step5|

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

//Sentient Level|
/ife ((characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon')) {:
	/var key=do No|
	/var key=variableName "sentientLevel"|
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
		/setvar key=genSettings index=wi_book_key "Sentient Level"|
		/setvar key=genSettings index=combineLorebookEntries No|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=useContext No|
		/wait {{getvar::wait}}|
		
		
		/getvar key=genSettings index=wi_book_key|
		/let key=wi_book_key {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=combineLorebookEntries|
		/let key=combineLorebookEntries {{pipe}}|
		
		
		
		/getvar key=genSettings index=wi_book_key|
		/setvar key=it {{pipe}}|
		/setvar key=genSettings index=buttonPrompt "Select how much {{getvar::firstName}} thinks and behaves like an animal versus a person."|
		/:"CMC Logic.GenerateWithSelector"|
		/setvar key={{var::variableName}} {{getvar::output}}|
			
		
		/addvar key=dataBaseNames {{var::variableName}}|
		/flushvar output|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar it|
		/flushvar genSettings|
	:}|
	/else {:
		/addvar key=dataBaseNames {{var::variableName}}|
	:}|
:}|
/else {:
	/setvar key=sentientLevel None|
	/addvar key=dataBaseNames sentientLevel|
:}|

/ife (sentientLevel != 'None') {:
	/setvar key=parsedSentientLevel {{noop}}|
	/split find="{{newline}}" {{getvar::sentientLevel}}|
	/let key=temp {{pipe}}||
	/foreach {{var::temp}} {:
		/ife (index == 0) {:
			/addvar key=parsedSentientLevel "- Animalistic Level: {{var::item}}{{newline}} - Description:"|
		:}|
		/ife (index >= 1) {:
			/addvar key=parsedSentientLevel "{{newline}}  {{var::item}}"|
		:}|
	:}|
	/addvar key=dataBaseNames parsedSentientLevel|
:}|
/else {:
	/setvar key=parsedSentientLevel None|
	/addvar key=dataBaseNames parsedSentientLevel|
:}|
//-----|

//Archetype|
/var key=do No|
/var key=variableName "personalityArchetype"|
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
	/setvar key=genSettings index=wi_book_key "Archetype Base"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=genSettings index=contextKey []|
	/setvar key=extra []|
	/addvar key=extra "- Character Overview: {{getvar::characterOverview}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/setvar key=settingModifier {Modifier}|
	/setvar key=settingArchetype {Archetype}|
	/setvar key=settingAddition {Addition}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	
	
	/ife (outputIsList == 'Yes') {:
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
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
	/flushvar settingModifier|
	/flushvar settingArchetype|
	/flushvar settingAddition|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|


//Archetype Details|
/var key=do No|
/var key=variableName "personalityArchetypeDetails"|
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
	/setvar key=genSettings index=wi_book_key "Archetype Details"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Archetype: {{getvar::personalityArchetype}}"|
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
	
	
	/ife (outputIsList == 'Yes') {:
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
	/flushvar settingModifier|
	/flushvar settingArchetype|
	/flushvar settingAddition|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|


//Reasoning|
/var key=do No|
/var key=variableName "personalityArchetypeReasoning"|
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
	/setvar key=genSettings index=wi_book_key "Archetype Reasoning"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/flushvar extra|
	/setvar key=genSettings index=contextKey []|
	/wait {{getvar::wait}}|
	
	/setvar key=settingModifier {Modifier}|
	/setvar key=settingArchetype {Archetype}|
	/setvar key=settingAddition {Addition}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	
	
	/ife (outputIsList == 'Yes') {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/ife (output != '') {:
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
	/flushvar settingModifier|
	/flushvar settingArchetype|
	/flushvar settingAddition|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|


/ife (personalityMainTrait == '') {:
	/let key=find "Identify Personality Tag: Examples"|
	/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
	/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{pipe}}|
	/let key=example {{pipe}}|
	
	/var key=find "Identify Personality Tag: Task"|
	/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
	/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{pipe}}|
	/let key=task {{pipe}}|
	
	/var key=find "Identify Personality Tag: Instruction"|
	/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
	/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{pipe}}|
	/let key=instruction {{pipe}}|
	
	/genraw "{{var::example}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruction}}"|
	/setvar key=personalityFoundTags {{pipe}}|
	
	/split {{getvar::personalityFoundTags}}|
	/buttons labels={{pipe}} Witch of these are the Main personality trait of {{getvar::firstName}}?|
	/setvar key=personalityMainTrait {{pipe}}|
	/ife (personalityMainTrait == '') {:
	    /echo Aborting |
		/abort
	:}|
:}|
/addvar key=dataBaseNames personalityMainTrait|

/findentry field=comment file="CMC Templates" "Archetype Template"|
/getentryfield field=content file="CMC Templates" {{pipe}}|
/setvar key=parsedArchetype {{pipe}}|
/addvar key=dataBaseNames parsedArchetype|

/ife (( alignmentChoice == '') or ( alignmentChoice == 'Yes')) {:
	/buttons labels=["Yes", "No"] Do you want to select and generate an Alignment and its details?|
	/setvar key=alignmentChoice {{pipe}}|
	/ife ( alignmentChoice == ''){:
		/echo Aborting |
		/abort
	:}|
	/elseif ( selected_btn == 'Yes') {:
		//Alignment|
		/var key=do No|
		/var key=variableName "personalityAlignment"|
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
			/setvar key=genSettings index=wi_book_key "Alignment"|
			/setvar key=genSettings index=combineLorebookEntries No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=useContext No|
			/wait {{getvar::wait}}|
			
			
			/getvar key=genSettings index=wi_book_key|
			/let key=wi_book_key {{pipe}}|
			/getvar key=genSettings index=inputIsList|
			/let key=inputIsList {{pipe}}|
			/getvar key=genSettings index=combineLorebookEntries|
			/let key=combineLorebookEntries {{pipe}}|
			
			
			/ife ( inputIsList == 'Yes') {:
				/setvar key={{var::variableName}} []|
				/ife ( combineLorebookEntries == 'Yes') {:
					/:"CMC Logic.Combine List Lorebooks"
				:}|
				/foreach {{getvar::genOrder}} {:
					/setvar key=it {{var::item}}|
					/getat index={{var::index}} {{var::genOrderContent}} |
					/setvar key=genSettings index=content {{pipe}}|
					/:"CMC Logic.GenerateWithSelector"|
					/ife (output != '') {:
						/addvar key={{var::variableName}} {{getvar::output}}|
					:}|
				:}|
			:}|
			/else {:
				/getvar key=genSettings index=wi_book_key|
				/setvar key=it {{pipe}}|
				/:"CMC Logic.GenerateWithSelector"|
				/ife (output != '') {:
					/setvar key={{var::variableName}} {{getvar::output}}|
				:}|
				
			:}|
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar it|
			/flushvar genSettings|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
		:}|
		//--------|
		
		
		//Alignment Details|
		/var key=do No|
		/var key=variableName "personalityAlignmentDetails"|
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
			/setvar key=genSettings index=wi_book_key "Alignment Details"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
			/addvar key=extra "{{getvar::parsedArchetype}}"|
			/addvar key=extra "- Alignment: {{getvar::alignment}}"|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/flushvar extra|
			/setvar key=genSettings index=contextKey []|
			/wait {{getvar::wait}}|
			
			/setvar key=settingModifier {Modifier}|
			/setvar key=settingArchetype {Archetype}|
			/setvar key=settingAddition {Addition}|
			
			/getvar key=genSettings index=inputIsList|
			/let key=inputIsList {{pipe}}|
			
			
			/ife (outputIsList == 'Yes') {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/ife (output != '') {:
				/setvar key={{var::variableName}} {{getvar::output}}|
			:}|
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
			/flushvar settingModifier|
			/flushvar settingArchetype|
			/flushvar settingAddition|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
		:}|
		//--------|
		
		
		//Alignment Ideals|
		/var key=do No|
		/var key=variableName "personalityAlignmentIdeals"|
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
			/setvar key=genSettings index=wi_book_key "Alignment Ideals"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
			/addvar key=extra "{{getvar::parsedArchetype}}"|
			/addvar key=extra "- Alignment: {{getvar::alignment}}"|
			/addvar key=extra "  ↳ Ideals: {{getvar::alignmentDetails}}"|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/flushvar extra|
			/setvar key=genSettings index=contextKey []|
			/wait {{getvar::wait}}|
			
			/setvar key=settingModifier {Modifier}|
			/setvar key=settingArchetype {Archetype}|
			/setvar key=settingAddition {Addition}|
			
			/getvar key=genSettings index=inputIsList|
			/let key=inputIsList {{pipe}}|
			
			
			/ife (outputIsList == 'Yes') {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			/else {:
				/setvar as=string key={{var::variableName}} {{noop}}|
			:}|
			//[[Generate with Prompt]]|
			/:"CMC Logic.GenerateWithPrompt"|
			/ife (output != '') {:
				/setvar key={{var::variableName}} {{getvar::output}}|
			:}|
			/addvar key=dataBaseNames {{var::variableName}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar genOrder|
			/flushvar genContent|
			/flushvar genSettings|
			/flushvar settingModifier|
			/flushvar settingArchetype|
			/flushvar settingAddition|
		:}|
		/else {:
			/addvar key=dataBaseNames {{var::variableName}}|
		:}|
		//--------|
	:}|
	/else {:
		/setvar key=personalityAlignment Nope|
		/addvar key=dataBaseNames personalityAlignment|
		/setvar key=personalityAlignmentDetails Nope|
		/addvar key=dataBaseNames personalityAlignmentDetails|
		/setvar key=personalityAlignmentIdeals Nope|
		/addvar key=dataBaseNames personalityAlignmentIdeals|
	:}|
:}|
/else {:
	/addvar key=dataBaseNames personalityAlignment|
	/addvar key=dataBaseNames personalityAlignmentDetails|
	/addvar key=dataBaseNames personalityAlignmentIdeals|
:}|

/ife (( personalityAlignment != 'Nope') and ( personalityAlignment != '')) {:
	/findentry field=comment file="CMC Templates" "Alignment Template"|
	/getentryfield field=content file="CMC Templates" {{pipe}}|
	/setvar key=parsedAlignment {{pipe}}|
:}|
/else {:
	/setvar key=parsedAlignment None|
:}|
/addvar key=dataBaseNames parsedAlignment|

//Personality Tags|
/var key=do No|
/var key=variableName "personalityFoundTags"|
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
	/setvar key=genSettings index=wi_book_key "Identify Personality Tag"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext No|
	/setvar key=genSettings index=contextKey []|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	
	
	/ife (outputIsList == 'Yes') {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/ife (output != '') {:
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
:}|


/var key=do No|
/var key=variableName "personalityTags"|
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
	/setvar key=genSettings index=wi_book_key "Personality Tags"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsTaskList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "{{getvar::parsedArchetype}}"|
	/addvar key=extra "- Character Overview: {{getvar::characterOverview}}"|
	/ife (parsedAlignment != 'None') {:
		/addvar key=extra "{{getvar::parsedAlignment}}"|
	:}|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/flushvar extra|
	/setvar key=genSettings index=contextKey []|
	/wait {{getvar::wait}}|
	
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	
	
	/ife (outputIsList == 'Yes') {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/:"CMC Logic.GenerateWithPrompt"|
	/ife (output != '') {:
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
//--------|



//Cognitive Abilities|
/var key=do No|
/var key=variableName "personalityIntelligenceLevel"|
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
	/setvar key=genSettings index=wi_book_key "Intelligence Level"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Character Overview: {{getvar::characterOverview}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/wait {{getvar::wait}}|
	
	
	/getvar key=genSettings index=wi_book_key|
	/let key=wi_book_key {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=combineLorebookEntries|
	/let key=combineLorebookEntries {{pipe}}|
	
	
	/ife ( inputIsList == 'Yes') {:
		/setvar key={{var::variableName}} []|
		/ife ( combineLorebookEntries == 'Yes') {:
			/:"CMC Logic.Combine List Lorebooks"
		:}|
		/foreach {{getvar::genOrder}} {:
			/setvar key=it {{var::item}}|
			/getat index={{var::index}} {{var::genOrderContent}} |
			/setvar key=genSettings index=content {{pipe}}|
			/:"CMC Logic.GenerateWithSelector"|
			/ife (output != '') {:
				/addvar key={{var::variableName}} {{getvar::output}}|
			:}|
		:}|
	:}|
	/else {:
		/getvar key=genSettings index=wi_book_key|
		/setvar key=it {{pipe}}|
		/:"CMC Logic.GenerateWithSelector"|
		/ife (output != '') {:
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
	:}|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar it|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|


/var key=do No|
/var key=variableName "personalitycognitiveAbilities"|
/ife (( 'Average' not in personalityIntelligenceLevel) or (characterArchetype == 'Animalistic') ) {:
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
		/setvar key=genSettings index=wi_book_key "Cognitive Abilities"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
		/addvar key=extra "{{getvar::parsedArchetype}}"|
		/ife (parsedAlignment != 'None') {:
			/addvar key=extra "{{newline}}{{getvar::parsedAlignment}}{{newline}}"|
		:}|
		/addvar key=extra "- Intelligence Level: {{getvar::personalityIntelligenceLevel}}"|
		/addvar key=extra "- Character Overview: {{getvar::characterOverview}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/flushvar extra|
		/setvar key=genSettings index=contextKey []|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		
		
		/ife (outputIsList == 'Yes') {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/ife (output != '') {:
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
:}|
/else {:
	/setvar key={{var::variableName}} None|
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|


//Social Skills and Integration Into Society|
/var key=do No|
/var key=variableName "personalitySocialBehavior"|
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
	/setvar key=genSettings index=wi_book_key "Social Behavior"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|
	/addvar key=extra "- Character Overview: {{getvar::characterOverview}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	
	/wait {{getvar::wait}}|
	
	
	/getvar key=genSettings index=wi_book_key|
	/let key=wi_book_key {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=combineLorebookEntries|
	/let key=combineLorebookEntries {{pipe}}|
	
	
	/ife ( inputIsList == 'Yes') {:
		/setvar key={{var::variableName}} []|
		/ife ( combineLorebookEntries == 'Yes') {:
			/:"CMC Logic.Combine List Lorebooks"
		:}|
		/foreach {{getvar::genOrder}} {:
			/setvar key=it {{var::item}}|
			/getat index={{var::index}} {{var::genOrderContent}} |
			/setvar key=genSettings index=content {{pipe}}|
			/:"CMC Logic.GenerateWithSelector"|
			/ife (output != '') {:
				/addvar key={{var::variableName}} {{getvar::output}}|
			:}|
		:}|
	:}|
	/else {:
		/getvar key=genSettings index=wi_book_key|
		/setvar key=it {{pipe}}|
		/:"CMC Logic.GenerateWithSelector"|
		/ife (output != '') {:
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
	:}|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar it|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|

/var key=do No|
/var key=variableName "personalitySocialSkills"|
/ife (( 'Average' not in personalityIntelligenceLevel) or (characterArchetype == 'Animalistic') ) {:
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
		/setvar key=genSettings index=wi_book_key "Social Profile"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsTaskList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "{{newline}}{{getvar::parsedArchetype}}"|
		/ife (parsedAlignment != 'None') {:
			/addvar key=extra "{{newline}}{{getvar::parsedAlignment}}{{newline}}"|
		:}|
		/addvar key=extra "- Intelligence Level: {{getvar::personalityIntelligenceLevel}}"|
		/ife (personalitycognitiveAbilities != 'None') {:
			/addvar key=extra "- Cognitive Abilities: {{getvar::personalitycognitiveAbilities}}{{newline}}"|
		:}|
		/addvar key=extra "- Social Behavior: {{getvar::personalitySocialBehavior}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/flushvar extra|
		/setvar key=genSettings index=contextKey []|
		/wait {{getvar::wait}}|
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		
		
		/ife (outputIsList == 'Yes') {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/:"CMC Logic.GenerateWithPrompt"|
		/ife (output != '') {:
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
:}|
/else {:
	/setvar key={{var::variableName}} None|
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|

/var key=do No|
/var key=variableName "personalityModestyLevel"|
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
	/setvar key=genSettings index=wi_book_key "Modesty Levels"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=useContext No|
	/wait {{getvar::wait}}|
	
	
	/getvar key=genSettings index=wi_book_key|
	/let key=wi_book_key {{pipe}}|
	/getvar key=genSettings index=inputIsList|
	/let key=inputIsList {{pipe}}|
	/getvar key=genSettings index=combineLorebookEntries|
	/let key=combineLorebookEntries {{pipe}}|
	
	
	/ife ( inputIsList == 'Yes') {:
		/setvar key={{var::variableName}} []|
		/ife ( combineLorebookEntries == 'Yes') {:
			/:"CMC Logic.Combine List Lorebooks"
		:}|
		/foreach {{getvar::genOrder}} {:
			/setvar key=it {{var::item}}|
			/getat index={{var::index}} {{var::genOrderContent}} |
			/setvar key=genSettings index=content {{pipe}}|
			/:"CMC Logic.GenerateWithSelector"|
			/ife (output != '') {:
				/addvar key={{var::variableName}} {{getvar::output}}|
			:}|
		:}|
	:}|
	/else {:
		/getvar key=genSettings index=wi_book_key|
		/setvar key=it {{pipe}}|
		
		/setvar key=genSettings index=buttonPrompt "What level of Modesty does {{getvar::firstName}} have?"|
		
		/:"CMC Logic.GenerateWithSelector"|
		/setvar key={{var::variableName}} {{getvar::output}}|
		
	:}|
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar it|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|

/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Aspirational & Unique Traits" {{pipe}}|
/forcesave|