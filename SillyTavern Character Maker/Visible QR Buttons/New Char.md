/popup <div>If you have problems with a prompt during generation you can</div><div>1. Try pressing the 'Generate New' button.</div>
<div>2. If the generation still poses problem you can try to Edit the prompt.</div>
<div>3. If you still have problem you can ask for help on Discord. Include what model you are using and what prompt you are having problem with</div>|

/let key=selected_btn {{noop}}|
/let key=databaseList {{noop}}|
/let key=qrList {{noop}}|
/let key=typeGuide {{noop}}|

/let key=branch {{noop}}|
/ife (beta != 'Yes') {:
	/var key=branch main|
:}|
/else {:
	/var key=branch Fetch-Files|
:}|

/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Version/Version.md |
/let key=updatedVersion {{pipe}}|

/let key=currentVersion {{noop}}|

/db-list source=character field=name |
/let key=a {{pipe}}|
/ife ('Current Version' in a) {:
	/db-get source=character  "Current Version"|
	/var key=currentVersion {{pipe}}|
:}|
/ife ((currentVersion == '') or (updatedVersion != currentVersion)) {:

	/buttons labels=["Yes", "No", "Changes"] <div>There is a new version.</div><div>Do you want to stop the script to update to the new version or see what's new?</div>|
	/var key=selected_btn {{pipe}}|
	/ife ( selected_btn == ''){:
		/echo Aborting |
		/abort
	:}|
	/ife (selected_btn == 'Changes') {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Version/Latest%20Update.md |
		/popup {{var::updatedVersion}}{{newline}}{{pipe}}|
	:}|
:}|

/len {{getglobalvar::promptOrder}}|
/let key=promptOrderLen {{pipe}}|
/ife ((promptOrder == '') or (promptOrderLen < 4)) {:
	/setglobalvar key=promptOrder ["contexts", "examples", "task", "instructions"]|
	/popup "Default order for the prompt set. If you want to change it press the 'CMC Menu' button.{{newline}}Default order: contexts, examples, task, instructions"|
:}|

/messages 0|
/let key=firstMess {{pipe}}|
/ife ( ('Installation Instructions' not in firstMess) and (continue != 'Yes')) {:
	/buttons labels=["Yes", "No"] <div>Doing this will delete all progress. And all Chat Attachments.</div><div>Do you want to continue?</div>|
	/var selected_btn {{pipe}}|
	/ife (( selected_btn == '') or ( selected_btn == 'No')) {:
		/echo Aborting|
		/abort|
	:}|
	//Empty the Database to prepare for the new character|
	/db-list source=chat field=name |
	/var key=databaseList {{pipe}}|
	/foreach {{var::databaseList}} {:
		/db-delete source=chat {{var::item}}|
	:}|
	
	/listvar scope=local return=object |
	/let  key=flvars {{pipe}}|
	/foreach {{var::flvars}} {:
		/getat index=key {{var::item}}|
		/flushvar {{pipe}}|
	:}|
	
:}|

/extension-exists SillyTavern-Variable-Viewer |
/let key=vV {{pipe}}|
/ife (vV == true) {:
	/buttons labels=["Enable", "Disable", "Skip"] <div>Do you want to enable debug mode?</div><div>This lets you check what the prompt sent to the LLM is.</div>|
	/let key=check {{pipe}}|
	/ife (check == '') {:
		/echo Aborting |
		/abort
	:}|
	/elseif (check == 'Enable') {:
		/setvar key=debug Yes|		
	:}|
	/elseif (check == 'Disable') {:
		/setvar key=debug No|		
	:}|
	/buttons labels=["Yes", "No"] Do you want to toggle the Variable Viewer window On/Off?|
		/let key=toggle {{pipe}}|
		/ife (toggle == '') {:
			/echo Aborting |
			/abort
		:}|
		/elseif (toggle == 'Yes') {:
			/variableviewer|
		:}|
:}|


/wi-list-books all=true|
/let key=wiList {{pipe}}|

/setvar key=lorebookList []|
/addvar key=lorebookList Anatomy|

