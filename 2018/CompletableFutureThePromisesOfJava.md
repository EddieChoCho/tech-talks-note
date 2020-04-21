# CompletableFuture: The Promises of Java

### Speaker: Venkat Subramaniam

* never use completeableFutrue without timeout, success on timeout, fail in timeout

completeOnTimeout(500, 3, TimeUnit.SECONDS);
orTimeout(3, TimeUnit.SECONDS);

complete
completeExceptionally(new RuntimeException())

* compose, combine

reduce == thenCombine

create(2).thenCombine(create(2), (a,b) -> a + b)
.thenAccept(Systme.out::println);

one to one function: map
Stream<T>.map(f11) => Stream<R>

onte to many function: flatMap
Stream<T>.map(f1n) => Stream<Stream<R>>
Stream<T>.flatMap(f1n) => Stream<R>

create(2).thenCompose(data -> create(data))
.thenAccept(Systme.out::println);

* TBD...