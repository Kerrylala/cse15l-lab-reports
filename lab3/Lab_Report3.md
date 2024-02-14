Part 1  
I choose the ArrayExample Method bugs from Lab 4. 

1. Test code cause the method to fail:
 ```
  @Test
  public void newtestReverseInPlace(){
    int[] input1 = {1,2,3};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{3,2,1},input1);
  }
 @Test
  public void newtestReversed(){
    int[] input1 = {1,2,3};
    assertArrayEquals(new int[]{3,2,1},ArrayExamples.reversed(input1));
  }
``` 
  
2. Test code cause the method not fail: 
```
  @Test 
  public void testReverseInPlace() {
    int[] input1 = { 3 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
	}
  @Test
  public void testReversed() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
```  
3. Symptom:  
   ![Image](lab31.png)![Image](lab32.png)
4. The Bug Before:  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }

  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);
  }


}
```
The Bug After:  
```
public class ArrayExamples {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }

  // Returns a *new* array with all the elements of the input array in reversed
  // order
  static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }

  // Averages the numbers in the array (takes the mean), but leaves out the
  // lowest number when calculating. Returns 0 if there are no elements or just
  // 1 element in the array
  static double averageWithoutLowest(double[] arr) {
    if(arr.length < 2) { return 0.0; }
    double lowest = arr[0];
    for(double num: arr) {
      if(num < lowest) { lowest = num; }
    }
    double sum = 0;
    for(double num: arr) {
      if(num != lowest) { sum += num; }
    }
    return sum / (arr.length - 1);
  }


}


```
**Bug Before:**   
Bug Explanation for `reverseInPlace`: The `reverseInPlace` method is intended to reverse the elements of an ArrayList.  
The method attempts to iterate over all elements and swap each with the corresponding element from the opposite end of the list (i.e., the element at index size - current index - 1).   
However, the loop incorrectly continues beyond the midpoint of the list, causing previously swapped elements to be overwritten.  
As a result, when the iteration reaches the last element, it is supposed to become the first, but the first element has already been swapped to the last position.   
This leads to a situation where, for example, the array {1, 2, 3} would be incorrectly modified to {3, 2, 3} instead of the expected {3, 2, 1}.  
Bug Explanation for `reversed`: The `reversed method` contains a bug within its for loop.   
The intention is to create a new array with elements in reverse order from the original array.   
However, the bug arises when the elements of the new array are incorrectly assigned to the original array,   
resulting in the original array being filled with default 0 values (since newArray is initialized but not yet populated).   
This mistake causes the original array to effectively become empty, while the new array remains unchanged.  
**Bug After:**  
Fixed Explanation for `reverseInPlace`:The correction is applied within the for loop, which now iterates from the first element (index 0) to the midpoint of the array.   
During each iteration, a temporary variable temp is used to store the element at the current index.   
This element is then swapped with the corresponding element from the end of the array (at index array length - current index - 1).   
With this modification, the input array {1, 2, 3} is correctly reversed to {3, 2, 1} through the swapping of elements at the first and last indices.  
Fixed Explanation for `reversed`: The fix involves adjusting the for loop to correctly assign the elements from the original array to the new array in reversed order.    
Instead of erroneously modifying the original array, the loop now populates the new array with the elements from the original array in reverse sequence.   
The method then returns this newly created reversed array. Consequently, for the input original array {1, 2, 3}, the method correctly returns a new array {3, 2, 1}.   
Part 2  




  
