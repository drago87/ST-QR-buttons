/let key=wi_uid {{noop}}|
/let key=actionType {{noop}}|
/let key=context {{noop}}|
/let key=find {{noop}}|
/let key=examples {{noop}}|
/let key=task {{noop}}|
/let key=instruct {{noop}}|
/let key=contextStarter {{noop}}|
/let key=contextStopper {{noop}}|
/let key=examplesStarter {{noop}}|
/let key=examplesStopper {{noop}}|
/let key=taskStarter {{noop}}|
/let key=taskStopper {{noop}}|
/let key=instructionsStarter {{noop}}|
/let key=instructionsStopper {{noop}}|
/let key=anatomyPrompt {{noop}}|
/let key=rolePr {{noop}}|
/let key=outputTr {{noop}}|

/findentry field=comment file="CMC Static Variables" "Need Stopper"|
/var key=wi_uid {{pipe}}|
/getentryfield field=content file="CMC Static Variables" {{var::wi_uid}}|
/let key=needed {{pipe}}|
/split find="---" {{var::needed}}|
/var key=needed {{pipe}}|
/ife (model in needed) {:
	/var key=contextStarter "<context>{{newline}}"|
	/var key=contextStopper "{{newline}}</context>"|
	/var key=examplesStarter "<examples>{{newline}}"|
	/var key=examplesStopper "{{newline}}</examples>"|
	/var key=taskStarter "<task>{{newline}}"|
	/var key=taskStopper "{{newline}}</task>"|
	/var key=instructionsStarter "<instructions>{{newline}}"|
	/var key=instructionsStopper "{{newline}}</instructions>"|
:}|

/getvar key=genSettings index=wi_book|
/let key=wi_book_f {{pipe}}|
/ife ( wi_book_f == '') {:
	/var key=wi_book_f "CMC Generation Prompts"|
:}|

/ife ((wi_book_f == 'CMC Generation Prompts') or (wi_book_f == 'CMC Information')) {:
	/var key=wi_book_f "{{var::wi_book_f}} {{getglobalvar::model}}"|
:}|

/getvar key=genSettings index=wi_book_key|
/let key=wi_book_key_f {{pipe}}|
/ife ( wi_book_key_f == '') {:
	/abort quiet=false Missing wi_book_key name in input.|
:}|
/getvar key=genSettings index=genIsList|
/let key=genIsList_f {{pipe}}|
/ife ( genIsList_f == '') {:
	/abort quiet=false Missing genIsList setting in input.|
:}|
/getvar key=genSettings index=guidencePrompt|
/let key=guidencePrompt_f {{pipe}}|
/ife ( guidencePrompt_f == '') {:
	/var key=find "{{var::wi_book_key_f}}: Guide"|
	/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
	/var key=wi_uid {{pipe}}|
	/getentryfield field=comment file="CMC Generation Prompts {{getglobalvar::model}}" {{var::wi_uid}}|
	/let key=testPrompt {{pipe}}|
	/ife ( find == testPrompt) {:
		/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{var::wi_uid}}|
		/var key=guidencePrompt_f {{pipe}}|
		/ife (guidencePrompt_f == '') {:
			/var key=find "Guidance Template"|
			/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{var::wi_uid}}|
			/var key=guidencePrompt_f {{pipe}}|
		:}|
	:}|
	/else {:
		/var key=find "Guidance Template"|
		/findentry field=comment file="CMC Templates" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="CMC Templates" {{var::wi_uid}}|
		/var key=guidencePrompt_f {{pipe}}|
	:}|
:}|
/getvar key=genSettings index=genIsSentence|
/let key=genIsSentence_f {{pipe}}|
/ife ( genIsSentence_f == '') {:
	/abort quiet=false Missing genIsSentence setting in input.|
:}|
/getvar key=genSettings index=needOutput|
/let key=needOutput_f {{pipe}}|
/ife ( needOutput_f == '') {:
	/var key=needOutput_f Yes|
:}|


/getvar key=genSettings index=genAmount|
/let key=genAmount_f {{pipe}}|
/ife ( genAmount_f == '') {:
	/var as=number key=genAmount_f 5|
:}|
/getvar key=genSettings index=useContext|
/let key=useContext_f {{pipe}}|
/ife ( useContext_f == '') {:
	/abort quiet=false Missing useContext setting in input.|
:}|
/getvar key=genSettings index=useAnatomy|
/let key=useAnatomy_f {{pipe}}|
/ife ( useAnatomy_f == '') {:
	/var key=useAnatomy_f false|
:}|

/getvar key=genSettings index=outputIsList|
/let key=outputIsList_f {{pipe}}|
/ife ( outputIsList_f == '') {:
	/var key=outputIsList_f No|
:}|
/getvar key=genSettings index=maxSizeOfList|
/let key=maxSizeOfList_f {{pipe}}|
/ife ( (outputIsList_f == 'Yes') and (maxSizeOfList_f == '')) {:
	
:}|
/elseif ( (outputIsList_f == 'Yes') and (maxSizeOfList_f is string)) {:
	/abort quiet=false maxSizeOfList needs to be empty or Number.|
:}|
/getvar key=genSettings index=random|
/let as=array key=random_f {{pipe}}|
/getvar key=genSettings index=contextKey|
/let as=array key=contextKey_f {{pipe}}|
/getvar key=genSettings index=extraContext|
/let as=array key=extraContext_f {{pipe}}|

