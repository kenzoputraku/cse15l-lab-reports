# Incremental programming, debugging and testing

## First Code Change
![First code change](lab2Report.1.png)

[Link to test file 1](https://github.com/kenzoputraku/markdown-parse/blob/main/image.md)
```
Kenzos-MacBook-Pro:markdown-parse kenzo$ javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java

Kenzos-MacBook-Pro:markdown-parse kenzo$ java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
```
### Symptom
```
[google.com, Image.png]
```

The failure inducing input file (image.md) has a line that contains the reference to the image. The (bug in the) code is not able to distinguish between that line and the line that contains the link so it produces the symptom where the file name (or reference to the image) is included in the array of the links.


## Second code change 
![Second code change](lab2Report.2.png)

[Link to the test file 2](https://github.com/kenzoputraku/markdown-parse/blob/main/incorrect.md)

```
Kenzos-MacBook-Pro:markdown-parse kenzo$ javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java

Kenzos-MacBook-Pro:markdown-parse kenzo$ java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
```
### Symptom 
```
Exception in thread "main" java.lang.StringIndexOutOfBoundsException: begin 22, end -1, length 36
        at java.base/java.lang.String.checkBoundsBeginEnd(String.java:4601)
        at java.base/java.lang.String.substring(String.java:2704)
        at MarkdownParse.getLinks(MarkdownParse.java:29)
        at MarkdownParse.main(MarkdownParse.java:40)
```
The failure inducing file contains a link format that is incorrect because it doesn't have a closing parenthesis at the end of the link, so (the bug) in the code the indexOf for the closeParen returns -1 since it can't find the "(" String. This causes the symptom of StringIndexOutOfBoundsException when the code call the substring method using a -1 index.

## Third code change
![Third code change](lab2Report.3.png)

[Link to test file 3](https://github.com/kenzoputraku/markdown-parse/blob/main/doubleParenthesis.md)

```
Kenzos-MacBook-Pro:markdown-parse kenzo$ javac -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar MarkdownParseTest.java

Kenzos-MacBook-Pro:markdown-parse kenzo$ java -cp .:lib/junit-4.13.2.jar:lib/hamcrest-core-1.3.jar org.junit.runner.JUnitCore MarkdownParseTest
```
### Symptom
```
Exception in thread "main" java.lang.StringIndexOutOfBoundsException: String index out of range: -2
        at java.base/java.lang.StringLatin1.charAt(StringLatin1.java:48)
        at java.base/java.lang.String.charAt(String.java:1512)
        at MarkdownParse.getLinks(MarkdownParse.java:25)
        at MarkdownParse.main(MarkdownParse.java:46)
```
The failure inducing file contains a line that have a link format but have double open and closing parenthesis. This causes the code to get the first closing parenthesis and so in the second loop, the openBracket will be equal to -1 since there is no more open Bracket in the file so when the code try to checks for the character before the open bracket in the if statement it will try to find the charAt(-2) and this produces the symptom of StringIndexOutofBoundsException.
