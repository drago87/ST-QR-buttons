/setvar key=dataBaseNames []|
/flushvar genSettings|

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

/buttons multiple=true labels=["Clothes", "Underwear", "Nude"] What type of image/images do you want to generate a Qwen prompt for?|
/setvar key=imageGen {{pipe}}|
/ife ( imageGen == ''){:
	/echo Aborting |
	/abort
:}|
/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let key=selected_btn {{noop}}|

/ife (real == 'Yes') {:
	/setvar key=characterCanonical "{{getvar::firstName}} from {{getvar::media_type}} {{getvar::mediaName}}"|

:}|

/setvar key=bodyProportions {{noop}}|
/ife ((buttSize != '') or (thighsSize != '') or (hipsSize != '') or (breastSize != '')) {:
	/setvar key=bodyAdd {{noop}}|
	/addvar key=bodyProportions "Body proportions (read as silhouettes only):"|
	/ife (buttSize != '') {:
		/ife (bodyAdd != '') {:
			/addvar key=bodyAdd ", "|
		:}|
		/addvar key=bodyAdd {{getvar::buttSize}}|
	:}|
	/ife (thighsSize != '') {:
		/ife (bodyAdd != '') {:
			/addvar key=bodyAdd ", "|
		:}|
		/addvar key=bodyAdd {{getvar::thighsSize}}|
	:}|
	/ife (hipsSize != '') {:
		/ife (bodyAdd != '') {:
			/addvar key=bodyAdd ", "|
		:}|
		/addvar key=bodyAdd {{getvar::hipsSize}}|
	:}|
	/ife (breastSize != '') {:
		/ife (bodyAdd != '') {:
			/addvar key=bodyAdd ", "|
		:}|
		/addvar key=bodyAdd {{getvar::breastSize}}|
	:}|
	/addvar key=bodyProportions " {{getvar::bodyAdd}}"|
	/flushvar bodyAdd|
:}|

/setvar key=genitalia "Genitalia:"|
/ife ((appearancePussy != 'None') and (appearancePussy != '')) {:
	/addvar key=genitalia {{newline}}- Pussy: {{getvar::appearancePussy}}|
:}|
/ife ((clitVisability != 'None') and (clitVisability != '')) {:
	/addvar key=genitalia {{newline}}- Clit visibility: {{getvar::clitVisability}}|
:}|
/ife ((labiaMinoraVisability != 'None') and (labiaMinoraVisability != '')) {:
	/addvar key=genitalia {{newline}}- Labia minora visibility: {{getvar::labiaMinoraVisability}}|
:}|
/ife ((appearanceCock != 'None') and (appearanceCock != '')) {:
	/addvar key=genitalia {{newline}}- Cock: {{getvar::appearanceCock}}|
:}|
/ife ((appearanceGenitals != 'None') and (appearanceGenitals != '')) {:
	/addvar key=genitalia {{newline}}- Combined genitals (futanari): {{getvar::appearanceGenitals}}|
:}|


/var key=do No|
/var key=variableName "style"|
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
	/setvar key=genSettings index=wi_book "CMC Variables"|
	/setvar key=genSettings index=wi_book_key "Style"|
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

/foreach {{getvar::imageGen}} {:

	/var key=do No|
	/var key=variableName "environment"|
	/ife ({{var::variableName}} == '') {:
		/var key=do Yes|
	:}|
	/elseif (skip == 'Update') {:
		/getvar key={{var::variableName}}|
		/buttons labels=["Yes", "No"] Do you want to use the same enviroment? (current enviroment: {{pipe}})|
		/var key=do {{pipe}}|
		/ife (do == '') {:
			/echo Aborting |
			/abort
		:}|
	:}|
	/ife ( do == 'Yes' ) {:
		/setvar key=genSettings {}|
		/setvar key=genSettings index=wi_book "CMC Variables"|
		/setvar key=genSettings index=wi_book_key "Environments"|
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
			/setvar key={{var::variableName}} {{getvar::output}}|
			
		:}|
		/flushvar output|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar it|
		/flushvar genSettings|
	:}|



	/var key=do No|
	/var key=variableName "image{{var::item}}"|
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
		/setvar key=genSettings index=wi_book_key "Image {{var::item}}"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=genAmount 8|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext No|
		/wait {{getvar::wait}}|
		
		
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|

		/flushvar output|
		/flushvar guidance|
		/flushvar genOrder|
		/flushvar genContent|
		/flushvar genSettings|
	:}|
:}|

/findentry field=comment file="CMC Image Gen output" "Character with clothes"|
/let key=wi_uid_clothes {{pipe}}|
/findentry field=comment file="CMC Image Gen output" "Character with underwear"|
/let key=wi_uid_underwear {{pipe}}|
/findentry field=comment file="CMC Image Gen output" "Nude character"|
/let key=wi_uid_nude {{pipe}}|
/setentryfield field=content file="CMC Image Gen output" uid={{var::wi_uid_clothes}} {{getvar::imageClothes}}|
/setentryfield field=content file="CMC Image Gen output" uid={{var::wi_uid_underwear}} {{getvar::imageUnderwear}}|
/setentryfield field=content file="CMC Image Gen output" uid={{var::wi_uid_nude}} {{getvar::imageNude}}|

/flushvar imageClothes|
/flushvar imageUnderwear|
/flushvar imageNude|

/setvar key=stepDone Yes|