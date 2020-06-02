# Quiz 4

## Question 2

Consider the following predicate.

```
speaks(sue,english).
speaks(sue,spanish).
speaks(bob,spanish).
speaks(maria,french).
```

Based on speaks/2, answer the following questions. Hint: Requiring the inequality of two variables X and Y can be expressed using the subgoal X \= Y.


(a) Write a goal to find all people who speak Spanish.

> `speaks(X, spanish).`

(b) Define a predicate talk/2 that finds pairs of people that can talk to one another in the same language.

> `talk(X, Y) :- speaks(X, Z), speaks(Y, Z).`

(c) Define a predicate bilingual/1 that yields true for people that can speak two languages.

> `billingual(X) :- speaks(X, B), speaks (X, C), B \= C.`

## Question 3

The following predicates that describe songs (first argument) and their performers (second argument) offered by two different streaming services.

```
itunes(s,p).
itunes(t,p).
itunes(u,q).

spotify(s,p).
spotify(v,q).
spotify(u,q).
```

(a) Define a predicate songs/1 that can produce all the songs available on any of the two streaming services.

> Assumption: "all the songs available on any of the two streaming services." == both streaming services. Logical structure of this sentance is ambiguous in english. "either of the two" or "one of the two" are more precise.
> `songs(X) :- itunes(N, Y); songs(X) :-  spotify(Z, W).`

(b) Using the predicate in (a), write a goal/query that lists all songs that can be listened to, irrespective of the streaming service that offers it.

> `songs(none).`

(c) Define a predicate performedBy/2 that lists all songs by a specific performer that are available on any of the two streaming services.

> NOTE: composer passed in arg B: `performedBy(A, B) :- itunes(T, B); spotify(U, B).`

(d) Using the predicate in (c), write a goal/query that lists all songs  performed by "p", irrespective of the streaming service that offers the song.

> `performedBy(none, p).`

## Question 4

The predicate answer/3 gives facts about how students have answered numbered multiple-choice questions (for example, answer(john,1,b) says that John has selected answer b for question 1). The predicate key/2 shows the correct answer for each question.

```
answer(john,1,b).  answer(mary,1,c).  key(1,c).
answer(john,2,a).  answer(mary,3,b).  key(2,a).
answer(john,3,b).  answer(tim,3,c).   key(3,b).
```

These predicates are the basis for the following questions.

(a) Define a predicate correct/2 that determines for a given question all the students who have answered that question correctly.

> `correct(A, B) :- answer(X, A, B).`

(b) Define a predicate discuss/2 that is true for those pairs of students who have answered a particular question differently.

> `discuss(A, B) :- answer(X, A, B), answer(Y, A, Z), Z \= B, X \= Y.`

(c) How many answers will Prolog produce for the goal discuss(X,Y)?

> 6