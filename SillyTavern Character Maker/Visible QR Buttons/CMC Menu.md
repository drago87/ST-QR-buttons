/buttons labels=["Set Prompt Order", "Use XML tags", "Change Model", "Add New Model", "Update QR Scripts", "Download Model Lorebooks", "Download newest version of Lorebooks"] What do you want to do?|
/let key=selection {{pipe}}|
/let key=reload 'No'|

/let key=branch {{noop}}|
/ife (beta != 'Yes') {:
	/var key=branch main|
:}|
/else {:
	/var key=branch Fetch-Files|
:}|

/ife (selection == 'Change Model') {:
	/findentry field=comment file="CMC Variables" "Models"|
	/let key=wi_uid {{pipe}}|
	/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
	/let key=selectModels {{pipe}}|
	/buttons multiple=true labels={{var::selectModels}} Select the model prompts you want use for generation.|
	/setglobalvar key=model {{pipe}}|
	/ife (model == '') {:
		/echo Aborting |
		/abort
	:}|
:}|

/ife (selection ==  'Set Prompt Order') {:
	/setglobalvar key=promptOrder []|
	/findentry field=comment file="CMC Variables" "Recommended Prompt Order"|
	/let key=wi_uid {{pipe}}|
	/getentryfield field=content file="CMC Variables" {{var::wi_uid}}|
	/let key=pRec {{pipe}}|
	/let key=pOrder ["role", "contexts", "examples", "task", "instructions", "output trigger", "Done"]|
	/let key=selected_btn {{noop}}|
	/len {{var::pOrder}}|
	/let key=len {{pipe}}|
	
	/whilee (len > 0) {:
		/len {{var::pOrder}}|
		/var key=len {{pipe}}|
		/buttons labels={{var::pOrder}}<div>Select the order you want the prompt to be in. Select 'Done' to skip the remaining. (Minimum of 3)</div><div>Default order: contexts, examples, task, instructions</div>Recommended order based on models</div>{{var::pRec}}|
		/var key=selected_btn {{pipe}}|
		/ife ( selected_btn == ''){:
			/echo Aborting |
			/abort
		:}|
		/ife (selected_btn != 'Done') {:
			/addglobalvar key=promptOrder {{var::selected_btn}}|
			/find index=true {{var::pOrder}} {:
				/test left={{var::item}} rule=eq right={{var::selected_btn}}|
			:}|
			/let key=i {{pipe}}|
			/splice start={{var::i}} delete=1 {{var::pOrder}}|
			/var key=pOrder {{pipe}}|
		:}|
		/else {:
			/var key=len 0|
		:}|
	:}|
:}|

/ife (selection == 'Use XML tags') {:
	/setglobalvar key=xmlTags {{noop}}|
	/buttons lables=["Yes", "No"] Do you want the prompts to use **XML Tags** for each part of the prompt? Recommended for DeepSeek models.|
	/setglobalvar key=xmlTags {{pipe}}|
	/ife (xmlTags == '') {:
		/echo Aborting |
		/abort
	:}|
:}|

/ife (selection == 'Add New Model') {:
	/buttons labels={{getglobalvar::models}} What model do you want to base the new model on?|
	/let key=selectedModel {{pipe}}|
	/ife (selectedModel == '') {:
		/echo Aborting |
		/abort
	:}|
	/input What is the name of the new model?|
	/let modelName {{pipe}}|
	/db-list source=chat field=name |
	/let key=databaseList {{pipe}}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::selectedModel}}/CMC%20Generation%20Prompts%20{{var::selectedModel}}.json|
	/let key=f {{pipe}}|
	/ife ( 'CMC Generation Prompts {{var::modelName}}.json' not in databaseList){:
		/db-add source=chat name="CMC Generation Prompts {{var::modelName}}.json" {{var::f}}|
		/db-disable source=chat CMC Generation Prompts {{var::modelName}}.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Generation Prompts {{var::modelName}}.json" {{var::f}}|
		/db-disable source=chat CMC Generation Prompts {{var::modelName}}.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::selectedModel}}/CMC%20Information%20{{var::selectedModel}}.json|
	/var key=f {{pipe}}|
	/ife ( 'CMC Generation Prompts {{var::modelName}}.json' not in databaseList){:
		/db-add source=chat name="CMC Information {{var::modelName}}.json" {{var::f}}|
		/db-disable source=chat CMC Information {{var::modelName}}.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Information {{var::modelName}}.json" {{var::f}}|
		/db-disable source=chat CMC Information {{var::modelName}}.json|
	:}|
	/addglobalvar key=models {{var::modelName}}|
	/join glue="{{newline}}---{{newline}}" {{getglobalvar::models}}|
	/let key=gluedModels {{pipe}}|
	/findentry field=comment file="CMC Variables" "Models"|
	/let key=wi_uid {{pipe}}|
	/setentryfield field=content file="CMC Variables" uid={{var::wi_uid}} {{var::gluedModels}}|
	/popup Don't forget to download the .json files from the databank. It will open automaticly.|
	/db
