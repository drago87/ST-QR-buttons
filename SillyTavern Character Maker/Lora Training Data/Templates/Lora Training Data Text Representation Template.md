# Instructions

When creating a `text_representation` schema, you must define semantic roles and assign formatting patterns to them.

Roles describe what the text means.  
Formatting describes how the text looks.

At minimum, define the following roles:

Role: narration  
Role: speech  
Role: thought  

Additional roles may be defined as needed, as long as each role has a unique semantic meaning and formatting pattern.

---

## How to Provide Role Information

When instructed to create a text representation format, provide role definitions using the following structure:

1) List the roles.
2) Assign a formatting style to each role.

### Example Input Format

```
Roles: narration, speech, thought

narration = plain text  
speech = "quoted text"  
thought = *asterisk wrapped*
```

### Meaning of the Example

- narration uses plain text with no wrappers.
- speech uses double quotes.
- thought uses asterisk wrapping.

The model will then convert this information into a structured `text_representation` schema.

---

# Visual Text Wrappers with Usage Characteristics

## 1) Quote-Based Wrappers

### Double quotes  
Pattern: "text"  
Use: Commonly used to represent spoken dialogue or explicit quoted content. Clear and highly readable.

### Typographic quotes  
Pattern: “text”  
Use: Formal or typographic quotation marks, often used in polished prose or published writing.

### Angle quotes  
Pattern: «text»  
Use: Quotation style used in some languages and typographic traditions. Visually distinct from standard quotes.

### Corner quotes (CJK style)  
Pattern: 「text」  
Use: Quotation marks used in East Asian writing systems. Distinct visual style suitable for stylistic or cultural formatting.

Note: In SillyTavern each of these will have the same color as Double quotes
### Single quotes  
Pattern: 'text'  
Use: Alternative quotation style, often used for nested quotes or stylistic variation.

---

## 2) Asterisk-Based Wrappers

### Single asterisk  
Pattern: *text*  
Use: Commonly used to represent emphasis, actions, thoughts, or stylistic highlighting in chat and roleplay.

### Double asterisk  
Pattern: **text**  
Use: Strong emphasis or bold-like highlighting. Visually more prominent than single asterisks.

### Triple asterisk  
Pattern: ***text***  
Use: Combined strong emphasis and stylistic distinction. Often used to indicate heightened intensity or importance.

---

## 3) Underscore-Based Wrappers

### Single underscore  
Pattern: _text_  
Use: Alternative emphasis style, often interpreted similarly to italic formatting.

### Double underscore  
Pattern: __text__  
Use: Strong emphasis or underline-like visual effect depending on interpretation context.

---

## 4) Tilde-Based Wrappers

### Single tilde  
Pattern: ~text~  
Use: Informal stylistic emphasis or tonal nuance, sometimes associated with playful or expressive tone.

### Double tilde  
Pattern: ~~text~~  
Use: Commonly interpreted as strikethrough or negation in markdown-like systems.

---

## 5) Backtick-Based Wrappers

### Single backtick  
Pattern: `text`  
Use: Inline code-like formatting or technical emphasis. Visually distinguishes text as literal or exact.

### Double backtick  
Pattern: ``text``  
Use: Alternative inline code wrapper when single backticks conflict with content.

Note: In SillyTavern these will look the same.
### Triple backtick (inline wrapper conceptually)  
Pattern:

```<FORMAT_TYPE>
text
```

- `<FORMAT_TYPE>` specifies the content type inside the triple backticks.
    
- Common values include:
    
    - `code` (default, for programming or structured content)
        
    - `text` (general plain text)
        
    - `json` (JSON objects or arrays)
        
    - `yaml` (YAML content)
        
    - `html` (HTML markup)
        
    - `xml` (XML content)
        
    - `bash` (shell commands)
        
    - `python` / `javascript` / other language names (specific programming languages)
        

Example:

```json
{
	"example_key": "example_value"
}
```

This format allows the triple backtick wrapper to indicate the type of content inside for clarity and syntax highlighting.

Note: In SillyTavern these can be used for status boxes, signs, letters etc..

---

## 6) Bracket-Based Wrappers

### Parentheses  
Pattern: (text)  
Use: Supplemental or secondary information, internal thoughts, or contextual aside.

### Square brackets  
Pattern: [text]  
Use: Meta-information, annotations, or system-like contextual markers.

