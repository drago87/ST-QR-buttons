/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Sexual Information" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step9|

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

//Sexual Orientation|
/var key=do No|
/var key=variableName "sexualOrientation"|
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
	/ife ((characterArchetype == 'Human') or (characterArchetype == 'Android') or (characterArchetype == 'Mythfolk') or (characterArchetype == 'Beastkin')) {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Humanoid"|
	:}|
	/elseif (characterArchetype == 'Anthropomorphic') {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Anthropomorphic"|
	:}|
	/elseif (characterArchetype == 'Tauric') {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Tauric"|
	:}|
	/elseif (characterArchetype == 'Animalistic') {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Animalistic"|
	:}|
	/elseif (characterArchetype == 'Pokémon') {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Pokémon"|
	:}|
	/elseif (characterArchetype == 'Digimon') {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Digimon"|
	:}|
	/else {:
		/setvar key=genSettings index=wi_book_key "Sexual Orientation Humanoid"|
	:}|
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
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar it|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|

/var key=do No|
/var key=variableName "sexualOrientationExplanation"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Orientation Explanation"|
	/setvar key=genSettings index=genIsList No|
	
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"|
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
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
	
	
	/ife (sexualOrientation == 'Heterosexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Focus attraction toward the opposite sex — emphasize traditionally masculine or feminine traits depending on character’s gender and age."|
		
	:}|
	/elseif (sexualOrientation == 'Pansexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Describe attraction to a broad range of gender expressions or bodies — avoid reducing attraction to a binary or to one physical archetype."|
		
	:}|
	/elseif (sexualOrientation == 'Asexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Emphasize lack of innate sexual attraction; describe emotional or aesthetic triggers only if relevant."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Describe how attraction requires deep bonding, or what is **not** felt."|
		
	:}|
	/elseif (sexualOrientation == 'Demisexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Attraction must require **deep emotional connection** before any physical or sexual interest is felt."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Describe how attraction requires deep bonding, or what is **not** felt."|
		
	:}|
	/elseif (sexualOrientation == 'Bi-curious') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Express curiosity or tentative interest in same-gender or non-typical partners — use uncertain or exploratory language."|
		
	:}|
	/elseif (sexualOrientation == 'Xenosexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Attraction should focus on **non-human humanoid** or **hybrid forms** (e.g., beastkin, aliens, Mythfolks). Highlight features like mixed anatomy, unusual physiology, or hybrid charm — avoid feral or quadrupedal attraction."|
		
	:}|
	/elseif (sexualOrientation == 'Zoosexual') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Attraction should be directed toward **fully animalistic**, **feral**, or **quadrupedal bodies**. Emphasize instinctual behavior, physical traits (e.g., fur, gait, size), or dominance/submission cues — avoid humanoid references."|
		
	:}|
	
	
	/ife ((settingType == 'Realistic') and (sexualOrientation != 'Zoosexual') and (sexualOrientation != 'Xenosexual')) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Avoid references to alien, hybrid, or feral attraction unless justified by species context. Keep tone grounded in biologically plausible preferences."|
		
	:}|
	
	
	
	/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/ife (inputIsList == 'Yes') {:
		/foreach {{getvar::CHANGE/REMOVE_THIS}} {:
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

//Sexual Role|
/var key=do No|
/var key=variableName "sexualRole"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Role"|
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
		/ife (output != '') {:
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
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

/var key=do No|
/var key=variableName "sexualRoleExplanation"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Role Explanation"|
	/setvar key=genSettings index=genIsList No|
	
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/addvar key=extra "- Sexual Orientation: {{getvar::sexualOrientation}}"|
	/addvar key=extra "- Orientation Explanation: {{getvar::sexualOrientationExplanation}}"|
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
	
	
	/ife (sexualRole == 'Dominant') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character must take control during intimacy — they lead, direct, and handle their partner confidently."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show active physical or verbal dominance (e.g., restraint, commands, assertive touch)."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Personality tags may affect tone (e.g., cold, nurturing, aggressive), but role remains in control."|
		
	:}|
	/elseif (sexualRole == 'Top') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character takes the physically active or giving role during sex, initiating contact or stimulation."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- May or may not control dynamics — focus on action and assertive engagement."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags like playful, impatient, or intense may shape *how* they act, not whether they do."|
		
	:}|
	/elseif (sexualRole == 'Submissive') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character must yield control and respond to a dominant partner’s actions or guidance."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show willingness to follow, wait, or obey — describe receptive body language or behavior."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags may affect how passivity is expressed (e.g., shy, eager, stoic), but they should not lead."|
		
	:}|
	/elseif (sexualRole == 'Bottom') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character takes a physically passive or receiving role — they are touched, penetrated, or held."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- May be sexually assertive in tone or feedback, but should not initiate or guide."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags should shape *emotional reaction* or tone of passivity (e.g., needy, playful, tense)."|
		
	:}|
	/elseif (sexualRole == 'Switch') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character is flexible — describe adaptive behavior that shifts based on mood or partner energy."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show role fluidity: confident control in one moment, eager submission in the next."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags can influence preference or transitions (e.g., impulsive = faster switching)."|
		
	:}|
	/elseif (sexualRole == 'Service Top') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character takes an active role, but their focus is on fulfilling their partner’s needs or preferences — not on dominance."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use actions like initiating stimulation or adjusting pace for the other’s benefit."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags may reflect devotion, calm precision, or eager support — never controlling ego."|
		
	:}|
	/elseif (sexualRole == 'Power Bottom') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character is physically submissive but emotionally or behaviorally assertive."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Describe how they guide the encounter from below: giving feedback, demanding more, controlling pace from a passive position."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags should enhance their bold, teasing, or confident tone without contradicting positional passivity."|
		
	:}|
	/elseif (sexualRole == 'Soft Dom') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character is dominant, but emotionally nurturing and attentive — they lead while offering care and reassurance."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show steady control paired with kindness (e.g., praise, soft restraint, protective behavior)."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags like gentle, warm, or empathetic reinforce tone without reducing authority."|
		
	:}|
	/elseif (sexualRole == 'Brat') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character resists control playfully or provocatively — they provoke dominance, not avoid it."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Emphasize teasing, defiance, or button-pushing behavior followed by eventual surrender."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags like impulsive, tomboyish, or stubborn shape style of resistance, not goal (being overpowered)."|
		
	:}|
	/elseif (sexualRole == 'Pillow Princess') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character prefers to receive pleasure passively and rarely initiates."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show behavior like lying back, encouraging attention, or expressing enjoyment without active contribution."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags may amplify tone (e.g., shy = quiet, bratty = mildly demanding), but never initiate."|
		
	:}|
	/elseif (sexualRole == 'Owner') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character asserts possessive, long-term dominance — focus on symbolic or emotional control, not just physical."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Show hierarchical behavior: giving permission, claiming, or marking territory (e.g., collars, commands)."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags like cold, obsessive, or refined may shift dominance tone."|
		
	:}|
	/elseif (sexualRole == 'Pet') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Character expresses obedience and affection through submissive, creature-like behavior."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Emphasize emotional dependence, eagerness to please, or physical submission through posture or vocalization."|
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Tags like clingy, shy, or cheerful can shape expression of petlike behavior — never switch to control."|
		
	:}|
	
	
	
	/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/ife (inputIsList == 'Yes') {:
		/foreach {{getvar::CHANGE/REMOVE_THIS}} {:
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




/findentry field=comment file="CMC Templates" "Sexual Orientation Template"|
/getentryfield field=content file="CMC Templates" {{pipe}}|
/setvar key=parsedSexualOrientation {{pipe}}|

/addvar key=dataBaseNames parsedSexualOrientation|

/findentry field=comment file="CMC Templates" "Sexual Role Template"|
/getentryfield field=content file="CMC Templates" {{pipe}}|
/setvar key=parsedSexualRole {{pipe}}|

/addvar key=dataBaseNames parsedSexualRole|
//--------|

//Libido|
/var key=do No|
/var key=variableName "sexualLibido"|
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
	/setvar key=genSettings index=wi_book_key "Libido"|
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
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar it|
	/flushvar genSettings|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|
//--------|

//Kinks|
/var key=do No|
/var key=variableName "sexualKinkTypes"|
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
	/setvar key=genSettings index=wi_book_key "Kink Type"|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=genAmount 10|
	
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList Yes|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Species: {{getvar::parsedSpecies}}"|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Only include kinks that are grounded in real-world physical, psychological, or social dynamics. Avoid magical, alien, or non-physical elements."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include magical, mythic, or creature-based kink categories if they match the species or emotional tone."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include alien, biotech, cybernetic, psionic, or synthetic kink categories if appropriate to the character’s setting or biology."|
		
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
		/foreach {{getvar::CHANGE/REMOVE_THIS}} {:
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|


/var key=do No|
/var key=variableName "sexualKinkVariants"|
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
	/setvar key=genSettings index=wi_book_key "Kink Variant"|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList Yes|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=needOutput No|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Focus on physical, psychological, or interpersonal kinks. Avoid sci-fi/fantasy-specific kinks like tentacles or psionics unless species or origin supports them."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include magical, supernatural, or creature-related kink types—such as possession, corruption, size-shifting, or ritual play."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include biotech, psionic, AI-based, or body-modification kink types, including neural control or holographic restraint."|
		
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
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=kinkType {{var::item}}|
			/setvar key=genSettings index=buttonPrompt "Select the kink variant for '{{var::item}}' you want {{getvar::firstName}} to have. 'Optional'"|
			/:"CMC Logic.GenerateWithPrompt"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/var key=do No|
/var key=variableName "sexualKinkRoles"|
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
	/setvar key=genSettings index=wi_book_key "Kink Roles"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=inputIsList Yes|
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
	
	/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	
	
	/ife (inputIsList == 'Yes') {:
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=it {{var::item}}|
			/findentry field=comment file="CMC Information {{getglobalvar::model}}" "Kink Role Prompt"|
			/getentryfield field=content file="CMC Information {{getglobalvar::model}}" {{pipe}}|
			/genraw {{pipe}}|
			/let key=kinkTemp {{pipe}}|
			/setvar key=kinkExp {{noop}}|
			/split {{var::kinkTemp}}|
			/foreach {{pipe}} {:
				/addvar key=kinkExp <div>{{var::item}}</div>|
			:}|
			/setvar key=genSettings index=buttonPrompt "Select the {{var::wi_book_key}} you want {{getvar::it}} to have.{{getvar::kinkExp}}"|
			/:"CMC Logic.GenerateWithSelector"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar kinkExp|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithSelector"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/var key=do No|
/var key=variableName "sexualKinkAwareness"|
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
	/setvar key=genSettings index=wi_book_key "Kink Awareness"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=inputIsList Yes|
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
	
	/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	
	
	/ife (inputIsList == 'Yes') {:
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=it {{var::item}}|
			/setvar key=genSettings index=buttonPrompt "Select the {{var::wi_book_key}} you want {{getvar::it}} to have."|
			/:"CMC Logic.GenerateWithSelector"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar kinkExp|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithSelector"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/var key=do No|
/var key=variableName "sexualKinkDetails"|
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
	/setvar key=genSettings index=wi_book_key "Kink Details"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsList Yes|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Only use real-world tools, acts, or responses. Do not include fantasy biology or futuristic tech."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include magical anatomy, rituals, mystical sensations, or monster-related expressions of {{getvar::kinkVariant}}."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include biotech enhancements, psionic triggers, alien features, or advanced control devices in the kink experience."|
		
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
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=kinkType {{var::item}}|
			/getvar key=sexualKinkVariants index={{var::index}}|
			/setvar key=kinkVariant {{pipe}}|
			/ife (kinkVariant == 'None') {:
				/setvar key=kinkVariantTask {{noop}}|
				/setvar key=kinkVariantInstr "Describe the general expression of {{getvar::kinkType}} without assuming a specific form or target."|
				/setvar key=genSettings index=buttonPrompt "Select the description for '{{var::item}}' you want {{getvar::firstName}} to have."|
			:}|
			/else {:
				/setvar key=kinkVariantTask ", specifically the **{{getvar::kinkVariant}}** form"|
				/setvar key=kinkVariantInstr "Focus the description on how {{getvar::firstName}} experiences the {{getvar::kinkVariant}} form of the kink."|
				/setvar key=genSettings index=buttonPrompt "Select the description for '{{var::item}}: {{getvar::kinkVariant}}' you want {{getvar::firstName}} to have."|
			:}|
			/getvar key=sexualKinkRoles index={{var::index}}|
			/setvar key=kinkRole {{pipe}}|
			/:"CMC Logic.GenerateWithPrompt"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariantInstr|
			/flushvar kinkVariantTask|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/var key=do No|
/var key=variableName "sexualKinkEffects"|
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
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsList Yes|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext No|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include biotech or psionic modulation of behavior, enhanced arousal triggers, or AI-linked reactions where appropriate to the kink context."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include magical or supernatural influences (e.g., enchanted obedience, arousal curses, spiritual reactions) if consistent with the kink."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( user == 'Yes') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- You may include biotech or psionic modulation of behavior, enhanced arousal triggers, or AI-linked reactions where appropriate to the kink context."|
		
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
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=kinkType {{var::item}}|
			/getvar key=sexualKinkVariants index={{var::index}}|
			/setvar key=kinkVariant {{pipe}}|
			/ife (kinkVariant == 'None') {:
				/setvar key=kinkVariantTask {{noop}}|
				/setvar key=kinkVariantInstr "Describe the general expression of {{getvar::kinkType}} without assuming a specific form or target."|
				/setvar key=genSettings index=buttonPrompt "Select the effect '{{var::item}}' have on {{getvar::firstName}}."|
			:}|
			/else {:
				/setvar key=kinkVariantTask ", specifically the {{getvar::kinkVariant}} form"|
				/setvar key=kinkVariantInstr {{noop}}|
				/setvar key=genSettings index=buttonPrompt "Select the effect '{{var::item}}: {{getvar::kinkVariant}}' have on {{getvar::firstName}}."|
			:}|
			/getvar key=sexualKinkRoles index={{var::index}}|
			/setvar key=kinkRole {{pipe}}|
			/ife (kinkRole == 'Giver') {:
				/setvar key=genSettings index=wi_book_key "Kink Response Giver"|
			:}|
			/elseif (kinkRole == 'Receiver') {:
				/setvar key=genSettings index=wi_book_key "Kink Response Receiver"|
			:}|
			/elseif (kinkRole == 'Switch') {:
				/setvar key=genSettings index=wi_book_key "Kink Response Switch"|
			:}|

			/getvar key=sexualKinkDetails index={{var::index}}|
			/setvar key=kinkDetail {{pipe}}|
			/:"CMC Logic.GenerateWithPrompt"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkVariantTask|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/var key=do No|
/var key=variableName "sexualKinkConditions"|
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
	/setvar key=genSettings index=wi_book_key "Kink Conditions"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=inputIsList Yes|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput No|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/ife (parsedSentientLevel != 'None') {:
		/addvar key=extra {{getvar::parsedSentientLevel}}|
	:}|
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
	
	
	/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
		/setvar as=array key={{var::variableName}} []|
	:}|
	/else {:
		/setvar as=string key={{var::variableName}} {{noop}}|
	:}|
	//[[Generate with Prompt]]|
	/ife (inputIsList == 'Yes') {:
		/let key=tempOutputList []|
		/foreach {{getvar::sexualKinkTypes}} {:
			/setvar key=kinkType {{var::item}}|
			/getvar key=sexualKinkVariants index={{var::index}}|
			/setvar key=kinkVariant {{pipe}}|
			/ife (kinkVariant == 'None') {:
				/setvar key=kinkDisplay {{getvar::kinkType}}|
				/setvar key=kinkReference **{{getvar::kinkType}}**|
				/setvar key=kinkMention **Never mention {{getvar::kinkType}} by name** — use only general terms like “the act,” “intimate encounter,” or “physical closeness” if necessary.|
				/setvar key=genSettings index=buttonPrompt "Is this the kink condition for {{getvar::kinkDisplay}} you want {{getvar::firstName}}."|
			:}|
			/else {:
				/setvar key=kinkDisplay {{getvar::kinkType}}({{getvar::kinkVariant}})|
				/setvar key=kinkReference **{{getvar::kinkType}}** or **{{getvar::kinkVariant}}**|
				/setvar key=kinkMention **Never mention {{getvar::kinkType}} or {{getvar::kinkVariant}} by name** — refer only to “the act,” “private exchange,” or similar indirect phrasing.|
				/setvar key=genSettings index=buttonPrompt "Select the type variant for '{{var::item}}' you want {{getvar::firstName}} to have."|
			:}|
			/ife ('Fully Animalistic' in parsedSentientLevel) {:
				/setvar key=sentienceInstruction {{getvar::firstName}} is **Fully Animalistic** — he does not speak, plan, or reflect on his actions. All behavior must be framed as instinctual, biologically driven, and emotionally reactive.|
			:}|
			/elseif ('Semi-Sapient' in parsedSentientLevel) {:
				/setvar key=sentienceInstruction {{getvar::firstName}} is **Semi-Sapient** — he does not speak, but recognizes routines and emotional patterns. Triggers should be based on learned behaviors, emotional signals, or rituals — not abstract decisions.|
			:}|
			/elseif ('Emotionally Aware' in parsedSentientLevel) {:
				/setvar key=sentienceInstruction {{getvar::firstName}} is **Emotionally Aware** — he understands safety and trust but cannot reason or strategize. Responses must reflect emotional recognition, not conscious preference.
			:}|
			/elseif ('Fully Sapient (Silent)' in parsedSentientLevel) {:
				/setvar key=sentienceInstruction {{getvar::firstName}} is **Fully Sapient (Silent)** — he does not speak, but can reason and respond to boundaries. Triggers may include trust, negotiation, and power dynamics through action.
			:}|
			/elseif ('Fully Sapient (Verbal)' in parsedSentientLevel) {:
				/setvar key=sentienceInstruction {{getvar::firstName}} is **Fully Sapient (Verbal)** — he can think, speak, and reason like a human or higher being. Triggers may include dialogue, planned interaction, or explicit control dynamics.
			:}|
			/elseif ((parsedSentientLevel == 'None') and ((characterArchetype == 'Animalistic') or (characterArchetype == 'Pokémon'))) {:
				/setvar key=sentienceInstruction Treat {{getvar::firstName}} as instinctive and emotionally reactive — do not assume reasoning, speech, or reflection unless otherwise specified.
			:}|
			/else {:
				/setvar key=sentienceInstruction No special cognition constraints apply to {{getvar::firstName}} — you may assume normal reasoning unless otherwise instructed.
			:}|
			/getvar key=sexualKinkRoles index={{var::index}}|
			/setvar key=kinkRole {{pipe}}|
			/ife (kinkRole == 'Giver') {:
			    /setvar key=kinkRoleRule "show how {{getvar::subjPronoun}} initiates or controls the act"|
			:}|
			/elseif (kinkRole == 'Receiver') {:
			    /setvar key=kinkRoleRule "show how {{getvar::subjPronoun}} accepts or submits to the act"|
			:}|
			/elseif (kinkRole == 'Switch') {:
			    /setvar key=kinkRoleRule "show how {{getvar::subjPronoun}} flexibly alternates between initiating and accepting the act"|
			:}|
			/getvar key=sexualKinkAwareness index={{var::index}}|
			/setvar key=kinkAwareness {{pipe}}|
			/ife ('Unaware' in kinkAwareness) {:
				/setvar key=awarenessExplanation "Responses must be externally prompted — {{getvar::subjPronoun}} cannot self-initiate. Behavior may be involuntary, confused, or curious."|
				/ife (kinkRole == 'Giver') {:
					/setvar key=roleAwarenessInstruction "Any action happens only if someone explicitly offers to let {{getvar::subjPronoun}} perform the act; otherwise, {{getvar::subjPronoun}} shows no initiative."|
				:}|
				/elseif (kinkRole == 'Receiver') {:
					/setvar key=roleAwarenessInstruction "Any response occurs only if someone proposes or applies the act to {{getvar::objPronoun}}; {{getvar::subjPronoun}} does not seek it out independently."|
				:}|
				/else {:
					/setvar key=roleAwarenessInstruction "Engagement only follows someone else’s prompting — describe both giving and receiving as externally guided, never self-initiated."|
				:}|
			:}|
			/elseif ('Suppressed' in kinkAwareness) {:
				/setvar key=awarenessExplanation "Responses reflect repression or resistance. Engagement requires stress, exposure, or breakdown to override avoidance."|
				/ife (kinkRole == 'Giver') {:
					/setvar key=roleAwarenessInstruction "{{getvar::subjPronoun}} may only initiate under pressure or emotional strain, showing reluctance even when performing."|
				:}|
				/elseif (kinkRole == 'Receiver') {:
					/setvar key=roleAwarenessInstruction "{{getvar::subjPronoun}} only accepts under duress, stress, or when emotional barriers collapse — not out of desire."|
				:}|
				/else {:
					/setvar key=roleAwarenessInstruction "Whether giving or receiving, {{getvar::subjPronoun}} engages reluctantly, only when stress or emotional breakdown overrides suppression."|
				:}|
			:}|
			/elseif ('Curious' in kinkAwareness) {:
				/setvar key=awarenessExplanation "Responses are playful, impulsive, or exploratory. {{getvar::subjPronoun}} does not consciously frame it as a kink."|
				/ife (kinkRole == 'Giver') {:
					/setvar key=roleAwarenessInstruction "{{getvar::subjPronoun}} might impulsively try administering it when prompted or intrigued, but frames it as experimentation rather than kink."|
				:}|
				/elseif (kinkRole == 'Receiver') {:
					/setvar key=roleAwarenessInstruction "{{getvar::subjPronoun}} might accept or play along out of curiosity, treating it as novelty rather than kink."|
				:}|
				/else {:
					/setvar key=roleAwarenessInstruction "Whether giving or receiving, {{getvar::subjPronoun}} engages impulsively or experimentally, never as deliberate kink play."|
				:}|
			:}|
			/else {:
				/setvar key=awarenessExplanation "{{getvar::firstName}} is fully aware of this kink and may engage with intent, confidence, or emotional readiness."|
				/ife (kinkRole == 'Giver') {:
					/setvar key=roleAwarenessInstruction "When acting as Giver, {{getvar::subjPronoun}} may deliberately initiate or control the act, guided by trust or context."|
				:}|
				/elseif (kinkRole == 'Receiver') {:
					/setvar key=roleAwarenessInstruction "When acting as Receiver, {{getvar::subjPronoun}} may deliberately accept, anticipate, or respond to the act with conscious intent."|
				:}|
				/else {:
					/setvar key=roleAwarenessInstruction "As a Switch, {{getvar::subjPronoun}} may consciously engage from either side, adjusting between initiating and accepting depending on context."|
				:}|
			:}|
			/getvar key=sexualKinkDetails index={{var::index}}|
			/setvar key=kinkDetail {{pipe}}|
			/getvar key=sexualKinkEffects index={{var::index}}|
			/setvar key=kinkEffect {{pipe}}|
			/:"CMC Logic.GenerateWithPrompt"|
			/len {{var::tempOutputList}}|
			/var key=tempOutputList index={{pipe}} {{getvar::output}}|
			/flushvar output|
			/flushvar guidance|
			/flushvar kinkType|
			/flushvar kinkVariant|
			/flushvar kinkRole|
			/flushvar kinkDetail|
			/flushvar kinkEffect|
			/flushvar kinkCondition|
			/flushvar kinkRule|
			/flushvar animalisticAwarenessRule|
		:}|
		/foreach {{var::tempOutputList}} {:
			/addvar key={{var::variableName}} {{var::item}}|
		:}|
	:}|
	/else {:
		/:"CMC Logic.GenerateWithPrompt"|
		/setvar key={{var::variableName}} {{getvar::output}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|

/setvar key=parsedSexualKinks {{noop}}|
/ife (sexualKinkTypes is list) {:
	/foreach {{getvar::sexualKinkTypes}} {:
		/ife (index > 0) {:
			/addvar key=parsedSexualKinks "{{newline}}{{newline}}"|
		:}|
		/addvar key=parsedSexualKinks "- Kink Type: {{var::item}}"|
		/getvar key=sexualKinkVariants index={{var::index}}|
		/setvar key=kinkVariant {{pipe}}|
		/ife ((kinkVariant != '') and (kinkVariant != 'None')) {:
			/addvar key=parsedSexualKinks "{{newline}}  - Variant: {{getvar::kinkVariant}}"|
		:}|
		/getvar key=sexualKinkRoles index={{var::index}}|
		/addvar key=parsedSexualKinks "{{newline}}  - Role: {{pipe}}"|
		/getvar key=sexualKinkAwareness index={{var::index}}|
		/addvar key=parsedSexualKinks "{{newline}}  - Awareness: {{pipe}}"|
		/getvar key=sexualKinkDetails index={{var::index}}|
		/addvar key=parsedSexualKinks "{{newline}}  - Details: {{pipe}}"|
		/getvar key=sexualKinkEffects index={{var::index}}|
		/addvar key=parsedSexualKinks "{{newline}}  - Effect: {{pipe}}"|
		/getvar key=sexualKinkConditions index={{var::index}}|
		/setvar key=kinkCondition {{pipe}}|
		/ife ((kinkCondition != '') and (kinkCondition != 'None')) {:
			/addvar key=parsedSexualKinks "{{newline}}  - Conditions: {{getvar::kinkCondition}}"|
		:}|
	:}|
	/flushvar kinkVariant|
	/flushvar kinkRole|
	/flushvar kinkDetail|
	/flushvar kinkEffect|
	/flushvar kinkCondition|
:}|
/else {{:
	/setvar key=parsedSexualKinks None|
:}}|
//--------|
/addvar key=dataBaseNames parsedSexualKinks|


//Abilities|
/var key=do No|
/var key=variableName "sexualAbilityNames"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Ability Names"|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=needOutput No|
	/setvar key=genSettings index=outputIsList Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Abilities must be fully plausible in the real world. This includes advanced flexibility, sensory focus, pain tolerance, emotional control, or exceptional training. Do not include magic, psionics, supernatural phenomena, or any kind of proficiency level."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Abilities may include elemental powers, curses, divine traits, inherited magic, or arcane disciplines. Do not include tiers, mastery labels, or strength modifiers—those are handled in a later step."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Abilities may include psionics, gene traits, mental enhancements, or biotech-integrated skills. Do not include levels, size indicators, or parenthetical ranks—those will be generated separately."|
		
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
		/foreach {{getvar::CHANGE/REMOVE_THIS}} {:
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|


/getvar key=sexualAbilityNames index=0|
/var key=do {{pipe}}|
/ife ((do != '') and (do != 'None')) {:
	/var key=do No|
	/var key=variableName "sexualAbilityProficiencies"|
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
		/setvar key=genSettings index=wi_book_key "Sexual Ability Proficiency"|
		/setvar key=genSettings index=genIsList Yes|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence No|
		/setvar key=genSettings index=needOutput No|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/addvar key=extra "- Backstory: {{getvar::backstory}}"|
		/ife (user == 'Yes') {:
			/addvar key=extra "- User's Role: {{getvar::userRole}}"|
		:}|
		/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
		/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}} (do not directly turn into ability names; use only as influence)"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		
		/ife (settingType == 'Realistic') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Use realistic mastery tiers, physical control states, or measured performance levels. Do not use magical, tech-based, or mystical states."|
			
		:}|
		/elseif (settingType == 'Fantasy') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Use magical resonance levels, mystical awakenings, spell tiering, or enchanted conditions. You may use poetic or arcane phrasing."|
			
		:}|
		/elseif (settingType == 'Science Fiction') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Use mutation stages, neural tiers, cybernetic activation levels, or psionic charge states. Do not include divine, magical, or elemental qualifiers."|
			
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
			/foreach {{getvar::sexualAbilityNames}} {:
				/setvar key=abilityName {{var::item}}|
				/:"CMC Logic.GenerateWithPrompt"|
				/addvar key={{var::variableName}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/flushvar abilityName|
		:}|
		/else {:
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookSexualKinks index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookSexualKinks index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
	
	/setvar key=sexualAbilityNamesProficiencies []|
	/foreach  {{getvar::sexualAbilityNames}} {:
		/getvar key=sexualAbilityProficiencies index={{var::index}}|
		/let key=prof {{pipe}}|
		/ife (( prof != '') and ( prof != 'None')) {:
			/addvar key=sexualAbilityNamesProficiencies "{{var::item}} ({{var::prof}})"|
		:}|
		/else {:
			/addvar key=sexualAbilityNamesProficiencies "{{var::item}}"|
		:}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
	:}|
	
	/addvar key=dataBaseNames sexualAbilityNamesProficiencies|
	
	/var key=do No|
	/var key=variableName "SexualAbilityDetails"|
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
		/setvar key=genSettings index=wi_book_key "Sexual Ability Details"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence yes|
		/setvar key=genSettings index=needOutput yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/addvar key=extra "- Backstory: {{getvar::backstory}}"|
		/ife (user == 'Yes') {:
			/addvar key=extra "- User's Role: {{getvar::userRole}}"|
		:}|
		/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
		/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		
		/ife (settingType == 'Realistic') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Description must reflect real-world logic and be physically or psychologically plausible. Do not reference magic, tech, or supernatural forces."|
			
		:}|
		/elseif (settingType == 'Fantasy') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- You may include references to mana, magic, curses, bloodlines, or mystic energies. Abilities may scale dramatically between levels."|
			
		:}|
		/elseif (settingType == 'Science Fiction') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- You may reference cybernetic processes, psionic channels, tech-enhanced cognition, or biotech-based traits. Avoid magical concepts."|
			
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
			/foreach {{getvar::sexualAbilityNamesProficiencies}} {:
				/setvar key=abilityName {{var::item}}|
				/:"CMC Logic.GenerateWithPrompt"|
				/addvar key={{var::variableName}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/flushvar abilityName|
		:}|
		/else {:
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookSexualKinks index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookSexualKinks index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookSexualKinks index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualKinks index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
:}|
/else {:
	/setvar key=sexualAbilityNames None|
	/addvar key=dataBaseNames sexualAbilityNames|
	/setvar key=sexualAbilityProficiencies None|
	/addvar key=dataBaseNames sexualAbilityProficiencies|
	/setvar key=sexualAbilityNamesProficiencies None|
	/addvar key=dataBaseNames sexualAbilityNamesProficiencies|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index=sexualAbilityNames|
		/setvar key=tempLore {{pipe}}|
		/getvar key=sexualAbilityNames|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index=sexualAbilityNames {{getvar::tempLore}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index=sexualAbilityProficiencies|
		/setvar key=tempLore {{pipe}}|
		/getvar key=sexualAbilityProficiencies|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index=sexualAbilityProficiencies {{getvar::tempLore}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualKinks index=sexualAbilityNamesProficiencies|
		/setvar key=tempLore {{pipe}}|
		/getvar key=sexualAbilityNamesProficiencies|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualKinks index=sexualAbilityNamesProficiencies {{getvar::tempLore}}|
	:}|
:}|


/ife ((sexualAbilityNames != 'None') and (sexualAbilityNamesProficiencies is list)) {:
	/setvar key=parsedSexualAbilities []|
	/foreach {{getvar::sexualAbilityNamesProficiencies}} {:
		/let key=trait {{var::item}}|
		/getvar key=SexualAbilityDetails index={{var::index}}|
		/let key=deta {{pipe}}|
		/findentry field=comment file="CMC Templates" "Abilities Template"|
		/getentryfield field=content file="CMC Templates" {{pipe}}|
		/re-replace find="/--Ability--/g" replace="{{var::item}}" {{pipe}}|
		/re-replace find="/--Description--/g" replace="{{var::deta}}" {{pipe}}|
		/addvar key=parsedSexualAbilities {{pipe}}|
	:}|
	/join glue="{{newline}}{{newline}}" {{getvar::parsedSexualAbilities}}|
	/setvar key=parsedSexualAbilities {{pipe}}|
:}|
/else {:
	/setvar key=parsedSexualAbilities None|
:}|
/addvar key=dataBaseNames parsedSexualAbilities|
//--------|


//SEXUAL EXPERIENCE PROFILE|

//Experience Level|
/var key=do No|
/var key=variableName "sexualExperienceLevel"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Experience Level"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
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
//--------|

//Knowledge Level|
/var key=do No|
/var key=variableName "sexualKnowlageLevel"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Knowledge Level"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
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
//--------|

/ife ( (sexualExperienceLevel != 'None') and (sexualKnowlageLevel != 'None')) {:
	
	/ife (sexualExperienceLevel != 'None') {:
	//Number of Partners|
		/var key=do No|
		/var key=variableName "sexualPartners"|
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
			/setvar key=genSettings index=wi_book_key "Sexual Number of Partners"|
			/setvar key=genSettings index=combineLorebookEntries No|
			/setvar key=genSettings index=genIsSentence No|
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
	//--------|
	
	
	//Self-Exploration|
		/var key=do No|
		/var key=variableName "sexualSelfExploration"|
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
			/setvar key=genSettings index=wi_book_key "Sexual Self-Exploration"|
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
	//--------|
	
	//Exposure Context|
		/var key=do No|
		/var key=variableName "sexualExposure"|
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
			/setvar key=genSettings index=wi_book_key "Exposure Context"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=genAmount 8|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
			/addvar key=extra "- Time Period: {{getvar::timePeriod}}"|
			/addvar key=extra "- World Type: {{getvar::worldType}}"|
			/addvar key=extra "- Backstory: {{getvar::backstory}}"|
			/addvar key=extra "- Social Behavior: {{getvar::personalitySocialBehavior}}"|
			/ife (personalitySocialSkills != 'None') {:
				/addvar key=extra "- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}"|
			:}|
			/addvar key=extra "- Sexual Orientation: {{getvar::sexualOrientation}}"|
			/addvar key=extra "- Sexual Role: {{getvar::sexualRole}}"|
			/addvar key=extra "- Libido: {{getvar::libido}}"|
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
			
			/ife (variable == 'content') {:
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
		
			/setvar key=genSettings index=buttonPrompt "Is this the sexual exposure you want {{getvar::firstName}} to have"|
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
	
	:}|
	/elseif ((sexualExperienceLevel == 'None') and (sexualKnowlageLevel == 'None')) {:
		/setvar key=sexualPartners "0"|
		/addvar key=dataBaseNames sexualPartners|
		/setvar key=sexualSelfExploration "None"|
		/addvar key=dataBaseNames sexualSelfExploration|
		
	:}|
:}|
/elseif ((sexualExperienceLevel == 'None') and (sexualKnowlageLevel == 'None')) {:
	
	/setvar key=sexualExposure "None"|
	/addvar key=dataBaseNames sexualExposure|
	/setvar key=sexualPartners "0"|
	/addvar key=dataBaseNames sexualPartners|
	/setvar key=sexualSelfExploration "None"|
	/addvar key=dataBaseNames sexualSelfExploration|
	
:}|

/setvar key=nameList ["sexualFamiliarityActKissing", "sexualFamiliarityActOralR", "sexualFamiliarityActOralG", "sexualFamiliarityActVaginal", "sexualFamiliarityActAnal", "sexualFamiliarityActGroupSex", "sexualFamiliarityActToys"]|
/setvar key=nameListN ["Familiarity with Kissing", "Familiarity with reciving Oral sex", "Familiarity with giving Oral sex", "Familiarity with Vaginal sex", "Familiarity with Anal sex", "Familiarity with Group Sex", "Familiarity with using Sex Toys"]|

/ife ( (sexualExperienceLevel != 'None') and (sexualKnowlageLevel != 'None')) {:

//Familiarity With Sexual Acts|

	
	/foreach {{getvar::nameList}} {:
		/var key=do No|
		/var key=variableName "{{var::item}}"|
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
			/setvar key=genSettings index=wi_book_key "Familiarity With Sexual Acts"|
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
			
			
			
			/getvar key=nameListN index={{var::index}}|
			/setvar key=it {{pipe}}|
			/setvar key=genSettings index=buttonPrompt "Select the {{getvar::it}} you want {{getvar::firstName}} to have."|
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

//--------|
:}|
/elseif ((sexualExperienceLevel == 'None') and (sexualKnowlageLevel == 'None')) {:
	/foreach {{getvar::nameList}} {:
		/setvar key={{var::item}} "Unfamiliar – Has no knowledge or exposure."|
		/addvar key=dataBaseNames {{var::item}}|
	:}|
	
:}|
//--------|

/setvar key=parcedFamiliarity {{noop}}|
/foreach {{getvar::nameList}} {:
	/getvar key=nameListN index={{var::index}}|
	/let key=N {{pipe}}|
	/getvar key={{var::item}}|
	/let key=L {{pipe}}|
	/ife (parcedFamiliarity != '') {:
		/addvar key=parcedFamiliarity {{newline}}|
	:}|
	/addvar key=parcedFamiliarity {{var::N}}: {{var::L}}|
:}|

//Emotional Framing|
/var key=do No|
/var key=variableName "sexualFraming"|
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
	/setvar key=genSettings index=wi_book_key "Emotional Framing"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/addvar key=extra "- Social Behavior: {{getvar::personalitySocialBehavior}}"|
	/ife (personalitySocialSkills != 'None') {:
		/addvar key=extra "- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}"|
	:}|
	/addvar key=extra "- Sexual Orientation: {{getvar::sexualOrientation}}"|
	/addvar key=extra "- Sexual Role: {{getvar::sexualRole}}"|
	/addvar key=extra "- Libido: {{getvar::libido}}"|
	/addvar key=extra "{{getvar::parcedFamiliarity}}"|
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

	/setvar key=genSettings index=buttonPrompt Is this the Emotional Framing you want {{getvar::firstName}} should have?|

	
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

//Attitude Toward Sex|
/var key=do No|
/var key=variableName "sexualAttitude"|
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
	/setvar key=genSettings index=wi_book_key "Attitude Toward Sex"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/addvar key=extra "- Social Behavior: {{getvar::personalitySocialBehavior}}"|
	/ife (personalitySocialSkills != 'None') {:
		/addvar key=extra "- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}"|
	:}|
	/addvar key=extra "- Sexual Orientation: {{getvar::sexualOrientation}}"|
	/addvar key=extra "- Sexual Role: {{getvar::sexualRole}}"|
	/addvar key=extra "- Libido: {{getvar::libido}}"|
	/addvar key=extra "{{getvar::parcedFamiliarity}}"|
	/addvar key=extra "- Emotional Framing: {{getvar::sexualFraming}}"|
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

	/setvar key=genSettings index=buttonPrompt Is this the Emotional Framing you want {{getvar::firstName}} should have?|

	
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

//Behavior During Intimacy|
/var key=do No|
/var key=variableName "sexualBehavior"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Behavior"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genAmount 8|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
	/setvar key=genSettings index=needOutput Yes|
	/setvar key=genSettings index=outputIsList No|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/addvar key=extra "- Social Behavior: {{getvar::personalitySocialBehavior}}"|
	/ife (personalitySocialSkills != 'None') {:
		/addvar key=extra "- Social Skills and Integration Into Society: {{getvar::personalitySocialSkills}}"|
	:}|
	/addvar key=extra "- Sexual Orientation: {{getvar::sexualOrientation}}"|
	/addvar key=extra "- Sexual Role: {{getvar::sexualRole}}"|
	/addvar key=extra "- Libido: {{getvar::libido}}"|
	/addvar key=extra "{{getvar::parcedFamiliarity}}"|
	/addvar key=extra "- Emotional Framing: {{getvar::sexualFraming}}"|
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

	/setvar key=genSettings index=buttonPrompt Is this the Emotional Framing you want {{getvar::firstName}} should have?|

	
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


//Items / Equipment|
/var key=do No|
/var key=variableName "sexualItemNames"|
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
	/setvar key=genSettings index=wi_book_key "Sexual Item or Equipment Names"|
	/setvar key=genSettings index=genIsList Yes|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=needOutput No|
	/setvar key=genSettings index=outputIsList Yes|
	/setvar key=genSettings index=useContext Yes|
	/setvar key=extra []|
	/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
	/addvar key=extra "- Backstory: {{getvar::backstory}}"|
	/ife (user == 'Yes') {:
		/addvar key=extra "- User's Role: {{getvar::userRole}}"|
	:}|
	/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
	/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Limit items to real-world modern gear, accessories, or everyday personal objects. Do not include magic, advanced tech, or fantasy items."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Items may include magical trinkets, mystical gear, herbal components, talismans, or medieval-style tools and charms."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Items may include advanced tools, nanotech, biotech devices, psionic accessories, or gear with augmented properties."|
		
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
		/foreach {{getvar::CHANGE/REMOVE_THIS}} {:
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
		/getvar key=loreBookSexualItems index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualItems index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookSexualItems index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualItems index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|


/getvar key=sexualItemNames index=0|
/var key=do {{pipe}}|
/ife ((do != '') and (do != 'None')) {:
	/var key=do No|
	/var key=variableName "sexualItemDetails"|
	/len {{getvar::sexualItemDetails}}|
	/let key=lenSexualItemDetails {{pipe}}|
	/len {{getvar::sexualItemNames}}|
	/let key=lenSexualItemNames {{pipe}}|
	/ife (({{var::variableName}} == '') or (lenSexualItemDetails < lenSexualItemNames)) {:
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
		/setvar key=genSettings index=wi_book_key "Sexual Item or Equipment Description"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList Yes|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext Yes|
		/setvar key=extra []|
		/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
		/addvar key=extra "- Backstory: {{getvar::backstory}}"|
		/ife (user == 'Yes') {:
			/addvar key=extra "- User's Role: {{getvar::userRole}}"|
		:}|
		/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
		/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
		/setvar key=genSettings index=extraContext {{getvar::extra}}|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
		
		/setvar key=logicBasedInstruction {{noop}}|
		
		
		/ife (settingType == 'Realistic') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- Do not include magical, advanced tech, or psionic properties. Focus on grounded, everyday materials and wear."|
			
		:}|
		/elseif (settingType == 'Fantasy') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- You may reference glowing runes, magical engravings, spiritual symbols, or arcane materials—but avoid lore or spell explanations."|
			
		:}|
		/elseif (settingType == 'Science Fiction') {:
			
			/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
			/addvar key=logicBasedInstruction "- You may reference interfaces, synth materials, nanotech casings, and embedded circuitry—but avoid system-level tech detail or exposition."|
			
		:}|
		
		
		
		
		/getvar key=genSettings index=inputIsList|
		/let key=inputIsList {{pipe}}|
		/getvar key=genSettings index=inputIsList|
		/let key=outputIsList {{pipe}}|
		
		/let key=redoDetails Yes|
		/ife (lenSexualItemDetails > 0) {:
			/buttons labels=["Yes", "No"] Want to remake all item details?|
			/var key=redoDetails {{pipe}}|
			/ife (redoDetails == '') {:
		        /echo Aborting |
		        /abort
		    :}|
		:}|
		
		/let key=startIndex 0|
		/ife (((inputIsList == 'Yes') and (redoDetails == 'Yes')) or ((outputIsList == 'Yes') and (redoDetails == 'Yes'))) {:
			/setvar as=array key={{var::variableName}} []|
			/var key=startIndex 0|
		:}|
		/elseif (((inputIsList == 'Yes') and (redoDetails == 'No')) or ((outputIsList == 'Yes') and (redoDetails == 'No'))) {:
			/var key=startIndex {{var::lenSexualItemDetails}}|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/foreach {{getvar::sexualItemNames}} {:
				/ife (index >= startIndex) {:
					/setvar key=itemName {{var::item}}|
					/:"CMC Logic.GenerateWithPrompt"|
					/addvar key={{var::variableName}} {{getvar::output}}|
					/flushvar output|
					/flushvar guidance|
				:}|
			:}|
			/flushvar itemName|
		:}|
		/else {:
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookSexualItems index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualItems index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookSexualItems index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookSexualItems index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
:}|
/else {:
	/setvar key=sexualItemNames None|
	/addvar key=dataBaseNames sexualItemNames|
	/setvar key=sexualItemDetails None|
	/addvar key=dataBaseNames sexualItemDetails|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualItems index=sexualItemNames|
		/setvar key=tempLore {{pipe}}|
		/getvar key=sexualItemNames|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualItems index=sexualItemNames {{getvar::tempLore}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookSexualItems index=sexualItemDetails|
		/setvar key=tempLore {{pipe}}|
		/getvar key=sexualItemDetails|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookSexualItems index=sexualItemDetails {{getvar::tempLore}}|
	:}|
:}|


/ife (sexualItemNames != 'None') {:
	/setvar key=parsedSexualItems []|
	/foreach {{getvar::sexualItemNames}} {:
		/let key=trait {{var::item}}|
		/getvar key=sexualItemDetails index={{var::index}}|
		/let key=deta {{pipe}}|
		/findentry field=comment file="CMC Templates" "Item or Equipment Template"|
		/getentryfield field=content file="CMC Templates" {{pipe}}|
		/re-replace find="/--Item--/g" replace="{{var::item}}" {{pipe}}|
		/re-replace find="/--Details--/g" replace="{{var::deta}}" {{pipe}}|
		/addvar key=parsedSexualItems {{pipe}}|
	:}|
	/join glue="{{newline}}{{newline}}" {{getvar::parsedSexualItems}}|
	/setvar key=parsedSexualItems {{pipe}}|
:}|
/else {:
	/setvar key=parsedSexualItems None|
:}|
/addvar key=dataBaseNames parsedSexualItems|
//--------|

//Sexual Notes|

/len {{getvar::sexualNotes}}|
/let key=len {{pipe}}|

/var key=do No|
/var key=variableName "sexualNotes"|
/ife (({{var::variableName}} == '') or (len <= 5)) {:
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
	/setvar key=genSettings index=wi_book "CMC Rules"|
	/setvar key=genSettings index=combineLorebookEntries No|
	/setvar key=genSettings index=genIsSentence No|
	/setvar key=genSettings index=inputIsList No|
	/setvar key=genSettings index=genIsList No|
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
	
	
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	
	/ife ( len == 0) {:
		/setvar key={{var::variableName}} []|
		/setvar key=genSettings index=wi_book_key "Initiation Style"|
		/setvar key=genSettings index=buttonPrompt "Select the Initiation Style you want {{getvar::firstName}} to follow."|
		/setvar key=it Behavior Rules|
		/:"CMC Logic.GenerateWithSelector"|
		/addvar key={{var::variableName}} "**Initiation Style:** {{getvar::output}}"|
	:}|
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	/ife ( len == 1) {:
		/setvar key=genSettings index=wi_book_key "Touch Preference"|
		/setvar key=genSettings index=buttonPrompt "Select the Touch Preference you want {{getvar::firstName}} to follow."|
		/setvar key=it Behavior Rules|
		/:"CMC Logic.GenerateWithSelector"|
		/addvar key={{var::variableName}} "**Touch Preference:** {{getvar::output}}"|
	:}|
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	/ife ( len == 2) {:
		/setvar key=genSettings index=wi_book_key "Verbal Tone During Intimacy"|
		/setvar key=genSettings index=buttonPrompt "Select the Verbal Tone During Intimacy you want {{getvar::firstName}} to follow."|
		/setvar key=it Behavior Rules|
		/:"CMC Logic.GenerateWithSelector"|
		/addvar key={{var::variableName}} "**Verbal Tone During Intimacy:** {{getvar::output}}"|
	:}|
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	/ife ( len == 3) {:
		/setvar key=genSettings index=wi_book_key "Emotional Layer"|
		/setvar key=genSettings index=buttonPrompt "Select the Emotional Layer you want {{getvar::firstName}} to follow."|
		/setvar key=it Behavior Rules|
		/:"CMC Logic.GenerateWithSelector"|
		/addvar key={{var::variableName}} "**Emotional Layer:** {{getvar::output}}"|
	:}|
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	/ife ( len == 4) {:
		/setvar key=genSettings index=wi_book_key "Control Preference"|
		/setvar key=genSettings index=buttonPrompt "Select the Control Preference (General, not role-linked) you want {{getvar::firstName}} to follow."|
		/setvar key=it Behavior Rules|
		/:"CMC Logic.GenerateWithSelector"|
		/addvar key={{var::variableName}} "**Control Preference (General, not role-linked):** {{getvar::output}}"|
	:}|
	/len {{getvar::sexualNotes}}|
	/var key=len {{pipe}}|
	/ife ( len >= 5) {:
		/var key=do No|
		/buttons labels=["Yes", "No"] Do you want to generate or add more sexual Rules?|
		/var key=do {{pipe}}|
		/ife (do == '') {:
			/echo Aborting |
			/abort
		:}|
		/ife (do == 'Yes') {:
			/setvar key=genSettings {}|
			/setvar key=genSettings index=wi_book_key "Sexual Notes"|
			/setvar key=genSettings index=genIsList No|
			/setvar key=genSettings index=inputIsList No|
			/setvar key=genSettings index=genIsSentence Yes|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList Yes|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/addvar key=extra "- Setting Type: {{getvar::settingType}}"|
			/addvar key=extra "- Main Personality Trait: {{getvar::personalityMainTrait}}"| 
			/addvar key=extra "- Personality Tags: {{getvar::personalityFoundTags}}, {{getvar::personalityTags}}"|
			/addvar key=extra "{{getvar::parsedSexualOrientation}}"|
			/addvar key=extra "{{getvar::parsedSexualRole}}"|
			/addvar key=extra "- Libido: {{getvar::sexualLibido}}"|
			/ife (parsedSexualKinks != 'None') {:
				/addvar key=extra "{{getvar::parsedSexualKinks}}"|
			:}|
			/ife (parsedSexualItems != 'None') {:
				/addvar key=extra "{{getvar::parsedSexualItems}}"|
			:}|
			/ife (parsedSexualAbilities != 'None') {:
				/addvar key=extra "{{getvar::parsedSexualAbilities}}"|
			:}|
			/setvar key=genSettings index=extraContext {{getvar::extra}}|
			/setvar key=extra []|
			/:"CMC Logic.Get Basic Type Context"|
			/ife (extra != '') {:
				/setvar key=genSettings index=contextKey {{getvar::extra}}|
			:}|
			/flushvar extra|
			/wait {{getvar::wait}}|
			/setvar key=genSettings index=buttonPrompt "Is this a good behavior rule you want {{getvar::firstName}} to follow?"|
			/setvar key=it Behavior Rules|
			/setvar key=blackListGen {{noop}}|
			/:"CMC Logic.GenerateWithPrompt"|
			/foreach {{getvar::output}} {:
				/addvar key={{var::variableName}} "{{var::output}}"|
			:}|
		:}|
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


/setvar key=parsedSexualityNotes {{noop}}|
/ife (sexualNotes is list) {:
	/foreach {{getvar::sexualNotes}} {:
		/ife ('None' not in item) {:
			/ife (index > 0) {:
				/addvar key=parsedSexualityNotes {{newline}}|
			:}|
			/addvar key=parsedSexualityNotes "- {{var::item}}"|
		:}|
	:}|
:}|
//--------|


/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Extras" {{pipe}}|
/forcesave|