:}|

/elseif (selection == 'Update QR Scripts') {:
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Version/Version.md |
	/let key=updatedVersion {{pipe}}|
	
	/let key=currentVersion {{noop}}|
	/let key=selected_btn {{noop}}|
	
	/db-list source=character field=name |
	/let key=a {{pipe}}|
	/ife ('Current Version' in a) {:
		/db-get source=character  "Current Version"|
		/var key=currentVersion {{pipe}}|
	:}|
	/ife ((currentVersion == '') or (updatedVersion != currentVersion)) {:
		/whilee (selected_btn == '') {:
			/buttons labels=["Yes", "No", "Changes"] <div>There is a new version.</div><div>Do you want to update to the new version or see what's new?</div>|
			/var key=selected_btn {{pipe}}|
			/ife ( selected_btn == ''){:
				/echo Aborting |
				/abort
			:}|
			/ife (selected_btn == 'Changes') {:
				/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Version/Latest%20Update.md |
				/popup {{var::updatedVersion}}{{newline}}---{{newline}}{{pipe}}|
				/var key=selected_btn {{noop}}|
			:}|
		:}|
	:}|
	/ife (selected_btn == 'Yes') {:
		/qr-set-delete CMC Generate|
		/qr-set-delete CMC Logic|
		/qr-chat-set-off CMC Main|
		/qr-set-delete CMC Main|
		//qr-set-delete CMC Menu|
		/qr-set-delete CMC Automate| 
		
		/wait 1000|
		/qr-set-create CMC Temp|
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Install/Install%20QR.md|
		
		/qr-create set="CMC Temp" label="Install QR" {{pipe}}|
		
		/:"CMC Temp.Install QR"|
		
		/wait 1000|
		/qr-set-delete CMC Temp |
		/wait 10000|
		/forcesave|
		/var key=reload Yes|
		/popup The Lorebooks will now be updated|
		/echo extendedTimeout=0 timeout=0 awaitDismissal=true Press to Continue|
	:}|
:}|

/ife (selection == 'Download Model Lorebooks') {:
	/buttons labels=["dans-personalityengine-v1.1.0-12b", "EsotericSage-12B.i1", "UncensoredLM-DeepSeek-R1-Distill-Qwen-14B", "Llama-Joycaption-Beta-One-Hf-Llava"]| Select the Model you want to download the Lorebooks for.|

	/let key=selectedModel {{pipe}}|
	/db-list source=chat field=name |
	/let key=databaseList {{pipe}}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::selectedModel}}/CMC%20Generation%20Prompts%20{{var::selectedModel}}.json|
	/let key=f {{pipe}}|
	/ife ( 'CMC Generation Prompts {{var::selectedModel}}.json' not in databaseList){:
		/db-add source=chat name="CMC Generation Prompts {{var::selectedModel}}.json" {{var::f}}|
		/db-disable source=chat CMC Generation Prompts {{var::selectedModel}}.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Generation Prompts {{var::selectedModel}}.json" {{var::f}}|
		/db-disable source=chat CMC Generation Prompts {{var::selectedModel}}.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::selectedModel}}/CMC%20Information%20{{var::selectedModel}}.json|
	/var key=f {{pipe}}|
	/ife ( 'CMC Information {{var::selectedModel}}.json' not in databaseList){:
		/db-add source=chat name="CMC Information {{var::selectedModel}}.json" {{var::f}}|
		/db-disable source=chat CMC Information {{var::selectedModel}}.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Information {{var::selectedModel}}.json" {{var::f}}|
		/db-disable source=chat CMC Information {{var::selectedModel}}.json|
	:}|
	/popup Don't forget to download the .json files from the databank. It will open automaticly.|
	/db
:}|

