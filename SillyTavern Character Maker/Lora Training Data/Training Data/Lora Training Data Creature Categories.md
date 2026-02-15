```json
{
  "schema_type": "creature_category",
  "instruction": "Provide creature category metadata with full schema, including explicit male and female reproductive structures for each variant and subvariant.",
  "input": ["Define the creature category 'Dragons' and its hierarchical variants."],
  "output": {
    "id": "dragon_category",
    "name": "Dragons",
    "description": "Dragons are large, sentient, reptilian creatures with wings, tails, and varying numbers of limbs. Western dragons are typically quadrupedal with wings; Eastern dragons are serpentine with forelimbs but no wings.",
    "classification_notes": "Dragons are defined by their shared reptilian physiology, winged or serpentine morphology, and internal reproductive system. This schema does NOT define individual dragons.",
    "common_traits": {
      "intelligence_level": "highly_intelligent",
      "social_structure": "variable",
      "habitat": ["mountains", "forests", "caves", "magical zones"],
      "diet": "carnivore | magical | variable",
      "lifespan": "centuries",
      "size_range": "Much larger than humans, variable by species",
      "movement": "Walking, flying (depending on wing structure)",
      "notes": ""
    },
    "variants": [
      {
        "variant_id": "western_dragon",
        "variant_name": "Dragon",
        "variant_role": "Western Dragon",
        "description": "Large, quadrupedal dragons with two wings and scale-covered bodies, capable of breath weapons.",
        "inherits_common_traits": true,
        "variant_traits": {
          "body_plan": "quadrupedal",
          "limb_configuration": "Four legs, two wings; tail present",
          "integument": "Scales",
          "locomotion": "Primarily walks on all four limbs; wings for flying or gliding",
          "notes": ""
        },
        "biology": {
          "respiration": "lungs",
          "reproduction": {
            "sexes": ["male", "female"],
            "birth_type": "oviparous",
            "female": {
              "genital_structure": "<female anatomy description>",
              "notes": ""
            },
            "male": {
              "genital_structure": "<male anatomy description>",
              "notes": ""
            },
            "general_rules": "Reproduction via eggs; reproductive structures consistent across Western Dragon subtypes unless modified by species-specific biology.",
            "notes": ""
          },
          "sexual_dimorphism": "strong",
          "anatomy_rule": "Western dragons always have four legs, two wings, and one tail. Limbs, wings, and tail must follow standard dragon morphology; do not describe additional limbs or remove wings unless defining a subvariant.",
          "notes": ""
        },
        "subvariants": [
          {
            "subvariant_id": "metallic_dragon",
            "subvariant_name": "Metallic Dragon",
            "distinguishing_features": "Scales visually resemble metals; coloration varies by species",
            "special_capabilities": ["metallic magical affinity"],
            "biology": {
              "reproduction": {
                "sexes": ["male", "female"],
                "birth_type": "oviparous",
                "female": {
                  "genital_structure": "<female anatomy description>",
                  "notes": ""
                },
                "male": {
                  "genital_structure": "<male anatomy description>",
                  "notes": ""
                },
                "general_rules": "Follows Western Dragon norms; metallic scales do not alter reproductive structures.",
                "notes": ""
              }
            },
            "anatomy_rule": "Follows all Western Dragon morphology rules; metallic scales are cosmetic and do not change limb or tail count.",
            "notes": ""
          },
          {
            "subvariant_id": "elemental_dragon",
            "subvariant_name": "Elemental Dragon",
            "distinguishing_features": "Adapted to elemental affinity (fire, water, air, earth, lightning, ice)",
            "special_capabilities": ["elemental breath weapon"],
            "biology": {
              "reproduction": {
                "sexes": ["male", "female"],
                "birth_type": "oviparous",
                "female": {
                  "genital_structure": "<female anatomy description>",
                  "notes": ""
                },
                "male": {
                  "genital_structure": "<male anatomy description>",
                  "notes": ""
                },
                "general_rules": "Elemental traits do not modify primary reproductive anatomy.",
                "notes": ""
              }
            },
            "anatomy_rule": "Follows all Western Dragon morphology rules; elemental modifications do not change number or placement of limbs or wings.",
            "notes": ""
          },
          {
            "subvariant_id": "furred_dragon",
            "subvariant_name": "Furred Dragon",
            "distinguishing_features": "Covered in fur instead of scales; smaller, more mammalian",
            "special_capabilities": ["reduced breath weapon potency"],
            "biology": {
              "reproduction": {
                "sexes": ["male", "female"],
                "birth_type": "viviparous",
                "female": {
                  "genital_structure": "<female anatomy description>",
                  "notes": ""
                },
                "male": {
                  "genital_structure": "<male anatomy description>",
                  "notes": ""
                },
                "general_rules": "Mammalian reproductive anatomy; live birth instead of eggs.",
                "notes": ""
              }
            },
            "anatomy_rule": "Quadrupedal with wings retained; tail present; no legs removed or extra limbs added; fur replaces scales only.",
            "notes": ""
          }
        ]
      },
      {
        "variant_id": "eastern_dragon",
        "variant_name": "Dragon",
        "variant_role": "Eastern Dragon",
        "description": "Long, serpentine dragons with forelimbs, no wings, capable of levitation or air-swimming.",
        "inherits_common_traits": true,
        "variant_traits": {
          "body_plan": "serpentine",
          "limb_configuration": "Two forelimbs; no hind limbs; tail present",
          "integument": "Fine scales",
          "locomotion": "Air-swimming; levitation possible",
          "notes": ""
        },
        "biology": {
          "respiration": "lungs",
          "reproduction": {
            "sexes": ["male", "female"],
            "birth_type": "oviparous",
            "female": {
              "genital_structure": "<female anatomy description>",
              "notes": ""
            },
            "male": {
              "genital_structure": "<male anatomy description>",
              "notes": ""
            },
            "general_rules": "Reproduction via eggs; serpentine morphology maintained.",
            "notes": ""
          },
          "sexual_dimorphism": "variable",
          "anatomy_rule": "Eastern dragons have serpentine bodies with two forelimbs and one tail; no wings or hind limbs unless defining a rare subvariant.",
          "notes": ""
        },
        "subvariants": []
      }
    ],
    "taxonomy_notes": "Variants and subvariants differ by scale type, elemental affinity, fur coverage, and limb/wings morphology, but all follow their respective anatomy rules.",
    "usage_notes": "Defines species rules; individual named dragons should use Creature Instance / Character schema.",
    "notes": ""
  }
}
```
```json
{
	"schema_type": "creature_category",
	"instruction": "Provide creature category metadata with full schema, including explicit male and female reproductive structures for each variant and subvariant.",
	"input": ["Define the creature category 'Mermaids' and its hierarchical variants."],
	"output": {
		"id": "mermaid_category",
		"name": "Mermaids",
		"description": "Mermaids are sapient aquatic humanoids with a human upper body and a serpentine, fish-like tail instead of legs. They are adapted for underwater life while retaining humanoid intelligence and dexterity.",
		"classification_notes": "Mermaids are defined by their humanoid upper body, serpentine tail, internal mammalian-like reproductive system, and fully aquatic locomotion. This schema does NOT define individual mermaids.",
		"common_traits": {
			"intelligence_level": "sapient",
			"social_structure": "Small pods, coastal communities, or solitary individuals",
			"habitat": ["Oceans", "Seas", "Reefs", "Deep waters"],
			"diet": "primarily piscivorous with omnivorous flexibility",
			"lifespan": "Comparable to or slightly longer than humans",
			"size_range": "Human upper-body proportions with a long, powerful tail",
			"movement": "Swimming via tail propulsion; no terrestrial locomotion",
			"notes": "Mermaids rely entirely on their tails for locomotion and physical interaction. Legs are completely absent."
		},
		"variants": [
			{
				"variant_id": "female_mermaid",
				"variant_name": "Mermaid",
				"variant_role": "Female aquatic humanoid",
				"description": "A female mermaid with a humanoid upper body and a scaled, serpentine tail; internal reproductive anatomy follows mammalian principles.",
				"inherits_common_traits": true,
				"variant_traits": {
					"body_plan": "Humanoid upper body with a single continuous tail",
					"limb_configuration": "Two humanoid arms; no hind limbs; tail for all lower-body function",
					"integument": "Fine overlapping fish scales covering tail; humanoid skin texture on torso",
					"locomotion": "Propulsion via tail; fully aquatic",
					"head_features": ["Humanoid facial structure", "Aquatic-adapted ears"],
					"notes": "No legs, knees, feet, or thighs. Tail is sole means of movement and interaction."
				},
				"biology": {
					"respiration": "Lungs adapted for extended breath-holding",
					"reproduction": {
						"sexes": ["female"],
						"birth_type": "viviparous",
						"female": {
							"genital_structure": "Internal female reproductive anatomy based on marine mammal biology; minimal external expression, integrated into ventral slit.",
							"notes": "Designed to minimize drag while preserving humanoid torso form."
						},
						"general_rules": "Sexual reproduction with internal fertilization; lower-body anatomy is tail-only.",
						"notes": ""
					},
					"sexual_dimorphism": "Subtle differences in torso shape and tail mass",
					"anatomy_rule": "NEVER describe mermaids having legs, feet, knees, thighs, or any lower human anatomy below the hips. Mermaids have fish tails instead of legs. ALWAYS emphasize the fish tail for locomotion, restraint, balance, and physical interaction instead of legs.",
					"notes": ""
				},
				"subvariants": []
			},
			{
				"variant_id": "male_mermaid",
				"variant_name": "Merman",
				"variant_role": "Male aquatic humanoid",
				"description": "A male mermaid variant with humanoid upper body and fully serpentine aquatic tail; adapted for internal mammalian reproductive biology and aquatic locomotion.",
				"inherits_common_traits": true,
				"variant_traits": {
					"body_plan": "Humanoid upper body with continuous serpentine tail",
					"limb_configuration": "Two humanoid arms; no hind limbs; tail for all lower-body function",
					"integument": "Fine overlapping fish scales covering tail; humanoid skin texture on torso",
					"locomotion": "Propulsion via tail; fully aquatic",
					"head_features": ["Humanoid facial structure", "Aquatic-adapted ears"],
					"notes": "Tail is sole means of locomotion; no legs present."
				},
				"biology": {
					"respiration": "Lungs adapted for extended breath-holding",
					"reproduction": {
						"sexes": ["male"],
						"birth_type": "viviparous",
						"male": {
							"genital_structure": "Internal male reproductive anatomy inside ventral slit; streamlined and hydrodynamic.",
							"notes": ""
						},
						"general_rules": "Sexual reproduction with live birth; tail replaces all lower limbs.",
						"notes": ""
					},
					"sexual_dimorphism": "Greater upper-body musculature and tail mass compared to females",
					"anatomy_rule": "NEVER describe mermen having legs, feet, knees, thighs, or any lower human anatomy below the hips. Mermen have fish tails instead of legs. ALWAYS emphasize the fish tail for locomotion, restraint, balance, and physical interaction instead of legs.",
					"notes": ""
				},
				"subvariants": []
			}
		],
		"taxonomy_notes": "Mermaids are aquatic humanoids with a humanoid torso and a serpentine tail; female and male variants share tail-based locomotion but retain only sex-specific reproductive anatomy.",
		"usage_notes": "Enforces tail-only lower-body anatomy; no leg-based descriptions are permitted for any mermaid variants.",
		"notes": ""
	}
}
```