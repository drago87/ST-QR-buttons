/qr-list CMC Main|
/getat index=1 {{pipe}}|
/let qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Outfit" {{pipe}}|

/:"CMC Logic.Get Char info"|

/setvar key=dataBaseNames []|
/flushvar genSettings|

/setvar key=stepVar Step4|

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
/setvar key=outfitsDone Yes|
/let key=do {{noop}}|
/let key=variableName {{noop}}|
/let key=selected_btn {{noop}}|

/let key=json_temp {
	"Nude": {
		"outfit_name": "Nude",
		"headwear": {
			"type": "string",
			"slot": "headwear",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing anything on {{getvar::possAdjPronoun}} head."
		},
		"accessories": {
			"type": "array",
			"slot": "accessory",
			"items": [
				{
					"name": "none",
					"accessory_slot": "Left Ear",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Ear."
				},
				{
					"name": "none",
					"accessory_slot": "Right Ear",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Ear."
				},
				{
					"name": "none",
					"accessory_slot": "Nose Bridge",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Nose Bridge."
				},
				{
					"name": "none",
					"accessory_slot": "Left Nostril",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Nostril."
				},
				{
					"name": "none",
					"accessory_slot": "Right Nostril",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Nostril."
				},
				{
					"name": "none",
					"accessory_slot": "Septum",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Septum."
				},
				{
					"name": "none",
					"accessory_slot": "Lower Lip",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Lower Lip."
				},
				{
					"name": "none",
					"accessory_slot": "Upper Lip",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Upper Lip."
				},
				{
					"name": "none",
					"accessory_slot": "Tongue",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Tongue."
				},
				{
					"name": "none",
					"accessory_slot": "Neck",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Neck."
				},
				{
					"name": "none",
					"accessory_slot": "Left Nipple",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Nipple."
				},
				{
					"name": "none",
					"accessory_slot": "Right Nipple",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Nipple."
				},
				{
					"name": "none",
					"accessory_slot": "Navel",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Navel."
				},
				{
					"name": "none",
					"accessory_slot": "Clitoris",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Clitoris."
				},
				{
					"name": "none",
					"accessory_slot": "Clitoris Hood",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Clitoris Hood."
				},
				{
					"name": "none",
					"accessory_slot": "Left Wrist",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Wrist."
				},
				{
					"name": "none",
					"accessory_slot": "Right Wrist",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Wrist."
				},
				{
					"name": "none",
					"accessory_slot": "Left Hand Thumb",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Hand Thumb."
				},
				{
					"name": "none",
					"accessory_slot": "Left Hand Index Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Hand Index Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Left Hand Middle Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Hand Middle Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Left Hand Ring Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Hand Ring Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Left Hand Pinky Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Hand Pinky Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Right Hand Thumb",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Hand Thumb."
				},
				{
					"name": "none",
					"accessory_slot": "Right Hand Index Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Hand Index Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Right Hand Middle Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Hand Middle Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Right Hand Ring Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Hand Ring Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Right Hand Pinky Finger",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Hand Pinky Finger."
				},
				{
					"name": "none",
					"accessory_slot": "Left Ankle",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Ankle."
				},
				{
					"name": "none",
					"accessory_slot": "Right Ankle",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Ankle."
				},
				{
					"name": "none",
					"accessory_slot": "Left Foot Big Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Foot Big Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Left Foot Second Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Foot Second Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Left Foot Middle Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Foot Middle Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Left Foot Fourth Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Foot Fourth Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Left Foot Little Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Left Foot Little Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Right Foot Big Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Foot Big Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Right Foot Second Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Foot Second Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Right Foot Middle Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Foot Middle Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Right Foot Fourth Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Foot Fourth Toe."
				},
				{
					"name": "none",
					"accessory_slot": "Right Foot Little Toe",
					"description": "{{getvar::firstName}} is not wearing an accessory on {{getvar::possAdjPronoun}} Right Foot Little Toe."
				}
			]
		},
		"makeup": {
			"type": "array",
			"slot": "makeup",
			"items": [
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Eyeshadow",
					"description": "{{getvar::firstName}} is not wearing any eyeshadow."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Eyeliner",
					"description": "{{getvar::firstName}} is not wearing any eyeliner."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Mascara",
					"description": "{{getvar::firstName}} is not wearing any mascara."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Eyebrows",
					"description": "{{getvar::firstName}} has not applied any product to {{getvar::possAdjPronoun}} eyebrows."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Lipstick",
					"description": "{{getvar::firstName}} is not wearing any lipstick or lip color."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Lip Liner",
					"description": "{{getvar::firstName}} is not wearing any lip liner."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Blush",
					"description": "{{getvar::firstName}} is not wearing any blush on {{getvar::possAdjPronoun}} cheeks."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Foundation",
					"description": "{{getvar::firstName}} is not wearing any foundation or base makeup."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Face Paint",
					"description": "{{getvar::firstName}} does not have any paint or markings on {{getvar::possAdjPronoun}} face."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Left Hand Fingernails",
					"description": "{{getvar::firstName}} is not wearing any polish on {{getvar::possAdjPronoun}} left fingernails."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Right Hand Fingernails",
					"description": "{{getvar::firstName}} is not wearing any polish on {{getvar::possAdjPronoun}} right fingernails."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Left Foot Toenails",
					"description": "{{getvar::firstName}} is not wearing any polish on {{getvar::possAdjPronoun}} left toenails."
				},
				{
					"name": "none",
					"permanent": "No",
					"makeup_slot": "Right Foot Toenails",
					"description": "{{getvar::firstName}} is not wearing any polish on {{getvar::possAdjPronoun}} right toenails."
				}
			],
		},
		"neckwear": {
			"type": "string",
			"slot": "neckwear",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing anything around {{getvar::possAdjPronoun}} neck that is not an accessory."
		},
		"outfit_type": "one",
		"mainwear_full": {
			"type": "string",
			"slot": "mainwear_full",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing a **outer layer** of clothing on {{getvar::possAdjPronoun}}."
		},
		"mainwear_top": {
			"type": "string",
			"slot": "mainwear_top",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing a **outer layer** of clothing on {{getvar::possAdjPronoun}} top half."
		},
		"mainwear_bottom": {
			"type": "string",
			"slot": "mainwear_bottom",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing a **outer layer** of clothing on {{getvar::possAdjPronoun}} bottom half."
		},
		"legwear": {
			"type": "string",
			"slot": "legwear",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing any legwear."
		},
		"socks": {
			"type": "string",
			"slot": "socks",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing any socks."
		},
		"shoes": {
			"type": "string",
			"slot": "shoes",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing any shoes."
		},
		"underwear_top": {
			"type": "string",
			"slot": "underwear top",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing any underwear on {{getvar::possAdjPronoun}} upper body."
		},
		"underwear_bottom": {
			"type": "string",
			"slot": "underwear bottom",
			"name": "none",
			"description": "{{getvar::firstName}} is not wearing any underwear on {{getvar::possAdjPronoun}} lower body."
		}
	}
}|
/json-pretty {{var::json_temp}}|
/setvar as=object key=outfits {{pipe}}|


