/setvar key=createdLorebook {"entries":{|

/setvar key=lorebookEntryTemplate \"--uid--":{"uid":--uid--,"key":[],"keysecondary":[],"comment":"--comment--","content":"--content--","constant":false,"vectorized":false,"selective":true,"selectiveLogic":--sLogic--,"addMemo":false,"order":100,"position":--pos--,"disable":false,"ignoreBudget":false,"excludeRecursion":false,"preventRecursion":false,"matchPersonaDescription":false,"matchCharacterDescription":true,"matchCharacterPersonality":false,"matchCharacterDepthPrompt":false,"matchScenario":false,"matchCreatorNotes":false,"delayUntilRecursion":false,"probability":100,"useProbability":true,"depth":--depth--,"outletName":"--outletName--","group":"","groupOverride":false,"groupWeight":100,"scanDepth":null,"caseSensitive":null,"matchWholeWords":null,"useGroupScoring":null,"automationId":"","role":--role--,"sticky":0,"cooldown":0,"delay":0,"triggers":[],"displayIndex":0,"characterFilter":{"isExclude":false,"names":[],"tags":[]}}|

/getvar key=loreBookEntries index=comment|
/let key=loreComments {{pipe}}|
/getvar key=loreBookEntries index=content|
/setvar key=loreContents {{pipe}}|
/getvar key=loreBookEntries index=selectiveLogic|
/setvar key=loreLogic {{pipe}}|
/getvar key=loreBookEntries index=primaryKeywords|
/setvar key=lorePrimary {{pipe}}|
/getvar key=loreBookEntries index=filterKeywords|
/setvar key=loreFilter {{pipe}}|

/foreach {{var::loreComments}} {:
	/ife (index > 0) {:
		/addvar key=createdLorebook ","|
	:}|
	/getvar key=loreContents index={{var::index}}|
	/let key=content {{pipe}}|
	/getvar key=loreLogic index={{var::index}}|
	/let key=logic {{pipe}}|
	/getvar key=lorePrimary index={{var::index}}|
	/let key=primary {{pipe}}|
	/getvar key=loreFilter index={{var::index}}|
	/let key=filter {{pipe}}|
	//|
	/re-replace find="/--uid--/g" replace={{var::index}} {{getvar::lorebookTemplate}}|
	/re-replace find="/--comment--/g" replace={{var::item}} {{pipe}}|
	/re-replace find="/--content--/g" replace={{var::content}} {{pipe}}|
	/re-replace find="/--selectiveLogic--/g" replace={{var::logic}} {{pipe}}|
	/re-replace find="/--PrimaryKeywords--/g" replace={{var::primary}} {{pipe}}|
	/re-replace find="/--FilterKeywords--/g" replace={{var::filter}} {{pipe}}|
	/addvar key=createdLorebook {{pipe}}|
:}|
/addvar key=createdLorebook "}"|
/flushvar loreContents|
/flushvar loreLogic|
/flushvar lorePrimary|
/flushvar loreFilter|

/*
//selectiveLogic 0:AND ANY, 1:NOT ALL, 2:NOT ANY, 3:AND ALL|
//position: 0:↑Char, 1:↓Char, 2:↑AN, 3:↓AN, 4:@D, 5:↑EM, 6:↓EM, 7:Outlet|
//role: only for position 4 all else 'null': 0: System, 1: User, 2: Char|
//↑: Before, ↓: After|
//Char: Character Definitions, EM: Example Messages, AN: Autor's Note, @D role: At Depth|
/setvar key=loreBookEntries index=comment ["Test1"]|
/setvar key=loreBookEntries index=content ["Something"]|
/setvar key=loreBookEntries index=selectiveLogic ["0"]|
/setvar key=loreBookEntries index=primaryKeywords ["Test1, Test2"]|
/setvar key=loreBookEntries index=filterKeywords ["{{noop}}"]|

/getvar key=loreBookEntries index=comment|
/setvar key=tempLore {{pipe}}|
/addvar key=tempLore Test2|
/setvar key=loreBookEntries index=comment {{getvar::tempLore}}|

/getvar key=loreBookEntries index=content|
/setvar key=tempLore {{pipe}}|
/addvar key=tempLore Something Else|
/setvar key=loreBookEntries index=content {{getvar::tempLore}}|

/getvar key=loreBookEntries index=selectiveLogic|
/setvar key=tempLore {{pipe}}|
/addvar key=tempLore 1|
/setvar key=loreBookEntries index=selectiveLogic {{getvar::tempLore}}|

/getvar key=loreBookEntries index=PrimaryKeywords|
/setvar key=tempLore {{pipe}}|
/addvar key=tempLore Test3, Test4|
/setvar key=loreBookEntries index=PrimaryKeywords {{getvar::tempLore}}|

/getvar key=loreBookEntries index=FilterKeywords|
/setvar key=tempLore {{pipe}}|
/addvar key=tempLore {{noop}}|
/setvar key=loreBookEntries index=FilterKeywords {{getvar::tempLore}}|

/flushvar tempLore|
*|