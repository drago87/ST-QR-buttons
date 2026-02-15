```json
{
	"schema_type": "archetype",
	"instruction": "Provide a clear definition and metadata for the requested archetype.",
	"input": ["What is an 'Anthropomorphic' character?"],
	"placeholder_conventions": {
		"purpose": "Provide consistent grounding for archetype definitions without hardcoding names.",
		"allowed_placeholders": ["<archetype_name>", "<Example>", "<Synonym>"],
		"usage_guidelines": "Use placeholders early in examples, then natural language afterwards."
	},
	"output": {
		"archetype": "Anthropomorphic",
		"definition": "A character that blends human and animal traits. They often have an animal-shaped body but display human posture, facial expressions, speech, reasoning, and social behaviors. Anthropomorphic characters are more animalistic in appearance and structure than Beastkin, having more fully integrated animal physiology.",
		"traits": {
			"morphology": "Animal body plan with human-like stance or posture. Greater animal integration than Beastkin, such as full fur coverage, paws, muzzles, or digitigrade legs.",
			"behavioral_profile": "Speaks, reasons, and interacts with the world similarly to humans. Uses tools, wears clothing, expresses emotions.",
			"communication": "Human-level language and expressiveness.",
			"cognitive_level": "Human-equivalent intelligence."
		},
		"examples": ["Maid Marian (Disney’s Robin Hood)", "Characters from Zootopia", "Fox McCloud (Star Fox)"],
		"synonyms": ["Furry", "Therianthrope (broad usage)", "Humanized Animal"],
		"not_includes": ["Fully human characters with minor animal traits", "Quadrupedal animals without human cognition or speech"],
		"notes": ""
	}
}
```

```json
{
	"schema_type": "archetype",
	"instruction": "Provide a clear definition and metadata for the requested archetype.",
	"input": ["What is a 'Beastkin' character?"],
	"placeholder_conventions": {
		"purpose": "Provide consistent grounding for archetype definitions without hardcoding names.",
		"allowed_placeholders": ["<archetype_name>", "<Example>", "<Synonym>"],
		"usage_guidelines": "Use placeholders early in examples, then natural language afterwards."
	},
	"output": {
		"archetype": "Beastkin",
		"definition": "A character that retains a mostly human appearance but possesses specific animal features such as ears, tails, claws, or fangs, while maintaining human structure, behavior, and posture. Beastkin are more humanoid in form than Anthropomorphic characters, with animal traits applied selectively rather than defining the entire body structure.",
		"traits": {
			"morphology": "Primarily human. Animal traits limited to appendages such as ears, tails, fur patches, or minimal claws or fangs. Much more humanoid than Anthropomorphic characters.",
			"behavioral_profile": "Human behavior with subtle animal influences (e.g., heightened senses, instincts).",
			"communication": "Primarily human speech.",
			"cognitive_level": "Human-equivalent intelligence."
		},
		"examples": ["Makoto Nanaya (BlazBlue)", "InuYasha", "Kemonomimi characters in anime"],
		"synonyms": ["Kemonomimi", "Nekomimi (subset)", "Human with animal traits"],
		"not_includes": ["Fully anthropomorphic animal bodies", "Quadrupedal animals", "Humanoid/animal hybrids with non-human lower bodies"],
		"notes": ""
	}
}
```

```json
{
	"schema_type": "archetype",
	"instruction": "Provide a clear definition and metadata for the requested archetype.",
	"input": ["What is a 'Tauric' character?"],
	"placeholder_conventions": {
		"purpose": "Provide consistent grounding for archetype definitions without hardcoding names.",
		"allowed_placeholders": ["<archetype_name>", "<Example>", "<Synonym>"],
		"usage_guidelines": "Use placeholders early in examples, then natural language afterwards."
	},
	"output": {
		"archetype": "Tauric",
		"definition": "A hybrid character with a humanoid upper body integrated directly into an animal-like lower body such as a centaur, lamia, or mermaid.",
		"traits": {
			"morphology": "Humanoid torso fused onto an animal body or tail; the lower body is non-human and typically quadrupedal, serpentine, or piscine.",
			"behavioral_profile": "Often exhibits human culture and reasoning, but movement and environmental interaction depend on the creature's lower-body species.",
			"communication": "Typically has human speech.",
			"cognitive_level": "Usually human-level or species-dependent."
		},
		"examples": ["Centaurs (classical mythology)", "Lamia and Naga", "Mermaids and Mermen"],
		"synonyms": ["Hybrid-form", "Humanoid-hybrid", "Half-beast upper-body humanoids"],
		"not_includes": ["Beastkin with small animal traits", "Anthropomorphic animals walking upright", "Fully animalistic quadrupeds"],
		"notes": ""
	}
}
```

```json
{
	"schema_type": "archetype",
	"instruction": "Provide a clear definition and metadata for the requested archetype.",
	"input": ["What is an 'Animalistic' character?"],
	"placeholder_conventions": {
		"purpose": "Provide consistent grounding for archetype definitions without hardcoding names.",
		"allowed_placeholders": ["<archetype_name>", "<Example>", "<Synonym>"],
		"usage_guidelines": "Use placeholders early in examples, then natural language afterwards."
	},
	"output": {
		"archetype": "Animalistic",
		"definition": "A creature that behaves and appears primarily as a non-human animal. These beings typically walk on all fours and lack human speech, posture, and complex reasoning.",
		"traits": {
			"morphology": "Fully animal body plan, real or fantastical; quadrupedal, serpentine, avian, etc.",
			"behavioral_profile": "Acts according to instinctual or species-specific behavior, not human social norms.",
			"communication": "Animal sounds or magical/nonverbal communication, but not human speech.",
			"cognitive_level": "Varies from simple animal instinct to mythical creature intelligence, but not human-like reasoning."
		},
		"examples": ["Wolves, dogs, horses", "Dragons and griffins", "Snakes and lizards"],
		"synonyms": ["Non-sapient animal", "Bestial creature", "Wild-type creature"],
		"not_includes": ["Characters with any human posture or speech", "Humanoid hybrids such as centaurs", "Human bodies with animal appendages"],
		"notes": ""
	}
}
```