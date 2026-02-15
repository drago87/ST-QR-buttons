```json
{
	"schema_type": "text_representation",
	"instruction": "Define a chat text representation system where narration, speech, and thoughts use distinct formatting patterns.",
	"input": [
		"Use Fiction Format",
		"Use Fiction Style",
		"Apply Fiction Format to the chat text",
		"Format chat text in Fiction Style"
	],
	"output": {
		"id": "fiction_format",
		"name": "Fiction Format",
		"category": ["Text Representation", "Chat Formatting", "Narrative Style"],
		"description": "A common fiction-style text representation where narration is plain text, speech is enclosed in quotes, and thoughts are wrapped in asterisks.",
		
		"roles": [
			{
				"id": "narration_role",
				"label": "Narration",
				"description": "Narrative text describing the scene or events, not attributed to any character's internal voice.",
				"formatting": {
					"type": "plain_text",
					"pattern": "<text>",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": false
					}
				},
				"examples": [
					{
						"raw_text": "The sun set over the horizon, casting long shadows.",
						"formatted_text": "The sun set over the horizon, casting long shadows."
					}
				]
			},
			{
				"id": "speech_role",
				"label": "Speech",
				"description": "Text representing a character speaking aloud.",
				"formatting": {
					"type": "quoted",
					"pattern": "\"<text>\"",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": true
					}
				},
				"examples": [
					{
						"raw_text": "Hello, are you there?",
						"formatted_text": "\"Hello, are you there?\""
					}
				]
			},
			{
				"id": "thought_role",
				"label": "Thought",
				"description": "Text representing a character's internal monologue or thinking.",
				"formatting": {
					"type": "wrapped",
					"pattern": "*<text>*",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": true
					}
				},
				"examples": [
					{
						"raw_text": "I hope this plan works.",
						"formatted_text": "*I hope this plan works.*"
					}
				]
			}
		],
		
		"default_behavior": {
			"fallback_role": "narration_role",
			"unknown_role_policy": "treat_as_fallback"
		},
		
		"notes": "All roles must be formatted consistently. Unknown or unspecified roles default to narration."
	}
}
```
```json
{
	"schema_type": "text_representation",
	"instruction": "Define a chat text representation system where narration, speech, and thoughts use distinct formatting patterns.",
	"input": [
		"Use Emphasized Narration Format",
		"Use Emphasized Narration Style",
		"Apply Emphasized Narration Format to the chat text",
		"Format chat text in Emphasized Narration Style"
	],
	"output": {
		"id": "emphasized_narration_format",
		"name": "Emphasized Narration Format",
		"category": ["Text Representation", "Chat Formatting", "Narrative Style"],
		"description": "A text representation style where narration is wrapped in asterisks, speech is enclosed in quotes, and thoughts are expressed in plain text.",
		
		"roles": [
			{
				"id": "narration_role",
				"label": "Narration",
				"description": "Narrative text describing the scene or events, visually emphasized through formatting.",
				"formatting": {
					"type": "wrapped",
					"pattern": "*<text>*",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": true
					}
				},
				"examples": [
					{
						"raw_text": "The wind howled through the empty street.",
						"formatted_text": "*The wind howled through the empty street.*"
					}
				]
			},
			{
				"id": "speech_role",
				"label": "Speech",
				"description": "Text representing a character speaking aloud.",
				"formatting": {
					"type": "quoted",
					"pattern": "\"<text>\"",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": true
					}
				},
				"examples": [
					{
						"raw_text": "We should leave now.",
						"formatted_text": "\"We should leave now.\""
					}
				]
			},
			{
				"id": "thought_role",
				"label": "Thought",
				"description": "Text representing a character's internal monologue or thinking without visual markers.",
				"formatting": {
					"type": "plain_text",
					"pattern": "<text>",
					"constraints": {
						"allows_nesting": false,
						"preserves_whitespace": true,
						"requires_delimiters": false
					}
				},
				"examples": [
					{
						"raw_text": "This situation feels wrong.",
						"formatted_text": "This situation feels wrong."
					}
				]
			}
		],
		
		"default_behavior": {
			"fallback_role": "thought_role",
			"unknown_role_policy": "treat_as_fallback"
		},
		
		"notes": "All roles must be formatted consistently. Unknown or unspecified roles default to thought."
	}
}
```