/ife ('role' in promptOrder) {:
	/var key=find "Model Role"|
	/findentry field=comment file="CMC Prompt {{getglobalvar::model}}" "{{var::find}}"|
	/var key=wi_uid {{pipe}}|
	/getentryfield field=comment file="CMC Prompt {{getglobalvar::model}}" {{var::wi_uid}}|
	/let key=testRole {{pipe}}|
	/ife (find == testRole) {:
		/getentryfield field=content file="CMC Prompt {{getglobalvar::model}}" {{var::wi_uid}}|
		/var key=testRole {{pipe}}|
		/ife (testRole != '') {:
			/var key=rolePr {{var::testRole}}|
		:}|
	:}|
:}|

/ife ('output trigger' in promptOrder) {:
	/var key=find "Output Trigger"|
	/findentry field=comment file="CMC Prompt {{getglobalvar::model}}" "{{var::find}}"|
	/var key=wi_uid {{pipe}}|
	/getentryfield field=comment file="CMC Prompt {{getglobalvar::model}}" {{var::wi_uid}}|
	/let key=testOutputTr {{pipe}}|
	/ife (find == testOutputTr) {:
		/getentryfield field=content file="CMC Prompt {{getglobalvar::model}}" {{var::wi_uid}}|
		/var key=testOutputTr {{pipe}}|
		/ife (testOutputTr != '') {:
			/var key=outputTr {{var::testOutputTr}}|
		:}|
	:}|
:}|


/ife ( useContext_f == 'Yes') {:
	/setvar key=gen Yes|
	/:"CMC Logic.Get Char info"|
	/:"CMC Logic.Set Base Context"|
	/var key=context {{getvar::baseContext}}|
	/flushvar gen|
:}|
/ife (( useContext_f == 'No') and ( extraContext_f != '')) {:
	/var key=context <!-- NSFW content is permitted if contextually appropriate -->{{newline}}CONTEXT (for your reference—do not include in the answer):"|
	/ife (real == Yes) {:
		/var key=context "{{var::context}}{{newline}}{{getvar::realParcedContext}}"|
	:}|
:}|

/ife ( extraContext_f != '') {:

	/foreach {{var::extraContext_f}} {:
		/var key=context {{var::context}}{{newline}}{{var::item}}|
	:}|
:}|

/ife ( contextKey_f != '') {:
	/foreach {{var::contextKey_f}} {:
		/var key=find "{{var::item}}: Context"|
		/findentry field=comment file="CMC Information {{getglobalvar::model}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=comment file="CMC Information {{getglobalvar::model}}" {{var::wi_uid}}|
		/let key=conTest {{pipe}}|
		/ife ( find == conTest) {:
			/getentryfield field=content file="CMC Information {{getglobalvar::model}}" {{var::wi_uid}}|
			/let key=cInfo {{pipe}}|
			/ife (context != '') {:
				/var key=context "{{var::context}}{{newline}}{{newline}}{{var::cInfo}}"|
			:}|
			/else {:
				/var key=context "{{var::cInfo}}"|
			:}|
		:}|
	:}|
:}|
/ife (context != '') {:
	/wi-list-books all=true|
	/setvar key=wiList {{pipe}}|
	/let key=findMale {{noop}}|
	/let key=findFemale {{noop}}|
	/ife ((loreAnatomy == 'Yes') and (useAnatomy_f != false) and ('CMC Anatomy' in wiList)) {:
		
		/ife ((parsedAnimalType == 'Fantasy') or (characterArchetype == 'Tauric') or (characterArchetype == 'Pokémon') or (characterType == 'Pokémon') or (characterArchetype == 'Digimon') or (characterType == 'Digimon')) {:
			/ife ((gender == 'Male') or (futanari == 'Yes')) {:
				/var key=findMale "Anatomy: {{getvar::seachPrivatesMale}}: {{getvar::species}}: Male"|
			:}|
			/ife ((gender == 'Female') or (futanari == 'Yes')) {:
				/var key=findFemale "Anatomy: {{getvar::seachPrivatesFemale}}: {{getvar::species}}: Female"|
			:}|
		:}|
		/else {:
			/ife ((gender == 'Male') or (futanari == 'Yes')) {:
				/var key=findMale "Anatomy: {{getvar::seachPrivatesMale}}: Male"|
			:}|
			/ife ((gender == 'Female') or (futanari == 'Yes')) {:
				/var key=findFemale "Anatomy: {{getvar::seachPrivatesFemale}}: Female"|
			:}|
		:}|
		/ife (futanari == 'Yes') {:
			/findentry field=comment file="CMC Anatomy" "{{var::findMale}}"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=comment file="CMC Anatomy" {{var::wi_uid}}|
			/let key=testComment {{pipe}}|
			/ife ( find == testComment) {:
				/getentryfield field=content file="CMC Anatomy" {{var::wi_uid}}|
				/setvar key=male_genital_structure {{pipe}}|
				/echo Found Optional Genitial Structure for ({{var::findMale}})|
			:}|
			/findentry field=comment file="CMC Anatomy" "{{var::findFemale}}"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=comment file="CMC Anatomy" {{var::wi_uid}}|
			/let key=testComment {{pipe}}|
			/ife ( find == testComment) {:
				/getentryfield field=content file="CMC Anatomy" {{var::wi_uid}}|
				/setvar key=female_genital_structure {{pipe}}|
				/echo Found Optional Genitial Structure for ({{var::findFemale}})|
			:}|
			/ife (gender == 'Male') {:
				/var key=find {{var::findMale}}|
			:}|
			/elseif (gender == 'Female') {:
				/var key=find {{var::findFemale}}|
			:}|
		:}|
		/else {:
			/ife (findMale != '') {:
				/var key=find {{var::findMale}}|
			:}|
			/elseif (findFemale != '') {:
				/var key=find {{var::findFemale}}|
			:}|
		:}|
		/ife (futanari == 'Yes') {:
			/re-replace find="/: (Male|Female)/" replace=": Futanari" {{var::find}}|
			/var key=find {{pipe}}|
		:}|
		/findentry field=comment file="CMC Anatomy" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=comment file="CMC Anatomy" {{var::wi_uid}}|
		/let key=testComment {{pipe}}|
		/ife ( find == testComment) {:
			/getentryfield field=content file="CMC Anatomy" {{var::wi_uid}}|
			/setvar key=genital_structure {{pipe}}|
			/echo Found Optional Genitial Structure for ({{var::find}})|
		:}|
		
		
		/ife ((parsedAnimalType == 'Fantasy') or (characterArchetype == 'Tauric') or (characterArchetype == 'Pokémon') or (characterType == 'Pokémon') or (characterArchetype == 'Digimon') or (characterType == 'Digimon')) {:
			/var key=find "Anatomy: {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::species}}: {{getvar::gender}}"|
		:}|
		/else {:
			/var key=find "Anatomy: {{getvar::characterArchetype}}: {{getvar::characterType}}: {{getvar::parsedAnimalType}}: {{getvar::gender}}"|
		:}|
		/findentry field=comment file="CMC Anatomy" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=comment file="CMC Anatomy" {{var::wi_uid}}|
		/var key=testComment {{pipe}}|
		/ife ( find == testComment) {:
			/getentryfield field=content file="CMC Anatomy" {{var::wi_uid}}|
			/var key=anatomyPrompt {{pipe}}|
			/ife (anatomyPrompt == '') {: 
			:}|
			/else {:
				/var key=anatomyPrompt "{{getvar::characterArchetype}} Anatomy: {{getvar::characterType}}{{newline}}{{var::anatomyPrompt}}"|
				/echo Found Optional Anatomy for ({{var::findFemale}})|
			:}|
		:}|
	:}|
	/ife (anatomyPrompt != '') {:
		/var key=context "[{{var::contextStarter}}{{var::context}}{{newline}}{{newline}}{{var::anatomyPrompt}}{{var::contextStopper}}]{{newline}}{{newline}}"|
	:}|
	/else {:
		/var key=context "[{{var::contextStarter}}{{var::context}}{{var::contextStopper}}]{{newline}}{{newline}}"|
	:}|
:}|

