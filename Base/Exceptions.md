Иерархия исключений: [[Excalidraw/Exceptions|Exceptions]]
## Throwable
Конструкторы:
- `Throwable();`
- `Throwable(message);`
- `Throwable(cause);`
- `Throwable(message, cause);`
- `protected Throwable(message, cause, suppression, stackTrace);`
Методы:
- `getMessage();` // сообщение
- `getStackTrace();` // стек
- `getCause();` // причина
- `add/getSuppressed();` // подавление исключения
## Создание своего исключения
```Java
public class MyException extends Exception {
	public MyException(String message) {
		super(message);
	}
}
```
## Пробрасывание исключений
```Java
public void methodName() throws MyException /*Указываем, что пробрасываем исключение выше по стеку вызова*/{
	throw MyException(); // пробрасываем исключение
}
```
## Обработка исключения
```java
pyblic void methodName() {
	try {
		// блок в котором ловим сключение
	} catch (MyException e) {
		// обработка исключения
	} finaly {
		// здесь выполняется код, который гарантировано выполнится
	}
}
```

----
## try-finaly
```Java
public int test() {
	try {
		return 5;
	} finaly {
		return 6; // вернется 6, т.к. в любом случае выполнится после блока try
	}
}
```

```Java
public int test() {
	int x = 5;
	try {
		return x; // вернем 5, т.к. значение уже вычеслено и сохранено
	} finaly {
		x = 6; 
	}
}
```
