https://www.baeldung.com/java-filter-stream-of-optional

Optional::stream

- Java 9 [will have](http://download.java.net/jdk9/docs/api/java/util/Optional.html#stream--). Nevertheless, `.flatMap(asStream())` or Java 9’s `.flatMap(Optional::stream)` isn’t that different to `.filter(Optional::isPresent).map(Optional::get)`. It doesn’t answer the OP’s question about the validity of the concept in general… 
- https://stackoverflow.com/a/30887846/13662830



https://levelup.gitconnected.com/java-optional-as-a-stream-c7bd79500619


