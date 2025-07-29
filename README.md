# 📄 Adobe Hackathon 2025 – Team Bareozgarians ;)

This repo contains solutions for both challenges in Adobe Hackathon 2025:

- **Challenge 1a**: Extracts a structured outline (title, H1–H3) from PDFs.
- **Challenge 1b**: Extracts semantically relevant content based on a given persona.

Both are fully containerized, modular, and run completely offline.

---

## 🧩 Overview

### 🔹 Challenge 1a – PDF Outlining - Made without any model, just good logic and simple design!!
Pipeline:
- `pdf_parser.py`: Extracts text blocks with font metadata.
- `title_detector.py`: Detects centered, prominent titles on the first page.
- `heading_detector.py`: Tags H1–H3 based on font/style.
- `hierarchy_fixer.py`: Ensures valid heading order.

Output: JSON with title and structured headings.

### 🔹 Challenge 1b – Persona-Based Extraction
Pipeline:
- `pdf_processor.py`: Groups paragraphs under headings.
- `ranking.py`: Ranks sections using `all-MiniLM-L12-v2` (sentence-transformers).
- `relevance_filter.py`: Applies rule-based filtering.
- `main.py`: Extracts top-ranked paragraphs.

Output: JSON with persona-relevant content.

---

## ⚙️ Tech Stack

- **Language**: Python 3.9
- **PDF Parsing**: PyMuPDF
- **Semantic Ranking**: sentence-transformers (MiniLM), PyTorch (CPU)
- **Model Size**: ~134MB
- **Deployment**: Docker (offline, no network access)

---

## 🚀 Usage

### 1. Prepare Folders

```bash
mkdir input output
# Place your PDFs in ./input/
```

How to Build and Run
The solution is containerized with Docker and has no external network dependencies at runtime.

### 2. Note for the submission:<br>
Create input and output folder as github doesnt allow empty folders so couldnt add it :(
<br>
Input file & Output file already provided for 1B
To demonstrate how our app works with the model, we’ve included a sample input file along with its corresponding output file.
<br>
### 3. Build the Docker Image
Use the following command from the root of the project directory:
```
docker build --platform linux/amd64 -t mysolution:latest .
```
### 4. Run the Container
To process PDFs, place them in the input directory. The container will automatically process all PDFs and place the corresponding JSON files in the output directory.

Use the following command to run the container, replacing $(pwd) with the absolute path to your project directory if you are not using a Unix-like shell.
```
docker run --rm -v $(pwd)/input:/app/input -v $(pwd)/output:/app/output --network none mysolution:latest
