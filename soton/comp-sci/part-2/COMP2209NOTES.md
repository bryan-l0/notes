# Functional Programming with Haskell

## Grades

**Labs - 20%**

**Coursework - 40%**

**Exam - 40%**

## Overview

Imperative Programs: Instructions on how to mainipulate memory

Functional Content: What is the solution to the problem

Implementational Content: How to solve the problem

Functional Programs:

- High level behaviour and low level implementation
- Humans focusing on the high level functional behaviour
- Compiler focusing on memory management and implementation details

## Basic Types and Functions

### Arithmetic Functions: 

    + * - div mod abs

### Comparison Functions: 

    > >= == /= <= <

### List Functions: 

  	head xs (select first element)
  	tail xs (remove first element)
  	length xs (length of list)
  	xs !! n (select nth element)
  	take n xs (select first n elements)
  	drop n xs (remove first n elements)
  	xs ++ ys (appends two lists)
  	sum xs (sums elements in list)
  	product xs (calculates product of elements in list)
  	reverse xs (reverses list)
  	repeat x (creates infinite list of xs)

### Boolean Functions:

  	&& :: Bool -> Bool -> Bool
  	|| :: Bool -> Bool -> Bool
  	not:: Bool -> Bool
  	if bexp then exp1 else exp2

### Types

    e :: t - Expression e that produces value with type t

### Chars

    ord :: Char -> Int
    chr :: Int -> Char

### Strings

Essentially a list of chars. 

    "hello" :: [Char]

### Numbers

Int: -2^63 to 2^63 - 1

    floor, ceiling, round :: Float -> Int
    fromInt :: Int -> Float

### List Formation

    (:) :: a -> [a] -> [a]
    [] :: [a]

    1 : 2 : 3 : []

is equivalent to 

    [1, 2, 3]

### Tuples

(T1,T2,...,Tn) is the type of n-tuples, with value (v1,v2,...,vn), and each vi has type Ti

### Functions

T -> U - Takes value of type T and returns value of type U

### Curried Functions

    add' :: Int -> (Int -> Int)
    add' x y = x + y

    mult :: Int -> (Int -> (Int -> Int))
    mult x y z = x * y * z

Eq Class: Types that support (==), (/=)
Ord Class: Types that support (<), (>), (<=), (>=), min, max
Show Class: Types that support the function show :: a -> String
Read Class: Types that support the function read :: String -> a
Num Class: Types that represent numbers and support (+), (-), (\*), negate, abs, signum
Integral Class: Types that support div and mod
Fractional Class: Types that support (/) and recip

### Guarded Equations

    abs5 :: (Ord a, Num a) => a -> a
    abs5 n | n >= 0    = m
           | otherwise = -m
     where m = n * 5

### Pattern Matching

    fetch :: Int -> [a] -> a
    fetch _ [] = error "Empty List"
    fetch 0 (x:_) = x
    fetch n (_:xs) = fetch (n - 1) xs

### Lambda Expressions

    add :: Num a => a -> a -> a
    add = \x -> (\y -> x + y)

    const :: a -> b -> a
    const x = \_ -> x

    addNewLine :: (String,String) -> (String,String)
    addNewLine (s,s') = (f s,f s')
     where f = \x -> (x++'\n')

### List Comprehension

    [x^2 | x <- [1..5]]
    [(x,y) | y <- [4,5], x <- [1,2,3]]

    factors :: Int -> [Int]
    factors n = [x | x <- [1..n], n `mod` x == 0]

### Recursion

    fac 0 = 1
    fac n = n * fac (n - 1)

    product :: Num a => [a] -> a
    product [] = 1
    product (x:xs) = x * product xs

### Mutual Recursion

    evens :: [a] -> [a]
    evens [] = []
    evens  (x:xs) = x : odds xs

    odds :: [a] -> [a]
    odds [] = []
    odds (x:xs) = evens xs

### Tail Recursion (May not optimal due to Haskell's lazy evauluation)

    fac' :: Int -> Int -> Int
    fac' acc 0 = acc
    fac' acc n = fac' (n * acc) (n - 1)

## Higher Order Functions

### Zip

    zip :: [a] -> [b] -> [(a,b)]
    zip [] _ = []
    zip _ [] = []
    zip (x:xs) (y:ys) = (x,y) : zip xs ys

### Map

    map :: (a -> b) -> [a] -> b
    map f [] = []
    map f (x:xs) = f x : map f xs

### Filter

    filter :: (a -> Bool) -> [a] -> [a]
    filter p [] = []
    filter p (x:xs) | p x       = x : filter p xs
                    | otherwise = filter p xs

