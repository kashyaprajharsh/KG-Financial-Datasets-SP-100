# Financial Knowledge Graph Dataset: S&P 100 Companies (2024)

A comprehensive knowledge graph dataset extracted from S&P 100 companies' 10-K filings for 2024, processed using the Qwen2.5-72B-Instruct model with three different extraction methodologies.

> **🚀 Quick Start**: Download the [compressed dataset (90MB)] and extract to get started immediately!

## 📊 Dataset Overview

This dataset contains structured knowledge graphs extracted from 101 S&P 100 companies' SEC 10-K filings for the year 2024. The knowledge graphs are represented as triplets (subject-relation-object) with entity types and extracted using three different LLM-based methodologies.

### Key Statistics
- **Companies**: 101 S&P 100 companies  
- **Year**: 2024 10-K filings
- **Model**: Qwen2.5-72B-Instruct
- **Extraction Methods**: 3 (Single-pass, Multi-pass, Reflection)
- **Output Formats**: JSON triplets + Interactive HTML visualizations
- **Total Files**: ~606 files (2 files per company per method)

## 🏗️ Dataset Structure

```
Qwen2.5-72B-Instruct/
├── single_pass/           # One-step extraction
│   ├── AAPL/
│   │   └── 2024/
│   │       ├── AAPL_triplets_internal_Qwen2.5-72B-Instruct_single_pass_2024.json
│   │       └── AAPL_internal_Qwen2.5-72B-Instruct_single_pass_2024_kg_view.html
│   ├── MSFT/
│   └── ... (101 companies total)
├── multi_pass/            # Two-step extraction with refinement
│   ├── AAPL/
│   └── ... (same structure)
└── reflection/            # Feedback-based extraction with iterative refinement
    ├── AAPL/
    └── ... (same structure)
```

## 📋 Data Format

### JSON File Structure
Each JSON file contains an array of chunks, where each chunk represents a section of the 10-K document with extracted knowledge graph triplets.

**Keys in each dictionary entry:**
- `page_id`: Page identifier (e.g., "page_1")
- `chunk_id`: Chunk identifier within the page (e.g., "chunk_1") 
- `source_file`: Original PDF filename (e.g., "AAPL_10k_2024.pdf")
- `ticker`: Stock ticker symbol (e.g., "AAPL")
- `chunk_triplet`: Dictionary containing extracted triplets
- `chunk_text`: Original text from the 10-K filing

### Triplet Structure
Each triplet is a 5-element array: `[Subject, Subject_Type, Relation, Object, Object_Type]`

**Example:**
```json
[
  {
    "page_id": "page_1",
    "chunk_id": "chunk_1", 
    "source_file": "AAPL_10k_2024.pdf",
    "ticker": "AAPL",
    "chunk_triplet": {
      "Triplet 1": [
        "Apple Inc.",           // Subject
        "ORG",                  // Subject Type
        "Has_Stake_In",         // Relation
        "Common Stock",         // Object
        "FIN_INST"             // Object Type
      ],
      "Triplet 2": [
        "Apple Inc.",
        "ORG", 
        "Has_Stake_In",
        "0.000% Notes due 2025",
        "FIN_INST"
      ]
    },
    "chunk_text": "## UNITED STATES\n\n## SECURITIES AND EXCHANGE COMMISSION Washington, D.C. 20549\n\n## FORM 10-K..."
  }
]
```

### HTML Visualization Files
Interactive knowledge graph visualizations that allow exploration of:
- Entity relationships
- Triplet networks  
- Document chunk mappings
- Filterable views by entity types

## 🔬 Extraction Methodologies

### 1. Single-Pass Extraction (`single_pass`)
**Comprehensive one-shot extraction with integrated normalization**

**Process Within Single Prompt**:
- **Step 1**: Maximum Triplet Extraction - Extract as many triplets as possible
- **Step 2**: Intelligent Normalization & Merging - Consolidate entities, standardize names, resolve company references
- **Step 3**: Strict JSON Formatting - Output structured triplets

**Key Features**:
- Entity name standardization (3-5 words max)
- Company reference normalization (consolidate variations to ticker symbols)  
- Duplicate entity merging and relationship consolidation
- Single comprehensive LLM call

### 2. Multi-Pass Extraction (`multi_pass`)
**Two-stage process with dedicated extraction and refinement phases**

**Stage 1 - Initial Extraction**:
- Maximum triplet extraction with basic normalization
- Company reference replacement with ticker symbols
- Initial entity name standardization

**Stage 2 - Dedicated Normalization**:
- Validate entity types and relationships against defined categories
- Advanced entity merging and relationship consolidation
- Remove redundant or duplicate triplets
- Ensure consistency in naming conventions

