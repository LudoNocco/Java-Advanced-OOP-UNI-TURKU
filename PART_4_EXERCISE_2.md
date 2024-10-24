# EXERCISE 2: ordering books in different ways
## PART A: defining the class Book 
```java
import java.nio.file.Path;

class Book implements Comparable<Book> {
    private final Path file;
    private final String title;
    private final int lineCount;
    private final int uniqueWordCount;

    public Book(Path file, String title, int lineCount, int uniqueWordCount) {
        this.file = file;
        this.title = title;
        this.lineCount = lineCount;
        this.uniqueWordCount = uniqueWordCount;
    }

    public Path getFile() { return file; }
    public String getTitle() { return title; }
    public int getLineCount() { return lineCount; }
    public int getUniqueWordCount() { return uniqueWordCount; }

    @Override
    public int compareTo(Book other) {
        return this.title.compareTo(other.title);
    }

    @Override
    public String toString() {
        return String.format("Book[title=%s, lines=%d, uniqueWords=%d]", title, lineCount, uniqueWordCount);
    }
}
```
In the code above we have the following fields and methods:
* Fields:
  * file: Path to the book file.
  * title: First line of the file, considered the book's title.
  * lineCount: Total number of lines in the book.
  * uniqueWordCount: Number of unique words in the book.
* Methods:
  * Getters for each field.
  * compareTo: to implement the natural ordering based on the title.
  * toString: to provide a string representation of the book.
## PART B: natural order by title 
```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.*;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

class TestZipper2 extends Zipper<Book> {
    private List<Book> books = new ArrayList<>();

    TestZipper2(String zipFile) throws IOException {
        super(zipFile);
    }

    @Override
    public void run() throws IOException {
        super.run();
        System.out.println("Books in natural order (by title):");
        sortByTitle();
        printBooks();
    }

    private void sortByTitle() {
        books.sort(null); 
    }

    private void printBooks() {
        books.forEach(System.out::println);
    }

    @Override
    protected Handler<Book> createHandler(Path file) {
        return new Handler<>(file) {
            @Override
            public Book handle() throws IOException {
                String title = extractTitle(file);
                int lineCount = countLines(file);
                int uniqueWordCount = countUniqueWords(file);

                Book book = new Book(file, title, lineCount, uniqueWordCount);
                books.add(book);
                return book;
            }

            private String extractTitle(Path file) throws IOException {
                List<String> lines = Files.readAllLines(file);
                return lines.isEmpty() ? "unknown" : lines.get(0);
            }

            private int countLines(Path file) throws IOException {
                return (int) Files.lines(file).count();
            }

            private int countUniqueWords(Path file) throws IOException {
                Pattern pattern = Pattern.compile("\\W+");
                Set<String> uniqueWords = Files.lines(file)
                        .flatMap(line -> Arrays.stream(pattern.split(line)))
                        .filter(word -> !word.isBlank())
                        .map(String::toLowerCase)
                        .collect(Collectors.toSet());
                return uniqueWords.size();
            }
        };
    }
}
```
The implementation is structured as follows:
* Books List: a collection where we store Book objects, with each object representing a file that we're working with.
* run Method: a function that processes files using super.run(). After that, it sorts the books by their titles and prints them.
* Helper methods:
	* sortByTitle: that organises the books in alphabetical order by their title, and
	* printBooks: that displays each book's information by printing it out.
* createHandler method: a function that processes individual files and transforms them into Book objects.
* Helper methods inside Handler:
	* extractTitle: which takes the first line of the file and uses it as the book’s title.
	* countLines: which finds out how many lines the file has.
	* countUniqueWords: which counts the number of different words in the file by breaking the text into individual words using “split”.
## PART C: order by line count
```java
private void sortByLineCount() {
    books.sort(Comparator.comparingInt(Book::getLineCount));
}
```
And using it in the run method looks like this:
```java
System.out.println("\nBooks in ascending order by line count:");
sortByLineCount();
printBooks();
```
First we use “sortByLineCount” to sort books in ascending order based on lineCount and then we can use the “Comparator” for integer comparison.
## PART D: order by descending unique word count
```java
private void sortByUniqueWordCountDescending() {
    books.sort(Comparator.comparingInt(Book::getUniqueWordCount).reversed());
}
```
Using it in the run method will look like this:
```java
System.out.println("\nBooks in descending order by unique word count:");
sortByUniqueWordCountDescending();
printBooks();
```
Here the reversed comparator allows us to reverse the natural ascending order to descending.
## PART E: composite order by title and line count
```java
private void sortByTitleThenLineCount() {
    books.sort(Comparator.<Book>naturalOrder().thenComparingInt(Book::getLineCount));
}
```
Using it in the run method will look like this:
```java
System.out.println("\nBooks in order by title, then by line count:");
sortByTitleThenLineCount();
printBooks();
```
Here we use “sortByTitleThenLineCount” to sort books first by title, then by line count if titles are equal. Lastly, we use the “Composite Comparator” by calling the method “thenComparingInt”.
