Assignment B4 Option A Exercise 2
================

``` r
library(stringr) # includes functions required for string manipulation
library(dplyr) # includes case_when() function vectorize multiple if_else statements
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(tibble) # includes tibble() function to create tibbles from scratch
library(testthat) # includes functions required to test and evaluate function behaviors
```

    ## 
    ## Attaching package: 'testthat'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     matches

# Go Loco Language : Pig Latin Derivative

## 1.0 Language Architecture

**Go Loco** applies a set of explicit rules to translate words, such
that each word undergoes a *rearrangement* and *addition*
transformation. The transformation however will differ slightly
depending on the original word structure.

### 1.1 Rearrangement Rules

1.  **Words that begin with a consonant, followed by a vowel**:

Move the consonant/vowel pair to the end of the word and then repeat the
consonant/vowel pair.  
Retain the first vowel at the front of the word.

2.  **Words that begin with a consonant cluster, followed by a vowel**:

Move the consonant cluster and following vowel to the end of the word
and then repeat this consonant cluster/vowel sound.  
Retain the first vowel at the front of the word.

3.  **Words that begin with a vowel**:

Move the vowel to the end and with the consonant following it.  
Repeat this consonant/vowel pair (consonant first) twice.

### 1.2 Addition Rules:

1.  Add “loco” to the end of every word.

## 2.0 Examples of Go Loco

### 2.1 Single Word Examples

banana -\> nanababaloco  
apple -\> pleapapaloco

### 2.2 Translating `words` data set in `stringr` package

The following list is the original `words` data set found in the
`stringr` package:

