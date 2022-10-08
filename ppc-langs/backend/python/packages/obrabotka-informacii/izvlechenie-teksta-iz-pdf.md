# Извлечение текста из pdf

Установка пакета для python 3 (так-то пакет написан для второго питона) `pip install pdfminer.six`

```python
from io import StringIO
from pdfminer.converter import TextConverter
from pdfminer.pdfinterp import PDFPageInterpreter, PDFResourceManager
from pdfminer.layout import LAParams
from pdfminer.pdfpage import PDFPage


def extract_text_from_pdf(pdf_path):
    rsrcmgr = PDFResourceManager()
    retstr = StringIO()
    codec = 'utf-8' # 'utf16', 'utf-8'
    laparams = LAParams()
    device = TextConverter(rsrcmgr, retstr, codec=codec, laparams=laparams)
    interpreter = PDFPageInterpreter(rsrcmgr, device)
    maxpages = 0
    caching = True
    pagenos = set()
    password = ""

    with open(pdf_path, 'rb') as fh:
        for page in PDFPage.get_pages(fh, pagenos, maxpages=maxpages, password=password, caching=caching, check_extractable=True):
            interpreter.process_page(page)

        text = retstr.getvalue()
    # close open handlers
    device.close()
    retstr.close()
    if text:
        return text
    return None

text = extract_text_from_pdf('myPDF.pdf')

```

