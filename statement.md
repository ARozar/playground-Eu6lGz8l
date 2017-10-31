# Intro
Quick sort is an algorithm that is known for its memory efficiency and good overall performance, particularly on larger unstructured datasets.  Though most real world programming tasks will not require you to implement quick sort there are many lessons that can be learned based on the approach that it takes and the considerations that one must have when using it.

Quicksort is a divide and conquer algorithm that partitions items in an array and then performs its sort operation by recursively calling its partition function on increasingly smaller partitions until all of the items are sorted.

## Breaking things down
At the heart of a Quicksort implementation is a partition function that operates on an array passed to it from a start and ending index specified.  What we refer to as the current partition are the items in the array between the supplied start and end index values.

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
The most important thing to note here is everytime the partition function is called the approach that is taken results in the pivot value being placed in the correct position.  It should be noted the five steps mentioned above are a high level description of what we look to do.  The actual implementation to achieve these five steps will be slightly different.

### Partitioning part one -  Choosing our pivot
In this tutorial we will choose our pivot by generating a random number between the start and end values of each partition.
We could equally do something such as choose the middle value of the partition.  I real life the implementation of this would be determined by the characteristics of the data that we were processing.

### Partitioning part two - Positioning our items and our Pivot
So the bulk of the work done in each partioning of the array will involve 
1. comparing the other items in our current partition
2. ensuring the pivot value ends up in the right position
3. taking note of the position of the pivot value once it has been correctly placed
 
The first thing we are doing is to move to the pivot value to the  end of the array.  That way we don't have to move it every time we find something smaller than it.  We then create a variable (called `pivotRank` in our example) that will track every time we find and move an item that is smaller than our pivot value to the left of our pivot.  I the case of our approach this means each time we find an item that is smaller than our pivot value we swap its position with the `pivotrank` variable then increment the `pivotrank` value by one.

Once our partition function has gone through every item in the current partition except from our pivot value we the `pivotRank` variable should represent the correct position of our pivot value.  So we can now take it from the end of the current partition and move it to its correct place.  Once we have done this we can return the value of the `pivotRank` which will now be used to work out call our partition function on the items that are less than and greater than our `pivotRank`.

The use of the `pivotrank` here is one of the ways in which our implementation differs from the high level thought process behind the algorithm.  Though a high level description talks about moving elements to the left and right of the pivot, we never actually need to move items to the right. By moving the pivot to the end of the current partition, tracking the `pivotrank` and swapping the pivot with the item at the index of the pivot rank we ensure that all items that are greater than our pivot are on the logical right of our pivot.

//see it in action...

```C# runnable
// { autofold
using System;
class Hello 
{
    static void Main() 
    {
// }
            var items = new int[] { 4,5,9,1,7,10,2,8,3,6};
            //Choose our pivot
            Random _pivotRng = new Random();
            
            int pivotIndex = _pivotRng.Next(0, items.Length);
            
            Console.WriteLine($"About to sort {string.Join(",",items)}{Environment.NewLine}");

            int postionedPivotIndex = positionPivot(items, 0, items.Length - 1, pivotIndex);

            Console.WriteLine(string.Join(",",items));

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
                        Console.WriteLine($"Moving item with value { currentItem } to position {currentRank}");
                        Swap(itemsToSort, i, currentRank);
                        currentRank += 1;
                    }
                }
                //move the pivot item to the correct position
                Swap(itemsToSort, currentRank, endPosition);

                //our pivot is now in the correct position
                return currentRank;
            }
            private static void Swap(int[] items, int left, int right)
            {
                if (left != right)
                {
                    int temp = items[left];
                    items[left] = items[right];
                    items[right] = temp;
                }
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
After positioning all the elements in our partition to the left and right of our pivot and taking note of the new index of the pivot (remember that the pivot value should now be in the correct position) we call our partition function again on two slices of the current partition that we are sorting, the subset of our partion that is to the left (all the values smaller than our pivot) and items to the right (all the items that are larger than our pivot).

One thing to note is that --//guard//
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



