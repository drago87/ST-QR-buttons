/qr-set-list all|
/let key=qrList {{pipe}}|

/let key=branch {{noop}}|
/ife (beta != 'Yes') {:
	/var key=branch main|
:}|
/else {:
	/var key=branch Fetch-Files|
:}|

/ife ('CMC Main' in qrList) {:
	/buttons labels=["Yes", "No"] want to update CMC Main scripts?|
	/let selected_btn {{pipe}}|
	/ife ( selected_btn == Yes) {:
		/qr-chat-set-off CMC Main|
		/qr-set-create CMC Main|
	:}|
:}|
/else {:
	/qr-set-create CMC Main|
:}|

/ife ('CMC Menu' in qrList) {:
	/qr-list CMC Menu|
	/let key=qrListContent {{pipe}}|
	/ife ('CMC Menu' not in qrListContent) {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/CMC%20Menu.md |
	
		/qr-create set="CMC Menu" label="CMC Menu" {{pipe}}|
	:}|
	/else {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/CMC%20Menu.md |
		/qr-create set="CMC Menu" label="CMC Menu2" {{pipe}}|
		//qr-update set="CMC Menu" label="CMC Menu2" {{pipe}}|
	:}|
:}|
/elseif ('CMC Menu' not in qrList) {:
	/qr-set-create CMC Menu|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/CMC%20Menu.md |
	
	/qr-create set="CMC Menu" label="CMC Menu" {{pipe}}|
:}|
/qr-set-list global|
/let key=globList {{pipe}}|
/ife ('CMC Menu' not in globList) {:
	/qr-set-on CMC Menu|
:}|

//-----|
/qr-list CMC Main|
/let key=qrListContent {{pipe}}|

//New Char|
/ife ('New Character' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/New%20Char.md|
	
	
	/re-replace find="/--TextParse--/g" replace="{{getvar::textParse}}" {{pipe}}|
	/qr-create set="CMC Main" label="New Character" title="Will make a new character and let you set the Gender, type(Human, Anthro etc..)" {{pipe}}|
	/qr-update set="CMC Main" label="New Character" title="Make a character from the beginning."|
:}|
//|-----|
/wait 100|
/qr-list CMC Main|
/let key=temp {{pipe}}|
/getat index=1 {{var::temp}}|
/let key=qrlabel {{pipe}}|
/var key=temp {{var::qrlabel}}|


//Create Character Generation|
/ife ('{{var::temp}}' not in qrList) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/Character%20Generation.md|
	
	
	/re-replace find="/--TextParse--/g" replace="{{getvar::textParse}}" {{pipe}}|
	/qr-create set="CMC Main" label="Character Generation" {{pipe}}|
	/qr-update set="CMC Main" label="Character Generation" title="Continues/Restart the last Generation or Starts the next Generation."|
	/qr-chat-set-on CMC Main|
:}|
//|-----|

/ife ('Redo current Greeting' not in qrList) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/Redo%20current%20Greeting.md |
	/qr-create set="CMC Main" label="Redo current Greeting" {{pipe}}|
	/qr-update set="CMC Main" label="Redo current Greeting" hidden=true title="Redoes the current Greeting"|
:}|

/ife ('Make a Alt. Greeting' not in qrList) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/Make%20a%20Alt.%20Greeting.md |
	/qr-create set="CMC Main" label="Make a Alt. Greeting" {{pipe}}|
	/qr-update set="CMC Main" label="Make a Alt. Greeting" hidden=true title="Makes a new Alternative Greeting"|
:}|

//Export Character|
/ife ('Export Character' not in qrList) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Visible%20QR%20Buttons/Export.md|
	
	
	/re-replace find="/--TextParse--/g" replace="{{getvar::textParse}}" {{pipe}}|
	/qr-create set="CMC Main" label="Character Export" {{pipe}}|
	/qr-update set="CMC Main" label="Character Export" hidden=true title="Exports the character to the DataBase and saves it as a .json file that can be imported to ST"|
