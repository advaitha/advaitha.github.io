# Challenge 03 - HackerRank - Company logo design

This time I decided to pick up a medium level challenge from HackerRank.

**Objective** - A newly opened multinational brand has decided to base their company logo on the three most common characters in the company name. They are now trying out various combinations of company names and logos based on this condition. Given a string , which is the company name in lowercase letters, your task is to find the top three most common characters in the string.
* Print the three most common characters along with their occurrence count.
* Sort in descending order of occurrence count.
* If the occurrence count is the same, sort the characters in alphabetical order.

**Inputs** \
aabbbccde

**output** \
b 3 \
a 2 \
c 2

**Constraints**
* Length of the string will be greater than 3 and less than or equal to 10000
* String will have atleast 3 distinct characters

I know that using the Counter functions from  collections module will make this problem easier. But I was not sure if I can use it in this challenge. HackerRank din't mind using collections module, so I decided to go ahead with Counter.

```python
from collections import Counter 
counter = Counter(s) # Returns a dictionary with the count of each character
top_3 = counter.most_common(3) # Returns the top 3 most common characters
for alphabet, count in top_3: # Print the top 3 values
    print(alphabet, count)
```

Running locally I got the expected output for the 'aabbbccde' string. I was glad that I was able to come up with a solution so quickly. But when I submitted the solution, half of the tests were not passing.

My suspected may be the most_common() method is not working as I expected.
I reversed the example input provided and tried to test it out.

**Inputs** \
'ccbbaa'

The above code output was as follows:

c 2 \
b 2 \
a 2

This is not the expected answer. so I need to sort the alphabets in the ascending order. 


First I wanted to tackle the case when all top 3 alphatbet counts are equal. In that case I need to sort the alphabets in the ascending order.

```python
if top_3[0][1] == top_3[1][1] == top_3[2][1]:
    top_3 = sorted(top_3, key=lambda x : x[0])
```

If counts are not equal, then this is what I intend to do:

![logic](/images/clogic.png)

The implementation in code for the logic

```python
if top_3[0][1] == top_3[1][1]:
    if top_3[0][0] > top_3[1][0]:
        a = top_3[0]
        b = top_3[1]
        top_3[0] = b
        top_3[1] = a

if top_3[1][1] == top_3[2][1]:
    if top_3[1][0] > top_3[2][0]:
        a = top_3[1]
        b = top_3[2]
        top_3[1] = b
        top_3[2] = a
```

This time I was confident that the test cases will pass.
But atlease half of them failed again.

![what's happening](/images/scratch.jpeg)

Then I got hold of a test case to see what is going wrong. 

### **Can you guess what is missing?**

*Hint:*

**Input** \
'qwertyuiopasdfghjklzxcvbnm'

**Output** \
e 1 \
q 1 \
w 1 \

This is not the expected output!?

Oh! I took care of sorting after taking the top three results. \
**what if the counts for all the alphabets are equal before sorting?** \
The results returned by _Counter object will be the first occurance and is not sorted in ascending order_.

what if I take care of sorting before using the Counter itself?

```python
from collections import Counter
s = sorted(s)
counter = Counter(s)
top_3 = counter.most_common(3)
for alphabet, count in top_3:
    print(alphabet, count)
```

The got the expected results for the string 'qwertyuiopasdfghjklzxcvbnm'
a 1
b 1
c 1

And this time all the tests were cleared.


## Lessons learnt
* Before finalizing the code, test all the assumptions. I inherently assumed Counter and most_common will return results in sorted ascending order.
* Think about different scenarios and see if the code returns expected results in all conditions


## Scope for improvement
* Trying the solution without using the Counter object and most_common method

I will be back with a new challenge soon.













