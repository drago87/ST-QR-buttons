Remove starting \`\`\`json and ending \`\`\` before pasting it into the Kink template.
```json
			// This block is pasted directly inside the "schema_type": "kink" when the kink has sexual mechanics. It replaces the existing sexual_extension
			"sexual_extension": {
				"scope": "sexual", // extension domain identifier
				
				"interaction_structure": {
					"primary_focus": "<body_part | sensation | dynamic | role | variable>",
					"interaction_type": "<active | passive | reciprocal | variable>",
					"typical_flow": "<high_level_description_of_how_the_kink_unfolds>",
					"notes": ""
				},
				
				"participants": [
					{
						"role": "<giver | receiver | switch | observer | variable>",
						"role_dynamics": "<how_this_participant_interacts_or_shifts_roles>",
						"awareness_state": "<aware | unaware | variable>",
						"notes": ""
					}
				],
				
				"intensity_scale": [
					{
						"id": "<kink_id>_low",
						"level": 1,
						"label": "Low Intensity",
						"description": "<light_exploratory_minimal_stimulation>",
						"notes": ""
					},
					{
						"id": "<kink_id>_medium",
						"level": 2,
						"label": "Moderate Intensity",
						"description": "<clear_engagement_with_noticeable_stimulation>",
						"notes": ""
					},
					{
						"id": "<kink_id>_high",
						"level": 3,
						"label": "High Intensity",
						"description": "<strong_stimulation_or_psychological_intensity>",
						"notes": ""
					}
				],
				
				"compatibility": {
					"required_conditions": [
						{
							"type": "<kink | item | role | anatomy | context | rule | personality>",
							"reference": "<id_or_name>",
							"description": "<why_this_condition_is_required>"
						}
					],
					"common_combinations": [
						{
							"type": "<kink | item | role | dynamic | personality>",
							"reference": "<id_or_name>",
							"description": "<why_they_pair_well>"
						}
					],
					"conflicts": [
						{
							"type": "<kink | item | boundary | anatomy | context | personality>",
							"reference": "<id_or_name>",
							"severity": "<low | moderate | high>",
							"reason": "<why_this_is_incompatible>"
						}
					],
					"notes": ""
				},
				
				"consent_model": {
					"default_mode": "<consensual | non_consensual | mixed | variable>",
					"boundary_sensitivity": "<low | moderate | high | variable>",
					"scenario_overrides": [
						{
							"scenario": "<description_of_roleplay_or_story_context>",
							"consent_present": "<true | false | ambiguous>",
							"risk_level": "<low | moderate | high>",
							"notes": "<optional_explanation>"
						}
					],
					"notes": ""
				},
				
				"notes": ""
			},
```