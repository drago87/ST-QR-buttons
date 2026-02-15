/ife (charName != 'No') {:
	/re-replace find="/<User_Input>\n/" replace="" {{lastMessage}}|
	/let key=clean {{pipe}}|
	/re-replace find="/\n<\/User_Input>/" replace="" {{var::clean}}|
	/var key=clean {{pipe}}|
	/reasoning-get {{var::i}}|
	/let key=think {{pipe}}|
	/ife (think != '') {:
		/reasoning-set {{var::think}}|
	:}|
	/re-replace find="/[\s\S]*?<\/think>\n/g" replace="" {{var::clean}}|
	/let key=mess {{pipe}}|
	/ife (mess != '') {:
		/message-edit await=true <User_Input>{{newline}}{{var::mess}}{{newline}}</User_Input>
	:}|
:}|

/ife (charName != 'No') {:
	/messages names=on {{lastMessageId}}|
	/let key=orgMessage {{pipe}}|
	/re-replace find="/:[\s\S]*/" replace="" {{var::orgMessage}} |
	/setvar key=charName {{pipe}}|
	/re-replace find="/<{{getvar::charName}}>\n/" replace="" {{lastMessage}}|
	/let key=clean {{pipe}}|
	/re-replace find="/\n<\/{{getvar::charName}}>/" replace="" {{var::clean}}|
	/var key=clean {{pipe}}|
	/reasoning-get {{var::i}}|
	/let key=think {{pipe}}|
	/ife (think != '') {:
		/reasoning-set {{var::think}}|
	:}|
	/re-replace find="/^[\s\S]*?<\/think>\s*/" replace="" {{var::clean}}|
	/let key=mess {{pipe}}|
	/ife (mess != '') {:
		/message-edit await=true <{{getvar::charName}}>{{newline}}{{var::mess}}{{newline}}</{{getvar::charName}}>
	:}|
:}|

/let key=exultationList ["Character Maker QR"]|
/split {{group}}|
/len {{pipe}}|
/let key=len {{pipe}}|
/let key=group {{noop}}|

/ife (len > 1) {:
	/buttons labels=["Yes", "No"] Is this a group chat?|
	/var key=group {{pipe}}|
	/ife (group == '') {:
		/echo Aborting |
		/abort
	:}|
:}|
/ife (('{{char}}' not in exultationList) and (group == 'No')) {:
	/genraw Summarize all individual character names mentioned in the text below.
	
	<Instructions>
	1. List the names as a simple comma-separated list.
	2. If only one character is described, return only their name.
	3. If no names are present, return: No
	4. Do not include titles (e.g., "The Lamia Cub") unless they are part of how the character is addressed.
	</Instructions>
	
	<Content_To_Analyze>
	{{char}}
	</Content_To_Analyze>
	
	Respond with <think> reasoning first, followed by the list.|
	/re-replace find="/s<think>[\s\S]*?<\/think>/g" replace="" {{pipe}}|
	/let key=output {{pipe}}|
	/ife (output == 'No') {:
		/split find="," {{var::output}}|
		/let key=nameList {{pipe}}|
		/push {{var::nameList}} {{var::output}}|
		/var key=nameList {{pipe}}|
		/unshift {{var::nameList}} {{char}}|
		/var key=nameList {{pipe}}|
		/buttons labels={{var::nameList}} select the name of the character.|
		/setvar key=charName {{pipe}}|
		/ife (charName == '') {:
			/abort quiet=false Missing character name setting in input.|
		:}|
	:}|
	/elseif  ((charName == '') or (output == 'No')) {:
		/genraw Summarize all individual character names mentioned in the text below.
	
	<Instructions>
	1. List the names as a simple comma-separated list.
	2. If only one character is described, return only their name.
	3. If no names are present, return: No
	4. Do not include titles (e.g., "The Lamia Cub") unless they are part of how the character is addressed.
	</Instructions>
	
	<Content_To_Analyze>
	{{charDescription}}
	</Content_To_Analyze>
	
	Respond with <think> reasoning first, followed by the list.|
		/re-replace find="/s<think>[\s\S]*?<\/think>/g" replace="" {{pipe}}|
		/var key=output {{pipe}}|
		/split find="," {{var::output}}|
		/let key=nameList {{pipe}}|
		/push {{var::nameList}} {{var::output}}|
		/var key=nameList {{pipe}}|
		/unshift {{var::nameList}} {{char}}|
		/var key=nameList {{pipe}}|
		/buttons labels={{var::nameList}} select the name of the character.|
		/setvar key=charName {{pipe}}|
		/ife (charName == '') {:
			/abort quiet=false Missing character name setting in input.|
		:}|
	:}|
:}|
/else {:
	/setvar key=charName No|
:}|
/ife ((charName != 'No') and (group == 'No')) {:
	/re-replace find="/<{{getvar::charName}}>\n/" replace="" {{lastMessage}}|
	/let key=clean {{pipe}}|
	/re-replace find="/\n<\/{{getvar::charName}}>/" replace="" {{var::clean}}|
	/var key=clean {{pipe}}|
	/reasoning-get {{var::i}}|
	/let key=think {{pipe}}|
	/ife (think != '') {:
		/reasoning-set {{var::think}}|
	:}|
	/re-replace find="/^[\s\S]*?<\/think>\s*/" replace="" {{var::clean}}|
	/let key=mess {{pipe}}|
	/message-edit await=true <{{getvar::charName}}>{{newline}}{{var::mess}}{{newline}}</{{getvar::charName}}>
:}|



