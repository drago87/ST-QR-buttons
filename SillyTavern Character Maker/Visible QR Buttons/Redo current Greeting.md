/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Generate First Message" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let selected_btn {{noop}}|
/let key=len {{noop}}|

/var key=do Yes|
/var key=variableName "altGreeting"|
/buttons labels=["Yes", "No"] Are you sure you want to redo this Greeting?|
/var key=do {{pipe}}|
/ife (do == '') {:
	/echo Aborting |
	/abort
:}|
/elseif (do == 'No') {:
	/echo Aborted by user|
	/abort
:}|

/ife ( do == 'Yes' ) {:
	/setvar key=genSettings {}|
	/setvar key=genSettings index=wi_book_key "First Message"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "Connections: {{getvar::connections}}"|
	/addvar key=extra "{{newline}}{{getvar::parsedApperance}}"|
	/ife (parsedSentientLevel != 'None') {:
		/addvar key=extra "{{getvar::parsedSentientLevel}}"|
	:}|
	/addvar key=extra "{{newline}}BACKGROUND SNAPSHOT — "STORY START"
This block represents the **first moment of the story**. 
It is sealed reference only. 
You must NOT copy, paraphrase, restate, or continue its content in any form.

Story start:
[{{getvar::scenarioOverview}}]"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
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
	
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife ( logicBasedInstruction != '') {:
		/addvar key=logicBasedInstruction {{newline}}|
	:}|
	/getvar key=writingInstruct index=0|
	/re-replace find="/\*\*Formatting Style:\*\*\s/g" replace="" {{pipe}}|
	/addvar key=logicBasedInstruction "- {{pipe}} Do **not** alter or reinterpret the formatting — apply it **exactly**."|
	/ife (('Fully Animalistic' in parsedSentientLevel) or ('Semi-Sapient' in parsedSentientLevel)) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Do **not** include a visual impression that involves posture, clothing, or emotional expression — use physical behaviors like stance, motion, or instinctual cues instead."|
		
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Do **not** include dialogue or speech. Replace the final line with a physical reaction or animal sound (e.g., snort, tail flick, attentive glance)."|
		
	:}|
	/elseif ('Emotionally Aware' in parsedSentientLevel) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		  /addvar key=logicBasedInstruction "- Avoid explicit speech or internal thoughts, but you **may** show emotion through posture, proximity, vocal sounds, or learned routine behavior. Final line should still be a **non-verbal** cue."|
	:}|
	/else {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Include a quick visual impression of {{getvar::firstName}} — posture, outfit, or energy."|
		
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- End with a single in-character line of dialogue, in quotes. Keep it short and expressive — no more than 15 words unless otherwise specified."|
		
	:}|
	
	
	/ife (('Fully Animalistic' in parsedSentientLevel) or ('Semi-Sapient' in parsedSentientLevel)) {:
		/setvar key=sceneChecklist "```{{newline}}Your scene must:{{newline}}- Describe where {{getvar::firstName}} is.{{newline}}- What {{getvar::subjPronoun}} is physically doing or reacting to.{{newline}}- What’s around {{getvar::objPronoun}} — include surroundings, weather, scent, or movement.{{newline}}- Use only instinctual behavior or physical signals — no dialogue, thoughts, or human-style emotion in a way that someone can respond to.{{newline}}```"|
	:}|
	
	/else {:
		/setvar key=sceneChecklist "```{{newline}}Your scene must:{{newline}}- Describe where {{getvar::firstName}} is.{{newline}}- What {{getvar::subjPronoun}} is doing or feeling.{{newline}}- What’s around {{getvar::objPronoun}} (setting, tone, mood).{{newline}}- How {{getvar::firstName}} looks — posture, expression, outfit, or notable traits.{{newline}}- End with a single in-character line of dialogue that reflects {{getvar::possAdjPronoun}} personality and tone in a way that someone can respond to.{{newline}}```"|
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
	
	//addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
:}|
/else {:
	//addvar key=dataBaseNames {{var::variableName}}|
:}|


/message-edit message={{getvar::altGreetID}} await=true {{getvar::altGreeting}}|

/swipes-list message={{getvar::altGreetID}}|
/setvar key=altGreetings {{pipe}}|
/addvar key=dataBaseNames altGreetings|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|

/forcesave|