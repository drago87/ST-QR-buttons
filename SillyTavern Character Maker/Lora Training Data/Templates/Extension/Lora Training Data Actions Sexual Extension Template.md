remove starting \`\`\`json and ending \`\`\` before pasting it into the Action template.

```json
		"sexual_extension": {
			"schema_type": "sexual_extension",
			
			"consent_framework": {
				"explicit_consent_possible": "<true | false>",
				"consent_required": "<true | false>",
				"consent_withdrawal_possible": "<true | false>",
				"notes": "<how consent is typically communicated, withheld, or withdrawn for this action>"
			},
			
			"sexual_roles": {
				"actor_role": "<initiator | participant | receiver | dominant | submissive | other (specify)>",
				"receiver_role": "<participant | receiver | submissive | other (specify)>",
				"role_notes": "<clarify role dynamics if they vary or are context-dependent>"
			},
			
			"anatomical_focus": {
				"actor_anatomy": [
					"<anatomical features used by the actor>"
					// (examples — select or extend as appropriate)
				],
				"receiver_anatomy": [
					"<anatomical features affected or stimulated>"
					// (examples — select or extend as appropriate)
				],
				"contact_type": "<external | internal | mixed | indirect | other (specify)>"
			},
			
			"stimulation_profile": {
				"primary_stimulation": [
					"<physical | sensory | emotional | psychological>"
					// (examples — select or extend as appropriate)
				],
				"intensity_curve": "<steady | escalating | fluctuating | abrupt | other (specify)>",
				"rhythm_pattern": "<slow | moderate | fast | irregular | other (specify)>"
			},
			
			"risk_and_aftercare": {
				"physical_risks": [
					"<potential physical risks or discomforts>"
					// (examples — optional)
				],
				"emotional_risks": [
					"<potential emotional or psychological risks>"
					// (examples — optional)
				],
				"aftercare_recommended": "<true | false>",
				"aftercare_notes": "<recommended aftercare behaviors if applicable>"
			},
			
			"cultural_or_contextual_notes": "<optional notes about norms, taboos, or context-specific interpretation>"
		},
```