:}|
//|-----|


/qr-set-list all|
/var key=qrListContent {{pipe}}|

/ife ('CMC Logic' in qrListContent) {:
	/buttons labels=["Yes", "No"] want to update CMC Logic scripts?|
	/let selected_btn {{pipe}}|
	/ife ( selected_btn == Yes) {:
		/qr-chat-set-off CMC Logic|
		/qr-set-create CMC Logic|
	:}|
:}|
/else {:
	/qr-set-create CMC Logic|
:}|

//Combine Lorebook|
/ife ( 'Combine Lorebook' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Combine%20List%20Lorebooks.md|
	
	/qr-create set="CMC Logic" label="Combine Lorebook" {{pipe}}|
:}|
//-----|


//Create SaveGen|
/ife ( 'SaveGen' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/SaveGen.md|
	/qr-create set="CMC Logic" label="SaveGen" {{pipe}}|
:}|
//-----|

//Create Set Base Context|
/ife ( 'Set Base Context' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Set%20Base%20Context.md|
	/qr-create set="CMC Logic" label="Set Base Context" {{pipe}}|
:}|
//-----|


//Create GenerateWithPrompt|
/ife ( 'GenerateWithPrompt' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Generate%20with%20Prompt.md|
	/qr-create set="CMC Logic" label="GenerateWithPrompt" {{pipe}}|
:}|
//-----|

//Create GenerateWithSelector|
/ife ( 'GenerateWithSelector' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Selector.md|
	
	/qr-create set="CMC Logic" label="GenerateWithSelector" {{pipe}}|
:}|
//-----|

/ife ( 'Get Char info' not in qrListContent) {:
	//Get Char info|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Get%20Character%20information.md|
	
	
	/re-replace find="/--TextParse--/g" replace="{{getvar::textParse}}" {{pipe}}|
	
	
	/qr-create set="CMC Logic" label="Get Char info" {{pipe}}|
	//|-----|
:}|

/ife ( 'Is Real' not in qrListContent) {:
	//Is Real|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Is%20Real.md|
	
	
	/re-replace find="/--TextParse--/g" replace="{{getvar::textParse}}" {{pipe}}|
	
	
	/qr-create set="CMC Logic" label="Is Real" {{pipe}}|
	//|-----|
:}|

/ife ( 'Parse' not in qrListContent) {:
	//Parse|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Parse.md|
	/qr-create set="CMC Logic" label="Parse" {{pipe}}|
	//|-----|
:}|

/ife ( 'Save DataBase' not in qrListContent) {:
	//Save DataBase|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Save%20DataBase.md|
	/qr-create set="CMC Logic" label="Save DataBase" {{pipe}}|
	//|-----|
:}|
/ife ( 'JEDParse' not in qrListContent) {:
	//JEDParse|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/JEDParse.md|
	/qr-create set="CMC Logic" label="JEDParse" {{pipe}}|
	//|-----|
:}|

/ife ( 'Get Basic Type Context' not in qrListContent) {:
	//JEDParse|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Functions/Get%20Basic%20Type%20Context.md|
	/qr-create set="CMC Logic" label="Get Basic Type Context" {{pipe}}|
	//|-----|
:}|
//Generate|
/ife ('CMC Generate' in qrList) {:
	/buttons labels=["Yes", "No"] want to update CMC Generate scripts?|
	/let selected_btn {{pipe}}|
	/ife ( selected_btn == Yes) {:
		/qr-chat-set-off CMC Generate|
		/qr-set-create CMC Generate|
	:}|
:}|
/else {:
	/qr-set-create CMC Generate|
:}|


