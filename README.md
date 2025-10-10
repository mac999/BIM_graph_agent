# BIM Graph Agent

BIM graph agent with data processing system that converts IFC files to Neo4j graph database and provides AI-powered natural language querying capabilities. This project is example and demonstration to show how to use graph database like neo4j as the viewpoint of RAG and AI Agent development.

<p align="center">
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img2.jpg" width="750"> </img></br>
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img1.jpg" height="200"> </img>
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img3.jpg" height="200"> </img></br>
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img9.jpg" height="255"> </img>
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img7.jpg" height="255"> </img>
<img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img8.jpg" height="255"> </img>
</p>

## Features

### Core System
- **IFC to Graph Conversion**: Automatically processes IFC files and converts them to Neo4j graph database
- **Data Integrity**: Accurately converts IFC elements and relationships without data loss
- **File Metadata Management**: Stores and links IFC file information with graph structure

### BIM Graph Agent (AI-Powered Expert System)
- **Natural Language Querying**: Ask questions about BIM data in plain English or Korean
- **Smart Property Analysis**: Analyzes nested JSON properties regardless of modeling tool
- **Interactive Console Interface**: Real-time conversational interface for BIM data exploration

## Requirements

### System Requirements
- Python 3.9 or higher
- Neo4j database (version 4.0 or higher)
- 8GB RAM minimum (16GB recommended for large IFC files)
- 2GB free disk space for database storage

### AI Agent Requirements
- Ollama server (local LLM runtime)
- qwen2.5-coder:7b model (for Cypher query generation)
- 8GB VRAM recommended (NVIDIA GPU optional for faster processing)
- Internet connection for initial model download

## Installation
1. Create elements database in Neo4j like below
<p align="center">
   <img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/db1.jpg" height="300"></img>
</p>

2. Clone or download this project
3. Install required packages:
   ```bash
   pip install -r requirements.txt
   ```
4. Configure environment variables in `.env` file:
   ```
   NEO4J_URI=bolt://localhost:7687
   NEO4J_USER=neo4j
   NEO4J_PASSWORD=your_password
   NEO4J_DATABASE=elements
   ```
5. Install and setup Ollama for BIM Graph Agent:
   ```bash
   # Install Ollama from https://ollama.ai
   
   # Pull required model (optimized single model approach)
   ollama pull qwen2.5-coder:7b
   
   # Start Ollama server
   ollama serve
   ```

## Usage

### Create elements graph databsae in Neo4j from input IFC files
```bash
python import_ifc.py [options]

Options:
  --input-dir DIR     Input directory containing IFC files (default: ./input)
  --clear-db          Initialize database before conversion
  --log-level LEVEL   Log level (DEBUG, INFO, WARNING, ERROR, CRITICAL)
  --no-log-file       Disable file logging
  --validate          Run validation after conversion
  --stats             Output statistics after conversion completion
  --help              Show help message
```

Convert all IFC files in the input directory:
```bash
python import_ifc.py --input-dir ./input --stats
```

Clear database and convert with debug logging:
```bash
python import_ifc.py --clear-db --log-level DEBUG --validate
```

### BIM Graph Agent (AI Expert System)

Launch the AI-powered natural language interface:
```bash
python BIM_graph_expert.py
```

#### Example Queries

**Basic Element Queries:**
- How many walls are in the building?
- Show me all doors in the project
- List all windows with their properties
<p align="center">
   <img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img4.jpg" width="400"></img>
</p>

**Property-Specific Queries:**
- What is the area of room A204?
<p align="center">
   <img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img5.jpg" width="400"></img>
</p>

**File and Metadata Queries:**
- What IFC files are loaded in the database?
- Show me the file information for this model
<p align="center">
   <img src="https://github.com/mac999/BIM_graph_agent/blob/main/doc/img6.jpg" width="400"></img>
</p>

**Advanced Analysis:**
- Find all load-bearing walls

**TBD (Relationship Queries etc)**
- Find all spaces on the Level 2
- Show me walls with thickness greater than 200mm
- What elements are connected to this wall?
- Find all elements that belong to the ground floor
- Calculate total floor area by level
- List all mechanical equipment in the building

