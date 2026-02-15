/var key=do No|
/var key=variableName "CHANGE_THIS"|
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
	/setvar key=genSettings index=wi_book "CHANGE/REMOVE_THIS"|
	/setvar key=genSettings index=wi_book_key "CHANGE_THIS"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|//Remove if not in use|
	/addvar key=extra ""|//Remove if not in use|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|//Remove if not in use|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|//Remove if not in use|
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

	/setvar key=genSettings index=buttonPrompt CHANGE_THIS_PROMPT|//Remove if not in use|
	//[[Generate with Prompt]]|
	/ife (inputIsList == 'Yes') {:
		/let key=tempOutputList []|
		/foreach {{getvar::CHANGE_REMOVE_THIS}} {:
			/getvar key={{var::variableName}}|
			/len {{pipe}}|
			/let key=len {{pipe}}|
			/ife (len == 0) {:
				/setvar as=array key={{var::variableName}} []|
			:}|
			
			/ife ((index > len) or ((index == 0) and (len == 0))) {:
				/setvar key={{var::variableName}}Item {{var::item}}|
				/setvar key=genSettings index=buttonPrompt "Select the type variant for '{{var::item}}' you want {{getvar::firstName}} to have."|
				/:"CMC Logic.GenerateWithPrompt"|
				/len {{var::tempOutputList}}|
				/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			:}|
			/flushvar output|
			/flushvar guidance|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
		/flushvar {{var::variableName}}Item|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	//Reccomended variable name: loreBook+what is generated. Example: loreBookOutfit|
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=CHANGE_THIS_TO_THE_NAME_OF_THE_VARIABLE_YOU_WANT_TO_SAVE_IT_AS index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=CHANGE_THIS_TO_THE_NAME_OF_THE_VARIABLE_YOU_WANT_TO_SAVE_IT_AS index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=CHANGE_THIS_TO_THE_NAME_OF_THE_VARIABLE_YOU_WANT_TO_SAVE_IT_AS index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=CHANGE_THIS_TO_THE_NAME_OF_THE_VARIABLE_YOU_WANT_TO_SAVE_IT_AS index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|