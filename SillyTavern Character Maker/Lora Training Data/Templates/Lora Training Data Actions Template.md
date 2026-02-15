```json
{
	"schema_type": "action",
	
	"_global_schema_notes": {
		// (This schema is an instructional template for both LLMs and human authors. Fields may include illustrative examples rather than closed enumerations.)
		"open_fields": [
			"actor_types",
			"body_parts_used",
			"valid_targets",
			"natural_language_triggers"
		],
		"examples_requirement": "Each action must include narrative examples for first person actor, first person receiver, and third person observer.",
		"consent_modeling": "Consent is modeled at the example level (true | false | ambiguous), not at the action level unless a domain extension specifies otherwise.",
		"enumeration_policy": "Arrays are illustrative unless explicitly marked as fixed. Pipe-delimited values (a | b | c) indicate static selections.",
		"extension_policy": "Domain-specific complexity should be modeled via optional extension blocks (e.g., sexual_extension). If a category implies an extension, the full extension schema should be included.",
		"training_note": "Structural clarity and repetition of intent are preferred over strict validation for LoRA training. Inline guidance is intentional."
	},
	
	"placeholder_conventions": {
		"purpose": "Provide consistent role grounding in narrative examples without hardcoding character names.",
		"allowed_placeholders": [
			"[ACTOR]",
			"[RECEIVER]",
			"[TARGET]"
		],
		"usage_guidelines": {
			"general_rule": "Use placeholders once early in the example to establish identity, then use pronouns naturally for readability.",
			"first_person_actor": "Use [RECEIVER] to identify the other party before pronoun use.",
			"first_person_receiver": "Use [ACTOR] to identify the other party before pronoun use.",
			"third_person_observer": "Use both [ACTOR] and [RECEIVER] initially, then rely on pronouns."
		},
		"pronoun_policy": "Pronouns are encouraged after role identity is established. Do not remove pronouns entirely."
	},
	
	"instruction": "Provide action metadata with full schema.",
	"input": "Define the action '<action_name>'.",
	
	"output": {
		"id": "<action_id>",
		"name": "<action_name>",
		
		"category": [
			"<physical>",
			"<social>",
			"<emotional>",
			"<combat>",
			"<sexual>",
			"<utility>"
			// (select applicable categories; list is illustrative)
		],
		
		"description": "<clear explanation of what the action is>",
		
		"performed_by": {
			"actor_types": [
				"humanoid",
				"quadruped",
				"serpentine",
				"avian",
				"any"
				// (examples — select what applies or add others as appropriate)
			],
			"requirements": [
				"<anatomical or situational requirements>"
			],
			"limitations": [
				"<what prevents or restricts this action>"
			]
		},
		
		"targets": {
			"can_target_self": true,
			"can_target_others": true,
			"valid_targets": [
				"person",
				"creature",
				"object",
				"area"
				// (examples — select or extend as appropriate)
			],
			"notes": ""
		},
		
		"execution": {
			"body_parts_used": [
				"hands",
				"forehooves",
				"mouth",
				"tail",
				"wings"
				// (examples — not exhaustive)
			],
			"movement_type": "<stationary | locomotion | gesture | rhythmic | forceful | other (specify)>",
			"duration": "<instant | brief | sustained>",
			"intensity_range": "<low to high>"
		},
		
		"narrative_guidance": {
			"first_person_actor": {
				"prose_focus": "<intent | physical effort | control>",
				"sentence_style": "<short and forceful | deliberate | restrained>",
				"perspective_emphasis": "actor-centric",
				"recommended_tense": "<present | past>",
				"descriptive_priority": [
					"<muscle engagement>",
					"<decision and follow-through>",
					"<impact or result>"
				],
				"avoid_phrases": [
					"<phrases implying passivity>"
				]
			},
			"first_person_receiver": {
				"prose_focus": "<sensation | shock | emotional response>",
				"sentence_style": "<fragmented | reactive | breathy>",
				"perspective_emphasis": "body-centric",
				"recommended_tense": "<present>",
				"descriptive_priority": [
					"<sudden sensation>",
					"<loss or shift of balance>",
					"<emotional reaction>"
				],
				"avoid_phrases": [
					"<phrases implying control>"
				]
			},
			"second_person_actor": {
				"prose_focus": "<intent | physical effort | control>",
				"sentence_style": "<short and forceful | deliberate | restrained>",
				"perspective_emphasis": "actor-centric",
				"recommended_tense": "<present | past>",
				"descriptive_priority": [
					"<muscle engagement>",
					"<decision and follow-through>",
					"<impact or result>"
				],
				"avoid_phrases": [
					"<phrases implying passivity>"
				]
			},
			"second_person_receiver": {
				"prose_focus": "<sensation | shock | emotional response>",
				"sentence_style": "<fragmented | reactive | breathy>",
				"perspective_emphasis": "body-centric",
				"recommended_tense": "<present>",
				"descriptive_priority": [
					"<sudden sensation>",
					"<loss or shift of balance>",
					"<emotional reaction>"
				],
				"avoid_phrases": [
					"<phrases implying control>"
				]
			},
			"third_person_observer": {
				"prose_focus": "<motion | cause-and-effect | interaction>",
				"sentence_style": "<clear | cinematic | descriptive>",
				"perspective_emphasis": "interaction-centric",
				"recommended_tense": "<past | present>",
				"descriptive_priority": [
					"<visible motion>",
					"<reaction timing>",
					"<spatial relationship>"
				],
				"avoid_phrases": [
					"<internal monologue unless specified>"
				]
			}
		},
		
		"natural_language_triggers": [
			"<verbs or phrases that imply this action>"
			// (examples — add as many as needed)
		],
		"extensions": {
			"sexual_extension": {
				// (If you are making a sexual action replace this with the sexual extension template)
			},
			"combat_extension": {
				// (If you are making a combat action replace this with the combat extension template)
			},
			"social_extension": {
				// (If you are making a social action replace this with the social extension template)
			}
		},
		"examples": {
			"first_person_actor": [
				{
					"consent": "<true | false | ambiguous>",
					"description": "<example prose using placeholder conventions>"
				}
			],
			"first_person_receiver": [
				{
					"consent": "<true | false | ambiguous>",
					"description": "<example prose using placeholder conventions>"
				}
			],
			"second_person_actor": [
				{
					"consent": "<true | false | ambiguous>",
					"description": "<example prose using placeholder conventions>"
				}
			],
			"secind_person_receiver": [
				{
					"consent": "<true | false | ambiguous>",
					"description": "<example prose using placeholder conventions>"
				}
			],
			"third_person_observer": [
				{
					"consent": "<true | false | ambiguous>",
					"description": "<example prose using placeholder conventions>"
				}
			]
		},
		
		"notes": "<optional clarifications, edge cases, or cross-species considerations>"
	}
}
```