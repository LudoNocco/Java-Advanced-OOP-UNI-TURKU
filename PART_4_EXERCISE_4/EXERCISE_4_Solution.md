# EXERCISE 4

## PART A: copied the code in the folder.
## PART B: use generics to modify the handlers so that the Zipper class  can handle different types of objects (like books, files, etc.).
```java
abstract public class Zipper<T> implements AutoCloseable {
    private final String zipFile;
    private final Path tempDirectory;

    public Zipper(String zipFile) throws IOException {
        this.zipFile = zipFile;
        this.tempDirectory = Files.createTempDirectory("tempDir");
    }

    @Override
    public void close() throws IOException {
        try (var stream = Files.walk(tempDirectory)) {
            stream.sorted(Comparator.reverseOrder()).forEach(path -> path.toFile().delete());
        }
    }

    public List<T> run() throws IOException {
        unzip();
        List<Handler<T>> handlers = createHandlers();
        List<T> results = new ArrayList<>();
        for (Handler<T> handler : handlers) {
            results.add(handler.handle());
        }
        return results;
    }

    protected abstract Handler<T> createHandler(Path file);

    protected abstract static class Handler<T> {
        protected final Path file;
        public Handler(Path file) {
            this.file = file;
        }
        abstract public T handle() throws IOException;
    }

    private void unzip() throws IOException {
    }
}
```
In short:
* Generics was added so that the Zipper class can handle different types of objects.
* Handler will process each file in the ZIP archive and convert it into a T type (like a Book object).
* Automatic cleanup: cleans up the temp directory when the object is closed.
## PART C: processing books with TestZipper2
```java
class TestZipper2 extends Zipper<Book> {
    public TestZipper2(String zipFile) throws IOException {
        super(zipFile);
    }

    @Override
    protected Handler<Book> createHandler(Path file) {
        return new Handler<>(file) {
            @Override
            public Book handle() throws IOException {
                List<String> lines = Files.readAllLines(file);
                String title = lines.isEmpty() ? "Untitled" : lines.get(0);
                int lineCount = lines.size();
                long wordCount = lines.stream().flatMap(line -> Stream.of(line.split("\\W+"))).count();
                return new Book(title, lineCount, (int) wordCount);
            }
        };
    }

    public static void main(String[] args) throws IOException {
        try (var zipper = new TestZipper2("books.zip")) {
            List<Book> books = zipper.run();
            books.sort(Comparator.comparing(Book::getTitle));
            books.forEach(System.out::println);
        }
    }
}

class Book {
    private final String title;
    private final int lineCount;
    private final int wordCount;

    public Book(String title, int lineCount, int wordCount) {
        this.title = title;
        this.lineCount = lineCount;
        this.wordCount = wordCount;
    }

    public String getTitle() {
        return title;
    }

    @Override
    public String toString() {
        return "Book: " + title + ", Lines: " + lineCount + ", Words: " + wordCount;
    }
}
```
In short:
* as for reading: we process files as books by extending Zipper<Book> and reading each file, treating it as a book by reading the title, counting the lines, and then counting the words.
* as for sorting: we sort the list of books by title, then we print them.
* as for using generics, I’ve made this choice to make the Zipper class flexible, so that instead of only processing books, we can use it for any type of data, like images or documents. More specifically, if we think of books from the areas of medicine or biology, we can see how this functionality is useful, because those types of books are mainly composed of detailed illustrations. So, if we wanted to process images instead of books, we’d just extend the Zipper class with “Image” instead of “Book”.
