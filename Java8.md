# 🔹 **Stream API Functions**

## 1. **Intermediate Operations** (return a Stream, lazy)

* `filter(Predicate<T>)` → filters elements by condition
* `map(Function<T,R>)` → transforms elements
* `flatMap(Function<T, Stream<R>>)` → flattens nested streams
* `distinct()` → removes duplicates
* `sorted()` → sorts naturally
* `sorted(Comparator<T>)` → sorts with custom comparator
* `limit(long n)` → takes first `n` elements
* `skip(long n)` → skips first `n` elements
* `peek(Consumer<T>)` → performs action without changing elements (for debugging)

---

## 2. **Terminal Operations** (end the stream, produce result)

* `forEach(Consumer<T>)` → performs action on each element
* `forEachOrdered(Consumer<T>)` → preserves encounter order in parallel streams
* `collect(Collector<T, A, R>)` → gathers elements (e.g., `toList()`, `toSet()`, `joining()`)
* `toArray()` → converts to array
* `reduce(BinaryOperator<T>)` → reduces elements to one value
* `reduce(identity, BinaryOperator<T>)` → reduces with an identity
* `min(Comparator<T>)` → smallest element (Optional)
* `max(Comparator<T>)` → largest element (Optional)
* `count()` → counts elements
* `anyMatch(Predicate<T>)` → true if any element matches
* `allMatch(Predicate<T>)` → true if all elements match
* `noneMatch(Predicate<T>)` → true if no element matches
* `findFirst()` → returns first element (Optional)
* `findAny()` → returns any element (Optional)

---

## 3. **Short-Circuiting Operations**

* Intermediate:

  * `limit(n)`
  * `skip(n)`

* Terminal:

  * `findFirst()`
  * `findAny()`
  * `anyMatch()`
  * `allMatch()`
  * `noneMatch()`

---

## 4. **Collectors (used with collect())**

* `Collectors.toList()`
* `Collectors.toSet()`
* `Collectors.toMap(keyMapper, valueMapper)`
* `Collectors.joining(delimiter)`
* `Collectors.groupingBy(Function)`
* `Collectors.partitioningBy(Predicate)`
* `Collectors.counting()`
* `Collectors.summingInt() / summingLong() / summingDouble()`
* `Collectors.averagingInt() / averagingLong() / averagingDouble()`
* `Collectors.reducing()`
---
