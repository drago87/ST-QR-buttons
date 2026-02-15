/buttons labels=["Yes", "No"] <div>Is the character you are making a "real" character from a game/anime/book etc..?</div><div>The Result depends on how much information the model you are using have about the character.</div> |
/setvar key=real {{pipe}}|
/ife ( real == ''){:
	/echo Aborting|
	/abort|
:}|
/elseif ( real == 'Yes') {:
	/input rows=8 What is the first name of the character?|
	/setvar key=firstName {{pipe}}|
	/ife ( firstName == ''){:
		/echo Aborting|
		/abort|
	:}|
	/else {::}|

	/input rows=8 What is the last name of {{getvar::firstName}}?|
	/setvar key=lastName {{pipe}}|
	/ife ( lastName == ''){:
		/buttons labels=["Yes", "No"] Are you sure you want to {{getvar::firstName}} to have no last name?|
		/let key=temp {{pipe}}|
		/ife ((temp == '') or (temp == 'No')) {:
			/echo Aborting|
			/abort|
		:}|
		/else {:
			/setvar key=lastName None|
		:}|
	:}|
	/else {::}|

	/input rows=8 <div>What is the type of media that {{getvar::firstName}} is in?</div><div>Anime,Manga, Movie etc..</div>|
	/setvar key=media_type {{pipe}}|
	/ife ( media_type == ''){:
		/echo Aborting|
		/abort|
	:}|
	/else {::}|

	/input rows=8 What is the name of the {{getvar::media_type}} that {{getvar::firstName}} is in?|
	/setvar key=mediaName {{pipe}}|
	/ife ( mediaName == ''){:
		/echo Aborting|
		/abort|
	:}|
	/else {::}|
	/setvar key=tempMedia {{noop}}|
	/ife (lastName != 'None') {:
		/setvar key=parsedMedia "{{getvar::firstName}} {{getvar::lastName}} is a character from the {{getvar::media_type}} _{{getvar::mediaName}}_."|
		/setvar key=tempMedia "{{getvar::firstName}} {{getvar::lastName}} from the {{getvar::media_type}} _{{getvar::mediaName}}_"|
	:}|
	/else {:
		/setvar key=parsedMedia "{{getvar::firstName}} is a character from the {{getvar::media_type}} _{{getvar::mediaName}}_."|
		/setvar key=tempMedia "{{getvar::firstName}} {{getvar::lastName}} from the {{getvar::media_type}} _{{getvar::mediaName}}_"|
	:}|
	
	/buttons labels=["Yes", "No", "Check"] <div>Do you want ot check what the LLM model you are using know about {{getvar::tempMedia}}.</div> |
	/setvar key=realT {{pipe}}|
	/ife ( realT == ''){:
		/echo Aborting|
		/abort|
	:}|
	/elseif ( realT == 'Check') {:
		/let key=tempPrompt {{noop}}|
		/whilee (tempPrompt != 'Done') {:
			/buttons ["Done", "Check Apperance", "Check Personality"] "Select what you want to know. Then press 'Done' when you know are done."|
			/ife ( realT == ''){:
				/echo Aborting|
				/abort|
			:}|
			/elseif (realT == 'Check Apperance') {:
				/genraw "What do you know about the Apperance of {{getvar::tempMedia}}. It is better to say that you have no information then to lie about {{getvar::firstName}}'s apperance."|
				/popup {{pipe}}|
			:}|
			/elseif (realT == 'Check Personality') {:
				/genraw "What do you know about the Personality of {{getvar::tempMedia}}. It is better to say that you have no information then to lie about {{getvar::firstName}}'s personality."|
				/popup {{pipe}}|
			:}|
			/elseif (realT == 'Done') {:
				/buttons ["Yes", "No"] "<div>After checking if your Model knows about the character do you still want to make {{getvar::tempMedia}}?</div><div>Even if your Model don't know about the character you can still make it but you will probebly need to manually make some of the parts.</div>"|
				/ife ( real == ''){:
					/echo Aborting|
					/abort|
				:}|
			:}|
		:}|
	:}|
		
	/ife ( real == 'Yes') {:
		/addvar key=dataBaseNames real|
		/addvar key=dataBaseNames firstName|
		/addvar key=dataBaseNames lastName|
		/addvar key=dataBaseNames media_type|
		/addvar key=dataBaseNames media_name|
		/addvar key=dataBaseNames parsedMedia|
	:}|
	/else {:
		/addvar key=dataBaseNames real|
		/setvar key=media_type None|
		/addvar key=dataBaseNames media_type|
		/setvar key=media_name None|
		/addvar key=dataBaseNames media_name|
		/setvar key=parsedMedia None|
		/addvar key=dataBaseNames parsedMedia|
	:}|
	
	
:}|
/else {:
	/addvar key=dataBaseNames real|
	/setvar key=media_type None|
	/addvar key=dataBaseNames media_type|
	/setvar key=media_name None|
	/addvar key=dataBaseNames media_name|
	/setvar key=parsedMedia None|
	/addvar key=dataBaseNames parsedMedia|
:}|
