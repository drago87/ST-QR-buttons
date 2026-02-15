```json
{
	"schema_type": "item", // Required: identifies this schema as an item definition
	
	"instruction": "Provide item metadata with full schema.",
	"input": "What is <item_name>?", // The natural-language query or prompt
	
	"output": {
		"id": "<item_id>", // Unique identifier for the item
		"name": "<item_name>", // Primary display name
		"synonyms": ["<synonyms>"], // Alternative names or terms
		"category": ["<category_from_list>"], // High-level categories (e.g. tool, weapon, clothing, device)
		"type": "<type_of_item_or_subcategory>", // More specific classification
		
		"short_description": "<describe_shortly_what_the_item_looks_like>", // One-sentence visual summary
		"indepth_description": "<describe_indepth_what_the_item_looks_like>", // Detailed physical description
		
		"intended_use": "<why_and_where_it_is_used>", // Purpose and typical context
		"operation": "<how_to_use_or_apply_item>", // How the item is operated or handled
		"materials": "<common_materials>", // Typical materials
		"safety_notes": "<safety_or_warnings>", // Risks, constraints, or warnings
		
		"variants": [
			{
				"name": "<variant_name>", // Optional: size, model, version, etc.
				"description": "<what makes this variant different>",
				"notes": ""
			}
		],
		
		"addons": [
			{
				"addon_id": "<addon_item_id>", // Reference to a separate Item schema (if applicable)
				"name": "<addon_name>", // Display name of the addon
				"addon_type": "<integrated | modular | optional | upgrade | consumable | cosmetic>", // Functional role of the addon
				"relationship": "<built_in | attachable | detachable | external>", // How it connects to the base item
				"description": "<what this addon does or changes>",
				"compatibility_notes": "<constraints or requirements>", // Compatibility rules or dependencies
				"notes": ""
			}
		],
		
		"requirements": {
			// Defines constraints or prerequisites for using the item. Values are illustrative, not exhaustive.
			"power_source": "<none | battery | magic | external | other>",
			"user_capabilities": ["<strength>", "<skill>", "<magic>"] ,
			// user_capabilities is an open-ended list; the examples above are not the only possible capabilities.
			"notes": ""
		},
		
		"metadata": { {
			"size": "<size_information>", // Dimensions or scale
			"weight": "<weight_information>", // Weight or mass
			"color_options": ["<available_colors>"], // Possible color variants
			"compatibility": ["<compatibility_notes_or_restrictions>"] // Systems, items, or contexts it works with
		},
		
		"notes": "" // Optional additional clarifications
	}
}
```