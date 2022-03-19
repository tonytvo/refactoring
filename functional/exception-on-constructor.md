before
```
public Barcode(String text) {
    if ("".equals(text)) throw new IllegalArgumentException("barcode can't be empty");
    else this.text = text;
}
```

after, "Named Constructor" (also called "Creation Method")
```
public static Either<IllegalArgumentException, Barcode> barcodeFrom(String text) {
    if ("".equals(text)) return Either.left(new IllegalArgumentException("barcode can't be empty"));
    else return Either.right(new Barcode(text));
}
```

maybe you consider that ```barcodeFrom()``` is really parsing the barcode from text

```
public static Either<ParsingFailure, Barcode> parse(String text) {...}
enum ParsingFailure { ... }
```

```
String response = Barcode.parse(commandText)
    .fold(
        parsingFailure -> formatParsingFailure(parsingFailure)
        barcode -> handleSellOneItemRequest(barcode));
```
