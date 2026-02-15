/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating External Interaction" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step8|

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

//Connections|
/var key=do No|
/var key=variableName "connections"|
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
	/setvar key=genSettings index=wi_book_key "Connections"|
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
	
	/setvar key=logicBasedInstruction {{noop}}|
	
	/ife (user == 'Yes') {:
		
		/ife ( logicBasedInstruction == '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- If a connection is --User--, write their name **exactly** as `--User--` with no surname."|
		
		/ife ( logicBasedInstruction == '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use only valid relationships to {{getvar::firstName}} (e.g., `Friend of {{getvar::firstName}}`, `Mentor of {{getvar::firstName}}`, etc.)."|
		
		/ife ( logicBasedInstruction == '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Do **not** add a full name, inferred gender, or descriptive label like “Andersson” — --User-- must stay anonymous in all contexts."|
	:}|
	/ife ((characterArchetype == 'Human') or (characterArchetype == 'Android') or (characterArchetype == 'Beastkin') or (characterArchetype == 'Mythfolk')) {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use standard human familial terms (e.g., “mother,” “foster brother”) unless species logic overrides this."|
		
	:}|
	/elseif (characterArchetype == 'Anthropomorphic') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Default to human-style terms, but allow species-aligned roles (e.g., “tribe-guardian,” “clan-sibling”) when fitting."|
		
	:}|
	/else {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use species-specific kin or pack structures (e.g., “clutchmate,” “herd elder,” “pack alpha”) and avoid human roles unless clearly justified."|
		
	:}|
	
	/ife (settingType == 'Realistic') {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Default to conventional human familial and social roles unless characterArchetype explicitly requires otherwise."|
		
	:}|
	/else {:
		
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Use culturally and biologically appropriate kinship structures (e.g., “soul-bonded trainer,” “unit sibling,” “a bonded mage-companion”) based on species or origin."|
		
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

//Abilities|
/var key=do No|
/var key=variableName "abilityNames"|
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
	/setvar key=genSettings index=wi_book_key "Ability Names"|
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
	/addvar key=extra "{{getvar::parsedOccupation}}"|
	/addvar key=extra "- Scenario Overview: {{getvar::scenarioOverview}}"|
	/setvar key=genSettings index=extraContext {{getvar::extra}}|
	/setvar key=extra []|
	/:"CMC Logic.Get Basic Type Context"|
	/ife (extra != '') {:
		/setvar key=genSettings index=contextKey {{getvar::extra}}|
	:}|
	/flushvar extra|
	/wait {{getvar::wait}}|
	
	/setvar key=logicBasedInstruction {{noop}}|
	/setvar key=settingRule {{noop}}|
	/ife (settingType == 'Realistic') {:
		/setvar key=settingRule "grounded physical, psychological, or sensory traits"|
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Abilities must be fully plausible in the real world. This includes advanced flexibility, sensory focus, pain tolerance, emotional control, or exceptional training. Do not include magic, psionics, supernatural phenomena, or any kind of proficiency level."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		/setvar key=settingRule "magical, divine, inherited, or mystical traits"|
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Abilities may include elemental powers, curses, divine traits, inherited magic, or arcane disciplines. Do not include tiers, mastery labels, or strength modifiers—those are handled in a later step."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		/setvar key=settingRule "psionic, cybernetic, gene-modified, or tech-integrated traits"|
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
	/addvar key=dataBaseNames {{var::variableName}}|
	/flushvar output|
	/flushvar guidance|
	/flushvar genOrder|
	/flushvar genContent|
	/flushvar genSettings|
	/flushvar settingRule|
:}|
/else {:
	/addvar key=dataBaseNames {{var::variableName}}|
:}|


/getvar key=abilityNames index=0|
/var key=do {{pipe}}|
/ife ((do != '') and (do != 'None')) {:
	/var key=do No|
	/var key=variableName "abilityProficiencies"|
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
		/setvar key=genSettings index=wi_book_key "Ability Proficiency"|
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
		/addvar key=extra "{{getvar::parsedOccupation}}"|
	/addvar key=extra "- Scenario Overview: {{getvar::scenarioOverview}}"|
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
			/foreach {{getvar::abilityNames}} {:
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
	
	/setvar key=abilityNamesProficiencies []|
	/foreach  {{getvar::abilityNames}} {:
		/getvar key=abilityProficiencies index={{var::index}}|
		/let key=prof {{pipe}}|
		/ife (( prof != '') and ( prof != 'None')) {:
			/addvar key=abilityNamesProficiencies "{{var::item}} ({{var::prof}})"|
		:}|
		/else {:
			/addvar key=abilityNamesProficiencies "{{var::item}}"|
		:}|
	:}|
	/addvar key=dataBaseNames abilityNamesProficiencies|
	
	/var key=do No|
	/var key=variableName "abilityDetails"|
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
		/setvar key=genSettings index=wi_book_key "Ability Details"|
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
		/addvar key=extra "{{getvar::parsedOccupation}}"|
	/addvar key=extra "- Scenario Overview: {{getvar::scenarioOverview}}"|
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
			/foreach {{getvar::abilityNamesProficiencies}} {:
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
	/setvar key=abilityNames None|
	/addvar key=dataBaseNames abilityNames|
	/setvar key=abilityProficiencies None|
	/addvar key=dataBaseNames abilityProficiencies|
	/setvar key=abilityNamesProficiencies None|
	/addvar key=dataBaseNames abilityNamesProficiencies|
:}|


/ife (abilityNames != 'None') {:
	/setvar key=parsedAbilities []|
	/foreach {{getvar::abilityNamesProficiencies}} {:
		/let key=trait {{var::item}}|
		/getvar key=abilityDetails index={{var::index}}|
		/let key=deta {{pipe}}|
		/findentry field=comment file="CMC Templates" "Abilities Template"|
		/getentryfield field=content file="CMC Templates" {{pipe}}|
		/re-replace find="/--Ability--/g" replace="{{var::item}}" {{pipe}}|
		/re-replace find="/--Description--/g" replace="{{var::deta}}" {{pipe}}|
		/addvar key=parsedAbilities {{pipe}}|
	:}|
	/join glue="{{newline}}{{newline}}" {{getvar::parsedAbilities}}|
	/setvar key=parsedAbilities {{pipe}}|
:}|
/else {:
	/setvar key=parsedAbilities None|
:}|
/addvar key=dataBaseNames parsedAbilities|
//--------|

//Items / Equipment|
/var key=do No|
/var key=variableName "itemNames"|
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
	/setvar key=genSettings index=wi_book_key "Item or Equipment Names"|
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
	/addvar key=extra "{{getvar::parsedOccupation}}"|
	/addvar key=extra "- Scenario Overview: {{getvar::scenarioOverview}}"|
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
		/setvar key=settingRule "practical, everyday tools or personal effects"|
:}|
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Limit items to real-world modern gear, accessories, or everyday personal objects. Do not include magic, advanced tech, or fantasy items."|
		
	:}|
	/elseif (settingType == 'Fantasy') {:
		/setvar key=settingRule "charms, relics, satchels, tomes, minor weapons, or magical trinkets"|
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Items may include magical trinkets, mystical gear, herbal components, talismans, or medieval-style tools and charms."|
		
	:}|
	/elseif (settingType == 'Science Fiction') {:
		 /setvar key=settingRule "tech tools, augmentations, energy items, or utility gear"|
		/ife ( logicBasedInstruction != '') {:
			/addvar key=logicBasedInstruction {{newline}}|
		:}|
		/addvar key=logicBasedInstruction "- Items may include advanced tools, nanotech, biotech devices, psionic accessories, or gear with augmented properties."|
		
	:}|
	/ife ((characterArchetype == 'Anthropomorphic') or (characterArchetype == 'Beastkin')) {:
		  /setvar key=logicBasedInstruction "- Items should be human-based in function and design, reflecting clothing, tools, or accessories that an anthropomorphic or beastkin character would logically carry."|
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
		/getvar key=loreBookItems index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookItems index={{var::variableName}} {{getvar::tempLore}}|
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
		/getvar key=loreBookItems index={{var::variableName}}|
		/setvar key=tempLore {{pipe}}|
		/getvar key={{var::variableName}}|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookItems index={{var::variableName}} {{getvar::tempLore}}|
	:}|
:}|


/getvar key=itemNames index=0|
/var key=do {{pipe}}|
/ife ((do != '') and (do != 'None')) {:
	/var key=do No|
	/var key=variableName "itemDetails"|
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
		/setvar key=genSettings index=wi_book_key "Item or Equipment Description"|
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
		/addvar key=extra "{{getvar::parsedOccupation}}"|
	/addvar key=extra "- Scenario Overview: {{getvar::scenarioOverview}}"|
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
		
		
		/ife ((inputIsList == 'Yes') or (outputIsList == 'Yes')) {:
			/setvar as=array key={{var::variableName}} []|
		:}|
		/else {:
			/setvar as=string key={{var::variableName}} {{noop}}|
		:}|
		//[[Generate with Prompt]]|
		/ife (inputIsList == 'Yes') {:
			/foreach {{getvar::itemNames}} {:
				/setvar key=itemName {{var::item}}|
				/:"CMC Logic.GenerateWithPrompt"|
				/addvar key={{var::variableName}} {{getvar::output}}|
				/flushvar output|
				/flushvar guidance|
			:}|
			/flushvar itemName|
		:}|
		/else {:
			/:"CMC Logic.GenerateWithPrompt"|
			/setvar key={{var::variableName}} {{getvar::output}}|
		:}|
		
		/ife (makeLoreBook == 'Yes') {:
			/getvar key=loreBookItems index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookItems index={{var::variableName}} {{getvar::tempLore}}|
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
			/getvar key=loreBookItems index={{var::variableName}}|
			/setvar key=tempLore {{pipe}}|
			/getvar key={{var::variableName}}|
			/addvar key=tempLore {{pipe}}|
			/setvar key=loreBookItems index={{var::variableName}} {{getvar::tempLore}}|
		:}|
	:}|
:}|
/else {:
	/setvar key=itemNames None|
	/addvar key=dataBaseNames itemNames|
	/setvar key=itemDetails None|
	/addvar key=dataBaseNames itemDetails|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookItems index=itemNames|
		/setvar key=tempLore {{pipe}}|
		/getvar key=itemNames|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookItems index=itemNames {{getvar::tempLore}}|
	:}|
	
	/ife (makeLoreBook == 'Yes') {:
		/getvar key=loreBookItems index=itemDetails|
		/setvar key=tempLore {{pipe}}|
		/getvar key=itemDetails|
		/addvar key=tempLore {{pipe}}|
		/setvar key=loreBookItems index=itemDetails {{getvar::tempLore}}|
	:}|
:}|


