---
title: "Facebook Puzzle #4 Solution – Breathalyzer"
date: "2011-11-18"
---

After reading this part of the [the puzzle’s description](https://www.facebook.com/careers/puzzles.php?puzzle_id=17):

> A change is defined as replacing a single letter with another letter, adding a letter in any position, or removing a letter from any position. The total score for the wall post is the minimum number of changes necessary to make all words in the post acceptable.

I was immediately reminded of the Levenshtein algorithm I read earlier in the PHP documentation.

Ironically, I didn’t come up with a solution in PHP, but Python:

\[gist id=1377213\]

Here are some sample benchmarks:

$ time python breathalyzer.py "hello world"
0 0 real 0m0.235s user 0m0.170s sys 0m0.020s

The program processes all the words quite fast if they are found identically written in the list.

Here is the the levenshtein algorithm in action:

$ time python breathalyzer.py "hellofdsf worldtra"
3 3 real 0m1.121s user 0m1.110s sys 0m0.007s

Just above a second… passable.

I want it to actually do some work, so let’s throw the phrase found at the puzzle’s page to the program: “tihs sententcnes iss nout varrry goud”

$ time python breathalyzer.py "tihs sententcnes iss nout varrry goud"
1 2 1 1 2 1 real 0m4.092s user 0m4.013s sys 0m0.070s

4 seconds! I haven’t benchmarked other solutions, so I cannot say if my implementation is one of the faster or slower ones… so leave your comments about your solution (and also some rocking benchmarks).