/ife (debug == 'Yes') {:
	/setvar key="01 Context" {{var::context}}|
:}|
/else {:
	/flushvar "01 Context"|
:}|
/var key=find "{{var::wi_book_key_f}}: Examples"|
/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
/var key=wi_uid {{pipe}}|
/getentryfield field=comment file="{{var::wi_book_f}}" {{var::wi_uid}}|
/let key=exaTemp {{pipe}}|
/ife (find == exaTemp) {:
	/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
	/let key=tEx {{pipe}}|
	/ife (tEx != '') {:
		/var key=examples [{{var::examplesStarter}}{{var::tEx}}{{var::examplesStopper}}]|
	:}|
:}|
/else {:
	/echo Missing WI entry '{{var::find}}' from the WI '{{var::wi_book_f}}'|
	/echo Aborting |
	/abort
:}|
/ife (debug == 'Yes') {:
	/setvar key="02 Examples" {{var::examples}}|
:}|
/else {:
	/flushvar "02 Examples"|
:}|
/var key=find "{{var::wi_book_key_f}}: Task"|
/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
/var key=wi_uid {{pipe}}|
/getentryfield field=comment file="{{var::wi_book_f}}" {{var::wi_uid}}|
/let key=taskTest {{pipe}}|
/ife (find == taskTest) {:
	/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
	/var key=task [{{var::taskStarter}}{{pipe}}{{var::taskStopper}}]|
:}|
/else {:
	/echo Missing WI entry '{{var::find}}' from the WI '{{var::wi_book_f}}'|
	/echo Aborting |
	/abort
:}|
/ife (debug == 'Yes') {:
	/setvar key="03 Task" {{var::task}}|
:}|
/else {:
	/flushvar "03 Task"|
:}|
/var key=find "{{var::wi_book_key_f}}: Instruction"|
/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
/var key=wi_uid {{pipe}}|
/getentryfield field=comment file="{{var::wi_book_f}}" {{var::wi_uid}}|
/let key=instructTest {{pipe}}|
/ife (find == instructTest) {:
	/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
	/var key=instruct [{{var::instructionsStarter}}{{pipe}}{{var::instructionsStopper}}]|
:}|
/else {:
	/echo Missing WI entry '{{var::find}}' from the WI '{{var::wi_book_f}}'|
	/echo Aborting |
	/abort
:}|
/ife (debug == 'Yes') {:
	/setvar key="04 Instruktions" {{var::instruct}}|
:}|
/else {:
	/flushvar "04 Instruktions"|
:}|
/let key=genState []|
/let key=selected_btn |
/let key=man|

/let key=isGeneration 'Yes'|
/setvar as=array key=tempList []|
/setvar key=output {{noop}}|
/setvar key=previousMilestones {{noop}}|
/setvar key=blackListGen {{noop}}|
/setvar key=mileS {{noop}}|
/setvar key=guidance {{noop}}|


