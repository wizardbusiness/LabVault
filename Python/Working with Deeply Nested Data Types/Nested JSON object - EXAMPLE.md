
```python
data = {

	"company": "Axiom Labs",
	"founded": 2011,
	"active": True,
	"headquarters": {
		"city": "Portland",
		"state": "OR",
		"zip": "97201"
	},
	"financials": {
		"revenue": 4_200_000.00,
		"profitable": False,
		"funding_rounds": [2_000_000, 5_000_000, 12_000_000]
	},
	"departments": [
		{
			"name": "Engineering",
			"headcount": 42,
			"lead": "Dana Scully",
			"tags": ["backend", "infra", "ml"],
			"projects": [
				{
					"title": "Nighthawk", 
					"status": "active",
					"budget": 300_000
				},
				{
					"title": "Orion", 
					"status": "completed", 
					"budget": 150_000
				},
			]
		},
		{
			"name": "Marketing",
			"headcount": 11,
			"lead": "Fox Mulder",
			"tags": ["growth", "content"],
			"projects": [
				{"title": "Blueshift", 
					"status": "active", 
					"budget": 80_000
				},
				{
					"title": "Echo", 
					"status": "paused", 
					"budget": 45_000
				},
			]
		},
		{
		"name": "Operations",
			"headcount": 9,
			"lead": None,
			"tags": ["logistics", "infra"],
			"projects": []
		}
	],
	"metadata": {
		"tags": ["Series B", "SaaS", "infra"],
		"notes": [
			"Rebranded from AxiomSoft in 2019",
			"Acquired Nocturn Inc. in 2022",
			None
			],
		"audit": {
			"last_reviewed": "2024-11-01",
			"reviewed_by": "compliance-bot",
			"flags": []
		}
	}
}
```
