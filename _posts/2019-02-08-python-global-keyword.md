
# On Python's `global` keyword

... and Python3's `nonlocal`

*Some a year ago I got confused about when (not) to use `global`. Then I learnt about proper usage and decided to write a blog about it. It took some time but here it is, my very first short blog programming blog post.*

Even though I knew about the `global` keyword, I had no need to use it. Most cases when I work with global variable look like this one:


```python
the_list = [1, 2, 3, 2, 3, 5, 5]
```


```python
my_global_dict_counter = {}

def add_to_dict_counter(my_list):
    for x in my_list:
        if x in my_global_dict_counter:
            my_global_dict_counter[x] += 1
        else:
            my_global_dict_counter[x] = 1

add_to_dict_counter(the_list)
my_global_dict_counter
```




    {1: 1, 2: 2, 3: 2, 5: 2}



Very similar code can lead to a pitfall. What the heck?


```python
my_global_int_counter = 0

def add_to_int_counter(my_list):
    for x in my_list:
        my_global_int_counter += 1

add_to_int_counter(the_list)
my_global_int_counter
```


    ---------------------------------------------------------------------------

    UnboundLocalError                         Traceback (most recent call last)

    <ipython-input-4-3696d367f54c> in <module>
          5         my_global_int_counter += 1
          6 
    ----> 7 add_to_int_counter(the_list)
          8 my_global_int_counter
    

    <ipython-input-4-3696d367f54c> in add_to_int_counter(my_list)
          3 def add_to_int_counter(my_list):
          4     for x in my_list:
    ----> 5         my_global_int_counter += 1
          6 
          7 add_to_int_counter(the_list)
    

    UnboundLocalError: local variable 'my_global_int_counter' referenced before assignment


For me this is very surprising behavior and until now I regard it to be ugly and contraintuitive. So what is the explanation? A variable in function is implicitly global unless it is being assigned (then it is considered local). That's it. There is a FAQ on the topic: https://docs.python.org/3/faq/programming.html#what-are-the-rules-for-local-and-global-variables-in-python. For the sake of completeness, working snippet:


```python
my_global_int_counter = 0

def add_to_int_counter(my_list):
    global my_global_int_counter
    for x in my_list:
        my_global_int_counter += 1

add_to_int_counter(the_list)
my_global_int_counter
```




    7



The need for `global` results from implicit variable declaration and ilustrates downside of it. For comparison, php has also implicit variable declaration and `global` keyword but all variables are local by default which makes the usage of globals more consistent.