### Double square brackets  
Pattern: [[text]]  
Use: Strongly separated meta-content or layered annotation. Visually more distinct than single brackets.

### Curly braces  
Pattern: {text}  
Use: Abstract, structural, or symbolic grouping. Often perceived as technical or system-oriented.

Note: In SillyTavern these will only show as-is. However, some models may place more emphasis on text inside square brackets.

### Angle brackets  
Pattern: \<text>  (the \ is only there to make sure it don't break formatting)
Use: Visually reduces prominence or hides content in some contexts. Suitable for representing suppressed, abstracted, or non-user-facing information.

Note: In SillyTavern this will be hidden from the user.

---

```json
<template goes here>
```
---

```json
{
	"schema_type": "text_representation",
	"_global_schema_notes": {
		"domain_scope": "Defines how semantic roles in text (e.g., narration, speech, thought, action) are mapped to formatting styles for chat or narrative representation.",
		"extensibility_policy": "New roles may be added as needed. Existing roles must retain unique identifiers.",
		"role_uniqueness_rule": "Each role must have a unique 'id' and distinct formatting pattern.",
		"format_consistency_rule": "Each role must define one primary formatting pattern. Optional variations may be added in the 'examples' field.",
		"semantic_separation_rule": "Roles describe meaning; formats describe visual representation."
	},
	"placeholder_conventions": {
		"purpose": "Defines variable role names, formatting patterns, and example content in the template.",
		"allowed_placeholders": [
			"<ROLE_ID>",
			"<ROLE_LABEL>",
			"<ROLE_DESCRIPTION>",
			"<FORMAT_TYPE>",
			"<FORMAT_PATTERN>",
			"<EXAMPLE_TEXT>",
			"<SYSTEM_NAME>",
			"<SYSTEM_DESCRIPTION>"
		],
		"usage_guidelines": {
			"<ROLE_ID>": "Unique machine-readable identifier for a role.",
			"<ROLE_LABEL>": "Human-readable name of the role.",
			"<ROLE_DESCRIPTION>": "Short textual description of the role's semantic meaning.",
			"<FORMAT_TYPE>": "Category of formatting (plain_text, quoted, wrapped, custom, etc.).",
			"<FORMAT_PATTERN>": "Concrete formatting syntax applied to the role's text.",
			"<EXAMPLE_TEXT>": "Sample text illustrating the role.",
			"<SYSTEM_NAME>": "Display name for the text representation system.",
			"<SYSTEM_DESCRIPTION>": "Textual description of the system purpose."
		}
	},
	"instruction": "Generate a structured specification mapping semantic text roles to formatting conventions for chat or narrative representation.",
	"input": [
		// Example user queries that trigger this text representation template
		"Use <name> Format",
		"Use <name> Style",
		"Apply <name> Format to the chat text",
		"Format chat text in <name> Style"
	],
	"output": {
		"id": "<SYSTEM_ID>", // Unique identifier for this text representation system
		"name": "<SYSTEM_NAME>", // Human-readable name
		"category": ["Text Representation", "Chat Formatting"], // High-level classification
		"description": "<SYSTEM_DESCRIPTION>", // Purpose of the system
		
		"roles": [
			{
				"id": "<ROLE_ID>", // Unique identifier for the role
				"label": "<ROLE_LABEL>", // Human-readable role name
				"description": "<ROLE_DESCRIPTION>", // What this role represents
				"formatting": {
					"type": "<FORMAT_TYPE>", // plain_text, quoted, wrapped, custom, etc.
					"pattern": "<FORMAT_PATTERN>", // e.g., "*<text>*", "\"<text>\"", "<text>"
					"constraints": {
						"allows_nesting": true, // Can this role's formatting be nested inside another?
						"preserves_whitespace": true, // Does it preserve whitespace exactly?
						"requires_delimiters": true // Are explicit delimiters required for clarity?
					}
				},
				"examples": [
					{
						"raw_text": "<EXAMPLE_TEXT>", // Example of user content
						"formatted_text": "<FORMAT_PATTERN>" // How it should appear visually
					}
				]
			} 
		], 
		"default_behavior": {
			"fallback_role": "<ROLE_ID>", // Role to use if a role is not defined
			"unknown_role_policy": "treat_as_fallback" // How unknown roles are handled
		},
		"notes": "All text formatting must be applied consistently per role. Unknown roles default to the fallback role."
	}
}
```