before
```
public Barcode(String text) {
    if ("".equals(text)) throw new IllegalArgumentException("barcode can't be empty");
    else this.text = text;
}
```

after
```
public static Either<IllegalArgumentException, Barcode> barcodeFrom(String text) {
    if ("".equals(text)) return Either.left(new IllegalArgumentException("barcode can't be empty"));
    else return Either.right(new Barcode(text));
}
```
