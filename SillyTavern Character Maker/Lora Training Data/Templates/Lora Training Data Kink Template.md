```json
{
	"schema_type": "kink", // fixed type identifier for kink schemas
	
	"instruction": "Provide kink metadata with full schema, including extensions for sexual, combat, and social contexts.",
	"input": "What is <kink_name>?", // natural language query form
	
	"output": {
		"id": "<kink_id>", // unique identifier, lowercase recommended
		"name": "<kink_name>", // human-readable name
		"synonyms": ["<synonyms>"], // alternative names or related terms
		"category": ["Kink", "<category_from_list>"], // hierarchical classification
		"description": "<describe_what_the_kink_is>", // neutral, descriptive definition
		
		"ontology": {
			"core_dynamic": "<physical | psychological | social | power | sensory | mixed>", // dominant mechanism of the kink
			"primary_focus": "<body_part | sensation | observation | control | role | narrative | variable>", // main experiential focus
			"consent_structure": "<consensual_by_default | non_consensual_by_default | mixed | variable>", // structural consent pattern
			"notes": "<optional_ontological_notes>" // conceptual clarifications
		},
		
		"roles": {
			"giver": {
				"id": "<kinkname>_giver_role", // always keep "_giver_role"
				"label": "Giver",
				"definition": "<enjoys_doing_or_applying>", // what the giver does or expresses
				"typical_awareness": "<aware | unaware | variable>", // whether the giver usually knows the interaction is happening
				"notes": "<optional_notes_or_context>"
			},
			"receiver": {
				"id": "<kinkname>_receiver_role", // always keep "_receiver_role"
				"label": "Receiver",
				"definition": "<enjoys_receiving_or_experiencing>", // what the receiver experiences
				"typical_awareness": "<aware | unaware | variable>", // whether the receiver usually knows the interaction is happening
				"notes": "<optional_notes_or_context>"
			}
		},
		
		"consent_and_safety": {
			"typical_consent_state": "<consensual | non_consensual | mixed | variable>", // descriptive: what usually happens
			"consent_requirement": "<required | not_required | context_dependent>", // normative: ethical requirement
			"negotiation_norm": "<expected | uncommon | context_dependent>", // how often negotiation occurs
			"physical_risk": "<low | moderate | high>", // bodily harm risk
			"psychological_risk": "<low | moderate | high>", // emotional harm risk
			"legal_risk": "<low | moderate | high>", // legal consequence risk
			
			"default_scenario": {
				"description": "<most_common_real_world_expression_of_the_kink>",
				"consent_present": "<true | false | ambiguous>",
				"risk_level": "<low | moderate | high>",
				"notes": "<optional_notes>"
			},
			
			"scenario_overrides": [
				{
					"scenario": "<description_of_alternative_context>",
					"consent_present": "<true | false | ambiguous>",
					"consent_requirement": "<required | not_required | context_dependent>",
					"risk_level": "<low | moderate | high>",
					"notes": "<optional_explanation>"
				}
			],
			
			"notes": "<optional_safety_context>"
		},
		
		"extensions": {
			"social_extension": {
				"scope": "social", // for group dynamics, etiquette, or roleplay context
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			}
		},
		
		"notes": "<optional_notes_or_context>"
	}
}
```