### Обобщенные классы и интерфейсы
- `class List<T>;`
- `class Map<K, V>;`
- `class NumberList<T extends Number>;`
- `class X<T extends Y & Z>;`
### Может использоваться внутри нестатических членов класса:
- Тип поля, параметра, переменной
- Возвращаемый тип метода
- Подставляться параметром другого типа

```Java
static class Option<T> {
	T value;

	public Option(T value) {
		this.value = value;
	}

	public T get() {
		if (value == null) {
			throw new NoSuchElementException();
		}
		return value;
	}

	public T orElse(T other) {
		return value == null ? other : value;
	}

	public boolean isPresent() {
		return value != null;
	}
}
```

`new SomeClass<?>()` - wildcard, какой-то тип, когда не важен тип. 