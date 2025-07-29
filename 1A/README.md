# Adobe Hackathon 2025 - PDF Outlining Solution

This project is a solution for Challenge 1a, designed to extract a structured title and outline from PDF documents and output them as JSON files.

## Approach

The solution follows a multi-stage pipeline to robustly identify and structure document elements:

1.  **PDF Parsing (`pdf_parser.py`):** The process begins by using the `PyMuPDF` library to extract all text blocks from the PDF. Each block is enriched with metadata, including font size, font name, position, and flags to identify content within columns or bordered boxes. This initial parsing also determines the document's most common body text style.

2.  **Title Detection (`title_detector.py`):** The title is identified using a set of strict heuristics. It looks for text on the first page that is positioned centrally, has a larger-than-average font, and is not overly common. It merges multi-line titles and validates that the complete title block is centered on the page.

3.  **Heading Detection (`heading_detector.py`):** Headings are initially identified as text blocks that are stylistically distinct from the body text (e.g., larger font, bold weight). An initial hierarchy (H1, H2, H3) is assigned based on the rank of font styles.

4.  **Hierarchy Refinement (`hierarchy_fixer.py`):** A crucial post-processing step ensures a logical and consistent document structure. This fixer runs in multiple passes to:
    * Enforce sequential order (e.g., an H3 cannot directly follow an H1).
    * Correct levels based on font size (e.g., an H2 cannot be larger than its preceding H1).
    * Ensure the document starts with an H1.

This layered approach makes the solution resilient to common PDF formatting inconsistencies.

## Models and Libraries Used

* **Language:** Python 3.9
* **Core Library:** **PyMuPDF (`fitz`)** - A lightweight, high-performance open-source library for PDF parsing and text extraction. No other external models or libraries are used.

## How to Build and Run

The solution is containerized with Docker and has no external network dependencies at runtime.

### **Build the Docker Image**

Use the following command from the root of the project directory:

```bash
docker build --platform linux/amd64 -t mysolution:latest .
```

### **Run the Container**

To process PDFs, place them in the `input` directory. The container will automatically process all PDFs and place the corresponding JSON files in the `output` directory.

Use the following command to run the container, replacing `$(pwd)` with the absolute path to your project directory if you are not using a Unix-like shell.

```bash
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolution:latest
```