/foreach {{getvar::lorebookList}} {:
	/setvar key=lore{{var::item}} "No"|
	/ife ('CMC {{var::item}}' in wiList) {:
		/buttons labels=["Yes", "No"] Do you want to use the optional {{var::item}} Lorebook?|
		/setvar key=lore{{var::item}} {{pipe}}|
		/ife (lore{{var::item}} == '') {:
			/echo Aborting |
			/abort
		:}|
	:}|
:}|

/setvar key=continue Yes|
/setvar key=wait 100|
/setvar key=stepDone No|
/setvar key=stepVar Step0|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Basic Information" {{pipe}}|

/swipes-count|
/let key=sw {{pipe}}|
/ife (sw > 1) {:
	/swipes-del 1|
:}|

/qr-list CMC Logic|
/var key=qrList {{pipe}} |
/setvar key=dataBaseNames []|
/var selected_btn {{noop}}|
/ife ( gender != '' ) {:
	/buttons labels=["Yes", "No"] Do you want to change the gender?|
	/var selected_btn {{pipe}}|
:}|
/ife ( (gender == '') or ( selected_btn == 'Yes')) {:
	/buttons labels=["Female", "Male"] What gender is the character you are making? |
	/setvar key=gender {{pipe}}|
	/ife ( gender == '') {:
		/echo Aborting |
		/abort
	:}|
:}|
/addvar key=dataBaseNames gender|


/var selected_btn {{noop}}|
/ife ( futanari != '' ) {:
	/buttons labels=["Yes", "No"] Do you want to change the futanari choice?|
	/var selected_btn {{pipe}}|
:}|
/ife ( (futanari == '') or ( selected_btn == 'Yes')) {:
	/buttons labels=["Yes", "No"] Is the character you are making a futanari? |
	/setvar key=futanari {{pipe}}|
	/ife ( gender == '') {:
		/echo Aborting |
		/abort
	:}|
:}|
/addvar key=dataBaseNames futanari|

/var selected_btn {{noop}}|
/ife ( characterArchetype != '' ) {:
	/buttons labels=["Yes", "No"] Do you want to change the type of character?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort|
	:}|
:}|
/ife ( (characterArchetype == '') or ( selected_btn == 'Yes')) {:
	/setvar key=characterArchetype "Help me Decide"|
	/findentry field=comment file="CMC Information {{getglobalvar::model}}" Type Guide|
	/getentryfield file="CMC Information {{getglobalvar::model}}" {{pipe}}| 
	/var typeGuide {{pipe}}|
	/whilee ( characterArchetype == 'Help me Decide') {:
		/buttons labels=["Help me Decide", "Human", "Anthropomorphic\n(Anthropomorphic is a character that combines both human and animal traits, often featuring an animal body with human-like posture, facial expressions, speech, and behavior.)", "Mythfolk\n(Mythfolk is races that mostly looks like humans like Dwarfs, Elves etc...)", "Tauric\n(Tauric are hybrid species with a humanoid upper body and an animal-like lower body, such as centaurs, lamias, and mermaids.)", "Beastkin\n(Beastkin is a character with animal features like ears and tail but otherwise human appearance.)", "Animalistic\n(Animalistic refers to standard animals, fantasy creatures, or monsters that behave and appear primarily as non-human beings, typically walking on all fours and lacking human speech or reasoning.)", "Pokémon", "Digimon", "Android\n(Android is a robot that looks and acts like a Human.)"] What type of character are you making? |
		/re-replace find="/(\n\()[\s\S]*$/g" replace="" {{pipe}}|
		/setvar key=characterArchetype {{pipe}}|
		/ife ( characterArchetype == ''){:
			/echo Aborting|
			/abort
		:}|
		/ife ( characterArchetype == 'Help me Decide' ){:
			/input rows=8 What race do you want the character to be?|
			/let key=inp {{pipe}}|
			/genraw as=char Respond to the question: What type of character is a {{var::inp}}?
The reply should be in this format:
'<div>{{getvar::inp}} is a x</div>'
x is one of the following "Human", "Anthropomorphic", "Mythfolk", "Tauric", "Beastkin", "Animalistic", "Pokémon", "Digimon", "Android"
INFORMATION: 
{{var::typeGuide}}
INSTRUCTION: Only respond in the given format.|

			/setvar key=characterArchetype {{pipe}}|
			/popup okButton=Continue result=true {{getvar::characterArchetype}}|
			/setvar key=characterArchetype {{pipe}}|
			/ife ( characterArchetype == '' ){:
				/echo Aborting |
				/abort
			:}|
			/elseif ( characterArchetype == '1' ){:
				/setvar key=characterArchetype "Help me Decide"|
			:}|
		:}|
	:}|
	/re-replace find="/\(.*$/g" replace="" {{getvar::characterArchetype}}|
	/setvar key=characterArchetype {{pipe}}|
:}|
/addvar key=dataBaseNames characterArchetype|


