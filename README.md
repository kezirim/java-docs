# java-docs
Java Documentations
## Data Structure and Algorithms

### Arrays

The `Arrays` class provides utility methods for working with arrays. Here are some common methods along with their time complexities:

1. **sort()**: Sorts the specified array into ascending numerical order.
   - Time Complexity: O(n log n) - Uses a variant of quicksort or mergesort algorithms for primitive types, and `Timsort` for objects, which performs well on nearly sorted data.

2. **binarySearch()**: Searches for a specific element in the sorted array using the binary search algorithm.
   - Time Complexity: O(log n) - Finds the index of the element in logarithmic time.

3. **copyOf()**: Copies the specified array, truncating or padding with zeros (if necessary) to obtain the specified length.
   - Time Complexity: O(n) - Copies elements proportionate to the size of the array.

4. **fill()**: Assigns the specified value to each element of the specified array.
   - Time Complexity: O(n) - Sets each element to the specified value, proportionate to the size of the array.

5. **toString()**: Returns a string representation of the contents of the specified array.
   - Time Complexity: O(n) - Generates a string representation by iterating through the array.

Here's an example demonstrating the usage of some of these methods:

```java
import java.util.Arrays;

public class ArraysExample {
    public static void main(String[] args) {
        // Initializing an array
        int[] numbers = {5, 2, 8, 1, 6};

        // Sorting the array
        Arrays.sort(numbers);
        System.out.println("Sorted Array: " + Arrays.toString(numbers));

        // Searching for an element in the sorted array
        int index = Arrays.binarySearch(numbers, 6);
        System.out.println("Index of 6: " + index);

        // Copying a portion of the array
        int[] copy = Arrays.copyOf(numbers, 3);
        System.out.println("Copied Array: " + Arrays.toString(copy));

        // Filling the array with a specific value
        int[] filledArray = new int[5];
        Arrays.fill(filledArray, 10);
        System.out.println("Filled Array: " + Arrays.toString(filledArray));
    }
}
```

This code showcases the usage of various `Arrays` methods such as sorting, binary search, copying, and filling arrays. The output will display the sorted array, the index of a searched element, a copied portion of the array, and a filled array with a specific value. The time complexities provided are for typical scenarios; actual performance may vary based on the JVM implementation and input data.


#### Method binarySearch
Certainly! The `Arrays.binarySearch()` method in Java performs a binary search on a sorted array to find the index of a specified value. It uses the binary search algorithm, which is efficient for searching in sorted collections.

##### Method Signature:

```java
public static int binarySearch(type[] a, type key)
public static int binarySearch(type[] a, int fromIndex, int toIndex, type key)
```

##### Parameters:

1. `a`: The array to be searched. It should be sorted in ascending order according to the natural ordering of its elements or by a custom comparator.

2. `key`: The value to be searched for in the array.

3. `fromIndex`: The index to start the search (inclusive).

4. `toIndex`: The index to end the search (exclusive).

##### Return Value:

- If the key is found in the array, the method returns the index of the key.
- If the key is not found, it returns a negative value (specifically, `-insertionPoint - 1`). The `insertionPoint` is the index at which the key would be inserted to maintain the sorted order of the array.

##### Considerations:

- The array must be sorted before using `binarySearch()`. If it's not sorted, the result is undefined.
- For arrays containing objects, the objects should implement the `Comparable` interface or a custom comparator should be provided.
- If the array contains duplicate values, the returned index can be any one of the matching elements. There's no guarantee which one will be returned.
- If the key is not found, the return value provides information on where the key would be inserted to maintain the sorted order.

##### Examples:

```java
int[] numbers = {1, 3, 5, 7, 9, 11};
int index = Arrays.binarySearch(numbers, 7); // Searches for 7 in the entire array
// index will be 3 (index of 7 in the array)
```

```java
int[] numbers = {1, 3, 5, 7, 9, 11};
int index = Arrays.binarySearch(numbers, 1, 4, 5); // Searches for 5 in the range [1, 4)
// index will be 2
```

```java
int[] numbers = {1, 3, 7, 9, 11};
int index = Arrays.binarySearch(numbers, 1, 4, 5); // Searches for 5 in the range [1, 4)
// index will be -3 (-(insertionPoint) - 1)
// Insertion point is 3 as 5 should be inserted at index 3 to maintain the sorted order within [1, 4)
```

The `Arrays.binarySearch()` method is powerful for quickly finding elements in a sorted array but requires the array to be sorted for reliable results.

##### Examples of binarySearch with object arrays

