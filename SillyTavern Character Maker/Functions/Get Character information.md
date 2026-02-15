/ife (gen != 'Yes') {:
	/db-list source=chat field=name |
	/let key=databaseList {{pipe}}|
	
	/foreach {{var::databaseList}} {:
		/ife ('.json' not in item) {:
			/db-get source=chat {{var::item}}| 
			/setvar key={{var::item}} {{pipe}}|
		:}|
	:}|
:}|
/ife (gen != 'Yes') {:
	/db-list source=character field=name |
	/let key=databaseList {{pipe}}|
	/foreach {{var::databaseList}} {:
		/ife (('.json' not in item) and ( item == 'model')) {:
			/db-get source=character {{var::item}}| 
			/setglobalvar key={{var::item}} {{pipe}}|
		:}|
	:}|
:}|
/ife (( lastName == '' ) or ( lastName == 'None' )) {:
	/setvar key=lastName {{noop}}|
	/setvar key=parsedName {{getvar::firstName}}|
:}|
/elseif ((firstName != '') and (lastName != '') ) {:
	/setvar key=parsedName {{getvar::firstName}} {{getvar::lastName}}|
:}|
/ife ( alias == '' ) {:
	/setvar key=alias {{noop}}|
:}|

//Parse character Age|
/setvar key=parcedAge {{noop}}|

/ife ((humanEquivalentAge == age) and (age != '')) {:
	/setvar key=parcedAge {{getvar::age}} years-old|
:}|
/elseif ((humanEquivalentAge != '') and (humanEquivalentAge != 'None') and (humanEquivalentAge != age)) {:
	/setvar key=parcedAge {{getvar::age}} years-old — roughly {{getvar::humanEquivalentAge}} years-old in human years.|
:}|
/elseif (age != '') {:
	/setvar key=parcedAge {{getvar::age}} years-old|
:}|
//-----------|

/ife (parsedAnimalType == 'None') {:
	/setvar key=parsedAnimalType {{noop}}|
:}|


//Parse character Species|
/setvar key=parsedSpecies {{noop}}|
/ife ((characterType == 'None') or (characterType == '') or ( characterType ==  characterArchetype)) {:
	/setvar key=characterType {{noop}}|
:}|
/ife (characterArchetype == species) {:
    /ife ((breed != 'None') and (breed != '')) {:
        /setvar key=parsedSpecies "{{getvar::species}} – {{getvar::breed}}"|
    :}|
    /else {:
        /setvar key=parsedSpecies "{{getvar::species}}"|
    :}|
:}|
/else {:
    /ife ((breed != 'None') and (breed != '')) {:
        /setvar key=parsedSpecies "{{getvar::characterArchetype}} {{getvar::species}} – {{getvar::breed}}"|
    :}|
    /else {:
        /setvar key=parsedSpecies "{{getvar::characterArchetype}} {{getvar::species}}"|
    :}|
:}|
//-----------|

//Parse Real Character info|
/ife ( real == 'Yes') {:
	/setvar key=realInfoParced "{{newline}}- Origin: {{getvar::media_type}} – {{getvar::mediaName}}"|
	/setvar key=realParcedContext "{{getvar::firstName}} is a character from the {{getvar::media_type}} _{{getvar::mediaName}}_. Use your knowledge about {{getvar::firstName}} and the {{getvar::media_type}} _{{getvar::mediaName}}_ when doing the assigned **TASK**"|
:}|
//-----------|

//Parse Nationality and Ethnicity|
/ife ( nationality == 'None' ) {:
	/setvar key=nationality {{noop}}|
:}|
/ife ( ethnicity == 'None' ) {:
	/setvar key=ethnicity {{noop}}|
:}|

/ife (gender == 'Female') {:
	/setvar key=subjPronoun she|
	/setvar key=objPronoun her|
	/setvar key=possAdjPronoun her|
	/setvar key=possPronoun hers|
	/setvar key=reflexivePronoun herself|
	/setvar key=isAre is|
	/setvar key=hasHave has|
	/setvar key=verbSees sees|
	/setvar key=verbFeels feels|
	/setvar key=verbPrefers prefers|
	/setvar key=verbTakes takes|
:}|
/elseif (gender == 'Male') {:
	/setvar key=subjPronoun he|
	/setvar key=objPronoun him|
	/setvar key=possAdjPronoun his|
	/setvar key=possPronoun his|
	/setvar key=reflexivePronoun himself|
	/setvar key=isAre is|
	/setvar key=hasHave has|
	/setvar key=verbSees sees|
	/setvar key=verbFeels feels|
	/setvar key=verbPrefers prefers|
	/setvar key=verbTakes takes|
:}|
/else {:
	/setvar key=subjPronoun they|
	/setvar key=objPronoun them|
	/setvar key=possAdjPronoun their|
	/setvar key=possPronoun theirs|
	/setvar key=reflexivePronoun themself|
	/setvar key=isAre are|
	/setvar key=hasHave have|
	/setvar key=verbSees see|
	/setvar key=verbFeels feel|
	/setvar key=verbPrefers prefer|
	/setvar key=verbTakes take|
:}|