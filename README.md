## Final Exam Preparation

The problems below are similar to what you can expect for the final exam.

### Higher-Order Functions

1. Implement a Scala function

   ```scala
   def dropWhile[A](p: A => Boolean)(l: List[A]): List[A]
   ```

   that takes a predicate `p` and a list `l` and drops the longest prefix of
   elements of `l` that satisfies `p`. Example:

   ```scala
   scala> dropWhile(_ < 0)(List(-1, -2, 3, -5, 6))
   res0: List[Int] = List(3, -5, 6)
   ```
   
   1. implement `dropWhile` using recursion (i.e., without using
      `foldLeft`/`foldRight`). Also implement a tail-recursive version.

   2. implement `dropWhile` using `foldLeft`/`foldRight`, whichever you find
      appropriate (though, make sure that you compute the correct result).
      
2. Implement a Scala function

   ```scala
   def countSat[A](p: A => Boolean)(l: List[A]): Int
   ```

   that counts the number of elements in `l` for which `p` returns true. Example:

   ```scala
   scala> countSat(_ < 0)(List(-1, -2, 3, -5, 6))
   res0: Int = 3
   
   scala> countSat(_ >= 0)(List(-1, -2, 3, -5, 6))
   res`: Int = 2
   ```
   
   Proceed as in part 1.



### Type Inference and Type Checking

For each of the programs below, answer the following questions:

a. Can you find concrete types for the missing parameter type
   annotations `t1`,...,`t5` such that the given program is well-typed
   according to the typing rules of JakartaScript? If yes, provide one
   such annotation. If no such type annotations exist, explain why.

b. If your answer to (a) is "yes", are your chosen types the only
   annotations that work?  If not, give at least one other choice of
   annotations that also makes the program well-typed.

c. If your answer to (a) is "no", can the program be safely
   evaluated according to the operational semantics of JakartaScript? If
   yes, what value does the program evaluate to? If not, where does the
   evaluation get stuck?

Programs:

1. ```javascript
   const f = function(x: t1) { return x + x; };
   f(3)
   ```
   
2. ```javascript
   const swap = function(f: t1) {
       return function(x: t2) {
       return function(y: t3) { return f(y)(x); };
     };
   };
   const f = function(x: t4) {
     return function(y: t5) { return x; };
   };
   swap(f)
   ```

3. ```javascript
   function(c: t1) {
     c.get = function () { return c.x; };
     c.inc = function () { c.x = c.x + 1; return c; };
     return c;
   }
   ```

4. ```javascript
   function(c: t1) {
     c.get = function () { return c.x; };
     c.inc = function () { c.x = c.x + 1; return undefined; };
     return c;
   }
   ```

### Parameter Passing Modes

Consider the typed variant of JakartaScript that supports the
different parameter passing modes we discussed. For each of the
following programs, say whether the program is well-typed. If not,
provide a brief explanation of the type error.  If yes, compute the
value that the program evaluates to and provide a brief explanation
why you obtain that specific value.


1. ```javascript
   let y = 2;
   const f = function(let x: Num) {
     x = x * x; return x;
   };
   f(y) + y
   ```

2. ```javascript
   let x = 1;
   const y = 2;
   const f = function(name y: Num) {
     return y - y;
   };
   f(x = x + y) + x
   ```
   
3. ```javascript
   let y = 2;
   const f = function(ref x: Num) {
     x = x * x; return x;
   };
   f(y) + y
   ```
  
4. ```javascript
   const y = 2;
   const f = function(ref x: Num) {
     x = x * x; return x;
   };
   f(y) + y
   ```

### Subtyping

1. Decide whether the following subtype relations hold

   a. `Num <: {var f: Num}`

   b. `{var f: Num, const g: Bool} <: {var g: Bool, const f: Num}`

   c. `{var f: Num} <: {var g: Bool, const f: Num}`

   d. `{var g: Bool, const f: Num} <: {var f: Num}`

   e. `{var f: Num} <: {const f: Num}`

   f. `{const f: Num} <: {var f: Num}`

   g. `{var f: Num} <: {var f: Any}`

   h. `{var f: Num} <: {const f: Any}`

   i. `{const f: Num} => {const f: Any} <: {const f: Any} => {const f: Num}`

   j. `{const f: Any} => {const f: Num} <: {const f: Num} => {const f: Any}`

1. Compute the joins and meets of the following pairs of types `(t1, t2)`. 
   Indicate the cases where the meet does not exist.

   a. `t1 = {var f: Bool}`
      `t2 = {const g: Bool}`


   b. `t1 = {var f: Num}`
      `t2 = {var f: Bool}`

   c. `t1 = {var f: Num, var h: Bool}`
      `t2 = {const f: Bool, const k: {var h: Bool} }`

   d. `t1 = {var f: Any, var h: Bool}`
      `t2 = {const f: Bool, const k: {var h: Bool} }`

   e. `t1 = {var f: Num} => {var f: Num}`
      `t2 = {const f: Any} => {const f: Bool}`

   f. `t1 = {var f: Num} => {var f: Any}`
      `t2 = {const f: Bool} => {const f: Bool}`


1. Suppose we extend our type systems for object types with a new kind
   of field mutability: a field `f` with mutability `sink` is only
   allowed to be written to, but not to be read from. For instance, the
   following function should be well-typed:

   ```javascript
   function (x: { sink f: Num, const g: Num }) {
     x.f = 3, x.g
   }
   ```

   On the other hand, the next function should not be well-typed
   because it tries to read from the `sink` field `f`:

   ```javascript
   function (x: { sink f: Num, const g: Num }) {
     x.f
   }
   ```

   a. Provide new typing rules for field dereference expressions and
      field assignment expressions that take into account the new kind
      of mutability (the semantics of `const` and `let` fields should
      remain unchanged).
   
      ```
               ?
      ------------------TypeFldDeref
      Gamma |- e.f : tau

                  ?
      ------------------------ TypeFldDeref
      Gamma |- e1.f = e2 : tau
      ```
      
   b. We can think of a `let` field as a combination of a `sink`
      field and a `const` field. Hence, it makes sense to extend our
      subtyping relation with the ability to *demote* a `let` field to
      a `sink` field, similar to our existing rule `SubObjMut`, which demotes a `let` field to a `const` field:
   
      ```
      { mut_1 f_1: tau_1, ..., let f: tau, ..., mut_n f_n: tau_n } 
      <:
      { mut_1 f_1: tau_1, ..., sink f: tau, ..., mut_n f_n: tau_n } 
      ```
      
      We can also introduce a form of depth subtyping for `sink` fields:
      
      ```
                                 ?
      --------------------------------------------------------
      { mut_1 f_1: tau_1, ..., sink f: tau, ..., mut_n f_n: tau_n } 
      <:
      { mut_1 f_1: tau_1, ..., sink f: tau', ..., mut_n f_n: tau_n } 
      ```
      
      What is the correct premise for this rule? Is it `tau <: tau'`
      or `tau' <: tau`? Only one of these two choices is
      correct. Provide a program that shows how the incorrect choice
      would break the soundness of the type system.
      
      
      
### State Monad

Consider the following function `fib` that computes the `n`-th Fibonacci
number.

```scala
def fib(n: Int): Int = if (n <= 1) n else fib(n - 1) + fib(n - 2)
```

The implementation of `fib` is quite naive. Its running time is exponential in `n` due to the two recursive calls. If we expand the recursive call `fib(n - 1)` we obtain:

```scala
if ((n - 1) <= 1) n - 1 else fib(n - 2) + fib(n - 3)
```

Thus, the second recursive call `fib(n - 2)` recomputes a result that
has already been computed in the first recursive call `fib(n - 1)`. A
simple idea to improve the implementation is to memoize the already
computed values of `fib` in a cache. Before we do the recursive calls,
we check whether the value is already in the cache to avoid
unnecessary recomputation. The following code implements this idea:

```scala
type Cache = Map[Int, Int]

def fib(n: Int) = fibMemo(n, Map.empty)._1

def fibMemo(n: Int, m: Cache): (Int, Cache) =
  if (n <= 1) (n, m) else {
    m.get(n) map (n => (n, m)) getOrElse {
      val (r, mr) = fibMemo(n - 1, m)
      val (s, ms) = fibMemo(n - 2, mr)
      val t = r + s
      (t, ms + (n -> t))
    }
  }
```

1. Complete the following variant of this implementation that uses a
   state monad to hide the threading of the memoization cache:

   ```scala
   def fib(n: Int) = fibMemo(n)(Map.empty)._1

   def fibMemo(n: Int): State[Cache,Int] =
     if (n <= 1) State.insert(n) else {
       for {
         c <- State.read { m: Cache => m.get(n) }
         t <- c map State.insert[Cache,Int] getOrElse (for {
           ???
         } yield ???)
         _ <- State.write { m: Cache => ??? }
       } yield t
     }
   ```

2. A simpler and even more efficient solution is to make the function
   `fib` tail-recursive so that it only requires a single recursive
   call. Complete the following code to obtain such a tail-recursive
   implementation. Your implementation should run in constant space and
   in time `O(n)`:

   ```scala
   def fib(n: Int) = fibTail(n, ???)

   def fibTail(n: Int, ???): Int = {
     ???
   }
   ```
   
   Hint: You may need to add more than one parameter to `fibTail` to
   accumulate intermediate results across recursive calls. If you have
   trouble coming up with a solution, then first think about how to
   implement the `fib` function using a single while loop. Then
   transform the loop into a tail-recursive function.