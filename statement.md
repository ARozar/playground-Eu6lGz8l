#Intro
Quick sort is an algorithm that is know for its memory efficiency and good overall performance.  Though most real world programming tasks will not require you to implement quick sort there are many lessons that can be learned based on the approach that it takes and the considerations that one must have when using it.

Quicksort is a divide and conquer algorithm that partitions items in an array and then performs its sort operation by recursively calling its partition function on increasingly smaller partitions until all of the items are sorted.


## Breaking things down
At the heart of a Quicksort implementation is a partition function that operates on an array passed to it from a start and ending index specified.  What we refer to as the current partition is the items in the array between the supplied start and end index values.

Each time the partition function is called we 
1. Choose a pivot value that is an index between the start and end index passed to our partition function.
2. Move all items in the current partition that are lower in value than our pivot to the left of the pivot.
3. Move all items in the current partition that are higher in value than our pivot to the right of the pivot.
4. When this is done we take note of the position of the pivot value.
5. We then call our partition function on the slice of the array that is made of the items to the left of the array and on all the items on the right of the array.

We keep on calling our partition function until all the elements are sorted.

### Try running the following code to see the algorithm in action.

```C# runnable
// { autofold
using System;

class Hello 
{
    static void Main() 
    {
// }

           var items = new int[] { 4,5,9,1,7,10,2,8,3,6};

            Console.WriteLine($"About to sort {string.Join(",",items)}{Environment.NewLine}");

            new QuickSort().Sort(items);

            Console.WriteLine(string.Join(",",items));

            Console.ReadLine();


// { autofold
    }
}

        public class QuickSort
        {
            private Random _pivotRng = new Random();

            public void Sort(int[] items)
            {
                partition(items, 0, items.Length - 1);
            }

            public void partition(int[] itemsToSort, int startPosition, int endPosition)
            {
                if (startPosition < endPosition)
                {
                    //choose a number to pivot around
                    int pivotIndex = _pivotRng.Next(startPosition, endPosition);
                    Console.WriteLine($"Chosing item with the value - {itemsToSort[pivotIndex]} as pivot.");
                    //move items that are less than our pivot to the left and items that are greater to the right
                    //by doing this our chosen pivot will be in the correct place
                    int postionedPivotIndex = positionPivot(itemsToSort, startPosition, endPosition, pivotIndex);
                    Console.WriteLine($"Moved to position {postionedPivotIndex + 1}. After partitioning items = {string.Join(",",itemsToSort)}{Environment.NewLine}");

                    //As the pivot is in the right position we now divide the inital collection of items by
                    //doing the same to the items less than our new pivot
                    partition(itemsToSort, startPosition, postionedPivotIndex - 1);
                    //and the same to the items greater than our new pivot
                    partition(itemsToSort, postionedPivotIndex + 1, endPosition);
                }
            }

            public int positionPivot(int[] itemsToSort, int startPosition, int endPosition, int pivotIndex)
            {
                //get the actual value of the item we are pivoting around
                int pivotValue = itemsToSort[pivotIndex];
                //move our pivotItem to the end.
                Swap(itemsToSort, pivotIndex, endPosition);

                //Get the start of the chuck of the items that we are sorting.
                //items less than our pivot item will be moved here
                //every time we do this we increment this value
                //by the time we have gone through every value in the current chunk we are sorting
                //this value will be the correct position of our pivot value
                int currentRank = startPosition;

                //go over every item except for the last one (our pivot) in the current part of the items that we are sorting
                for (int i = startPosition; i < endPosition; i++)
                {
                    var currentItem = itemsToSort[i];

                    if (currentItem < pivotValue)
                    {
                        Swap(itemsToSort, i, currentRank);
                        currentRank += 1;
                    }
                }
                //move the pivot item to the correct position
                Swap(itemsToSort, currentRank, endPosition);

                //our pivot is now in the correct position
                return currentRank;
            }

            private void Swap(int[] items, int left, int right)
            {
                if (left != right)
                {
                    int temp = items[left];
                    items[left] = items[right];
                    items[right] = temp;
                }
            }         
        }
// }
```
The most important thing to note here is everytime the partition function is called the approach that is taken results in the pivot value being placed in the correct position.

### Choosing our pivot
In this tutorial we will choose our pivot by generating a random number between the start and end values of each partition.
We could equally do something such as choose the middle value of the partition.  I real life the implementation of this would be determined by the characteristics of the data that we were processing.

