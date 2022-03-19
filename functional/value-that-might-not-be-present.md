Scenario 1
  - before
```
if (maybeValue.isPresent()) {
    return Option.some(f(maybeValue.get()));
}
else {
    return Option.none();
    //return "fallback value";
}
```
  - after
```
return maybeValue.map(f);
//return maybeValue.map(f).getOrElse("fallback value");
```