/var selected_btn {{noop}}|
/ife ( characterType != '' ) {:
	/buttons labels=["Yes", "No"] Do you want to change the character type?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
:}|
/ife ( (selected_btn == 'Yes') or (characterType == '')) {:
	/setvar key=characterType None|
:}|
/ife (((characterType == 'None') or ( selected_btn == 'Yes')) and (( characterArchetype == 'Anthropomorphic') or ( characterArchetype == 'Beastkin') or ( characterArchetype == 'Digimon') or ( characterArchetype == 'Pokémon'))) {:
	/buttons labels=["Pokémon", "Digimon", "Animalistic"] Select the type you want?|
	/setvar key=characterType {{pipe}}|
	/ife ( characterType == '') {:
		/echo Aborting|
		/abort
	:}|
:}|
/addvar key=dataBaseNames characterType|

/ife ((characterArchetype == 'Human') or (characterArchetype == 'Mythfolk') or (characterArchetype == 'Tauric') or (characterArchetype == 'Android')) {:
	/ife (characterArchetype == 'Android') {:
		/setvar key=characterType Human|
	:}|
	/else {:
		/setvar key=characterType {{getvar::characterArchetype}}|
	:}|
:}|


/ife ( (characterArchetype != 'Human') and (characterArchetype != 'Mythfolk') and (characterArchetype != 'Android')) {:
	/var selected_btn {{noop}}|
	/let key=anatomyTrue {{noop}}|
	/ife ('CMC Anatomy' in wiList) {:
		/var key=anatomyTrue " Needed if you want it to use the CMC Anatomy Lorebook"|
	:}|
	/ife ( animalBase != '' ) {:
		/buttons labels=["Yes", "No"] Do you want to change the animal base?{{var::anatomyTrue}}|
		/var selected_btn {{pipe}}|
		/ife ( selected_btn == '') {:
			/echo Aborting|
			/abort
		:}|
	:}|
	/ife ( ( selected_btn == 'Yes') or ( animalBase == '')) {:
		/buttons labels=["Mammal", "Reptile", "Bird", "Fish", "Amphibian", "Invertebrate", "Fantasy"] What type of species should the character be? This will guide later generations. |
		/re-replace find="/\(.*$/g" replace="" {{pipe}}|
		/setvar key=animalBase {{pipe}}|
		/ife ( animalBase == '') {:
			/echo Aborting|
			/abort
		:}|
	:}|
:}|
/elseif (characterArchetype == 'Human') {:
	/setvar key=animalBase Humanoid|
:}|
/elseif (characterArchetype == 'Android') {:
	/setvar key=animalBase Synthetic|
:}|
/else {:
	/setvar key=animalBase None|
:}|
/addvar key=dataBaseNames animalBase|

