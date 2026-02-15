```json
{
	"instruction": "Define clothing category metadata with full schema.",
	"input": ["What is one_piece?"],
	"output": {
		"id": "one_piece",
		"name": "One-Piece Garment",
		"description": "A single integrated garment that covers the torso and may also extend to the legs or arms. It is worn as a unified piece rather than as separate upper and lower components.",
		"typical_items": [
			"robes",
			"dresses",
			"jumpsuits",
			"unitards",
			"onesies",
			"single-piece kimono"
		],
		"placement_rules": "Must cover the torso. May optionally cover legs or arms. Cannot be separated into top and bottom.",
		"notes": "Mutually exclusive with two-piece categories."
	}
}
```
```json
{
	"instruction": "Define clothing category metadata with full schema.",
	"input": ["What is two_piece_top?"],
	"output": {
		"id": "two_piece_top",
		"name": "Two-Piece: Top",
		"description": "Any upper-body garment worn as part of a two-piece outfit. Covers the torso and sometimes the arms.",
		"typical_items": [
			"shirts",
			"blouses",
			"kimono tops",
			"jackets",
			"tunics"
		],
		"placement_rules": "Covers the torso and may cover the arms. Must be worn separately from two_piece_bottom items.",
		"notes": "Common in layered outfits and uniforms."
	}
}
```
```json
{
	"instruction": "Define clothing category metadata with full schema.",
	"input": ["What is two_piece_bottom?"],
	"output": {
		"id": "two_piece_bottom",
		"name": "Two-Piece: Bottom",
		"description": "Lower-body garments designed to be worn separately from an upper-body piece.",
		"typical_items": [
			"pants",
			"skirts",
			"shorts",
			"hakama",
			"jeans"
		],
		"placement_rules": "Covers the pelvis and legs to some extent. Must be distinct from two_piece_top items.",
		"notes": "Compatible with both modern and traditional clothing systems."
	}
}
```
```json
{
	"instruction": "Define clothing category metadata with full schema.",
	"input": ["What is legwear?"],
	"output": {
		"id": "legwear",
		"name": "Leg Garments",
		"description": "Clothing worn around the legs that is separate from the waist garment itself. May provide coverage, warmth, compression, or support.",
		"typical_items": [
			"compression leggings",
			"greaves sleeves",
			"traditional leg wraps",
			"shin coverings",
			"stockings"
		],
		"placement_rules": "Worn on the legs, starting below the pelvis. May be used together with both one-piece and two-piece outfits.",
		"notes": "If you prefer, this category can include tabi-style leg wrappings, shin guards, or fitted underlayer leggings."
	}
}
```