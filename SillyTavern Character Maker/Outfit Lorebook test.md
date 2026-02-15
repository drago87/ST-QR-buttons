/setvar key=createdLorebook {"entries":{|

/setvar key=lorebookEntryTemplate \"--uid--":{"uid":--uid--,"key":--primKey--,"keysecondary":--secKey--,"comment":"--comment--","content":"--content--","constant":false,"vectorized":false,"selective":true,"selectiveLogic":--sLogic--,"addMemo":false,"order":--order--,"position":--pos--,"disable":false,"ignoreBudget":false,"excludeRecursion":false,"preventRecursion":false,"matchPersonaDescription":false,"matchCharacterDescription":false,"matchCharacterPersonality":false,"matchCharacterDepthPrompt":false,"matchScenario":false,"matchCreatorNotes":false,"delayUntilRecursion":false,"probability":100,"useProbability":true,"depth":--depth--,"outletName":"--outletName--","group":"","groupOverride":false,"groupWeight":100,"scanDepth":--scanDepth--,"caseSensitive":null,"matchWholeWords":null,"useGroupScoring":null,"automationId":"","role":--role--,"sticky":--sticky--,"cooldown":0,"delay":0,"triggers":[],"displayIndex":--uid--,"characterFilter":{"isExclude":false,"names":[],"tags":[]}}|



/let key=lorebookIndex 0|

/keys {{getvar::outfits}}|
/let key=keys1 {{pipe}}|
/filter {{var::keys1}} {: it=
	/ife (it != 'Nude') {:
		/return true
	:}
:}|
/var key=keys1 {{pipe}}|
/push {{var::keys1}} Nude|
/var key=keys1 {{pipe}}|

/setvar key=forRemoval []
/setvar key=headwearLorebook []|
/addvar key=forRemoval headwearLorebook|
/let key=accessoriesSlots ["Left Ear", "Right Ear", "Nose Bridge", "Left Nostril", "Right Nostril", "Septum", "Lower Lip", "Upper Lip", "Tongue", "Neck", "Left Nipple", "Right Nipple", "Navel", "Clitoris", "Clitoris Hood", "Left Wrist", "Right Wrist", "Left Hand Thumb", "Left Hand Index Finger", "Left Hand Middle Finger", "Left Hand Ring Finger", "Left Hand Pinky Finger", "Right Hand Thumb", "Right Hand Index Finger", "Right Hand Middle Finger", "Right Hand Ring Finger", "Right Hand Pinky Finger", "Left Ankle", "Right Ankle", "Left Foot Big Toe", "Left Foot Second Toe", "Left Foot Middle Toe", "Left Foot Fourth Toe", "Left Foot Little Toe", "Right Foot Big Toe", "Right Foot Second Toe", "Right Foot Middle Toe", "Right Foot Fourth Toe", "Right Foot Little Toe"]|
/foreach {{var::accessoriesSlots}} {:
	/re-replace find="/\s/g" replace="" {{var::item}}|
	/let key=ttemp accessoriesLorebook{{pipe}}|
	/setvar key={{var::ttemp}} []|
	/addvar key=forRemoval {{var::ttemp}}|
:}|

/let key=makeupSlots ["Eyeshadow", "Eyeliner", "Mascara", "Eyebrows", "Lipstick", "Lip Liner", "Blush", "Foundation", "Face Paint", "Left Hand Fingernails", "Right Hand Fingernails", "Left Foot Toenails", "Right Foot Toenails"]|
/foreach {{var::makeupSlots}} {:
	/re-replace find="/\s/g" replace="" {{var::item}}|
	/let key=ttemp makeupLorebook{{pipe}}|
	/setvar key={{var::ttemp}} []|
	/addvar key=forRemoval {{var::ttemp}}|
:}|
/setvar key=neckwearLorebook []|
/addvar key=forRemoval neckwearLorebook|
/setvar key=mainwearfullLorebook []|
/addvar key=forRemoval mainwearfullLorebook|
/setvar key=mainweartopLorebook []|
/addvar key=forRemoval mainweartopLorebook|
/setvar key=mainwearbottomLorebook []|
/addvar key=forRemoval mainwearbottomLorebook|
/setvar key=legwearLorebook []|
/addvar key=forRemoval legwearLorebook|
/setvar key=socksLorebook []|
/addvar key=forRemoval socksLorebook|
/setvar key=underweartopLorebook []|
/addvar key=forRemoval underweartopLorebook|
/setvar key=underwearbottomLorebook []|
/addvar key=forRemoval underwearbottomLorebook|


/foreach {{var::keys1}} {:
	/let key=outfitName {{var::item}}|
	/getvar key=outfits index={{var::item}}|
	/let key=outfit {{pipe}}|
	/getat index=outfit_type {{var::outfit}}|
	/let key=outfitType {{pipe}}|
	/keys {{var::outfit}}|
	/let key=keys2 {{pipe}}|
	/var key=ind {{noop}}|
	/find {{var::keys2}} {: it=
		/ife (it == 'outfit_type') {:
			/var key=ind {{var::index}}|
		:}|
	:}|
	/splice start={{var::ind}} delete=1 {{var::keys2}}|
	/var key=keys2 {{pipe}}|
	/foreach {{var::keys2}} {:
		/getat index={{var::item}} {{var::outfit}}|
		/let key=objectSlot {{pipe}}|
		/getat index=type {{var::objectSlot}}|
		/let key=objType {{pipe}}|
		/ife (objType == 'string') {:
			/getat index=slot {{var::objectSlot}}|
			
			/getat index=name {{var::objectSlot}}|
			/let key=name {{pipe}}|
			/re-replace find="/_/g" replace="" {{var::slot}}|
			/let key=compressedslot {{pipe}}|
			/addvar key={{var::compressedslot}}Lorebook {{var::name}}|
			
			/getat index=description {{var::objectSlot}}|
			
			/ife (outfitName != 'Nude') {:
			    // 1. Format Display Name |
			    /re-replace find="/_/g" replace=" " {{var::slot}}|
			    /let key=displaySlot {{pipe}}|
			    /re-replace find="/\b(\w)/g" cmd="/to-upper $1" replace="$1" {{var::displaySlot}}|
			    /var key=displaySlot {{pipe}}|
				
			    // 2. Comprehensive Primary Keys (Including Equip Verbs) |
			    /let key=primKey ["{{getvar::firstName}} put on", "{{getvar::firstName}} is wearing", "{{getvar::firstName}} changes into", "puts the {{var::name}} on {{getvar::firstName}}", "helps {{getvar::firstName}} into", "dressing {{getvar::firstName}} in", "equips {{getvar::firstName}} with", "{{getvar::firstName}} equipped", "{{getvar::firstName}} equippes", "equipping {{getvar::firstName}}", "equippes on {{getvar::firstName}}"]|
			    
			    // 3. Secondary Keys |
			    /let key=secKey ["{{var::outfitName}}", "{{var::name}}"]|
				
			    // 4. Branch: Special Case for Two-Piece Outfits |
			    /ife ((slot == 'mainwear_top') and (outfitType == 'two')) {:
			        // Create the 'Manager' Entry that injects the split outlets into the 'full' outlet |
			        /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
			        /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--content--/g" replace="\{{outlet::mainwear_top}}{{newline}}\{{outlet::mainwear_bottom}}" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--pos--/g" replace="7" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        // The manager targets the 'mainwear_full' outlet in the character sheet |
			        /re-replace find="/--outletName--/g" replace="mainwear_full" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--order--/g" replace="50" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        // SET STICKY TO 9999 |
			        /re-replace find="/--sticky--/g" replace="9999" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        /re-replace find="/--comment--/g" replace="{{var::outfitName}} Split Manager" {{getvar::templateCop}}|
			        /setvar key=templateCop {{pipe}}|
			        
			        // 8. Remaining Cleanup (Ensuring all placeholders are cleared) |
					/re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|
					/re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|
					
			        // Append Manager Entry |
			        /ife (lorebookIndex != 0) {:
				        /addvar key=createdLorebook ","
			        :}|
			        /addvar key=createdLorebook {{getvar::templateCop}}|
			        /add {{var::lorebookIndex}} 1|
			        /var key=lorebookIndex {{pipe}}|
			    :}|
				
			    // 5. Standard Entry (Runs for all items, including the individual pieces of a two-piece) |
			    /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
			    /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--content--/g" replace="- {{var::displaySlot}}: {{var::name}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--pos--/g" replace="7" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--outletName--/g" replace="{{var::slot}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--order--/g" replace="50" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    // SET STICKY TO 9999 |
			    /re-replace find="/--sticky--/g" replace="9999" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--comment--/g" replace="{{var::outfitName}} {{var::displaySlot}} Outlet" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    
			    
				// 8. Remaining Cleanup (Ensuring all placeholders are cleared) |
				/re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				
			    // Final Append |
			    /ife (lorebookIndex != 0) {:
				    /addvar key=createdLorebook ","
			    :}|
			    /addvar key=createdLorebook {{getvar::templateCop}}|
			    /add {{var::lorebookIndex}} 1|
			    /var key=lorebookIndex {{pipe}}|
			    
			    //Description|
			    // 1. Prepare Sensory Triggers (Primary Key) |
				// Using the Item Name and Formatted Slot Name as triggers |
				/let key=primKey ["{{var::name}}", "{{var::displaySlot}}"]|
				
				// 2. Secondary Key (Character Name + Outfit Name) |
				// Logic 3 (AND ALL) ensures the AI only describes this item for this character/outfit |
				/let key=secKey ["{{getvar::firstName}}", "{{var::outfitName}}"]|
				
				// 3. Start Template Copy and UID |
				/setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
				/re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 4. Replace Primary and Secondary Keys |
				/re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 5. Set Content (The Full Flowery Description) |
				/re-replace find="/--content--/g" replace="{{var::description}}" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 6. Metadata: Position 0 (Before Char Defs) |
				/re-replace find="/--pos--/g" replace="0" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--sLogic--/g" replace="3" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 7. Expiration & Search Depth (Limited for efficiency) |
				// Sticky 10 ensures it stays for a few turns; ScanDepth 10 limits the 'lookback' |
				/re-replace find="/--sticky--/g" replace="10" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--scanDepth--/g" replace="10" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 8. Remaining Cleanup (Ensuring all placeholders are cleared) |
				/re-replace find="/--outletName--/g" replace="" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--comment--/g" replace="{{var::outfitName}} {{var::displaySlot}} Detail" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				
				// 9. Append and Increment |
				/ife (lorebookIndex != 0) {:
					/addvar key=createdLorebook ","
				:}|
				/addvar key=createdLorebook {{getvar::templateCop}}|
				/add {{var::lorebookIndex}} 1|
				/var key=lorebookIndex {{pipe}}|
			:}|
			/else {:
			    // 1. Format the Compressed Slot for Registry lookup |
			    /re-replace find="/_/g" replace="" {{var::slot}}|
			    /let key=compressedslot {{pipe}}|
			    
			    // 2. Format Display Name (e.g. mainwear_bottom -> Mainwear Bottom) |
			    /re-replace find="/_/g" replace=" " {{var::slot}}|
			    /let key=displaySlot {{pipe}}|
			    /re-replace find="/\b(\w)/g" cmd="/to-upper $1" replace="$1" {{var::displaySlot}}|
			    /var key=displaySlot {{pipe}}|
				
			    // 3. Get Registry of worn items for triggers |
			    /getvar key={{var::compressedslot}}Lorebook|
			    /let key=itemNames {{pipe}}|
				
				// --- A. GENERATE NUDE OUTLET ENTRY (POS 7) --- |
				/setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
				
				// 1. Precise State Triggers (Subject + State + Slot) |
				// We use your pronouns to cover all bases |
				/let key=primKey ["{{getvar::firstName}} is naked", "{{getvar::firstName}} is nude", "{{getvar::firstName}} is bare", "{{getvar::subjPronoun}} is naked", "{{getvar::subjPronoun}} is nude", "{{getvar::firstName}} has nothing on {{getvar::possAdjPronoun}} {{var::displaySlot}}", "{{getvar::subjPronoun}} has nothing on {{getvar::possAdjPronoun}} {{var::displaySlot}}", "{{getvar::firstName}}'s {{var::displaySlot}} is bare"]|
				
				// 2. Action Triggers (Subject/Verb/Item) |
				// We loop through the items to create specific removal sentences |
				/foreach {{var::itemNames}} {:
				    /let key=currItem {{var::item}}|
				    /ife (currItem != 'none') {:
				        // Self-removal |
				        /push primKey "{{getvar::firstName}} takes off {{getvar::possAdjPronoun}} {{var::currItem}}"|
				        /var key=primKey {{pipe}}|
				        /push primKey "{{getvar::subjPronoun}} removes {{getvar::possAdjPronoun}} {{var::currItem}}"|
				        /var key=primKey {{pipe}}|
				        // Third-party removal |
				        /push primKey "removes {{getvar::firstName}}'s {{var::currItem}}"|
				        /var key=primKey {{pipe}}|
				        /push primKey "takes the {{var::currItem}} off {{getvar::objPronoun}}"|
				        /var key=primKey {{pipe}}|
				        // Generic cleaning/discarding |
				        /push primKey "discards {{getvar::firstName}}'s {{var::currItem}}"|
				        /var key=primKey {{pipe}}|
				        /push primKey "wipes the {{var::currItem}} off {{getvar::objPronoun}}"|
				        /var key=primKey {{pipe}}|
				    :}|
				:}|
				
				// 3. Secondary Keys (Contextual Safety) |
				// We keep the name here so even with Logic 0, it has a high weight toward Char |
				/let key=secKey ["{{getvar::firstName}}", "{{getvar::displaySlot}}"]|
				
				// 4. Metadata: Position 7, Order 100, Logic 0 (AND ANY) |
				/re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
				/re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
				/setvar key=templateCop {{pipe}}|
			
			    /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--content--/g" replace="- {{var::displaySlot}}: Nothing" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--pos--/g" replace="7" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--outletName--/g" replace="{{var::slot}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--sticky--/g" replace="9999" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    
			    // Cleanup remaining placeholders |
			    /re-replace find="/--scanDepth--/g" replace="null" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--comment--/g" replace="Nude {{var::displaySlot}} Outlet" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			
			    /ife (lorebookIndex != 0) {:
				    /addvar key=createdLorebook ","
			    :}|
			    /addvar key=createdLorebook {{getvar::templateCop}}|
			    /add {{var::lorebookIndex}} 1|
			    /var key=lorebookIndex {{pipe}}|
			
			    // --- B. GENERATE NUDE SENSORY DETAIL (POS 0) --- |
			    /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
			    
			    // Reset keys for Second Entry using /var |
			    
				/var key=primKey ["Nothing", "none", "naked", "bare", "exposed", "look at {{getvar::firstName}}", "looks at {{getvar::firstName}}", "observing {{getvar::firstName}}", "staring at {{getvar::firstName}}"]|
			    /var key=secKey ["{{getvar::firstName}}", "{{var::displaySlot}}"]|

			
			    /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--content--/g" replace="{{var::description}}" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--pos--/g" replace="0" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--sticky--/g" replace="10" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--scanDepth--/g" replace="10" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--sLogic--/g" replace="3" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--outletName--/g" replace="" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			    /re-replace find="/--comment--/g" replace="Nude {{var::displaySlot}} Detail" {{getvar::templateCop}}|
			    /setvar key=templateCop {{pipe}}|
			
			    /ife (lorebookIndex != 0) {:
				    /addvar key=createdLorebook ","
			    :}|
			    /addvar key=createdLorebook {{getvar::templateCop}}|
			    /add {{var::lorebookIndex}} 1|
			    /var key=lorebookIndex {{pipe}}|
			:}|
		:}|
		/elseif (objType == 'array') {:
		    /getat index=items {{var::objectSlot}}|
		    /let key=objItem {{pipe}}|
		    /getat index=slot {{var::objectSlot}}|
		    /let key=objslot {{pipe}}|
		    
		    /foreach {{var::objItem}} {:
		        // 1. Get the specific sub-slot name (e.g., Left Ear) |
		        /getat index={{var::objslot}}_slot {{var::item}}|
		        /let key=eqslot {{pipe}}|
		        
		        // 2. Format names for Registry variable (e.g., accessoryLorebookLeftEar) |
		        /re-replace find="/\s/g" replace="" {{var::eqslot}}|
		        /let key=compressedeqslot {{pipe}}|
		        /let key=targetRegistry {{var::objslot}}Lorebook{{var::compressedeqslot}}|
				
		        // 3. Capture current item data |
		        /getat index=name {{var::item}}|
		        /let key=eqname {{pipe}}|
		        /getat index=description {{var::item}}|
		        /let key=eqdesc {{pipe}}|
		        /getat index=permanent {{var::item}}|
		        /let key=isPermanent {{pipe}}|
		        
				
		        // 4. Format Display Name for the entry (e.g., Left Ear) |
		        /re-replace find="/_/g" replace=" " {{var::eqslot}}|
		        /let key=displaySlot {{pipe}}|
		        /re-replace find="/\b(\w)/g" cmd="/to-upper $1" replace="$1" {{var::displaySlot}}|
		        /var key=displaySlot {{pipe}}|
				
		        /ife (outfitName != 'Nude') {:
		            
					// --- A. REGISTRY LOGGING --- |
				    // 1. Check if the specific sub-slot registry exists |
				    /listvar scope=local return=object|
				    /map {{pipe}} {: /getat index=key {{var::item}} :}|
				    /let key=keyList {{pipe}}|
				    
				    // 2. Initialize if missing (e.g. accessoriesLorebookLeftWing) |
				    /ife ('{{var::targetRegistry}}' not in keyList) {:
				        /setvar key={{var::targetRegistry}} []|
				        /addvar key=forRemoval {{var::targetRegistry}}|
				    :}|
				    
				    // 3. Add the item name (e.g. Ring) to the registry |
				    /ife (isPermanent != 'Yes') {:
					    /addvar key={{var::targetRegistry}} {{var::eqname}}|
					:}|
				    // --- B. NON-NUDE OUTLET ENTRY (POS 7) --- |
		            /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
		            /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
					
		            /ife (objslot == 'accessory') {:
		                /var key=primKey ["{{getvar::firstName}} put on", "{{getvar::firstName}} is wearing", "puts the {{var::eqname}} on {{getvar::firstName}}", "helps {{getvar::firstName}} into", "equips {{getvar::firstName}} with"]|
		            :}|
		            /else {:
		                /var key=primKey ["{{getvar::firstName}} applied", "{{getvar::firstName}} is wearing", "applies {{var::eqname}} to {{getvar::firstName}}", "painting {{getvar::firstName}}'s {{var::displaySlot}}"]|
		            :}|
		            /var key=secKey ["{{var::outfitName}}", "{{var::eqname}}"]|
					
		            /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--content--/g" replace="- {{var::displaySlot}}: {{var::eqname}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--pos--/g" replace="7" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--outletName--/g" replace="{{var::objslot}}s" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--sticky--/g" replace="9999" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--order--/g" replace="50" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--scanDepth--/g" replace="null" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--comment--/g" replace="{{var::outfitName}} {{var::displaySlot}} Outlet" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
					
		            /ife (lorebookIndex != 0) {: /addvar key=createdLorebook "," :}|
		            /addvar key=createdLorebook {{getvar::templateCop}}|
		            /add {{var::lorebookIndex}} 1|
		            /var key=lorebookIndex {{pipe}}|
					
		            // --- B. NON-NUDE SENSORY DETAIL (POS 0) --- |
		            /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
		            /var key=primKey ["{{var::eqname}}", "{{var::displaySlot}}", "look at {{getvar::firstName}}"]|
		            /var key=secKey ["{{getvar::firstName}}", "{{var::outfitName}}"]|
					
		            /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--content--/g" replace="{{var::eqdesc}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--pos--/g" replace="0" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--sticky--/g" replace="10" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--scanDepth--/g" replace="10" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--sLogic--/g" replace="3" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--outletName--/g" replace="" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--comment--/g" replace="{{var::outfitName}} {{var::displaySlot}} Detail" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
					
		            /ife (lorebookIndex != 0) {: /addvar key=createdLorebook "," :}|
		            /addvar key=createdLorebook {{getvar::templateCop}}|
		            /add {{var::lorebookIndex}} 1|
		            /var key=lorebookIndex {{pipe}}|
		        :}|
		        
				/else {:
				    // 1. Format the Compressed Slot for Registry lookup |
				    /re-replace find="/\s/g" replace="" {{var::eqslot}}|
				    /let key=compressedeqslot {{pipe}}|
				    /let key=targetRegistry {{var::objslot}}Lorebook{{var::compressedeqslot}}|
				    
				    // 2. Format Display Name (e.g. Left Ear) |
				    /re-replace find="/_/g" replace=" " {{var::eqslot}}|
				    /let key=displaySlot {{pipe}}|
				    /re-replace find="/\b(\w)/g" cmd="/to-upper $1" replace="$1" {{var::displaySlot}}|
				    /var key=displaySlot {{pipe}}|
					
				    // 3. Get Registry of worn items for triggers |
				    /getvar key={{var::targetRegistry}}|
				    /let key=itemNames {{pipe}}|
					
				    // --- A. GENERATE NUDE OUTLET ENTRY (POS 7) --- |
				    /setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
				    
				    // Craft Triggers using /push |
				    /let key=primKey ["is naked", "is nude", "Nothing", "{{getvar::firstName}} is bare"]|
				    /foreach {{var::itemNames}} {:
				        /let key=currItem {{var::item}}|
				        // skip 'none' (permanent items were already excluded from registry) |
				        /ife (currItem != 'none') {:
				            /push primKey "takes off {{var::currItem}}"|
				            /var key=primKey {{pipe}}|
				            /push primKey "removes {{var::currItem}}"|
				            /var key=primKey {{pipe}}|
				            /push primKey "discards {{var::currItem}}"|
				            /var key=primKey {{pipe}}|
				            
				            // Slot-specific removal verbs |
				            /ife (objslot == 'accessory') {:
				                /push primKey "unfastens {{var::currItem}}"|
				                /var key=primKey {{pipe}}|
				                /push primKey "unclips {{var::currItem}}"|
				                /var key=primKey {{pipe}}|
				            :}|
				            /else {:
				                /push primKey "scrubs off {{var::currItem}}"|
				                /var key=primKey {{pipe}}|
				                /push primKey "wipes off {{var::currItem}}"|
				                /var key=primKey {{pipe}}|
				                /push primKey "washes off {{var::currItem}}"|
				                /var key=primKey {{pipe}}|
				            :}|
				        :}|
				    :}|
				    /let key=secKey ["{{getvar::firstName}}", "{{var::displaySlot}}"]|
					
				    /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    
				    // Content logic: Permanent items stay visible, others become 'Nothing' |
				    /ife (isPermanent == 'Yes') {:
				        /re-replace find="/--content--/g" replace="- {{var::displaySlot}}: {{var::eqname}}" {{getvar::templateCop}}|
				    :}|
				    /else {:
				        /re-replace find="/--content--/g" replace="- {{var::displaySlot}}: Nothing" {{getvar::templateCop}}|
				    :}|
				    /setvar key=templateCop {{pipe}}|
				    
				    // Metadata: Pos 7, Order 100, Sticky 9999, Logic 0 |
				    /re-replace find="/--pos--/g" replace="7" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--outletName--/g" replace="{{var::objslot}}s" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--sticky--/g" replace="9999" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--comment--/g" replace="Nude {{var::displaySlot}} Outlet" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    
				    // Cleanup metadata |
				    /re-replace find="/--scanDepth--/g" replace="null" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
					
				    /ife (lorebookIndex != 0) {:
					    /addvar key=createdLorebook ","
					:}|
				    /addvar key=createdLorebook {{getvar::templateCop}}|
				    /add {{var::lorebookIndex}} 1|
				    /var key=lorebookIndex {{pipe}}|
		            
		            
					// --- B. GENERATE NUDE SENSORY DETAIL (POS 0) --- |
					/setvar key=templateCop {{getvar::lorebookEntryTemplate}}|
					
					// 1. Specific "Sentence" Triggers (Primary Key) |
					// These are unique enough that Logic 0 won't trigger them by accident |
					/var key=primKey ["{{getvar::firstName}} is naked", "{{getvar::firstName}} is nude", "{{getvar::subjPronoun}} is bare", "{{getvar::firstName}} has nothing on {{getvar::possAdjPronoun}} {{var::displaySlot}}", "{{getvar::subjPronoun}} has nothing on {{getvar::possAdjPronoun}} {{var::displaySlot}}", "{{getvar::firstName}}'s {{var::displaySlot}} is bare", "look at {{getvar::firstName}}", "looks at {{getvar::firstName}}", "observing {{getvar::firstName}}"]|
					
					// 2. Secondary Key (The 'Char' Anchor) |
					// In Logic 0, this just ensures the character's name is weighted heavily |
					/var key=secKey ["{{getvar::firstName}}", "{{var::displaySlot}}"]|
					
					// 3. Selective Logic 0 (AND ANY) |
					// This allows ANY of those specific 'Naked' sentences to trigger the description |
					/re-replace find="/--sLogic--/g" replace="0" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|
					
					// 4. Metadata and Closure |
					/re-replace find="/--pos--/g" replace="0" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|
					/re-replace find="/--order--/g" replace="100" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|
					/re-replace find="/--comment--/g" replace="Nude {{var::displaySlot}} Detail" {{getvar::templateCop}}|
					/setvar key=templateCop {{pipe}}|

		            /re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--content--/g" replace="{{var::eqdesc}}" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--sticky--/g" replace="10" {{getvar::templateCop}}|
		            /setvar key=templateCop {{pipe}}|
		            /re-replace find="/--depth--/g" replace="null" {{getvar::templateCop}}|
				    /setvar key=templateCop {{pipe}}|
				    /re-replace find="/--role--/g" replace="null" {{getvar::templateCop}}|
					
		            /ife (lorebookIndex != 0) {: /addvar key=createdLorebook "," :}|
		            /addvar key=createdLorebook {{getvar::templateCop}}|
		            /add {{var::lorebookIndex}} 1|
		            /var key=lorebookIndex {{pipe}}|
		        :}|
		    :}|
		:}|
	:}|
:}|

 /addvar key=lorebookEntryTemplate "}}"|

/foreach {{getvar::forRemoval}} {:
	/flushvar {{var::item}}|
:}|
/flushvar forRemoval|

// Test Data. Can be ignored|
/*
/let key=json_temp {
	"outfit_type": "two",
	"mainwear_top": {
		"type": "string",
		"slot": "mainwear_top",
		"name": "Bikini Top",
		"description": "{{getvar::firstName}} is wearing a blue Bikini on {{getvar::possAdjPronoun}} top half."
	},
	"mainwear_bottom": {
		"type": "string",
		"slot": "mainwear_bottom",
		"name": "Bikini Top",
		"description": "{{getvar::firstName}} is wearing a blue G-String Bikini on {{getvar::possAdjPronoun}} bottom half."
	}
}|
/json-pretty {{var::json_temp}}|
/setvar key=outfits index=Swimwear {{pipe}}|


{
	"Winter Clothes": {
		"headwear": {
			"type": "string",
			"slot": "headwear",
			"name": "Woolen Hat",
			"description": "{{getvar::firstName}} is wearing a red Woolen Hat with green stripes on {{getvar::possAdjPronoun}} head."
		}
	},
	"Swimwear": {
		"headwear": {
			"type": "string",
			"slot": "headwear",
			"name": "Swimming Cap",
			"description": "{{getvar::firstName}} is wearing a blue Swimming Cap on {{getvar::possAdjPronoun}} head."
		}
	},
}|
//selectiveLogic 0:AND ANY, 1:NOT ALL, 2:NOT ANY, 3:AND ALL|
//position: 0:↑Char, 1:↓Char, 2:↑AN, 3:↓AN, 4:@D, 5:↑EM, 6:↓EM, 7:Outlet|
//role: only for position 4 all else 'null': 0: System, 1: User, 2: Char|
//↑: Before, ↓: After|
//Char: Character Definitions, EM: Example Messages, AN: Autor's Note, @D role: At Depth|
//scanDepth: null or number
*|