/ife ( (characterArchetype != 'Human') and (characterArchetype != 'Mythfolk') and (characterArchetype != 'Android')) {:
	/buttons labels=["Yes pick a specific category", "No"] Do you want the character to use the animal base as-is, or pick a more specific category like Canine, Feline, or Reptilian when generating the species later?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
	/elseif ( selected_btn == 'No') {:
		/setvar key=speciesGroup {{getvar::animalBase}}|
	:}|
	/else {:
		/let key=find {{getvar::animalBase}}: List|
		/findentry field=comment file="CMC Variables" {{var::find}}|
		/getentryfield field=content file="CMC Variables" {{pipe}}|
		/split find="---" {{pipe}} |
		/setvar key=speciesGroup {{pipe}}|
		/buttons labels={{getvar::speciesGroup}} Select the Species Group you want to use for later when generating the Species.|
		/setvar key=speciesGroup {{pipe}}|
		/ife ( speciesGroup == '') {:
			/echo Aborting|
			/abort
		:}|
		/re-replace find="/ \(.*$/g" replace="" {{getvar::speciesGroup}}|
		/setvar key=speciesGroup {{pipe}}|
	:}|
:}|
/else {:
	/setvar key=speciesGroup Humanoid|
:}|
/ife ( (characterArchetype == 'Human') and (characterArchetype == 'Mythfolk') and (characterArchetype == 'Android')) {:
	/setvar key=parsedAnimalType Humanoid|
:}|
/elseif (((characterArchetype == Anthropomorphic) or (characterArchetype == Anthropomorphic)) and ((characterType == 'Pokémon') or (characterType == 'Digimon') or (characterType == 'Animalistic'))) {:
	/setvar key=parsedAnimalType {{getvar::characterType}}|
:}|
/elseif (animalBase != speciesGroup) {:
	/setvar key=parsedAnimalType {{getvar::speciesGroup}}|
:}|
/else {:
	/setvar key=parsedAnimalType {{getvar::animalBase}}|
:}|

