/let key=models ["dans-personalityengine-v1.1.0-12b", "EsotericSage-12B.i1", "UncensoredLM-DeepSeek-R1-Distill-Qwen-14B", "Llama-Joycaption-Beta-One-Hf-Llava"]|
/buttons multiple=true labels={{var::models}} Select the models you want to download the model lorebook prompts for.|
/setglobalvar key=models {{pipe}}|
/ife (models == '') {:
	/echo Aborting |
	/abort
:}|

/let key=branch {{noop}}|
/ife (beta != 'Yes') {:
	/var key=branch main|
:}|
/else {:
	/var key=branch Fetch-Files|
:}|

/setvar key=choicePrompt 
<div>Prompts are tested with this model.<div>|

/ife ('dans-personalityengine-v1.1.0-12b' in models) {:
	/addvar key=choicePrompt <div><a href="https://huggingface.co/bartowski/Dans-PersonalityEngine-V1.1.0-12b-GGUF/blob/main/Dans-PersonalityEngine-V1.1.0-12b-Q6_K.gguf">Dans-PersonalityEngine-V1.1.0-12b-Q6_K.gguf</a></div>|
:}|
/ife ('EsotericSage-12B.i1' in models) {:
	/addvar key=choicePrompt <div><a href="https://huggingface.co/mradermacher/EsotericSage-12B-i1-GGUF/blob/main/EsotericSage-12B.i1-Q6_K.gguf">EsotericSage-12B.i1-Q6_K.gguf</a></div>|
:}|
/ife ('UncensoredLM-DeepSeek-R1-Distill-Qwen-14B' in models) {:
	/addvar key=choicePrompt <div><a href="https://huggingface.co/bartowski/uncensoredai_UncensoredLM-DeepSeek-R1-Distill-Qwen-14B-GGUF/blob/main/uncensoredai_UncensoredLM-DeepSeek-R1-Distill-Qwen-14B-Q4_K_M.gguf">uncensoredai_UncensoredLM-DeepSeek-R1-Distill-Qwen-14B-Q4_K_M</a></div>|
:}|
/ife ('Llama-Joycaption-Beta-One-Hf-Llava' in models) {:
	/addvar key=choicePrompt <div><a href="https://huggingface.co/concedo/llama-joycaption-beta-one-hf-llava-mmproj-gguf/blob/main/Llama-Joycaption-Beta-One-Hf-Llava-Q8_0.gguf">Llama-Joycaption-Beta-One-Hf-Llava-Q8_0.gguf (Vision Model)</a></div>|
	/addvar key=choicePrompt <div><a href="https://huggingface.co/concedo/llama-joycaption-beta-one-hf-llava-mmproj-gguf/blob/main/Llama-Joycaption-Beta-One-Hf-Llava-Q4_K.gguf">Llama-Joycaption-Beta-One-Hf-Llava-Q4_0.gguf (Vision Model If you need it for only Image captioning. It is small enough for you to **Sidecar** it (load it in a separate instance of KoboldCPP))</a></div>|
	/addvar key=choicePrompt <div><a href="https://huggingface.co/concedo/llama-joycaption-beta-one-hf-llava-mmproj-gguf/blob/main/llama-joycaption-beta-one-llava-mmproj-model-f16.gguf">Mmproj file for Llama-Joycaption-Beta-One-Hf-Llava (Needed for Image captioning. Load it in the same instance with the normal LLM. (Add it to Loaded Files->Mmproj File))</a></div>|
:}|

/popup {{getvar::choicePrompt}}|

/db-list source=character field=name |
/let key=databaseList {{pipe}}|

