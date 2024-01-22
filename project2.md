# Project 2

Use R to simulate the following problems. Use the conditional probability simulation examples as guide. \@ref(conditional-probability-dice) and \@ref(conditional-probability-test)

::: {.problem}
**Problem 1**

A cab was involved in a hit and run accident at night. Two cab companies, the Green and the Blue, operate in the city. You are given the following data:

* 85% of the cabs in the city are Green and 15% are Blue.
* A witness identified the cab as Blue. 
* The court tested the reliability of the witness under the same circumstances that existed on the night of the accident and concluded that the witness correctly identified each one of the two colors 80% of the time and failed 20% of the time.

What is the probability that the cab involved in the accident was Blue rather than Green?
:::

The next problems are variants of the boy-girl paradox. For each of them simulate a large number of families with two children using a data frame with two columns. The first column is the older kid (no twins) and the second one the younger one. Each row represents the children of the family. Each child has an equal chance of being either a boy or a girl. 

::: {.problem}
**Problem 2**

Consider a family with two children. Given that the older one is a boy,  what is the probability that both children are boys?
:::

This problem has no ambiguity.

::: {.problem}
**Problem 3**

Consider a family with two children. Given that one of the children is a boy, what is the probability that both children are boys?
:::

This problem can be ambiguous. How do we know that at least one of the children is a boy?

If we do know the gender of the children of all families, we get one answer. We select the families that have at least one boy and then check how many of them have two boys. Do this for this problem.


On the other hand, we may not know this for every family. Consider the following case:


::: {.problem}
**Problem 4**

Mr. Smith is the father of two. We meet him walking along the street with a young boy whom he proudly introduces as his son. What is the probability that Mr. Smith’s other child is also a boy?
:::

In this case, we know that Mr. Smith has at least one boy but the situation is different.  

For this problem, assume that Mr. Smith was equally likely to select one of his two children to walk with him. Therefore, define a new column named **walking** that selects one of the two children in the family at random. To do this use the ```rowwise()``` operator described before. Then select the rows that have a "boy" in the **walking** column and check how many of these rows have two boys.

A variant of Problem 2 that makes the paradox more clear is: *Mr. Smith is the father of two. We meet him walking along the street with a young boy whom he proudly introduces as his older child. What is the probability that Mr. Smith’s other child is also a boy?*



