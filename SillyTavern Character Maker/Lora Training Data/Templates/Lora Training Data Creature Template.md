```json
{
	"schema_type": "creature_category",

	"instruction": "Provide creature category metadata with full schema, including explicit male and female reproductive structures for each variant and subvariant.",
	"input": "Define the creature category '[CREATURE_CATEGORY]' and its hierarchical variants.",

	"output": {
		"id": "<creature_category_id>",
		"name": "<creature_category_name>",
		"description": "<high-level description of the creature category as a whole>",

		"classification_notes": "Defines biological and structural rules shared by all variants in this category. This schema does NOT define individual creatures.",

		"common_traits": {
			"intelligence_level": "<animalistic | sapient | highly_intelligent | variable>",
			"social_structure": "<solitary | pack_based | hierarchical | variable>",
			"habitat": ["<common habitats>"],
			"diet": "<carnivore | herbivore | omnivore | magical | variable>",
			"lifespan": "<typical lifespan or range>",
			"size_range": "<relative size compared to humans>",
			"movement": "<primary locomotion methods>",
			"notes": ""
		},

		"variants": [
			{
				"variant_id": "<variant_id>",
				"variant_name": "<variant_name>",
				"variant_role": "<primary morphological or taxonomic variant>",
				"description": "<what distinguishes this variant from the base category>",

				"inherits_common_traits": true|false,

				"variant_traits": {
					"body_plan": "<quadrupedal | bipedal | serpentine | hybrid | other>",
					"limb_configuration": "<summary of limbs, wings, tails>",
					"integument": "<scales | fur | feathers | hide | mixed>",
					"locomotion": "<movement differences from base>",
					"notes": ""
				},

				"biology": {
					"respiration": "<lungs | gills | magical | mixed>",
					"reproduction": {
						 "sexes": ["male", "female", "asexual"], // (Array of included sexes. Use ["female"] for female-only, ["male"] for male-only, ["male","female"] for both sexes, ["asexual"] when it have no sex)
				        "birth_type": "oviparous | viviparous | ovoviviparous | asexual | other", // (Type of birth)
						"female": {
							"genital_structure": "<female anatomy description>",
							"notes": ""
						},
						"male": {
							"genital_structure": "<male anatomy description>",
							"notes": ""
						},
						"general_rules": "<shared reproductive traits across sexes>",
						"notes": ""
					},
					"sexual_dimorphism": "<none | mild | strong | variable>",
					"notes": ""
				},

				"subvariants": [
					{
						"subvariant_id": "<subvariant_id>",
						"subvariant_name": "<subvariant_name>",
						"distinguishing_features": "<what differentiates this subvariant>",
						"special_capabilities": ["<abilities or traits>"],
						"biology": {
							"reproduction": {
								"sexes": ["male | female"],
								"birth_type": "oviparous | viviparous | ovoviviparous | asexual | other",
								"female": {
									"genital_structure": "<female anatomy description>",
									"notes": ""
								},
								"male": {
									"genital_structure": "<male anatomy description>",
									"notes": ""
								},
								"general_rules": "<shared reproductive traits across sexes>",
								"notes": ""
							}
						},
						"notes": ""
					}
				]
			}
		],

		"taxonomy_notes": "<how variants and subvariants relate biologically>",
		"usage_notes": "Used to define creature species rules. Individual named creatures should use the Creature Instance / Character schema.",
		"notes": ""
	}
}
```