``` r
words
```

    ##   [1] "a"           "able"        "about"       "absolute"    "accept"     
    ##   [6] "account"     "achieve"     "across"      "act"         "active"     
    ##  [11] "actual"      "add"         "address"     "admit"       "advertise"  
    ##  [16] "affect"      "afford"      "after"       "afternoon"   "again"      
    ##  [21] "against"     "age"         "agent"       "ago"         "agree"      
    ##  [26] "air"         "all"         "allow"       "almost"      "along"      
    ##  [31] "already"     "alright"     "also"        "although"    "always"     
    ##  [36] "america"     "amount"      "and"         "another"     "answer"     
    ##  [41] "any"         "apart"       "apparent"    "appear"      "apply"      
    ##  [46] "appoint"     "approach"    "appropriate" "area"        "argue"      
    ##  [51] "arm"         "around"      "arrange"     "art"         "as"         
    ##  [56] "ask"         "associate"   "assume"      "at"          "attend"     
    ##  [61] "authority"   "available"   "aware"       "away"        "awful"      
    ##  [66] "baby"        "back"        "bad"         "bag"         "balance"    
    ##  [71] "ball"        "bank"        "bar"         "base"        "basis"      
    ##  [76] "be"          "bear"        "beat"        "beauty"      "because"    
    ##  [81] "become"      "bed"         "before"      "begin"       "behind"     
    ##  [86] "believe"     "benefit"     "best"        "bet"         "between"    
    ##  [91] "big"         "bill"        "birth"       "bit"         "black"      
    ##  [96] "bloke"       "blood"       "blow"        "blue"        "board"      
    ## [101] "boat"        "body"        "book"        "both"        "bother"     
    ## [106] "bottle"      "bottom"      "box"         "boy"         "break"      
    ## [111] "brief"       "brilliant"   "bring"       "britain"     "brother"    
    ## [116] "budget"      "build"       "bus"         "business"    "busy"       
    ## [121] "but"         "buy"         "by"          "cake"        "call"       
    ## [126] "can"         "car"         "card"        "care"        "carry"      
    ## [131] "case"        "cat"         "catch"       "cause"       "cent"       
    ## [136] "centre"      "certain"     "chair"       "chairman"    "chance"     
    ## [141] "change"      "chap"        "character"   "charge"      "cheap"      
    ## [146] "check"       "child"       "choice"      "choose"      "Christ"     
    ## [151] "Christmas"   "church"      "city"        "claim"       "class"      
    ## [156] "clean"       "clear"       "client"      "clock"       "close"      
    ## [161] "closes"      "clothe"      "club"        "coffee"      "cold"       
    ## [166] "colleague"   "collect"     "college"     "colour"      "come"       
    ## [171] "comment"     "commit"      "committee"   "common"      "community"  
    ## [176] "company"     "compare"     "complete"    "compute"     "concern"    
    ## [181] "condition"   "confer"      "consider"    "consult"     "contact"    
    ## [186] "continue"    "contract"    "control"     "converse"    "cook"       
    ## [191] "copy"        "corner"      "correct"     "cost"        "could"      
    ## [196] "council"     "count"       "country"     "county"      "couple"     
    ## [201] "course"      "court"       "cover"       "create"      "cross"      
    ## [206] "cup"         "current"     "cut"         "dad"         "danger"     
    ## [211] "date"        "day"         "dead"        "deal"        "dear"       
    ## [216] "debate"      "decide"      "decision"    "deep"        "definite"   
    ## [221] "degree"      "department"  "depend"      "describe"    "design"     
    ## [226] "detail"      "develop"     "die"         "difference"  "difficult"  
    ## [231] "dinner"      "direct"      "discuss"     "district"    "divide"     
    ## [236] "do"          "doctor"      "document"    "dog"         "door"       
    ## [241] "double"      "doubt"       "down"        "draw"        "dress"      
    ## [246] "drink"       "drive"       "drop"        "dry"         "due"        
    ## [251] "during"      "each"        "early"       "east"        "easy"       
    ## [256] "eat"         "economy"     "educate"     "effect"      "egg"        
    ## [261] "eight"       "either"      "elect"       "electric"    "eleven"     
    ## [266] "else"        "employ"      "encourage"   "end"         "engine"     
    ## [271] "english"     "enjoy"       "enough"      "enter"       "environment"
    ## [276] "equal"       "especial"    "europe"      "even"        "evening"    
    ## [281] "ever"        "every"       "evidence"    "exact"       "example"    
    ## [286] "except"      "excuse"      "exercise"    "exist"       "expect"     
    ## [291] "expense"     "experience"  "explain"     "express"     "extra"      
    ## [296] "eye"         "face"        "fact"        "fair"        "fall"       
    ## [301] "family"      "far"         "farm"        "fast"        "father"     
    ## [306] "favour"      "feed"        "feel"        "few"         "field"      
    ## [311] "fight"       "figure"      "file"        "fill"        "film"       
    ## [316] "final"       "finance"     "find"        "fine"        "finish"     
    ## [321] "fire"        "first"       "fish"        "fit"         "five"       
    ## [326] "flat"        "floor"       "fly"         "follow"      "food"       
    ## [331] "foot"        "for"         "force"       "forget"      "form"       
    ## [336] "fortune"     "forward"     "four"        "france"      "free"       
    ## [341] "friday"      "friend"      "from"        "front"       "full"       
    ## [346] "fun"         "function"    "fund"        "further"     "future"     
    ## [351] "game"        "garden"      "gas"         "general"     "germany"    
    ## [356] "get"         "girl"        "give"        "glass"       "go"         
    ## [361] "god"         "good"        "goodbye"     "govern"      "grand"      
    ## [366] "grant"       "great"       "green"       "ground"      "group"      
    ## [371] "grow"        "guess"       "guy"         "hair"        "half"       
    ## [376] "hall"        "hand"        "hang"        "happen"      "happy"      
    ## [381] "hard"        "hate"        "have"        "he"          "head"       
    ## [386] "health"      "hear"        "heart"       "heat"        "heavy"      
    ## [391] "hell"        "help"        "here"        "high"        "history"    
    ## [396] "hit"         "hold"        "holiday"     "home"        "honest"     
    ## [401] "hope"        "horse"       "hospital"    "hot"         "hour"       
    ## [406] "house"       "how"         "however"     "hullo"       "hundred"    
    ## [411] "husband"     "idea"        "identify"    "if"          "imagine"    
    ## [416] "important"   "improve"     "in"          "include"     "income"     
    ## [421] "increase"    "indeed"      "individual"  "industry"    "inform"     
    ## [426] "inside"      "instead"     "insure"      "interest"    "into"       
    ## [431] "introduce"   "invest"      "involve"     "issue"       "it"         
    ## [436] "item"        "jesus"       "job"         "join"        "judge"      
    ## [441] "jump"        "just"        "keep"        "key"         "kid"        
    ## [446] "kill"        "kind"        "king"        "kitchen"     "knock"      
    ## [451] "know"        "labour"      "lad"         "lady"        "land"       
    ## [456] "language"    "large"       "last"        "late"        "laugh"      
    ## [461] "law"         "lay"         "lead"        "learn"       "leave"      
    ## [466] "left"        "leg"         "less"        "let"         "letter"     
    ## [471] "level"       "lie"         "life"        "light"       "like"       
    ## [476] "likely"      "limit"       "line"        "link"        "list"       
    ## [481] "listen"      "little"      "live"        "load"        "local"      
    ## [486] "lock"        "london"      "long"        "look"        "lord"       
    ## [491] "lose"        "lot"         "love"        "low"         "luck"       
    ## [496] "lunch"       "machine"     "main"        "major"       "make"       
    ## [501] "man"         "manage"      "many"        "mark"        "market"     
    ## [506] "marry"       "match"       "matter"      "may"         "maybe"      
    ## [511] "mean"        "meaning"     "measure"     "meet"        "member"     
    ## [516] "mention"     "middle"      "might"       "mile"        "milk"       
    ## [521] "million"     "mind"        "minister"    "minus"       "minute"     
    ## [526] "miss"        "mister"      "moment"      "monday"      "money"      
    ## [531] "month"       "more"        "morning"     "most"        "mother"     
    ## [536] "motion"      "move"        "mrs"         "much"        "music"      
    ## [541] "must"        "name"        "nation"      "nature"      "near"       
    ## [546] "necessary"   "need"        "never"       "new"         "news"       
    ## [551] "next"        "nice"        "night"       "nine"        "no"         
    ## [556] "non"         "none"        "normal"      "north"       "not"        
    ## [561] "note"        "notice"      "now"         "number"      "obvious"    
    ## [566] "occasion"    "odd"         "of"          "off"         "offer"      
    ## [571] "office"      "often"       "okay"        "old"         "on"         
    ## [576] "once"        "one"         "only"        "open"        "operate"    
    ## [581] "opportunity" "oppose"      "or"          "order"       "organize"   
    ## [586] "original"    "other"       "otherwise"   "ought"       "out"        
    ## [591] "over"        "own"         "pack"        "page"        "paint"      
    ## [596] "pair"        "paper"       "paragraph"   "pardon"      "parent"     
    ## [601] "park"        "part"        "particular"  "party"       "pass"       
    ## [606] "past"        "pay"         "pence"       "pension"     "people"     
    ## [611] "per"         "percent"     "perfect"     "perhaps"     "period"     
    ## [616] "person"      "photograph"  "pick"        "picture"     "piece"      
    ## [621] "place"       "plan"        "play"        "please"      "plus"       
    ## [626] "point"       "police"      "policy"      "politic"     "poor"       
    ## [631] "position"    "positive"    "possible"    "post"        "pound"      
    ## [636] "power"       "practise"    "prepare"     "present"     "press"      
    ## [641] "pressure"    "presume"     "pretty"      "previous"    "price"      
    ## [646] "print"       "private"     "probable"    "problem"     "proceed"    
    ## [651] "process"     "produce"     "product"     "programme"   "project"    
    ## [656] "proper"      "propose"     "protect"     "provide"     "public"     
    ## [661] "pull"        "purpose"     "push"        "put"         "quality"    
    ## [666] "quarter"     "question"    "quick"       "quid"        "quiet"      
    ## [671] "quite"       "radio"       "rail"        "raise"       "range"      
    ## [676] "rate"        "rather"      "read"        "ready"       "real"       
    ## [681] "realise"     "really"      "reason"      "receive"     "recent"     
    ## [686] "reckon"      "recognize"   "recommend"   "record"      "red"        
    ## [691] "reduce"      "refer"       "regard"      "region"      "relation"   
    ## [696] "remember"    "report"      "represent"   "require"     "research"   
    ## [701] "resource"    "respect"     "responsible" "rest"        "result"     
    ## [706] "return"      "rid"         "right"       "ring"        "rise"       
    ## [711] "road"        "role"        "roll"        "room"        "round"      
    ## [716] "rule"        "run"         "safe"        "sale"        "same"       
    ## [721] "saturday"    "save"        "say"         "scheme"      "school"     
    ## [726] "science"     "score"       "scotland"    "seat"        "second"     
    ## [731] "secretary"   "section"     "secure"      "see"         "seem"       
    ## [736] "self"        "sell"        "send"        "sense"       "separate"   
    ## [741] "serious"     "serve"       "service"     "set"         "settle"     
    ## [746] "seven"       "sex"         "shall"       "share"       "she"        
    ## [751] "sheet"       "shoe"        "shoot"       "shop"        "short"      
    ## [756] "should"      "show"        "shut"        "sick"        "side"       
    ## [761] "sign"        "similar"     "simple"      "since"       "sing"       
    ## [766] "single"      "sir"         "sister"      "sit"         "site"       
    ## [771] "situate"     "six"         "size"        "sleep"       "slight"     
    ## [776] "slow"        "small"       "smoke"       "so"          "social"     
    ## [781] "society"     "some"        "son"         "soon"        "sorry"      
    ## [786] "sort"        "sound"       "south"       "space"       "speak"      
    ## [791] "special"     "specific"    "speed"       "spell"       "spend"      
    ## [796] "square"      "staff"       "stage"       "stairs"      "stand"      
    ## [801] "standard"    "start"       "state"       "station"     "stay"       
    ## [806] "step"        "stick"       "still"       "stop"        "story"      
    ## [811] "straight"    "strategy"    "street"      "strike"      "strong"     
    ## [816] "structure"   "student"     "study"       "stuff"       "stupid"     
    ## [821] "subject"     "succeed"     "such"        "sudden"      "suggest"    
    ## [826] "suit"        "summer"      "sun"         "sunday"      "supply"     
    ## [831] "support"     "suppose"     "sure"        "surprise"    "switch"     
    ## [836] "system"      "table"       "take"        "talk"        "tape"       
    ## [841] "tax"         "tea"         "teach"       "team"        "telephone"  
    ## [846] "television"  "tell"        "ten"         "tend"        "term"       
    ## [851] "terrible"    "test"        "than"        "thank"       "the"        
    ## [856] "then"        "there"       "therefore"   "they"        "thing"      
    ## [861] "think"       "thirteen"    "thirty"      "this"        "thou"       
    ## [866] "though"      "thousand"    "three"       "through"     "throw"      
    ## [871] "thursday"    "tie"         "time"        "to"          "today"      
    ## [876] "together"    "tomorrow"    "tonight"     "too"         "top"        
    ## [881] "total"       "touch"       "toward"      "town"        "trade"      
    ## [886] "traffic"     "train"       "transport"   "travel"      "treat"      
    ## [891] "tree"        "trouble"     "true"        "trust"       "try"        
    ## [896] "tuesday"     "turn"        "twelve"      "twenty"      "two"        
    ## [901] "type"        "under"       "understand"  "union"       "unit"       
    ## [906] "unite"       "university"  "unless"      "until"       "up"         
    ## [911] "upon"        "use"         "usual"       "value"       "various"    
    ## [916] "very"        "video"       "view"        "village"     "visit"      
    ## [921] "vote"        "wage"        "wait"        "walk"        "wall"       
    ## [926] "want"        "war"         "warm"        "wash"        "waste"      
    ## [931] "watch"       "water"       "way"         "we"          "wear"       
    ## [936] "wednesday"   "wee"         "week"        "weigh"       "welcome"    
    ## [941] "well"        "west"        "what"        "when"        "where"      
    ## [946] "whether"     "which"       "while"       "white"       "who"        
    ## [951] "whole"       "why"         "wide"        "wife"        "will"       
    ## [956] "win"         "wind"        "window"      "wish"        "with"       
    ## [961] "within"      "without"     "woman"       "wonder"      "wood"       
    ## [966] "word"        "work"        "world"       "worry"       "worse"      
    ## [971] "worth"       "would"       "write"       "wrong"       "year"       
    ## [976] "yes"         "yesterday"   "yet"         "you"         "young"