/ife ( genIsSentence_f != 'Yes') {:
	/ife ( outputIsList_f == 'Yes') {:
		/var key=actionType add|
	:}|
	/else {:
		/var key=actionType set|
	:}|
	/var key=man "Manually {{var::actionType}}"|
:}|
/else {:
	/var key=man "Edit output"|
:}|
/let t |
/whilee ( output == '') {:
	/ife ( (outputIsList_f == 'Yes') and (maxSizeOfList_f is number)) {:
		/len {{getvar::tempList}}|
		/let key=len {{pipe}}|
		/ife (len == maxSizeOfList_f) {:
			/setvar key=save Done|
			/flushvar guidance|
			/:"CMC Logic.SaveGen"|
			/break|
		:}|
	:}|
	/ife (wi_book_key_f == 'Story Plan Details') {:
		/len {{getvar::tempOutputList}}|
		/let len={{pipe}}|
		/ife (len > 0 ) {:
			/setvar key=mileS "Milestone {{var::len}}"|
		:}|
		/else {:
			/setvar key=mileS "Start"|
		:}|
	:}|
	
	/ife ((wi_book_key_f == 'Sexual Notes') and ((tempList != '') or (sexualNotes != ''))) {:
		/setvar key=blackListGen "Avoid duplicating or restating the following existing notes:{{newline}}[ALREADY GENERATED SEXUAL NOTES AVOID THESE]{{newline}}\""|
		/foreach {{getvar::sexualNotes}} {:
			/addvar key=blackListGen "{{newline}}- {{var::item}}"|
		:}|
		/foreach {{getvar::tempList}} {:
			/addvar key=blackListGen "{{newline}}- {{var::item}}"|
		:}|
		/addvar key=blackListGen "{{newline}}\""|
	:}|
	/elseif ((wi_book_key_f == 'Behavior Notes') and ((tempList != '') or (behaviorNotes != ''))) {:
		/setvar key=blackListGen "Avoid duplicating or restating the following existing notes:{{newline}}[ALREADY GENERATED BEHAVIOR NOTES AVOID THESE]{{newline}}\""|
		/foreach {{getvar::behaviorNotes}} {:
			/addvar key=blackListGen "{{newline}}- {{var::item}}"|
		:}|
		/foreach {{getvar::tempList}} {:
			/addvar key=blackListGen "{{newline}}- {{var::item}}"|
		:}|
		/addvar key=blackListGen "{{newline}}\""|
	:}|
	/elseif (( wi_book_key_f == 'Story Plan Details') and ((tempList != '') or (storyPlanMilestonesList != ''))) {:
		/setvar key=previousMilestones "{{newline}}**REFERENCE ONLY:** Earlier milestones are provided below for pacing continuity. Do not mention or duplicate them:"|
		/foreach {{getvar::storyPlanMilestonesList}} {:
			/ife (index > 0) {:
				/addvar key=previousMilestones "{{newline}}"|
			:}|
			/ife (index > 0) {:
				/getvar key=tempOutputList index={{var::index}}|
				/addvar key=previousMilestones "{{newline}}- Milestone {{var::index}}: {{var::item}}{{newline}}  - Details: {{pipe}}"|
			:}|
			/else {:
				/getvar key=tempOutputList index={{var::index}}|
				/addvar key=previousMilestones "{{newline}}- Start: {{var::item}}{{newline}}  - Details: {{pipe}}"|
			:}|
		:}|
	:}|
	/echo Generating {{var::wi_book_key_f}}|
	/ife ((random_f is list) and ( wi_book_key_f == 'Speech Single Ticks')) {:
		/var key=instruct {{noop}}|
		/pick {{var::random_f}}|
		/setvar key=rand {{pipe}}|
	:}|
	/var key=genState []|
	/ife (genIsList_f == 'Yes') {:
		/join {{getvar::tempList}}|
		/setvar key=excludedList {{pipe}}|
		/setvar key=outputGen []|
		/whilee ((i++ < genAmount_f ) and (stuckPrevention++ < 10)) {:
			/var key=task {{noop}}|
			/var key=find "{{var::wi_book_key_f}}: Task"|
			/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
			/var key=task [{{var::taskStarter}}{{pipe}}{{var::taskStopper}}]|
			/var key=find "{{var::wi_book_key_f}}: Instruction"|
			/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
			/var key=wi_uid {{pipe}}|
			/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
			/var key=instruct [{{var::instructionsStarter}}{{pipe}}{{var::instructionsStopper}}]|
			/echo Generatig {{var::wi_book_key_f}} {{var::i}}/{{var::genAmount_f}}|
			/len {{getglobalvar::promptOrder}}|
			/let key=promptOrderLen {{pipe}}|
			/ife ((promptOrder == '') or (promptOrderLen < 4)) {:
				/setglobalvar key=promptOrder ["contexts", "examples", "task", "instructions"]|
				/popup "Default order for the prompt set. If you want to change it press the 'CMC Menu' button.{{newline}}Default order: contexts, examples, task, instructions"|
			:}|
			/setvar key=promptToRun {{noop}}|
			/foreach {{getglobalvar::promptOrder}} {:
				/ife ((rolePr != '') and (item == 'role')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<role>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::role}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</role>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((context != '') and (item == 'contexts')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<context>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::context}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</context>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((examples != '') and (item == 'examples')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<examples>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::examples}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</examples>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((task != '') and (item == 'task')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<task>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::task}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</task>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((instruct != '') and (item == 'instructions')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<instructions>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::instruct}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}<instructions>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((outputTr != '') and (item == 'output trigger')) {:
					/addvar key=promptToRun {{var::outputTr}}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
			:}|
			//genraw "{{var::context}}{{var::examples}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruct}}"|
			/genraw "{{getvar::promptToRun}}"|
			/re-replace find="/<think>[\s\S]*?<\/think>\s*/g" replace="" {{pipe}}|
			/var key=t {{pipe}}|
			/ife (debug == 'Yes') {:
				/setvar key="00 Genraw" "{{var::context}}{{var::examples}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruct}}"|
				/setvar key="01 Context" {{var::context}}|
				/setvar key="02 Examples" {{var::examples}}|
				/setvar key="03 Task" {{var::task}}|
				/setvar key="04 Instruktions" {{var::instruct}}|
			:}|
			/ife ((t not in outputGen) and (t != '')) {:
				/addvar key=outputGen {{var::t}}|
				/join {{getvar::tempList}}|
				/setvar key=excludedList {{pipe}}|
				/ife (excludedList != '') {:
					/addvar key=excludedList ", "|
				:}|
				/join {{getvar::outputGen}}|
				/addvar key=excludedList {{pipe}}|
				/var key=stuckPrevention 0|
			:}|
			/else {:
				/add {{var::i}} -1|
				/var key=i {{pipe}}|
			:}|
		:}|
		/join glue="---" {{getvar::outputGen}}|
		/var key=t {{pipe}}|
		/flushvar outputGen|
		/flushvar excludedList|
		/ife (debug == 'Yes') {:
			/setvar key="05 Output" {{var::t}}|
		:}|
		/else {:
			/flushvar "05 Output"|
		:}|
		/reasoning-parse return=content {{var::t}}|
		/var key=t {{pipe}}|
	:}|
	/else {:
		/ife ( wi_book_key_f == 'Speech Examples QA') {:
			/*
			/let key=d Not Done|
			/whilee (d == 'Not Done') {:
				/split {{var::random_f}}|
				/pick {{pipe}}|
				/setvar key=target {{pipe}}|
				/ife (('--User--' in speechPromptClaim) and (target == '--User--')) {:
				:}|
				/else {:
					/var key=d Done|
				:}|
			:}|
			*|
			/split {{var::random_f}}|
			/let key=targ {{pipe}}|
			/ife ('{target}' in speechPromptClaim) {:
				/buttons labels={{var::targ}} <div>Select the person/entity you want to replace '{target}' with in this sentence:</div><div>{{getvar::speechPromptClaim}}</div>|
				/setvar key=target {{pipe}}|
				/re-replace find="/{target}/g" replace="{{getvar::target}}" {{getvar::speechPromptClaim}}|
				/setvar key=speechPromptClaimRand {{pipe}}|
			:}|
			/else {:
				/getat index=0 {{var::targ}}|
				/setvar key=target {{pipe}}|
				/re-replace find="/{target}/g" replace="{{getvar::target}}" {{getvar::speechPromptClaim}}|
				/setvar key=speechPromptClaimRand {{pipe}}|
			:}|
		:}|
		/var key=task {{noop}}|
		/var key=find "{{var::wi_book_key_f}}: Task"|
		/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
		/var key=task [{{var::taskStarter}}{{pipe}}{{var::taskStopper}}]|
		/var key=find "{{var::wi_book_key_f}}: Instruction"|
		/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
		/var key=instruct [{{var::instructionsStarter}}{{pipe}}{{var::instructionsStopper}}]|
		/ife (wi_book_key_f != 'First Message') {:
			
			/len {{getglobalvar::promptOrder}}|
			/let key=promptOrderLen {{pipe}}|
			/ife ((promptOrder == '') or (promptOrderLen < 4)) {:
				/setglobalvar key=promptOrder ["contexts", "examples", "task", "instructions"]|
				/popup "Default order for the prompt set. If you want to change it press the 'CMC Menu' button.{{newline}}Default order: contexts, examples, task, instructions"|
			:}|
			/setvar key=promptToRun {{noop}}|
			/foreach {{getglobalvar::promptOrder}} {:
				/ife ((rolePr != '') and (item == 'role')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<role>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::role}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</role>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((context != '') and (item == 'contexts')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<context>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::context}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</context>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((examples != '') and (item == 'examples')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<examples>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::examples}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</examples>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((task != '') and (item == 'task')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<task>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::task}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</task>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((instruct != '') and (item == 'instructions')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<instructions>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::instruct}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}<instructions>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((outputTr != '') and (item == 'output trigger')) {:
					/addvar key=promptToRun {{var::outputTr}}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
			:}|
			//genraw "{{var::context}}{{var::examples}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruct}}"|
			/genraw "{{getvar::promptToRun}}"|
			/re-replace find="/<think>[\s\S]*?<\/think>\s*/g" replace="" {{pipe}}|
			/var key=t {{pipe}}|
		:}|
		/elseif (wi_book_key_f == 'First Message') {:
			/messages names=off 1|
			/setvar key=fullCharacterSheet {{pipe}}|
			/re-replace find="/--FirstName--/g" replace="{{getvar::firstName}}" {{getvar::fullCharacterSheet}}|
			/setvar key=fullCharacterSheet {{pipe}}|
			
			/len {{getglobalvar::promptOrder}}|
			/let key=promptOrderLen {{pipe}}|
			/ife ((promptOrder == '') or (promptOrderLen < 4)) {:
				/setglobalvar key=promptOrder ["contexts", "examples", "task", "instructions"]|
				/popup "Default order for the prompt set. If you want to change it press the 'CMC Menu' button.{{newline}}Default order: contexts, examples, task, instructions"|
			:}|
			/setvar key=promptToRun {{noop}}|
			/foreach {{getglobalvar::promptOrder}} {:
				/ife ((rolePr != '') and (item == 'role')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<role>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::role}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</role>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((context != '') and (item == 'contexts')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<context>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::context}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</context>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((examples != '') and (item == 'examples')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<examples>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::examples}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</examples>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((task != '') and (item == 'task')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<task>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::task}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}</task>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((instruct != '') and (item == 'instructions')) {:
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "<instructions>{{newline}}"|
					:}|
					/addvar key=promptToRun {{var::instruct}}|
					/ife (xmlTags == 'Yes') {:
						/addvar key=promptToRun "{{newline}}<instructions>"|
					:}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
				/elseif ((outputTr != '') and (item == 'output trigger')) {:
					/addvar key=promptToRun {{var::outputTr}}|
					/ife (index < promptOrderLen) {:
						/addvar key=promptToRun {{newline}}{{newline}}|
					:}|
				:}|
			:}|
			//genraw "{{var::context}}{{var::examples}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruct}}{{newline}}{{newline}}## [CHARACTER_SHEET_REFERENCE]
Below is the full character sheet for {{getvar::firstName}}. Use it to understand {{getvar::subjPronoun}}’s personality, tone, and behavioral cues. This is reference only — do not quote or summarize it.

{{getvar::fullCharacterSheet}}"|
			/genraw "{{getvar::promptToRun}}{{newline}}{{newline}}## [CHARACTER_SHEET_REFERENCE]
Below is the full character sheet for {{getvar::firstName}}. Use it to understand {{getvar::subjPronoun}}’s personality, tone, and behavioral cues. This is reference only — do not quote or summarize it.

{{getvar::fullCharacterSheet}}"|
			/re-replace find="/<think>[\s\S]*?<\/think>\s*/g" replace="" {{pipe}}|
			/var key=t {{pipe}}|
		:}|
		/re-replace find="/^[;\s]+/g" replace="" {{var::t}}|
		/var key=t {{pipe}}|
		/ife (wi_book_key_f != 'First Message' ) {:
			/re-replace find="/^\*/g" replace="" {{var::t}}|
			/re-replace find="/\*$/g" replace="" {{pipe}}|
			/var key=t {{pipe}}|
		:}|
		/ife (debug == 'Yes') {:
			/setvar key="05 Output" {{var::t}}|
		:}|
		/else {:
			/flushvar "05 Output"|
		:}|
		/reasoning-parse return=content {{var::t}}|
		/var key=t {{pipe}}|
		/trimend {{var::t}}|
		/var key=t {{pipe}}|
	:}|
	/ife (genIsList_f == 'Yes') {:
		/split find="---" {{var::t}}|
		/var key=t {{pipe}}|
		/ife ((wi_book_key_f != 'Height') and (wi_book_key_f != 'Length')) {:
			/foreach {{var::t}} {:
				/re-replace find="/\./g" replace="" {{var::item}}|
				/let key=t1 {{pipe}}|
				/re-replace find="/_/g" replace=" " {{pipe}}|
				/var key=t1 {{pipe}}|
				/to-lower {{var::t1}}|
				/var key=t1 {{pipe}}|
				/re-replace find="/\b([a-z])/" cmd="/to-upper $1" replace="$1" {{var::t1}}|
				/var key=t1 {{pipe}}|
				/wait 1|
				/setat index={{var::index}} value={{var::t}} {{var::t1}}|
				/var key=t {{pipe}}|
			:}|
			/var key=genState {{pipe}}|
		:}|
		/else {:
			/var key=genState {{var::t}}|
		:}|
		/ife ((wi_book_key_f == 'Personality Tags' ) and (personalityFoundTags != '')) {:
			/split {{getvar::personalityFoundTags}}|
			/let key=temp {{pipe}}|
			/reverse {{var::temp}}|
			/var key=temp {{pipe}}|
			/foreach {{var::genState}} {:
				/ife ( item not in temp) {:
					/len {{var::temp}}|
					/var key=temp index={{pipe}} {{var::item}}|
				:}|
			:}|
			/var key=genState {{var::temp}}|
			/join glue="--- " {{var::genState}}|
			/let key=tempItem {{pipe}}|
			/var as=array key=genState []|
			/var key=genState index=0 {{var::tempItem}}|
		:}|
	:}|
	/else {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} {{var::t}}|
	:}|
	
	/ife (debug == 'Yes') {:
		/setvar key="00 Genraw" "{{var::context}}{{var::examples}}{{newline}}{{newline}}{{var::task}}{{newline}}{{newline}}{{var::instruct}}"|
		/setvar key="01 Context" {{var::context}}|
		/setvar key="02 Examples" {{var::examples}}|
		/setvar key="03 Task" {{var::task}}|
		/setvar key="04 Instruktions" {{var::instruct}}|
	:}|
	
	
	/ife (('Outfit' in wi_book_key_f) and ('Description' not in wi_book_key_f) and ('Outfit Accessories' not in wi_book_key_f)) {:
		/unshift genState "None"|
		/var key=genState {{pipe}}|
	:}|
	
	
	/ife ( ('Random' not in genState) and (genIsList_f == 'Yes')) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} "Random"|
	:}|
	/let key=manualBlacklist ["seasons"]|
	/ife ( (wi_book_key_f not in manualBlacklist) and ( man not in genState)) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} {{var::man}}|
	:}|
	/let key=guidenceBlacklist ["Age Gen", "Age Species"]|
	/ife (( 'Guidance' not in genState) and (wi_book_key_f not in guidenceBlacklist)) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} Guidance|
	:}|
	/ife ( 'Generate New' not in genState) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} Generate New|
	:}|
	/ife (( 'Customize Parts of the generation' not in genState) and (wi_book_key_f == 'Archetype Base' )) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} Customize Parts of the generation|
	:}|
	/let key=forsedOutputWhitelist ["Appearance Features Humanoid", "Appearance Features Other"]|
	/ife ((wi_book_key_f is string) and (('Done' not in genState) and (((outputIsList_f == 'Yes') and (tempList != '')) or ((outputIsList_f != 'Yes') or (needOutput_f == 'No')) or ( wi_book_key_f in forsedOutputWhitelist)))) {:
		/len {{var::genState}}|
		/var key=genState index={{pipe}} "Done"|
	:}|
	
	/getvar key=genSettings index=buttonPrompt|
	/let key=buttonPrompt_f {{pipe}}|
	/ife ( buttonPrompt_f == '') {:
		/var key=buttonPrompt_f "Select the {{var::wi_book_key_f}} you want {{getvar::firstName}} to have."|
	:}|
	
	/let key=basicBlacklist ["Identify Personality Tag", "Personality QA", "Personality Tags", "Speech Examples QA", "Ability Proficiency", "Ability Description", "Appearance QA", "Appearance Features Other", "Appearance Features Humanoid"]|
	/ife (wi_book_key_f not in basicBlacklist) {:
		/buttons labels={{var::genState}} {{var::buttonPrompt_f}}|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif ( wi_book_key_f == 'Identify Personality Tag') {:
		/buttons labels={{var::genState}} <div>Are these the correct Personality tags found in {{getvar::firstName}}'s Archetype?</div><div>{{getvar::archetype}}</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Personality Tags') {:
		/buttons labels={{var::genState}} Is this list of Personality Traits correct for {{getvar::firstName}}?|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Personality QA') {:
		/buttons labels={{var::genState}} <div>Is this a good Answer by {{getvar::firstName}} for the question:</div><div>{{getvar::question}}</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Speech Examples QA') {:
		/buttons labels={{var::genState}} <div>Is this a good reaction to:</div><div>{{getvar::speechPromptClaimRand}}</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Appearance QA') {:
		/buttons labels={{var::genState}} <div>Is this a good answer to:</div><div>{{getvar::question}}</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Ability Proficiency') {:
		/buttons labels={{var::genState}} <div>Does this feel like the right level or state for {{getvar::firstName}}'s {{getvar::abilityName}}?</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif (wi_book_key_f == 'Ability Description') {:
		/buttons labels={{var::genState}} <div>Does this feel like a good description for: {{getvar::abilityName}}?</div>|
		/var key=selected_btn {{pipe}}|
	:}|
	/elseif ((wi_book_key_f == 'Appearance Features Other') or (wi_book_key_f == 'Appearance Features Humanoid')) {:
		/let key=oTemp {{noop}}|
		/foreach {{getvar::tempList}} {:
			/var key=oTemp "{{var::oTemp}}<div>{{var::item}}</div>"
		:}|
		/buttons labels={{var::genState}} <div>Select the Appearance Feature you want {{getvar::firstName}} to have</div><div>Here is the ones already Selected</div>{{var::oTemp}}|
		/var key=selected_btn {{pipe}}|
	:}|


	/ife ( selected_btn == ''){:
		/echo Aborting |
		/abort
	:}|
	/elseif (selected_btn == 'Random') {:
		/find index=true {{var::genState}} {:
			/test left={{var::item}} rule=eq right="Random"|
		:}|
		/let key=i {{pipe}}|
		/slice start=0 end={{var::i}} {{var::genState}}|
		/pick items=1 {{var::genState}}|
		/var key=selected_btn {{pipe}}|
		/setvar key=save {{var::selected_btn}}|
		/flushvar guidance|
		/:"CMC Logic.SaveGen"|
	:}|
	/elseif (selected_btn == 'Guidance') {:
		/let key=gu {{noop}}|
		/ife (guidance != '') {:
			/buttons labels=["Remove", "Set", "Change", "Cancel"] <div>What would you like to do with the kink guidance?</div>
<div>
  <ul>
    <li><strong>Remove</strong> – Delete the current guidance entirely.</li>
    <li><strong>Set</strong> – Write a sentence or phrase, and it will be auto-tagged into kink categories.</li>
    <li><strong>Change</strong> – Manually edit the existing guidance.</li>
    <li><strong>Cancel</strong> – Keep the current guidance as is.</li>
  </ul>
</div>|
			/var key=gu {{pipe}}|
		:}|
		/else {:
			/buttons labels=["Set", "Cancel"] <div>No kink guidance has been set yet.</div>
<div>
  <ul>
    <li><strong>Set</strong> – Write a sentence or phrase describing a kink theme or interest. It will be automatically tagged into kink categories.</li>
    <li><strong>Cancel</strong> – Skip this for now and continue without setting any guidance.</li>
  </ul>
</div>|
			/var key=gu {{pipe}}|
		:}|
		/ife ( gu == ''){:
			/echo Aborting |
			/abort
		:}|
		/elseif ( gu == 'Remove') {:
			/setvar key=guidance {{noop}}|
		:}|
		/elseif ( gu == 'Set') {:
			/input What kind of themes, feelings, or ideas do you want to explore through {{getvar::firstName}}?|
			/setvar key=guideTemp {{pipe}}|
			/ife (wi_book_key_f == 'Kink Type') {:
				/var key=find "Kink Type Guidance"|
				/findentry field=comment file="CMC Generation Prompts {{getglobalvar::model}}" "{{var::find}}"|
				/var key=wi_uid {{pipe}}|
				/getentryfield field=content file="CMC Generation Prompts {{getglobalvar::model}}" {{var::wi_uid}}|
				/let key=mainPrompt {{pipe}}|
				/var key=find "Kink Information"|
				/findentry field=comment file="CMC Information {{getglobalvar::model}}" "{{var::find}}"|
				/var key=wi_uid {{pipe}}|
				/getentryfield field=content file="CMC Information {{getglobalvar::model}}" {{var::wi_uid}}|
				/let key=infoPrompt {{pipe}}|
				/genraw "{{var::infoPrompt}}{{newline}}{{newline}}{{var::mainPrompt}}"|
				/re-replace find="/<think>[\s\S]*?<\/think>\s*/g" replace="" {{pipe}}|
				/setvar key=guideTemp {{pipe}}|
				/setvar key=guidance "**Kink Guidance Input:** [{{getvar::guideTemp}}]{{newline}}This reflects a core kink or arousal theme relevant to the character. At least one kink type in the output must reflect this input — directly or as a clear reinterpretation. [**IMPORTANT** Start with this!]"|
			:}|
			/else {:
				/setvar key=guidance "{{var::guidencePrompt_f}}{{newline}}[{{getvar::guideTemp}}]"|
			:}|
			/flushvar guideTemp|
		:}|
		/elseif ( gu == 'Change') {:
			/input default={{getvar::guidance}} Edit what you want the response should be guided towards.|
			/setvar key=guidance "{{pipe}}"|
		:}|
		/var key=task {{noop}}|
		/var key=find "{{var::wi_book_key_f}}: Task"|
		/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
		/var key=task [{{var::taskStarter}}{{pipe}}{{var::taskStopper}}]|
	:}|
	/elseif ( selected_btn == 'Customize Parts of the generation') {:
		/buttons labels=["Yes", "Reset", "No"] Do you want to Customize the {Modifier} of the formula {Modifier} + {Archetype} + {Addition}?|
		/let key=sel "{{pipe}}"|
		/ife (sel == '') {:
			/echo Aborting |
			/abort
		:}|
		/elseif ( sel == 'Yes') {:
			/input Write what you want to have as a Modifier.|
			/setvar key=settingModifier {{pipe}}|
			/ife (sel == '') {:
				/echo Aborting |
				/abort
			:}|
			/else {:
				/setvar key=settingModifier "{Modifier} = {{getvar::settingModifier}}"|
			:}|
		:}|
		/elseif ( sel == 'Reset') {:
			/setvar key=settingModifier {Modifier}|
		:}|
		/buttons labels=["Yes", "Reset", "No"] Do you want to Customize the {Archetype} of the formula {Modifier} + {Archetype} + {Addition}?|
		/var key=sel {{pipe}}|
		/ife (sel == '') {:
			/echo Aborting |
			/abort
		:}|
		/elseif ( sel == 'Yes') {:
			/input Write what you want to have as a Archetype.|
			/setvar key=settingArchetype {{pipe}}|
			/ife (sel == '') {:
				/echo Aborting |
				/abort
			:}|
			/else {:
				/setvar key=settingArchetype "{Archetype} = {{getvar::settingArchetype}}"|
			:}|
		:}|
		/elseif ( sel == 'Reset') {:
			/setvar key=settingArchetype {Archetype}|
		:}|
		/buttons labels=["Yes", "Reset", "No"] Do you want to Customize the {Addition} of the formula {Modifier} + {Archetype} + {Addition}?|
		/var key=sel {{pipe}}|
		/ife (sel == '') {:
			/echo Aborting |
			/abort
		:}|
		/elseif ( sel == 'Yes') {:
			/input Write what you want to have as a Addition.|
			/setvar key=settingAddition {{pipe}}|
			/ife (sel == '') {:
				/echo Aborting |
				/abort
			:}|
			/else {:
				/setvar key=settingAddition "{Addition} = {{getvar::settingAddition}}"|
			:}|
		:}|
		/elseif ( sel == 'Reset') {:
			/setvar key=settingAddition {Addition}|
		:}|
		/var key=task {{noop}}|
		/var key=find "{{var::wi_book_key_f}}: Task"|
		/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
		/var key=task [{{var::taskStarter}}{{pipe}}{{var::taskStopper}}]|
		/ife (debug == 'Yes') {:
			/setvar key="03 Task" {{var::task}}|
		:}|
		/else {:
			/flushvar "03 Task"|
		:}|
		/var key=find "{{var::wi_book_key_f}}: Instruction"|
		/findentry field=comment file="{{var::wi_book_f}}" "{{var::find}}"|
		/var key=wi_uid {{pipe}}|
		/getentryfield field=content file="{{var::wi_book_f}}" {{var::wi_uid}}|
		/var key=instruct [{{var::instructionsStarter}}{{pipe}}{{var::instructionsStopper}}]|
		/ife (debug == 'Yes') {:
			/setvar key="04 Instruktions" [{{var::instruct}}]|
		:}|
		/else {:
			/flushvar "04 Instruktions"|
		:}|
		/var key=t {{noop}}|
	:}|
	/elseif ( selected_btn =='Generate New') {:
		/var key=t {{noop}}|
	:}|
	/elseif ( selected_btn == man) {:
		/ife ( genIsSentence_f == 'Yes' ){:
			
			/getat index=0 {{var::genState}}|
			/input default={{pipe}} Edit the output to your liking.|
		:}|
		/else {:
			/ife (outputIsList_f == 'Yes') {:
				/input rows=8 What {{var::wi_book_key_f}} do you like to add to the {{var::wi_book_key_f}} list?|
			:}|
			/else {:
				/input rows=8 What {{var::wi_book_key_f}} do you like to set as {{var::wi_book_key_f}} list?|
			:}|
		:}|
		/var key=selected_btn {{pipe}}|
		/setvar key=save {{var::selected_btn}}|
		/flushvar guidance|
		/:"CMC Logic.SaveGen"|
	:}|
	/else {:
		/ife (( selected_btn == 'Done') and ((outputIsList_f == 'Yes') and (tempList == '') or (needOutput_f == 'No'))) {:
			/ife (needOutput_f == 'No') {:
				/setvar key=save Done|
			:}|
			/else {:
				/setvar key=save None|
			:}|
			/flushvar guidance|
			/:"CMC Logic.SaveGen"|
		:}|
		/else {:
			/setvar key=save {{var::selected_btn}}|
			/flushvar guidance|
			/:"CMC Logic.SaveGen"|
		:}|
	:}|
:}|
/flushvar previousMilestones|
/flushvar blackListGen|
/flushvar mileS|
/flushvar guidance|