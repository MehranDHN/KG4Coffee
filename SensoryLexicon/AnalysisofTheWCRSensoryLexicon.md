# Analysis of the WCR Sensory Lexicon and Its Relation to Coffee Ontologies

This document analyzes the World Coffee Research (WCR) Sensory Lexicon, a standardized framework for evaluating coffee flavor, aroma, and texture, and explores its integration with the corrected coffee ontologies (`coffee.ttl`, `Beans.ttl`, and `Shops.ttl`) from the coffee business domain, hosted at [KG4Coffee](https://github.com/MehranDHN/KG4Coffee). The lexicon, developed by WCR and Kansas State University’s Sensory Analysis Center, identifies 110 sensory attributes with references and intensity scores, aiming to advance coffee quality understanding. By aligning the lexicon with the ontologies, we can enhance their sensory modeling, enabling richer knowledge graphs in GraphDB for applications like supply chain transparency, personalized recommendations, and quality research.

## Overview of the WCR Sensory Lexicon

### Description
- **Source**: World Coffee Research (WCR), Sensory Analysis Center at Kansas State University.
- **Publication**: First edition (2016), updated to version 2.0 (2017).
- **Content**: Defines 110 sensory attributes (flavor, aroma, texture) in coffee, each with:
  - **Definition**: Describes the attribute (e.g., “blackberry” as “sweet, dark, fruity, floral, slightly sour, somewhat woody aromatic”).
  - **References**: Standardized items (e.g., Smucker’s Blackberry Jam for blackberry flavor) to benchmark intensity.
  - **Intensity Score**: Rated on a 1–15 scale (e.g., blackberry flavor intensity of 5.5 for the reference).
  - **Preparation Instructions**: Ensures consistent evaluation (e.g., covered glass snifters for aroma).
- **Version 2.0 Updates**: Added 24 FlavorActiV references (pharmaceutical-grade flavor capsules) for global applicability, addressing U.S.-centric grocery store references.
- **Purpose**:
  - Standardize coffee sensory evaluation with a universal “dictionary.”
  - Enable replicable, data-driven quality assessment for research and industry.
  - Link sensory attributes to chemical, genetic, and environmental factors to improve coffee quality.
- **Applications**:
  - Foundation for the Specialty Coffee Association (SCA) Coffee Taster’s Flavor Wheel, used by cuppers and Q-graders.
  - Supports research on coffee varieties, processing, roasting, and brewing.
  - Enhances quality control, cupping calibration, and consumer communication.

### Key Features
1. **Comprehensive Attributes**:
   - Includes 110 attributes: flavors (e.g., fruity, nutty), aromas (e.g., floral, smoky), textures (e.g., syrupy, creamy).
   - Organized hierarchically, with parent categories (e.g., “Fruity”) and specific descriptors (e.g., “Blackberry”).
   - Later additions include hazelnut, pineapple, chamomile.
2. **Quantitative Measurement**:
   - 1–15 intensity scale ensures precise, replicable scoring (e.g., “blueberry, flavor: 4”).
   - References (e.g., pineapple juice) standardize evaluations globally.
3. **Global Applicability**:
   - FlavorActiV capsules (e.g., for sour, bitter) are shelf-stable and accessible worldwide.
   - Acts as a “Rosetta Stone” for coffee sensory language.
4. **Living Document**:
   - Open to new attributes (e.g., pitanga, cupuaçu) via user contributions.
   - Plans for sub-lexicons (e.g., espresso, varietal evaluation).
5. **Scientific Foundation**:
   - Developed by sensory scientists and coffee professionals, evaluating 100+ samples from 14 countries.
   - Links sensory data to molecular and environmental analysis.

### Strengths
- **Standardization**: Reduces subjectivity with a shared vocabulary and methodology.
- **Precision**: Intensity scores and references enable objective analysis.
- **Research Enablement**: Facilitates studies on quality origins (genetics, processing).
- **Industry Impact**: Adopted by SCA, Blue Bottle, and others for cupping and quality control.
- **Global Reach**: Version 2.0’s universal references ensure accessibility.

### Limitations
- **Complexity**: 110 attributes require training for effective use.
- **Initial U.S. Bias**: First edition’s grocery references were U.S.-centric (mitigated in version 2.0).
- **Incomplete Coverage**: Lacks some regional flavors (e.g., Brazilian pitanga), though expandable.
- **Resource Intensity**: References and trained panelists may be costly for small producers.
- **Consumer Relevance**: Technical terms (e.g., “bright acidity”) may not align with consumer descriptors (e.g., “pineapple”).

### Significance
The WCR Sensory Lexicon transforms coffee sensory evaluation into a scientific discipline, bridging subjective tasting and objective analysis. It enables industry collaboration, quality improvement, and consumer trust through consistent flavor descriptions. Its integration with the SCA Flavor Wheel and ongoing updates make it a cornerstone of coffee research and practice.

## Relation to Coffee Ontologies

The WCR Sensory Lexicon enhances the ontologies’ sensory modeling, addressing limitations in `coffee.ttl`, `Beans.ttl`, and `Shops.ttl` (corrected for XSD datatypes). Below, I analyze alignment, propose enhancements, and explore improved GraphDB applications.

### Alignment with Ontologies
The ontologies model sensory attributes via `coffee:Flavor` and `coffee:hasFlavor`, with individuals like `Fruity`, `Chocolatey`, `Floral`, and `Nutty`. Alignment with the lexicon includes:
1. **Flavor Representation**:
   - **Ontology**: `coffee:Flavor` includes broad descriptors (e.g., `Fruity`), with `hasFlavor` linking beans/drinks (e.g., `coffee:EthiopianTypicaBean` has `Fruity`, `Floral`).
   - **Lexicon**: Offers parent categories (e.g., “Fruity”) and specific attributes (e.g., “Blackberry”, “Pineapple”), with definitions and intensity scores.
   - **Synergy**: The ontology’s `Flavor` class can adopt the lexicon’s hierarchy, adding specific attributes as subclasses/individuals.
2. **Aroma and Texture**:
   - **Ontology**: Lacks aroma/texture classes, but `hasAcidityLevel` and `hasBody` (e.g., “High”, “Light”) address sensory qualities.
   - **Lexicon**: Includes aroma (e.g., jasmine, smoky) and texture (e.g., syrupy, creamy) attributes with references.
   - **Synergy**: New `Aroma` and `Texture` classes can capture these dimensions.
3. **Quantitative Aspects**:
   - **Ontology**: Sensory properties are qualitative (e.g., `hasAcidityLevel "High"`), but `hasCaffeineContent` uses `xsd:decimal`.
   - **Lexicon**: Uses a 1–15 intensity scale for all attributes.
   - **Synergy**: A `hasIntensity` property can add numerical scores to sensory attributes.
4. **References and Standards**:
   - **Ontology**: No flavor standards.
   - **Lexicon**: Provides references (e.g., Smucker’s Blackberry Jam, FlavorActiV capsules).
   - **Synergy**: A `hasReference` property can link flavors to lexicon standards.

### Proposed Ontology Enhancements
To integrate the lexicon, the ontologies can be updated to model its 110 attributes, intensity scores, and references, addressing limitations (shallow flavor hierarchy, no aroma/texture, qualitative data). Enhancements include:

1. **Expand Flavor Hierarchy**:
   - **Current**: Flat `coffee:Flavor` individuals (e.g., `Fruity`, `Floral`).
   - **Proposed**: Deep hierarchy mirroring the lexicon:
     ```turtle
     coffee:Flavor a owl:Class ;
         rdfs:label "Flavor"@en ;
         rdfs:comment "Sensory flavor attributes of coffee, per WCR Sensory Lexicon."@en .
     coffee:Fruity a owl:Class ;
         rdfs:subClassOf coffee:Flavor ;
         rdfs:label "Fruity Flavor"@en .
     coffee:Blackberry a owl:Class ;
         rdfs:subClassOf coffee:Fruity ;
         rdfs:label "Blackberry Flavor"@en ;
         rdfs:comment "Sweet, dark, fruity, floral, slightly sour flavor, per WCR Lexicon."@en .
     coffee:Pineapple a owl:Class ;
         rdfs:subClassOf coffee:Fruity ;
         rdfs:label "Pineapple Flavor"@en ;
         rdfs:comment "Sweet, tropical, tangy flavor, per WCR Lexicon."@en .
     ```
   - Add attributes like `Hazelnut`, `Chamomile`, `Pomegranate`.
   - **Benefit**: Enables queries like “Drinks with blackberry flavor.”

2. **Add Aroma and Texture Classes**:
   - **Current**: No aroma/texture modeling.
   - **Proposed**: New classes and properties:
     ```turtle
     coffee:Aroma a owl:Class ;
         rdfs:label "Aroma"@en ;
         rdfs:comment "Sensory aroma attributes of coffee, per WCR Sensory Lexicon."@en .
     coffee:Texture a owl:Class ;
         rdfs:label "Texture"@en ;
         rdfs:comment "Sensory texture attributes of coffee, per WCR Sensory Lexicon."@en .
     coffee:hasAroma a owl:ObjectProperty ;
         rdfs:domain [ a owl:Class ; owl:unionOf (coffee:CoffeeBean coffee:CoffeeDrink) ] ;
         rdfs:range coffee:Aroma ;
         rdfs:label "has Aroma"@en ;
         rdfs:comment "Links a bean or drink to its aroma profile."@en .
     coffee:hasTexture a owl:ObjectProperty ;
         rdfs:domain coffee:CoffeeDrink ;
         rdfs:range coffee:Texture ;
         rdfs:label "has Texture"@en ;
         rdfs:comment "Links a drink to its texture profile."@en .
     coffee:Jasmine a coffee:Aroma ;
         rdfs:label "Jasmine Aroma"@en ;
         rdfs:comment "Floral, sweet aroma, per WCR Lexicon."@en .
     coffee:Syrupy a coffee:Texture ;
         rdfs:label "Syrupy Texture"@en ;
         rdfs:comment "Thick, viscous texture, per WCR Lexicon."@en .
     ```
   - **Benefit**: Captures lexicon’s full sensory scope for queries like “Drinks with jasmine aroma and syrupy texture.”

3. **Incorporate Intensity Scores**:
   - **Current**: Qualitative sensory data.
   - **Proposed**: Add intensity property:
     ```turtle
     coffee:hasIntensity a owl:DatatypeProperty ;
         rdfs:domain [ a owl:Class ; owl:unionOf (coffee:Flavor coffee:Aroma coffee:Texture) ] ;
         rdfs:range xsd:decimal ;
         rdfs:label "has Intensity"@en ;
         rdfs:comment "Intensity score (1-15) for a sensory attribute, per WCR Sensory Lexicon."@en .
     ```
   - Example: `coffee:EthiopianTypicaBean coffee:hasFlavor coffee:Blackberry ; coffee:hasIntensity 5.5^^xsd:decimal .`
   - **Benefit**: Enables quantitative queries (e.g., “Beans with fruity flavor intensity > 5”).

4. **Model References**:
   - **Current**: No flavor standards.
   - **Proposed**: Add reference class and property:
     ```turtle
     coffee:SensoryReference a owl:Class ;
         rdfs:label "Sensory Reference"@en ;
         rdfs:comment "Standard reference for a sensory attribute, per WCR Sensory Lexicon."@en .
     coffee:hasReference a owl:ObjectProperty ;
         rdfs:domain [ a owl:Class ; owl:unionOf (coffee:Flavor coffee:Aroma coffee:Texture) ] ;
         rdfs:range coffee:SensoryReference ;
         rdfs:label "has Reference"@en ;
         rdfs:comment "Links a sensory attribute to its WCR Lexicon reference."@en .
     coffee:SmuckersBlackberryJam a coffee:SensoryReference ;
         rdfs:label "Smucker's Blackberry Jam"@en ;
         rdfs:comment "Reference for blackberry flavor, per WCR Lexicon."@en .
     coffee:Blackberry coffee:hasReference coffee:SmuckersBlackberryJam .
     ```
   - **Benefit**: Ensures industry-standard traceability.

5. **Enhance Individuals**:
   - **Current**: Limited flavors (e.g., `Fruity`, `Chocolatey`).
   - **Proposed**: Add lexicon attributes and link to beans/drinks:
     ```turtle
     coffee:EthiopianTypicaBean a coffee:EthiopianTypica ;
         coffee:hasFlavor coffee:Blackberry ;
         coffee:hasIntensity 5.5^^xsd:decimal ;
         coffee:hasAroma coffee:Jasmine ;
         coffee:hasIntensity 4.0^^xsd:decimal ;
         coffee:hasReference coffee:SmuckersBlackberryJam .
     coffee:Espresso a coffee:EspressoBasedDrink ;
         coffee:hasFlavor coffee:Pineapple ;
         coffee:hasIntensity 4.5^^xsd:decimal ;
         coffee:hasTexture coffee:Syrupy ;
         coffee:hasIntensity 6.0^^xsd:decimal .
     ```
   - **Benefit**: Enriches sensory data for analysis.

### Enhanced GraphDB Use Cases
Integrating the lexicon into the ontologies enhances the GraphDB use cases from the ontologies document, leveraging the lexicon’s precision. Updated use cases:

1. **Supply Chain Transparency**:
   - **Original**: “Shops selling drinks from Organic Ethiopian beans grown above 1500 meters.”
   - **Enhanced**: “Shops selling drinks with blackberry flavor (intensity > 5) from Organic Ethiopian beans.”
   - **SPARQL**:
     ```sparql
     SELECT ?shop ?bean ?flavor ?intensity
     WHERE {
         ?shop shops:sells ?item .
         ?item shops:usesBean ?bean .
         ?bean beans:hasCertification beans:Organic ;
               beans:hasAltitude ?altitude ;
               coffee:hasOrigin coffee:Ethiopia ;
               coffee:hasFlavor ?flavor ;
               coffee:hasIntensity ?intensity .
         ?flavor rdfs:subClassOf* coffee:Fruity .
         FILTER (?altitude > 1500 && ?intensity > 5 && ?flavor = coffee:Blackberry)
     }
     ```
   - **Benefit**: Precise sourcing with lexicon attributes.

2. **Personalized Recommendations**:
   - **Original**: “Drinks for customers preferring Fruity flavors and Organic beans.”
   - **Enhanced**: “Drinks with pineapple flavor (intensity > 4) and syrupy texture from Organic beans.”
   - **SPARQL**:
     ```sparql
     SELECT ?item ?shop ?flavor ?texture
     WHERE {
         ?shop shops:sells ?item .
         ?item shops:usesBean ?bean ;
               coffee:hasFlavor ?flavor ;
               coffee:hasTexture ?texture ;
               coffee:hasIntensity ?intensity .
         ?bean beans:hasCertification beans:Organic .
         FILTER (?flavor = coffee:Pineapple && ?texture = coffee:Syrupy && ?intensity > 4)
     }
     ```
   - **Benefit**: Tailored recommendations using lexicon’s granularity.

3. **Customer Sentiment Analysis**:
   - **Original**: “Shops with high ratings for drinks with LightRoast beans.”
   - **Enhanced**: “Shops with ratings ≥ 4 for drinks with floral aroma (intensity > 3) from LightRoast beans.”
   - **SPARQL**:
     ```sparql
     SELECT ?shop ?rating ?aroma
     WHERE {
         ?shop shops:sells ?item ;
               shops:hasReview ?review .
         ?item shops:usesBean ?bean ;
               coffee:hasAroma ?aroma ;
               coffee:hasIntensity ?intensity .
         ?bean coffee:hasRoast coffee:LightRoast .
         ?review shops:hasRating ?rating .
         FILTER (?rating >= 4 && ?aroma = coffee:Jasmine && ?intensity > 3)
     }
     ```
   - **Benefit**: Nuanced analysis with aroma and intensity.

4. **Quality Control and Research**:
   - **New**: “Beans with high-intensity chocolatey flavor linked to farms and processing.”
   - **SPARQL**:
     ```sparql
     SELECT ?bean ?farm ?process ?intensity
     WHERE {
         ?bean coffee:hasFlavor coffee:Chocolatey ;
               coffee:hasIntensity ?intensity ;
               beans:grownOn ?farm ;
               coffee:processedBy ?process .
         FILTER (?intensity > 7)
     }
     ```
   - **Benefit**: Links sensory data to quality factors, aligning with lexicon’s research goals.

### Implementation Steps
1. **Update `coffee.ttl`**:
   - Add `Aroma`, `Texture`, `SensoryReference` classes.
   - Add `hasAroma`, `hasTexture`, `hasIntensity`, `hasReference` properties.
   - Expand `Flavor` hierarchy with lexicon attributes.
2. **Enhance Individuals**:
   - Update beans/drinks with lexicon attributes, intensities, and references.
3. **Import into GraphDB**:
   - Load updated TTL files.
   - Use OWL reasoning for subclass inferences.
4. **Develop Queries**:
   - Create SPARQL queries for lexicon-based use cases.
5. **Validate with Lexicon**:
   - Ensure ontology attributes cover the lexicon’s 110 attributes and references.

### Challenges
- **Complexity**: 110 attributes increase ontology size, potentially impacting GraphDB performance.
- **Training**: Users need training to apply lexicon standards (e.g., 1–15 scale).
- **Regional Gaps**: Lexicon may lack region-specific flavors (e.g., cupuaçu), requiring contributions.
- **Consumer Alignment**: Technical terms need translation for consumer marketing (e.g., “blackberry intensity 5.5” to “berry-like”).

## Conclusion
The WCR Sensory Lexicon provides a scientifically validated framework for coffee sensory evaluation, with 110 attributes, intensity scores, and references that enhance the ontologies’ sensory modeling. By integrating the lexicon’s hierarchy, aromas, textures, and quantitative measures, the ontologies can:
- Model specific attributes (e.g., blackberry, jasmine, syrupy).
- Enable precise, data-driven analysis with intensity scores.
- Align with industry standards via references.
- Amplify GraphDB applications like recommendations and quality research.

The enhanced ontologies create a robust knowledge graph for GraphDB, supporting precise queries (e.g., drinks with high-intensity fruity flavors) and insights for coffee businesses. Balancing complexity and usability is key to ensuring the ontologies remain practical for professionals and consumers.

### Recommendations
1. **Update Ontologies**: Implement proposed classes, properties, and lexicon attributes.
2. **Pilot in GraphDB**: Test with sample data and SPARQL queries.
3. **Engage with WCR**: Propose regional attributes (e.g., pitanga) to the lexicon.
4. **Consumer Translation**: Map lexicon terms to consumer-friendly descriptors.
5. **Training Support**: Provide guidelines for lexicon standards.
