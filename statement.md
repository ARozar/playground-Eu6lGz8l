# Welcome!

This C# template lets you get started quickly with a simple one-page playground.

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
                    Console.WriteLine($"Chose item with value - {itemsToSort[pivotIndex]} as pivot.");
                    //move items that are less than our pivot to the left and items that are greater to the right
                    //by doing this our chosen pivot will be in the correct place
                    int postionedPivotIndex = positionPivot(itemsToSort, startPosition, endPosition, pivotIndex);
                    Console.WriteLine($"Moved to position {postionedPivotIndex + 1}. After partition items = {string.Join(",",itemsToSort)}{Environment.NewLine}");

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
                //move our pivotItem to the end
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

# Advanced usage

If you want a more complex example (external libraries, viewers...), use the [Advanced C# template](https://tech.io/select-repo/386)
