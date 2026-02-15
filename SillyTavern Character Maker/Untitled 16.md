# Master Template Instructions

I am designing structured schemas for LoRA training data that will be converted to JSONL and used with Oobabooga.

Your task is to create or update schema templates and schema instances with high structural rigor and consistency.

**Context:**  
These schemas are intended to structure situations that may occur during interactive roleplay between a user and a language model character.  
The templates capture participants, objects, actions, thresholds, phases, and progression so the model can generate consistent, structured responses during roleplay.

────────────────────────  
TASK  
────────────────────────

Proceed with the requested schema type or schema update.  

Remember: schemas will eventually be converted to JSONL. Ensure all domain-specific rules, optional extensions, scenarios, and placeholders are fully explicit and consistent. Do **not** reference LoRA, ML, or training pipelines inside any schema fields.

────────────────────────  
TOP-LEVEL STRUCTURE RULES  
────────────────────────

Every schema template or schema instance MUST include these top-level fields:

- "schema_type" // identifies the domain (e.g., kink, item, action, character, archetype, scale, situation, etc.)
- "instruction" // describes what the schema should generate
- "input" // natural-language query format; should be an array of strings representing multiple phrasings (can include dynamic placeholders like <object_name> and <narrative_text>)
- "output" // the structured schema definition

Optional, domain-dependent meta-spec fields:

- "_global_schema_notes" // schema-level modeling rules and policies
- "placeholder_conventions" // placeholder rules, only when relevant to the domain
- "schema_notes" // meta-notes about schema design (not about ML or training)

────────────────────────  
INPUT STRUCTURE RULES  
────────────────────────

1. Use **placeholders** in the input array to support dynamic triggering from narrative text.
2. `<placeholders>` (e.g., `<object_name>`) should be replaced with concrete values during instantiation.
3. `[ACTOR]` and `[RECEIVER]` remain symbolic and are **never replaced**.
4. Include multiple phrasings per template to catch narrative variations, including verbs, adjectives, and object references.
5. Input examples may include freeform narrative using `<narrative_text>`.

────────────────────────  
PLACEHOLDER RULES  
────────────────────────

1. Use `placeholder_conventions` to define which placeholders exist and how they are used.
2. `<placeholders>`: replaced with concrete domain values, e.g., `<object_name>` → 'dildo', 'fist', 'stick'.
3. `[symbolic_placeholders]`: remain symbolic, e.g., `[ACTOR]`, `[RECEIVER]`.
4. Each placeholder in `allowed_placeholders` should include a usage guideline explaining context and replacement rules.
5. Do not invent placeholder systems beyond the defined rules unless domain requires it.

────────────────────────  
OUTPUT STRUCTURE RULES  
────────────────────────

1. Every `output` object must begin with the following fields in order:

- "id" // unique identifier
- "name" // primary display name
- "category" // high-level domain categories
- "description" // textual description of the domain entity

2. Optional or extended fields follow (participants, objects, phases, thresholds, threshold_model, extensions, addons, metadata, etc.).

3. All **non-top-level elements** (participants, objects, phases, thresholds, threshold_model, etc.) **must include a `notes` field**:

- In **templates**, `notes` should contain a placeholder explanation (`<explanation_of_expected_content>`) describing what is expected in that element.  
- In **instantiated schemas**, `notes` can be left empty or filled with domain-specific details.

4. Top-level `notes` contains **general notes about the situation/schema as a whole**, describing the purpose, domain rules, or any global considerations.

────────────────────────  
CONTENT RULES  
────────────────────────

4. Templates must define required fields, placeholders, and structural patterns explicitly.
5. Avoid vague fields; use structured fields whenever possible.
6. Include bidirectional progression or residual adaptation when applicable.
7. Ensure placeholders in input and output are consistent with `placeholder_conventions`.
8. Document placeholder usage clearly to ensure correct instantiation.

────────────────────────  
META-SCHEMA PRINCIPLES  
────────────────────────

- Domain data belongs in "output".
- Schema modeling rules belong in "_global_schema_notes".
- Placeholder logic belongs in "placeholder_conventions".
- LoRA, ML, or training context must not appear in the schema fields.

────────────────────────  
FORMATTING RULES  
────────────────────────

1. All JSON must use tab (`\t`) indentation.
2. Empty lines between objects or arrays must contain at least one tab.
3. Maintain consistent tab spacing for nested structures.
4. Comments (`//`) in templates must follow the same tab indentation level as the fields they describe.

────────────────────────  
EXAMPLE OUTPUT ORDER  
────────────────────────

```json
{
	"output": {
		"id": "example_id",
		"name": "Example Name",
		"category": ["Example", "Subcategory"],
		"description": "Textual description of the example entity.",
		
		"participants": {
			"actor": {
				"id": "actor_id",
				"name": "[ACTOR]",
				"role": "initiator",
				"description": "<actor_role_description>",
				"notes": "<explanation_of_expected_content>"
			},
			"receiver": {
				"id": "receiver_id",
				"name": "[RECEIVER]",
				"role": "recipient",
				"description": "<receiver_role_description>",
				"notes": "<explanation_of_expected_content>"
			}
		},
		
		"objects": [
			{
				"id": "object_id",
				"name": "<object_name>",
				"type": "<object_type>",
				"description": "<object_description>",
				"notes": "<explanation_of_expected_content>"
			}
		],
		
		"phases": [
			{
				"id": "phase_id",
				"name": "<phase_name>",
				"descriptions": {
					"forward": "<description_forward>",
					"backward": "<description_backward>"
				},
				"order_index": 1,
				"notes": "<explanation_of_expected_content>"
			}
		],
		
		"threshold_model": {
			"type": "<ordered|bidirectional|conditional|network>",
			"order_mode": "<static|dynamic>",
			"description": "<description_of_transition_logic>",
			"notes": "<explanation_of_expected_content>"
		},
		
		"thresholds": [],
		
		"notes": "<Notes about the situation/schema as a whole>"
	}
}
```

────────────────────────  
CANVAS MANAGEMENT RULES  
────────────────────────

- Always create a canvas named `"Master Template Instructions"` for the authoritative template.
    
- Include the makefile-style metadata at the top:
```makefile
MASTER_CANVAS_NAME := Master Template Instructions
MASTER_CANVAS_ID := <real_internal_ID>

```
────────────────────────  
AWAIT FURTHER INSTRUCTIONS  
────────────────────────