Here is a sample of how the words contained in this data set would be
translated into **Go Loco**.

``` r
# rules applied to words that begin with a consonant/vowel pair
words_trans <-
  tibble("English" = words,
         "Go Loco" = case_when(str_starts(words, "[^aioeu](?=[aiouey])") ~  
                      str_c(str_sub(words, start = 2, end = -1), 
                            str_sub(words, start = 1, end = 2), 
                            str_sub(words, start = 1, end = 2), 
                            "loco"),
# rules applied to words that begin with a consonant cluster/vowel sound
                    str_starts(words, "[^aioeu](?![aiouey])") ~ 
                      str_c(str_extract(words, "([aioeuy]).*"), 
                            str_extract(words, "^[^aioue]+[aiouey]+"),
                            str_extract(words, "^[^aioue]+[aiouey]+"),
                            "loco"),
# rules applied to words that begin with a vowel
                    str_starts(words, "[aioeu]") ~
                      str_c(str_extract(words, "(?<=[^aioue]).*"), 
                            str_extract(words, "(^[aioue]+[^aioue])"),
                            str_extract(words, "(^[aioue]+[^aioue])"),
                            str_extract(words, "(^[aioue])"),
                            "loco")))

words_trans
```

    ## # A tibble: 980 × 2
    ##    English  `Go Loco`      
    ##    <chr>    <chr>          
    ##  1 a        <NA>           
    ##  2 able     leababaloco    
    ##  3 about    outababaloco   
    ##  4 absolute soluteababaloco
    ##  5 accept   ceptacacaloco  
    ##  6 account  countacacaloco 
    ##  7 achieve  hieveacacaloco 
    ##  8 across   rossacacaloco  
    ##  9 act      tacacaloco     
    ## 10 active   tiveacacaloco  
    ## # ℹ 970 more rows

Note that the word must be at least 2 letters long to be translated into
**Go Loco** e.g. “a” does not have a **Go Loco** translation.

## 3.0 Function `goloco`

The following code creates a function called `goloco` that takes a
string or character vector input for `words` and outputs a character
vector corresponding to the **Go Loco** translation of the input.

``` r
#' @title Translate word(s) into Go Loco "Pig Latin" derivative language
#'
#' @param words A string or character vector to translate into Go Loco
#'
#' @return A character vector with the translated input
#' @import stringr
#' @import dplyr
#' @export
#'
#' @examples
#' goloco(banana)
#' goloco(c("pineapple", "melon", "apple"))
#' goloco(str_split_1("the quick fox jumped over the lazy moon", " "))
#' 
goloco <- function(words) {
  if(is.numeric(words)) {
        stop("Sorry! You can't make numbers go loco :(")
    }
# rules applied to words that begin with a consonant/vowel pair
  case_when(str_starts(words, "[^aioeu](?=[aiouey])") ~  
                      str_c(str_sub(words, start = 2, end = -1), 
                            str_sub(words, start = 1, end = 2), 
                            str_sub(words, start = 1, end = 2), 
                            "loco"),
# rules applied to words that begin with a consonant cluster/vowel sound
                    str_starts(words, "[^aioeu](?![aiouey])") ~ 
                      str_c(str_extract(words, "([aioeuy]).*"), 
                            str_extract(words, "^[^aioue]+[aiouey]+"),
                            str_extract(words, "^[^aioue]+[aiouey]+"),
                            "loco"),
# rules applied to words that begin with a vowel
                    str_starts(words, "[aioeu]") ~
                      str_c(str_extract(words, "(?<=[^aioue]).*"), 
                            str_extract(words, "(^[aioue]+[^aioue])"),
                            str_extract(words, "(^[aioue]+[^aioue])"),
                            str_extract(words, "(^[aioue])"),
                            "loco"))
}
```

