head
neck
one_piece
two_piece_top
two_piece_bottom
legwear
socks
shoes
underwear_top
underwear_bottom






/foreach {{getvar::outfits}} {:
	/getvar key=outfits index={{var::index}}|
	/let key=outfit_obj {{pipe}}|
	/getat index=outfit_name {{var::outfit_obj}}|
	/let key=outfit_name {{pipe}}|
	/foreach {{var::outfit_obj}} {:
		/getat index=type {{var::item}}|
		/let key=type {{pipe}}|
		/echo {{var::type}}|
		/ife (type == 'string') {:
			/getat index=name {{var::item}}|
			/let key=name_obj {{pipe}}|
			/getat index=description {{var::item}}|
			/let key=description_obj {{pipe}}|
			//setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		:}|
		/elseif (type == 'array') {:
			/getat index=items {{var::item}}|
			/let key=items {{pipe}}|
			/foreach {{var::items}} {:
				/getat index=name {{var::item}}|
				/let key=name_obj {{pipe}}|
				/getat index=description {{var::item}}|
				/let key=description_obj {{pipe}}|
			:}|
		:}|
	:}|
:}|
/join {{getvar::00a}}|
/setvar key=00a {{pipe}}|


	/foreach {{getvar::outfits}} {:
		/getvar key=outfits index={{var::index}}|
		/let key=outfit_obj {{pipe}}|
		/getat index=outfit_name {{var::outfit_obj}}|
		/let key=outfit_name {{pipe}}|
		/foreach {{var::outfit_obj}} {:
			/getat index=type {{var::item}}|
			/let key=type {{pipe}}|
			/echo {{var::type}}|
			/ife (type == 'string') {:
				/getat index=name {{var::item}}|
				/let key=name_obj {{pipe}}|
				/getat index=description {{var::item}}|
				/let key=description_obj {{pipe}}|
				//setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
			:}|
			/elseif (type == 'array') {:
				/getat index=items {{var::item}}|
				/let key=items {{pipe}}|
				/foreach {{var::items}} {:
					/getat index=name {{var::item}}|
					/let key=name_obj {{pipe}}|
					/getat index=description {{var::item}}|
					/let key=description_obj {{pipe}}|
				:}|
			:}|
		:}|
	:}|







		/getat index=head {{var::outfit_obj}}|
		/let key=outfit_head {{pipe}}|
		/getat index=name {{var::outfit_head}}|
		/let key=name {{pipe}}|
		/getat index=description {{var::outfit_head}}|
		/let key=description {{pipe}}|
		/setvar key=templateCopy {{getvar::lorebookEntryTemplate}}|
		/re-replace find="/--uid--/g" replace="{{var::lorebookIndex}}" {{getvar::templateCopy}}|
		/re-replace find="/--comment--/g" replace="Start trigger for Outfit: {{var::outfit_name}}" {{pipe}}|
		/re-replace find="/--content--/g" replace="Start trigger for Outfit: {{var::outfit_name}}" {{pipe}}|
		/setvar key=templateCopy {{pipe}}|
		
		//addvar key=createdLorebook {{getvar::templateCopy}}|
	:}|