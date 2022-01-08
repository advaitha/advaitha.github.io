# Challenge 02 - Merge Sorted Lists

I can clearly remembered that day. In in an interview, I was asked to open an IDE and merge two sorted lists. I could not do it in the stipulated time. As you might have guessed, I din't hear back from the interviewer. I am glad to pick it up and now resolve it.

Objective - Given two sorted lists, merge it into a single sorted list \

**Inputs** \
list1 = [4, 5, 6] \
list2 = [-2, -1, 0, 7]

**output** \
[-2, -1, 0, 4, 5, 6, 7] 

It took me some time to think of the approach and testing the logic. After some trail and error, I came up with the following code.

```python
# Create a new list to store merged list
merged_list = []

for i in list1:  # Loop over the first list
    for j in list2: # Loop over the second list
        
        if i < j: # If the first list element is less than the second list element
            merged_list.append(i) # Add the first list element to the merged list
            i = i+1 # Increment the number for the outer loop
            break # Break out of the inner loop
        
        if i > j: # If the first list element is greater than the second list element
            merged_list.append(j) # Add the second list element to the merged list
            list2 = list2[1:]  # Remove the first element from the second list         
            break  # Break out of the inner loop

# Take care of elements in when the first list is exhausted
if len(list2) > 0: 
    for k in list2:
        merged_list.append(k)
```

This was not giving the expected answer

For the inputs \
list1 = [1,3,5,7]  \
list2 = [2,4,6,8] 

The above code was returning the output \
[1, 2, 4, 6, 8]

**Can you guess why is this happening?**

It is because of the highlighted code. 

![merged_list_error](/images/merge_sort_error.png)



The increment for the outer loop was not working.
After some googling, I understood that the behaviour of the for loop in python is not like the for loop in C for(i=0;i<n;i++). Python for loop behaves like a 'foreach' loop in java.

This image from wikipedia should help understand what is a foreach loop.

![foreach_loop](/images/foreach_wiki.png)
 
Then I switched from the for loop to the while loop.
Here is my updated code which gave the expected result

```python
merged_list = []
i = 0

while (i < len(list1)):
    for j in list2:
        
        if list1[i] < j:
            merged_list.append(list1[i])
            i = i+1
            break

        if list1[i] > j:
            merged_list.append(j)
            list2 = list2[1:]
            break

if len(list2) > 0:
    for k in list2:
        merged_list.append(k)
```

For the inputs \
list1 = [1,3,5,7]  \
list2 = [2,4,6,8] 

The output is \
[1, 2, 3, 4, 5, 6, 7, 8]

This is no where near ideal. 

## Scope for improvement
* Traverse the shorter list instead of fixing the list which needs to be traversed - Optimize the time.
* Instead of creating a new list, make changes to the existing list itself - Optimize space usage (I saw some elegant solutions online using this method)


This challenge certainly costed me an interview. I am confident that I will do better atleast if the same question is asked again. I hope that I will do better on other questions as well by completing the goal which I set for this year. I will be back with a new challenge soon.