/ife ((selection == 'Download newest version of Lorebooks') or (reload == 'Yes')) {:
	/setvar key=counter 0|
	/db-list source=chat field=name |
	/let key=databaseList {{pipe}}|
	/let key=f {{noop}}|
	
	/foreach {{getglobalvar::models}} {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::item}}/CMC%20Generation%20Prompts%20{{var::item}}.json|
		/var key=f {{pipe}}|
		/addvar key=counter 1|
		
		/ife ( 'CMC Generation Prompts {{var::item}}.json' not in databaseList){:
			/db-add source=chat name="CMC Generation Prompts {{var::item}}.json" {{var::f}}|
			/db-disable source=chat CMC Generation Prompts {{var::item}}.json|
		:}|
		/else {:
			/db-update source=chat name="CMC Generation Prompts {{var::item}}.json" {{var::f}}|
			/db-disable source=chat CMC Generation Prompts {{var::item}}.json|
		:}|
		
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::item}}/CMC%20Information%20{{var::item}}.json|
		/var key=f {{pipe}}|
		/addvar key=counter 1|
		
		/ife ( 'CMC Generation Prompts {{var::item}}.json' not in databaseList){:
			/db-add source=chat name="CMC Information {{var::item}}.json" {{var::f}}|
			/db-disable source=chat CMC Information {{var::item}}.json|
		:}|
		/else {:
			/db-update source=chat name="CMC Information {{var::item}}.json" {{var::f}}|
			/db-disable source=chat CMC Information {{var::item}}.json|
		:}|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Variables.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Variables.json' not in databaseList){:
		/db-add source=chat name="CMC Variables.json" {{var::f}}|
		/db-disable source=chat CMC Variables.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Variables.json" {{var::f}}|
		/db-disable source=chat CMC Variables.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Questions.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Questions.json' not in databaseList){:
		/db-add source=chat name="CMC Questions.json" {{var::f}}|
		/db-disable source=chat CMC Questions.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Questions.json" {{var::f}}|
		/db-disable source=chat CMC Questions.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Rules.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Rules.json' not in databaseList){:
		/db-add source=chat name="CMC Rules.json" {{var::f}}|
		/db-disable source=chat CMC Rules.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Rules.json" {{var::f}}|
		/db-disable source=chat CMC Rules.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Templates.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Templates.json' not in databaseList){:
		/db-add source=chat name="CMC Templates.json" {{var::f}}|
		/db-disable source=chat CMC Templates.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Templates.json" {{var::f}}|
		/db-disable source=chat CMC Templates.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Static%20Variables.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Static Variables.json' not in databaseList){:
		/db-add source=chat name="CMC Static Variables.json" {{var::f}}|
		/db-disable source=chat CMC Static Variables.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Static Variables.json" {{var::f}}|
		/db-disable source=chat CMC Static Variables.json|
	:}|
	
	/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Guides.json |
	/var key=f {{pipe}}|
	/addvar key=counter 1|
	
	/ife ( 'CMC Guides.json' not in databaseList){:
		/db-add source=chat name="CMC Guides.json" {{var::f}}|
		/db-disable source=chat CMC Guides.json|
	:}|
	/else {:
		/db-update source=chat name="CMC Guides.json" {{var::f}}|
		/db-disable source=chat CMC Guides.json|
	:}|
	
	/buttons labels=["Yes", "No"] Do you want to download the optional CMC Anatomy (WIP) lorebook?|
	/let key=button {{pipe}}|
	/ife (button == 'Yes') {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Anatomy.json |
		/var key=f {{pipe}}|
		/addvar key=counter 1|
		
		/ife ( 'CMC Guides.json' not in databaseList){:
			/db-add source=chat name="CMC Anatomy.json" {{var::f}}|
			/db-disable source=chat CMC Anatomy.json|
		:}|
		/else {:
			/db-update source=chat name="CMC Anatomy.json" {{var::f}}|
			/db-disable source=chat CMC Anatomy.json|
		:}|
	:}|
	
	
	/popup Download all .json files starting with CMC (Should be {{getvar::counter}} of them) from SillyTavern Data Bank (It will open when you press ok) and import them into the lorebook/World Info.|
	/db|
	/flushvar counter|
	/echo extendedTimeout=0 timeout=0 awaitDismissal=true Press to Continue|
:}|

/ife (reload == 'Yes') {:
	/wait 1000|
	/reload-page|
:}|