```java
import java.util.Arrays;
import java.util.Comparator;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }
}

public class BinarySearchObjectArrayExample {
    public static void main(String[] args) {
        Person[] people = {
            new Person("Alice", 25),
            new Person("Bob", 30),
            new Person("Charlie", 20),
            new Person("David", 35)
        };

        // Sorting the array based on the person's name using a Comparator
        Arrays.sort(people, Comparator.comparing(Person::getName));

        // Creating a sample person to search for
        Person searchPerson = new Person("Charlie", 20);

        // Performing binary search using a custom Comparator
        int index = Arrays.binarySearch(people, searchPerson, Comparator.comparing(Person::getName));

        if (index >= 0) {
            System.out.println("Person found at index: " + index);
        } else {
            System.out.println("Person not found");
        }
    }
}
```


```java
import java.util.Arrays;

class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    @Override
    public int compareTo(Person otherPerson) {
        // Compare persons based on their age
        return Integer.compare(this.age, otherPerson.getAge());
    }

    @Override
    public String toString() {
        return name + " - " + age;
    }
}

public class BinarySearchObjectArrayExample {
    public static void main(String[] args) {
        // Creating an array of Person objects (sorted by age)
        Person[] persons = {
                new Person("Alice", 25),
                new Person("Bob", 30),
                new Person("Charlie", 35),
                new Person("David", 40)
        };

        // Sorting the array based on age (natural ordering defined in Person class)
        Arrays.sort(persons);

        // Searching for a person with age 35 in the array
        Person searchKey = new Person("", 35);
        int index = Arrays.binarySearch(persons, searchKey);

        // Displaying the result of the search
        if (index >= 0) {
            System.out.println("Person found at index " + index + ": " + persons[index]);
        } else {
            System.out.println("Person not found. Insertion point would be at index " + (-index - 1));
        }
    }
}
```


### ArrayList
`ArrayList` is a dynamic array implementation that allows flexible manipulation of elements, offering fast insertion and retrieval of elements at specific positions. Here are some key methods along with their time complexities:

1. **add(E e)**: Appends the specified element to the end of the list.
   - Time Complexity: O(1) amortized (O(n) occasionally) - Appending an element at the end takes constant time on average, but occasionally it might need to resize the underlying array, which takes linear time.

2. **add(int index, E element)**: Inserts the specified element at the specified position in the list.
   - Time Complexity: O(n) - Inserting an element at a specific index requires shifting subsequent elements if the index is not at the end.

3. **get(int index)**: Retrieves the element at the specified index in the list.
   - Time Complexity: O(1) - Retrieving an element by index is constant time.

4. **remove(int index)**: Removes the element at the specified position in the list.
   - Time Complexity: O(n) - Removing an element at a specific index requires shifting subsequent elements.

5. **size()**: Returns the number of elements in the list.
   - Time Complexity: O(1) - Getting the size is constant time as the `ArrayList` keeps track of its size internally.

Here's an example demonstrating the usage of `ArrayList`:

```java
import java.util.ArrayList;

public class ArrayListExample {
    public static void main(String[] args) {
        // Creating an ArrayList
        ArrayList<Integer> arrayList = new ArrayList<>();

        // Adding elements to the list
        arrayList.add(5);
        arrayList.add(10);
        arrayList.add(15);

        // Printing the elements in the list
        System.out.println("ArrayList elements: " + arrayList);

        // Inserting an element at a specific index
        arrayList.add(1, 8);

        // Printing the updated elements in the list
        System.out.println("Updated ArrayList elements: " + arrayList);

        // Removing an element at a specific index
        arrayList.remove(2);

        // Printing the final elements in the list
        System.out.println("Final ArrayList elements: " + arrayList);
    }
}
```

This code snippet showcases common operations such as adding elements, inserting an element at a specific index, and removing an element at a specific index from an `ArrayList`. `ArrayList` is suitable when frequent access and iteration are required but may not be the best choice for frequent insertions or deletions in the middle, as these operations can be relatively slow due to the shifting of elements.


### HashTable
The `HashTable` class has various methods that allow you to manipulate key-value pairs in the hash table. Here are some of the essential methods:

1. **put(key, value)**: Inserts a key-value pair into the hash table. If the key already exists, the value associated with that key is replaced.

2. **get(key)**: Retrieves the value associated with the specified key. Returns `null` if the key is not found.

3. **remove(key)**: Removes the key-value pair associated with the specified key from the hash table.

4. **containsKey(key)**: Checks if the hash table contains a specific key. Returns `true` if the key is found; otherwise, returns `false`.

5. **containsValue(value)**: Checks if the hash table contains a specific value. Returns `true` if the value is found; otherwise, returns `false`.

6. **isEmpty()**: Checks if the hash table is empty. Returns `true` if the hash table contains no key-value mappings.

7. **size()**: Returns the number of key-value mappings in the hash table.

8. **keys()**: Returns an `Enumeration` of all the keys in the hash table.

