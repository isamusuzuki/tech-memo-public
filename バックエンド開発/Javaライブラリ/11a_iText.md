# iText

作成日 2025/01/27

## インストール

参照サイト => [Installing iText for Java](https://kb.itextpdf.com/itext/installing-itext-for-java)

> you will always need kernel, io and layout.

```xml
<properties>
   <itext.version>9.0.0</itext.version>
</properties>
<dependencies>
    <dependency>
        <groupId>com.itextpdf</groupId>
        <artifactId>kernel</artifactId>
        <version>${itext.version}</version>
    </dependency>
    <dependency>
        <groupId>com.itextpdf</groupId>
        <artifactId>io</artifactId>
        <version>${itext.version}</version>
    </dependency>
    <dependency>
        <groupId>com.itextpdf</groupId>
        <artifactId>layout</artifactId>
        <version>${itext.version}</version>
    </dependency>
    <dependency>
        <groupId>com.itextpdf</groupId>
        <artifactId>bouncy-castle-adapter</artifactId>
        <version>${itext.version}</version>
    </dependency>
</dependencies>
```

## 最初のコード

[README](https://github.com/itext/itext-java)に載っていたコードを写経する

```java
package com.example;

import java.io.IOException;

import com.itextpdf.kernel.pdf.PdfDocument;
import com.itextpdf.kernel.pdf.PdfWriter;
import com.itextpdf.layout.Document;
import com.itextpdf.layout.element.Paragraph;

public class Main {
    public static void main(String[] args) throws IOException {
        try (Document document = new Document(new PdfDocument(new PdfWriter("./hello-pdf.pdf")))) {
            document.add(new Paragraph("Hello PDF!"));
        }
    }
}
```

hello-pdf.pdf ファイルは、Java プロジェクトのルートに生成された

## 日本語フォント

参照サイト => [itext7 で PDF を作成する～日本語を出力しよう～](https://qiita.com/naverut/items/74eb2476b825382a13c8)

```java
package com.example;

import java.io.IOException;
import java.net.URISyntaxException;
import java.nio.file.Path;

import com.itextpdf.kernel.font.PdfFont;
import com.itextpdf.kernel.font.PdfFontFactory;
import com.itextpdf.kernel.geom.PageSize;
import com.itextpdf.kernel.pdf.PdfDocument;
import com.itextpdf.kernel.pdf.PdfWriter;
import com.itextpdf.layout.Document;
import com.itextpdf.layout.element.Paragraph;

public class Nihongo {
    public static void createPdf(String filename) throws URISyntaxException, IOException {
        // PDF文書を作成する
        PdfDocument pdfDoc = new PdfDocument(new PdfWriter(filename));
        Document doc = new Document(pdfDoc, PageSize.A4.rotate());

        // 日本語フォントを読み込む
        Path path = Path.of(Main.class.getClassLoader().getResource("NotoSansJP-Regular.ttf").toURI());
        PdfFont font = PdfFontFactory.createFont(path.toString());

        // PDF文書に文章を追加する
        doc.add(new Paragraph("こんにちは、世界！").setFont(font).setFontSize(48));

        doc.close();
    }
}
```
