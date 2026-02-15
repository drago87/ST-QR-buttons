remove starting \`\`\`json and ending \`\`\` before pasting it into the Action template.

```json
		"combat_extension": {
			"schema_type": "combat_extension",

			"combat_context": {
				"engagement_type": "<melee | ranged | grappling | ambush | mixed | other (specify)>",
				"lethality_intent": "<nonlethal | restrained | lethal | ambiguous>",
				"combat_environment": [
					"<open terrain>",
					"<confined space>",
					"<urban>",
					"<natural>"
					// (examples — select or extend as appropriate)
				]
			},

			"combat_roles": {
				"actor_role": "<attacker | defender | aggressor | ambusher | duelist | other (specify)>",
				"target_role": "<defender | opponent | victim | other (specify)>",
				"role_notes": "<clarify role dynamics if they shift during the action>"
			},

			"force_application": {
				"force_type": "<blunt | piercing | slashing | constricting | energy | other (specify)>",
				"force_direction": "<linear | sweeping | downward | upward | rotational | other (specify)>",
				"contact_mode": "<direct | indirect | sustained | momentary | other (specify)>"
			},

			"weapons_and_tools": {
				"weapons_used": [
					"<natural anatomy>",
					"<improvised weapon>",
					"<manufactured weapon>"
					// (examples — select or extend as appropriate)
				],
				"weapon_notes": "<constraints, reach considerations, or special properties>"
			},

			"damage_and_effects": {
				"intended_effects": [
					"<pain>",
					"<disabling>",
					"<disarming>",
					"<incapacitating>",
					"<killing>"
					// (examples — select or extend as appropriate)
				],
				"collateral_risk": "<low | moderate | high>",
				"effect_notes": "<edge cases, partial success, or unintended outcomes>"
			},

			"rules_of_engagement": {
				"consent_present": "<true | false | ambiguous>",
				"authority_or_justification": "<self-defense | duel | law enforcement | warfare | criminal | other (specify)>",
				"constraints": [
					"<moral>",
					"<legal>",
					"<situational>"
					// (examples — optional)
				]
			},

			"aftermath": {
				"immediate_outcome": "<actor_advantage | target_advantage | stalemate | disengagement>",
				"follow_up_required": [
					"<medical attention>",
					"<retreat>",
					"<pursuit>"
					// (examples — optional)
				],
				"aftermath_notes": "<narrative or mechanical consequences>"
			},

			"cultural_or_genre_notes": "<optional notes about genre conventions, honor codes, or setting-specific combat norms>"
		}
```