/ife (itemNames != 'None') {:
	/setvar key=parsedItems []|
	/foreach {{getvar::itemNames}} {:
		/let key=trait {{var::item}}|
		/getvar key=itemDetails index={{var::index}}|
		/let key=deta {{pipe}}|
		/findentry field=comment file="CMC Templates" "Item or Equipment Template"|
		/getentryfield field=content file="CMC Templates" {{pipe}}|
		/re-replace find="/--Item--/g" replace="{{var::item}}" {{pipe}}|
		/re-replace find="/--Details--/g" replace="{{var::deta}}" {{pipe}}|
		/addvar key=parsedItems {{pipe}}|
	:}|
	/join glue="{{newline}}{{newline}}" {{getvar::parsedItems}}|
	/setvar key=parsedItems {{pipe}}|
:}|
/else {:
	/setvar key=parsedItems None|
:}|
/addvar key=dataBaseNames parsedItems|
//--------|

//Secrets|
/var key=do No|
/var key=variableName "secrets"|
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
	/setvar key=genSettings index=wi_book_key "Secrets"|
	/setvar key=genSettings index=genIsList No|
	/setvar key=genSettings index=genIsSentence Yes|
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
		/ife (parsedAbilities != None) {:
			/addvar key=extra "{{newline}}**Abilities**{{newline}}{{getvar::parsedAbilities}}"|
		:}|
		/ife (parsedAbilities != None) {:
			/addvar key=extra "{{newline}}**Items or Gear**{{newline}}{{getvar::parsedItems}}"|
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

/setvar key=parsedSecret None|
/ife (secrets != 'None') {:
	/setvar key=parsedSecret "### SECRET{{newline}}[IMPORTANT NOTE FOR AI: This section represents concealed internal truths about {{getvar::firstName}}. These secrets should not be stated directly in narration, dialog, or internal thoughts unless {{getvar::firstName}} is actively confronted, emotionally compromised, or chooses to reveal them. Secrets influence behavior, tone, and reactions—but remain hidden from others unless explicitly triggered in-scene.]{{newline}}"|
	/foreach {{getvar::secrets}} {:
		/addvar key=parsedSecret "{{newline}}{{var::item}}"
	:}|
:}|
/addvar key=dataBaseNames parsedSecret|


/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Sexual Information" {{pipe}}|
/forcesave|