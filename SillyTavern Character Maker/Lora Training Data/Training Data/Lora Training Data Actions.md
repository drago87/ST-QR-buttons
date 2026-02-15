```json
{
	"schema_type": "action",
	
	"_global_schema_notes": {
		"open_fields": [
			"actor_types_examples",
			"body_parts_used_examples",
			"valid_targets_examples",
			"natural_language_triggers"
		],
		"examples_requirement": "Each action must include examples for first person actor, first person receiver, and third person observer.",
		"consent_modeling": "Consent is modeled at the example level (true | false | ambiguous), not at the action level unless a domain extension specifies otherwise.",
		"enumeration_policy": "Lists ending in *_examples are illustrative, not closed enumerations. Additional values are allowed when anatomically or contextually appropriate.",
		"extension_policy": "Domain-specific complexity should be modeled via optional extension blocks (e.g., sexual_extension)."
	},
	
	"placeholder_conventions": {
		"purpose": "Provide consistent role grounding in narrative examples without hardcoding character names.",
		"placeholders": [
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
		"pronoun_policy": "Pronouns are encouraged after role identity is established."
	},
	
	"instruction": "Provide action metadata with full schema.",
	"input": ["Define the action 'Penis outside pussy movement'."],
	
	"output": {
	    "id": "penis_outside_pussy_movement",
	    "name": "Penis outside pussy movement",
	    "category": ["physical", "sexual"],
	    "description": "A sexual action involving controlled, repetitive movement of the penis along the external genital area (labia and surrounding tissue) without vaginal penetration.",
		
	    "performed_by": {
			"actor_types_examples": [
		        "humanoid",
		        "any (if anatomy matches)"
			],
			"requirements": [
				"anatomical: external penis",
				"close physical proximity to target’s external genitalia"
			],
			"limitations": [
		        "lack of compatible anatomy",
		        "physical barriers such as clothing",
		        "situational or consent-related constraints"
			]
	    },
		
		"targets": {
			"can_target_self": false,
			"can_target_others": true,
			"valid_targets_examples": [
				"person",
				"creature"
			],
		"notes": "Target must possess external genital structures analogous to labia for the action to be meaningful."
	    },
		
	    "execution": {
			"body_parts_used_examples": [
		        "genitalia: penis",
		        "pelvic region"
			],
			"movement_type": "rhythmic",
			"duration": "sustained",
			"intensity_range": "low to high"
	    },
		
	    "narrative_guidance": {
			"first_person_actor": {
		        "prose_focus": "control",
		        "sentence_style": "deliberate",
		        "perspective_emphasis": "actor-centric",
		        "recommended_tense": "present",
		        "descriptive_priority": [
					"intentional movement",
					"pace adjustment",
					"partner response"
		        ],
		        "avoid_phrases": [
					"accidental or passive framing"
		        ]
			},
			"first_person_receiver": {
		        "prose_focus": "sensation",
		        "sentence_style": "reactive",
		        "perspective_emphasis": "body-centric",
		        "recommended_tense": "present",
		        "descriptive_priority": [
					"surface sensation",
					"anticipation or discomfort",
					"emotional reaction"
		        ],
		        "avoid_phrases": [
					"phrases implying full control when consent is absent"
		        ]
			},
			"third_person_observer": {
		        "prose_focus": "interaction",
		        "sentence_style": "descriptive",
		        "perspective_emphasis": "interaction-centric",
		        "recommended_tense": "past",
		        "descriptive_priority": [
					"repetitive motion",
					"body alignment",
					"reciprocal or resisted response"
		        ],
				"avoid_phrases": [
					"internal monologue unless specified"
		        ]
			}
	    },
		
	    "natural_language_triggers": [
			"rub against",
			"grind along",
			"move along externally"
	    ],
		
	    "sexual_extension": {
			"_schema_note": "Required because category includes 'sexual'.",
			
			"anatomy": {
		        "actor_required_examples": [
				"penis"
		        ],
		        "target_required_examples": [
					"external genitalia",
					"labia or analogous structure"
		        ],
		        "compatibility_notes": "Applicable across species with compatible external genital anatomy."
			},
			
			"stimulation": {
		        "type_examples": [
					"friction",
					"pressure"
		        ],
		        "intended_effects_examples": [
					"arousal",
					"anticipation"
		        ],
		        "sensitivity_factors": [
					"pace",
					"duration",
					"prior arousal",
					"surface sensitivity"
		        ]
			},
			
			"consent": {
		        "model": "example-level",
		        "possible_states": [
					"true",
					"false",
					"ambiguous"
		        ],
		        "notes": "Consent is represented per example rather than as an inherent property of the action."
			},
			
			"emotional_context": {
		        "possible_tones_examples": [
					"intimate",
					"playful",
					"tense",
					"exploitative"
		        ],
		        "relationship_dependency": "unspecified"
			},
			
			"aftereffects": {
		        "possible_outcomes_examples": [
					"increased arousal",
					"frustration",
					"emotional closeness",
					"emotional discomfort"
		        ],
		        "cleanup_or_recovery": "May involve repositioning, emotional reassurance, or disengagement depending on context."
			}
	    },
		
	    "examples": {
			"first_person_actor": [
				{
					"consent": true,
					"description": "I move my penis along [RECEIVER]'s pussy, feeling the slick feeling and smooth texture as i go back and forth. As i reach the top of the pussy i feel her clit scrape against the underside of my shaft."
				},
		        {
					"consent": false,
					"description": "I hold down [RECEIVER]'s hips with one hand and keep my penis moving along her pussy, ignoring her attempts to push me away. Her labia are tight and she feels stiff beneath me, she reacts each time my shaft glides over her clit, the clit getting harder with each pass."
				}
			],
			"first_person_receiver": [
		        {
					"consent": true,
					"description": "As [ACTOR]'s penis is rubbing between my pussy lips, my clit starts to get hard and sensitive from the friction. I can feel his cock pressing against my skin and i'm starting to anticipate what he'll do next."
		        },
		        {
					"consent": false,
					"description": "I try to move my hips away, but [ACTOR]'s hold me down. His cock is rubbing against my labia, making my clit throb and ache. The sensation is intense and uncomfortable, and i don't enjoy it."
		        }
			],
			"third_person_observer": [
				{
					"consent": true,
					"description": "[ACTOR] and [RECEIVER] move closely, their bodies aligned perfectly as [ACTOR]'s cock rubs against [RECEIVER]'s pussy. She seems to be enjoying it, her hips tilting upwards slightly."
				},
				{
					"consent": false,
					"description": "[ACTOR] keeps moving his penis along [RECEIVER]'s pussy even though she's trying to resist. He's not paying attention to her reactions, just focusing on his own pleasure."
				}
			]
	    },
		
	    "notes": "This action is typically associated with foreplay but may occur in a wide range of relational and situational contexts."
	}
}
```