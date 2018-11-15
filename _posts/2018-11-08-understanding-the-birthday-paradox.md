---
title: Understanding the Birthday Paradox
author: tshanky
permalink: /2018/11/08/understanding-the-birthday-paradox/
modified:
excerpt: "The Birthday Paradox or Birthday Problem highlights a mathematical and probabilistic outcome that is intuitively hard to believe."
tags: [mathematics, probability]
---
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

By definition, a paradox is a seemingly absurd statement or proposition that when investigated or explained may prove to be well-founded and true. 

It's hard to believe that there is more than **50% chance** that at least 2 people in a group of randomly chosen **23** people have the same birthday. Further, if the group had 75 people then its almost certain that 2 of them were born on the same day of the year. The number 23 appears too small for such a high probability that at least 2 people have the same birthday, and hence the paradox. In this post we will reason through the birthday problem and learn and understand the mathematics behind the chance of sharing birthdays.

## What are the Chances at least 2 people out of 23 People in a Room Have the Same Birthday?

The number of different pairs one can form with 23 people is the number of ways you can select 2 people from a total of 23. The general formula for choosing `k` things from a set of `n` things is:

$$
\binom{n}{k}
$$

Using this formula you can calculate that there are 253 different pairs of people among a set of 23 people.

$$
\binom{23}{2} = \frac{23!}{2!(23-2)!} = \frac{23\times22}{2} = 253
$$

