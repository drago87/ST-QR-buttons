/ife (imageGen == 'Yes') {:
	/message-get {{getvar::imageID}} |
	/let key=images {{pipe}}|
	/getat index=extra {{var::images}}|
	/var key=images {{pipe}}|
	/getat index=inline_image {{var::images}}|
	/let key=inline_image {{pipe}}|
	/ife (inline_image) {:
		/getat index=media {{var::images}}| 
		/setvar key=images {{pipe}}|
		/length {{var::images}}|
		/let key=imageAmount {{pipe}}|
		/setvar key=imageSelect {{noop}}|
		/setvar key=imageSelectCoice []|
		/ife (imageAmount == 1) {:
			/setvar key=imageIndex 0|
		:}|
		/elseif (imageAmount > 1) {:
			/let key=i 0|
			/foreach {{getvar::images}} {:
				/getat index=type {{var::item}}|
				/let key=test {{pipe}}|
				/ife (test == 'image') {:
					/getat index=url {{var::item}}|
					/addvar key=imageSelect <div style="display:inline-block; margin:5px;"><img src="{{pipe}}" style="width:150px; height:auto; border-radius:5px;"></div>|
					/addvar key=imageSelectCoice "Image {{var::i}}"|
				:}|
				/add {{var::i}} 1|
				/var key=i {{pipe}}|
			:}|
		:}|
	:}|
	
:}|

/buttons labels={{getvar::imageSelectCoice}} Select the image you want to use.{{getvar::imageSelect}}|
/let key=imageIndex {{pipe}}|
/re-replace find="/Image /" replace="" {{var::imageIndex}}|
/var key=imageIndex {{pipe}}|


/caption quiet=true mesId={{getvar::imageID}} index={{getvar::imageIndex}} "Identify all garments and wearable accessories. Output only the clothing tags in a comma-separated list. Exclude all non-clothing tags."|
/setvar key=00 {{pipe}}|

/setvar key=imageSelectCoice []|
/let key=i 0|
/foreach {{getvar::images}} {:
	/getat index=type {{var::item}}|
	/let key=test {{pipe}}|
	/ife (test == image) {:
		/getat index=url {{var::item}}|
		/addvar key=imageSelect <div style="display:inline-block; margin:5px;"><img src="{{pipe}}" style="width:150px; height:auto; border-radius:5px;"></div>|
		/addvar key=imageSelectCoice "Image {{var::i}}"|
	:}|
	/add {{var::i}} 1|
	/var key=i {{pipe}}|
:}|