/to-lower {{model}}|
/let key=model {{pipe}}|
/wi-list-books|
/let key=books {{pipe}}|
/ife (('deepseek' in model) and ('DeepSeek' not in books)) {:
	/world state=on silent=true DeepSeek|
:}|
/elseif ('DeepSeek' in books) {:
	/world state=off silent=true DeepSeek|
:}|

/let key=i 0|
/whilee (i <= {{lastMessageId}}) {:
	/messages names=on {{var::i}}|
	/let key=orgMessage {{pipe}}|
	/re-replace find="/:[\s\S]*/" replace="" {{var::orgMessage}} |
	/let key=name {{pipe}}|
	/ife (name == '{{user}}') {:
		/re-replace find="/<User_Input>\n/" replace="" {{var::orgMessage}}|
		/let key=clean {{pipe}}|
		/re-replace find="/\n<\/User_Input>/" replace="" {{var::clean}}|
		/var key=clean {{pipe}}|
		/reasoning-get {{var::i}}|
		/let key=think {{pipe}}|
		/ife (think != '') {:
			/reasoning-set at={{var::i}} {{var::think}}|
		:}|
		/re-replace find="/[\s\S]*?<\/think>\n/g" replace="" {{var::clean}}|
		/let key=mess {{pipe}}|
		/re-replace find="/^{{var::name}}:\s?/" replace="" {{var::mess}}|
		/var key=mess {{pipe}}|
		/message-edit message={{var::i}} await=true <User_Input>{{newline}}{{var::mess}}{{newline}}</User_Input>
	:}|
	/elseif (name == 'System') {:
		/re-replace find="/<System_Input>\n/" replace="" {{var::orgMessage}}|
		/let key=clean {{pipe}}|
		/re-replace find="/\n<\/System_Input>/" replace="" {{var::clean}}|
		/var key=clean {{pipe}}|
		/reasoning-get {{var::i}}|
		/let key=think {{pipe}}|
		/ife (think != '') {:
			/reasoning-set at={{var::i}} {{var::think}}|
		:}|
		/re-replace find="/[\s\S]*?<\/think>\n/g" replace="" {{var::clean}}|
		/let key=mess {{pipe}}|
		/re-replace find="/^{{var::name}}:\s?/" replace="" {{var::mess}}|
		/var key=mess {{pipe}}|
		/message-edit message={{var::i}} await=true <System_Input>{{newline}}{{var::mess}}{{newline}}</System_Input>
	:}|
	/else {:
		/re-replace find="/<{{var::name}}>\n/" replace="" {{var::orgMessage}}|
		/let key=clean {{pipe}}|
		/re-replace find="/\n<\/{{var::name}}>/" replace="" {{var::clean}}|
		/var key=clean {{pipe}}|
		/reasoning-get {{var::i}}|
		/let key=think {{pipe}}|
		/ife (think != '') {:
			/reasoning-set at={{var::i}} {{var::think}}|
		:}|
		/re-replace find="/^[\s\S]*?<\/think>\s*/" replace="" {{var::clean}}|
		/let key=mess {{pipe}}|
		/re-replace find="/^{{var::name}}:\s?/" replace="" {{var::mess}}|
		/var key=mess {{pipe}}|
		/message-edit message={{var::i}} await=true <{{var::name}}>{{newline}}{{var::mess}}{{newline}}</{{var::name}}>
	:}|
	/add i 1|
	/var key=i {{pipe}}|
:}|


/to-lower {{model}}|
/let key=model {{pipe}}|
/wi-list-books|
/let key=books {{pipe}}|
/ife (('deepseek' in model) and ('DeepSeek' not in books)) {:
	/world state=on silent=true DeepSeek|
:}|
/elseif ('deepseek' not in model) {:
	/world state=off silent=true DeepSeek|
:}|


/let key=exultationList ["Character Maker QR"]|
/let key=lastID {{lastMessageId}}|
/ife ((lastID >= 2) and ('{{char}}' not in exultationList)) {:
	/let key=i 0|
	/add {{var::lastID}} -2|
	/let key=stopID {{pipe}}|
	/whilee (i <= stopID) {:
		/let key=messageEdit {{noop}}|
		/message-get {{var::i}}|
		/let key=message {{pipe}}|
		/re-replace regex="\s*<\/history>" replace="" {{var::message}}|
		/var key=messageEdit {{pipe}}|
		/ife ((i == 0) and ('<history>' not in messageEdit)) {:
			/var key=messageEdit "<history>{{newline}}{{var::messageEdit}}"|
		:}|
		/ife (i == stopID) {:
			/var key=messageEdit "{{var::messageEdit}}{{newline}}</history>"|
		:}|
		/message-edit message={{var::i}} {{var::messageEdit}}| 
	:}|
:}|