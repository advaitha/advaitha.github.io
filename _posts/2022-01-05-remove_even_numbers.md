# Challenge 01 - Removing Even numbers from the list

Objective - Remove even numbers from a given list \
Example Input = [1,2,3,4,5,6] \
Output = [1,3,5]


My initial solution is as follows:-

```python
def remove_even_error(lst):
    for number in lst:
        if number % 2 == 0:
            lst.remove(number)                           
    return lst
```

I used the below list to test my function
my_list = [-147,17,-73,80,-188,198,-4,-10,-12,-20,-40]


The result was not as expected.
```python
remove_even_error(my_list)
```
[-147, 17, -73, -4, -40]

My initial thought was modulus operator is not working as expected.
I checked the modulus of a few numbers and it was working fine. \
![modulus](/images/modulus_1.png)

Even though the modulus is working fine, I was not able to remove the even numbers from the list.

I googled what might the reason, and found something interesting about modulus operator.

Guess what is the result of `-5 % 4` without  reading further?
Astonishingly it is 3. \
![modulus_2](/images/modulus_2.png)

The modulus operator works as follows in python \
![modulus_git](/images/modulus_git.png) \
(Image Credit: https://stackoverflow.com/questions/3883004/the-modulo-operation-on-negative-numbers-in-python)

But this was not happening in my case. I used a bunch of print statements to check what was going wrong. After spending some time, I found the reason. \
![function_error](/images/function_error.png)

I was modifying the list itself which I was iterating over. 
Once created a new list, I was able to remove the even numbers from the list.

``` python
def remove_even(lst):
    new_list = lst.copy()
    for i in range(len(lst)):
        if (lst[i] % 2) == 0:
            new_list.remove(lst[i])                           
    return new_list
```

```python
remove_even(my_list)
```
[-81, 187, -199, 3, 9, 11, 19, 39]

Further using list comprehension, the code is as follows

```python
def remove_even(lst):
    return [~number for number in lst if number % 2 == 0]
```

In my next challenge, I will be tackling a problem due to which I lost a job oppurtunity in the past. More about it in my next challenge.