## 4.0 Examples of `goloco`

### 4.1 Translating single words using `goloco`

Here is an example of applying `goloco` to translate single words.

``` r
goloco("banana")
```

    ## [1] "ananababaloco"

``` r
goloco("apple")
```

    ## [1] "pleapapaloco"

``` r
goloco("pineapple")
```

    ## [1] "ineapplepipiloco"

### 4.2 Translating a sentence using `goloco`

Here is a example of applying `goloco` to translate a sentence.

``` r
# requires splitting the sentence into its component words, using the separator " " 
goloco(str_split_1("we all like to eat apples and bananas", " "))
```

    ## [1] "eweweloco"      "lalalaloco"     "ikelililoco"    "ototoloco"     
    ## [5] "eateateloco"    "plesapapaloco"  "dananaloco"     "ananasbabaloco"

### 4.3 Translating `words` data set in `stringr` package using `goloco`

Recall the `words` data set found in the `stringr` package:

``` r
head(words, n = 100)
```

    ##   [1] "a"           "able"        "about"       "absolute"    "accept"     
    ##   [6] "account"     "achieve"     "across"      "act"         "active"     
    ##  [11] "actual"      "add"         "address"     "admit"       "advertise"  
    ##  [16] "affect"      "afford"      "after"       "afternoon"   "again"      
    ##  [21] "against"     "age"         "agent"       "ago"         "agree"      
    ##  [26] "air"         "all"         "allow"       "almost"      "along"      
    ##  [31] "already"     "alright"     "also"        "although"    "always"     
    ##  [36] "america"     "amount"      "and"         "another"     "answer"     
    ##  [41] "any"         "apart"       "apparent"    "appear"      "apply"      
    ##  [46] "appoint"     "approach"    "appropriate" "area"        "argue"      
    ##  [51] "arm"         "around"      "arrange"     "art"         "as"         
    ##  [56] "ask"         "associate"   "assume"      "at"          "attend"     
    ##  [61] "authority"   "available"   "aware"       "away"        "awful"      
    ##  [66] "baby"        "back"        "bad"         "bag"         "balance"    
    ##  [71] "ball"        "bank"        "bar"         "base"        "basis"      
    ##  [76] "be"          "bear"        "beat"        "beauty"      "because"    
    ##  [81] "become"      "bed"         "before"      "begin"       "behind"     
    ##  [86] "believe"     "benefit"     "best"        "bet"         "between"    
    ##  [91] "big"         "bill"        "birth"       "bit"         "black"      
    ##  [96] "bloke"       "blood"       "blow"        "blue"        "board"

Here is an example of applying `goloco` to translate the character
vector `words`.

