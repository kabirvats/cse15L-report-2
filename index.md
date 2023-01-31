# Lab Report 2 - Servers and Bugs

## Part 1 - Servers

## Part 2 - Bugs

This week, I was tasked with finding bugs in certain programs. One such program was designed to find the average of an array, disregarding the lowest term. The method is shown below:
```
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
```
This method works for some inputted arrays, but for other inputted arrays it fails to return the correct average. The type of input that produces an error is any array with multiple instances of the lowest value. Shown below is a JUnit test that produces an incorrect result, and the failure message from the terminal.
```
@Test
public void testAverageWithoutLowest() {
  double[] input1 = {14,26,14,50};
  assertEquals(30.0, ArrayExamples.averageWithoutLowest(input1), 0);
}
```
The test fails:

![Image](https://kabirvats.github.io/cse15l-report-1/testFailure.PNG)

Now shown is a JUnit test that produces a correct result, and the resulting success message from the terminal:
```
@Test
public void testAverageWithoutLowest() {
  double[] input1 = {14,26,12,50};
  assertEquals(30.0, ArrayExamples.averageWithoutLowest(input1), 0);
}
```
The test succeeds:

![Image](https://kabirvats.github.io/cse15l-report-2/testSuccess.PNG)

The bug originates from this line of code: `if(num != lowest) { sum += num; }` which neglects the value of `num` if it is equal to the lowest value of the array. 

In order to fix this, we have to ensure it only neglects the first instance of the lowest value of the array, and still uses all subsequent instances of that value in the calculation of the average. One way to fix this would be changing the for:each loop that calculates the sum from
```
for(double num: arr) {
    if(num != lowest) { sum += num; }
}
```
to 
```
boolean foundLowest = false;
for(double num: arr) {
    if(!foundLowest && num == lowest) {
      foundLowest = true;
      continue;
    }
    sum += num;
}
```
This fix makes it so that the first time the lowest value is found, it is skipped in calculation and the loop continues to execute. After the first time it finds the lowest value, it changes the boolean `foundLowest` to `true`, so no other values are skipped.