/var selected_btn {{noop}}|
/ife ((privatesMale != '') or (privatesFemale != '')) {:
	/buttons labels=["Yes", "No"] Do you want to change the Privates you selected?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
:}|
/ife (selected_btn != 'No') {:
	/buttons labels=["Yes", "No"] Do you want the character to have Privates that differs from it's species type?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
	/elseif ( selected_btn == 'Yes') {:
		/setvar key=privatesFemale {{noop}}|
		/setvar key=privatesMale {{noop}}|
		/setvar key=searchPrivatesFemale {{noop}}|
		/setvar key=searchPrivatesMale {{noop}}|
		/setvar key=loopGenitalia []|
		/ife (futanari == 'Yes') {:
			/addvar key=loopGenitalia Male|
			/addvar key=loopGenitalia Female|
		:}|
		/elseif (gender == 'Male') {:
			/addvar key=loopGenitalia Male|
		:}|
		/else {:
			/addvar key=loopGenitalia Female|
		:}|
		/foreach {{getvar::loopGenitalia}} {:
			/ife (privates{{var::item}} == '') {:
				/buttons labels=["Mammal", "Reptile", "Bird", "Fish", "Amphibian", "Invertebrate", "Fantasy"] <div>What type of species should the characters {{var::item}} Privates be? This will guide later generations.</div><div>Choosing Fantasy will use the selected species you chose during `Core Identity Generation`.</div>|
				/let key=t {{pipe}}|
				/ife (t == '') {:
					/echo Aborting |
					/abort
				:}|
				/elseif (t != 'Fantasy') {:
					/let key=find {{var::t}}: List|
					/findentry field=comment file="CMC Variables" {{var::find}}|
					/getentryfield field=content file="CMC Variables" {{pipe}}|
					/split find="---" {{pipe}} |
					/setvar key=temp1 {{pipe}}|
					/setvar key=temp {{getvar::temp1}}|
					/addvar key=temp Use Base Type|
					/buttons labels={{getvar::temp}} Select the Species Group you want to use for later when generating the {{var::item}} Privates.|
					/setvar key=temp {{pipe}}|
					/ife ( temp == '') {:
						/echo Aborting|
						/abort
					:}|
					/re-replace find="/\(.*$/g" replace="" {{getvar::temp}}|
					/setvar key=temp {{pipe}}|
					/ife ( temp == 'Use Base Type') {:
						/setvar key=temp {{getvar::animalBase}}|
					:}|
					/ife (futanari == 'Yes') {:
						/buttons labels=["Yes", "No"] Do you want to use the same type of Privates for the Male and Female parts?|
						/var selected_btn {{pipe}}|
						/ife ( selected_btn == '') {:
							/echo Aborting|
							/abort
						:}|
						/elseif ( selected_btn == 'Yes') {:
							/buttons labels=["Yes", "No"] <div>Do you want to use the same base for the female genitalia as the base character?</div><div>{{getvar::characterArchetype}}: {{getvar::characterType}}</div>|
							/let key=tempPrivateFemale {{pipe}}|
							/ife (tempPrivateFemale == 'Yes') {:
								/setvar key=privatesFemale {{getvar::temp}}|
								/setvar key=searchPrivatesFemale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::temp}}|
							:}|
							/elseif (tempPrivateFemale == 'No') {:
								/buttons labels=["Human", "Anthropomorphic\n(Anthropomorphic is a character that combines both human and animal traits, often featuring an animal body with human-like posture, facial expressions, speech, and behavior.)", "Mythfolk\n(Mythfolk is races that mostly looks like humans like Dwarfs, Elves etc...)", "Tauric\n(Tauric are hybrid species with a humanoid upper body and an animal-like lower body, such as centaurs, lamias, and mermaids.)", "Beastkin\n(Beastkin is a character with animal features like ears and tail but otherwise human appearance.)", "Animalistic\n(Animalistic refers to standard animals, fantasy creatures, or monsters that behave and appear primarily as non-human beings, typically walking on all fours and lacking human speech or reasoning.)", "Pokémon", "Digimon", "Android\n(Android is a robot that looks and acts like a Human.)"] What type of female genitalia are you making? |
								/re-replace find="/(\n\()[\s\S]*$/g" replace="" {{pipe}}|
								/setvar key=tempFemaleGenitaliaCharacterArchetype {{pipe}}|
								/re-replace find="/\(.*$/g" replace="" {{getvar::tempFemaleGenitaliaCharacterArchetype}}|
								/setvar key=tempFemaleGenitaliaCharacterArchetype {{pipe}}|
								
								/var selected_btn {{noop}}|
								/ife ( tempFemaleGenitaliaCharacterType != '' ) {:
									/buttons labels=["Yes", "No"] Do you want to change the character type?|
									/var selected_btn {{pipe}}|
									/ife ( selected_btn == '') {:
										/echo Aborting|
										/abort
									:}|
								:}|
								/ife ( (selected_btn == 'Yes') or (tempFemaleGenitaliaCharacterType == '')) {:
									/setvar key=tempFemaleGenitaliaCharacterType None|
								:}|
								/ife (((tempFemaleGenitaliaCharacterType == 'None') or ( selected_btn == 'Yes')) and (( tempFemaleGenitaliaCharacterArchetype == 'Anthropomorphic') or ( tempFemaleGenitaliaCharacterArchetype == 'Beastkin') or ( tempFemaleGenitaliaCharacterArchetype == 'Digimon') or ( tempFemaleGenitaliaCharacterArchetype == 'Pokémon'))) {:
									/buttons labels=["Pokémon", "Digimon", "Animalistic"] Select the type you want?|
									/setvar key=tempFemaleGenitaliaCharacterType {{pipe}}|
									/ife ( tempFemaleGenitaliaCharacterType == '') {:
										/echo Aborting|
										/abort
									:}|
								:}|
								/setvar key=privatesFemale {{getvar::temp}}|
								/setvar key=searchPrivatesFemale {{getvar::tempFemaleGenitaliaCharacterArchetype}}: {{getvar::tempFemaleGenitaliaCharacterType}}: {{getvar::temp}}|
							:}|
							
							/elseif ( selected_btn == 'Yes') {:
							/buttons labels=["Yes", "No"] <div>Do you want to use the same base for the male genitalia as the base character?</div><div>{{getvar::characterArchetype}}: {{getvar::characterType}}</div>|
							/let key=tempPrivateMale {{pipe}}|
							/ife (tempPrivateMale == 'Yes') {:
								/setvar key=privatesMale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::temp}}|
							:}|
							/elseif (tempPrivateMale == 'No') {:
								/buttons labels=["Human", "Anthropomorphic\n(Anthropomorphic is a character that combines both human and animal traits, often featuring an animal body with human-like posture, facial expressions, speech, and behavior.)", "Mythfolk\n(Mythfolk is races that mostly looks like humans like Dwarfs, Elves etc...)", "Tauric\n(Tauric are hybrid species with a humanoid upper body and an animal-like lower body, such as centaurs, lamias, and mermaids.)", "Beastkin\n(Beastkin is a character with animal features like ears and tail but otherwise human appearance.)", "Animalistic\n(Animalistic refers to standard animals, fantasy creatures, or monsters that behave and appear primarily as non-human beings, typically walking on all fours and lacking human speech or reasoning.)", "Pokémon", "Digimon", "Android\n(Android is a robot that looks and acts like a Human.)"] What type of male genitalia are you making? |
								/re-replace find="/(\n\()[\s\S]*$/g" replace="" {{pipe}}|
								/setvar key=tempMaleGenitaliaCharacterArchetype {{pipe}}|
								/re-replace find="/\(.*$/g" replace="" {{getvar::tempMaleGenitaliaCharacterArchetype}}|
								/setvar key=tempMaleGenitaliaCharacterArchetype {{pipe}}|
								
								/var selected_btn {{noop}}|
								/ife ( tempMaleGenitaliaCharacterType != '' ) {:
									/buttons labels=["Yes", "No"] Do you want to change the character type?|
									/var selected_btn {{pipe}}|
									/ife ( selected_btn == '') {:
										/echo Aborting|
										/abort
									:}|
								:}|
								/ife ( (selected_btn == 'Yes') or (tempMaleGenitaliaCharacterType == '')) {:
									/setvar key=tempMaleGenitaliaCharacterType None|
								:}|
								/ife (((tempMaleGenitaliaCharacterType == 'None') or ( selected_btn == 'Yes')) and (( tempMaleGenitaliaCharacterArchetype == 'Anthropomorphic') or ( tempMaleGenitaliaCharacterArchetype == 'Beastkin') or ( tempMaleGenitaliaCharacterArchetype == 'Digimon') or ( tempMaleGenitaliaCharacterArchetype == 'Pokémon'))) {:
									/buttons labels=["Pokémon", "Digimon", "Animalistic"] Select the type you want?|
									/setvar key=tempMaleGenitaliaCharacterType {{pipe}}|
									/ife ( tempMaleGenitaliaCharacterType == '') {:
										/echo Aborting|
										/abort
									:}|
								:}|
								/setvar key=privatesMale {{getvar::tempMaleGenitaliaCharacterArchetype}}: {{getvar::tempMaleGenitaliaCharacterType}}{{getvar::temp}}|
							:}|
						:}|
						/elseif ( selected_btn == 'No') {:
							/setvar key=privates{{var::item}} {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::temp}}|
						:}|
					:}|
				:}|
				/else {:
					/setvar key=privates{{var::item}} "To be selected"|
					/getvar key=loopGenitalia index=1|
					/setvar key=check {{pipe}}|
					/ife (check == '') {:
						/ife (item != 'Male') {:
							/setvar key=privatesFemale None|
							/setvar key=searchPrivatesFemale None|
						:}|
						/ife (item != 'Female') {:
							/setvar key=privatesMale None|
							/setvar key=seachPrivatesMale None|
						:}|
					:}|
				:}|
			:}|
		:}|
	:}|
	/elseif (selected_btn == 'No') {:
		/ife (animalBase == 'Fantasy') {:
			/ife (futanari == 'Yes') {:
				/setvar key=privatesFemale "To be selected"|
				/setvar key=privatesMale "To be selected"|
			:}|
			/elseif (gender == 'Male') {:
				/setvar key=privatesFemale None|
				/setvar key=privatesMale "To be selected"|
			:}|
			/elseif (gender == 'Female') {:
				/setvar key=privatesFemale "To be selected"|
				/setvar key=privatesMale None|
			:}|
		:}|
		/elseif (futanari == 'Yes') {:
			/setvar key=privatesFemale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::animalBase}}|
			/setvar key=privatesMale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::animalBase}}|
		:}|
		/elseif (gender == 'Male') {:
			/setvar key=privatesFemale None|
			/setvar key=privatesMale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::animalBase}}|
		:}|
		/else {:
			/setvar key=privatesFemale {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::animalBase}}|
			/setvar key=privatesMale None|
		:}|
	:}|
	/flushvar temp|
	/flushvar temp1|
	/flushvar loopGenitalia|
	/flushvar tempMaleGenitaliaCharacterArchetype|
	/flushvar tempFemaleGenitaliaCharacterArchetype|
	/flushvar tempPrivateFemale|
	/flushvar tempPrivateMale|
	