``` r
goloco(stringr::words)
```

    ##   [1] NA                    "leababaloco"         "outababaloco"       
    ##   [4] "soluteababaloco"     "ceptacacaloco"       "countacacaloco"     
    ##   [7] "hieveacacaloco"      "rossacacaloco"       "tacacaloco"         
    ##  [10] "tiveacacaloco"       "tualacacaloco"       "dadadaloco"         
    ##  [13] "dressadadaloco"      "mitadadaloco"        "vertiseadadaloco"   
    ##  [16] "fectafafaloco"       "fordafafaloco"       "terafafaloco"       
    ##  [19] "ternoonafafaloco"    "ainagagaloco"        "ainstagagaloco"     
    ##  [22] "eagagaloco"          "entagagaloco"        "oagagaloco"         
    ##  [25] "reeagagaloco"        "airairaloco"         "lalalaloco"         
    ##  [28] "lowalalaloco"        "mostalalaloco"       "ongalalaloco"       
    ##  [31] "readyalalaloco"      "rightalalaloco"      "soalalaloco"        
    ##  [34] "thoughalalaloco"     "waysalalaloco"       "ericaamamaloco"     
    ##  [37] "ountamamaloco"       "dananaloco"          "otherananaloco"     
    ##  [40] "swerananaloco"       "yananaloco"          "artapapaloco"       
    ##  [43] "parentapapaloco"     "pearapapaloco"       "plyapapaloco"       
    ##  [46] "pointapapaloco"      "proachapapaloco"     "propriateapapaloco" 
    ##  [49] "eaararaloco"         "gueararaloco"        "mararaloco"         
    ##  [52] "oundararaloco"       "rangeararaloco"      "tararaloco"         
    ##  [55] "asasaloco"           "kasasaloco"          "sociateasasaloco"   
    ##  [58] "sumeasasaloco"       "atataloco"           "tendatataloco"      
    ##  [61] "horityautautaloco"   "ailableavavaloco"    "areawawaloco"       
    ##  [64] "ayawawaloco"         "fulawawaloco"        "abybabaloco"        
    ##  [67] "ackbabaloco"         "adbabaloco"          "agbabaloco"         
    ##  [70] "alancebabaloco"      "allbabaloco"         "ankbabaloco"        
    ##  [73] "arbabaloco"          "asebabaloco"         "asisbabaloco"       
    ##  [76] "ebebeloco"           "earbebeloco"         "eatbebeloco"        
    ##  [79] "eautybebeloco"       "ecausebebeloco"      "ecomebebeloco"      
    ##  [82] "edbebeloco"          "eforebebeloco"       "eginbebeloco"       
    ##  [85] "ehindbebeloco"       "elievebebeloco"      "enefitbebeloco"     
    ##  [88] "estbebeloco"         "etbebeloco"          "etweenbebeloco"     
    ##  [91] "igbibiloco"          "illbibiloco"         "irthbibiloco"       
    ##  [94] "itbibiloco"          "ackblablaloco"       "okeblobloloco"      
    ##  [97] "oodblooblooloco"     "owblobloloco"        "ueblueblueloco"     
    ## [100] "oardboboloco"        "oatboboloco"         "odyboboloco"        
    ## [103] "ookboboloco"         "othboboloco"         "otherboboloco"      
    ## [106] "ottleboboloco"       "ottomboboloco"       "oxboboloco"         
    ## [109] "oyboboloco"          "eakbreabrealoco"     "iefbriebrieloco"    
    ## [112] "illiantbribriloco"   "ingbribriloco"       "itainbribriloco"    
    ## [115] "otherbrobroloco"     "udgetbubuloco"       "uildbubuloco"       
    ## [118] "usbubuloco"          "usinessbubuloco"     "usybubuloco"        
    ## [121] "utbubuloco"          "uybubuloco"          "ybybyloco"          
    ## [124] "akecacaloco"         "allcacaloco"         "ancacaloco"         
    ## [127] "arcacaloco"          "ardcacaloco"         "arecacaloco"        
    ## [130] "arrycacaloco"        "asecacaloco"         "atcacaloco"         
    ## [133] "atchcacaloco"        "ausecacaloco"        "entceceloco"        
    ## [136] "entrececeloco"       "ertainceceloco"      "airchaichailoco"    
    ## [139] "airmanchaichailoco"  "ancechachaloco"      "angechachaloco"     
    ## [142] "apchachaloco"        "aracterchachaloco"   "argechachaloco"     
    ## [145] "eapcheachealoco"     "eckchecheloco"       "ildchichiloco"      
    ## [148] "oicechoichoiloco"    "oosechoochooloco"    "istChriChriloco"    
    ## [151] "istmasChriChriloco"  "urchchuchuloco"      "ityciciloco"        
    ## [154] "aimclaiclailoco"     "assclaclaloco"       "eancleaclealoco"    
    ## [157] "earcleaclealoco"     "ientclieclieloco"    "ockclocloloco"      
    ## [160] "oseclocloloco"       "osesclocloloco"      "otheclocloloco"     
    ## [163] "ubclucluloco"        "offeecocoloco"       "oldcocoloco"        
    ## [166] "olleaguecocoloco"    "ollectcocoloco"      "ollegecocoloco"     
    ## [169] "olourcocoloco"       "omecocoloco"         "ommentcocoloco"     
    ## [172] "ommitcocoloco"       "ommitteecocoloco"    "ommoncocoloco"      
    ## [175] "ommunitycocoloco"    "ompanycocoloco"      "omparecocoloco"     
    ## [178] "ompletecocoloco"     "omputecocoloco"      "oncerncocoloco"     
    ## [181] "onditioncocoloco"    "onfercocoloco"       "onsidercocoloco"    
    ## [184] "onsultcocoloco"      "ontactcocoloco"      "ontinuecocoloco"    
    ## [187] "ontractcocoloco"     "ontrolcocoloco"      "onversecocoloco"    
    ## [190] "ookcocoloco"         "opycocoloco"         "ornercocoloco"      
    ## [193] "orrectcocoloco"      "ostcocoloco"         "ouldcocoloco"       
    ## [196] "ouncilcocoloco"      "ountcocoloco"        "ountrycocoloco"     
    ## [199] "ountycocoloco"       "ouplecocoloco"       "oursecocoloco"      
    ## [202] "ourtcocoloco"        "overcocoloco"        "eatecreacrealoco"   
    ## [205] "osscrocroloco"       "upcuculoco"          "urrentcuculoco"     
    ## [208] "utcuculoco"          "addadaloco"          "angerdadaloco"      
    ## [211] "atedadaloco"         "aydadaloco"          "eaddedeloco"        
    ## [214] "ealdedeloco"         "eardedeloco"         "ebatededeloco"      
    ## [217] "ecidededeloco"       "ecisiondedeloco"     "eepdedeloco"        
    ## [220] "efinitededeloco"     "egreededeloco"       "epartmentdedeloco"  
    ## [223] "ependdedeloco"       "escribededeloco"     "esigndedeloco"      
    ## [226] "etaildedeloco"       "evelopdedeloco"      "iedidiloco"         
    ## [229] "ifferencedidiloco"   "ifficultdidiloco"    "innerdidiloco"      
    ## [232] "irectdidiloco"       "iscussdidiloco"      "istrictdidiloco"    
    ## [235] "ividedidiloco"       "ododoloco"           "octordodoloco"      
    ## [238] "ocumentdodoloco"     "ogdodoloco"          "oordodoloco"        
    ## [241] "oubledodoloco"       "oubtdodoloco"        "owndodoloco"        
    ## [244] "awdradraloco"        "essdredreloco"       "inkdridriloco"      
    ## [247] "ivedridriloco"       "opdrodroloco"        "ydrydryloco"        
    ## [250] "ueduduloco"          "uringduduloco"       "heaceaceloco"       
    ## [253] "lyeareareloco"       "teaseaseloco"        "yeaseaseloco"       
    ## [256] "eateateloco"         "onomyececeloco"      "ucateededeloco"     
    ## [259] "fectefefeloco"       "gegegeloco"          "hteigeigeloco"      
    ## [262] "hereiteiteloco"      "ecteleleloco"        "ectriceleleloco"    
    ## [265] "eveneleleloco"       "seeleleloco"         "ployememeloco"      
    ## [268] "courageeneneloco"    "deneneloco"          "gineeneneloco"      
    ## [271] "glisheneneloco"      "joyeneneloco"        "ougheneneloco"      
    ## [274] "tereneneloco"        "vironmenteneneloco"  "ualeqeqeloco"       
    ## [277] "pecialeseseloco"     "opeeureureloco"      "eneveveloco"        
    ## [280] "eningeveveloco"      "ereveveloco"         "eryeveveloco"       
    ## [283] "idenceeveveloco"     "actexexeloco"        "ampleexexeloco"     
    ## [286] "ceptexexeloco"       "cuseexexeloco"       "erciseexexeloco"    
    ## [289] "istexexeloco"        "pectexexeloco"       "penseexexeloco"     
    ## [292] "perienceexexeloco"   "plainexexeloco"      "pressexexeloco"     
    ## [295] "traexexeloco"        "eeyeyeloco"          "acefafaloco"        
    ## [298] "actfafaloco"         "airfafaloco"         "allfafaloco"        
    ## [301] "amilyfafaloco"       "arfafaloco"          "armfafaloco"        
    ## [304] "astfafaloco"         "atherfafaloco"       "avourfafaloco"      
    ## [307] "eedfefeloco"         "eelfefeloco"         "ewfefeloco"         
    ## [310] "ieldfifiloco"        "ightfifiloco"        "igurefifiloco"      
    ## [313] "ilefifiloco"         "illfifiloco"         "ilmfifiloco"        
    ## [316] "inalfifiloco"        "inancefifiloco"      "indfifiloco"        
    ## [319] "inefifiloco"         "inishfifiloco"       "irefifiloco"        
    ## [322] "irstfifiloco"        "ishfifiloco"         "itfifiloco"         
    ## [325] "ivefifiloco"         "atflaflaloco"        "oorflooflooloco"    
    ## [328] "yflyflyloco"         "ollowfofoloco"       "oodfofoloco"        
    ## [331] "ootfofoloco"         "orfofoloco"          "orcefofoloco"       
    ## [334] "orgetfofoloco"       "ormfofoloco"         "ortunefofoloco"     
    ## [337] "orwardfofoloco"      "ourfofoloco"         "ancefrafraloco"     
    ## [340] "eefreefreeloco"      "idayfrifriloco"      "iendfriefrieloco"   
    ## [343] "omfrofroloco"        "ontfrofroloco"       "ullfufuloco"        
    ## [346] "unfufuloco"          "unctionfufuloco"     "undfufuloco"        
    ## [349] "urtherfufuloco"      "uturefufuloco"       "amegagaloco"        
    ## [352] "ardengagaloco"       "asgagaloco"          "eneralgegeloco"     
    ## [355] "ermanygegeloco"      "etgegeloco"          "irlgigiloco"        
    ## [358] "ivegigiloco"         "assglaglaloco"       "ogogoloco"          
    ## [361] "odgogoloco"          "oodgogoloco"         "oodbyegogoloco"     
    ## [364] "overngogoloco"       "andgragraloco"       "antgragraloco"      
    ## [367] "eatgreagrealoco"     "eengreegreeloco"     "oundgrougrouloco"   
    ## [370] "oupgrougrouloco"     "owgrogroloco"        "uessguguloco"       
    ## [373] "uyguguloco"          "airhahaloco"         "alfhahaloco"        
    ## [376] "allhahaloco"         "andhahaloco"         "anghahaloco"        
    ## [379] "appenhahaloco"       "appyhahaloco"        "ardhahaloco"        
    ## [382] "atehahaloco"         "avehahaloco"         "eheheloco"          
    ## [385] "eadheheloco"         "ealthheheloco"       "earheheloco"        
    ## [388] "eartheheloco"        "eatheheloco"         "eavyheheloco"       
    ## [391] "ellheheloco"         "elpheheloco"         "ereheheloco"        
    ## [394] "ighhihiloco"         "istoryhihiloco"      "ithihiloco"         
    ## [397] "oldhoholoco"         "olidayhoholoco"      "omehoholoco"        
    ## [400] "onesthoholoco"       "opehoholoco"         "orsehoholoco"       
    ## [403] "ospitalhoholoco"     "othoholoco"          "ourhoholoco"        
    ## [406] "ousehoholoco"        "owhoholoco"          "oweverhoholoco"     
    ## [409] "ullohuhuloco"        "undredhuhuloco"      "usbandhuhuloco"     
    ## [412] "eaididiloco"         "entifyididiloco"     "ififiloco"          
    ## [415] "agineimimiloco"      "portantimimiloco"    "proveimimiloco"     
    ## [418] "ininiloco"           "cludeininiloco"      "comeininiloco"      
    ## [421] "creaseininiloco"     "deedininiloco"       "dividualininiloco"  
    ## [424] "dustryininiloco"     "formininiloco"       "sideininiloco"      
    ## [427] "steadininiloco"      "sureininiloco"       "terestininiloco"    
    ## [430] "toininiloco"         "troduceininiloco"    "vestininiloco"      
    ## [433] "volveininiloco"      "sueisisiloco"        "ititiloco"          
    ## [436] "emititiloco"         "esusjejeloco"        "objojoloco"         
    ## [439] "oinjojoloco"         "udgejujuloco"        "umpjujuloco"        
    ## [442] "ustjujuloco"         "eepkekeloco"         "eykekeloco"         
    ## [445] "idkikiloco"          "illkikiloco"         "indkikiloco"        
    ## [448] "ingkikiloco"         "itchenkikiloco"      "ockknoknoloco"      
    ## [451] "owknoknoloco"        "abourlalaloco"       "adlalaloco"         
    ## [454] "adylalaloco"         "andlalaloco"         "anguagelalaloco"    
    ## [457] "argelalaloco"        "astlalaloco"         "atelalaloco"        
    ## [460] "aughlalaloco"        "awlalaloco"          "aylalaloco"         
    ## [463] "eadleleloco"         "earnleleloco"        "eaveleleloco"       
    ## [466] "eftleleloco"         "egleleloco"          "essleleloco"        
    ## [469] "etleleloco"          "etterleleloco"       "evelleleloco"       
    ## [472] "ielililoco"          "ifelililoco"         "ightlililoco"       
    ## [475] "ikelililoco"         "ikelylililoco"       "imitlililoco"       
    ## [478] "inelililoco"         "inklililoco"         "istlililoco"        
    ## [481] "istenlililoco"       "ittlelililoco"       "ivelililoco"        
    ## [484] "oadlololoco"         "ocallololoco"        "ocklololoco"        
    ## [487] "ondonlololoco"       "onglololoco"         "ooklololoco"        
    ## [490] "ordlololoco"         "oselololoco"         "otlololoco"         
    ## [493] "ovelololoco"         "owlololoco"          "ucklululoco"        
    ## [496] "unchlululoco"        "achinemamaloco"      "ainmamaloco"        
    ## [499] "ajormamaloco"        "akemamaloco"         "anmamaloco"         
    ## [502] "anagemamaloco"       "anymamaloco"         "arkmamaloco"        
    ## [505] "arketmamaloco"       "arrymamaloco"        "atchmamaloco"       
    ## [508] "attermamaloco"       "aymamaloco"          "aybemamaloco"       
    ## [511] "eanmemeloco"         "eaningmemeloco"      "easurememeloco"     
    ## [514] "eetmemeloco"         "embermemeloco"       "entionmemeloco"     
    ## [517] "iddlemimiloco"       "ightmimiloco"        "ilemimiloco"        
    ## [520] "ilkmimiloco"         "illionmimiloco"      "indmimiloco"        
    ## [523] "inistermimiloco"     "inusmimiloco"        "inutemimiloco"      
    ## [526] "issmimiloco"         "istermimiloco"       "omentmomoloco"      
    ## [529] "ondaymomoloco"       "oneymomoloco"        "onthmomoloco"       
    ## [532] "oremomoloco"         "orningmomoloco"      "ostmomoloco"        
    ## [535] "othermomoloco"       "otionmomoloco"       "ovemomoloco"        
    ## [538] NA                    "uchmumuloco"         "usicmumuloco"       
    ## [541] "ustmumuloco"         "amenanaloco"         "ationnanaloco"      
    ## [544] "aturenanaloco"       "earneneloco"         "ecessaryneneloco"   
    ## [547] "eedneneloco"         "everneneloco"        "ewneneloco"         
    ## [550] "ewsneneloco"         "extneneloco"         "iceniniloco"        
    ## [553] "ightniniloco"        "ineniniloco"         "ononoloco"          
    ## [556] "onnonoloco"          "onenonoloco"         "ormalnonoloco"      
    ## [559] "orthnonoloco"        "otnonoloco"          "otenonoloco"        
    ## [562] "oticenonoloco"       "ownonoloco"          "umbernunuloco"      
    ## [565] "viousoboboloco"      "casionococoloco"     "dododoloco"         
    ## [568] "ofofoloco"           "fofofoloco"          "ferofofoloco"       
    ## [571] "ficeofofoloco"       "tenofofoloco"        "ayokokoloco"        
    ## [574] "dolololoco"          "ononoloco"           "ceononoloco"        
    ## [577] "eononoloco"          "lyononoloco"         "enopopoloco"        
    ## [580] "erateopopoloco"      "portunityopopoloco"  "poseopopoloco"      
    ## [583] "ororoloco"           "derororoloco"        "ganizeororoloco"    
    ## [586] "iginalororoloco"     "herototoloco"        "herwiseototoloco"   
    ## [589] "htougougoloco"       "outoutoloco"         "erovovoloco"        
    ## [592] "nowowoloco"          "ackpapaloco"         "agepapaloco"        
    ## [595] "aintpapaloco"        "airpapaloco"         "aperpapaloco"       
    ## [598] "aragraphpapaloco"    "ardonpapaloco"       "arentpapaloco"      
    ## [601] "arkpapaloco"         "artpapaloco"         "articularpapaloco"  
    ## [604] "artypapaloco"        "asspapaloco"         "astpapaloco"        
    ## [607] "aypapaloco"          "encepepeloco"        "ensionpepeloco"     
    ## [610] "eoplepepeloco"       "erpepeloco"          "ercentpepeloco"     
    ## [613] "erfectpepeloco"      "erhapspepeloco"      "eriodpepeloco"      
    ## [616] "ersonpepeloco"       "otographphopholoco"  "ickpipiloco"        
    ## [619] "icturepipiloco"      "iecepipiloco"        "aceplaplaloco"      
    ## [622] "anplaplaloco"        "ayplayplayloco"      "easepleaplealoco"   
    ## [625] "usplupluloco"        "ointpopoloco"        "olicepopoloco"      
    ## [628] "olicypopoloco"       "oliticpopoloco"      "oorpopoloco"        
    ## [631] "ositionpopoloco"     "ositivepopoloco"     "ossiblepopoloco"    
    ## [634] "ostpopoloco"         "oundpopoloco"        "owerpopoloco"       
    ## [637] "actiseprapraloco"    "epareprepreloco"     "esentprepreloco"    
    ## [640] "essprepreloco"       "essureprepreloco"    "esumeprepreloco"    
    ## [643] "ettyprepreloco"      "eviousprepreloco"    "icepripriloco"      
    ## [646] "intpripriloco"       "ivatepripriloco"     "obableproproloco"   
    ## [649] "oblemproproloco"     "oceedproproloco"     "ocessproproloco"    
    ## [652] "oduceproproloco"     "oductproproloco"     "ogrammeproproloco"  
    ## [655] "ojectproproloco"     "operproproloco"      "oposeproproloco"    
    ## [658] "otectproproloco"     "ovideproproloco"     "ublicpupuloco"      
    ## [661] "ullpupuloco"         "urposepupuloco"      "ushpupuloco"        
    ## [664] "utpupuloco"          "ualityququloco"      "uarterququloco"     
    ## [667] "uestionququloco"     "uickququloco"        "uidququloco"        
    ## [670] "uietququloco"        "uiteququloco"        "adioraraloco"       
    ## [673] "ailraraloco"         "aiseraraloco"        "angeraraloco"       
    ## [676] "ateraraloco"         "atherraraloco"       "eadrereloco"        
    ## [679] "eadyrereloco"        "ealrereloco"         "ealiserereloco"     
    ## [682] "eallyrereloco"       "easonrereloco"       "eceiverereloco"     
    ## [685] "ecentrereloco"       "eckonrereloco"       "ecognizerereloco"   
    ## [688] "ecommendrereloco"    "ecordrereloco"       "edrereloco"         
    ## [691] "educerereloco"       "eferrereloco"        "egardrereloco"      
    ## [694] "egionrereloco"       "elationrereloco"     "ememberrereloco"    
    ## [697] "eportrereloco"       "epresentrereloco"    "equirerereloco"     
    ## [700] "esearchrereloco"     "esourcerereloco"     "espectrereloco"     
    ## [703] "esponsiblerereloco"  "estrereloco"         "esultrereloco"      
    ## [706] "eturnrereloco"       "idririloco"          "ightririloco"       
    ## [709] "ingririloco"         "iseririloco"         "oadroroloco"        
    ## [712] "oleroroloco"         "ollroroloco"         "oomroroloco"        
    ## [715] "oundroroloco"        "uleruruloco"         "unruruloco"         
    ## [718] "afesasaloco"         "alesasaloco"         "amesasaloco"        
    ## [721] "aturdaysasaloco"     "avesasaloco"         "aysasaloco"         
    ## [724] "emeschescheloco"     "oolschooschooloco"   "iencesciescieloco"  
    ## [727] "orescoscoloco"       "otlandscoscoloco"    "eatseseloco"        
    ## [730] "econdseseloco"       "ecretaryseseloco"    "ectionseseloco"     
    ## [733] "ecureseseloco"       "eeseseloco"          "eemseseloco"        
    ## [736] "elfseseloco"         "ellseseloco"         "endseseloco"        
    ## [739] "enseseseloco"        "eparateseseloco"     "eriousseseloco"     
    ## [742] "erveseseloco"        "erviceseseloco"      "etseseloco"         
    ## [745] "ettleseseloco"       "evenseseloco"        "exseseloco"         
    ## [748] "allshashaloco"       "areshashaloco"       "eshesheloco"        
    ## [751] "eetsheesheeloco"     "oeshoeshoeloco"      "ootshooshooloco"    
    ## [754] "opshosholoco"        "ortshosholoco"       "ouldshoushouloco"   
    ## [757] "owshosholoco"        "utshushuloco"        "icksisiloco"        
    ## [760] "idesisiloco"         "ignsisiloco"         "imilarsisiloco"     
    ## [763] "implesisiloco"       "incesisiloco"        "ingsisiloco"        
    ## [766] "inglesisiloco"       "irsisiloco"          "istersisiloco"      
    ## [769] "itsisiloco"          "itesisiloco"         "ituatesisiloco"     
    ## [772] "ixsisiloco"          "izesisiloco"         "eepsleesleeloco"    
    ## [775] "ightslisliloco"      "owslosloloco"        "allsmasmaloco"      
    ## [778] "okesmosmoloco"       "ososoloco"           "ocialsosoloco"      
    ## [781] "ocietysosoloco"      "omesosoloco"         "onsosoloco"         
    ## [784] "oonsosoloco"         "orrysosoloco"        "ortsosoloco"        
    ## [787] "oundsosoloco"        "outhsosoloco"        "acespaspaloco"      
    ## [790] "eakspeaspealoco"     "ecialspespeloco"     "ecificspespeloco"   
    ## [793] "eedspeespeeloco"     "ellspespeloco"       "endspespeloco"      
    ## [796] "uaresquasqualoco"    "affstastaloco"       "agestastaloco"      
    ## [799] "airsstaistailoco"    "andstastaloco"       "andardstastaloco"   
    ## [802] "artstastaloco"       "atestastaloco"       "ationstastaloco"    
    ## [805] "aystaystayloco"      "epstesteloco"        "ickstistiloco"      
    ## [808] "illstistiloco"       "opstostoloco"        "orystostoloco"      
    ## [811] "aightstraistrailoco" "ategystrastraloco"   "eetstreestreeloco"  
    ## [814] "ikestristriloco"     "ongstrostroloco"     "ucturestrustruloco" 
    ## [817] "udentstustuloco"     "udystustuloco"       "uffstustuloco"      
    ## [820] "upidstustuloco"      "ubjectsusuloco"      "ucceedsusuloco"     
    ## [823] "uchsusuloco"         "uddensusuloco"       "uggestsusuloco"     
    ## [826] "uitsusuloco"         "ummersusuloco"       "unsusuloco"         
    ## [829] "undaysusuloco"       "upplysusuloco"       "upportsusuloco"     
    ## [832] "upposesusuloco"      "uresusuloco"         "urprisesusuloco"    
    ## [835] "itchswiswiloco"      "ystemsysyloco"       "abletataloco"       
    ## [838] "aketataloco"         "alktataloco"         "apetataloco"        
    ## [841] "axtataloco"          "eateteloco"          "eachteteloco"       
    ## [844] "eamteteloco"         "elephoneteteloco"    "elevisionteteloco"  
    ## [847] "ellteteloco"         "enteteloco"          "endteteloco"        
    ## [850] "ermteteloco"         "erribleteteloco"     "estteteloco"        
    ## [853] "anthathaloco"        "ankthathaloco"       "ethetheloco"        
    ## [856] "enthetheloco"        "erethetheloco"       "ereforethetheloco"  
    ## [859] "eytheytheyloco"      "ingthithiloco"       "inkthithiloco"      
    ## [862] "irteenthithiloco"    "irtythithiloco"      "isthithiloco"       
    ## [865] "outhouthouloco"      "oughthouthouloco"    "ousandthouthouloco" 
    ## [868] "eethreethreeloco"    "oughthrouthrouloco"  "owthrothroloco"     
    ## [871] "ursdaythuthuloco"    "ietitiloco"          "imetitiloco"        
    ## [874] "ototoloco"           "odaytotoloco"        "ogethertotoloco"    
    ## [877] "omorrowtotoloco"     "onighttotoloco"      "oototoloco"         
    ## [880] "optotoloco"          "otaltotoloco"        "ouchtotoloco"       
    ## [883] "owardtotoloco"       "owntotoloco"         "adetratraloco"      
    ## [886] "affictratraloco"     "aintraitrailoco"     "ansporttratraloco"  
    ## [889] "aveltratraloco"      "eattreatrealoco"     "eetreetreeloco"     
    ## [892] "oubletroutrouloco"   "uetruetrueloco"      "usttrutruloco"      
    ## [895] "ytrytryloco"         "uesdaytutuloco"      "urntutuloco"        
    ## [898] "elvetwetweloco"      "entytwetweloco"      "otwotwoloco"        
    ## [901] "ypetytyloco"         "derununuloco"        "derstandununuloco"  
    ## [904] "ionununuloco"        "itununuloco"         "iteununuloco"       
    ## [907] "iversityununuloco"   "lessununuloco"       "tilununuloco"       
    ## [910] "upupuloco"           "onupupuloco"         "eususuloco"         
    ## [913] "ualususuloco"        "aluevavaloco"        "ariousvavaloco"     
    ## [916] "eryveveloco"         "ideoviviloco"        "iewviviloco"        
    ## [919] "illageviviloco"      "isitviviloco"        "otevovoloco"        
    ## [922] "agewawaloco"         "aitwawaloco"         "alkwawaloco"        
    ## [925] "allwawaloco"         "antwawaloco"         "arwawaloco"         
    ## [928] "armwawaloco"         "ashwawaloco"         "astewawaloco"       
    ## [931] "atchwawaloco"        "aterwawaloco"        "aywawaloco"         
    ## [934] "eweweloco"           "earweweloco"         "ednesdayweweloco"   
    ## [937] "eeweweloco"          "eekweweloco"         "eighweweloco"       
    ## [940] "elcomeweweloco"      "ellweweloco"         "estweweloco"        
    ## [943] "atwhawhaloco"        "enwhewheloco"        "erewhewheloco"      
    ## [946] "etherwhewheloco"     "ichwhiwhiloco"       "ilewhiwhiloco"      
    ## [949] "itewhiwhiloco"       "owhowholoco"         "olewhowholoco"      
    ## [952] "ywhywhyloco"         "idewiwiloco"         "ifewiwiloco"        
    ## [955] "illwiwiloco"         "inwiwiloco"          "indwiwiloco"        
    ## [958] "indowwiwiloco"       "ishwiwiloco"         "ithwiwiloco"        
    ## [961] "ithinwiwiloco"       "ithoutwiwiloco"      "omanwowoloco"       
    ## [964] "onderwowoloco"       "oodwowoloco"         "ordwowoloco"        
    ## [967] "orkwowoloco"         "orldwowoloco"        "orrywowoloco"       
    ## [970] "orsewowoloco"        "orthwowoloco"        "ouldwowoloco"       
    ## [973] "itewriwriloco"       "ongwrowroloco"       "earyeyeloco"        
    ## [976] "esyeyeloco"          "esterdayyeyeloco"    "etyeyeloco"         
    ## [979] "ouyoyoloco"          "oungyoyoloco"