### Positioning our Pivot
So the bulk of the work done in each partioning of the array will involve 
1. comparing the other items in our current partition
2. ensuring the pivot value ends up in the right position
3. taking note of the position of the pivot value once it has been correctly placed
 

```C# runnable
class Hello 
{
    static void Main() 
    {
// }

           var items = new int[] { 4,5,9,1,7,10,2,8,3,6};

            Console.WriteLine($"About to sort {string.Join(",",items)}{Environment.NewLine}");

            new QuickSort().Sort(items);

            Console.WriteLine(string.Join(",",items));

            Console.ReadLine();


// { autofold
    }
                public static int positionPivot(int[] itemsToSort, int startPosition, int endPosition, int pivotIndex)
            {
                //get the actual value of the item we are pivoting around
                int pivotValue = itemsToSort[pivotIndex];
                //move our pivotItem to the end.
                Swap(itemsToSort, pivotIndex, endPosition);

                //Get the start of the chuck of the items that we are sorting.
                //items less than our pivot item will be moved here
                //every time we do this we increment this value
                //by the time we have gone through every value in the current chunk we are sorting
                //this value will be the correct position of our pivot value
                int currentRank = startPosition;

                //go over every item except for the last one (our pivot) in the current part of the items that we are sorting
                for (int i = startPosition; i < endPosition; i++)
                {
                    var currentItem = itemsToSort[i];

                    if (currentItem < pivotValue)
                    {
                        Swap(itemsToSort, i, currentRank);
                        currentRank += 1;
                    }
                }
                //move the pivot item to the correct position
                Swap(itemsToSort, currentRank, endPosition);

                //our pivot is now in the correct position
                return currentRank;
            }

}

        public class QuickSort
        {
            private Random _pivotRng = new Random();

            public void Sort(int[] items)
            {
                partition(items, 0, items.Length - 1);
            }

            public void partition(int[] itemsToSort, int startPosition, int endPosition)
            {
                if (startPosition < endPosition)
                {
                    //choose a number to pivot around
                    int pivotIndex = _pivotRng.Next(startPosition, endPosition);
                    Console.WriteLine($"Chosing item with the value - {itemsToSort[pivotIndex]} as pivot.");
                    //move items that are less than our pivot to the left and items that are greater to the right
                    //by doing this our chosen pivot will be in the correct place
                    int postionedPivotIndex = positionPivot(itemsToSort, startPosition, endPosition, pivotIndex);
                    Console.WriteLine($"Moved to position {postionedPivotIndex + 1}. After partitioning items = {string.Join(",",itemsToSort)}{Environment.NewLine}");

                    //As the pivot is in the right position we now divide the inital collection of items by
                    //doing the same to the items less than our new pivot
                    partition(itemsToSort, startPosition, postionedPivotIndex - 1);
                    //and the same to the items greater than our new pivot
                    partition(itemsToSort, postionedPivotIndex + 1, endPosition);
                }
            }

            public int positionPivot(int[] itemsToSort, int startPosition, int endPosition, int pivotIndex)
            {
                //get the actual value of the item we are pivoting around
                int pivotValue = itemsToSort[pivotIndex];
                //move our pivotItem to the end.
                Swap(itemsToSort, pivotIndex, endPosition);

                //Get the start of the chuck of the items that we are sorting.
                //items less than our pivot item will be moved here
                //every time we do this we increment this value
                //by the time we have gone through every value in the current chunk we are sorting
                //this value will be the correct position of our pivot value
                int currentRank = startPosition;

                //go over every item except for the last one (our pivot) in the current part of the items that we are sorting
                for (int i = startPosition; i < endPosition; i++)
                {
                    var currentItem = itemsToSort[i];

                    if (currentItem < pivotValue)
                    {
                        Swap(itemsToSort, i, currentRank);
                        currentRank += 1;
                    }
                }
                //move the pivot item to the correct position
                Swap(itemsToSort, currentRank, endPosition);

                //our pivot is now in the correct position
                return currentRank;
            }

            private void Swap(int[] items, int left, int right)
            {
                if (left != right)
                {
                    int temp = items[left];
                    items[left] = items[right];
                    items[right] = temp;
                }
            }         
        }
// }
```

### Divide and conquer! Sorting our partitions.

## Putting it all together

## Performance
### Average

### Best case

### Worst case

### Other things to consider
//we control the pivot value
//memory cache

## Take aways

This C# template lets you get started quickly with a simple one-page playground.



