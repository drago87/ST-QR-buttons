```json
{
	"schema_type": "kink",
	
	"instruction": "Provide kink metadata with full schema, including extensions for sexual, social, and combat contexts.",
	"input": ["What is Bondage?"],
	
	"output": {
		"id": "bondage",
		"name": "Bondage",
		"synonyms": ["restraint play", "ties", "rope play"],
		"category": ["Kink", "Restraint", "Control"],
		"description": "A consensual activity involving the temporary restriction of movement for structured power exchange, sensory control, roleplay, or erotic stimulation.",
		
		"ontology": {
			"core_dynamic": "physical",
			"primary_focus": "control",
			"secondary_focus": "sensory",
			"subtype": "rope | cuffs | positional",
			"consent_structure": "consensual_by_default",
			"notes": "Bondage emphasizes voluntary submission, restraint methods, and safety measures such as safewords and release mechanisms."
		},
		
		"roles": {
			"giver": {
				"id": "bondage_giver_role",
				"label": "Giver",
				"definition": "The participant who applies, manages, or maintains the restraints.",
				"typical_awareness": "aware",
				"notes": "Responsible for safety, monitoring the receiver, and adhering to negotiated limits."
			},
			"receiver": {
				"id": "bondage_receiver_role",
				"label": "Receiver",
				"definition": "The participant who is restrained or experiences limited mobility.",
				"typical_awareness": "aware",
				"notes": "May experience psychological, sensory, or sexual stimulation from restraint."
			}
		},
		
		"consent_and_safety": {
			"typical_consent_state": "consensual",
			"consent_requirement": "required",
			"negotiation_norm": "expected",
			"physical_risk": "moderate",
			"psychological_risk": "low",
			"legal_risk": "low",
			
			"default_scenario": {
				"description": "One participant restrains another within negotiated limits using ropes, cuffs, or other implements.",
				"consent_present": "true",
				"risk_level": "moderate",
				"notes": "Clear communication, safewords, and quick release mechanisms are standard practice."
			},
			
			"scenario_overrides": [
				{
					"scenario": "Casual, light restraint during foreplay or erotic play.",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "low",
					"notes": "Low-risk application with minimal physical or psychological strain."
				},
				{
					"scenario": "Advanced rope or positional bondage with prolonged restraint.",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "Requires advanced skills, monitoring, and risk mitigation due to circulation or nerve compression hazards."
				},
				{
					"scenario": "Self-bondage performed by the receiver alone.",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "The participant must plan for safe release and avoid positions that could cause injury; no external supervision is present."
				},
				{
					"scenario": "Forced bondage applied by a partner without prior consent.",
					"consent_present": "false",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "This scenario is non-consensual and carries ethical, legal, and physical risks; should not be practiced outside of explicit negotiation or fantasy roleplay."
				}
			],
			
			"notes": "Safety, communication, and consent are central. Scenarios vary in intensity and supervision requirements."
		},
		
		"extensions": {
			"sexual_extension": {
				"scope": "sexual",
				
				"interaction_structure": {
					"primary_focus": "body_part",
					"interaction_type": "reciprocal",
					"typical_flow": "The giver restrains the receiver to create sensation, control, or erotic stimulation. Duration, intensity, and positioning vary by negotiation.",
					"notes": ""
				},
				
				"participants": [
					{
						"role": "giver",
						"role_dynamics": "Applies and maintains restraint, adjusts based on feedback, ensures safety.",
						"awareness_state": "aware",
						"notes": ""
					},
					{
						"role": "receiver",
						"role_dynamics": "Experiences restraint, provides feedback via safewords or non-verbal signals.",
						"awareness_state": "aware",
						"notes": ""
					}
				],
				
				"intensity_scale": [
					{
						"id": "bondage_low",
						"level": 1,
						"label": "Low Intensity",
						"description": "Light, casual restraint with minimal physical restriction; short duration, minimal control.",
						"notes": ""
					},
					{
						"id": "bondage_medium",
						"level": 2,
						"label": "Moderate Intensity",
						"description": "Noticeable restraint with some sensory limitation; moderate duration, some psychological stimulation.",
						"notes": ""
					},
					{
						"id": "bondage_high",
						"level": 3,
						"label": "High Intensity",
						"description": "Prolonged or complex restraints with high physical or psychological impact; typically involves advanced techniques.",
						"notes": ""
					},
					{
						"id": "bondage_self",
						"level": 3,
						"label": "Self-Bondage",
						"description": "Receiver restrains themselves; high-risk, requires careful planning and emergency release strategy.",
						"notes": "Scenario-specific high-risk mapping."
					},
					{
						"id": "bondage_forced",
						"level": 3,
						"label": "Forced Bondage",
						"description": "Applied by a partner without prior consent; non-consensual and ethically/legal high-risk.",
						"notes": "Scenario-specific high-risk mapping."
					}
				],
				
				"compatibility": {
					"required_conditions": [
						{
							"type": "rule",
							"reference": "safeword_or_signal",
							"description": "Mandatory for ensuring safe negotiation and emergency release."
						}
					],
					"common_combinations": [
						{
							"type": "kink",
							"reference": "discipline",
							"description": "Restraint can be paired with control or impact play."
						},
						{
							"type": "kink",
							"reference": "sensory_play",
							"description": "Enhances sensation through immobilization."
						}
					],
					"conflicts": [
						{
							"type": "boundary",
							"reference": "non-consent",
							"severity": "high",
							"reason": "Bondage requires explicit consent; non-consensual application is dangerous and unethical."
						}
					],
					"notes": ""
				},
				
				"consent_model": {
					"default_mode": "consensual",
					"boundary_sensitivity": "high",
					"scenario_overrides": [
						{
							"scenario": "Self-bondage",
							"consent_present": "true",
							"risk_level": "high",
							"notes": "Participant must ensure self-release and safety."
						},
						{
							"scenario": "Forced bondage",
							"consent_present": "false",
							"risk_level": "high",
							"notes": "Non-consensual; high ethical, physical, and legal risks."
						}
					],
					"notes": ""
				},
				
				"notes": ""
			},
			
			"social_extension": {
				"scope": "social",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			},
			
			"combat_extension": {
				"scope": "combat",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			}
		},
		
		"notes": "Bondage is inherently consensual by default. Safety, communication, and monitoring are essential for all play levels."
	}
}
```
```json
{
	"schema_type": "kink",
	
	"instruction": "Provide kink metadata with full schema, including extensions for sexual, social, and combat contexts.",
	"input": ["What is Voyeurism?"],
	
	"output": {
		"id": "voyeurism",
		"name": "Voyeurism",
		"synonyms": ["watching", "observation play", "spectatorship"],
		"category": ["Kink", "Observation"],
		"description": "A kink in which a participant derives arousal, interest, or psychological engagement from observing others in private or intimate situations, typically without the observed participant's awareness or consent. Consensual variants exist primarily in roleplay or performative contexts.",
		
		"ontology": {
			"core_dynamic": "psychological",
			"primary_focus": "observation",
			"secondary_focus": "power",
			"subtype": "non-consensual / roleplay-consensual",
			"consent_structure": "non_consensual_by_default",
			"notes": "Voyeurism relies on asymmetry of awareness and often involves secrecy or tension between observer and observed."
		},
		
		"roles": {
			"giver": {
				"id": "voyeurism_giver_role",
				"label": "Giver",
				"definition": "The participant being observed.",
				"typical_awareness": "unaware",
				"notes": "Awareness may occur in roleplay or performative scenarios."
			},
			"receiver": {
				"id": "voyeurism_receiver_role",
				"label": "Receiver",
				"definition": "The participant who observes others and derives satisfaction from watching.",
				"typical_awareness": "aware",
				"notes": "Can act secretly or within negotiated roleplay depending on the scenario."
			}
		},
		
		"consent_and_safety": {
			"typical_consent_state": "non_consensual",
			"consent_requirement": "context_dependent",
			"negotiation_norm": "context_dependent",
			"physical_risk": "low",
			"psychological_risk": "moderate",
			"legal_risk": "high",
			
			"default_scenario": {
				"description": "Secret observation without the observed participant's awareness or permission.",
				"consent_present": "false",
				"risk_level": "moderate",
				"notes": "Consent is absent by default, creating ethical and legal concerns."
			},
			
			"scenario_overrides": [
				{
					"scenario": "Consensual roleplay with pre-negotiated observation.",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "low",
					"notes": "Participants agree to simulate secrecy or observation."
				},
				{
					"scenario": "Observed participant notices being watched but pretends not to notice.",
					"consent_present": "ambiguous",
					"consent_requirement": "context_dependent",
					"risk_level": "low",
					"notes": "Consent may be implicit or negotiated prior, context-dependent."
				}
			],
			
			"notes": "Voyeurism is defined by secrecy and asymmetry; ethical considerations depend on the scenario."
		},
		
		"extensions": {
			"sexual_extension": {
				"scope": "sexual",
				
				"interaction_structure": {
					"primary_focus": "dynamic",
					"interaction_type": "passive",
					"typical_flow": "The observer watches the actions of another participant, deriving stimulation from secrecy, distance, or asymmetry of awareness.",
					"notes": ""
				},
				
				"participants": [
					{
						"role": "receiver",
						"role_dynamics": "Primarily observes and remains physically detached.",
						"awareness_state": "aware",
						"notes": ""
					},
					{
						"role": "giver",
						"role_dynamics": "Performs or exists within the observed scenario, usually unaware.",
						"awareness_state": "unaware",
						"notes": ""
					}
				],
				
				"intensity_scale": [
					{
						"id": "voyeurism_low",
						"level": 1,
						"label": "Low Intensity",
						"description": "Casual observation from a distance, minimal arousal; mostly non-invasive.",
						"notes": ""
					},
					{
						"id": "voyeurism_medium",
						"level": 2,
						"label": "Moderate Intensity",
						"description": "Focused observation with psychological or sexual stimulation, limited awareness from the observed party.",
						"notes": ""
					},
					{
						"id": "voyeurism_high",
						"level": 3,
						"label": "High Intensity (Secret Observation)",
						"description": "Close or prolonged observation without consent, significant psychological arousal; ethically and legally sensitive.",
						"notes": "Maps scenario to non-consensual high-risk context."
					},
					{
						"id": "voyeurism_roleplay",
						"level": 3,
						"label": "High Intensity (Roleplay)",
						"description": "Consensual roleplay observation, pre-negotiated; simulation of secrecy with erotic or psychological stimulation.",
						"notes": "Scenario-specific, high intensity but consensual."
					},
					{
						"id": "voyeurism_aware_pretend",
						"level": 2,
						"label": "Moderate Intensity (Aware Pretend)",
						"description": "Observed participant notices but pretends not to; partially consensual, moderate stimulation.",
						"notes": "Scenario-specific, moderate intensity, partial consent."
					}
				],
				
				"compatibility": {
					"required_conditions": [
						{
							"type": "dynamic",
							"reference": "asymmetry_of_awareness",
							"description": "Observation requires unequal knowledge between participants."
						}
					],
					"common_combinations": [
						{
							"type": "kink",
							"reference": "exhibitionism",
							"description": "Works well when the observed participant performs for an audience in roleplay."
						},
						{
							"type": "dynamic",
							"reference": "power_exchange",
							"description": "Observation can reinforce power imbalance."
						}
					],
					"conflicts": [
						{
							"type": "boundary",
							"reference": "privacy",
							"severity": "high",
							"reason": "Voyeurism conflicts with strong privacy boundaries."
						}
					],
					"notes": ""
				},
				
				"consent_model": {
					"default_mode": "non_consensual",
					"boundary_sensitivity": "high",
					"scenario_overrides": [
						{
							"scenario": "Secret observation",
							"consent_present": "false",
							"risk_level": "high",
							"notes": "Non-consensual; high ethical and legal risk."
						},
						{
							"scenario": "Consensual roleplay",
							"consent_present": "true",
							"risk_level": "low",
							"notes": "Simulated secrecy; pre-negotiated."
						},
						{
							"scenario": "Observed notices but pretends",
							"consent_present": "ambiguous",
							"risk_level": "low",
							"notes": "Partial consent or negotiation implied."
						}
					],
					"notes": ""
				},
				
				"notes": ""
			},
			
			"social_extension": {
				"scope": "social",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			},
			
			"combat_extension": {
				"scope": "combat",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			}
		},
		
		"notes": "Voyeurism is structurally non-consensual by default, with exceptions in roleplay or performative contexts."
	}
}
```
```json
{
	"schema_type": "kink",
	
	"instruction": "Provide kink metadata with full schema, including extensions for sexual, social, and combat contexts.",
	"input": ["What is Pet Play?"],
	
	"output": {
		"id": "pet_play",
		"name": "Pet Play",
		"synonyms": ["animal roleplay", "puppy play", "kitten play", "pony play"],
		"category": ["Kink", "Roleplay", "Power Exchange"],
		"description": "A roleplay kink in which a participant adopts the behaviors, mindset, and sometimes appearance of a pet or animal, often including elements of submission, playfulness, and erotic engagement.",
		
		"ontology": {
			"core_dynamic": "psychological",
			"primary_focus": "roleplay",
			"secondary_focus": "submission",
			"subtype": "puppy | kitten | pony",
			"consent_structure": "consensual_by_default",
			"notes": "Pet Play emphasizes voluntary role adoption, fantasy, and structured power dynamics. Subtypes define the animal role, associated behaviors, props, and sexual activities."
		},
		
		"roles": {
			"giver": {
				"id": "pet_play_giver_role",
				"label": "Giver",
				"definition": "The participant who acts as the owner, trainer, or handler of the pet roleplayer.",
				"typical_awareness": "aware",
				"notes": "Responsible for guidance, commands, and oversight of the play, including safety and consent."
			},
			"receiver": {
				"id": "pet_play_receiver_role",
				"label": "Receiver",
				"definition": "The participant assuming the pet role, performing behaviors, and potentially engaging in erotic acts.",
				"typical_awareness": "aware",
				"notes": "May include subtype-specific props and erotic behaviors depending on negotiation."
			}
		},
		
		"consent_and_safety": {
			"typical_consent_state": "consensual",
			"consent_requirement": "required",
			"negotiation_norm": "expected",
			"physical_risk": "low",
			"psychological_risk": "moderate",
			"legal_risk": "low",
			
			"default_scenario": {
				"description": "Standard Pet Play roleplay with full awareness and prior negotiation between participants.",
				"consent_present": "true",
				"risk_level": "low",
				"notes": "Most Pet Play scenarios are fully consensual, emphasizing play, fantasy, and erotic exploration."
			},
			
			"scenario_overrides": [
				{
					"scenario": "Puppy Play",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Behaviors include crawling, barking, fetching, play wrestling. Props: ears, leash, collar. Erotic activities: doggy style sex."
				},
				{
					"scenario": "Kitten Play",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Behaviors include purring, kneading, playful scratching, rolling. Props: ears, tail. Erotic activities: licking genitalia."
				},
				{
					"scenario": "Pony Play",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "Behaviors include walking on all fours, pulling carts. Props: hooves (binding arms and legs, padded elbow protections), harness (saddle), cart. Erotic activities: optional, often combined with bondage."
				},
				{
					"scenario": "Erotic Pet Play with sexual props",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Use of anal plugs, tails, or other erotic devices requires negotiation and safewords."
				},
				{
					"scenario": "Public or semi-public Pet Play",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "low",
					"notes": "Social interaction, roleplay focused, no sexual activity unless pre-negotiated."
				},
				{
					"scenario": "Short Session Pet Play (under 30 minutes)",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "low",
					"notes": "Brief roleplay with minimal physical strain."
				},
				{
					"scenario": "Extended Session Pet Play (over 1 hour)",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Extended duration may require hydration, breaks, and careful monitoring of props or sexual devices."
				}
			],
			
			"notes": "Pet Play is always consensual; subtype, props, and session length determine intensity and physical risk."
		},
		
		"extensions": {
			"sexual_extension": {
				"scope": "sexual",
				
				"interaction_structure": {
					"primary_focus": "role",
					"interaction_type": "reciprocal",
					"typical_flow": "The owner directs the pet-role participant, including subtype-specific props and sexual behaviors depending on negotiation.",
					"notes": "Props and subtypes define intensity, risk, and erotic activities."
				},
				
				"participants": [
					{
						"role": "giver",
						"role_dynamics": "Directs, guides, or stimulates the pet roleplayer; ensures consent and safety.",
						"awareness_state": "aware",
						"notes": ""
					},
					{
						"role": "receiver",
						"role_dynamics": "Performs subtype-specific pet behaviors; may engage in erotic acts depending on negotiation.",
						"awareness_state": "aware",
						"notes": ""
					}
				],
				
				"intensity_scale": [
					{
						"id": "pet_play_low",
						"level": 1,
						"label": "Low Intensity",
						"description": "Non-erotic, playful roleplay with minimal props. Puppy: ears, collar; Kitten: ears, tail; Pony: basic walking or harness practice.",
						"notes": "Low-risk, consensual scenario."
					},
					{
						"id": "pet_play_medium",
						"level": 2,
						"label": "Moderate Intensity",
						"description": "Longer sessions, more immersive roleplay. Puppy: crawling, fetching, doggy style sex; Kitten: kneading, rolling, licking genitalia; Pony: extended walking, harness, cart.",
						"notes": "Moderate-risk scenario with negotiated sexual elements."
					},
					{
						"id": "pet_play_high",
						"level": 3,
						"label": "High Intensity",
						"description": "Extended duration, full erotic engagement. Puppy: doggy style sex, tail props; Kitten: genital licking, full nudity; Pony: hooves (arms/legs bound), saddle, cart, optional sexual acts.",
						"notes": "High-risk scenario requiring careful consent, negotiation, and safewords."
					}
				],
				
				"compatibility": {
					"required_conditions": [
						{
							"type": "rule",
							"reference": "consent_and_safewords",
							"description": "Mandatory for all erotic or extended Pet Play."
						}
					],
					"common_combinations": [
						{
							"type": "kink",
							"reference": "bondage",
							"description": "Enhances control and roleplay dynamics."
						},
						{
							"type": "kink",
							"reference": "discipline",
							"description": "Commands and correction complement pet behaviors."
						}
					],
					"conflicts": [
						{
							"type": "boundary",
							"reference": "non-consent",
							"severity": "high",
							"reason": "Pet Play must always be consensual."
						}
					],
					"notes": ""
				},
				
				"consent_model": {
					"default_mode": "consensual",
					"boundary_sensitivity": "high",
					"scenario_overrides": [
						{
							"scenario": "Puppy Play",
							"consent_present": "true",
							"risk_level": "moderate",
							"notes": "Props: ears, leash, collar. Erotic: doggy style sex."
						},
						{
							"scenario": "Kitten Play",
							"consent_present": "true",
							"risk_level": "moderate",
							"notes": "Props: ears, tail. Erotic: licking genitalia."
						},
						{
							"scenario": "Pony Play",
							"consent_present": "true",
							"risk_level": "high",
							"notes": "Props: hooves, harness, cart. Erotic activities optional; often combined with bondage."
						}
					],
					"notes": ""
				},
				
				"notes": ""
			},
			
			"social_extension": {
				"scope": "social",
				"interaction_structure": {
					"primary_focus": "roleplay",
					"interaction_type": "reciprocal",
					"typical_flow": "Participants interact in public or social settings, performing subtype-specific pet behaviors for fun or social engagement.",
					"notes": ""
				},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			},
			
			"combat_extension": {
				"scope": "combat",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			}
		},
		
		"notes": "Pet Play is always consensual. Subtype, props, sexual behaviors, and session length define intensity, physical risk, and erotic potential."
	}
}
```
```json
{
	"schema_type": "kink",
	
	"instruction": "Provide kink metadata with full schema, including extensions for sexual, social, and combat contexts.",
	"input": ["What is Enema?"],
	
	"output": {
		"id": "enema",
		"name": "Enema",
		"synonyms": ["anal irrigation", "bladder irrigation", "uterine infusion"],
		"category": ["Kink", "Medical Play", "Anal Play", "Bladder Play", "Uterus Play"],
		"description": "A sexual or medical roleplay kink involving the insertion of liquids into the anal canal, bladder, or uterus for erotic, therapeutic, or power-exchange purposes.",
		
		"ontology": {
			"core_dynamic": "physical | erotic",
			"primary_focus": "bodily stimulation",
			"secondary_focus": "submission",
			"subtype": "anal | bladder | uterus",
			"consent_structure": "consensual_by_default",
			"notes": "Enemas can be erotic, medical, or roleplay-based. Subtypes define the anatomical target and level of intensity."
		},
		
		"roles": {
			"giver": {
				"id": "enema_giver_role",
				"label": "Giver",
				"definition": "The participant who administers the enema, controls flow and type of liquid.",
				"typical_awareness": "aware",
				"notes": "Responsible for hygiene, safety, and monitoring participant comfort."
			},
			"receiver": {
				"id": "enema_receiver_role",
				"label": "Receiver",
				"definition": "The participant who receives the enema.",
				"typical_awareness": "aware",
				"notes": "Must communicate discomfort and consent; may engage in erotic or submissive roleplay."
			}
		},
		
		"consent_and_safety": {
			"typical_consent_state": "consensual",
			"consent_requirement": "required",
			"negotiation_norm": "expected",
			"physical_risk": "moderate",
			"psychological_risk": "moderate",
			"legal_risk": "low",
			
			"default_scenario": {
				"description": "Standard enema play administered with prior negotiation, proper hygiene, and participant awareness.",
				"consent_present": "true",
				"risk_level": "moderate",
				"notes": "Safety measures include lubrication, proper positioning, flow control, and hygiene protocols."
			},
			
			"scenario_overrides": [
				{
					"scenario": "Anal Enema",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Liquid inserted into the rectum; hygiene and lubrication critical; may be erotic or roleplay-based."
				},
				{
					"scenario": "Bladder Enema / Catheter Play",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "Liquid inserted into the bladder; must use sterile techniques and careful monitoring; higher risk of infection or injury."
				},
				{
					"scenario": "Uterus Enema / Uterine Infusion",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "high",
					"notes": "Liquid introduced into the uterus; medical expertise recommended; very high risk if improperly performed."
				},
				{
					"scenario": "Erotic Roleplay with Enemas",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "May include submissive/dominant dynamics, additional erotic stimulation, or power exchange."
				},
				{
					"scenario": "Short Session (under 15 minutes)",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "low",
					"notes": "Minimal volume, low physical strain."
				},
				{
					"scenario": "Extended Session (over 30 minutes)",
					"consent_present": "true",
					"consent_requirement": "required",
					"risk_level": "moderate",
					"notes": "Larger volume, multiple infusions, requires monitoring for comfort and safety."
				}
			],
			
			"notes": "Enemas are high-consent, high-hygiene activities. Participants must negotiate limits, fluid types, and session length. Medical safety is essential for bladder and uterine play."
		},
		
		"extensions": {
			"sexual_extension": {
				"scope": "sexual",
				
				"interaction_structure": {
					"primary_focus": "body stimulation",
					"interaction_type": "reciprocal | giver-led",
					"typical_flow": "The giver administers the enema; the receiver may engage in erotic roleplay, sexual stimulation, or submissive responses.",
					"notes": "Intensity and eroticism depend on fluid type, anatomical target, and negotiated roleplay."
				},
				
				"participants": [
					{
						"role": "giver",
						"role_dynamics": "Administers liquid, monitors hygiene, flow, and consent.",
						"awareness_state": "aware",
						"notes": ""
					},
					{
						"role": "receiver",
						"role_dynamics": "Receives enema, may perform submissive or erotic behaviors.",
						"awareness_state": "aware",
						"notes": ""
					}
				],
				
				"intensity_scale": [
					{
						"id": "enema_low",
						"level": 1,
						"label": "Low Intensity",
						"description": "Small volume, short duration, mostly roleplay or light erotic stimulation.",
						"notes": "Minimal risk, beginner-friendly scenario."
					},
					{
						"id": "enema_medium",
						"level": 2,
						"label": "Moderate Intensity",
						"description": "Medium volume, moderate duration, can include anal, bladder, or erotic roleplay.",
						"notes": "Moderate-risk scenario; hygiene and consent critical."
					},
					{
						"id": "enema_high",
						"level": 3,
						"label": "High Intensity",
						"description": "Large volume, extended session, may involve bladder or uterine infusion; medical safety precautions required.",
						"notes": "High-risk scenario; only for fully negotiated and informed participants."
					}
				],
				
				"compatibility": {
					"required_conditions": [
						{
							"type": "rule",
							"reference": "consent_and_hygiene",
							"description": "Mandatory for all enema activities; prevents injury and infection."
						}
					],
					"common_combinations": [
						{
							"type": "kink",
							"reference": "bondage",
							"description": "Enhances submission and positional control during administration."
						},
						{
							"type": "kink",
							"reference": "power_exchange",
							"description": "Dominant/submissive dynamics are common."
						}
					],
					"conflicts": [
						{
							"type": "boundary",
							"reference": "non-consent",
							"severity": "high",
							"reason": "Enemas without explicit negotiation are unsafe and unethical."
						}
					],
					"notes": ""
				},
				
				"consent_model": {
					"default_mode": "consensual",
					"boundary_sensitivity": "high",
					"scenario_overrides": [
						{
							"scenario": "Anal Enema",
							"consent_present": "true",
							"risk_level": "moderate",
							"notes": "Small to moderate volume, proper hygiene required."
						},
						{
							"scenario": "Bladder Enema",
							"consent_present": "true",
							"risk_level": "high",
							"notes": "Medical safety critical; sterile equipment required."
						},
						{
							"scenario": "Uterus Enema",
							"consent_present": "true",
							"risk_level": "high",
							"notes": "Medical knowledge required; high risk if improperly performed."
						},
						{
							"scenario": "Erotic Roleplay Enema",
							"consent_present": "true",
							"risk_level": "moderate",
							"notes": "Includes sexual or submissive dynamics; always negotiated."
						}
					],
					"notes": ""
				},
				
				"notes": ""
			},
			
			"social_extension": {
				"scope": "social",
				"interaction_structure": {
					"primary_focus": "roleplay or exhibition",
					"interaction_type": "giver-led",
					"typical_flow": "Participants may observe or guide enema play in fetish or social roleplay contexts.",
					"notes": ""
				},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			},
			
			"combat_extension": {
				"scope": "combat",
				"interaction_structure": {},
				"participants": [],
				"intensity_scale": [],
				"compatibility": {},
				"consent_model": {},
				"notes": ""
			}
		},
		
		"notes": "Enema play is high-consent, hygiene-critical, and potentially high-risk, especially for bladder or uterine infusion. Subtypes, fluid type, and session length define intensity and risk."
	}
}
```