### 3. Reflection Extraction (`reflection`)
**Three-step iterative process with systematic self-correction**

**Step 1 - Initial Extraction**:
- Extract triplets following strict normalization rules
- All company references must use exact ticker symbol
- Concise entity naming (1-3 words, max 3 words)

**Step 2 - Critical Feedback**:
- Rigorous evaluation of extracted triplets
- Detailed feedback for each problematic triplet
- Issue identification with specific suggestions
- Assessment of entity quality, relationship validity, and business relevance

**Step 3 - Systematic Correction**:
- Correction of identified issues based on feedback
- Automatic dropping of low-value or incorrect triplets  
- Individual triplet correction using targeted prompts

## Company Coverage

The dataset includes 101 S&P 100 companies as of 2024, covering major sectors:

**Technology:** AAPL, MSFT, GOOGL, META, NVDA, ORCL, CRM, ADBE, INTC, CSCO, etc.

**Financial Services:** JPM, BAC, WFC, BLK, AXP, GS, MS, SCHW, USB, etc.

**Healthcare:** JNJ, UNH, PFE, ABBV, LLY, TMO, ABT, DHR, BMY, etc.

**Energy:** XOM, CVX, COP, EOG, SLB, KMI, OXY, etc.

**Industrial:** GE, CAT, BA, HON, UPS, RTX, LMT, etc.

**Consumer:** WMT, PG, KO, PEP, NKE, COST, etc.

## 📖 Entity Types (24 Categories)

**Comprehensive Financial Taxonomy**

### Core Business Entities
- **ORG**: Filing Company (the public company subject of the 10-K filing)
- **COMP**: External companies (competitors, suppliers, customers, partners)
- **SEGMENT**: Internal business divisions (e.g., Cloud segment, North America retail)
- **PERSON**: Key individuals (CEO, CFO, executives, board members)

### Geographic & Regulatory
- **GPE**: Geographic entities (countries, states, cities mentioned in operations/risks)
- **ORG_GOV**: Government bodies (e.g., United States Government)
- **ORG_REG**: Regulatory bodies (SEC, Federal Reserve, ECB)

### Financial & Market
- **FIN_INST**: Financial instruments (bonds, derivatives, options, securities)
- **FIN_MARKET**: Market indices (S&P 500, Dow Jones)
- **FIN_METRIC**: Financial metrics (Net Income, EBITDA, CapEx, R&D Expense)
- **ECON_IND**: Economic indicators (Inflation Rate, GDP Growth, Interest Rate)

### Products & Operations
- **PRODUCT**: Products or services (iPhone, AWS)
- **CONCEPT**: Abstract concepts (Artificial Intelligence, Digital Transformation)
- **RAW_MATERIAL**: Essential materials (Lithium)
- **LOGISTICS**: Supply chain entities (Ports)

### Risk & Compliance
- **RISK_FACTOR**: Documented risks (market risk, cybersecurity risk, geopolitical risk)
- **LITIGATION**: Legal disputes, lawsuits, regulatory investigations
- **REGULATORY_REQUIREMENT**: Regulations (Basel III, SEC rules, GDPR)
- **ACCOUNTING_POLICY**: Accounting policies (revenue recognition, lease accounting)

### Strategic & ESG
- **EVENT**: Material events (pandemics, natural disasters, M&A events)
- **SECTOR**: Industries (Technology, Healthcare, Financials)
- **ESG_TOPIC**: ESG themes (Carbon Emissions, DEI, Renewable Energy)
- **MACRO_CONDITION**: Economic trends (Recession, Labor Shortages)
- **COMMENTARY**: Management statements (outlooks, guidance)

## 🔗 Relationship Types (27 Categories)

**Comprehensive Financial Relationships**

### Ownership & Control
- **Has_Stake_In**: Ownership or equity interest
- **Regulates**: Regulatory oversight  
- **Operates_In**: Operational geography/market presence

### Business Activities
- **Announces**: Public disclosures
- **Introduces**: New product/policy rollouts
- **Produces**: Manufacturing/development
- **Invests_In**: Capital allocation
- **Partners_With**: Strategic collaboration
- **Supplies**: Vendor/supplier relationships

### Financial Impact
- **Impacts**: Broad influence on performance
- **Positively_Impacts**: Positive outcomes
- **Negatively_Impacts**: Adverse outcomes
- **Increases/Decreases**: Growth or decline
- **Affects_Stock**: Stock price influence

### Risk & Events  
- **Involved_In**: Direct event involvement
- **Impacted_By**: Material effects from events
- **Faces**: Legal/regulatory challenges
- **Depends_On**: Reliance relationships