## 5.0 Tests for `goloco` Functionality

The following section formally tests the `goloco` function given various
types of input to test the robustness of its intended use and its
response to inappropriate inputs.

### 5.1 String input, starts with consonant

This test evaluates if `goloco` correctly translates a string that
begins with a consonant.

``` r
test_that("translating words that starts with consonant", {
  expect_identical(object = goloco("lollipop"),
                   expected = "ollipoplololoco")
})
```

    ## Test passed 🥇

### 5.2 String input, starts with vowel

This test evaluates if `goloco` correctly translates a string that
begins with a vowel.

``` r
test_that("translating words that starts with vowel", {
  expect_identical(object = goloco("alligator"),
                   expected = "ligatoralalaloco")
})
```

    ## Test passed 😸

### 5.3 Character vector input

This test evaluates if `goloco` correctly translates a vector of
character strings.

``` r
test_that("translating vector of character strings", {
  expect_identical(object = goloco(c("balloon", "clown", "cake")),
                   expected = c("alloonbabaloco", "ownclocloloco", "akecacaloco"))
})
```

    ## Test passed 🌈

### 5.4 Numeric input

This test evaluates if `goloco` correctly prevents numeric inputs.

``` r
test_that("error given numeric input", {
  expect_error(object = goloco(123))
})
```

    ## Test passed 😀
