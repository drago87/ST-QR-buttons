/input Who do you want the name of the author of the character to be?|
/setvar key=author {{pipe}}|
/let key=done Regenerate|
/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "Export Prompt"|
/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{pipe}}|
/let key=prompt {{pipe}}|
/let key=output {{noop}}|
/whilee (done == 'Regenerate') {:
	/echo Generationg Brief Character Overview|
	/genraw {{var::prompt}}|
	/var key=output {{pipe}}|
	/buttons labels=["Yes", "Edit", "Regenerate"] <div>Are you happy with the Character Overview?</div><div>{{var::output}}</div>|
	/var key=done {{pipe}}|
	/ife ( done == ''){:
		/echo Aborting |
		/abort
	:}|
	/elseif (done == 'Edit') {:
		/input default={{var::output}} Edit the Character Overview to your liking.|
		/var key=output {{pipe}}|
	:}|
:}|
/re-replace find="/\n/g" replace="\r\n" {{var::output}}|
/re-replace find="/\"/g" replace="\\\"" {{pipe}}|
/var key=output {{pipe}}|
/re-replace find="/\n/g" replace="\r\n" {{getvar::fullCharacterSheet}}|
/re-replace find="/\"/g" replace="\\\"" {{pipe}}|
/let key=fullCharacterSheetFixed {{pipe}}|
/re-replace find="/\n/g" replace="\r\n" {{getvar::firstMessage}}|
/re-replace find="/\"/g" replace="\\\"" {{pipe}}|
/let key=firstMessageFixed {{pipe}}|
/let key=postHist "{\{original}}
## **RESPONSE RULES:**
You MUST follow the writing instructions below with **exact formatting**:
{{getvar::parsedWritingInstruct}}

Each rule is **mandatory** — do not simplify, guess, or loosely interpret. Match the exact phrasing, punctuation, and tone shown in every line. If a style element is ambiguous, assume it must be followed literally."|
/re-replace find="/\n/g" replace="\r\n" {{var::postHist}}|
/re-replace find="/\"/g" replace="\\\"" {{pipe}}|
/re-replace find="/--FirstName--/g" replace="{/{char}}" {{pipe}}|
/var key=postHist {{pipe}}|
/let key=tag {{noop}}|
/setvar key=tags []|
/addvar key=tags CMC|
/let key=done No|
/let key=input {{noop}}|
/whilee (done != 'Yes') {:
	/input rows=1 <div>Add the tags you want to add to the character card. (One at a time).</div><div>Press cancel or leave the textbox empty to continue.</div>|
	/var key=input {{pipe}}|
	/ife ( input == '') {:
		/var key=done Yes|
	:}|
	/else {:
		/addvar key=tags {{var::input}}|
	:}
:}|
/setvar key=tagString1 {{noop}}|
/addvar key=tagString1 "{{newline}}"|
/setvar key=tagString2 {{noop}}|
/addvar key=tagString2 "{{newline}}"|
/foreach {{getvar::tags}} {:
	/len {{getvar::tags}}|
	/let key=len {{pipe}}|
	//add {{var::len}} -1|
	/addvar key=tagString1 "            \"{{var::item}}\""|
	/addvar key=tagString2 "        \"{{var::item}}\""|
	/ife (index < len-1) {:
		/addvar key=tagString1 ",{{newline}}"|
		/addvar key=tagString2 ",{{newline}}"|
	:}|
	/else {:
		/addvar key=tagString1 "{{newline}}"|
		/addvar key=tagString2 "{{newline}}"|
	:}
:}|

/setvar key=strAltGreet "["|
/len {{getvar::altGreetings}}|
/var len {{pipe}}|
/ife (len != 0) {:
	/let key=altOutput {{noop}}|
	/foreach {{getvar::altGreetings}} {:
		/re-replace find="/\n/g" replace="\r\n" {{var::item}}|
		/re-replace find="/\"/g" replace="\\\"" {{pipe}}|
		/var key=altOutput {{pipe}}|
		/ife (index != 0) {:
			/addvar key= ","|
		:}|
		/addvar key=strAltGreet "{{newline}}            \"{{var::altOutput}}\""|
	:}|
	/addvar key=strAltGreet "{{newline}}        ]"|
:}|
/else {:
	/addvar key=strAltGreet "]"|
:}|

/messages names=off 0|
/setvar key=scenario {{pipe}}|

/findentry field=comment file="CMC Templates" "Character Card Template"|
/getentryfield field=content file="CMC Templates" {{pipe}}|
/re-replace find="/--CharName--/g" replace="{{getvar::firstName}}" {{pipe}}|
/re-replace find="/--Summary--/g" replace="" {{pipe}}|
/re-replace find="/--Scenario--/g" replace="{{getvar::scenario}}" {{pipe}}|
/re-replace find="/--Example_Dial--/g" replace="" {{pipe}}|
/re-replace find="/--Main_Prompt--/g" replace="{{var::postHist}}" {{pipe}}|
/re-replace find="/--Post_Hist--/g" replace="" {{pipe}}|
/re-replace find="/--Alt_Greet1--/g" replace="" {{pipe}}|
/re-replace find="/--Char_Notes--/g" replace="" {{pipe}}|
/re-replace find="/--Group_Greeting1--/g" replace="" {{pipe}}|
/re-replace find="/--Tag1--/g" replace="{{getvar::tagString1}}" {{pipe}}|
/re-replace find="/--Tag2--/g" replace="{{getvar::tagString2}}" {{pipe}}|
/re-replace find="/--Created_By--/g" replace="{{getvar::author}}" {{pipe}}|
/re-replace find="/--Creator_Notes--/g" replace="{{var::output}}" {{pipe}}|

/re-replace find="/--Description--/g" replace="{{var::fullCharacterSheetFixed}}" {{pipe}}|
/re-replace find="/--First_Mess--/g" replace="{{var::firstMessageFixed}}" {{pipe}}|
/re-replace find="/--User--/g" replace="{\{user}}" {{pipe}}|
/db-add source=chat name="{{getvar::firstName}}.json" {{pipe}}|
/db-disable source=chat "{{getvar::firstName}}.json"|

//qr-update set="CMC Main" label="Generate Persona" hidden=false title="Generates a Persona that could be used for this character."|

/popup <div>Download {{getvar::firstName}}.json from the SillyTavern Data Bank (It will open when you press ok) and import it as a character to be able to change the image.</div><div>I would love it if you could upload the character to the discord so that i can see what you have made with the CMC Generator.</div>|
/db|