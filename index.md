# Lab Report 2
## Part 1
Code for ```StringServer```:
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList; 
class Handler implements URLHandler {

    String str = "";
    ArrayList<String> words = new ArrayList<String>();
    public String handleRequest(URI url) {
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");
            String temp = "";
            for(String p : parameters){
                temp = temp + p + " ";
            }
            temp = temp.replace("s ", "");
            if(!words.contains(temp.trim())){
                words.add(temp.trim());
            }
            for(int i =0; i< words.size(); i++){
                if(!str.contains(words.get(i))){
                    str = str + words.get(i) + "\n";
                }
            }
        } 
        return str;
    }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}

``` 

1. Screenshot 1: <br>
<img width="607" alt="截屏2023-04-24 下午9 58 20" src="https://user-images.githubusercontent.com/130001791/234178269-be507c9a-64d6-4506-865e-2412b4bbcca8.png"><br>

-```handleRequest``` and ```main``` method are called.<br>
-Relevant argument:```url``` for ```handleRequest``` and ```args``` for ```main```.<br>
values of relavant field in ```handleRequest```: ```str```, ```words```,```parameters```,```temp```.<br>
values of relavant field in ```main```method:```port```.<br>
-Change of Values:```str``` changed from an empty string to  ```hello```, ```temp``` changed from empty string to ```hello```.```words```is an arraylist that added ```temp```as element, so it contains ``` hello```.```parameters``` is ```["s", "hello"]```, it does not change because I did not have field that can change its value. ```port``` does not change, it is ```4000``` because I used ```4000``` as argument in terminal.<br>

2. Screenshot 2: <br>
<img width="727" alt="截屏2023-04-24 下午9 59 38" src="https://user-images.githubusercontent.com/130001791/234178465-7aada3f0-ba43-45e6-82ac-b6302fb6c296.png">

-```handleRequest``` and ```main``` method are called.
-Relevant argument:```url``` for ```handleRequest``` and ```args``` for ```main```.<br>
values of relavant field in ```handleRequest```: ```str```, ```words```,```parameters```,```temp```.<br>
values of relavant field in ```main```method:```port```.<br>
-Change of Values:<br>
```str``` changed from an empty string to  
```
hello
how are you
```

<br>

```words``` is an arraylist that add ```temp``` as elements, so it contains  
```
["hello", "how are you"]
```

<br>

```temp``` changed from empty string to 
```
how are you
```
<br>

```parameters``` is ```["s", "how are you"]```, it does not change because I did not have field that can change its value<br>

```port``` does not change, it is ```4000``` because I used ```4000``` as argument in terminal.
<br>
## Part 2
1. This is a failure-inducing input for a buggy program from lab3:

```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
@Test 
public void testReverseInPlace2() {
  int[] input1 = { 1,2,3 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 3,2,1 }, input1);
 }
 }
```

2. This is an input that doesn't include a failure:

```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
@Test 
public void testReverseInPlace() {
  int[] input1 = { 2 };
  ArrayExamples.reverseInPlace(input1);
  assertArrayEquals(new int[]{ 2 }, input1);
}
}
```

3. This is the symptom, as the out put of running the test above:<br>
<img width="691" alt="截屏2023-04-24 下午5 07 22" src="https://user-images.githubusercontent.com/130001791/234140992-368cabaa-acd1-4bed-995c-c417c6f2030a.png">
4. This is the bug:<br>
Before fixing:

```
public class ArrayExamples {
static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
}
```

After fixing :
```
public class ArrayExamples {
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
}
```

Before fixing, the for loop first swap the first element with the last element in array. For example, in an array of 3 element ```{1,2,3}```, 
`arr[0]= arr[2]` will make the array look like ```{3,2,3}`. But when the iteration reach the end of the array `arr[2]`, it makes 
`arr[2] = arr[0]` But here we already changed the value stored in `arr[0]`, so `arr[2]=arr[0]` will make the array look like ```{3,2,3}```, which cause the failure in Junit test showed in previous screenshot.

Oops! We do not want to change the value stored in the first half of the array when we reverse in place the second half of the array, otherwise the value we want in the second half of the array does not change! To do this, we can creat a ```int temp```  and make it equal to ```arr[i]``` in the for-loop body to store the value of first half of the array. Also we can make the array iterate only to the middle of the array, so the fisrt half of the array will be reversed with the second half. To do this we revise the loop condition to be ```i < arr.length/2``` .  since the loop only iterate to the middle of the array, so the reverse in palce the second half of the array, we use ```arr[arr.length - i - 1] = temp``` to let the value of second half of the array equal to temp which is the original value of the first half. This way, all the elements in the array are reversed in place.<br>
Here is a screenshot of the running test and terminal output after we fix the bug:<br>
<img width="696" alt="截屏2023-05-06 下午12 07 56" src="https://user-images.githubusercontent.com/130001791/236642402-090aaf14-07d9-464e-953e-263eb1fdcf32.png">


## Part 3
In lab2 and lab3, I learned how to write a servee and how to add things to the server, and I also learned how to reverse in place arrays and how to debug.
