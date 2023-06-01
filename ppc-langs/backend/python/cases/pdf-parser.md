# PDF Parser

## Декодирование стримов

```python
import sys, zlib

def main(fn):
    data = open(fn, 'rb').read()
    i = 0
    first = True
    last = None
    while True:
        i = data.find(b'>>\nstream\n', i)
        if i == -1:
            break
        i += 10
        try:
            last = cdata = zlib.decompress(data[i:])
            if first:
                first = False
            else:
                pass#print cdata
        except:
            pass
    print(last.decode('utf-8'))

if __name__=='__main__':
    main(*sys.argv[1:])
```

## Пример извлечения текста

```python
from pdfreader import SimplePDFViewer 

fd = open('cv3.pdf', "rb") 
viewer = SimplePDFViewer(fd) 
viewer.render() 
"".join(viewer.canvas.strings)

pdfReader = PyPDF2.PdfFileReader(open('file.pdf','rb')) 
pageObj = pdfReader.getPage(0) 
text = pageObj.extractText()
```