### Market & Reporting
- **Discloses**: Financial reporting
- **Guides_On**: Management commentary
- **Complies_With**: Regulatory compliance
- **Subject_To**: Governance effects

### Additional Relations
- **Related_To**, **Member_Of**, **Causes_Shortage_Of**, **Stock_Decline_Due_To**, **Stock_Rise_Due_To**, **Market_Reacts_To**

## 🔍 Usage Examples

### Loading JSON Data (Python)
```python
import json

# Load triplets for a specific company and method
with open('Qwen2.5-72B-Instruct/single_pass/AAPL/2024/AAPL_triplets_internal_Qwen2.5-72B-Instruct_single_pass_2024.json', 'r') as f:
    apple_kg = json.load(f)

# Extract all triplets
triplets = []
for chunk in apple_kg:
    for triplet_key, triplet_data in chunk['chunk_triplet'].items():
        triplets.append(triplet_data)

print(f"Total triplets extracted: {len(triplets)}")
```

### Comparative Analysis
```python
# Compare extraction methods for the same company
methods = ['single_pass', 'multi_pass', 'reflection']
triplet_counts = {}

for method in methods:
    file_path = f'Qwen2.5-72B-Instruct/{method}/AAPL/2024/AAPL_triplets_internal_Qwen2.5-72B-Instruct_{method}_2024.json'
    with open(file_path, 'r') as f:
        data = json.load(f)
        count = sum(len(chunk['chunk_triplet']) for chunk in data)
        triplet_counts[method] = count

print("Triplet counts by method:", triplet_counts)
```

### Entity Analysis
```python
# Analyze entity types across the dataset
entity_types = {'subjects': {}, 'objects': {}}

for chunk in apple_kg:
    for triplet in chunk['chunk_triplet'].values():
        subject_type = triplet[1]
        object_type = triplet[4]
        
        entity_types['subjects'][subject_type] = entity_types['subjects'].get(subject_type, 0) + 1
        entity_types['objects'][object_type] = entity_types['objects'].get(object_type, 0) + 1

print("Subject entity types:", entity_types['subjects'])
print("Object entity types:", entity_types['objects'])
```

## 📝 Research Applications

This dataset is suitable for various research applications:

1. **Knowledge Graph Construction**: Building comprehensive financial knowledge graphs
2. **Information Extraction**: Evaluating LLM-based extraction methods
3. **Financial NLP**: Training models on financial document understanding
4. **Comparative Analysis**: Studying different extraction methodologies
5. **Entity Recognition**: Financial entity type classification research
6. **Relationship Mining**: Discovering patterns in financial relationships



## 📊 Dataset Statistics

- **Total JSON Files**: 303 (101 companies × 3 methods)
- **Total HTML Files**: 303 (interactive visualizations)
- **Companies Successfully Processed**: 101 S&P 100 companies
- **Average Triplets per File**: ~1,956 triplets (varies by method)
  - Single-pass: ~1,867 triplets per company
  - Multi-pass: ~1,752 triplets per company  
  - Reflection: ~2,251 triplets per company
- **Total Estimated Triplets**: ~592,668 triplets across all files
- **Average File Size**: ~555KB per JSON file
- **File Size Range**: 525KB - 593KB per JSON file
- **Total Dataset Size**: 687MB uncompressed, 90MB compressed (tar.gz)

## 🛠️ Installation & Setup


### Quick Download & Extract
```bash
# Download the compressed dataset

tar -xzf financial_kg_sp100_2024_qwen.tar.gz
cd Qwen2.5-72B-Instruct
```

## 🎯 Quality Metrics & Validation

### Extraction Quality
- **Success Rate**: 100% (303/303 files successfully processed)
- **Entity Coverage**: 24 distinct entity types with balanced distribution
- **Relationship Coverage**: 27 relationship types covering business, financial, and regulatory domains
- **Triplet Quality**: Reflection method shows highest triplet count and quality

### Data Validation
- ✅ All JSON files are valid and parseable
- ✅ Consistent schema across all files
- ✅ No missing required fields
- ✅ Entity types conform to predefined taxonomy
- ✅ Relationship types conform to predefined taxonomy

## ⚖️ Ethics

### Ethical Considerations
- **Data Source**: All data is derived from publicly available SEC 10-K filings
- **Privacy**: No personal information beyond publicly disclosed executive names
- **Bias**: Dataset reflects information as reported by companies; may contain reporting biases
- **Usage Guidelines**: Intended for research and educational purposes

### Disclaimer
This dataset is for research purposes only. Financial decisions should not be made based solely on this data. Always consult official SEC filings and professional financial advice.




**Last Updated**: January 2025 | **Version**: 1.0 | **Status**: Active