### $ Operator

    ($) :: (a -> b) -> a -> b
    f $ x = f x

Associates to the right instead of left

    sqrt $ a^2 + b^2 = sqrt (a^2 + b^2)

### Fold(r/l)

    foldr :: (a -> b -> b) -> b -> [a] -> b
    foldr f v [] = v
    foldr f v (x:xs) = f x (foldr f v xs)

    sum :: Num a => [a] -> a
    sum = foldr (+) 0

    product :: Num a => [a] -> a
    product = foldr (*) 1

    and :: [Bool] -> Bool
    and = foldr (&&) True

    foldl :: (b -> a -> b) -> b -> [a] -> b
    foldl f acc [] = acc
    foldl f acc (x:xs) = foldl f (f acc x) xs

### Type Synonyms

    type Pos = (Int, Int)
    origin :: Pos
    origin = (0, 0)
    moveLeft :: Pos -> Pos
    moveLeft (x, y) = (x-1, y)

    type Assoc k v = [ (k, v) ]
    find :: Eq k => k -> Assoc k v -> v
    find k t = head [ v | (k', v) <- t, k == k' ]

This can not be partially applied.

They can be nested but not recursed.

    type Pos = (Int, Int), type Translation = Pos -> Pos
    type Tree = (Int, [Tree]) -- Does not work

## New Types with Constructors

Lets say we want a type Foo that behaves like an Int but doesn't take an Int, e.g. sanitised inputs.

We can introduct a constructor into the value of the type. This allows for type safety.

    data Foo = F Int - :t F 5 =: F 5 :: Foo
    data Day = Mon | Tue | Wed | Thu | Fri | Sat | Sun
    isWeekend :: Day -> Bool
    isWeekend Sat = True
    isWeekend Sun = True
    isWeekend _ = False

    isWeekday :: Day -> Bool
    isWeekday = not.isWeekend

### Constructors with Multiple Parameters

    data Shape = Circle Float | Rect Float Float
    square :: Float -> Shape
    square n = Rect n n

    area :: Float -> Shape
    area (Circle r) = pi * r^2
    area (Rect x y) = x * y

### Values of the Maybe type is either the nullary constructor Nothing or a value with type Just

    data Maybe a = Nothing | Just a

    safeHead :: [a] -> Maybe a
    safeHead [] = Nothing
    safeHead xs = Just (head xs)

Declaring types like Foo above has a computational cost, hence we can use the newtype declaration. It can only be used for types of a single unary constructor.

    newtype Foo = F Int
    add :: Foo -> Foo -> Foo
    add (F n) (F m) = F (n + m)
    add (F 3) (F 5) returns (F 8)

### Recursive Types

    data MyList a = Empty | Cons a (MyList a)
    dup Empty = Empty
    dup (Cons m ms) = Cons m $ Cons m (dup ms)

    data Tree a = Leaf a | Node (Tree a) a (Tree a)
    t :: Tree Int
    t = Node (Node (Leaf 1) 3 (Leaf 4)) 5 (Node (Leaf 6) 7 (Leaf 9))
    contains :: Eq a => a -> Tree a -> Bool
    contains x (Leaf y) = x == y
    contains x (Node l y r) = x == y || contains x l || contains x r
    flatten :: Tree a -> [a]
    flatten (Leaf y) = [y]
    flatten (Node l y r) = flatten l ++ [y] ++ flatten r

    data LTree a = Leaf a | Node (LTree a) (LTree a)
    data ITree a = Leaf | Node (ITree a) a (ITree a)
    data DTree a b = Leaf a | Node (DTree a b) b (DTree a b)
    data MTree a = Node a [MTree a]

### Writing Functions on all Tree Types

    class Tree a where
      flatten :: Tree a -> [a]

    class Eq a => Ord a where
      (<), (<=), (>), (>=) :: a -> a -> Bool
      min, max :: a -> a -> Bool
      min x y | x <= y = x
              | otherwise = y
      max x y | x <= y = y
              | otherwise = x

    class CONSTRAINTS => NAME a where
      function :: type
      function :: type
      ...
      function = default definition
      function = default definition
      ...

Declaring Instances - use the instance keyword, and implement all undefined methods

    instance Ord Bool where
      False < True = True
      _     < _    = False
      b <= c = (b < c) || (b == c)
      b > c = c < b
      b >= c = c <= b

    class Tree a where
      flatten :: Tree a -> [a]

    instance Tree LTree where
      flatten (Leaf a) = [a]
      flatten (Node l r) = flatten l ++ flatten r

    instance Tree ITree where
      flatten (Leaf) = []
      flatten (Node l y r) = flatten l ++ [y] ++ flatten r

    instance Tree MTree where
      flatten (Node a cs) = a : map flatten cs

### Deriving Instances

    data Day = Mon | Tue | Wed | Thu | Fri | Sat | Sun
      deriving (Eq, Ord, Show, Read)

This implements functions in those classes by following the structure of type Day.

e.g. Show will print the string name, < is defined s.t. Mon < Tue ... < Sun.

If the type is declared to derive a class, all the parameters must derive that class too.

    data Shape = Circle Float | Rect Float Float
      deriving (Eq, Ord, Show, Read)

This doesn't work because (a -> a) is not an instance of the Eq class so no default == implementation can be found.

    data DocumentedFunction a = Doc String (a -> a)
      deriving Eq

### Red Black Trees

    data RBTree a = Leaf a | RedNode a (RBTree a) (RBTree a) | BlackNode a (RBTree a) (RBTree a)
    blackH (Leaf _ _ ) = 0
    blackH (RedNode _ l r) = maxlr
    blackH (BlackNode _ l r) = 1 + maxlr
      where maxlr = max (blackH l) (blackH r)

### AST for Arithmetic Expressions

    data Expr = Val Int | Add Expr Expr | Sub Expr Expr
    eval :: Expr -> Int
    eval (Val n) = n
    eval (Add e1 e2) = eval e1 + eval e2
    eval (Sub e1 e2) = eval e1 - eval e2

    data Prop = Const Bool | Var Char | Not Prop | And Prop Prop | Imply Prop Prop
    eval :: Subst -> Prop -> Bool
    eval s (Const b) = b
    eval s (Var c) = find c s
    eval s (Not p) = not $ eval s p
    eval s (And p q) = eval s p && eval s q
    eval s (Imply p q) = eval s p <= eval s q

### Higher Order Functions and Trees

    data LTree a = Leaf a | Node (LTree a) (LTree a)
    lTMap :: (a -> b) -> LTree a -> LTree b
    lTMap g (Leaf x) = Leaf (g x)
    lTMap g (Node l r) = Node (lTMap g l) (lTMap g r)

    data ITree a = Leaf | Node a (ITree a) (ITree a)
    iTMap :: (a -> b) -> ITree a -> ITree b
    iTMap g (Leaf) = Leaf
    iTMap g (Node x l r) = Node (g x) (iTMap g l) (iTMap g r)

    data MTree a = Node a [MTree a]
    mTMap :: (a -> b) -> MTree a -> MTree b
    mTMap g (Node x ts) = Node (g x) (map (mTMap g) ts)

## Functors

Represents types for which makes sense to have a map function.

    class Functor f where
      fmap :: (a -> b) -> f a -> f b
    instance Functor [] where
      fmap = map
    instance Functor MTree where
      fmap = mTMap
    instance Functor Maybe where
      fmap _ Nothing = Nothing
      fmap g (Just x) = Just (g x)
    altMap :: Functor t => (a -> b) -> (a -> b) -> t a -> t b

t is now any type that supports a map

### Expected Properties of fmap

    fmap id = id
    fmap (g.h) = fmap g . fmap h

## Zippers

    data Direction a = L a (Tree a) | R a (Tree a)
    type Trail a = [Direction a]

    goLeft, goRight, goUp :: (Tree a, Trail a) -> (Tree a, Trail a)
    goLeft (Node x l r, ts) = (l, L x r:ts)
    goRight (Node x l r, ts) = (r, R x l:ts)
    goUp (t, L x r : ts) = (Node x t r, ts)
    goUp (t, R x l : ts) = (Node x l t, ts)

    data Direction a = L a (Tree a) | R a (Tree a)
    type Zipper a = (Tree a, Trail a)

    modify :: (a -> a) -> Zipper a -> Zipper a
    modify f (Node x l r, ts) = (Node (f x) l r, ts)
    modify f (Leaf, ts) = (Leaf, ts)

    attach :: Tree a -> Zipper a -> Zipper a
    attach t (_, ts) = (t, ts)

    goRoot :: Zipper a -> Zipper a
    goRoot (t, []) = (t, [])
    goRoot z = goRoot (goUp z)

## Graphs

### Indexed Approach - Data.Graph

    type Table a = Array Vertex a
    type Graph = Table [Vertex]

### Cyclic Dependency Approach

#### Loopy Lists: tail of first node is the initial list

    repeat x = let xs = x : xs in xs
    cyclic = let x = 0 : y
                 y = 1 : x in x

#### Double Linked Lists

    data DLList a = Empty | Node a (DLList a) (DLList a) - 

Problem: Can't build prev and next before cur

    mkDLList :: [a] -> DLList a
    mkDLList [] = Empty
    mkDLList [x] = Node x Empty Empty
    mkDLList [x1, x2] = let node1 = Node x1 Empty node2
                            node2 = Node x2 node1 Empty
     in node1
    mkDLList [x1, x2, x3] = let node1 = Node x1 Empty node2
                                node2 = Node x2 node1 node3
                                node3 = Node x3 node2 Empty
     in node1

#### General Definition of mkDLList

    mkDLList' :: [a] -> DLList a -> DLList a
    mkDLList' [] prev = Empty
    mkDLList' (x:xs) prev = let cur = Node x prev (mkDLList' xs cur)
     in cur

    mkDLList :: [a] -> DLList
    mkDLList xs = mkDLList' xs Empty


