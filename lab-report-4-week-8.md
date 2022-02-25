# Week 8 Lab Report

## MarkdownParse Repositories

[My MarkdownParse repository](https://github.com/akshatja1n/markdown-parse)

[Reviewed MarkdownParse repository](https://github.com/JaredJose/markdown-parse)

## Snippet 1

### Expected Output
Should return a List with the elements:

["`google.com", "google.com", "ucsd.edu"]

![VScode preview](snippet1.png)
>Notice that the three valid links are the text in cyan

### Test Code
The test method from my MarkdownParseTest.java:
```
@Test
public void testSnippet1() throws IOException {
    String fileToTest = "lab-snippet1.md";

    List<String> expected = List.of("`google.com", "google.com", "ucsd.edu");

    assertEquals("Check " + fileToTest, expected, MarkdownParse.getLinks(readFile(fileToTest)));
}
```

### Output for My Implementation
Test failed.
Output:

```
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet1.md expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com, ucsd.edu]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:151)
```

### Output for Reviewed Implementation
Test failed.
Output:

```
1) testSnippet1(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet1.md expected:<[`google.com, google.com, ucsd.edu]> but was:<[url.com, `google.com, google.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet1(MarkdownParseTest.java:78)
```


## Snippet 2

### Expected Output
Should return a List with the elements:

["a.com", "a.com(())", "example.com"]

![VScode preview](snippet2.png)
>Notice that the three valid links are the text in cyan

### Test Code
The test method from my MarkdownParseTest.java:
```
@Test
public void testSnippet2() throws IOException {
    String fileToTest = "lab-snippet2.md";

    List<String> expected = List.of("a.com", "a.com(())", "example.com");

    assertEquals("Check " + fileToTest, expected, MarkdownParse.getLinks(readFile(fileToTest)));
}
```

### Output for My Implementation
Test failed.
Output:

```
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet2.md expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((, example.com]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:160)
```



### Output for Reviewed Implementation
Test failed.
Output:

```
2) testSnippet2(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet2.md expected:<[a.com, a.com(()), example.com]> but was:<[a.com, a.com((]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet2(MarkdownParseTest.java:87)
```


## Snippet 3

### Expected Output
Should return a List with the elements:

["https://ucsd-cse15l-w22.github.io/"]

![VScode preview](snippet3.png)
>Notice that the only valid "link" is the cyan text that is not an actual link

### Test Code
The test method from my MarkdownParseTest.java:
```
@Test
public void testSnippet3() throws IOException {
    String fileToTest = "lab-snippet3.md";

    List<String> expected = List.of("https://ucsd-cse15l-w22.github.io/");

    assertEquals("Check " + fileToTest, expected, MarkdownParse.getLinks(readFile(fileToTest)));
}
```

### Output for My Implementation
Test failed.
Output:

```
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet3.md expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:169)
```



### Output for Reviewed Implementation
Test failed.
Output:

```
3) testSnippet3(MarkdownParseTest)
java.lang.AssertionError: Check lab-snippet3.md expected:<[https://ucsd-cse15l-w22.github.io/]> but was:<[
    https://www.twitter.com
, 
    https://ucsd-cse15l-w22.github.io/
, github.com

And there's still some more text after that.

[this link doesn't have a closing parenthesis for a while](https://cse.ucsd.edu/



]>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at MarkdownParseTest.testSnippet3(MarkdownParseTest.java:96)
```

## Analysis

>For context, my MarkdownParse's getLinks() method is different from most implementations we've seen so far. I split the input string into new lines using "\n". Then, I check each of the lines for links, rather than the full string itself. This made the code simpler to write, for me.

### Snippet 1
Yes, I believe that a code change of less than 10 lines can be made to my code to allow it to not only pass the input in Snippet 1, but also work for singular back ticks in the code. I would have to first check whether the line starts with a back tick. Next, only if the line starts with a back tick, I would check the position of the next back tick. If the square brackets start after the second back tick, then my code works normally, and if the square brackets start before the back tick, I skip that line. This would pass Snippet 1 and include implementation in general for back ticks.

### Snippet 2
Yes, I believe that a code change of less than 10 lines can be made to my code to allow it to not only pass the input in Snippet 2, but also work for brackets and parentheses in the text and links. The solution for my code would be to find the last ")" in the code when trying to find the link, since this would result in any embedded () to be included in the link. An example of this is in Snippet 2, where my code returns a.com(( instead of a.com(()). Looking for the last ")" would allow a.com(()) to be captured, instead of taking the first "(".

### Snippet 3
I don't believe that a code change of less than 10 lines would allow my code to check for new lines in the brackets and parentheses, and pass Snippet 3. This is because, as stated above, my getLinks() separates the input string into lines, and goes through line by line, checking for inputs. When implementing this form of getLinks(), I had no idea that the text can be sepearated or that the link can be on a new line. As a result, I would have to redo a large part of my getLinks, ensuring that I account for new lines inside brackets. One way of doing this could be by only separating the string into new lines if the new line character (\n) is not in between [] or ().