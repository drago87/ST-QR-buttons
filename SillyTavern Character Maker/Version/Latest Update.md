<div style="background:#0b0d12;border:1px solid rgba(100,200,255,.35);border-radius:12px;color:#e9edf5;font-family:Inter,system-ui,sans-serif;max-width:520px;margin:20px auto;">
  <!-- Header -->
  <div style="padding:12px 16px;text-align:center;text-transform:uppercase;letter-spacing:.14em;font-weight:700;font-size:13px;background:#0a1a24;border-bottom:1px solid rgba(100,200,255,.25);color:#64c8ff;">
    🔄 Update
  </div>

  <!-- Divider -->
  <div style="height:1px;background:rgba(100,200,255,.2);margin:0;"></div>
	
	<!-- Content -->
	<div style="padding:16px;font-size:13px;line-height:1.6;color:#d5f1ff;">
	    Latest changes and notes:
		<ul style="margin:8px 0 0 16px; padding:0; list-style-type:disc; text-align:left;">
		<div style="margin-bottom: 1.2em;">⚠️ most of this is untested but should work in teory. If you find a bug please report it here or on Discord (Prefer Discord)</div>
		<li>Moved beta branch to main on github and have updated files to reflect that.</li>
		<li>Updated the instructions with the new links, instructions for debug and  beta usage.</li>
		<li>Added a update check to the "New Character" script.</li>
		<li>Added the Optional Outfits Lorebook <a href="https://github.com/drago87/ST-Character-Maker/blob/main/SillyTavern%20Character%20Maker/LoreBooks/General/CMC%20Anatomy.json" target="_blank" rel="noopener" style="color:#64c8ff;text-decoration:none;font-weight:500;">CMC Outfits</a> (Not fully implemented yet.)</li>
		<li>Updated the "CMC Guides" with a chatGPT message to generate outfits.</li>
		<li>Started to add logic for making the character sheet into a lorebook (Instead of having everything about the character in the Character Description you will have the option to save it as a Lorebook. This will in most cases save tokens when playing with the generated character.)
		<ul>
			<li>Appearance</li>
			<li>Outfit</li>
			<li>Items</li>
			<li>Sexual Items</li>
			<li>Sexual Kinks</li>
			<li>Sexual Abilities</li>
		</ul>
		</li>
	       
		<li>Added generation for Pussy Depth, Pussy Stretch, Dick Lenght and Dick Girth</li>
		<li>Updated the "CMC Templates" "Character Template" to be able to add the added generation. Also renamed it to "Character Template Standard"</li>
		<li>Added "Character Template Lorebook" to the "CMC Templates" to work better with the Lorebook mode.</li>
		<li>Uppdated the "CMC Prompt" Lorebooks for the new generations</li>
		<li>Added a check for when doing characters that already exist (aka if you want to make Goku from Dragon Ball, Rukia from bleach etc..) letting you ask your LLM what it knows about the character.</li>
		<li>Added a check for when doing characters that already exist (aka if you want to make Goku from Dragon Ball, Rukia from bleach etc..) letting you ask your LLM what it knows about the character.</li>
		<li>Added a way to change the prompt order for LLM's that work better when using a different order from contexts, examples, task, instructions. (This is set as the standard order but can be changed from the "CMC Menu". (Updated for it to work))</li>
		<li>Added "role" and "output trigger" to the order.</li>
		<li>Added "Modesty Levels" to personality. Updated the "CMC Variables" lorebook with the choices. (Possible to manually add more)</li>
		<li>Updated the Character Templates to include "Modesty Levels"</li>
		<li>Added more gender specific variables to the "Get Character information script"</li>
		<li>Added a removal for thinking models to remove the <think> reasoning</li>
		<li>Added "Prompts" and "Information" lorebook for DeepSeek models (Still a copy of the EsotericSage)</li>
		<li>Added "Model Role" and "Output Trigger" to the "Prompts" lorebooks ("Model Role" and "Output Trigger" is empty for the old lorebooks EsotericSage have "Output Trigger" built into the "Instruction" part of the prompts)</li>
		<li>Added a option in the CMC Menu to Enable **XML Tags** for the prompts (Recommended for DeepSeek models)</li>
		<li>Added Logic to use a Vision Model to help with Appearance and Outfit generation</li>
		<li>Removed the Quantization from the model name.</li>
		<li>Added generation for Socks and removed it from footwear</li>
		<li>Renamed the variable 'outfitLegs' to 'outfitLegwear' and 'outfitLegsDescription' to 'outfitLegwearDescription'</li>
		<li>Split 'Pubic Hair Style' (CMC Variables) into 2 'Cock Pubic Hair Style' and 'Pussy Pubic Hair Style'</li>
		</ul>
	
	</div>
</div>