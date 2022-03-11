# Week 10 Lab Report

## Finding Different outputs
I used `diff` on the outputs of running the bash files for both implemenations of markdown-parse, mine and the given one. This allowed me to see which files resulted in different outputs between the two implementations. After finding some different outputs, I checked the respective files for the outputs to see what the expected output was, which let me determine if each implemenation had the correct output.

## Different Output 1
One file with different ouputs was 22.md, and the code is shown below:

```
[foo](/bar\* "ti\*tle")
```

### Correct Output
The correct output using VSCode's md file preview is:

\[bar*\]

This is because the part of the string in the parentheses is encapsulated in quotes, which makes that part a title for the link. A title refers to the text that shows up when hovering over a link. In this case, that title is ti*tle.

### My Output
My output was:

```
[/bar\* "ti\*tle"]
```

### Given Markdown Output
Given markdown output was:

```
[]
```

Both of the given implementations are wrong, since the actual output for 22.md is bar*.

### Bug Fix 1
There are two things to note here. 

The first is accounting for escaped characters that have been escaped using the back slash. To solve this, I can code a helper method that takes a string, and returns a string with the escaped characters converted to what they are supposed to be. For example, the string "bar\\*" would be converted to "bar\*" by my method. 

```
// Returns the given string with the escaped characters converted to the actual values.
private static String escapedCharacters(String text){}
```


Then, I would call this method on any string before adding it to the output ArrayList. 

```
toReturn.add(escapedCharacters(returnString));
```

The second thing to account for is text that denotes the title for a link. For example, if you hover in the link outputted by the 22.md file, it would display ti*tle. As a result, to account for this, I would remove any text in quotations in my returnString string, including the quotations themselves. This can be done by first checking if there are two quotations in the returnString, and then using indexOf() I would find substring and remove it from the returnString. 

## Different Output 2
Another file with different ouputs was 41.md, and the code is shown below:

```
[a](url &quot;tit&quot;)
```

### Correct Output
The correct output using VSCode's md file preview is no link at all:

[]

This is because there is a space in the string encapsulated by the parentheses, which invalidates the link code. A space is valid when the string after the space is encapsulated by quotations indicating that it is a title. However, there is no title here.

### My Output
My output was:

```
[url &quot;tit&quot;]
```

### Given Markdown Output
Given markdown output was:

```
[]
```

Only my implementation for this is incorrect, because I return a link whereas no link is actually displayed in the correct output or the given markdown output.

### Bug Fix 2
To fix this bug, the main thing I have to do is check for spaces in the string encapsulated by parentheses. If there is a space, AND if the string after the space is not a title, the link is not valid. As a result, I do not add it to the ArrayList that will be returned. I can check if the string after the space is not a title if it is not encapsulated by quotations. All of this can be done with a few nested if/else statements in the deepest code block in my code, where a string is added to the ArrayList that will be returned.