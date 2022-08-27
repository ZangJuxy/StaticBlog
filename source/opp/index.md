

# 空检查属性获取-Opp

&nbsp;&nbsp; 因为作者在`Hutool`中的写的`Opt`有些地方考虑不周，所以在我们`stream-query`中进行升级命名为`Opp`。</br>
&nbsp;&nbsp; 在嵌套对象的属性获取中，由于子对象无法得知是否为`null`，每次获取属性都要检查属性兑现是否为`null`，使得代码会变得特备臃肿，因此使用`Opp`来优雅的链式获取属性对象值。

## empty

>返回一个包裹着null的Opp对象

```java
Opp<Object> empty = Opp.empty();
empty.get();
out>>null
```

## required

>返回一个包裹的元素不可能为空的Opp对象(注意没有判断空字符串)
>会抛出NPE

```java
String empty = null;
Opp<String> required = Opp.required(empty);

```

## of

>返回一个包裹的元素可能为空的Opp对象
>并不会因为要包裹的对象为null抛出NPE

```java
String empty = null;
Opp<String> required = Opp.of(empty);
```

## blank

>返回一个包裹的元素可能为空的Opp对象(进行了空字符串的判断)

```java
boolean aNull = Opp.blank("  ").isNull();
out>>true
```

## empty(Collection)

>返回一个包裹的集合可能为空的Opp对象，额外判断了集合内元素为空的情况

```java
Opp.empty(Collections.<String>emptyList()).isNull();
out>>true
```

## ofTry

>这是一个能够捕获异常到Opp对象中的方法并且可以指定返回值和兼容多线程的操作，方法重载，可以只进行操作，也可以进行操作，在后续参数传入我们要捕获的异常.class对象

我们在进行操作的时候如果没有指定要捕获的异常那么默认就会捕获所有异常，即使出现了异常也不会报错

```java
List<String> last = null;
Opp<String> stringOpp = Opp.ofTry(() -> last.get(0));
// 此时应该会NPE异常但是我们会将所有的异常捕获所以此时不会报错而且我们的Opp对象中的exception属性为java.lang.NullPointerException
System.out.println(stringOpp.getException());
out>>java.lang.NullPointerException
```

如果不是我们要捕获的异常或者其子类就会抛出

```java
Opp<String> stringOpp = Opp.ofTry(() -> last.get(0), IndexOutOfBoundsException.class);
// 现在的意思就是将IndexOutOfBoundsException和其子异常进行捕获，如果发生的异常不是IndexOutOfBoundsException或者其子类就会直接抛出
out>>java.lang.RuntimeException: java.lang.NullPointerException
```

>以下方法应当配合ofTry()使用更佳

### getException()

>获取异常，当调用ofTry()时，异常信息不会抛出，而是保存，调用此方法获取抛出的异常

```java
Opp<String> stringOpp = Opp.ofTry(() -> last.get(0));
System.out.println(stringOpp.getException());
out>>java.lang.NullPointerException
```

## isFail()

>当调用 ofTry时，捕获到我们要捕获的异常时返回true

```java
// 适用场景如果当一个请求发生超时异常时if ofTry(操作,超时异常.class).isFail return xxx;
这样就可以在超时的第一时间返回一个想展示给用户的数据
```

## get()

>返回包裹里的元素，取不到则为null，注意和java.util.Optional的get()不一样，不会抛出NoSuchElementException，如果需要一个绝对不能为null的值则可以使用orElseThrow()

```java
Object opp = Opp.of(null).get();
out>>null
```

> 如果我们想要获取一个对象中的值,我们提供了lambda表达式的方法更简洁方便

```java
Opp.of(User.builder().username("Zverify").build()).get(User::getUsername);
out>>Zverify
```

## isNull()

>判断包裹里的元素是不是null，不存在为 true，否则为 false

```java
Opp.of(null).isNull();
out>>true
```

## isNonNull()

>判断包裹里元素的值是否存在，存在为 true，否则为 false

```java
Opp.of(null).isNonNull();
out>>false
```

## ifPresent()

>如果包裹里的值存在，就执行传入的操作

```java
Opp.ofNullable("Hello Steam!").ifPresent(System.out::println);
out>>Hello Steam!
```

## filter()

>判断包裹里的值存在并且与给定的条件是否满足 断言执行结果是否为true
>如果满足条件则返回本身
>满足条件或者元素本身为空时返回一个返回一个空的Opp对象

```java

```
// TODO
