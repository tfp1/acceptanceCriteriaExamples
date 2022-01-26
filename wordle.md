1. Get list of [all words](https://gist.github.com/cfreshman/a03ef2cba789d8cf00c08f767e0fad7b). N = 2315
2. Calculate the frequency of each letter by position
3. Select the top 5 letters for each position
4. Deduping the list, returns a set of 14 letters
5. Sorting those letters by _total_ frequency gives us the most valuable letters.
6. The first 5 letters `{e,a,r,o,t}` can easily be anagrammed into `{orate|oater}`.
    - Neither word appears in the list, but the likelihood of a first guess word is only valuable once, so we're fine "wasting" this guess anyway
8. To calculate the value of `orate`, I sum the frequency of each letter (o,r,a,t,e) across all known words, then divide by the total number of letter-positions. We get a score of `39.41%`.
9. The second 5 letters `{l,i,s,n,c}` proves a little harder to anagram. I opt for `{spicy}`, skipping `l` and `n` to use letter #12 and #13. Spicy scores an additional `22.38%`, meaning we've covered 61.79% of letter-positions in our first two guesses.
10. To round out our top 14, we have `{l,n,u,b}`. Obviously we need to add to this set for a complete guess, so this method seems to be at its logical conclusion.
11. I did try to guess a few valuable words based on our available letters and the rest of the frequency chart, but found the method to be lacking the rigor necessary to provide confidence in its conclusion. These words included: `baulk` (14.39%), `fault` (18.40%), `flunk` (18.88%).
12. Now, I decide to calculate the _actual_ value of our first two guesses.
13. Since we have a known list of all solutions, we can calculate the result of our first two guesses and use that to sieve the remaining word list.
14. With our sieved word list after guess #1, we have a range of 1 to 195 possible solutions _regardless of our success in guessing letters_. Not bad to eliminate 92% of potential solutions in a single guess.
15. Using the sieved word list from guess #1 for guess #2, we have a range of 1 to 26. Only 1.1% of solutions remain!
16. For our third guess, we _could_ use one of our words above. Returning `baulk` (1-12), `fault` (1-13), `flunk` (1-13) which all provide fairly good odds for our final three guesses. But could we find a more effective word?
17. Depending on the results for Guess #1 and Guess #2, we have a superset of 51 unique words. Note, each scenario still only has a maximum 26 results.
18. To maximize the next guess, I want to find the high value letters we want to hit. So we repeat the process of calculating the frequency of laters, regardless of position.
19. First, we filter out the letters we've already guessed `{o,r,a,t,e,s,p,i,c,y}`
20. Now we have 16 unique letters (hmm... 16 + 10 = 26. Interesting that we haven't eliminated any letters yet!)
21. Sorting by total frequency again gives our new top 5 letters as `{l,b,d,g,n}`. Not a lot of vowels to work with here.
22. Anagramming these letters with a wild card gives us a few viable options to pursue. All but `bling` appear on our original word list, so they're all viable guesess at this point.
    - bland
    - blend
    - blind
    - bling
    - blond
    - gland
23. In a real world game, we'd have more concrete information that would guide us to choosing one of these options, but we can still measure their effectiveness in sieving the word list in the abstract.
24. Interestingly, we see a very narrow range only slightly better than our unrigorous words, seeing a range of 1-7 versus the unrigorous range of 1-13
25. At this point, we have either guessed the word or have a total of 6 options remaining.
26. Cheating a bit, we can look at the words that elude the first three guesses and they all share the same trait: a duplicate of a letter that we've already guessed!
    - For example, `coombs` would fall in this category if it were a real word and on the list.
    - We'd have matched on the `{o,s,c}`but all are in the wrong position and we have not received information indicating the `o` is repeated.
28. Incidentally, `bling` performs the worst, returning an additional word that doesn't have a duplication of letters we've already guessed.
