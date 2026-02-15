/setvar key=createdLorebook {"entries":{|

/setvar key=lorebookEntryTemplate \"--uid--":{"uid":--uid--,"key":--primKey--,"keysecondary":--secKey--,"comment":"--comment--","content":"--content--","constant":false,"vectorized":false,"selective":true,"selectiveLogic":--sLogic--,"addMemo":false,"order":--order--,"position":--pos--,"disable":false,"ignoreBudget":false,"excludeRecursion":false,"preventRecursion":false,"matchPersonaDescription":false,"matchCharacterDescription":false,"matchCharacterPersonality":false,"matchCharacterDepthPrompt":false,"matchScenario":false,"matchCreatorNotes":false,"delayUntilRecursion":false,"probability":100,"useProbability":true,"depth":--depth--,"outletName":"--outletName--","group":"","groupOverride":false,"groupWeight":100,"scanDepth":--scanDepth--,"caseSensitive":null,"matchWholeWords":null,"useGroupScoring":null,"automationId":"","role":--role--,"sticky":--sticky--,"cooldown":0,"delay":0,"triggers":[],"displayIndex":--uid--,"characterFilter":{"isExclude":false,"names":[],"tags":[]}}|



/let key=lorebookIndex 0|


/getvar key=appearance_obj index=unitType|
/let key=temp1 {{pipe}}|

/getvar key=appearance_obj index=features|
/let key=temp2 {{pipe}}|
/ife (temp2 != '') {:
	/getat index=name {{var::item}}|
	/let key=temp2.name {{pipe}}|
	
	/getat index=type {{var::item}}|
	/let key=temp2.type {{pipe}}|
	
	/getat index=placement {{var::item}}|
	/let key=temp2.placement {{pipe}}|
	
	/getat index=description {{var::item}}|
	/let key=temp2.description {{pipe}}|
	
	
:}|
/getvar key=appearance_obj index=size|
/let key=temp3 {{pipe}}|
/ife (temp3 != '') {:
	/getat index=length {{var::temp3}}|
	/let key=temp3.length {{pipe}}|
	/ife (temp3.length != ''){:
	
	:}|
	
	/getat index=height {{var::temp3}}|
	/let key=temp3.height {{pipe}}|
	/ife (temp3.height != ''){:
	
	:}|
	
:}|

/getvar key=appearance_obj index=covering|
/let key=temp4 {{pipe}}|
/ife (temp4 != '') {:
	/getat index=coveringType {{var::temp4}}|
	/let key=temp4.coveringType {{pipe}}|
	
	/getat index=coveringFront {{var::temp4}}|
	/let key=temp4.coveringFront {{pipe}}|
	
	/getat index=coveringSidesBack {{var::temp4}}|
	/let key=temp4.coveringSidesBack {{pipe}}|
	
	/getat index=coveringArmsLegs {{var::temp4}}|
	/let key=temp4.coveringArmsLegs {{pipe}}|
:}|

/getvar key=appearance_obj index=face|
/let key=temp5 {{pipe}}|

/getvar key=appearance_obj index=hair|
/let key=temp6 {{pipe}}|

/getat index=length {{var::temp6}}|
/let key=temp6.length {{pipe}}|

/getat index=style {{var::temp6}}|
/let key=temp6.style {{pipe}}|

/getat index=description {{var::temp6}}|
/let key=temp6.description {{pipe}}|

/getvar key=appearance_obj index=eyes|
/let key=temp7 {{pipe}}|

/getvar key=appearance_obj index=butt|
/let key=temp8 {{pipe}}|
/ife (temp8 != '') {:
	/getat index=buttSize {{var::temp8}}|
	/let key=temp8.buttSize {{pipe}}|
	
	/getat index=buttShape {{var::temp8}}|
	/let key=temp8.buttShape {{pipe}}|
	
	/getat index=buttFirmness {{var::temp8}}|
	/let key=temp8.buttFirmness {{pipe}}|
:}|

/getvar key=appearance_obj index=pelvis|
/let key=temp9 {{pipe}}|
/ife (temp9 != '') {:
	/getat index=hipsSize {{var::temp9}}|
	/let key=temp9.hipsSize {{pipe}}|
	
	/getat index=thighsSize {{var::temp9}}|
	/let key=temp9.thighsSize {{pipe}}|
:}|

/getvar key=appearance_obj index=breasts|
/let key=temp10 {{pipe}}|
/ife (temp10 != '') {:
	/getat index=breastSize {{var::temp10}}|
	/let key=temp10.breastSize {{pipe}}|
	
	/getat index=breastShape {{var::temp10}}|
	/let key=temp10.breastShape {{pipe}}|
	
	/getat index=breastFirmness {{var::temp10}}|
	/let key=temp10.breastFirmness {{pipe}}|
	
	/getat index=description {{var::temp10}}|
	/let key=temp10.description {{pipe}}|
	
	/getat index=nipples {{var::temp10}}|
	/let key=temp10.nipples {{pipe}}|
	
	/getat index=nippleType {{var::temp10.nipples}}|
	/let key=temp10.nipples.nippleType {{pipe}}|
	
	/getat index=nippleProtrusion {{var::temp10.nipples}}|
	/let key=temp10.nipples.nippleProtrusion {{pipe}}|
	
	/getat index=areolaSize {{var::temp10.nipples}}|
	/let key=temp10.nipples.areolaSize {{pipe}}|
	
	/getat index=areolaShape {{var::temp10.nipples}}|
	/let key=temp10.nipples.areolaShape {{pipe}}|
	
	/getat index=description {{var::temp10.nipples}}|
	/let key=temp10.nipples.description {{pipe}}|
:}|

/getvar key=appearance_obj index=pussy|
/let key=temp11 {{pipe}}|
/ife (temp11 != '') {:
	/getat index=pussyDepth {{var::temp11}}|
	/let key=temp11.pussyDepth {{pipe}}|
	
	/getat index=pussyStretch {{var::temp11}}|
	/let key=temp11.pussyStretch {{pipe}}|
	
	/getat index=clitVisibility {{var::temp11}}|
	/let key=temp11.clitVisibility {{pipe}}|
	
	/getat index=labiaMajoraFullness {{var::temp11}}|
	/let key=temp11.labiaMajoraFullness {{pipe}}|
	
	/getat index=labiaMajoraOutsideColor {{var::temp11}}|
	/let key=temp11.labiaMajoraOutsideColor {{pipe}}|
	
	/getat index=labiaMajoraInsideColor {{var::temp11}}|
	/let key=temp11.labiaMajoraInsideColor {{pipe}}|
	
	/getat index=labiaMinoraVisibility {{var::temp11}}|
	/let key=temp11.labiaMinoraVisibility {{pipe}}|
	
	/getat index=labiaMinoraOutsideColor {{var::temp11}}|
	/let key=temp11.labiaMinoraOutsideColor {{pipe}}|
	
	/getat index=labiaMinoraInsideColor {{var::temp11}}|
	/let key=temp11.labiaMinoraInsideColor {{pipe}}|
	
	/getat index=externalVulvaState {{var::temp11}}|
	/let key=temp11.externalVulvaState {{pipe}}|
	
	/getat index=pubicHair {{var::temp11}}|
	/let key=temp11.pubicHair {{pipe}}|
	
	/getat index=description {{var::temp11}}|
	/let key=temp11.description {{pipe}}|
:}|

/getvar key=appearance_obj index=cock|
/let key=temp12 {{pipe}}|
/ife (temp12 != '') {:
	/getat index=size {{var::temp12}}|
	/let key=temp12.size {{pipe}}|
	
	/getat index=testicularPosition {{var::temp12}}|
	/let key=temp12.testicularPosition {{pipe}}|
	
	/getat index=pubicHair {{var::temp12}}|
	/let key=temp12.pubicHair {{pipe}}|
	
	/getat index=description {{var::temp12}}|
	/let key=temp12.description {{pipe}}|
:}|

/getvar key=appearance_obj index=combined_description|
/let key=temp13 {{pipe}}|
/ife (temp13 != '') {:
	
:}|

/getvar key=appearance_obj index=body_description|
/let key=temp14 {{pipe}}|

/getvar key=appearance_obj index=anus_description|
/let key=temp14 {{pipe}}|

/getvar key=appearance_obj index=cock|
/let key=temp15 {{pipe}}|
/ife (temp15 != '') {:
	/foreach {{var::temp15}} {:
		/getat index=name {{var::item}}|
		/let key=temp15.name {{pipe}}|
		
		/getat index=details {{var::item}}|
		/let key=temp15.details {{pipe}}|
		
		/getat index=effect {{var::item}}|
		/let key=temp15.effect {{pipe}}|
	:}|
:}|

 /addvar key=lorebookEntryTemplate "}}"|

// Information|
/*
Here is some information about what settings mean for the lorebook
//selectiveLogic 0:AND ANY, 1:NOT ALL, 2:NOT ANY, 3:AND ALL|
//position: 0:↑Char, 1:↓Char, 2:↑AN, 3:↓AN, 4:@D, 5:↑EM, 6:↓EM, 7:Outlet|
//role: only for position 4 all else 'null': 0: System, 1: User, 2: Char|
//↑: Before, ↓: After|
//Char: Character Definitions, EM: Example Messages, AN: Autor's Note, @D role: At Depth|
//scanDepth: null or number
//depth: null or number

primKey and secKey needs to be a array

like this
```
/let key=primKey ["Test1", "Test2"]|
```
or
```
/let key=primKey []|
/push primKey "Test1"|
/var key=primKey {{pipe}}|
/push primKey "Test2"|
/var key=primKey {{pipe}}|
```
or a combination of them

For each lorebook we make we need these.

/setvar key=templateCop {{getvar::lorebookEntryTemplate}}|

/re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--primKey--/g" replace="{{var::primKey}}" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--secKey--/g" replace="{{var::secKey}}" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--content--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--pos--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--outletName--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--order--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--sLogic--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--sticky--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--comment--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--role--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|
/re-replace find="/--depth--/g" replace="" {{getvar::templateCop}}|
/setvar key=templateCop {{pipe}}|

/ife (lorebookIndex != 0) {:
	/addvar key=createdLorebook ","|
:}|
/addvar key=createdLorebook {{getvar::templateCop}}|
/add {{var::lorebookIndex}} 1|
/var key=lorebookIndex {{pipe}}|
*/

// Test Data. Can be ignored|
/*
/let key=json_temp {
	"unitType": "",
	"appearanceFeatures": {
		"features": [
			{
				"name": "",
				"type": "",
				"placement": "",
				"description": ""				
			}
		]
	},
	"size": {
		"length": "",
		"height": ""
	},
	"covering": {
		"coveringType": "",
		"coveringFront": "",
		"coveringSidesBack": "",
		"coveringArmsLegs": ""
	},
	"face": "",
	"hair": {
		"length": "",
		"style": "",
		"description": ""
	},
	"eyes": "",
	"butt": {
		"buttSize": "",
		"buttShape": "",
		"buttFirmness": ""
	},
	"pelvis": {
		"hipsSize": "",
		"thighsSize": ""
	},
	"breasts": {
		"breastSize": "",
		"breastShape": "",
		"breastFirmness": "",
		"description": "",
		"nipples": {
			"nippleType": "",
			"nippleProtrusion": "",
			"areolaSize": "",
			"areolaShape": "",
			"description": ""
		},
	},
	"pussy": {
		pussyDepth: "",
		pussyStretch: "",
		clitVisibility: "",
		labiaMajoraFullness: "",
		labiaMajoraOutsideColor: "",
		labiaMajoraInsideColor: "",
		labiaMinoraVisibility: "",
		labiaMinoraOutsideColor: "",
		labiaMinoraInsideColor: "",
		externalVulvaState: "",
		pubicHair: "",
		description: ""
	},
	"cock": {
		"size": "",
		"testicularPosition": "",
		"pubicHair": "",
		"description": ""
	},
	"combined_description": "",
	"body_description": "",
	"anus_description": "",
	"traits": [
		{
			"name": "",
			"details": "",
			"effect": ""
		}
	]|
	
}|
/json-pretty {{var::json_temp}}|
/setvar key=outfits index=Swimwear {{pipe}}|
*|