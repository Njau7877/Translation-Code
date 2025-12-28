PDF English-to-German Translator 📄➡️🇩🇪

This project translates an English PDF document into German using Python.
It extracts text from a PDF, translates it in safe-sized chunks using Google Translate via deep-translator, and then generates a new German PDF.

The workflow is designed to run smoothly in Google Colab.

✨ Features

Extracts text from multi-page PDFs

Translates large documents by chunking text

Avoids googletrans dependency conflicts

Generates a clean, readable German PDF

Supports German characters (ä, ö, ü, ß)

Simple and beginner-friendly

🛠️ Technologies Used

Python 3

Google Colab

PyPDF2 – PDF text extraction

deep-translator – Reliable Google Translate wrapper

ReportLab – PDF generation

📦 Installation

Run the following command in Google Colab:

!pip install PyPDF2 reportlab deep-translator


⚠️ Do NOT install googletrans. It causes dependency conflicts with Colab.

🚀 Usage
1️⃣ Upload your PDF
from google.colab import files
uploaded = files.upload()


Upload an English PDF (example: plan1.pdf).

2️⃣ Extract text from the PDF
import PyPDF2

with open("plan1.pdf", "rb") as file:
    reader = PyPDF2.PdfReader(file)
    english_text = ""
    for page in reader.pages:
        english_text += page.extract_text() + "\n"

3️⃣ Translate text (English → German)
from deep_translator import GoogleTranslator

def translate_text_in_chunks(text, chunk_size=4500):
    translated_chunks = []
    for i in range(0, len(text), chunk_size):
        chunk = text[i:i + chunk_size]
        translated = GoogleTranslator(source='en', target='de').translate(chunk)
        translated_chunks.append(translated)
    return "".join(translated_chunks)

german_text = translate_text_in_chunks(english_text)

4️⃣ Generate the German PDF
from reportlab.platypus import SimpleDocTemplate, Paragraph
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib.pagesizes import letter

doc = SimpleDocTemplate("plan1_translated_german.pdf", pagesize=letter)
styles = getSampleStyleSheet()
story = []

for line in german_text.split("\n"):
    story.append(Paragraph(line, styles["Normal"]))

doc.build(story)

5️⃣ Download the translated PDF
files.download("plan1_translated_german.pdf")

📄 Output

Input: English PDF

Output: plan1_translated_german.pdf (German translation)

⚠️ Known Limitations

Original formatting (tables, images, fonts) is not preserved

Scanned PDFs (image-only) require OCR first

Google Translate rate limits may apply for very large files

✅ Why deep-translator?

googletrans forces outdated dependencies and breaks Colab environments.
deep-translator is:

More stable

Easier to maintain

Compatible with modern Python environments

🔮 Possible Improvements

OCR support for scanned PDFs

Preserve headings and formatting

Batch processing of multiple PDFs

Support for other languages

📜 License

This project is for educational and personal use.
Ensure compliance with Google Translate’s terms of service when using at scale.
