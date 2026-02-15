/let key=branch {{noop}}|
/if else={: /var key=branch Fetch-Files| :} left=beta rule=neq right="Yes" {:
/var key=branch main|
:}|

/extension-exists SillyTavern-LALib | /let key=lalib {{pipe}}|
/if else={: /popup <div>The Extension <a href="https://github.com/LenAnderson/SillyTavern-LALib">LALib</a> is needed for this to work.</div><div>To install it press the <i class="fa-solid fa-cubes"></i> button.</div><div> Then press the "<i class="fa-solid fa-cloud-arrow-down"></i> Install extension" button and paste the LALib github URL into the text box.</div>:} left=lalib right=true rule=eq {:
	/extension-state SillyTavern-LALib | /let key=laliben {{pipe}}|
	/if else={: /popup The Extension LALib needs to be enabled for this to work. :} left=laliben right=true rule=eq {:
		/fetch https://raw.githubusercontent.com/drago87/ST-Character-Maker/refs/heads/{{var::branch}}/SillyTavern%20Character%20Maker/Install/Install%20Script%20choice.md|
		/qr-create set="CMC Temp" label="Install Script" {{pipe}}|
		//[[Install Script choice]]|
		/qr-chat-set-on CMC Temp|
		/:"CMC Temp.Install Script"|
		
	  :}|
:}|