## Project Structure

```
BIM_graph_rag/
├── input/                      # Input directory for IFC files
│   └── Duplex_A_20110907.ifc   # Sample IFC file
├── src/                        # Source code modules
│   ├── ifc_parser.py           # IFC file parsing module
│   ├── neo4j_database.py       # Neo4j database connection module
│   ├── graph_converter.py      # IFC to graph conversion logic
│   └── utils.py                # Logging utilities
├── logs/                       # Log files directory
├── requirements.txt            # Required Python packages
├── .env                        # Environment configuration
├── import_ifc.py              # Main CLI application
├── BIM_graph_expert.py        # BIM Graph Agent (AI expert system)
└── README.md                  # This file
```

## Graph Data Model

### Nodes
- **IFCFile**: Contains file metadata (filename, path, creation date, etc.)
- **Element**: Represents IFC elements with labels like `:Element:IfcWall`, `:Element:IfcDoor`

### Node Properties
- `globalId`: IFC GlobalId (unique identifier)
- `name`: Element name
- `ifcClass`: IFC class name
- `description`: Element description
- `sourceFileId`: Reference to source IFC file
- `properties`: PropertySet information (stored as JSON)

### Relationships
- `BELONGS_TO_FILE`: Links elements to their source IFC file
- `AGGREGATES`: Aggregation relationships between elements
- `CONNECTS_TO`: Connection relationships between elements
- `CONTAINED_IN`: Spatial containment relationships
- `ASSIGNED_TO`: Group assignment relationships

## Output

The application provides:
- Real-time progress logging
- Conversion statistics (node/relationship counts)
- Element type distribution
- Validation results (optional)
- Comprehensive error handling and reporting

## BIM Graph Agent Architecture

The BIM Graph Agent is an advanced AI system that enables natural language querying of BIM graph data with intelligent property analysis.

### System Architecture
1. **Natural Language Processing**: User enters questions in English or Korean
2. **Smart Query Generation**: qwen2.5-coder:7b model creates optimized Cypher queries
3. **Graph Data Retrieval**: Executes queries against Neo4j elements database
4. **Intelligent Property Analysis**: Analyzes nested JSON properties from any modeling tool
5. **Contextual Response Generation**: Provides detailed answers with data sources and units

### Key Capabilities
- **Universal Property Support**: Analyzes properties from any BIM modeling tool (Revit, ArchiCAD, Tekla, etc.)
- **Multi-Language Support**: Handles English and Korean property names and queries
- **Smart Schema Detection**: Automatically adapts to different IFC modeling approaches
- **Performance Optimization**: Single LLM model approach eliminates memory switching overhead
- **Interactive Learning**: Shows generated Cypher queries for educational purposes

### Advanced Features
- **Flexible Property Matching**: Finds area, volume, and other properties regardless of naming conventions
- **Cross-Tool Compatibility**: Works with models from different BIM software vendors
- **Intelligent Fallback**: Returns full property JSON when specific paths are unavailable
- **Real-Time Analysis**: Processes queries and responses in real-time conversation flow

### Usage Guidelines
- Ask questions in natural language (no technical Cypher knowledge required)
- Be specific about elements (walls, doors, spaces, etc.) for better results
- Property queries automatically search through all available property sets
- The system shows the generated Cypher query and data sources for transparency

## Troubleshooting

### Common Issues

1. **Neo4j Connection Failed**
   - Ensure Neo4j is running
   - Check connection settings in `.env` file
   - Verify database credentials

2. **IFC File Parsing Failed**
   - Check if IFC file is valid (IFC2x3 or IFC4 schema)
   - Ensure file is not corrupted
   - Check file permissions

3. **Memory Issues with Large Files**
   - Increase available system memory
   - Process files individually if needed

### Log Files
Log files are automatically created in the `logs/` directory with timestamps for troubleshooting.

## License

This project is developed for BIM data processing and graph analysis purposes.

## Author

Taewook Kang (laputa99999@gmail.com)




















