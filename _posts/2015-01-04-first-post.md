<main class="jss202">

#### Async in Flutter - Futures

10 min read, 25 July, 2018

###### 

When building Flutter apps, it's common to have code that works in an asynchronous way. A typical example is to retrieve data from a remote server. An asynchronous action may succeed or fail, and the code needs to handle both cases. If you have experiences with other programming languages, you may be familiar with concepts like [`Future` in Java](https://docs.oracle.com/javase/10/docs/api/java/util/concurrent/Future.html) and [`Promise` in JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise). Dart has the similar concept of [`Future`](https://docs.flutter.io/flutter/dart-async/Future-class.html) to represent a potential value, or error, that will be available at some time in the future. Even the name `Future` is the same as what's used in Java, the usage pattern of `Future` in Dart is more like `Promise` in JavaScript.

# [](https://flutter-academy.com/async-in-flutter-futures/#use-code-classlanguage-textfuturecode)Use `Future`

We start from how to use `Future` objects. The class `Future` is included in the `dart:async` package. A `Future` object can be in two states:

*   **pending** - In this state, the computation represented by this `Future` is still in progress, and no result is available.
*   **completed** - In this state, the computation is completed either successfully or with failure and the result is available. We can further divide this state into two sub-states: **completed with value** and **completed with error**.

`Future` class is generic with the type argument specifying the type of its value. Given a `Future` object, we can add callback listeners to be called when the value or error is available.

## [](https://flutter-academy.com/async-in-flutter-futures/#code-classlanguage-textfuturethencode)`Future.then`

The method `Future.then` is used to add callbacks when the future completes. We can add callbacks for both states **completed with value** and **completed with error**. In the following code, we use the factory constructor `Future.delayed` to create a new `Future` object that will be completed after 3 seconds. Based on the random boolean value, the `Future` may complete with the value `100` or an error `boom!`. We use `then` to add callbacks for both cases and output different messages to the console.

```
import 'dart:async';
import 'dart:math';

void main() {
  var random = new Random();
  var future = new Future.delayed(new Duration(seconds: 3), () {
    if (random.nextBool()) {
      return 100;
    } else {
      throw 'boom!';
    }
  });
  future.then((value) {
    print('completed with value $value');
  }, onError: (error) {
    print('completed with error $error');
  });
}
```

The return value of `then` is actually a new `Future` object. This new `Future` object is completed with the result of invoking the corresponding callback. The callback can return a simple value or another `Future` object. This means we can chain different `Future` objects together. A callback can use the value from its previous `Future` object as the input, and its return value will be the input for the callback of the next `Future` object. In the code below, `Future.value` creates a new `Future` completed with the given value. In the `then` callbacks, a new `Future` is returned. The output result is `a b c d`.

```
import 'dart:async';

void main() {
  var future = new Future.value('a').then((v1) {
    return new Future.value('$v1 b').then((v2) {
      return new Future.value('$v2 c').then((v3) {
        return new Future.value('$v3 d');
      });
    });
  });
  future.then(print, onError: print);
}
```

## [](https://flutter-academy.com/async-in-flutter-futures/#code-classlanguage-textfuturecatcherrorcode)`Future.catchError`

`Future.catchError` adds a callback to handle errors in the `Future`. By default the callback handles all errors. We can also pass a predicate function `test` to check if an error should be handled. In the code below, the `test` predicate specifies that only error objects with type `String` are handled. `Future.error` creates a new `Future` completed with the given error.

```
import 'dart:async';

void main() {
  new Future.error('boom!').catchError(print, test: (error) {
    return error is String;
  });
}
```

You may wonder what's the difference between the `onError` callback in `Future.then` and `Future.catchError`. `onError` callback in `Future.then` only handles errors thrown in the original `Future`, but it cannot handle errors thrown inside of the callback itself. The error thrown in the `onError` callback completes the `Future` returned by `then` with the given error.

In the code below, the original `Future` is completed with error `boom!`. The `onError` callback in the first `then` method handles this error and throw another error `new error`. This new error completes the `Future` returned by the first `then` method and it's handled by the `onError` callback in the second `then` method.

This may look a little bit complicated. The key point to understand is that **`then` method returns a new `Future` and `onError` can only handle errors in the current `Future`**.

```
import 'dart:async';

void main() {
  new Future.error('boom!').then(print, onError: (error) {
    print('handle original error: $error');
    throw 'new error';
  }).then(print, onError: (error) {
    print('handle new error: $error');
  });
}
```

When `Future`s are chained together, it usually doesn't matter where the error comes from, so it's a common practice to use `catchError` to add a callback to catch all errors thrown in the `Future` chain. It's not recommended to use both `onError` and `catchError` at the same time.

The code below shows a common way to handle errors. Three `Future`s are chained together with the second `Future` completes with an error. The last `catchError` catches all errors in the chain.

```
import 'dart:async';

void main() {
  new Future.value(1).then((v) {
    return new Future.error('boom!');
  }).then((v) {
    return new Future.value('hello');
  }).catchError((error) {
    print('error: $error');
  });
}
```

## [](https://flutter-academy.com/async-in-flutter-futures/#code-classlanguage-textfuturewhencompletecode)`Future.whenComplete`

The pattern of using `then().catchError()` may remind you of the common `try-catch` structure, and `Future.whenComplete` is the missing part - `finally`. The callback added with `Future.whenComplete` is always called after the `Future` is completed. `whenComplete` also returns a new `Future` object.

In the code below, the message `done!` is always printed out.

