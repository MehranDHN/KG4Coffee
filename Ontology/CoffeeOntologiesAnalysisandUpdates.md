# Analysis of the Updated Ontologies using GROK for the Coffee Domain

This document consolidates the analyses and corrected, updated versions of three ontologies (`coffee.ttl`, `Beans.ttl`, and `Shops.ttl`) from the coffee business domain, hosted at [KG4Coffee](https://github.com/MehranDHN/KG4Coffee). These ontologies model concepts like coffee beans, drinks, shops, and customer interactions, forming a knowledge graph ideal for GraphDB applications. The original ontologies were analyzed, and updates addressed limitations, including the lack of deep subclass hierarchies, missing OWL restrictions, minimal annotations, and limited individuals. 

## Introduction

The coffee industry involves complex relationships between beans, processing methods, drinks, shops, and customers. Ontologies, represented in RDF/OWL, model these relationships, capturing explicit data (e.g., a bean’s origin) and implicit knowledge (e.g., shops selling drinks made from Organic beans with Fruity flavors). GraphDB excels at querying and reasoning over such knowledge graphs, enabling businesses to uncover insights that relational databases struggle to reveal.

The three ontologies are:
- **`coffee.ttl`**: Models beans, drinks, flavors, roasts, processes, origins, and brewing methods.
- **`Beans.ttl`**: Details beans, including varieties, certifications, and farms, extending `coffee.ttl`.
- **`Shops.ttl`**: Represents shops, menus, staff, customers, and locations, integrating with `coffee.ttl` and `Beans.ttl`.

**IRI Nodes and Literal Nodes**: There are tree types of nodes in the RDF and two of them are very immportant. Later we willl discus the defination and usecases:
-  `IRI Nodes`  being an IRI exactly identifying a datatype that determines how the lexical form maps to a literal value in target **The Domain Of Interest** (Coffee business here).
- `Literal Nodes` Literals are used for values such as strings, numbers, and dates. A literal in an RDF graph consists of two, three, or four elements. ` literals (e.g., `"High"` for `hasAcidityLevel`.
- Retained `xsd:date` for `hasOrderDate`
- `Blank Nodes` We don't disscuss them here.

The document includes:
1. **Original Analyses**: Summaries of each ontology’s structure, strengths, limitations, and features (e.g., `AllDisjointClasses`, flat hierarchies).
2. **Updated Ontologies**: Corrected TTL files with enhancements (hierarchies, restrictions, annotations, individuals).
3. **GraphDB Use Cases**: Examples of business applications.
4. **Conclusion**: Summary of improvements and GraphDB utility.

## Analysis of Original Ontologies

### 1. coffee.ttl Analysis
The `coffee.ttl` ontology provides a foundational coffee domain model.

#### Overview
- **Format**: Turtle (TTL), RDF/OWL.
- **Purpose**: Represents beans, drinks, flavors, roasts, processes, origins, and brewing methods.
- **Namespaces**:
  - `coffee`: `http://www.example.org/coffee#`.
  - Standard: `rdf`, `rdfs`, `owl`, `xsd`.
- **Structure**: Defines classes, properties, individuals, and two `AllDisjointClasses` constructs.

#### Key Components
- **Classes**:
  - `CoffeeBean`, `CoffeeDrink`, `Flavor`, `Roast`, `Process`, `Origin`, `BrewMethod`.
  - **Hierarchy**: Flat, no subclasses (e.g., `Arabica` is an individual).
- **Properties**:
  - **Object**: `hasFlavor`, `hasRoast`, `hasOrigin`, `processedBy`, `usedIn`, `brewedWith`.
  - **Data**: `hasCaffeineContent`, `hasPrice`, `hasAcidityLevel`, `hasBody`.
- **Individuals**:
  - Beans: `Arabica`, `Robusta`, `EthiopianYirgacheffe`.
  - Drinks: `Espresso`, `Cappuccino`.
  - Flavors: `Fruity`, `Chocolatey`.
- **Disjoint Classes**:
  - **Construct 1**: `CoffeeBean`, `CoffeeDrink`, `Flavor`, etc., are mutually exclusive.
  - **Construct 2**: `LightRoast`, `MediumRoast`, `DarkRoast` are mutually exclusive.

#### Strengths
- Broad coverage of coffee concepts.
- Clear relationships (e.g., `hasFlavor`).
- Semantic structure for reasoning.

#### Limitations
- **Shallow Hierarchy**: No subclasses.
- **Limited Individuals**: Few instances.
- **No Restrictions**: Missing OWL constraints.
- **Minimal Annotations**: No `rdfs:label`/`rdfs:comment`.
- **Static Data**: No dynamic updates.

### 2. Beans.ttl Analysis
The `Beans.ttl` ontology focuses on beans, extending `coffee.ttl`.

#### Overview
- **Format**: Turtle (TTL), RDF/OWL.
- **Purpose**: Models beans with varieties, certifications, and farms.
- **Namespaces**:
  - `beans`: `http://www.example.org/beans#`.
  - `coffee`: Links to `coffee.ttl`.
  - Standard: `rdf`, `rdfs`, `owl`, `xsd`.

#### Key Components
- **Classes**:
  - `Bean`, `Variety`, `Certification`, `Farm`.
  - **Hierarchy**: Flat, `Bean` equivalent to `coffee:CoffeeBean`.
- **Properties**:
  - **Object**: `hasVariety`, `hasCertification`, `grownOn`, plus `coffee:hasFlavor`.
  - **Data**: `hasAltitude`, `hasHarvestYear`, `hasPricePerKg`.
- **Individuals**:
  - Beans: `YirgacheffeBean`, `BourbonBean`.
  - Varieties: `Typica`, `Bourbon`.
- **Disjoint Classes**: Likely `Bean`, `Variety`, etc., are mutually exclusive.

#### Strengths
- Detailed bean attributes.
- Integration with `coffee.ttl`.
- Supports sourcing/quality control.

#### Limitations
- **Shallow Hierarchy**: Varieties as individuals.
- **Limited Individuals**: Few instances.
- **No Restrictions**: Missing constraints.
- **Minimal Annotations**: No labels.
- **Static Data**: No updates.

### 3. Shops.ttl Analysis
The `Shops.ttl` ontology models shops and customer interactions.

#### Overview
- **Format**: Turtle (TTL), RDF/OWL.
- **Purpose**: Represents shops, menus, staff, customers, orders.
- **Namespaces**:
  - `shops`: `http://www.example.org/shops#`.
  - `coffee`, `beans`: Links to other ontologies.
  - Standard: `rdf`, `rdfs`, `owl`, `xsd`.

#### Key Components
- **Classes**:
  - `CoffeeShop`, `MenuItem`, `Staff`, `Customer`, `Location`, `Order`, `Review`.
  - **Hierarchy**: Flat, no subtypes.
- **Properties**:
  - **Object**: `sells`, `usesBean`, `employs`, `visitedBy`, `locatedIn`, `placesOrder`, `containsItem`, `hasReview`.
  - **Data**: `hasPrice`, `hasRating`, `hasAddress`, `hasOpeningHours`, `hasOrderDate`.
- **Individuals**:
  - Shops: `DowntownBrew`.
  - Menu Items: `EspressoItem`.
- **Disjoint Classes**: Likely `CoffeeShop`, `MenuItem`, etc., are mutually exclusive.

#### Strengths
- Comprehensive shop modeling.
- Integration with other ontologies.
- Supports CRM/sentiment analysis.

#### Limitations
- **Shallow Hierarchy**: No subtypes.
- **Limited Individuals**: Few instances.
- **No Restrictions**: Missing constraints.
- **Minimal Annotations**: No labels.
- **Static Data**: No updates.

### Deep Subclass Hierarchies Explanation
The ontologies lack **deep subclass hierarchies**, using top-level classes without nested subclasses. For example:
- `coffee.ttl`: `CoffeeBean` has individuals (`Arabica`), not subclasses (`ArabicaBean`).
- `Beans.ttl`: `Variety` has individuals (`Typica`), not subclasses.
- `Shops.ttl`: `CoffeeShop` has no `PhysicalShop` subtype.

A **deep hierarchy** example:
```turtle
:Bean a owl:Class .
:ArabicaBean rdfs:subClassOf :Bean .
:Typica rdfs:subClassOf :ArabicaBean .
:EthiopianTypica rdfs:subClassOf :Typica .
```
This supports granular queries and reasoning.

## Updated Ontologies

The updated ontologies address limitations by:
1. **Deep Subclass Hierarchies**: Adding nested subclasses.
2. **OWL Restrictions**: Including `rdfs:domain`/`rdfs:range`.
3. **Annotations**: Adding `rdfs:label`/`rdfs:comment`.
4. **More Individuals**: Expanding instances.
5. **Corrected Datatypes**: Using proper XSD types (e.g., `xsd:integer` for whole numbers).

### 1. Updated coffee.ttl
#### Changes
- **Hierarchy**:
  - `CoffeeBean`: Added `ArabicaBean`, `RobustaBean`, `Typica`, `Bourbon`, `EthiopianTypica`.
  - `CoffeeDrink`: Added `EspressoBasedDrink`, `FilteredDrink`.
- **Restrictions**: Added `rdfs:domain`/`rdfs:range`.
- **Annotations**: `rdfs:label`/`rdfs:comment` for all elements.
- **Individuals**: Added `Vietnam`, `RobustaBean1`, `FrenchPress`.
- **Datatypes**:
  - `hasPrice`, `hasCaffeineContent`: `xsd:decimal` (e.g., `4.0^^xsd:decimal`).
  - `hasAcidityLevel`, `hasBody`: Simplified `xsd:string` (e.g., `"High"`).

#### Updated TTL
```turtle
@prefix coffee: <http://www.example.org/coffee#> .
@prefix beans: <http://www.example.org/beans#> .
@prefix shops: <http://www.example.org/shops#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

# Ontology Declaration
coffee: a owl:Ontology ;
    rdfs:label "Coffee Ontology"@en ;
    rdfs:comment "An ontology for coffee-related concepts, including beans, drinks, flavors, and brewing methods."@en .

# Classes with Deep Hierarchy
coffee:CoffeeBean a owl:Class ;
    rdfs:label "Coffee Bean"@en ;
    rdfs:comment "A class representing coffee beans, the raw material for coffee drinks."@en .

coffee:ArabicaBean a owl:Class ;
    rdfs:subClassOf coffee:CoffeeBean ;
    rdfs:label "Arabica Bean"@en ;
    rdfs:comment "A subclass of CoffeeBean for Arabica species, known for mild flavors."@en .

coffee:RobustaBean a owl:Class ;
    rdfs:subClassOf coffee:CoffeeBean ;
    rdfs:label "Robusta Bean"@en ;
    rdfs:comment "A subclass of CoffeeBean for Robusta species, known for strong flavors."@en .

coffee:Typica a owl:Class ;
    rdfs:subClassOf coffee:ArabicaBean ;
    rdfs:label "Typica Variety"@en ;
    rdfs:comment "A variety of Arabica bean, known for balanced flavors."@en .

coffee:Bourbon a owl:Class ;
    rdfs:subClassOf coffee:ArabicaBean ;
    rdfs:label "Bourbon Variety"@en ;
    rdfs:comment "A variety of Arabica bean, known for sweet flavors."@en .

coffee:EthiopianTypica a owl:Class ;
    rdfs:subClassOf coffee:Typica ;
    rdfs:label "Ethiopian Typica"@en ;
    rdfs:comment "Typica beans grown in Ethiopia, often with floral notes."@en .

coffee:CoffeeDrink a owl:Class ;
    rdfs:label "Coffee Drink"@en ;
    rdfs:comment "A class representing beverages made from coffee beans."@en .

coffee:EspressoBasedDrink a owl:Class ;
    rdfs:subClassOf coffee:CoffeeDrink ;
    rdfs:label "Espresso-Based Drink"@en ;
    rdfs:comment "A subclass of CoffeeDrink prepared using espresso."@en .

coffee:FilteredDrink a owl:Class ;
    rdfs:subClassOf coffee:CoffeeDrink ;
    rdfs:label "Filtered Drink"@en ;
    rdfs:comment "A subclass of CoffeeDrink prepared using filtration methods."@en .

coffee:Flavor a owl:Class ;
    rdfs:label "Flavor"@en ;
    rdfs:comment "A class representing sensory characteristics of coffee."@en .

coffee:Roast a owl:Class ;
    rdfs:label "Roast"@en ;
    rdfs:comment "A class representing roast levels of coffee beans."@en .

coffee:Process a owl:Class ;
    rdfs:label "Process"@en ;
    rdfs:comment "A class representing processing methods for coffee beans."@en .

coffee:Origin a owl:Class ;
    rdfs:label "Origin"@en ;
    rdfs:comment "A class representing geographic origins of coffee beans."@en .

coffee:BrewMethod a owl:Class ;
    rdfs:label "Brew Method"@en ;
    rdfs:comment "A class representing methods for brewing coffee."@en .

# Disjoint Classes
[] a owl:AllDisjointClasses ;
    owl:members (coffee:CoffeeBean coffee:CoffeeDrink coffee:Flavor coffee:Roast coffee:Process coffee:Origin coffee:BrewMethod) .

# Object Properties
coffee:hasFlavor a owl:ObjectProperty ;
    rdfs:domain [ a owl:Class ; owl:unionOf (coffee:CoffeeBean coffee:CoffeeDrink) ] ;
    rdfs:range coffee:Flavor ;
    rdfs:label "has Flavor"@en ;
    rdfs:comment "Links a coffee bean or drink to its flavor profile."@en .

coffee:hasRoast a owl:ObjectProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range coffee:Roast ;
    rdfs:label "has Roast"@en ;
    rdfs:comment "Links a coffee bean to its roast level."@en .

coffee:hasOrigin a owl:ObjectProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range coffee:Origin ;
    rdfs:label "has Origin"@en ;
    rdfs:comment "Links a coffee bean to its geographic origin."@en .

coffee:processedBy a owl:ObjectProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range coffee:Process ;
    rdfs:label "processed By"@en ;
    rdfs:comment "Links a coffee bean to its processing method."@en .

coffee:usedIn a owl:ObjectProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range coffee:CoffeeDrink ;
    rdfs:label "used In"@en ;
    rdfs:comment "Links a coffee bean to a drink it is used in."@en .

coffee:brewedWith a owl:ObjectProperty ;
    rdfs:domain coffee:CoffeeDrink ;
    rdfs:range coffee:BrewMethod ;
    rdfs:label "brewed With"@en ;
    rdfs:comment "Links a coffee drink to its brewing method."@en .

# Data Properties
coffee:hasCaffeineContent a owl:DatatypeProperty ;
    rdfs:domain coffee:CoffeeDrink ;
    rdfs:range xsd:decimal ;
    rdfs:label "has Caffeine Content"@en ;
    rdfs:comment "Specifies the caffeine content of a coffee drink in milligrams."@en .

coffee:hasPrice a owl:DatatypeProperty ;
    rdfs:domain [ a owl:Class ; owl:unionOf (coffee:CoffeeBean coffee:CoffeeDrink) ] ;
    rdfs:range xsd:decimal ;
    rdfs:label "has Price"@en ;
    rdfs:comment "Specifies the price in USD."@en .

coffee:hasAcidityLevel a owl:DatatypeProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range xsd:string ;
    rdfs:label "has Acidity Level"@en ;
    rdfs:comment "Specifies the acidity level (e.g., low, medium, high) of a coffee bean."@en .

coffee:hasBody a owl:DatatypeProperty ;
    rdfs:domain coffee:CoffeeBean ;
    rdfs:range xsd:string ;
    rdfs:label "has Body"@en ;
    rdfs:comment "Specifies the body (e.g., light, medium, full) of a coffee bean."@en .

# Individuals
coffee:EthiopianTypicaBean a coffee:EthiopianTypica ;
    rdfs:label "Ethiopian Typica Bean"@en ;
    coffee:hasFlavor coffee:Fruity, coffee:Floral ;
    coffee:hasRoast coffee:LightRoast ;
    coffee:hasOrigin coffee:Ethiopia ;
    coffee:processedBy coffee:Washed ;
    coffee:hasAcidityLevel "High" ;
    coffee:hasBody "Light" ;
    coffee:hasPrice 18.0^^xsd:decimal .

coffee:ColombianBourbonBean a coffee:Bourbon ;
    rdfs:label "Colombian Bourbon Bean"@en ;
    coffee:hasFlavor coffee:Chocolatey ;
    coffee:hasRoast coffee:MediumRoast ;
    coffee:hasOrigin coffee:Colombia ;
    coffee:processedBy coffee:Washed ;
    coffee:hasAcidityLevel "Medium" ;
    coffee:hasBody "Medium" ;
    coffee:hasPrice 16.5^^xsd:decimal .

coffee:RobustaBean1 a coffee:RobustaBean ;
    rdfs:label "Vietnam Robusta Bean"@en ;
    coffee:hasFlavor coffee:Nutty ;
    coffee:hasRoast coffee:DarkRoast ;
    coffee:hasOrigin coffee:Vietnam ;
    coffee:processedBy coffee:Natural ;
    coffee:hasAcidityLevel "Low" ;
    coffee:hasBody "Full" ;
    coffee:hasPrice 12.0^^xsd:decimal .

coffee:Espresso a coffee:EspressoBasedDrink ;
    rdfs:label "Espresso"@en ;
    coffee:usedIn coffee:EthiopianTypicaBean ;
    coffee:brewedWith coffee:EspressoMachine ;
    coffee:hasCaffeineContent 63.0^^xsd:decimal ;
    coffee:hasPrice 3.5^^xsd:decimal .

coffee:Cappuccino a coffee:EspressoBasedDrink ;
    rdfs:label "Cappuccino"@en ;
    coffee:usedIn coffee:ColombianBourbonBean ;
    coffee:brewedWith coffee:EspressoMachine ;
    coffee:hasCaffeineContent 75.0^^xsd:decimal ;
    coffee:hasPrice 4.5^^xsd:decimal .

coffee:PourOverCoffee a coffee:FilteredDrink ;
    rdfs:label "Pour Over Coffee"@en ;
    coffee:usedIn coffee:EthiopianTypicaBean ;
    coffee:brewedWith coffee:PourOver ;
    coffee:hasCaffeineContent 95.0^^xsd:decimal ;
    coffee:hasPrice 4.0^^xsd:decimal .

coffee:Fruity a coffee:Flavor ;
    rdfs:label "Fruity Flavor"@en ;
    rdfs:comment "A flavor profile with fruit-like notes."@en .

coffee:Floral a coffee:Flavor ;
    rdfs:label "Floral Flavor"@en ;
    rdfs:comment "A flavor profile with floral notes."@en .

coffee:Chocolatey a coffee:Flavor ;
    rdfs:label "Chocolatey Flavor"@en ;
    rdfs:comment "A flavor profile with chocolate-like notes."@en .

coffee:Nutty a coffee:Flavor ;
    rdfs:label "Nutty Flavor"@en ;
    rdfs:comment "A flavor profile with nut-like notes."@en .

coffee:LightRoast a coffee:Roast ;
    rdfs:label "Light Roast"@en ;
    rdfs:comment "A light roast level, preserving origin flavors."@en .

coffee:MediumRoast a coffee:Roast ;
    rdfs:label "Medium Roast"@en ;
    rdfs:comment "A medium roast level, balancing flavor and body."@en .

coffee:DarkRoast a coffee:Roast ;
    rdfs:label "Dark Roast"@en ;
    rdfs:comment "A dark roast level, emphasizing bold flavors."@en .

coffee:Washed a coffee:Process ;
    rdfs:label "Washed Process"@en ;
    rdfs:comment "A wet processing method for coffee beans."@en .

coffee:Natural a coffee:Process ;
    rdfs:label "Natural Process"@en ;
    rdfs:comment "A dry processing method for coffee beans."@en .

coffee:Honey a coffee:Process ;
    rdfs:label "Honey Process"@en ;
    rdfs:comment "A hybrid processing method for coffee beans."@en .

coffee:Ethiopia a coffee:Origin ;
    rdfs:label "Ethiopia"@en ;
    rdfs:comment "A coffee-growing region in East Africa."@en .

coffee:Colombia a coffee:Origin ;
    rdfs:label "Colombia"@en ;
    rdfs:comment "A coffee-growing region in South America."@en .

coffee:Vietnam a coffee:Origin ;
    rdfs:label "Vietnam"@en ;
    rdfs:comment "A coffee-growing region in Southeast Asia."@en .

coffee:EspressoMachine a coffee:BrewMethod ;
    rdfs:label "Espresso Machine"@en ;
    rdfs:comment "A brewing method using high pressure."@en .

coffee:PourOver a coffee:BrewMethod ;
    rdfs:label "Pour Over"@en ;
    rdfs:comment "A brewing method using manual filtration."@en .

coffee:FrenchPress a coffee:BrewMethod ;
    rdfs:label "French Press"@en ;
    rdfs:comment "A brewing method using immersion."@en .
```

### 2. Updated Beans.ttl
#### Changes
- **Hierarchy**:
  - `Variety`: Added `ArabicaVariety` → `TypicaVariety`, `BourbonVariety`.
  - `Bean`: Linked to `coffee:CoffeeBean`.
- **Restrictions**: Added `rdfs:domain`/`rdfs:range`.
- **Annotations**: `rdfs:label`/`rdfs:comment` for all elements.
- **Individuals**: Added `GeishaBean`, `GeishaVariety`.
- **Datatypes**:
  - `hasPricePerKg`: `xsd:decimal` (e.g., `20.0^^xsd:decimal`).
  - `hasAltitude`, `hasHarvestYear`: `xsd:integer` (e.g., `1800^^xsd:integer`).
  - `hasAcidityLevel`, `hasBody`: Simplified `xsd:string`.

#### Updated TTL
```turtle
@prefix coffee: <http://www.example.org/coffee#> .
@prefix beans: <http://www.example.org/beans#> .
@prefix shops: <http://www.example.org/shops#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

# Ontology Declaration
beans: a owl:Ontology ;
    rdfs:label "Beans Ontology"@en ;
    rdfs:comment "An ontology for detailed modeling of coffee beans, including varieties, certifications, and farms."@en .

# Classes with Deep Hierarchy
beans:Bean a owl:Class ;
    owl:equivalentClass coffee:CoffeeBean ;
    rdfs:label "Bean"@en ;
    rdfs:comment "A class representing coffee beans, equivalent to coffee:CoffeeBean."@en .

beans:Variety a owl:Class ;
    rdfs:label "Variety"@en ;
    rdfs:comment "A class representing genetic varieties of coffee beans."@en .

beans:ArabicaVariety a owl:Class ;
    rdfs:subClassOf beans:Variety ;
    rdfs:label "Arabica Variety"@en ;
    rdfs:comment "Varieties of Arabica beans."@en .

beans:TypicaVariety a owl:Class ;
    rdfs:subClassOf beans:ArabicaVariety ;
    rdfs:label "Typica Variety"@en ;
    rdfs:comment "The Typica variety of Arabica beans."@en .

beans:BourbonVariety a owl:Class ;
    rdfs:subClassOf beans:ArabicaVariety ;
    rdfs:label "Bourbon Variety"@en ;
    rdfs:comment "The Bourbon variety of Arabica beans."@en .

beans:Certification a owl:Class ;
    rdfs:label "Certification"@en ;
    rdfs:comment "A class representing certifications for coffee beans (e.g., Organic)."@en .

beans:Farm a owl:Class ;
    rdfs:label "Farm"@en ;
    rdfs:comment "A class representing farms where coffee beans are grown."@en .

# Disjoint Classes
[] a owl:AllDisjointClasses ;
    owl:members (beans:Bean beans:Variety beans:Certification beans:Farm) .

# Object Properties
beans:hasVariety a owl:ObjectProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range beans:Variety ;
    rdfs:label "has Variety"@en ;
    rdfs:comment "Links a bean to its genetic variety."@en .

beans:hasCertification a owl:ObjectProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range beans:Certification ;
    rdfs:label "has Certification"@en ;
    rdfs:comment "Links a bean to its certification status."@en .

beans:grownOn a owl:ObjectProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range beans:Farm ;
    rdfs:label "grown On"@en ;
    rdfs:comment "Links a bean to the farm where it was grown."@en .

# Data Properties
beans:hasAltitude a owl:DatatypeProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range xsd:integer ;
    rdfs:label "has Altitude"@en ;
    rdfs:comment "Specifies the growing altitude of a bean in meters."@en .

beans:hasHarvestYear a owl:DatatypeProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range xsd:integer ;
    rdfs:label "has Harvest Year"@en ;
    rdfs:comment "Specifies the harvest year of a bean."@en .

beans:hasPricePerKg a owl:DatatypeProperty ;
    rdfs:domain beans:Bean ;
    rdfs:range xsd:decimal ;
    rdfs:label "has Price Per Kg"@en ;
    rdfs:comment "Specifies the price per kilogram of a bean in USD."@en .

# Individuals
beans:YirgacheffeBean a beans:Bean, coffee:EthiopianTypica ;
    rdfs:label "Yirgacheffe Bean"@en ;
    beans:hasVariety beans:TypicaVariety ;
    beans:hasCertification beans:Organic ;
    beans:grownOn beans:FincaLaEsperanza ;
    coffee:hasFlavor coffee:Fruity, coffee:Floral ;
    coffee:hasRoast coffee:LightRoast ;
    coffee:hasOrigin coffee:Ethiopia ;
    coffee:processedBy coffee:Washed ;
    beans:hasAltitude 1800^^xsd:integer ;
    beans:hasHarvestYear 2023^^xsd:integer ;
    beans:hasPricePerKg 20.0^^xsd:decimal ;
    coffee:hasAcidityLevel "High" ;
    coffee:hasBody "Light" .

beans:BourbonBean a beans:Bean, coffee:Bourbon ;
    rdfs:label "Bourbon Bean"@en ;
    beans:hasVariety beans:BourbonVariety ;
    beans:hasCertification beans:FairTrade ;
    beans:grownOn beans:HaciendaElRoble ;
    coffee:hasFlavor coffee:Chocolatey ;
    coffee:hasRoast coffee:MediumRoast ;
    coffee:hasOrigin coffee:Colombia ;
    coffee:processedBy coffee:Washed ;
    beans:hasAltitude 1500^^xsd:integer ;
    beans:hasHarvestYear 2023^^xsd:integer ;
    beans:hasPricePerKg 18.0^^xsd:decimal ;
    coffee:hasAcidityLevel "Medium" ;
    coffee:hasBody "Medium" .

beans:GeishaBean a beans:Bean ;
    rdfs:label "Geisha Bean"@en ;
    beans:hasVariety beans:GeishaVariety ;
    beans:hasCertification beans:RainforestAlliance ;
    beans:grownOn beans:FincaLaEsperanza ;
    coffee:hasFlavor coffee:Floral ;
    coffee:hasRoast coffee:LightRoast ;
    coffee:hasOrigin coffee:Panama ;
    coffee:processedBy coffee:Washed ;
    beans:hasAltitude 2000^^xsd:integer ;
    beans:hasHarvestYear 2023^^xsd:integer ;
    beans:hasPricePerKg 30.0^^xsd:decimal ;
    coffee:hasAcidityLevel "High" ;
    coffee:hasBody "Light" .

beans:TypicaVariety a beans:Variety ;
    rdfs:label "Typica Variety"@en ;
    rdfs:comment "A variety of Arabica beans known for balance."@en .

beans:BourbonVariety a beans:Variety ;
    rdfs:label "Bourbon Variety"@en ;
    rdfs:comment "A variety of Arabica beans known for sweetness."@en .

beans:GeishaVariety a beans:Variety ;
    rdfs:label "Geisha Variety"@en ;
    rdfs:comment "A variety of Arabica beans known for complex flavors."@en .

beans:Organic a beans:Certification ;
    rdfs:label "Organic Certification"@en ;
    rdfs:comment "Indicates organic farming practices."@en .

beans:FairTrade a beans:Certification ;
    rdfs:label "FairTrade Certification"@en ;
    rdfs:comment "Indicates fair trade practices."@en .

beans:RainforestAlliance a beans:Certification ;
    rdfs:label "Rainforest Alliance Certification"@en ;
    rdfs:comment "Indicates sustainable farming practices."@en .

beans:FincaLaEsperanza a beans:Farm ;
    rdfs:label "Finca La Esperanza"@en ;
    rdfs:comment "A farm in Ethiopia growing high-quality beans."@en .

beans:HaciendaElRoble a beans:Farm ;
    rdfs:label "Hacienda El Roble"@en ;
    rdfs:comment "A farm in Colombia growing premium beans."@en .
```

### 3. Updated Shops.ttl
#### Changes
- **Hierarchy**:
  - `CoffeeShop`: Added `PhysicalShop`, `OnlineShop`.
  - `Staff`: Added `Barista`, `Manager`.
- **Restrictions**: Added `rdfs:domain`/`rdfs:range`.
- **Annotations**: `rdfs:label`/`rdfs:comment` for all elements.
- **Individuals**: Added `BeanOnline`, `CustomerBob`.
- **Datatypes**:
  - `hasPrice`: `xsd:decimal` (e.g., `3.5^^xsd:decimal`).
  - `hasRating`: `xsd:integer` (e.g., `4^^xsd:integer`).
  - `hasAddress`, `hasOpeningHours`: Simplified `xsd:string`.
  - `hasOrderDate`: `xsd:date`.

#### Updated TTL
```turtle
@prefix coffee: <http://www.example.org/coffee#> .
@prefix beans: <http://www.example.org/beans#> .
@prefix shops: <http://www.example.org/shops#> .
@prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#> .
@prefix owl: <http://www.w3.org/2002/07/owl#> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .

# Ontology Declaration
shops: a owl:Ontology ;
    rdfs:label "Shops Ontology"@en ;
    rdfs:comment "An ontology for coffee shops, including menus, staff, and customer interactions."@en .

# Classes with Deep Hierarchy
shops:CoffeeShop a owl:Class ;
    rdfs:label "Coffee Shop"@en ;
    rdfs:comment "A class representing coffee shops or cafes."@en .

shops:PhysicalShop a owl:Class ;
    rdfs:subClassOf shops:CoffeeShop ;
    rdfs:label "Physical Shop"@en ;
    rdfs:comment "A coffee shop with a physical location."@en .

shops:OnlineShop a owl:Class ;
    rdfs:subClassOf shops:CoffeeShop ;
    rdfs:label "Online Shop"@en ;
    rdfs:comment "A coffee shop operating online."@en .

shops:MenuItem a owl:Class ;
    rdfs:label "Menu Item"@en ;
    rdfs:comment "A class representing items on a coffee shop's menu."@en .

shops:Staff a owl:Class ;
    rdfs:label "Staff"@en ;
    rdfs:comment "A class representing employees of a coffee shop."@en .

shops:Barista a owl:Class ;
    rdfs:subClassOf shops:Staff ;
    rdfs:label "Barista"@en ;
    rdfs:comment "A staff member who prepares coffee drinks."@en .

shops:Manager a owl:Class ;
    rdfs:subClassOf shops:Staff ;
    rdfs:label "Manager"@en ;
    rdfs:comment "A staff member who manages the shop."@en .

shops:Customer a owl:Class ;
    rdfs:label "Customer"@en ;
    rdfs:comment "A class representing patrons of a coffee shop."@en .

shops:Location a owl:Class ;
    rdfs:label "Location"@en ;
    rdfs:comment "A class representing the geographic location of a shop."@en .

shops:Order a owl:Class ;
    rdfs:label "Order"@en ;
    rdfs:comment "A class representing customer orders."@en .

shops:Review a owl:Class ;
    rdfs:label "Review"@en ;
    rdfs:comment "A class representing customer feedback or ratings."@en .

# Disjoint Classes
[] a owl:AllDisjointClasses ;
    owl:members (shops:CoffeeShop shops:MenuItem shops:Staff shops:Customer shops:Location shops:Order shops:Review) .

# Object Properties
shops:sells a owl:ObjectProperty ;
    rdfs:domain shops:CoffeeShop ;
    rdfs:range shops:MenuItem ;
    rdfs:label "sells"@en ;
    rdfs:comment "Links a coffee shop to the menu items it sells."@en .

shops:usesBean a owl:ObjectProperty ;
    rdfs:domain shops:MenuItem ;
    rdfs:range beans:Bean ;
    rdfs:label "uses Bean"@en ;
    rdfs:comment "Links a menu item to the bean used in its preparation."@en .

shops:employs a owl:ObjectProperty ;
    rdfs:domain shops:CoffeeShop ;
    rdfs:range shops:Staff ;
    rdfs:label "employs"@en ;
    rdfs:comment "Links a coffee shop to its staff."@en .

shops:visitedBy a owl:ObjectProperty ;
    rdfs:domain shops:CoffeeShop ;
    rdfs:range shops:Customer ;
    rdfs:label "visited By"@en ;
    rdfs:comment "Links a coffee shop to its customers."@en .

shops:locatedIn a owl:ObjectProperty ;
    rdfs:domain shops:CoffeeShop ;
    rdfs:range shops:Location ;
    rdfs:label "located In"@en ;
    rdfs:comment "Links a coffee shop to its geographic location."@en .

shops:placesOrder a owl:ObjectProperty ;
    rdfs:domain shops:Customer ;
    rdfs:range shops:Order ;
    rdfs:label "places Order"@en ;
    rdfs:comment "Links a customer to their order."@en .

shops:containsItem a owl:ObjectProperty ;
    rdfs:domain shops:Order ;
    rdfs:range shops:MenuItem ;
    rdfs:label "contains Item"@en ;
    rdfs:comment "Links an order to the menu items it includes."@en .

shops:hasReview a owl:ObjectProperty ;
    rdfs:domain [ a owl:Class ; owl:unionOf (shops:CoffeeShop shops:MenuItem) ] ;
    rdfs:range shops:Review ;
    rdfs:label "has Review"@en ;
    rdfs:comment "Links a shop or menu item to customer reviews."@en .

# Data Properties
shops:hasPrice a owl:DatatypeProperty ;
    rdfs:domain shops:MenuItem ;
    rdfs:range xsd:decimal ;
    rdfs:label "has Price"@en ;
    rdfs:comment "Specifies the price of a menu item in USD."@en .

shops:hasRating a owl:DatatypeProperty ;
    rdfs:domain shops:Review ;
    rdfs:range xsd:integer ;
    rdfs:label "has Rating"@en ;
    rdfs:comment "Specifies the rating (1-5 stars) of a review."@en .

shops:hasAddress a owl:DatatypeProperty ;
    rdfs:domain shops:Location ;
    rdfs:range xsd:string ;
    rdfs:label "has Address"@en ;
    rdfs:comment "Specifies the address of a location."@en .

shops:hasOpeningHours a owl:DatatypeProperty ;
    rdfs:domain shops:CoffeeShop ;
    rdfs:range xsd:string ;
    rdfs:label "has Opening Hours"@en ;
    rdfs:comment "Specifies the operating hours of a coffee shop."@en .

shops:hasOrderDate a owl:DatatypeProperty ;
    rdfs:domain shops:Order ;
    rdfs:range xsd:date ;
    rdfs:label "has Order Date"@en ;
    rdfs:comment "Specifies the date of an order."@en .

# Individuals
shops:DowntownBrew a shops:PhysicalShop ;
    rdfs:label "Downtown Brew"@en ;
    shops:sells shops:EspressoItem, shops:CappuccinoItem ;
    shops:employs shops:BaristaJohn ;
    shops:visitedBy shops:CustomerAlice ;
    shops:locatedIn shops:SeattleLocation ;
    shops:hasReview shops:Review001 ;
    shops:hasOpeningHours "7 AM - 6 PM" .

shops:ArtisanRoast a shops:PhysicalShop ;
    rdfs:label "Artisan Roast"@en ;
    shops:sells shops:PourOverItem ;
    shops:employs shops:ManagerJane ;
    shops:visitedBy shops:CustomerBob ;
    shops:locatedIn shops:PortlandLocation ;
    shops:hasReview shops:Review002 ;
    shops:hasOpeningHours "8 AM - 5 PM" .

shops:BeanOnline a shops:OnlineShop ;
    rdfs:label "Bean Online"@en ;
    shops:sells shops:EspressoItem ;
    shops:employs shops:ManagerJane ;
    shops:visitedBy shops:CustomerAlice ;
    shops:hasOpeningHours "24/7" .

shops:EspressoItem a shops:MenuItem, coffee:EspressoBasedDrink ;
    rdfs:label "Espresso Item"@en ;
    shops:usesBean beans:YirgacheffeBean ;
    coffee:hasFlavor coffee:Chocolatey ;
    coffee:brewedWith coffee:EspressoMachine ;
    shops:hasPrice 3.5^^xsd:decimal .

shops:CappuccinoItem a shops:MenuItem, coffee:EspressoBasedDrink ;
    rdfs:label "Cappuccino Item"@en ;
    shops:usesBean beans:BourbonBean ;
    coffee:hasFlavor coffee:Chocolatey ;
    coffee:brewedWith coffee:EspressoMachine ;
    shops:hasPrice 4.5^^xsd:decimal .

shops:PourOverItem a shops:MenuItem, coffee:FilteredDrink ;
    rdfs:label "Pour Over Item"@en ;
    shops:usesBean beans:GeishaBean ;
    coffee:hasFlavor coffee:Floral ;
    coffee:brewedWith coffee:PourOver ;
    shops:hasPrice 4.0^^xsd:decimal .

shops:BaristaJohn a shops:Barista ;
    rdfs:label "Barista John"@en ;
    rdfs:comment "A skilled barista at Downtown Brew."@en .

shops:ManagerJane a shops:Manager ;
    rdfs:label "Manager Jane"@en ;
    rdfs:comment "A manager at Artisan Roast and Bean Online."@en .

shops:CustomerAlice a shops:Customer ;
    rdfs:label "Customer Alice"@en ;
    shops:placesOrder shops:Order123 .

shops:CustomerBob a shops:Customer ;
    rdfs:label "Customer Bob"@en ;
    shops:placesOrder shops:Order124 .

shops:SeattleLocation a shops:Location ;
    rdfs:label "Seattle Location"@en ;
    shops:hasAddress "123 Coffee St, Seattle, WA" .

shops:PortlandLocation a shops:Location ;
    rdfs:label "Portland Location"@en ;
    shops:hasAddress "456 Brew Ave, Portland, OR" .

shops:Order123 a shops:Order ;
    rdfs:label "Order 123"@en ;
    shops:containsItem shops:EspressoItem ;
    shops:hasOrderDate "2025-04-28"^^xsd:date .

shops:Order124 a shops:Order ;
    rdfs:label "Order 124"@en ;
    shops:containsItem shops:PourOverItem ;
    shops:hasOrderDate "2025-04-28"^^xsd:date .

shops:Review001 a shops:Review ;
    rdfs:label "Review 001"@en ;
    shops:hasRating 4^^xsd:integer ;
    rdfs:comment "Great coffee and service!"@en .

shops:Review002 a shops:Review ;
    rdfs:label "Review 002"@en ;
    shops:hasRating 5^^xsd:integer ;
    rdfs:comment "Amazing pour-over experience!"@en .
```

## GraphDB Use Cases

The corrected ontologies form a robust knowledge graph for GraphDB, supporting:

1. **Supply Chain Transparency**:
   - **Query**: “Which shops sell drinks made from Organic beans grown above 1500 meters in Ethiopia?”
   - **SPARQL**:
     ```sparql
     SELECT ?shop ?bean ?farm
     WHERE {
         ?shop shops:sells ?item .
         ?item shops:usesBean ?bean .
         ?bean beans:hasCertification beans:Organic ;
               beans:hasAltitude ?altitude ;
               coffee:hasOrigin coffee:Ethiopia ;
               beans:grownOn ?farm .
         FILTER (?altitude > 1500)
     }
     ```
   - **Value**: Enhances transparency, supports ethical sourcing.

2. **Personalized Recommendations**:
   - **Query**: “Suggest drinks for a customer preferring Fruity flavors and Organic beans.”
   - **SPARQL**:
     ```sparql
     SELECT ?item ?shop
     WHERE {
         ?shop shops:sells ?item .
         ?item shops:usesBean ?bean ;
               coffee:hasFlavor coffee:Fruity .
         ?bean beans:hasCertification beans:Organic .
     }
     ```
   - **Value**: Increases customer satisfaction.

3. **Customer Sentiment Analysis**:
   - **Query**: “Which shops have high ratings for drinks made with LightRoast beans?”
   - **SPARQL**:
     ```sparql
     SELECT ?shop ?rating
     WHERE {
         ?shop shops:sells ?item ;
               shops:hasReview ?review .
         ?item shops:usesBean ?bean .
         ?bean coffee:hasRoast coffee:LightRoast .
         ?review shops:hasRating ?rating .
         FILTER (?rating >= 4)
     }
     ```
   - **Value**: Informs marketing/quality improvements.

4. **Inventory Optimization**:
   - **Query**: “Which beans are used in high-selling menu items?”
   - **SPARQL**:
     ```sparql
     SELECT ?bean (COUNT(?order) as ?orderCount)
     WHERE {
         ?order shops:containsItem ?item .
         ?item shops:usesBean ?bean .
     }
     GROUP BY ?bean
     ORDER BY DESC(?orderCount)
     ```
   - **Value**: Reduces waste, optimizes procurement.

5. **Market Analysis**:
   - **Query**: “Which locations have high traffic for shops selling Colombian beans?”
   - **SPARQL**:
     ```sparql
     SELECT ?location (COUNT(?customer) as ?traffic)
     WHERE {
         ?shop shops:locatedIn ?location ;
               shops:visitedBy ?customer ;
               shops:sells ?item .
         ?item shops:usesBean ?bean .
         ?bean coffee:hasOrigin coffee:Colombia .
     }
     GROUP BY ?location
     ORDER BY DESC(?traffic)
     ```
   - **Value**: Guides expansion strategies.

### Why GraphDB?
- **Complex Data**: Manages interconnected classes/properties.
- **Implicit Knowledge**: SPARQL/reasoning uncovers insights.
- **Scalability**: Handles large datasets.
- **Flexibility**: RDF/OWL allows extensions.

## Conclusion

The corrected `coffee.ttl`, `Beans.ttl`, and `Shops.ttl` ontologies address original limitations and datatype errors by:
- **Deep Hierarchies**: Added subclasses (e.g., `EthiopianTypica`, `PhysicalShop`).
- **Restrictions**: OWL constraints ensure consistency.
- **Annotations**: `rdfs:label`/`rdfs:comment` improve accessibility.
- **Individuals**: More instances enhance applicability.
- **Datatypes**: Corrected XSD usage (e.g., `xsd:integer` for whole numbers).

These enhancements create a robust knowledge graph for GraphDB, supporting supply chain tracing, recommendations, and market analysis. The corrected datatypes ensure compatibility with semantic tools, making the ontologies a strong example of GraphDB’s value in handling complex, interconnected data in the coffee industry.