/*
/ife ('model' not in databaseList) {:
	/db-add source=character name=model {{getglobalvar::model}}|
	/db-disable source=character model|
:}|
/else {:
	/db-update source=character name=model {{getglobalvar::model}}|
	/db-disable source=character model|
:}|
*|

/qr-list CMC Temp|
/let key=qrList {{pipe}}|

/ife ('Character maker install script' in qrList) {:
	/qr-delete set="CMC Temp" label="Character maker install script"|
	/qr-update set="CMC Temp" label="Install Script" title="A script that will walk you through the setup."|
	/qr-chat-set-on CMC Temp|
:}|
/qr-set-list all|
/var key=qrList {{pipe}}|

/ife ('CMC Main' not in qrList) {:
	/buttons labels=["Manually", "Automatically"] Do you want to Manually or Automatically download the QR scripts?|
	/setvar key=selected_btn {{pipe}}|

	/ife ( selected_btn == '') {:
		/echo Aborting |
		/abort|
	:}|
	/elseif ( selected_btn == 'Manually') {:
		/popup <div>You need to manually download these files and import them to Extensions → Quick Reply</div>
<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/QR%20Sets/CMC%20Generate.json">CMC Generate</a></div><div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/QR%20Sets/CMC%20Logic.json">CMC Logic</a></div><div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/QR%20Sets/CMC%20Main.json">CMC Main</a></div><div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/QR%20Sets/CMC%20Automate.json">CMC Automate (optional but recommended)</a></div>|
	:}|
	/elseif ( selected_btn == 'Automatically') {:
		/qr-list CMC Temp|
		/let x {{pipe}}|
		/ife ('Install QR' not in x) {:
			/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Install/Install%20QR.md|
			/qr-create set="CMC Temp" label="Install QR" {{pipe}}|
		:}|
		//[[Install QR]]|
		/:"CMC Temp.Install QR"|
		/qr-delete set="CMC Temp" label="Install QR"|
	:}|
:}|

/ife ( selected_btn != '') {:
	/buttons labels=["Manually", "Semi Automatically"] Do you want to Manually or Semi Automatically download the World Info/Lore Book?|
	/let key=selected_btn {{pipe}}|
	
	/ife ( selected_btn == '') {:
		/echo Aborting |
		/abort|
	:}|
	/elseif ( selected_btn == 'Manually') {:
		/setvar key=popupLinks "Model Specific Lorebooks"|
		/foreach {{getglobalvar::models}} {:
			/addvar key=popupLinks "<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::item}}/CMC%20Generation%20Prompts%20{{var::item}}.json">CMC Generation Prompts {{var::item}}</a></div>|
			/addvar key=popupLinks "<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/{{var::item}}/CMC%20Information%20{{var::item}}.json">CMC Information {{var::item}}</a></div>|
			/addvar key=popupLinks "<div>---</div>"|
	
		:}|
		/popup <div>You need to manually download these files and import them to the World Info</div>
		{{getvar::popupLinks}}
		<div>General Lorebooks</div>
	<div><a href="https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Questions.json">CMC Questions</a></div>
	<div><a href="https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Rules.json">CMC Rules</a></div>
	<div><a href="https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Templates.json">CMC Templates</a></div>
	<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20%20Static%20Variablers.json">CMC Static Variablers</a></div>
	<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Variables.json">CMC Variablers</a></div>
	<div><a href="https://github.com/drago87/ST-Character-Maker/blob/{{var::branch}}/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Anatomy.json">CMC Anatomy (Optional, WIP)</a></div>|
	
	:}|
	/elseif ( selected_btn == 'Semi Automatically') {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Install/Install%20WI.md|
		/qr-create set="CMC Temp" label="Install WI" {{pipe}}|
		//[[Install WI]]|
		/:"CMC Temp.Install WI"|
		/qr-delete set="CMC Temp" label="Install WI"
	:}|
	
	/ife ( selected_btn == '') {:
		/echo Aborting |
		/abort|
	:}|
	/else {:
		/qr-set-list all|
		/var qrList {{pipe}}|
		/ife ( 'CMC Temp' in qrList ) {:
			/qr-chat-set-off CMC Temp|
			/qr-set-delete CMC Temp|
		:}|
	:}|
:}|

/flushvar selected_btn|
/flushvar popupLinks|