:}|
/addvar key=dataBaseNames privatesFemale|
/addvar key=dataBaseNames privatesMale|


/var selected_btn {{noop}}|
/ife (real != '') {:
	/buttons labels=["Yes", "No"] Do you want to change if the character is real or not?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
:}|
/ife (selected_btn != 'No') {:
	/:"CMC Logic.Is Real"|
:}|
/buttons labels=["Yes", "No"] Is the character going to be part of a Chat Group?|
/var selected_btn {{pipe}}|
/ife ( selected_btn == '') {:
	/echo Aborting|
	/abort
:}|
/elseif ( selected_btn == 'Yes') {:
	/setvar key=chatGroup Yes|
	/buttons labels=["Yes", "No"] Is the user going to be part of the Chat Group?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting|
		/abort
	:}|
	/elseif ( selected_btn == 'Yes') {:
		/setvar key=user Yes|
	:}|
	/else {:
		/setvar key=user No|
	:}|
:}|
/else {:
	/setvar key=user Yes|
	/setvar key=chatGroup No|
:}|
/addvar key=dataBaseNames user|
/addvar key=dataBaseNames chatGroup|
/addvar key=dataBaseNames wait|

/ife (makeLoreBook != 'Yes') {:
	/findentry field=comment file="CMC Templates" Character Template Standard|
	/getentryfield file="CMC Templates" {{pipe}}|
	/setvar key=char_template {{pipe}}|
:}|
/else {:
	/findentry field=comment file="CMC Templates" Character Template Lorebook|
	/getentryfield file="CMC Templates" {{pipe}}|
	/setvar key=char_template {{pipe}}|
:}|
/message-edit message=0 await=true <h2 align='center'>Scenario Overview</h2>{{newline}}{{newline}}--ScenarioOverview--|
/sendas name={{char}} {{getvar::char_template}}|

/buttons labels=["Yes", "No"] Are you using a vision capable LLM and if so do you want to upload a image of your character to help with Appearance and Outfit generation?|
/setvar key=imageGen {{pipe}}|
/setvar key=firstMessID 2|
/setvar key=altGreetID 3|
/setvar key=taglineID 4|
/setvar key=imageID {{noop}}|
/ife (imageGen == 'Yes') {:

	/sendas name={{user}} Press the <img src="https://img.icons8.com/?size=100&id=X8KeZWFUUtu4&format=png&color=8E8484" style="width: 24px; height: 24px;"> icon above this message and select the character image you want to use. You may need to press the <img src="https://img.icons8.com/?size=100&id=36944&format=png&color=8E8484" style="width: 24px; height: 24px;"> for it to appear.|
	/echo extendedTimeout=0 timeout=0 awaitDismissal=true Press to Continue|
	/message-edit await=true Character Image|
	/setvar key=imageID 2|
	/setvar key=firstMessID 3|
	/setvar key=altGreetID 4|
	/setvar key=taglineID 5|	
:}|

/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating World Info" {{pipe}}|

/flushvar continue|
/forcesave|