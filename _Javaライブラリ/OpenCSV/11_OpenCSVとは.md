# OpenCSV ライブラリ

作成日 2025/01/10、更新日 2025/01/14

## OpenCSV とは

A simple library for reading and writing CSV in Java

公式マニュアル（英語） => [Opencsv](https://opencsv.sourceforge.net/)

Maven リポジトリ => [OpenCSV](https://mvnrepository.com/artifact/com.opencsv/opencsv)

- 最新バージョン: 5.9
- 日付: Nov 22, 2023

pom.xml に dependency を追加する

```xml
<dependency>
    <groupId>com.opencsv</groupId>
    <artifactId>opencsv</artifactId>
    <version>5.9</version>
</dependency>
```

## 参照サイト

1. [opencsv を使ってみよう](https://qiita.com/mogumogusityau/items/3ef2c865238c054fae02)
1. [study-java-opencsv](https://github.com/foreverfl/study-java-opencsv/blob/main/docs/JAPANESE.md)
1. [OpenCSV の使用方法について](https://qiita.com/T-H9703EnAc/items/0cbb28935ca7a7e30d9c)
1. [OpenCSV の使用方法について（データクラスに配列やリストがある場合）](https://qiita.com/T-H9703EnAc/items/65eae82f23cc8f925362)

## サンプルコード 1

```text
src/main/java/com/exmaple/demo/
    |--MyBean.java
    `--ZuluApplication.java
```

```java
// MyBean.java
@Data
@Builder
public class MyBean {
    @CsvBindByPosition(position = 0)
    @CsvBindByName(column = "カテゴリー")
    private String category;

    @CsvBindByPosition(position = 1)
    @CsvBindByName(column = "トピック")
    private String topic;

    @CsvBindByPosition(position = 2)
    @CsvBindByName(column = "質問")
    private String question;

    @CsvBindByPosition(position = 3)
    @CsvBindByName(column = "答え")
    private String answer;
}

// ZuluApplication.java
public class ZuluApplication {
    @SneakyThrows
    public static void main (String[] args) {
        List<MyBean> beans = new ArrayList<MyBean>();
        beans.add(MyBean.builder()
            .category("プラモデル")
            .topic("初心者")
            .question("最初に必要な道具")
            .answer("ニッパー")
            .build());
        beans.add(MyBean.builder()
            .category("レゴ")
            .topic("大人")
            .question("おすすめ商品")
            .answer("アルテミス ロケット発射台")
            .build());

        StringWriter writer = new StringWriter();

        StatefulBeanToCsv<MyBean> beanToCsv = new StatefulBeanToCsvBuilder<MyBean>(writer).build();
        beanToCsv.write(beans);
        writer.close();

        System.out.print(writer.toString());
    }
}
```

### サンプルコード 1 の問題点

- JavaBean に取り付けたアノテーションが 2 つあると、カラム名が省略される
- `@CsvBindByPosition`アノテーションをコメントアウトすると、カラムの順番が狂う

## 2 つのアノテーションが両方機能するようにする

```text
src/main/java/com/exmaple/demo/
    |--CustomMappingSrategy.java  ... 追加
    |--MyBean.java                ... 変更なし、省略
    `--ZuluApplication.java       ... 修正
```

- `ColumnPositionMappingStrategy`を継承したクラスを作成する
- `generateHeader()`メソッドをオーバーライドする
- `CustomMappingStrategy`クラスをインスタンス化した後、`setType()`は必須

```java
// CustomMappingSrategy.java
import com.opencsv.bean.BeanField;
import com.opencsv.bean.ColumnPositionMappingStrategy;
import com.opencsv.bean.CsvBindByName;
import com.opencsv.exceptions.CsvRequiredFieldEmptyException;

public class CustomMappingStrategy<T> extends ColumnPositionMappingStrategy<T> {
    @Override
    public String[] generateHeader(T bean) throws CsvRequiredFieldEmptyException {
        super.generateHeader(bean);

        // データクラス（T bean）のフィールドの数
        final int fieldSize = getFieldMap().values().size();

        // ヘッダー情報の初期化
        String[] header = new String[fieldSize];

        for (int i = 0; i < fieldSize; i++) {
            // データクラス（T bean）のフィールドを取得
            BeanField<T, Integer> beanField = super.findField(i);
            String columnHeaderName = extractHeaderName(beanField);
            header[i] = columnHeaderName;
       }
        return header;
    }

    private String extractHeaderName(final BeanField<T, Integer> beanField) {
        if (beanField == null || beanField.getField() == null || beanField.getField().getDeclaredAnnotationsByType(
                CsvBindByName.class).length == 0) {
            return "";
        }

        final CsvBindByName bindByNameAnnotation = beanField.getField().getDeclaredAnnotationsByType(CsvBindByName.class)[0];
        return bindByNameAnnotation.column();
    }
}

// ZuluApplication.java
public class ZuluApplication {
    public static void main (String[] args) {
        // 前半省略
        StringWriter writer = new StringWriter();
        CustomMappingStrategy<MyBean> mappingStrategy = new CustomMappingStrategy<>();
        mappingStrategy.setType(MyBean.class);
        StatefulBeanToCsv<MyBean> beanToCsv = new StatefulBeanToCsvBuilder<MyBean>(writer).withMappingStrategy(mappingStrategy).build();
        beanToCsv.write(beans);
        writer.close();

        System.out.print(writer.toString());
    }
}
```
