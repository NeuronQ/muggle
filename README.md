# The Muggle Programming Language

An ultra-general-purpose programming language designed to be friendly to non-professional programmers ("muggles"), and at the same time productive for professional software developers.

**Main audience:**
- professionals writing code without having a Computer Science or Software Engineering background - think physicists, astronomists, chemists, biologist, machine learning reasearches, economists, psychologist etc., and also... hobbyists
- self-thought software developers

**Problems it aims to solve:**
- do not sacrifice **correctness**, **code undertandability** and **debuggability** for *"ease of use"* or *"ease of learning":* we ***can have the cake and eat it too!*** :cake:
- have a **friendly and familiar syntax**: not in the sense of looking a lot like other programming languages, but in the sense that the notation is *similar* to that used in other domains like *math* or **physics**, *easy to learn for mosts*, and, *very* easy to read and understand (by other people than who wrote the code originally)
- **make it easy to understand the intention/reason, general logic and cotrol flow of a program** even in the absence of extensive comments and documentation
- ability to optionally but easily code in some **logical correctness guarantees** (types, constraints etc.), even if these *may be severely detrimental to performance* (eg. if "typing is checked at runtime" etc. - someone will probably be able to optimize the code later, once you get it *correct!*)
- **target multiple platforms** including **browsers** (transpilation to *readable* JavaScript), **mobile** (both Android and iOS), **desktop** and **server** (transpiling to JS and using Node.js to run might be enough for start)
- **highly interactive** for *web* and *desktop* version - must feel like an **interpreted language** with a **capable REPL** (all running apps should be able to "spawn a console" and have injected into them at runtime through it) and **integrated debugger**
- **batteries included** - capable and comprehensive standard library, including solid solutions for **web and network**, **math oriented programming** and **machine learning**
- ability to **user existing ecosystems** - NPM and PyPI are *a must* for start (depending on transpilation target, only one of them can be available, but if parts of the standard library use them as a base, those parts must *work identically with both of them*, regardless of the complexities and effort needed to make this happen)
- **powerful abstractions capabilities** (including functional-programming, meta-linguistical aka "macros", polymorphism, message-passing-style-OOP), that should be *easy to use* (eg. for "library users"), even if they may be clunkier to implement
- **simple but good support for concurrency** - figure out a way to combine the intuitiveness of async/await with the simplicity/power of channels (think go-routines)
- optimise for **programmer happiness** but take into account that the language will be used by *two* classes of programmers at the same time: (1) *domain experts* or *generalists* and (2) *professional software developers* - both need to also *be happy with the code written by the other camp!*
- **multiple syntaxes** with a *standardized intermediary syntax for ASTs*

**A quick taste of Muggle:**

```
let answer = 42;

let my_button = document | select('#my-button') in {
  def my_button << click {
    ...
  }
};
 
let articles = [
  %{ 'title': '...', 'tags': ['tagA', 'tagB', ...], 'words': 420 }%,
  ...
];

// functional naive
let stats_per_tag ^%{ ^str: ^%{'words': ^int, 'articles': ^[]} } =
  articles | map article {
    article.tags | map tag { %{tag, article}% }
  })
  | flatten
  | group_by tag_article { tag_article.article }
  | map tag__tag_articles { [
    tag__tag_articles[0],
    %{ 'articles': tag__tag_articles[1] | map ta { ta.article },
       'words': tag__tag_articles[1] | map ta { ta.article.words } | reduce (+) }%
  ] }
  | to_dict;

// functional with destructuring
let stats_per_tag ^%{ ^str: ^%{'words': ^int, 'articles': ^[]} } =
  articles | map article {
    article.tag | map tag { [tag, article] }
  }
  | flatten
  | group_by [tag, article] { tag }
  | map [tag, tag_articles] { [
    tag,
    tag_articles | map tag, article { article }
  ] }
  | map tag, articles { [
    tag,
    %{ articles
       'words': articles | map a { a.words } | reduce (+) }%
   ] }
  | to_dict;

// functional idiomatic
let stats_per_tag ^%{ ^str: ^%{'words': ^int, 'articles': ^[]} }
  = articles | index_by(getter('tags'))
  | map tag, articles { [
    tag,
    %{ articles
       'words': articles | map_reduce(getter('tags'), (+)) }%
  ] };

// imperative, procedural, mutable aka "old school"
var stats_per_tag ^%{ ^str: ^%{'words': ^int, 'articles': ^[]} } = %{};
// `%default([]){}` is just sugar for `default_dict([])()`
var articles_by_tag = %default([]){} // type ^%default{^str: ^[]};
articles | for article {
  article.tags | for tag {
    articles_by_tag[tag] ++= article;
  }
};
articles_by_tag | for tag, articles {
  let stats = %{'articles': articles, 'words': 0};
  articles | for article {
    stats.words += article.words;
  };
  stats_per_tag[tag] = stats;
};

// imperative-style but immutable, aka "list/dict comprehension"
let articles_by_tag = articles | each article {
  article.tags | each tag { => tag: article }
};
let stats_per_tag = articles_by_tag | each tag, articles {
  => tag: %{
    articles,
    words: sum(articles | ~each a { => a.words })
  }
};
```
# Types

We're going to separate the possible type constraints (aka "the type system"),
for the collection of core baked-in standard types (that satisfy some of these contraints).

## Type constraints

* its **Interface** - what function/methods it supports (interaces don't need to
  be implemented explictly by a type, we use *duck typing* like in Go and in dynamic laguages);
  we specify an interface like so: `^Enumerable` *-- not that "inteface duck-typing" is
  basically a form of structural typing*

* **structural contraints** - defined like `^{ str: {'words': int, 'articles': []} }`
  *-- obviously another form of structural typing (ight contain dependent typing features)**

* we can also have **named structural contraints** - like `^.Foo`

* explicit types, useful mainly for working with ADTs - like `^:Node`

## Core types

Scalars:

```
type interface Number {
  include Comparable;
  (+) ^^ (Int, Int) -> Int;
  (/) ^^ (Int, Int) -> Int?;
}

// Implementations: Int8, UInt8, Int64, BigInt etc.
```

```Str```

```Char```

```Float```

```Bool```

## Compound

```
let li = [1, "one", true];
let nos ^Number = [1, 2+3i, 1.04, 2e-3];
let ints ^Int8 = [2; 3; 4];

let pair = ("x", 42);

let set_of_words = #{"yes", "no"};

type rec Point(x ^Float, y ^Float);
let p1 = Point(1.1, 2);

fun (Point) quadrant() {
  if (this.x >= 0) {
    if (this.y >= 0) { 1 }
    else { 4 }
  } else {
    if (y >= 0) { 2 }
    else { 3 }
  }
}
let qdr = p1:quadrant;

type class Cat(
  // these two have getters/setters generated
  name ^Str,
  age ^Int,
  _code ^Str, // private (no getter/setter)
);
meth (Cat) say(msd ^Str) {
}
```
