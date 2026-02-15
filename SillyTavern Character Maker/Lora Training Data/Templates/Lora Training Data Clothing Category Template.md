```json
{
	"schema_type": "clothing_length_scale",
	"instruction": "Provide clothing coverage and length metadata with full schema. This template is applicable to any garment type, including skirts, dresses, pants, socks, gloves, etc.",
	"input": "Define the clothing length and coverage scale for [GARMENT].",
	"placeholder_conventions": {
		"purpose": "Allow flexible, generic references for garment types and coverage levels.",
		"allowed_placeholders": [
			"[GARMENT]",       // e.g., 'skirt', 'dress', 'sock'
			"<scale_name>",    // e.g., 'minimal', 'short', 'mid', 'long', 'maximal'
			"<body_area>",     // e.g., 'hips', 'mid-thigh', 'ankles'
			"<expected_movement>", 
			"<constraints_or_none>",
			"<context_or_style>",
			"<story_or_character_context>",
			"<brief_summary_of_coverage>"
		],
		"usage_guidelines": {
			"general_rule": "Use placeholders consistently. Replace with garment-specific values when generating concrete scales.",
			"length_scale": "Do not limit number of entries; the LLM can create as many coverage levels as appropriate."
		}
	},
	"output": {
		"id": "[GARMENT]_coverage_length_scale",  // unique ID for this scale
		"name": "[GARMENT] Coverage & Length Scale",
		"description": "A standardized scale describing coverage and length for [GARMENT]. Used to determine relative exposure, movement, and narrative implications.",
		
		"applies_to": [
			"[GARMENT]" // list can include multiple garment types if needed
		],

		"coverage_axis_definition": {
			"reference_points": [
				"<body_area>" // e.g., 'hips', 'knees', 'ankles', 'feet'
			],
			"direction": "Ascending scale_index values indicate increasing coverage and potential movement restriction."
		},

		"length_scale": [
			{
				"id": "[GARMENT]_coverage_<scale_name>",  // dynamic ID
				"scale_index": "<incremental_number>",    // 0,1,2,... ascending
				"name": "<scale_name> Coverage",          // e.g., 'Minimal Coverage'
				"synonyms": ["<scale_name> [GARMENT]"],  // alternative phrasing
				"approx_position": "<body_area>",        // where the garment reaches
				"coverage_description": "<brief_summary_of_coverage>", 
				"movement_behavior": "<expected_movement>", 
				"movement_constraints": "<constraints_or_none>",
				"common_use": "<context_or_style>",      // casual, formal, ceremonial, fetish, etc.
				"narrative_implications": "<story_or_character_context>", // e.g., modest, playful, bold
				"notes": ""
			}
			// LLM can generate additional entries; number of length_scale items is not limited
		],

		"usage_notes": "This scale defines garment length and body coverage relative to the wearer. The same scale_index values remain comparable across different garment types, even if the physical manifestation differs. Lower scale_index = less coverage, higher freedom of movement; higher scale_index = more coverage, heavier or formal garments."
	}
}
```