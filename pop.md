https://confluence.i2cat.net/display/5GP/2021-03-23+Team2+Meeting

1. Product Offering Price (POP), define structure and mechanism for the conformation of bundled POP resources to be attached to PO.

   - Current approach relies on popRelationship and bundledPopRelationship to allows the definition of different prices per usage limits and per VNF, respectively. However, this modeling doesn't ensure the right mapping btw POP and VNF.

   - Proposal: to use prodSpecCharValueUse. This requires that the product specification should contain the required characteristics with the list of values to be considered.

   - Revise use of priceType and unitOfMeasure

https://confluence.i2cat.net/display/5GP/2021-04-07+POP+Meeting

1. When creating a (VNF/NS) PO, a baseline POS (pre-operation setup) will be associated to the offer (via productSpecificationRelationship), which comprises characteristics and values needed for the e-licensing mgmt.

   - The use of a baseline POS could potentially be extended to other offer types, if needed

2. Simple POPs can be composed or reused as needed (one per every single aspect to be charged for this offer), and bundled all together in a parent POP that is associated with the PO. Example of simple POPs to be - supported include:

   - one time offer provisioning
   - number of users consuming a given VNF
   - number of instances deployed for a given VNF
   - time of usage of a given VNF

3. When creating a POP from the MKP portal a scroll down list could be depicted to select the characteristic from the POS and for each characteristic another scroll down list could be depicted to select the value(s).

   - Single value (if isUnique = true) or multiple values (if isUnique = false).

4. If the characteristic appears as configurable (configurable = true), the value(s) should be added by the user filling the form. This is used for the FunctionDescriptorName. In the resource/service specification, a characteristic called the same could be added (when creating the RS/SS) and their values retrieved in a scroll down list to avoid errors.

5. License agreements can be added (optional) to every POP. Such agreements are expressed via LP templates and are referred to in the POP by using the PricingLogicAlgorithm (according to TMF specs: an entity that represents an instantiation of an interface specification to external rating function).

6. eLM needs to have the complete information of the offer (and in particular its POPs) to be monitored.

https://confluence.i2cat.net/display/5GP/2021-04-29+Team2+Meeting

1. Updates about POP modeling and its relation with license agreements
   - License LPT (LPR) also contain the the file with the pricing logic algorithm (PLA)
   - License definitions are filled LPT. ATOS will share a sample of template Bravo, Fernando
   - When composing a POP, a reference to the license definition containing the required PLA is added
   - POP can then be reused for creation of several PO

--- SLACK ---

- POPs are just like POs -> same configs/editing views
  - POP object that we have found to be the most useful -> pop.json
- POP configurations need to sit at the PO specification level and then populated at the POP itself.

- isunique=True would mean that a single dropdown for a prodSpecCharValueUse should be provided - A use of the ProductSpecificationCharacteristicValue by a ProductOfferingPrice to which additional properties (attributes) apply or override the properties of similar properties contained in ProductSpecificationCharacteristicValue. It should be noted that characteristics which their value(s) addressed by this object must exist in corresponding product specification. The available characteristic values for a ProductSpecificationCharacteristic in a Product specification can be modified at the ProductOffering and ProcuctOfferingPrice level. The list of values in ProductSpecificationCharacteristicValueUse is a strict subset of the list of values as defined in the corresponding product specification characteristics.

- isunique=False would require to add more than one value from the dropdown as separate productSpecCharacteristicValue for the same prodSpecCharValueUse.
- There is also "configurable": true which would mean that the user has to fill it in manually instead of from a dropdown.

Duvidas:

- de onde vem as caracteristicas faladas em: 3. When creating a POP from the MKP portal a scroll down list could be depicted to select the characteristic from the POS and for each characteristic another scroll down list could be depicted to select the value(s).
  - de onde vem os values dos dropdowns (inner dropdowns)
  - ao fazer uma lista dos POPs que existem é mostrar esta info. temos estas carac e dentro disso temos estes valores. na criação nao sei como pré-preencher estes valores. é uma relaçao que foi definido no resource/service ou é preciso ir buscar a algum lado (GET API)?
- oq é suposto alterar qdo se associa um POP a um PO (c/ e s/ configurable tbm, oqeq ja vem e oqeq n vem)
- o que é obrigatorio ser preenchido pelo user para fazer o JSON?

Ecras:

- nova secção p/ POP (product offering price)
  - listar POPs que existem e
    - poder abrir os detalhes deles:
      - ir buscar campos
        - mostrar as infos dos 2 dropdowns (tabela de pré-vis)
  - criar novos POPs
    - +/- a mesma coisa que listagem
      - inputs de informaçao relativa aos 2 dropdowns