### Cyclic Definitions for Graphs

Basic cyclic graph structure utilising a table of nodes with their outgoing edge:

    data Graph a = GNode a (Graph a)
    mkGraph :: [(a, Int)] -> Graph a
    mkGraph table = table' !! 0
     where table' :: [Graph a]
           table' = map (\(x, n) -> GNode x (table' !! n)) table

Generalised to work with multiple outgoing edges:

    data GGraph a = GGNode a [GGraph a]
    mkGGraph :: [(a, [Int])] -> GGraph a
    mkGGraph table = table' !! 0
     where 
      table' = map (\(x, ns) -> GGNode x (map (table' !!) ns)) table


### Graphs - Inductive Graphs

Functional Graph Library (FGL) with Data.Graph.Inductive.Graph



## Equational Reasoning

Commutativity: x y = y x
Associativity: x + (y + z) = (x + y) + z
Left Distributivity: x (y + z) = xy + xz
Right Distributivity: (x + y) z = xz + yz


### Reasoning with Bool
Proving not (not b) = b

    not :: Bool -> Bool
    not True = False
    not False = True

    not (not True) = not (False)
                   = True
    not (not False) = not (True)
                    = False
    => not (not b) = b

Taking cases into account, e.g. isZero

    isZero :: Int -> Bool
    isZero 0 = True   =>  isZero 0 | True   = True
    isZero n = False  =>  isZero n | n /= 0 = False

