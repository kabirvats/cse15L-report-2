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
