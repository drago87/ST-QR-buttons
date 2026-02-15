```json
{
	"schema_type": "anatomical_size",

	"instruction": "Define an anatomical size descriptor.",
	"input": ["What does '<size_name>' mean for breast size?"],

	"output": {
		"id": "anatomy_breasts",
		"name": "Breasts",

		"scope": "All size ranges reflect adult anatomy unless otherwise specified.",

		"synonyms": [
			"breasts",
			"boobs",
			"chest",
			"bust"
		],

		"description": "The mammary region of the torso. Used in the size system to determine visual fullness, projection, mass, and movement characteristics.",

		"scale_ordering": "ascending",

		"size_scale": [
			{
				"id": "breast_size_flat",
				"name": "Flat",
				"synonyms": [
					"flat chest",
					"no breasts",
					"minimal chest"
				],
				"description": "Minimal or no visible projection of breast tissue. The chest profile remains largely aligned with the ribcage surface.",
				"relative_scale": {
					"symbolic": "lowest"
				},
				"movement_profile": "No independent movement. The chest remains aligned with the ribcage with no bounce or sway under normal physical activity.",
				"scale_order": 0,
				"notes": "Used to describe undeveloped or very small breast volume. Clothing fit is largely unaffected by breast mass."
			},
			{
				"id": "breast_size_small",
				"name": "Small",
				"synonyms": [
					"small breasts",
					"petite chest"
				],
				"description": "Slight projection of breast tissue with modest volume. The chest shows definition but remains compact.",
				"relative_scale": {
					"symbolic": "below_average"
				},
				"movement_profile": "Minimal independent movement. The chest remains mostly stable with little to no bounce; slight deformation may occur under motion.",
				"scale_order": 1,
				"notes": "Does not significantly alter posture or require high-support garments."
			},
			{
				"id": "breast_size_medium",
				"name": "Medium",
				"synonyms": [
					"average breasts",
					"moderate chest"
				],
				"description": "Balanced, proportionate breast volume with moderate projection. Commonly treated as an average reference size.",
				"relative_scale": {
					"symbolic": "average"
				},
				"movement_profile": "Noticeable but controlled movement. Light vertical bounce and subtle lateral sway closely following torso motion.",
				"scale_order": 2,
				"notes": "Often used as a reference point for clothing and armor sizing."
			},
			{
				"id": "breast_size_large",
				"name": "Large",
				"synonyms": [
					"large breasts",
					"big bust"
				],
				"description": "Fuller breast volume with substantial projection and visible curvature relative to the torso.",
				"relative_scale": {
					"symbolic": "above_average"
				},
				"movement_profile": "Pronounced movement with visible bounce and sway. Motion exhibits mild inertia and delayed settling after torso movement.",
				"scale_order": 3,
				"notes": "Often benefits from supportive garments for comfort during extended activity."
			},
			{
				"id": "breast_size_huge",
				"name": "Huge",
				"synonyms": [
					"huge breasts",
					"massive bust"
				],
				"description": "Significantly above-average breast volume with high projection and mass. Chest curvature is highly pronounced.",
				"relative_scale": {
					"symbolic": "highest_common_scale"
				},
				"movement_profile": "High-mass movement with strong inertia. Exhibits pronounced multi-directional bounce and delayed motion even after the torso stops.",
				"scale_order": 4,
				"notes": "May require specialized clothing support. Sagging may occur without adequate support."
			}
		]
	}
}
```
```json
{
	"schema_type": "anatomical_size",

	"instruction": "Define an anatomical size descriptor.",
	"input": ["What does '<size_name>' mean for penis size?"],

	"output": {
		"id": "anatomy_penis",
		"name": "Penis",

		"scope": "All size ranges reflect adult anatomy unless otherwise specified. For children scale it to 0.3.",

		"synonyms": [
			"penis",
			"dick",
			"cock",
			"member",
			"shaft"
		],

		"description": "External male genital organ used for reproduction and urination. The size system categorizes visible length, girth, and perceived prominence across common states.",

		"scale_ordering": "ascending",

		"size_scale": [
			{
				"id": "penis_size_small",
				"name": "Small",
				"synonyms": [
					"small penis",
					"small dick",
					"short dick"
				],
				"description": "Below-average size in visible length and/or girth. Often described as compact or reduced in prominence.",
				"relative_scale": {
					"flaccid": {
						"approx_length_cm": "2–6",
						"approx_length_in": "0.8–2.4",
						"girth_characterization": ["slim", "finger-width"],
						"movement_profile": "Loose, gravity-driven movement with light swing and minimal inertia. Motion follows body movement and settles naturally."
					},
					"erect": {
						"approx_length_cm": "4–9",
						"approx_length_in": "1.5–3.5",
						"girth_characterization": ["slim to narrow", "adult finger"],
						"movement_profile": "Rigid with minimal swing. When displaced, it flexes slightly and springs back into position rather than oscillating freely."
					}
				},
				"scale_order": 0,
				"notes": "Natural-language cues include 'small', 'short', 'tiny', or descriptions emphasizing reduced size. Can have problem with peeling the foreskin back."
			},
			{
				"id": "penis_size_medium",
				"name": "Medium",
				"synonyms": [
					"average penis",
					"normal dick"
				],
				"description": "The central size range in population distribution, neither notably reduced nor notably enlarged.",
				"relative_scale": {
					"flaccid": {
						"approx_length_cm": "6–10",
						"approx_length_in": "2.4–4",
						"girth_characterization": ["average", "adult thumb"],
						"movement_profile": "Moderate pendular movement when unsupported. Motion has mild inertia and settles gently after shifts in stance."
					},
					"erect": {
						"approx_length_cm": "10–15",
						"approx_length_in": "4–6",
						"girth_characterization": ["average", "adult thumb"],
						"movement_profile": "Firm with reduced swing. When deflected, it springs back toward alignment rather than continuing to sway."
					}
				},
				"scale_order": 1,
				"notes": "Usually implied when no explicit size description is present in the source text."
			},
			{
				"id": "penis_size_large",
				"name": "Large",
				"synonyms": [
					"large penis",
					"big dick",
					"big cock"
				],
				"description": "A penis size above the common population range in either length or girth.",
				"relative_scale": {
					"flaccid": {
						"approx_length_cm": "10+",
						"approx_length_in": "4+",
						"girth_characterization": ["full", "two adult fingers"],
						"movement_profile": "Pronounced, weighty movement with visible swing and inertia. Settles slowly after motion stops."
					},
					"erect": {
						"approx_length_cm": "16+",
						"approx_length_in": "6.5+",
						"girth_characterization": ["thick to full", "two adult fingers"],
						"movement_profile": "Rigid and prominent with minimal swing. When deflected, it snaps or springs back toward its aligned position rather than oscillating."
					}
				},
				"scale_order": 2,
				"notes": "Textual cues include 'large', 'big', 'well-endowed', or other descriptors indicating conspicuous size. The foreskin often retracts naturally when erect."
			}
		]
	}
}
```