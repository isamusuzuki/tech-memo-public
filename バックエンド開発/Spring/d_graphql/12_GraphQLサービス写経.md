# 「GraphQL サービスの構築」を写経する

作成日 2024/12/24

参照サイト => [GraphQL サービスの構築](https://spring.pleiades.io/guides/gs/graphql-server)

## 1. Spring Initializer から開始

- ブラウザで[Spring Initializer](https://start.spring.io/)を開く
- 次の設定に変更する

| Key              | Value                                           |
| ---------------- | ----------------------------------------------- |
| Project          | Maven                                           |
| Language         | Java                                            |
| Spring Boot      | 3.4.1                                           |
| Project Metadata | (初期値のまま)                                  |
| Packaging        | Jar                                             |
| Java             | 21                                              |
| Dependencies     | "Spring Web" と "Spring for GraphQL" を追加する |

- GENERATE ボタンをクリックすると、`demo.zip`がダウンロードされる
- 解凍して、Visual Studio Code でフォルダを開く

## 2. スキーマ

`src/main/resources/graphql/`フォルダに`schema.graphqls`ファイルを追加する

```java
type Query {
    bookById(id: ID): Book
}

type Book {
    id: ID
    name: String
    pageCount: Int
    author: Author
}

type Author {
    id: ID
    firstName: String
    lastName: String
}
```

## 3. データのソース

`src/main/java/com/example/demo/`フォルダにBookクラスとAuthorクラスを作成する

Book.java

```java
package com.example.demo;

import java.util.Arrays;
import java.util.List;

public record Book (String id, String name, int pageCount, String authorId) {

    private static List<Book> books = Arrays.asList(
            new Book("book-1", "Effective Java", 416, "author-1"),
            new Book("book-2", "Hitchhiker's Guide to the Galaxy", 208, "author-2"),
            new Book("book-3", "Down Under", 436, "author-3")
    );

    public static Book getById(String id) {
        return books.stream()
        .filter(book -> book.id().equals(id))
        .findFirst()
        .orElse(null);
    }
}
```

Author.java

```java
package com.example.demo;

import java.util.Arrays;
import java.util.List;

public record Author (String id, String firstName, String lastName) {

    private static List<Author> authors = Arrays.asList(
            new Author("author-1", "Joshua", "Bloch"),
            new Author("author-2", "Douglas", "Adams"),
            new Author("author-3", "Bill", "Bryson")
    );

    public static Author getById(String id) {
        return authors.stream()
        .filter(author -> author.id().equals(id))
        .findFirst()
        .orElse(null);
    }
}
```

## 4. データを取得するためのコードの追加

- `src/main/java/com/example/demo/DemoApplication.java`ファイルを編集する

```java
import org.springframework.graphql.data.method.annotation.Argument;
import org.springframework.graphql.data.method.annotation.QueryMapping;
import org.springframework.graphql.data.method.annotation.SchemaMapping;
import org.springframework.stereotype.Controller;

@Controller
public class BookController {
    @QueryMapping
    public Book bookById(@Argument String id) {
        return Book.getById(id);
    }

    @SchemaMapping
    public Author author(Book book) {
        return Author.getById(book.authorId());
    }
}
```

## 5. 最初のクエリを実行する

- GraphQL プレイグラウンドを有効にする

`src/main/resources/application.properties`

```text
spring.graphql.graphiql.enabled=true
``

- アプリケーションを起動する
- [http://localhost:8080/graphiql](http://localhost:8080/graphiql) に移動する
- クエリを入力する

```java
query bookDetails {
  bookById(id: "book-1") {
    id
    name
    pageCount
    author {
      id
      firstName
      lastName
    }
  }
}
```

- ウィンドウの上部にある再生ボタンをクリックする