9. **elements()**: Returns an `Enumeration` of all the values in the hash table.

10. **clear()**: Removes all key-value pairs from the hash table, making it empty.

It's important to note that `HashTable` is a synchronized class, meaning it's thread-safe. However, due to its synchronization, it might be slower in performance compared to other newer implementations like `HashMap`, which isn't synchronized but generally faster in a single-threaded environment.

Here's an example demonstrating some of these methods:

```java
import java.util.Hashtable;
import java.util.Enumeration;

public class HashTableExample {
    public static void main(String[] args) {
        Hashtable<String, Integer> hashtable = new Hashtable<>();

        // Adding elements
        hashtable.put("One", 1);
        hashtable.put("Two", 2);
        hashtable.put("Three", 3);

        // Retrieving value using key
        int value = hashtable.get("Two");
        System.out.println("Value for key 'Two': " + value);

        // Checking if a key exists
        boolean keyExists = hashtable.containsKey("Four");
        System.out.println("Key 'Four' exists: " + keyExists);

        // Getting keys
        Enumeration<String> keys = hashtable.keys();
        System.out.println("Keys in hashtable:");
        while (keys.hasMoreElements()) {
            System.out.println(keys.nextElement());
        }

        // Removing an element
        hashtable.remove("Three");
        System.out.println("Size after removing: " + hashtable.size());

        // Clearing the hashtable
        hashtable.clear();
        System.out.println("Is the hashtable empty? " + hashtable.isEmpty());
    }
}
```

### PriorityQueue
`PriorityQueue` is an implementation of a priority queue data structure that orders elements according to their natural ordering or by a specified comparator. Here are some of the main methods of the `PriorityQueue` class, along with their time complexities:

1. **add(E e) / offer(E e)**: Inserts the specified element into this priority queue.
   - Time Complexity: O(log n) - In the worst case, where n is the size of the queue, insertion takes logarithmic time.

2. **remove() / poll()**: Retrieves and removes the head of this queue, or returns null if this queue is empty.
   - Time Complexity: O(log n) - Removing the head element also takes logarithmic time.

3. **peek()**: Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.
   - Time Complexity: O(1) - Peeking at the head element can be done in constant time.

Here's an example of how you might use a `PriorityQueue` in Java:

```java
import java.util.PriorityQueue;

public class PriorityQueueExample {
    public static void main(String[] args) {
        // Creating a priority queue
        PriorityQueue<Integer> pq = new PriorityQueue<>();

        // Adding elements to the queue
        pq.offer(10);
        pq.offer(5);
        pq.offer(8);
        pq.offer(12);

        // Printing the elements of the queue
        System.out.println("PriorityQueue elements: " + pq);

        // Removing elements from the queue
        while (!pq.isEmpty()) {
            System.out.println("Removed: " + pq.poll());
        }
    }
}
```
#### Custom `Comparator` Example:

```java
public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return name + " - " + age;
    }
}

import java.util.Comparator;
import java.util.PriorityQueue;

public class PriorityQueueWithComparatorExample {
    public static void main(String[] args) {
        // Creating a priority queue with a custom Comparator (ordering by age in reverse)
        PriorityQueue<Person> pq = new PriorityQueue<>(Comparator.comparingInt(Person::getAge).reversed());

        // Adding Persons to the queue
        pq.offer(new Person("Alice", 25));
        pq.offer(new Person("Bob", 30));
        pq.offer(new Person("Charlie", 20));

        // Printing the elements of the queue
        System.out.println("PriorityQueue elements with custom comparator:");
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```
#### Custom `Comparator` with 2D arrays:

A custom comparator implementation that compares 2D arrays based on the first element as the primary criterion and the second element as the tie-breaker.

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.PriorityQueue;

public class PriorityQueueWith2DArrayComparatorAndTieBreaker {
    public static void main(String[] args) {
        // Custom Comparator to compare 2D arrays based on first element (primary) and second element (tie-breaker)
        Comparator<int[]> arrayComparator = Comparator.<int[]>comparingInt(arr -> arr[0])
                                                .thenComparingInt(arr -> arr[1]);

        // Creating a priority queue with the custom Comparator
        PriorityQueue<int[]> pq = new PriorityQueue<>(arrayComparator);

        // Adding 2D arrays (each containing two elements) to the queue
        int[][] array2D = {{1, 3}, {3, 4}, {2, 2}, {1, 2}, {3, 1}};
        pq.addAll(Arrays.asList(array2D));

        // Printing the elements of the queue
        System.out.println("PriorityQueue elements with custom comparator and tie-breaker:");
        while (!pq.isEmpty()) {
            int[] current = pq.poll();
            System.out.println(Arrays.toString(current));
        }
    }
}
```

Output:

```
PriorityQueue elements with custom comparator and tie-breaker:
[1, 2]
[1, 3]
[2, 2]
[3, 1]
[3, 4]
```

#### Reverse Order Example:

```java
import java.util.Collections;
import java.util.PriorityQueue;