https://github.com/5GZORRO/elicensing-manager-core/tree/develop-mid-release-v1.2.0/resources

--- notas reuniao guillermo ---
prodSpecCharValue:

- this are optional and we click on a plus to add this ones
- basically a PO and POS they both have access the POS (product offer specification) it's a different object with specific config & required in order to have a PO
- POS should be able to create on the Portal
- via dependency field

generic_po_specification should be available at the catalog for everyone to use. it's a generic specification which has some productspec characteristics which are later editable/configurable

po_slice_webserver_and_database_specification a user needs to create this one before they create a PO. they can add productSpecCharacteristic as they want (like Database, resources, etc...)

- productSpecificationRelationship this links to generic PO specification.
- this can be auto but not closed, relaçao 1:N

po_slice_webserver_and_database product specification is linked, relaçao 1:1

- list POP
- later vai incluir uma lista de bundlePOP

pop_slice_webserver_and_database this one is used to bundle inner POP (via bundledPopRelationship), the info is inside each POP

- POP that price is number of users but at network level service

pop_vnf_database_time_of_use main POP config that should be available

- mais importante preço. pricingLogicAlgorithm precisa de ser linkado a um file
- prodSpecCharValueUse this one needs to be selected at least FunctionDescriptorName FunctionDescriptorType PriceLogic, the 3 are mandatory
- funcdescriptor name needs to be filled manually
- FunctionDescriptorType is a selector (if not unique)
- one specification has 1 or more values, defined via: "configurable": true, "isUnique": true,

i select time of use, you can select any of the values (MAX, MIN)

the POP can be create before and mapped after to PO - mas lets keep it simple

unit of measure:

- 1 user é weird mas é suposto ser isso

https://drive.google.com/file/d/1P_i0B9ew7LtUUUAJeJ3VFHFynsRXfpcz/view?usp=sharing

{

- "id": "pop_webservers_n_users",
  "href": "",
- "name": "webserver cost based on users connected",
- "description": "Recurring charge for concurrent number of users connected to a webserver over a month",
- "validFor": {
  "startDateTime": "2021-02-03T00:00",
  "endDateTime": "2022-02-02T00:00"
  },
- "recurringChargePeriodType": "month",
- "recurringChargePeriodLength": 1,
  "isBundle": false,
  "bundledPopRelationship": "",
  "pricingLogicAlgorithm": [
  {
  "id": "pla_n_users",
  "href": "",
  "name": "Algorithm for a step-type cost of users",
  "description": "Recurring charge for concurrent number of users connected to a webserver over a month",
  "validFor": {
  "startDateTime": "2021-02-03T00:00",
  "endDateTime": "2022-02-02T00:00"
  },
  "plaSpecId": "pla_n_users",
  "@type": "pricingLogicAlgorithm"
  }
  ],
- "lifecycleStatus": "Active",
- "priceType": "recurrent",
- "unitOfMeasure": "1 user",
- "price": {
  "unit": "EUR",
  "amount": 20
  },
- "prodSpecCharValueUse": [
  {
  "name": "FunctionDescriptorName",
  "description": "Descriptor name which links this specification to an actual Virtual Network Function",
  "valueType": "string",
  "configurable": true,
  "isUnique": false,
  "productSpecCharacteristicValue": [
  {
  "isDefault": true,
  "valueType": "string",
  "value": "webserver-vnf"
  }
  ]
  },
  {
  "name": "FunctionDescriptorType",
  "description": "Type of component that correspond to the FunctionDescriptorName values in a POP. This PSC affects to the entire POP, therefore each type of FunctionDescriptorName would require a new POP.",
  "valueType": "string",
  "configurable": true,
  "isUnique": true,
  "productSpecCharacteristicValue": [
  {
  "isDefault": true,
  "valueType": "string",
  "value": "VIRTUAL_NETWORK_FUNCTION"
  }
  ]
  },
  {
  "name": "PriceLogic",
  "description": "Charged by the number of users connected over a month",
  "valueType": "string",
  "productSpecCharacteristicValue": [
  {
  "isDefault": true,
  "valueType": "string",
  "value": "N_OF_USERS"
  }
  ]
  },
  {
  "name": "UnitOfMeasureAggregation",
  "description": "Additional logic required to correctly measure the PriceType",
  "valueType": "string",
  "productSpecCharacteristicValue": [
  {
  "isDefault": false,
  "valueType": "string",
  "value": "MAX"
  },
  {
  "isDefault": false,
  "valueType": "string",
  "value": "CONCURRENT"
  }
  ]
  }
  ],
  "@type": "ProductOfferingPrice"
  }
