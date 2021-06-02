This POP, with the ID {{id}} - {{name}} and href: {{href}}.

Description:
{{description}}.

LifeCycleStatus: {{lifeCycleStatus}}.

Bundle: {{isBundle}}.

Last Update: {{lastUpdate  as "DD/MM/YYYY"}}.

{{#with validFor}}This POP is valid since {{startDateTime as "DD/MM/YYYY"}} until {{endDateTime as "DD/MM/YYYY"}}.{{/with}}

Price type: {{priceType}}, period type: {{recurringChargePeriodType}}, period length: {{recurringChargePeriodLength}}.
Price: {{#with price}}{{value}} {{unit}}.{{/with}}
Percentage: {{percentage}}.

PRODUCT OFFERING TERM:
{{#ulist productOfferingTerm}}
{{name}}: {{description}}. {{#with duration}}Duration: {{units}} {{amount}}.{{/with}} {{#with validFor}}Valid since {{startDateTime as "DD/MM/YYYY"}} until {{endDateTime as "DD/MM/YYYY"}}.{{/with}}
{{/ulist}}

PLACE:
{{#ulist place}}
{{id}}: {{name}}. Href: {{href}}
{{/ulist}}

CONSTRAINT:
{{#ulist constraint}}
{{id}}: {{name}}. Href: {{href}}.
{{/ulist}}

PRICING LOGIC ALGORITHM:
{{#ulist pricingLogicAlgorithm}}
{{id}} (plaSecId: {{plaSecId}}): {{name}}, {{description}}. Href: {{href}}. {{#with validFor}}Valid since {{startDateTime as "DD/MM/YYYY"}} until {{endDateTime as "DD/MM/YYYY"}}.{{/with}}
{{/ulist}}

TAX:
{{#ulist tax}}
{{#with taxAmount}}Tax amount: {{value}} {{unit}}{{/with}} {{taxCategory}}, rate: {{taxRate}}.
{{/ulist}}

version: {{version}}.