Cases need to be interpreted by conjoining the negation of the previous case guards to them.


### List Example

#### Proving reverse xs = xs for any singleton list xs

    reverse :: [a] -> [a]
    reverse [] = []
    reverse (x:xs) = reverse xs ++ [x]

    reverse [x]
    = {syntactic sugar} reverse (x : [])
    = {defn of reverse} reverse [] ++ [x]
    = {defn of reverse} [] ++ [x]
    = {defn of ++} [x]  (++) :: [a] -> [a] -> [a], [] ++ ys = ys, (x:xs) ++ ys = x : (xs ++ ys)
    => reverse [x] = [x]

However, this has only proved reverse [x] = [x] for some variable x.

The logical principle of generalisation says this is fine if there are no assumptions made about the variable x, because x can be instantiated by any value, and the equation is true for x, then the equation is true for any value.


### Structural Induction

Proof by induction uses the fact that natural numbers are a Structured Data Type.

    data Nat = Zero | Succ Nat

To prove P(n) holds for all values of type Nat, we prove:

- P(Zero)
- P(Succ x) under the assumption that P(x)

The semantics of algebraic data types are such that this principle is valid - this covers all values of type Nat.

    add :: Nat -> Nat -> Nat
    add Zero m = m
    add (Succ n) m = Succ (add n m)

#### Associativity Proof

We need to show for all x, y, z:

    add x (add y z) = add (add x y) z

We can do this by induction.

##### Base Case (Zero)

    add Zero (add y z)
    = { definition of add }

    add y z
    = { definition of add 'backwards' }

##### Inductive Case (Succ x)

    add (Succ x) (add y z)
    = { definition of add }
    Succ (add x (add y z)))
    = { inductive hypothesis }
    Succ (add (add x y) z)
    = { definition of add 'backwards' }
    add (Succ (add x y) z)
    = { definition of add 'backwards' }

Proof by induction works on any structured data type.

To prove P(x) for all values in type T, it is sufficient to prove:
- P(C) for all constructors C in T that have no recursive arguments.
- P(F x) assuming that P(x) holds for all constructors F in T that have a single recursive argument.
- P(G x y) assuming that P(x) and P(y) hold for all constructors G in T that have two recursive arguments.

#### Induction on Lists

Consider the datatype Lists: 

    ( data [ a ] = [] | (:) a [ a ])

It has a constructor [], as well as (:) whose second argument is recursive.

To prove P(xs) for all lists we must prove:

- P([])
- P(x : xs) under the assumption that P(xs)

#### Involutive Proof


