Genomic Sequence Analysis with Python: FASTA Parsing, ORF Detection & Repeat Mining

Final exam project for Python for Genomic Data Science (Johns Hopkins University). The notebook parses a multi-sequence FASTA file and answers a set of core genomics questions: sequence counts and lengths, open reading frames (ORFs), and repeated subsequences. All the logic is written from scratch in plain Python.

Overview
Task	Approach
Parse a multi-record FASTA file	Line-by-line parser that builds a {identifier: sequence} dictionary
Sequence statistics	Record count, length of each sequence, longest and shortest sequence
ORF detection	Custom orf_finder() that scans reading frames 1–3 for ATG → stop codon
Longest ORF (global and per sequence)	Iterates over every sequence and frame and keeps the maximum
Repeat mining	Custom find_repeats() that counts every k-mer of length n across all sequences
Dataset
File: dna2.fasta
Records: 18 DNA sequences, all fragments of GenBank entry EQ086233.1 (identifiers like gi|142022655|gb|EQ086233.1|<n>)
Length range: 115 – 4,894 nt
Workflow
1. Load the FASTA file

Opens the file and handles a missing file with try/except IOError.

2. Build the sequence dictionary

Each header line (>) starts a new entry. The first word of the header becomes the key, and the sequence lines that follow are joined into the value.

3. Sequence statistics
Total number of records
Length of each sequence (nt)
Longest and shortest sequence, found with max() / min() and a len() key
4. Open Reading Frame detection

orf_finder(dna, frame) steps through the sequence codon by codon from the chosen frame offset (0, 1 or 2):

Opens an ORF at the first ATG
Closes it at the first in-frame stop codon (TAA, TAG, TGA)
Returns a list of (start_position, orf_length) tuples

The notebook then finds:

The longest ORF in the whole file: its sequence ID, length, start position and frame
The longest ORF in one chosen sequence (example: ...|16)
5. Repeat analysis

find_repeats(sequences, n) slides a window of length n across every sequence, counting overlapping k-mers, and keeps only the ones that appear more than once. The notebook reports:

Number of distinct repeats of length n
The most frequent repeat(s) and how many times they occur
Key Results
Question	Result
Number of records	18
Longest sequence	gi|142022655|gb|EQ086233.1|255, 4,894 nt
Shortest sequence	gi|142022655|gb|EQ086233.1|346, 115 nt
Longest ORF (all sequences)	...|45, 2,394 nt, starts at position 385, frame 1
Longest ORF in ...|16	1,644 nt, starts at position 1440, frame 3
Distinct repeats of length 7	7,102
Most frequent 7-mer	CGCGCCG, occurs 63 times

Positions are 1-based. Frames are numbered 1–3 on the forward strand.

Repository Structure
.
├── Final_exam_biopython.ipynb   # Main analysis notebook
├── data/
│   └── dna2.fasta               # Input FASTA file
├── requirements.txt
└── README.md
How to Run
bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

# 2. Install dependencies
pip install -r requirements.txt

# 3. Launch the notebook
jupyter notebook Final_exam_biopython.ipynb

Put dna2.fasta in data/ and make sure the path in the first cell points to it:

python
f = "data/dna2.fasta"

To analyze other parameters:

Another sequence's longest ORF: change the identifier in the per-sequence ORF cell
Another repeat length: change n in find_repeats(sequences, n=7)
Requirements
Python 3.8+
Jupyter Notebook
Biopython (imported for BLAST / SeqIO; the core analysis uses only plain Python)

requirements.txt:

biopython
notebook
Limitations & Possible Extensions
Forward strand only: ORFs on the reverse complement are not searched. 
Adding Bio.Seq.reverse_complement() would cover all 6 reading frames.

Complete ORFs only: an ORF with no stop codon before the end of the sequence is not reported.
Nested ORFs: only the first ATG before each stop is used, so ORFs that start inside another ORF are not counted separately.
Possible next steps: run NCBIWWW.qblast() on the longest ORF to identify the gene, translate ORFs into protein, or plot the length distribution.
Skills Demonstrated

Python · Bioinformatics · FASTA parsing · ORF detection · k-mer analysis · Dictionaries & lambda functions · Jupyter

Author

Eric Zamora, B.S. in Genomic Biotechnology (UANL)