/ife ( 'Step1 Generate Core Identity' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step1%20Generate%20Core%20Identity.md|
	/qr-create set="CMC Generate" label="Step1 Generate Core Identity" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step2 Generate World-Setting Information' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step2%20Generate%20World%20%26%20Setting%20Information.md|
	/qr-create set="CMC Generate" label="Step2 Generate World-Setting Information" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step3 Generate Appearance-Anatomy' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step3%20Generate%20Appearance%20%26%20Anatomy.md|
	/qr-create set="CMC Generate" label="Step3 Generate Appearance-Anatomy" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step4 Generate Outfit CharSheet' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step4%20Outfit%20CharSheet.md|
	/qr-create set="CMC Generate" label="Step4 Generate Outfit CharSheet" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step4 Generate Outfit Lorebook' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step4%20Outfit%20Lorebook.md|
	/qr-create set="CMC Generate" label="Step4 Generate Outfit Lorebook" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step5 Generate Mental Traits-Personality' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step5%20Generate%20Mental%20Traits%20%26%20Personality.md|
	/qr-create set="CMC Generate" label="Step5 Generate Mental Traits-Personality" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step6 Generate Aspirational-Unique Traits' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step6%20Generate%20Aspirational%20%26%20Unique%20Traits.md|
	/qr-create set="CMC Generate" label="Step6 Generate Aspirational-Unique Traits" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step7 Generate Speech Patterns' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step7%20Speech%20Patterns.md|
	/qr-create set="CMC Generate" label="Step7 Generate Speech Patterns" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step8 Generate External Interaction' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step8%20External%20Interaction.md|
	/qr-create set="CMC Generate" label="Step8 Generate External Interaction" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step9 Generate Sexual Information' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step9%20Sexual%20Information.md|
	/qr-create set="CMC Generate" label="Step9 Generate Sexual Information" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step10 Generate Extras' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step10%20Extras.md|
	/qr-create set="CMC Generate" label="Step10 Generate Extras" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step11 First Message' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step11%20First%20Message.md|
	/qr-create set="CMC Generate" label="Step11 First Message" {{pipe}}|
	//|-----|
:}|

/ife ( 'Step12 Tagline' not in qrListContent) {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Generate/Step12%20Tagline.md|
	/qr-create set="CMC Generate" label="Step12 Tagline" {{pipe}}|
	//|-----|
:}|


/qr-set-list global|
/let qrGlobal {{pipe}}|

/ife ( 'CMC Automate' not in qrGlobal) {:
	/qr-set-create CMC Automate|
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Automation/Auto%20Add%20QR%20to%20Character.md|
	/qr-create set="CMC Automate" label="Auto Add QR to Character" {{pipe}}|
	/qr-set visible=false "CMC Automate"|
	/qr-update hidden=true new=true set="CMC Automate" label="Auto Add QR to Character"|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Automation/Update%20button%20label.md|
	/qr-create set="CMC Automate" label="Update button label" {{pipe}}|
	/qr-update hidden=true load=true new=true set="CMC Automate" label="Update button label"|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Automation/Hide%20First%20Message%20Buttons.md|
	/qr-create set="CMC Automate" label="Hide First Message Buttons" {{pipe}}|
	/qr-update hidden=true load=true new=true bot=true user=true set="CMC Automate" label="Hide First Message Buttons"|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Automation/Rename%20Menu.md|
	/qr-create set="CMC Automate" label="Rename Menu" {{pipe}}|
	/qr-update hidden=true load=true set="CMC Automate" label="Rename Menu"|
	//|-----|
:}|


/qr-chat-set-on visible=true "CMC Main"|

/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Version/Version.md |
/let key=updatedVersion {{pipe}}|

/let key=currentVersion {{noop}}|

/db-list source=character field=name |
/let key=a {{pipe}}|
/ife ("Current Version" in a) {:
	/db-get source=character  "Current Version"|
	/var key=currentVersion {{pipe}}|
:}|
/ife ((currentVersion == '') or (updatedVersion != currentVersion)) {:
	/ife ("Current Version" in a) {:
		/db-update source=character name="Current Version" {{var::updatedVersion}}|
		/db-disable source=character "Current Version"|
	:}|
	/else {:
		/db-add source=character name="Current Version" {{var::updatedVersion}}|
		/db-disable source=character "Current Version"|
	:}|
:}|