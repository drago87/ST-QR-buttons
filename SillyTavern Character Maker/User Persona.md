/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Continue making First Message" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step10|
/setvar key=stepDone No|

/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let selected_btn {{noop}}|
/let key=len {{noop}}|

/var key=do Yes|
/var key=variableName "persona"|
/ife ({{var::variableName}} != '') {:
	/buttons labels=["Yes", "No"] Do you want to remake {{getvar::firstName}}'s First Message|
    /var key=do {{pipe}}|
    /ife (do == '') {:
        /echo Aborting |
        /abort
    :}|
:}|
/ife ( do == 'Yes' ) {:
	/setvar key=genSettings {}|
	/setvar key=genSettings index=wi_book_key "Persona"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting: {{getvar::settingType}}"|
	/addvar key=extra "- Personality: {{getvar::parsedArchetype}}"|
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/addvar key=extra "- Residence: {{getvar::residence}}"|
	/addvar key=extra "- User Role: {{getvar::userRole}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
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

/messages names=off {{getvar::taglineID}}|
/let key=mess {{pipe}}|
/ife (mess == '') {:
	/sendas name={{char}} [{{getvar::persona}}]|
:}|
/else {:
	/message-edit message={{getvar::taglineID}} await=true [{{getvar::persona}}]|
:}|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-update set="CMC Main" label="Character Export" hidden=false title="Exports the character to the DataBase and saves it as a .json file that can be imported to ST as a character (Without a image)"|
/forcesave|