```
import 'dart:async';
import 'dart:math';

void main() {
  var random = new Random();
  new Future.delayed(new Duration(seconds: 3), () {
    if (random.nextBool()) {
      return 100;
    } else {
      throw 'boom!';
    }
  }).then(print).catchError(print).whenComplete(() {
    print('done!');
  });
}
```

## [](https://flutter-academy.com/async-in-flutter-futures/#code-classlanguage-textfuturetimeoutcode)`Future.timeout`

The asynchronous computation of a `Future` may take quite a long time to complete. For example, a remote service call may be blocked due to heavy load on the server. `Future.timeout` allows to time-out a `Future` after specified time limit has passed. The time limit is specified as a `Duration`.

In the code below, the original `Future` will be completed after 5 seconds delay, but the timeout value specified in `timeout` is 2 seconds, so the returned `Future` is completed with a `TimeoutException` error after 2 seconds.

```
import 'dart:async';

void main() {
  new Future.delayed(new Duration(seconds: 5), () {
    return 1;
  }).timeout(new Duration(seconds: 2)).then(print).catchError(print);
}
```

If you run the code, you can see the output as below.

```
TimeoutException after 0:00:02.000000: Future not completed
```

In some cases, when the timeout happens, we may want to use a default value instead. We can use the optional parameter `onTimeout` to specify a value or a `Future` to be the result of the returned `Future` when timed-out.

In the code below, `onTimeout` returns the value `0`, so the output will be `0`, not the value `1` returned by the original `Future`.

```
import 'dart:async';

void main() {
  new Future.delayed(new Duration(seconds: 5), () {
    return 1;
  })
      .timeout(new Duration(seconds: 2), onTimeout: () {
        return 0;
      })
      .then(print)
      .catchError(print);
}
```

# [](https://flutter-academy.com/async-in-flutter-futures/#create-code-classlanguage-textfuturecode)Create `Future`

Now we know how to work with `Future`s in Flutter, but how should we get these `Future` objects? Usually we can get `Future` objects by using libraries. For example, methods `get`, `post` and `put` in the package [http](https://pub.dartlang.org/packages/http) return `Future` objects with the HTTP response. We can also create `Future` objects using constructors. We already see the usage of `Future.delayed`, `Future.value`, `Future.error` in the code examples above.

The code below shows the very basic way to create `Future` objects by passing the computation to run. The created `Future` is completed with the result of the function, either the function's return value or the error thrown in the function. The passed-in function is invoked asynchronously with `Timer.run`.

```
import 'dart:async';

void main() {
  new Future(() {
    var sum = 0;
    for (var i = 0; i < 50000; i++) {
      sum += i;
    }
    return sum;
  }).then(print);
}
```

`Future.sync` also accepts a function as the computation to run, but it runs the function immediately. `Future.sync` is useful when you want to wrap a function call as a `Future`.

`Future.microtask` also accepts a function as the computation to run, but it runs the function asynchronously with `scheduleMicrotask`.

# [](https://flutter-academy.com/async-in-flutter-futures/#code-classlanguage-textasynccode-and-code-classlanguage-textawaitcode)`async` and `await`

From the above code examples, we already know how to use `then` and `catchError` to handle the result of a completed `Future`. However, when multiple `Future`s are chained together, the code becomes hard to read - the infamous [callback hell](http://callbackhell.com/). It's better to use `async` and `await` to work with `Future`s. Code that uses `async` and `await` is asynchronous, but it looks a lot like synchronous code.

`await` waits for the result of a `Future`. To use `await`, the code must be in a function marked as `async`. In the code below, the `main` function is marked as `async`. The `Future` is completed with the value `1` after 3 seconds delay. `var result = await future` waits for the `Future` to complete and assigns the result to `result`.

```
import 'dart:async';

void main() async {
  var future = new Future.delayed(new Duration(seconds: 3), () {
    return 1;
  });
  var result = await future;
  print(result + 1);
}
```

With `async` and `await`, the error handling is done using the traditional `try-catch-finally` structure.

In the code below, we use `try-catch-finally` to handle errors and run complete callbacks.

```
import 'dart:async';

void main() async {
  var future = new Future.delayed(new Duration(seconds: 3), () {
    return new Future.error('boom!');
  });
  try {
    var result = await future;
    print(result + 1);
  } catch (e) {
    print('error: $e');
  } finally {
    print('done!');
  }
}
```

Using `async` and `await` makes it very easy to write code to handle chained `Future`s. In the code below, `fun1`, `fun2` and `fun3` are all `async` functions. We can use `await` to wait for them to complete.

```
void main() async {
  var fun1 = (int v) async => v + 1;
  var fun2 = (int v) async => v - 1;
  var fun3 = (int v) async => v * 10;

  var value = 10;
  value = await fun1(value);
  value = await fun2(value);
  value = await fun3(value);
  print(value);
}
```

The code below uses package `http` as an example of library functions that return `Future`s.

```
import 'package:http/http.dart' as http;

void main() async {
  var response = await http.get('http://google.com');
  print(response.body);
}
```

# [](https://flutter-academy.com/async-in-flutter-futures/#summary)Summary

After reading this article, you should have a basic idea of how to work with `Future`s in Flutter. Below are some key points in this article.

*   Use `then().catchError().whenComplete()` to handle the result of `Future`s.
*   Use `Future.delayed()`, `Future.value()`, `Future.error()`, `Future()`, `Future.sync()` to create new `Future`s.
*   Use `async` and `await` to work with `Future`s in a synchronous way.

# [](https://flutter-academy.com/async-in-flutter-futures/#source-code)Source code

Source code of this article is available on [GitHub](https://github.com/flutter-academy/flutter-async-futures).

</main>
