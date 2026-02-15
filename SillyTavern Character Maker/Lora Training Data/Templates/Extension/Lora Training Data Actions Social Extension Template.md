remove starting \`\`\`json and ending \`\`\` before pasting it into the Action template.

```json
		"social_extension": {
			"schema_type": "social_extension",

			"social_context": {
				"interaction_type": "<cooperative | persuasive | confrontational | deceptive | supportive | neutral | other (specify)>",
				"formality_level": "<casual | polite | formal | ceremonial | other (specify)>",
				"setting": [
					"<private>",
					"<public>",
					"<institutional>",
					"<intimate>"
					// (examples — select or extend as appropriate)
				]
			},

			"social_roles": {
				"actor_role": "<speaker | initiator | negotiator | authority | subordinate | peer | other (specify)>",
				"target_role": "<listener | respondent | audience | authority | subordinate | peer | other (specify)>",
				"role_notes": "<clarify role dynamics, power balance, or role shifts during the interaction>"
			},

			"intent_and_strategy": {
				"primary_intent": [
					"<inform>",
					"<persuade>",
					"<request>",
					"<command>",
					"<comfort>",
					"<deceive>"
					// (examples — select or extend as appropriate)
				],
				"communication_strategy": "<direct | indirect | manipulative | empathetic | assertive | passive | other (specify)>",
				"leverage_used": [
					"<authority>",
					"<emotional appeal>",
					"<logic>",
					"<social pressure>",
					"<deception>"
					// (examples — optional)
				]
			},

			"communication_modality": {
				"channel": "<verbal | nonverbal | written | symbolic | mixed>",
				"delivery_style": "<calm | animated | restrained | aggressive | playful | other (specify)>",
				"nonverbal_cues": [
					"<gesture>",
					"<facial expression>",
					"<posture>",
					"<proximity>"
					// (examples — optional)
				]
			},

			"emotional_dynamics": {
				"actor_emotional_state": [
					"<calm>",
					"<anxious>",
					"<confident>",
					"<angry>",
					"<empathetic>"
					// (examples — select or extend as appropriate)
				],
				"target_emotional_response": [
					"<receptive>",
					"<resistant>",
					"<confused>",
					"<comforted>",
					"<intimidated>"
					// (examples — select or extend as appropriate)
				],
				"emotional_shift": "<none | positive | negative | mixed | other (specify)>"
			},

			"outcomes_and_consequences": {
				"immediate_outcome": "<agreement | refusal | compliance | misunderstanding | escalation | disengagement>",
				"relationship_impact": "<strengthened | weakened | unchanged | strained | altered>",
				"follow_up_expected": [
					"<continued discussion>",
					"<future obligation>",
					"<avoidance>"
					// (examples — optional)
				]
			},

			"social_norms_and_context": {
				"norm_compliance": "<compliant | borderline | violating | other (specify)>",
				"power_dynamics": "<equal | asymmetric | shifting | other (specify)>",
				"context_notes": "<cultural, organizational, or situational norms affecting interpretation>"
			},

			"cultural_or_genre_notes": "<optional notes about genre conventions, etiquette systems, or setting-specific social rules>"
		},
```