# Optional

Optional 这个特性是从 Java8 开始引入的，这篇文档将描述它的使用，以及引入这个特性的意义所在。

## 代替 if-else 判空 ##

比如说我们要去获取一个 Banner 的数据，但是在这个数据可能是不存的，我们会为此进行判空操作:
```Java
public BannerEntity getByName(String name) {
	BannerEntity banner = this.bannerRepository.findByName(name);
	if (banner == null) {
		throw new NotFoundException(300001);
	}
	return banner;
}
```

但是如果我们使用了 Optional 这个对象之后，我们可以把上面的写法改成下面的形式:
```Java
public BannerEntity getByName(String name) {
	Optional<BannerEntity> banner = this.bannerRepository.findByName(name);
	return banner.orElseThrow(() -> new NotFoundException(4324323));
}
```
从表面上来看，这更像是一种语法糖，可以让我们的代码更加的简洁。但实际上，**更大的意义在于使用了 Optional对象， 让我们必须处理变量为空的情况**。

## Optional 的基础使用 ##

Optional是一个泛型类，需要我们将指定传入类型。下面的代码构建了一个空字符串的 Optional 对象，并不是说使用 Optional 就不需要做判断，而是说必须要进行判断:
```Java
Optional<String> empty = Optional.empty();
empty.get(); // 抛出 java.util.NoSuchElementException: No value present
```
然后我们看下面的例子，我们可以利用 `Optional.of` 方法来指定一个值，如果我们指定的是 `null` 呢?
```Java
// 会直接抛出 java.lang.NullPointerException
Optional<String> empty = Optional.of(null);
```
使用 Optional 对象，可以防止传入一个 null 值。如果一定要传入 null 值呢？
``
Optional<String> empty = Optional.ofNullable(null);
// 可以使用 get 方法取值
String s = empty.get(); // 抛出 java.util.NoSuchElementException: No value present
``
使用 get 方法进行取值的时候，如果是空值，就会直接报错。下面的例子演示了默认值的写法:
```Java
Optional<String> value = Optional.ofNullable(null);
String v = value.orElse("default");     // It's default
```

## Consumer/Supplier ##

我们来看下面的代码，传入一个表达式作为消费者(Consumer)去消费非空的值:
```Java
Optional<String> value = Optional.ofNullable(null);
value.ifPresent(System.out::println);   // It's empty
Optional<String> value2 = Optional.ofNullable("Hello");
value2.ifPresent(System.out::println);      // It's Hello
```
然后再来看下面的代码，传入一个表达式作为提供者(Supplier):
```Java
Optional<String> value = Optional.ofNullable(null);
value.orElseThrow(() -> new NotNullException());
```
属于 Consumer 还是 Supplier 取决于表达式的作用是消费一个值还是提供一个值。

## Runnable/Function/Predicate ##

接着上面的  Consumer 和 Supplier, 它们都是从返回值的角度来考虑的。我们再来说三种传参类型, 分别是  Runnable、Function、Predicate 的含义以及对应的链式操作。

Runnable 是指输入的表达式既无输入又无输出，比如说 `Optional.stream().onClose()` 方法。

而 Function 表示既有输入又有输出的表达式，比如说 map 方法:
```Java
Optional<String> value = Optional.ofNullable("Hello");
// It's Hello World
value.map(v -> v + " World").ifPresent(System.out::println);
```
而 Predicate 指的是返回一个布尔值，比如说 filter方法。

## 真正的意义 ##

如果你不使用 Optional 对象的话，即使获取到是空值，也不会报错，你可以在方法调用栈上继续传递下去。 一直到具体使用它的时候，会抛出 NullPointerException。

**在 Java 中，空指针异常是让人很头疼的事情，或者说这是意见糟糕的事情。并不仅仅是因为它这个异常本身，而是因为空指针异常是一种隐藏性的错误，随着函数调用栈逐渐加深，要排查这样的错误会越来越困难，你不得不追踪整个函数调用栈，在源头找到这样错误的原因。排错成本会非常高、排错也会更加的困难。**