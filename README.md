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
let answer = 42

let my_button = document:select('#my-button') in {
  def my_button << click {
    ...
  }
}
 
let articles = [
  %{ 'title': '...', 'tags': ['tagA', 'tagB', ...], 'words': 420 },
  ...
} 

let stats_per_tag ^%{ ^str: %{'words': ^int, 'articles': ^[]} } =
  articles.map(\article ->
    article['tag'].map(\tag -> %{tag, article} })
  )
  .flatten()
  .group_by(\ta -> ta['tag'])
  .map(\tag, tag_articles -> [
    tag,
    %{'articles': tag_articles.map(\ta -> ta.article),
      'words': tag_articles.map(\ta -> ta.article.words).reduce(+)}
   ])
   .to_dict()

let stats_per_tag =
  articles.index_by(\article -> article['tags']
  .map(\tag, articles -> [
    tag,
    %{'articles': articles,
      'words': articles.map(\article -> article['words']).reduce(+)}
   ])

let articles_by_tag = %{*: []}
for article in articles {
  for tag in article['tags'] {
    articles_by_tag[tag] ++= article
  }
}
let stats_per_tag = %{} ^%{ ^str: %{'words': ^int, 'articles': ^[]} }
for tag, articles in articles_by_tag {
  let stats = %{'articles': articles, 'words': 0}
  for article in articles {
    stats['words'] += article['words']
  }
  stats_per_tag[tag] = stats
}
```