public class ReverseOrderPriorityQueueExample {
    public static void main(String[] args) {
        // Creating a priority queue with reverse ordering of strings
        PriorityQueue<String> pq = new PriorityQueue<>(Collections.reverseOrder());

        // Adding elements to the queue
        pq.offer("Apple");
        pq.offer("Banana");
        pq.offer("Orange");
        pq.offer("Grapes");

        // Printing the elements of the queue
        System.out.println("PriorityQueue elements with reverse ordering:");
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```

### LinkedList
`LinkedList` is a class that implements the `List` and `Deque` interfaces, providing methods to manipulate linked lists. When used as a queue, it behaves like a double-ended queue (`Deque`) allowing elements to be added or removed from both ends.

Here are some main methods of `LinkedList` that are commonly used in a queue-like manner, along with their time complexities:

1. **add(E e) / offer(E e)**: Adds the specified element to the end of the list.
   - Time Complexity: O(1) - Adding an element to the end of a linked list is constant time.

2. **remove() / poll()**: Removes and returns the first element in the list.
   - Time Complexity: O(1) - Removing the first element is constant time in a linked list.

3. **peek() / peekFirst()**: Retrieves, but does not remove, the first element in the list.
   - Time Complexity: O(1) - Retrieving the first element is constant time.

Here's an example of how you might use a `LinkedList` as a queue:

```java
import java.util.LinkedList;

public class LinkedListAsQueueExample {
    public static void main(String[] args) {
        // Creating a linked list as a queue
        LinkedList<Integer> queue = new LinkedList<>();

        // Adding elements to the queue
        queue.offer(5);
        queue.offer(10);
        queue.offer(15);

        // Printing the elements in the queue
        System.out.println("Queue elements: " + queue);

        // Removing elements from the queue
        while (!queue.isEmpty()) {
            System.out.println("Removed: " + queue.poll());
        }
    }
}
```

This code demonstrates using a `LinkedList` as a queue by adding elements to the end of the list (`offer`) and removing elements from the front of the list (`poll`). The output will display the elements in the queue and then remove them in the order they were added.

Keep in mind that while `LinkedList` allows fast insertion and deletion at both ends, accessing elements in the middle can be less efficient compared to random access data structures like arrays (`O(n)` time complexity for accessing elements by index).


### Deque Interface
`Deque` interface represents a double-ended queue, allowing insertion, deletion, and retrieval of elements from both ends. It stands for "double-ended queue" and supports operations at both ends of the queue.

Here are some of the primary methods provided by the `Deque` interface:

1. **addFirst(E e) / offerFirst(E e)**: Inserts the specified element at the front of the deque.
   - Time Complexity: O(1) - Adding an element at the beginning of a deque is constant time.

2. **addLast(E e) / offerLast(E e)**: Inserts the specified element at the end of the deque.
   - Time Complexity: O(1) - Adding an element at the end of a deque is constant time.

3. **removeFirst() / pollFirst()**: Removes and returns the first element in the deque.
   - Time Complexity: O(1) - Removing the first element is constant time.

4. **removeLast() / pollLast()**: Removes and returns the last element in the deque.
   - Time Complexity: O(1) - Removing the last element is constant time.

5. **getFirst() / peekFirst()**: Retrieves, but does not remove, the first element of the deque.
   - Time Complexity: O(1) - Retrieving the first element is constant time.

6. **getLast() / peekLast()**: Retrieves, but does not remove, the last element of the deque.
   - Time Complexity: O(1) - Retrieving the last element is constant time.

Here's an example of how you can use a `Deque` as a double-ended queue:

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class DequeExample {
    public static void main(String[] args) {
        // Creating a deque
        Deque<Integer> deque = new ArrayDeque<>();

        // Adding elements at both ends of the deque
        deque.addFirst(5);
        deque.addLast(10);
        deque.offerFirst(3);
        deque.offerLast(15);

        // Printing the elements in the deque
        System.out.println("Deque elements: " + deque);

        // Removing elements from both ends of the deque
        System.out.println("First element removed: " + deque.pollFirst());
        System.out.println("Last element removed: " + deque.pollLast());

        // Printing the updated elements in the deque
        System.out.println("Updated deque elements: " + deque);
    }
}
```

This code snippet demonstrates using an `ArrayDeque` (a specific implementation of `Deque`) to perform various operations such as adding elements at both ends, removing elements from both ends, and printing the deque's contents.

`Deque` provides flexibility in handling elements at both ends, making it suitable for implementing double-ended queues, stacks, or queues, depending on how the methods are utilized.