/ife (overallOutfit == '') {:
	/input Give the outfit a name<div>Example1: School Uniform</div><div>Example2: School Swimsuit</div><div>Example3: Office Outfit</div><div>Writing `No Outfit` will only add the nudity Lorbook Entries</div>|
	/setvar key=overallOutfit {{pipe}}|
	/ife ( overallOutfit == ''){:
		/echo Aborting |
		/abort
	:}|
	/else {:
		/setvar key=overallOutfit {{var::selected_btn}}|
	:}|
:}|

/ife (overallOutfit != 'No Outfit') {:
	/let key=keepGoing Yes|
	/whilee (keepGoing) {:
		/ife (overallOutfit == '') {:
			/input Give the outfit a name<div>Example1: School Uniform</div><div>Example2: School Swimsuit</div><div>Example3: Office Outfit</div><div>Writing `No Outfit` will only add the nudity Lorbook Entries</div>|
			/setvar key=overallOutfit {{pipe}}|
			/ife ( overallOutfit == ''){:
				/echo Aborting |
				/abort
			:}|
			/else {:
				/setvar key=overallOutfit {{var::selected_btn}}|
			:}|
		:}|
		//Outfit Head|
		/var key=do No|
		/var key=variableName "outfitHead"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Head"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 20|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Head Description|
		/ife (outfitHead != 'None') {:
			/var key=do No|
			/var key=variableName "outfitHeadDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Head Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/addvar key=extra "- Hair: {{getvar::appearanceHair}}"|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::parsedSpecies}} includes visible head features (ears, horns, fins, wings, etc), consider how they affect fit or placement of the headwear."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If any of the Features includes scars, prosthetics, cybernetics, piercings, or other physical modifications, consider how they visually contrast with or influence the item’s appearance or placement."|
				:}|
				
				/getvar key=genSettings index=inputIsList|
				/let key=inputIsList {{pipe}}|
				/getvar key=genSettings index=inputIsList|
				/let key=outputIsList {{pipe}}|
				
				/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitHeadDescription None|
			/addvar key=dataBaseNames outfitHeadDescription|
		:}|
		//--------|
		
		//Outfit Accessories|
		/var key=do No|
		/var key=variableName "outfitAccessories"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Accessories"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput No|
			/setvar key=genSettings index=outputIsList Yes|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		/setvar key=outfitAccesorySlots []|
		/ife (outfitAccessories != 'None') {:
			/let key=slotSelections ["Left Ear", "Right Ear", "Nose Bridge", "Left Nostril", "Right Nostril", "Septum", "Lower Lip", "Upper Lip", "Tongue", "Neck", "Left Nipple", "Right Nipple", "Navel", "Clitoris", "Clitoris Hood","Left Wrist", "Right Wrist", "Left Hand Thumb", "Left Hand Index Finger", "Left Hand Middle Finger", "Left Hand Ring Finger", "Left Hand Pinky Finger", "Right Hand Thumb", "Right Hand Index Finger", "Right Hand Middle Finger", "Right Hand Ring Finger", "Right Hand Pinky Finger","Left Ankle", "Right Ankle", "Left Foot Big Toe", "Left Foot Second Toe", "Left Foot Middle Toe", "Left Foot Fourth Toe", "Left Foot Little Toe", "Right Foot Big Toe", "Right Foot Second Toe", "Right Foot Middle Toe", "Right Foot Fourth Toe", "Right Foot Little Toe", "Other"]|
			/foreach {{getvar::outfitAccessories}} {:
				/buttons labels={{var::slotSelections}} Select the accessory slot you want for {{var::item}}|
				/var key=selected_btn {{pipe}}|
				/ife (selected_btn == '') {:
					/echo Aborting |
					/abort
				:}|
				/elseif (selected_btn =='Other') {:
					/input Write the accessory slot you want {{var::item}} to be equiped in.|
					/var key=selected_btn {{pipe}}|
					/ife (selected_btn == '') {:
						/echo Aborting |
						/abort
					:}|
				:}|
				/addvar key=outfitAccesorySlots {{var::selected_btn}}|
			:}|
		:}|
		//Outfit Accessories Description|
		/ife (outfitAccessories != 'None') {:
			/var key=do No|
			/var key=variableName "outfitAccessoriesDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Accessories Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList Yes|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::parsedSpecies}} includes tails, horns, paws, wings, or other non-human limbs, consider how this affects where or how the accessory is worn."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
				/addvar key=logicBasedInstruction {{newline}}|
			:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes prosthetics, piercings, cybernetics, or scars, consider how the accessory interacts with or highlights these features."|
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
					/foreach {{getvar::outfitAccessories}} {:
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
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitAccessoriesDescription None|
			/addvar key=dataBaseNames outfitAccessoriesDescription|
		:}|
		//--------|
		/wait {{getvar::wait}}|
		/setvar key=parsedAccessories {{noop}}|
		/ife (outfitAccessoriesDescription == 'None') {:
			/addvar key=parsedAccessories "{{getvar::outfitAccessoriesDescription}}"|
		:}|
		/elseif (outfitAccessoriesDescription is list) {:
			/foreach {{getvar::outfitAccessoriesDescription}} {:
				/ife (index > 0) {:
					/addvar key=parsedAccessories {{newline}}|
				:}|
				/addvar key=parsedAccessories "  - {{var::item}}"|
			:}|
		:}|
		/addvar key=dataBaseNames parsedAccessories|
		
		//Outfit Makeup|
		/var key=do No|
		/var key=variableName "outfitMakeup"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Makeup"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		/setvar key=outfitMakeupSlots []|
		/setvar key=outfitMakeupPermament []
		/ife (outfitMakeup != 'None') {:
			/let key=slotSelections ["Eyeshadow", "Eyeliner", "Mascara", "Eyebrows", "Lipstick", "Lip Liner", "Blush", "Foundation", "Face Paint", "Left Hand Fingernails", "Right Hand Fingernails", "Left Foot Toenails", "Right Foot Toenails", "Other"]|
			/foreach {{getvar::outfitMakeup}} {:
				/buttons labels={{var::slotSelections}} Select the makeup slot you want for {{var::item}}|
				/var key=selected_btn {{pipe}}|
				/ife (selected_btn == '') {:
					/echo Aborting |
					/abort
				:}|
				/elseif (selected_btn =='Other') {:
					/input Write the makeup slot you want {{var::item}} to be applied to.|
					/var key=selected_btn {{pipe}}|
					/ife (selected_btn == '') {:
						/echo Aborting |
						/abort
					:}|
				:}|
				/addvar key=outfitMakeupSlots {{var::selected_btn}}|
				/buttons labels=["Yes", "No"] Is {{var::item}} something Permament?|
				/var key=selected_btn {{pipe}}|
				/ife (selected_btn == '') {:
					/echo Aborting |
					/abort
				:}|
				/addvar key=outfitMakeupPermament {{var::selected_btn}}|
			:}|
		:}|
		
		//Outfit Makeup Description|
		/ife (outfitMakeup != 'None') {:
			/var key=do No|
			/var key=variableName "outfitMakeupDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Makeup Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList Yes|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, feathers, or non-human markings, ensure the makeup is applied in visible or exposed areas — such as facial skin patches, ridges, horns, or ceremonial markings — and accounts for species texture."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes prosthetics, cybernetics, scars, piercings, or skin conditions, consider how the makeup highlights or contrasts with these features visually."|
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
					/foreach {{getvar::outfitMakeup}} {:
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
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitMakeupDescription None|
			/addvar key=dataBaseNames outfitMakeupDescription|
		:}|
		//--------|
		/wait {{getvar::wait}}|
		/setvar key=parsedMakeup {{noop}}|
		/ife (outfitMakeupDescription == 'None') {:
			/addvar key=parsedMakeup " {{getvar::outfitMakeupDescription}}"|
		:}|
		/elseif (outfitMakeupDescription is list) {:
			/foreach {{getvar::outfitMakeupDescription}} {:
				/addvar key=parsedMakeup "{{newline}}  - {{var::item}}"|
			:}|
		:}|
		/addvar key=dataBaseNames parsedMakeup|
		
		//Outfit Neck|
		/var key=do No|
		/var key=variableName "outfitNeck"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Neck"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Neck Description|
		/ife (outfitNeck != 'None') {:
			/var key=do No|
			/var key=variableName "outfitNeckDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Neck Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, manes, ruffs, or neck-based traits (e.g., gills, fins, feathers), describe how the neckwear fits, wraps around, or contrasts with those features."|
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes scars, prosthetics, piercings, or visible augmentations around the neck or upper torso, reflect how the neckwear interacts with or complements these features."|
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitNeckDescription None|
			/addvar key=dataBaseNames outfitNeckDescription|
		:}|
		//--------|
		
		/ife (mainwearType == '') {:
			/buttons labels=["One-Piece", "Two-Piece"] Do you want to have a One-Piece outfit or a Two-Piece outfit?|
			/var key=selected_btn {{pipe}}|
			/ife (selected_btn == '') {:
				/echo Aborting |
				/abort
			:}|
			/setvar key=mainwearType {{var::selected_btn}}|
		:}|
		
		/ife ( mainwearType == 'One-Piece') {:
			//Outfit One-Piece|
			/var key=do No|
			/var key=variableName "outfitMainwear"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Mainwear"|
				/setvar key=genSettings index=genIsList Yes|
				/setvar key=genSettings index=genAmount 8|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence No|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
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
				
				/ife (overallOutfit != 'No') {:
					/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
				
				/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
			//--------|
			
			//Outfit One-Piece Description|
			/ife (outfitMainwear != 'None') {:
				/var key=do No|
				/var key=variableName "outfitMainwearDescription"|
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
					/setvar key=genSettings index=wi_book_key "Outfit Mainwear Description"|
					/setvar key=genSettings index=genIsList No|
					/setvar key=genSettings index=inputIsList No|
					/setvar key=genSettings index=inputIsTaskList No|
					/setvar key=genSettings index=genIsSentence Yes|
					/setvar key=genSettings index=needOutput Yes|
					/setvar key=genSettings index=outputIsList No|
					/setvar key=genSettings index=useContext Yes|
					/setvar key=extra []|
					/ife ( appearanceFeatures != 'None') {:
						/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
					:}|
					/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
					/ife ( appearanceBreasts != 'None') {:
						/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
					:}|
					/setvar key=genSettings index=extraContext {{getvar::extra}}|
					/setvar key=extra []|
					/:"CMC Logic.Get Basic Type Context"|
					/ife (extra != '') {:
						/setvar key=genSettings index=contextKey {{getvar::extra}}|
					:}|
					/flushvar extra|
					/wait {{getvar::wait}}|
					
					/setvar key=logicBasedInstruction {{noop}}|
					
					/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes wings, tail bases, dorsal fins, fur crests, or unusual body shapes, describe how the garment is shaped or opened to accommodate those features."|
					:}|
					/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical gear, prosthetics, or cybernetics on the torso or pelvis, describe how the garment adjusts to fit, support, or conceal them."|
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
					
					/:"CMC Logic.GenerateWithPrompt"|
					/setvar key={{var::variableName}} {{getvar::output}}|
					
					/addvar key=dataBaseNames {{var::variableName}}|
					/flushvar output|
					/flushvar logicBasedInstruction|
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
				/setvar key=outfitMainwearDescription None|
				/addvar key=dataBaseNames outfitMainwearDescription|
			:}|
			//--------|
		:}|
		/else {:
			/setvar key=outfitMainwear Skip|
			/addvar key=dataBaseNames outfitMainwear|
			/setvar key=outfitMainwearDescription Skip|
			/addvar key=dataBaseNames outfitMainwearDescription|
		:}|
		
		/ife ( mainwearType == 'Two-Piece') {:
			//Outfit Top|
			/var key=do No|
			/var key=variableName "outfitTop"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Top"|
				/setvar key=genSettings index=genIsList Yes|
				/setvar key=genSettings index=genAmount 10|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence No|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
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
				
				/ife (overallOutfit != 'No') {:
					/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
				
				/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
			//--------|
			
			//Outfit Top Description|
			/ife (outfitTop != 'None') {:
				/var key=do No|
				/var key=variableName "outfitTopDescription"|
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
					/setvar key=genSettings index=wi_book_key "Outfit Top Description"|
					/setvar key=genSettings index=genIsList No|
					/setvar key=genSettings index=inputIsList No|
					/setvar key=genSettings index=inputIsTaskList No|
					/setvar key=genSettings index=genIsSentence Yes|
					/setvar key=genSettings index=needOutput Yes|
					/setvar key=genSettings index=outputIsList No|
					/setvar key=genSettings index=useContext Yes|
					/setvar key=extra []|
					/ife ( appearanceFeatures != 'None') {:
						/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
					:}|
					/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
					/ife ( appearanceBreasts != 'None') {:
						/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
					:}|
					/setvar key=genSettings index=extraContext {{getvar::extra}}|
					/setvar key=extra []|
					/:"CMC Logic.Get Basic Type Context"|
					/ife (extra != '') {:
						/setvar key=genSettings index=contextKey {{getvar::extra}}|
					:}|
					/flushvar extra|
					/wait {{getvar::wait}}|
		
					/setvar key=logicBasedInstruction {{noop}}|
					
					/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes wings, tail bases, dorsal fins, fur crests, or unusual body shapes, describe how the top is shaped or adapted to fit those features comfortably or functionally."|
					:}|
					/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical supports, prosthetics, or cybernetics on the torso or arms, describe how the top accommodates or interacts with them."|
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
					
					/:"CMC Logic.GenerateWithPrompt"|
					/setvar key={{var::variableName}} {{getvar::output}}|
					
					/addvar key=dataBaseNames {{var::variableName}}|
					/flushvar output|
					/flushvar logicBasedInstruction|
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
				/setvar key=outfitTopDescription None|
				/addvar key=dataBaseNames outfitTopDescription|
			:}|
			//--------|
			
			//Outfit Bottom|
			/var key=do No|
			/var key=variableName "outfitBottom"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Bottom"|
				/setvar key=genSettings index=genIsList Yes|
				/setvar key=genSettings index=genAmount 10|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence No|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
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
				
				/ife (overallOutfit != 'No') {:
					/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
				
				/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
			//--------|
			
			//Outfit Bottom Description|
			/ife (outfitTop != 'None') {:
				/var key=do No|
				/var key=variableName "outfitBottomDescription"|
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
					/setvar key=genSettings index=wi_book_key "Outfit Bottom Description"|
					/setvar key=genSettings index=genIsList No|
					/setvar key=genSettings index=inputIsList No|
					/setvar key=genSettings index=inputIsTaskList No|
					/setvar key=genSettings index=genIsSentence Yes|
					/setvar key=genSettings index=needOutput Yes|
					/setvar key=genSettings index=outputIsList No|
					/setvar key=genSettings index=useContext Yes|
					/setvar key=extra []|
					/ife ( appearanceFeatures != 'None') {:
						/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
					:}|
					/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
					/setvar key=genSettings index=extraContext {{getvar::extra}}|
					/setvar key=extra []|
					/:"CMC Logic.Get Basic Type Context"|
					/ife (extra != '') {:
						/setvar key=genSettings index=contextKey {{getvar::extra}}|
					:}|
					/flushvar extra|
					/wait {{getvar::wait}}|
		
					/setvar key=logicBasedInstruction {{noop}}|
					
					/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes tails, fur, multiple legs, or digitigrade limbs, describe how {{getvar::outfitBottom}} adapts to the species-specific lower anatomy."|
					:}|
					/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes braces, prosthetics, or assistive gear on the lower body, describe how {{getvar::outfitBottom}} fits or adjusts to accommodate them."|
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
					
					/:"CMC Logic.GenerateWithPrompt"|
					/setvar key={{var::variableName}} {{getvar::output}}|
					
					/addvar key=dataBaseNames {{var::variableName}}|
					/flushvar output|
					/flushvar logicBasedInstruction|
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
				/setvar key=outfitBottomDescription None|
				/addvar key=dataBaseNames outfitBottomDescription|
			:}|
			//--------|
		:}|
		/else {:
			/setvar key=outfitTop Skip|
			/addvar key=dataBaseNames outfitTop|
			/setvar key=outfitTopDescription Skip|
			/addvar key=dataBaseNames outfitTopDescription|
			/setvar key=outfitBottom Skip|
			/addvar key=dataBaseNames outfitBottom|
			/setvar key=outfitBottomDescription Skip|
			/addvar key=dataBaseNames outfitBottomDescription|
		:}|
		
		//Outfit Legs|
		/var key=do No|
		/var key=variableName "outfitLegwear"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Legwear"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
			/setvar key=extra []|
			/ife (appearanceFeatures != 'None') {:
				/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
			:}|
			/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
			/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
			/addvar key=extra "- Bottom Outfit: {{getvar::outfitBottom}}"|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Legs Description|
		/ife (outfitLegwear != 'None') {:
			/var key=do No|
			/var key=variableName "outfitLegwearDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Legwear Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife (appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
				/addvar key=extra "- Species Group: {{getvar::speciesGroup}}"|
				/addvar key=extra "- Bottom Outfit: {{getvar::outfitBottom}}"|
				/setvar key=genSettings index=extraContext {{getvar::extra}}|
				/setvar key=extra []|
				/:"CMC Logic.Get Basic Type Context"|
				/ife (extra != '') {:
					/setvar key=genSettings index=contextKey {{getvar::extra}}|
				:}|
				/flushvar extra|
				/wait {{getvar::wait}}|
				
				/setvar key=logicBasedInstruction {{noop}}|
				
				/ife ((('tail' in appearanceFeatures ) or ('Tail' in appearanceFeatures )) and (('wings' in appearanceFeatures ) or ('Wings' in appearanceFeatures ))) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If both tail and wings are present, the outfit should account for both anatomical openings or contours without compromising garment structure. Prioritize ergonomic design over decorative cutouts."|
					
				:}|
				/elseif (('wings' in appearanceFeatures ) or ('Wings' in appearanceFeatures )) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has wings, the outfit must either leave the upper back open, include structured wing slots, or have tailored cutouts to avoid restricting motion. Do not ignore wing presence in the garment fit."|
					
				:}|
				/elseif (('tail' in appearanceFeatures ) or ('Tail' in appearanceFeatures )) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has a tail, ensure the garment includes a slit, flap, stretch opening, or contour that accommodates tail placement and movement. Placement should appear intentional and integrated."|
					
				:}|
				/elseif ((('tail' not in appearanceFeatures ) and ('Tail' not in appearanceFeatures )) and (('wings' not in appearanceFeatures ) or ('Wings' not in appearanceFeatures ))) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- Do not include any garment openings or anatomical adaptations for tails or wings, as none are present."|
					
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
				
				/:"CMC Logic.GenerateWithPrompt"|
				/setvar key={{var::variableName}} {{getvar::output}}|
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitLegwearDescription None|
			/addvar key=dataBaseNames outfitLegwearDescription|
		:}|
		//--------|
		
		//Outfit Socks|
		/var key=do No|
		/var key=variableName "outfitSocks"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Socks"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Socks Description|
		/ife (outfitSocks != 'None') {:
			/var key=do No|
			/var key=variableName "outfitSocksDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Socks Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has paws, claws, hooves, or digitigrade limbs, describe how the socks are shaped, opened, or adjusted to fit their structure."|
					
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has prosthetics, braces, or other medical features, describe how the socks provide support or structural compatibility."|
					
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
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitsocksDescription None|
			/addvar key=dataBaseNames outfitsocksDescription|
		:}|
		//--------|
		
		//Outfit Shoes|
		/var key=do No|
		/var key=variableName "outfitShoes"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Shoes"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 10|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Shoes Description|
		/ife (outfitShoes != 'None') {:
			/var key=do No|
			/var key=variableName "outfitShoesDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Shoes Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has paws, claws, hooves, or digitigrade limbs, describe how the shoes are shaped, opened, or adjusted to fit their structure."|
					
				:}|
				/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If the character has prosthetics, braces, or other medical features, describe how the shoes provide support or structural compatibility."|
					
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
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
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
			/setvar key=outfitShoesDescription None|
			/addvar key=dataBaseNames outfitShoesDescription|
		:}|
		//--------|
		
		/ife (appearanceBreasts != 'None') {:
			//Outfit Underwear (Top)|
			/var key=do No|
			/var key=variableName "outfitUnderwearTop"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Underwear Top"|
				/setvar key=genSettings index=genIsList Yes|
				/setvar key=genSettings index=genAmount 5|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence No|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
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
				
				/ife (overallOutfit != 'No') {:
					/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
				
				/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
			//--------|
			
			//Outfit Underwear (Top) Description|
			/ife (outfitUnderwearTop != 'None') {:
				/var key=do No|
				/var key=variableName "outfitUnderwearTopDescription"|
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
					/setvar key=genSettings index=wi_book_key "Outfit Underwear Top Description"|
					/setvar key=genSettings index=genIsList No|
					/setvar key=genSettings index=inputIsList No|
					/setvar key=genSettings index=inputIsTaskList No|
					/setvar key=genSettings index=genIsSentence Yes|
					/setvar key=genSettings index=needOutput Yes|
					/setvar key=genSettings index=outputIsList No|
					/setvar key=genSettings index=useContext Yes|
					/setvar key=extra []|
					/ife ( appearanceFeatures != 'None') {:
						/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
					:}|
					/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
					/ife ( appearanceBreasts != 'None') {:
						/addvar key=extra "- Breasts: {{getvar::appearanceBreasts}}"|
					:}|
					/setvar key=genSettings index=extraContext {{getvar::extra}}|
					/setvar key=extra []|
					/:"CMC Logic.Get Basic Type Context"|
					/ife (extra != '') {:
						/setvar key=genSettings index=contextKey {{getvar::extra}}|
					:}|
					/flushvar extra|
					/wait {{getvar::wait}}|
					
					/setvar key=logicBasedInstruction {{noop}}|
					
					/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes fur, scales, extra limbs, or an unusual torso shape, describe how {{getvar::outfitUnderwearTop}} adapts to or works with these features."|
					:}|
					/elseif (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
						/ife ( logicBasedInstruction != '') {:
							/addvar key=logicBasedInstruction {{newline}}|
						:}|
						/setvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes medical gear, scars, or prosthetics, note how {{getvar::outfitUnderwearTop}} provides comfort or coverage in those areas."|
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
					
					/:"CMC Logic.GenerateWithPrompt"|
					/setvar key={{var::variableName}} {{getvar::output}}|
					
					/addvar key=dataBaseNames {{var::variableName}}|
					/flushvar output|
					/flushvar logicBasedInstruction|
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
				/setvar key=outfitUnderwearTopDescription None|
				/addvar key=dataBaseNames outfitUnderwearTopDescription|
			:}|
			//--------|
		:}|
		/else {:
			/setvar key=outfitUnderwearTop Skip|
			/setvar key=outfitUnderwearTopDescription Skip|
			/addvar key=dataBaseNames outfitUnderwearTop|
			/addvar key=dataBaseNames outfitUnderwearTopDescription|
		:}|
		
		//Outfit Underwear (Bottom)|
		/var key=do No|
		/var key=variableName "outfitUnderwearBottom"|
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
			/setvar key=genSettings index=wi_book_key "Outfit Underwear Bottom"|
			/setvar key=genSettings index=genIsList Yes|
			/setvar key=genSettings index=genAmount 5|
			/setvar key=genSettings index=inputIsTaskList No|
			/setvar key=genSettings index=genIsSentence No|
			/setvar key=genSettings index=needOutput Yes|
			/setvar key=genSettings index=outputIsList No|
			/setvar key=genSettings index=useContext Yes|
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
			
			/ife (overallOutfit != 'No') {:
				/setvar key=guidance "The response should be guided toward: {{getvar::overallOutfit}}"|
			:}|
			
			/ife ((outputIsList == 'Yes') or (outputIsList == 'Yes')) {:
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
		//--------|
		
		//Outfit Underwear (Bottom) Description|
		/ife (outfitUnderwearBottom != 'None') {:
			/var key=do No|
			/var key=variableName "outfitUnderwearBottomDescription"|
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
				/setvar key=genSettings index=wi_book_key "Outfit Underwear Bottom Description"|
				/setvar key=genSettings index=genIsList No|
				/setvar key=genSettings index=inputIsList No|
				/setvar key=genSettings index=inputIsTaskList No|
				/setvar key=genSettings index=genIsSentence Yes|
				/setvar key=genSettings index=needOutput Yes|
				/setvar key=genSettings index=outputIsList No|
				/setvar key=genSettings index=useContext Yes|
				/setvar key=extra []|
				/ife ( appearanceFeatures != 'None') {:
					/addvar key=extra "{{getvar::parsedAppearanceFeatures}}"|
				:}|
				/addvar key=extra "- Body: {{getvar::appearanceBody}}"|
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
				
				/ife (( characterArchetype != 'Human') and ( characterArchetype != 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes tails, fur, digitigrade legs, extra limbs, or tauric anatomy, mention how {{getvar::outfitUnderwearBottom}} accommodates or wraps around them."|
					
				:}|
				/elseif ((( characterArchetype == 'Human') or ( characterArchetype == 'Android')) and (appearanceFeatures != 'None')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- If {{getvar::possAdjPronoun}} Features includes scars, braces, or prosthetics around the hips or legs, describe how {{getvar::outfitUnderwearBottom}} adjusts or provides comfort."|
					
				:}|
				/ife (( characterArchetype == 'Human') or ( characterArchetype == 'Android')) {:
					
					/ife ( logicBasedInstruction != '') {:
						/addvar key=logicBasedInstruction {{newline}}|
					:}|
					/addvar key=logicBasedInstruction "- Do not mention tails, fur, digitigrade joints, claws, wings, extra limbs, or species-based adaptations. Describe only human anatomy and fit."|
					
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
				
				/addvar key=dataBaseNames {{var::variableName}}|
				/flushvar output|
				/flushvar logicBasedInstruction|
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
			
			/addvar key=dataBaseNames outfitUnderwearBottomDescription|
		:}|
		//--------|
		
		/Add to Variable|
		/let key=outfit_obj {}|
		/var key=outfit_obj index=outfit_name {{getvar::overallOutfit}}|
		
		/ife (outfitHeadWear != 'none') {:
			/let key=head_obj {}|
			/var key=head_obj index=name {{getvar::outfitHeadWear}}|
			/ife (outfitHeadWear != 'none') {:
				/var key=head_obj index=description {{getvar::outfitHeadDescription}}|
			:}|
			/else {:
				/var key=head_obj index=description "{{getvar::firstName}} is not wearing anything on {{getvar::possAdjPronoun}} head."|
			:}|
			/var key=head_obj index=type string|
			/var key=head_obj index=slot headwear|
			/var key=outfit_obj index=headwear {{var::head_obj}}|
		:}|
		
		/ife ((outfitAccessories is list) and outfitAccessories != 'none') {:
			/let key=accessories_obj_ {}|
			/setvar key=accessories_arr_obj []|
			/let key=accessories_obj {}|
			/foreach {{getvar::outfitAccessories}} {:
				/var key=accessories_obj index=name {{var::item}}|
				/getvar key=outfitAccessoriesDescription index={{var::index}}|
				/let key=temp_acc {{pipe}}|
				/ife (item != 'none') {:
					/var key=accessories_obj index=description {{var::temp_acc}}|
				:}|
				/else {:
					/var key=accessories_obj index=description "{{getvar::firstName}} is not wearing any accessories."|
				:}|
				/getvar key=outfitAccesorySlots index={{var::index}}|
				/var key=accessories_obj index=accessory_slot {{pipe}}|
				
				/addvar key=accessories_arr_obj index={{var::index}} {{var::accessories_obj}}|
				/var key=accessories_obj {}|
			:}|
			/var key=makeup_obj_ index=type array|
			/var key=makeup_obj_ index=slot accessory|
			/var key=makeup_obj_ index=items {{getvar::accessories_arr_obj}}|
			/var key=outfit_obj index=accessories {{var::makeup_obj_}}|
			/flushvar accessories_arr_obj|
		:}|
		
		/ife ((outfitMakeup is list) and (outfitMakeup != 'none')) {:
			/let key=makeup_obj_ {}|
			/setvar key=makeup_arr_obj []|
			/let key=makeup_obj {}|
			/foreach {{getvar::outfitMakeup}} {:
				/var key=makeup_obj index=name {{var::item}}|
				/getvar key=outfitMakeupDescription index={{var::index}}|
				/let key=temp_make {{pipe}}|
				/ife (item != 'none') {:
					/var key=makeup_obj index=description {{var::temp_make}}|
				:}|
				/else {:
					/var key=makeup_obj index=description "{{getvar::firstName}} is not wearing any makeup."|
				:}|
				/getvar key=outfitMakeupSlots index={{var::index}}|
				/var key=makeup_obj index=accessory_slot {{pipe}}|
				
				/getvar key=outfitMakeupPermament index={{var::index}}|
				/var key=makeup_obj index=permanent {{pipe}}|
				
				/addvar key=makeup_arr_obj index={{var::index}} {{var::makeup_obj}}|
				/var key=makeup_obj {}|
			:}|
			/var key=makeup_obj_ index=type array|
			/var key=makeup_obj_ index=slot makeup|
			/var key=makeup_obj_ index=items {{getvar::makeup_arr_obj}}|
			/var key=outfit_obj index=makeup {{var::makeup_obj_}}|
			/fluschvar makeup_arr_obj|
		:}|
		/ife (outfitNeckWear != 'none') {:
			/let key=neck_obj {}|
			/var key=neck_obj index=name {{getvar::outfitNeckWear}}|
		
			/var key=neck_obj index=description {{getvar::outfitNeckWearDescription}}|
		
			/var key=neck_obj index=type string|
			/var key=neck_obj index=slot neckwear|
			/var key=outfit_obj index=neckwear {{var::neck_obj}}|
		:}|
		
		
		/let key=outfit_type {}|
		/ife (mainwearType == 'One-Piece') {:
			/var key=outfit_type index=outfit_type One|
		:}|
		/else {:
			/var key=outfit_type index=outfit_type Two|
		:}|
		/var key=outfit_obj index=outfit_type {{var::outfit_type}}|
		
		/ife (mainwearType == 'One-Piece') {:
			/ife (outfitMainwear != 'none') {:
				/let key=mainwear_obj {}|
				/var key=mainwear_obj index=name {{getvar::outfitMainwear}}|
			
				/var key=mainwear_obj index=description {{getvar::outfitMainwearDescription}}|
			
				/var key=mainwear_obj index=type string|
				/var key=mainwear_obj index=slot mainwear|
				/var key=outfit_obj index=mainwear_full {{var::mainwear_obj}}|
			:}|
		:}|
		/else {:
			/ife (outfitTop != 'none') {:
				/let key=outfit_top_obj {}|
				/var key=outfit_top_obj index=name {{getvar::outfitTop}}|
			
				/var key=outfit_top_obj index=description {{getvar::outfitTopDescription}}|
			
				/var key=outfit_top_obj index=type string|
				/var key=outfit_top_obj index=slot mainwear top|
				/var key=outfit_obj index=mainwear_top {{var::outfit_top_obj}}|
			:}|
			
			/ife (outfitBottom != 'none') {:
				/let key=outfit_bottom_obj {}|
				/var key=outfit_bottom_obj index=name {{getvar::outfitBottom}}|
			
				/var key=outfit_bottom_obj index=description {{getvar::outfitBottomDescription}}|
			
				/var key=outfit_bottom_obj index=type string|
				/var key=outfit_bottom_obj index=slot mainwear bottom |
				/var key=outfit_obj index=mainwear_bottom {{var::outfit_bottom_obj}}|
			:}|
		:}|
		
		/ife (outfitLegwear != 'none') {:
			/let key=legwear_obj {}|
			/var key=legwear_obj index=name {{getvar::outfitLegwear}}|
			
			/var key=legwear_obj index=description {{getvar::outfitLegwearDescription}}|
			
			/var key=legwear_obj index=type string|
			/var key=legwear_obj index=slot legwear |
			/var key=outfit_obj index=legwear {{var::legwear_obj}}|
		:}|
		
		/ife (outfitSocks != 'none') {:
			/let key=socks_obj {}|
			/var key=socks_obj index=name {{getvar::outfitSocks}}|
		
			/var key=socks_obj index=description {{getvar::outfitSocksDescription}}|
			
			/var key=socks_obj index=type string|
			/var key=socks_obj index=slot socks |
			/var key=outfit_obj index=socks {{var::socks_obj}}|
		:}|
		
		/ife (outfitShoes != 'none') {:
			/let key=shoes_obj {}|
			/var key=shoes_obj index=name {{getvar::outfitShoes}}|
			
			/var key=shoes_obj index=description {{getvar::outfitShoesDescription}}|
			
			/var key=shoes_obj index=type string|
			/var key=shoes_obj index=slot shoes |
			/var key=outfit_obj index=shoes {{var::shoes_obj}}|
		:}|
		
		/ife (outfitUnderwearTop != 'none') {:
			/let key=underwear_top_obj {}|
			/var key=underwear_top_obj index=name {{getvar::outfitUnderwearTop}}|
			
			/var key=underwear_top_obj index=description {{getvar::outfitUnderwearTopDescription}}|
			
			/var key=underwear_top_obj index=type string|
			/var key=underwear_top_obj index=slot underwear top |
			/var key=outfit_obj index=underwear_top {{var::underwear_top_obj}}|
		:}|
		
		/ife (outfitUnderwearBottom != 'none') {:
			/let key=underwear_bottom_obj {}|
			/var key=underwear_bottom_obj index=name {{getvar::outfitUnderwearBottom}}|
			
			/var key=underwear_bottom_obj index=description {{getvar::outfitUnderwearBottomDescription}}|
			
			/var key=underwear_bottom_obj index=type string|
			/var key=underwear_bottom_obj index=slot underwear bottom |
			/var key=outfit_obj index=underwear_bottom {{var::underwear_bottom_obj}}|
		:}|
		/setvar as=object key=outfits index={{getvar::overallOutfit}} {{var::outfit_obj}}|
		
		/buttons labels=["Yes", "No"] Do you want to add more outfits?|
		/setvar key=keepGoing {{pipe}}|
		/ife (keepGoing == '') {:
			/echo Aborting |
			/abort
		:}|
		/elseif (keepGoing == 'Yes') {:
			/setvar key=overallOutfit {{noop}}|
		:}|
	:}|
:}|
/else {:
	/setvar key=outfitHeadDescription None|
	/addvar key=dataBaseNames outfitHeadDescription|
	/setvar key=parsedAccessories None|
	/addvar key=dataBaseNames parsedAccessories|
	/setvar key=parsedMakeup None|
	/addvar key=dataBaseNames parsedMakeup|
	/setvar key=outfitNeckDescription None|
	/addvar key=dataBaseNames outfitNeckDescription|
	/setvar key=outfitMainwearDescription Skip|
	/addvar key=dataBaseNames outfitTopDescription|
	/setvar key=outfitTopDescription Skip|
	/addvar key=dataBaseNames outfitTopDescription|
	/setvar key=outfitBottomDescription Skip|
	/addvar key=dataBaseNames outfitBottomDescription|
	/setvar key=outfitLegwearDescription None|
	/addvar key=dataBaseNames outfitLegwearDescription|
	/setvar key=outfitShoesDescription None|
	/addvar key=dataBaseNames outfitShoesDescription|
	/setvar key=outfitUnderwearTopDescription None|
	/addvar key=dataBaseNames outfitUnderwearTopDescription|
	/setvar key=outfitUnderwearBottomDescription None|
	/addvar key=dataBaseNames outfitUnderwearBottomDescription|
:}|

/setvar key=parsedOutfit {{noop}}|
/ife ((outfitMainwearDescription != 'Skip') and (outfitMainwearDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Mainwear: {{getvar::outfitMainwearDescription}}"|
:}|
/ife ((outfitTopDescription != 'Skip') and (outfitTopDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Top: {{getvar::outfitTopDescription}}"|
:}|
/ife ((outfitBottomDescription != 'Skip') and (outfitBottomDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Bottom: {{getvar::outfitBottomDescription}}"|
:}|
/ife ((outfitLegwearDescription != '') and (outfitLegwearDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Legs: {{getvar::outfitLegwearDescription}}"|
:}|
/ife ((outfitShoesDescription != '') and (outfitShoesDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Shoes: {{getvar::outfitShoesDescription}}"|
:}|
/ife ((outfitUnderwearTopDescription != 'Skip') and (outfitUnderwearTopDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Underwear (Top): {{getvar::outfitUnderwearTopDescription}}"|
:}|
/ife ((outfitUnderwearBottomDescription != '') and (outfitUnderwearBottomDescription != 'None')) {:
	/addvar key=parsedOutfit "{{newline}}- Underwear (Bottom): {{getvar::outfitUnderwearBottomDescription}}"|
:}|
/ife (parsedOutfit != '') {:
	/setvar key=parsedOutfit "###OUTFIT{{newline}}{{getvar::parsedOutfit}}"|
	/setvar key=parsedOutfitTan {{getvar::parsedOutfit}}|
:}|

/ife (((characterArchetype == 'Human') or (characterArchetype == 'Beastkin')) and (genTan == '')) {:
	/buttons labels=["Yes", "No"] Do yo want {{getvar::firstName}} to have some sort of tan?|
	/setvar key=genTan {{pipe}}|
	/ife (genTan == '') {:
        /echo Aborting |
        /abort
    :}|
:}|
/ife ( ( (characterArchetype == 'Human') or (characterArchetype == 'Beastkin') ) and (genTan == 'Yes') ) {:
	/let key=outTan {{noop}}|
	/ife (parsedOutfit == '') {:
		/var key=outTan ["Full Body Tan", "No Tan", "New Outfit"]|
	:}|
	/else {:
		/var key=outTan ["Full Body Tan", "No Tan", "New Outfit", "Current Outfit"]|
	:}|
	/buttons labels={{var::outTan}} Do you want to have a full body tan, no tan, current outfit or select a new outfit that to mold the tan after?|
	/var selected_btn {{pipe}}|
	/ife ( selected_btn == '') {:
		/echo Aborting |
		/abort
	:}|
	/elseif ( selected_btn == 'Full Body Tan') {:
		/setvar key=parsedOutfitTan "Full-body exposure"|
	:}|
	/elseif ( selected_btn == 'No Tan') {:
		/setvar key=parsedOutfitTan "No visible sun exposure"|
	:}|
	/elseif ( selected_btn == 'New Outfit') {:
		/input default="One-Piece Swimsuit" What is the name of the outfit that caused the tan?|
		/setvar key=parsedOutfitTan "Visible tanlines from a {{pipe}}"
	:}|
	/elseif ( selected_btn == 'Current Outfit') {:
		/input default="One-Piece Swimsuit" What is the name of the outfit that caused the tan?|
		/setvar key=parsedOutfitTan "Visible tanlines from a {{getvar::overallOutfit}}"
	:}|
:}|

//Tanlines|
/ife (((characterArchetype == 'Human') or (characterArchetype == 'Beastkin')) and (genTan != 'No')) {:
	/var key=do No|
	/var key=variableName "tanlines"|
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
		/setvar key=genSettings index=wi_book_key "Tanlines"|
		/setvar key=genSettings index=genIsList No|
		/setvar key=genSettings index=inputIsList No|
		/setvar key=genSettings index=genIsSentence Yes|
		/setvar key=genSettings index=needOutput Yes|
		/setvar key=genSettings index=outputIsList No|
		/setvar key=genSettings index=useContext No|
		/setvar key=extra []|
		/:"CMC Logic.Get Basic Type Context"|
		/ife (extra != '') {:
			/setvar key=genSettings index=contextKey {{getvar::extra}}|
		:}|
		/flushvar extra|
		/wait {{getvar::wait}}|
	
		/setvar key=genSettings index=buttonPrompt Is this desctiption of {{getvar::firstName}}'s tanlines good?|
	
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
	
:}|
/else {:
	/setvar key=tanlines None|
	/addvar key=dataBaseNames tanlines|
:}|
//---------|

/:"CMC Logic.JEDParse"|

/:"CMC Logic.Save DataBase"|

/setvar key=stepDone Yes|
/qr-list CMC Main|
/getat index=1 {{pipe}}|
/var qrlabel {{pipe}}|
/qr-get set="CMC Main" label={{var::qrlabel}}|
/getat index="message" {{pipe}}|
/qr-update set="CMC Main" label={{var::qrlabel}} newlabel="Start Generating Mental Traits & Personality" {{pipe}}|
/forcesave|