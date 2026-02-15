```json
{
	"schema_type": "clothing_coverage_length_scale",
	"instruction": "Provide clothing coverage and length metadata with full schema.",
	"input": ["Define the clothing length and coverage scale for Skirts."],
	"output": {
		"id": "skirts_coverage_length_scale",
		"name": "Skirt Coverage & Length Scale",
		"description": "A standardized scale describing skirt length from minimal exposure to long trailing skirts, including movement, coverage, and narrative considerations.",
		"applies_to": ["skirts", "two_piece_bottom"],
		"coverage_axis_definition": {
			"reference_points": ["Hips", "Upper thighs", "Mid-thigh", "Knees", "Calves", "Ankles", "Ground / trailing"],
			"direction": "Ascending scale_index values indicate increased body coverage and length."
		},
		"length_scale": [
			{
				"id": "skirt_minimal",
				"scale_index": 0,
				"name": "Minimal Coverage",
				"synonyms": ["barely-there skirt"],
				"approx_position": "At the hips",
				"coverage_description": "Covers only essential areas; most legs exposed.",
				"movement_behavior": "Extremely mobile; shifts easily with motion.",
				"movement_constraints": "None",
				"common_use": "Erotic or fetish fashion.",
				"narrative_implications": "Overt exposure, vulnerability, exhibitionism.",
				"notes": ""
			},
			{
				"id": "skirt_micro",
				"scale_index": 1,
				"name": "Micro",
				"synonyms": ["micro skirt"],
				"approx_position": "Upper thigh",
				"coverage_description": "Covers hips; upper thighs largely exposed.",
				"movement_behavior": "Rides up easily during motion.",
				"movement_constraints": "Low; requires awareness when sitting.",
				"common_use": "Provocative fashion.",
				"narrative_implications": "Bold, flirtatious, attention-drawing.",
				"notes": ""
			},
			{
				"id": "skirt_mini",
				"scale_index": 2,
				"name": "Mini",
				"synonyms": ["short skirt"],
				"approx_position": "Mid-thigh",
				"coverage_description": "Partial thigh coverage.",
				"movement_behavior": "Light sway during motion.",
				"movement_constraints": "Low",
				"common_use": "Casual or nightlife wear.",
				"narrative_implications": "Youthful, playful.",
				"notes": ""
			},
			{
				"id": "skirt_above_knee",
				"scale_index": 3,
				"name": "Above Knee",
				"synonyms": ["above-knee skirt"],
				"approx_position": "Just above the knee",
				"coverage_description": "Covers most of thighs; knee exposed.",
				"movement_behavior": "Stable sway.",
				"movement_constraints": "Minimal",
				"common_use": "Casual, school uniforms.",
				"narrative_implications": "Appropriate yet youthful.",
				"notes": ""
			},
			{
				"id": "skirt_knee",
				"scale_index": 4,
				"name": "Knee-Length",
				"synonyms": ["knee skirt"],
				"approx_position": "At the knee",
				"coverage_description": "Fully covers thighs and knee.",
				"movement_behavior": "Stable movement.",
				"movement_constraints": "Minimal",
				"common_use": "Professional or everyday wear.",
				"narrative_implications": "Practical, modest.",
				"notes": ""
			},
			{
				"id": "skirt_midi",
				"scale_index": 6,
				"name": "Midi",
				"synonyms": ["mid-calf skirt"],
				"approx_position": "Mid-calf",
				"coverage_description": "Covers legs below the knees.",
				"movement_behavior": "Restrained sway.",
				"movement_constraints": "Moderate stride limitation.",
				"common_use": "Formal or vintage styles.",
				"narrative_implications": "Elegant, reserved.",
				"notes": ""
			},
			{
				"id": "skirt_maxi",
				"scale_index": 8,
				"name": "Maxi",
				"synonyms": ["floor-length skirt"],
				"approx_position": "Ankles or floor",
				"coverage_description": "Full lower-body coverage.",
				"movement_behavior": "Flowing fabric.",
				"movement_constraints": "Noticeable.",
				"common_use": "Formal wear.",
				"narrative_implications": "Graceful, dramatic.",
				"notes": ""
			},
			{
				"id": "skirt_trailing",
				"scale_index": 10,
				"name": "Trailing",
				"synonyms": ["skirt with train"],
				"approx_position": "Dragging behind on the ground",
				"coverage_description": "Excessive coverage with trailing fabric.",
				"movement_behavior": "Fabric drags behind.",
				"movement_constraints": "High",
				"common_use": "Ceremonial or fantasy attire.",
				"narrative_implications": "Status, formality, grandeur.",
				"notes": ""
			}
		]
	}
}
```
```json
{
	"schema_type": "clothing_coverage_length_scale",
	"instruction": "Provide clothing coverage and length metadata with full schema.",
	"input": ["Define the clothing length and coverage scale for] Dresses.",
	"output": {
		"id": "dresses_coverage_length_scale",
		"name": "Dress Coverage & Length Scale",
		"description": "A standardized scale describing dress length from minimal torso coverage to long trailing gowns, including movement, coverage, and narrative context.",
		"applies_to": ["dresses", "one_piece"],
		"coverage_axis_definition": {
			"reference_points": ["Torso", "Hips", "Thighs", "Knees", "Calves", "Ankles", "Ground / trailing"],
			"direction": "Ascending scale_index values indicate increased body coverage and formality."
		},
		"length_scale": [
			{
				"id": "dress_minimal",
				"scale_index": 0,
				"name": "Minimal Coverage",
				"synonyms": ["barely-there dress"],
				"approx_position": "Just below hips",
				"coverage_description": "Covers only essential torso and hip areas.",
				"movement_behavior": "Extremely mobile; fabric moves freely.",
				"movement_constraints": "None",
				"common_use": "Erotic or fetish fashion.",
				"narrative_implications": "Extreme exposure, vulnerability, exhibitionism.",
				"notes": ""
			},
			{
				"id": "dress_mini",
				"scale_index": 2,
				"name": "Mini Dress",
				"synonyms": ["short dress"],
				"approx_position": "Mid-thigh",
				"coverage_description": "Covers torso and upper thighs.",
				"movement_behavior": "Free movement with light sway.",
				"movement_constraints": "Low",
				"common_use": "Casual or party wear.",
				"narrative_implications": "Youthful, flirtatious.",
				"notes": ""
			},
			{
				"id": "dress_above_knee",
				"scale_index": 3,
				"name": "Above Knee Dress",
				"synonyms": ["above-knee dress"],
				"approx_position": "Just above knees",
				"coverage_description": "Torso and most of legs covered; knees visible.",
				"movement_behavior": "Stable with minimal sway.",
				"movement_constraints": "Minimal",
				"common_use": "Casual, school or office settings.",
				"narrative_implications": "Appropriate yet youthful.",
				"notes": ""
			},
			{
				"id": "dress_knee",
				"scale_index": 4,
				"name": "Knee-Length Dress",
				"synonyms": ["day dress"],
				"approx_position": "At knees",
				"coverage_description": "Covers most legs including knees.",
				"movement_behavior": "Stable movement.",
				"movement_constraints": "Minimal",
				"common_use": "Professional, daily wear.",
				"narrative_implications": "Balanced, respectable.",
				"notes": ""
			},
			{
				"id": "dress_midi",
				"scale_index": 6,
				"name": "Midi Dress",
				"synonyms": ["tea-length dress"],
				"approx_position": "Mid-calf",
				"coverage_description": "Extensive leg coverage below knees.",
				"movement_behavior": "Flowing yet controlled.",
				"movement_constraints": "Moderate",
				"common_use": "Formal or semi-formal events.",
				"narrative_implications": "Elegant, composed.",
				"notes": ""
			},
			{
				"id": "dress_maxi",
				"scale_index": 8,
				"name": "Maxi Dress",
				"synonyms": ["floor-length gown"],
				"approx_position": "Ankles or floor",
				"coverage_description": "Full lower-body coverage with long fabric.",
				"movement_behavior": "Flowing fabric with heavier movement.",
				"movement_constraints": "Noticeable",
				"common_use": "Evening wear, ceremonies.",
				"narrative_implications": "Formal, graceful.",
				"notes": ""
			},
			{
				"id": "dress_trailing",
				"scale_index": 10,
				"name": "Trailing Gown",
				"synonyms": ["dress with train"],
				"approx_position": "Trailing on the ground",
				"coverage_description": "Excess fabric extending beyond feet, full-body coverage.",
				"movement_behavior": "Fabric drags behind; movement restricted.",
				"movement_constraints": "High",
				"common_use": "Royal, bridal, ceremonial, fantasy attire.",
				"narrative_implications": "Grandeur, authority, ceremony.",
				"notes": ""
			}
		]
	}
}
```
```json
{
	"schema_type": "clothing_coverage_length_scale",

	"instruction": "Provide clothing coverage and length metadata with full schema.",
	"input": ["Define the clothing length and coverage scale for Thigh-High Socks."],
	"output": {
		"id": "thigh_high_socks_coverage_length_scale",
		"name": "Thigh-High Socks Coverage & Length Scale",
		"description": "A standardized scale describing thigh-high sock length from minimal coverage to full thigh coverage. Focuses on leg coverage, mobility, and aesthetic exposure.",

		"applies_to": [
			"socks",
			"legwear"
		],

		"coverage_axis_definition": {
			"reference_points": [
				"Ankles",
				"Calves",
				"Knees",
				"Mid-thigh",
				"Upper-thigh"
			],
			"direction": "Ascending scale_index values indicate increased coverage and length up the leg."
		},

		"length_scale": [
			{
				"id": "thigh_high_socks_knee",
				"scale_index": 0,
				"name": "Knee-High",
				"synonyms": ["knee socks"],
				"approx_position": "At the knee",
				"coverage_description": "Covers feet and reaches just over the knees.",
				"movement_behavior": "Stable, minimal slippage.",
				"movement_constraints": "None.",
				"common_use": "School uniforms, casual wear.",
				"narrative_implications": "Practical, modest.",
				"notes": ""
			},
			{
				"id": "thigh_high_socks_mid_thigh",
				"scale_index": 1,
				"name": "Mid-Thigh",
				"synonyms": ["mid-thigh socks"],
				"approx_position": "Mid-thigh",
				"coverage_description": "Covers legs from feet to middle of the thigh.",
				"movement_behavior": "May require adjustment to prevent sliding.",
				"movement_constraints": "Slight; can roll down if worn loosely.",
				"common_use": "Fashionable or playful outfits.",
				"narrative_implications": "Slightly provocative, stylish.",
				"notes": ""
			},
			{
				"id": "thigh_high_socks_upper_thigh",
				"scale_index": 2,
				"name": "Upper-Thigh",
				"synonyms": ["over-the-knee socks", "thigh-high socks"],
				"approx_position": "Upper-thigh",
				"coverage_description": "Extends from feet to the upper thighs.",
				"movement_behavior": "May require elastic or garter support; prone to sliding.",
				"movement_constraints": "Moderate; tight fit may restrict movement slightly.",
				"common_use": "Fashion, cosplay, intimate or stylish outfits.",
				"narrative_implications": "Bold, playful, or sensual aesthetic.",
				"notes": ""
			}
		]
	}
}
```