> If you are confused about the combination formula or do not understand factorial, you will benefit from reviewing the fundamentals of combination. Watch the [Khan Academy video on the subject](https://www.khanacademy.org/math/precalculus/prob-comb#combinations "Combinations"){:target="_blank"} or read the [Wikipedia page on combination](https://en.wikipedia.org/wiki/Combination "Combination"){:target="_blank"}. 

For a given pair, the probability that those two people have the same birthday is `1/365`. Therefore, the probability that the two have different birthdays is `364/365`, `1 - 1/365`. There are 365 days in the year. 1 out of these 365 days is when the two have the same birthday and the other 364 days is when their birthdays don't match.

> We have ignored the case of a leap year, when there are 366 days in a year. The math here wouldn't quite hold for people born on February 29th.

The probability that none of the 253 pairs share birthdays is:

$$
\left(\frac{364}{365}\right)^{253} = 0.499522846
$$

This implies that the probability that at least one of these pairs share a birthday is `1 - 0.499522846`, which is `0.500477154`. 

In other words, the chance that one or more of these pairs, out of a total of 253 pairs, share a birthday, is greater than 50%. 50.0477% to be precise.

## How Does this Work for n People in a Room?

What if there weren't 23 people in the room? What if there were 45, or 15, or 80 people in the room? To calculate the probabilities for all these cases and many more, let's consider the generic variable `n`.

You can select pairs of people among n people as follows:

$$
\binom{n}{2} = \frac{n!}{2!(n-2)!} = \frac{n\times(n-1)}{2}
$$

Following the same reasoning as for the case of 23 people, the probability that all pairs among these n people have different birthdays is:

$$
\left(\frac{364}{365}\right)^{\frac{n\times(n-1)}{2}}
$$

The general probability function to calculate the chance that at least one of these pairs have the same birthday is:

$$
p(n) = 1 - \left(\frac{364}{365}\right)^{\frac{n\times(n-1)}{2}}
$$

p(45) is 0.9338645976, a 93.3864% chance of a match, p(15) is 0.2502879086, around 25%, and p(80) is 0.9998282405 or 99.9828%, which implies almost a certain match.

## Are Birthdays of any two People Among a set of n People Independent?

In our formula for calculating probability of at least one pair with a matching birthday, among all possible pairs from a total of `n` people, we stated that the probability of no match is:

$$
\left(\frac{364}{365}\right)^{\frac{n\times(n-1)}{2}}
$$

This formula is built on 2 assumptions:
1. The distribution of birthdays is uniform across the sample space. That is, the probability that someone's birthday is on any of the 365 possible days is equally likely. Empirical studies have shown that this statement is not true. Months between July and October typically have higher births. [Here is](https://www.panix.com/~murphy/bday.html "On uniformity of distribution of birthdays in a calendar year"){:target="_blank"} one interesting write-up summarizing the reasonableness of the assumption of uniformity of distribution of birthdays in a calendar year.
2. For all possible pairs among the n people, the chance that any one pair has matching birthday is independent of all the other pairs. However, reason through a bit and you will soon realize that its not the case. Let's assume Alice and Bob have the same birthday and Bob and Charlie have the same birthday. Applying transitivity, its clear that the match for these 2 pairs also implies that Alice and Charlie have the same birthday. Therefore, the chance of match for all possible pairs is not independent of each other. [In probability theory, independence of events or occurrences](https://en.wikipedia.org/wiki/Independence_(probability_theory) "Independence"){:target="_blank"} allows us to calculate joint probabilities by simply multiplying their probabilities, which is what we do in our formula.

Let's rework the case of 23 people in a room. The more accurate joint probability of each of the 23 people having a different birthday is:

$$
1\times\left(1 - \frac{1}{365}\right)\times\left(1 - \frac{2}{365}\right)\times\left(1 - \frac{3}{365}\right)...\times\left(1 - \frac{22}{365}\right)
$$

Why? Start with any one person among those 23. The chance that the person has a unique birthday is 100%. For the second person, the chance of having a different birthday effectively means having a birthday on any of the other days other than the birthday of the first person, which is $$1 - \left(\frac{1}{365}\right)$$. Follow the same logic for the next set of people and you will find that the formula we just wrote is obvious.

While, this formula is more accurate, it is a bit complicated as compared to our formula under the assumption of independence of events. Let's use some mathematical tools that will help us approximate this formula.

According to [Taylor Series](https://en.wikipedia.org/wiki/Taylor_series "Taylor Series"){:target="_blank"} an exponential function can be written as an infinite sum of terms as follows:

$$
e^x = 1 + x + \frac{x^2}{2!} + ....
$$

When x is very small, the exponential function can be approximated to the first couple of terms of this series.

$$
e^x \approx 1 + x
$$

> Will explain Taylor Series and its applications in a subsequent post.

Applying Taylor Series approximation for the exponential function

$$
e^\frac{-1}{365} \approx \left(1 - \frac{1}{365}\right)
$$

Our complex joint probability function then takes a shape like so:

$$
1\times(e^\frac{-1}{365})\times(e^\frac{-2}{365})\times(e^\frac{-3}{365})...\times(e^\frac{-22}{365})
$$

$$e^0 = 1$$ so the probability that no pairs among 23 people have matching birthdays is approximated to

$$
e^\frac{(-1-2-3...-22)}{365} = e^\frac{-(22\times23)}{2\times365} = 0.499998
$$

Fairly close to the value, 0.499522846, we calculated assuming independence of events.

> The sequence of first n integers forms an [Arithmetic progression](https://en.wikipedia.org/wiki/Arithmetic_progression "Arithmetic progression"){:target="_blank"}. The sum of such a progression of first n integers is $$\frac{n\times(n+1)}{2}$$.

Using this approach, the general approximated formula for at least one pair with same birthday among `n` people is:

$$
p(n) \approx 1 - e^\frac{-(n\times(n+1))}{2\times365}
$$

## What About Martians?

The average length of a solar day, also known as sol, on Mars is 24 hours, 39 minutes, and 35.244147 seconds. [A year on Mars is about 668.5991 sols](https://en.wikipedia.org/wiki/Timekeeping_on_Mars "Timekeeping on Mars"){:target="_blank"}, which is roughly around 686.98 Earth solar days. Let's approximate it to 668 Martian sols for our calculation.

Therefore, the probability that at least 2 Martians among a room full of `n` Martians share a birthday is:

$$
p(n) \approx 1 - e^\frac{-(n\times(n+1))}{2\times668}
$$

This means you need 31 Martians in a room so that there is greater than 50% chance that at least 2 of them share a birthday.

## The Birthday Problem Formula

The general formula we have so far

$$
p(n) \approx 1 - e^\frac{-(n\times(n+1))}{2\times365}
$$

could be approximated further by dropping the lower powers of n in the exponential. The formula then takes the following form:

$$
p(n) \approx 1 - e^\frac{-(n^2)}{2\times365}
$$

365 is the total number of possibilities for number of days on Earth. We noticed that it is 668 for Mars. To generalize the birthday problem to finding at least 1 match among pairs chosen from `n` things out of a total number of `U` things in a sample space or universe, we get the following:

$$
p(n) \approx 1 - e^\frac{-(n^2)}{2\times(U)}
$$

If the desired probability was 50%, i.e. p(n) = 0.5, then we could approximate to:

$$
0.5 \approx 1 - e^\frac{-(n^2)}{2\times(U)}
$$

$$
-0.5 \approx e^\frac{-(n^2)}{2\times(U)}
$$

taking the natural log on both sides

$$
-2\log_{e}(0.5)\times(U) \approx n^2
$$

$$
n \approx 1.177\sqrt{U}
$$


## What are the Chances of Somebody Else in the Same Room Having the Same Birthday as you?

This question can be a bit deceiving. Many confuse this question with the more common one of finding at least 1 match among all possible pairs in a room full of `n` people!

When you are finding possible matches for your birthday among `n` people in a room, you are looking at only `n-1` pairs and not $$\binom{n}{2}$$ pairs. This means, whereas there are 253 possible pairs among 23 people in a room, there are only 22 pairs possible where you are a member of that pair.

This changes the mathematics drastically.

Whereas you need only 23 people to have a greater than 50% chance that at least 1 pair chosen from these 23 people share a birthday, you need 253 people other than yourself in a room to have a 50% chance that someone has the same birthday as you in the room. Extending the calculation further, you need 2775 people in a room to have greater than 99.9506% chance of a match. This same chance is achieved with only 75 people, when the problem seeks to achieve a possible match, not necessarily with you but with anyone in the room.

You may have noticed that the power of factorial and combination are at play

$$
\binom{23}{2} = 253
$$ 
`and` 
$$
\binom{75}{2} = 2775
$$

## Other Fun Problems Based on Similar Ideas

Here are a couple of fun problems to think about:
1. What is the probability of someone else in a room full of n people having the same birthday as me and no other person sharing a birthday?
2. What is the chance of finding at least one pair in a group of n people with birthdays within k calendar days of each other's?

If you reason it through, based on what you have just read, then you will find the answer. Will leave this as an exercise for you to enjoy.

## Conclusion

Paradoxes are counter intuitive. The birthday problem has been written about a lot and often serves as an interesting example in many probability and statistics classes. I hope you enjoyed reading this post and learnt the fundamentals behind this problem. Chime in with your questions, thoughts, or comments.