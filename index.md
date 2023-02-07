# Lab Report 2 - Servers and Bugs

## Part 1 - Servers

Last week, we learned how to host a server. For this lab, I wrote a server called StringServer that created a server that displayed an element of its URL on the webpage.

Here is the code for the StringServer:
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;
class Handler implements URLHandler {
    ArrayList<String> things = new ArrayList<>();
    public String handleRequest(URI url) {
        if (url.getPath().equals("/add-message")) {
            String[] params = url.getQuery().split("=");
                if(params[0].equals("s"));
                    things.add(params[1]);  
                    return String.join("\n", things);
        }
        return "ERROR: please visit the path /add-message?s= \"Anything You Want\"";;
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```
Here are some screenshots from the running server:

![Image](https://kabirvats.github.io/cse15L-report-2/server1.png)

In this first screenshot, the method handleRequest is called with the input parameter of the url, and the array params is used to check if the string after the question mark matches the expected "s=[whatever]" behavior. It is important to input the url that the user uses into this method because the method handleRequest parses through this inputted url to find the requested changes to the webpage. After this, the user's string is put on the arraylist Things and shown on the webpage. The fields that are changed are the array params and the ArrayList things, and the displayed message is the return value of handleRequest. 

![Image](https://kabirvats.github.io/cse15L-report-2/server2.png)

In this second screenshot, the same method is called with the new input url, and the user's second requested string is added to things, with a new line. The second value of params is also changed in the process, as well as the ArrayList things. then this is returned by handleRequest.

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
This method works for some inputted arrays, but for other inputted arrays it fails to return the correct average. The type of input that produces an error is any array with multiple instances of the lowest value. Shown below is a JUnit test that produces an incorrect result, and the failure message from the terminal. The method inputs the array {14,26,14,50}, where the average without the lowest number (14) is 30.0. However, the faulty code calculates the average as if neither 14 was there, still using the length of 3 (the length of the array - 1).
```
@Test
public void testAverageWithoutLowest() {
  double[] input1 = {14,26,14,50};
  assertEquals(30.0, ArrayExamples.averageWithoutLowest(input1), 0);
}
```
The test fails:

![Image](https://kabirvats.github.io/cse15L-report-2/TestFailure.png)

Now shown is a JUnit test that produces a correct result, and the resulting success message from the terminal. This method has an input of {14,26,12,50}. The method has the same expected output as the previous test(calculates the average without 12) of 30.0. The method generates a correct output of 30.0.
```
@Test
public void testAverageWithoutLowest() {
  double[] input1 = {14,26,12,50};
  assertEquals(30.0, ArrayExamples.averageWithoutLowest(input1), 0);
}
```
The test succeeds:

![Image](https://kabirvats.github.io/cse15L-report-2/TestsSuccess.png)

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
The fully fixed method would look be as follows:
```
static double averageWithoutLowest(double[] arr) {
  if(arr.length < 2) { return 0.0; }
  double lowest = arr[0];
  for(double num: arr) {
    if(num < lowest) { lowest = num; }
  }
  double sum = 0;
  boolean foundLowest = false;
  for(double num: arr) {
    if(!foundLowest && num == lowest) {
      foundLowest = true;
      continue;
    }
    sum += num;
  }
  return sum / (arr.length - 1);
}
```

This fix makes it so that the first time the lowest value is found, it is skipped in calculation and the loop continues to execute. After the first time it finds the lowest value, it changes the boolean `foundLowest` to `true`, so no other values are skipped.

One unique thing I learned during the past two weeks that I hadn't learned previously was how easy it is to run a server on your own device, without having to use anything other than a simple java file. Being able to host the server, and modify the text that was displayed was very intriguing, and I'm curious what tasks I can use this to perform in the future.
