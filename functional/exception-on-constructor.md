before
```
public Barcode(String text) {
    if ("".equals(text)) throw new IllegalArgumentException("barcode can't be empty");
    else this.text = text;
}
```
