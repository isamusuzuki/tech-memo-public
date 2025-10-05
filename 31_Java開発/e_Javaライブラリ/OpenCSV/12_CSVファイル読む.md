# CSV ファイルを読む

作成日 2025/01/23

## 1. CSV ファイルを用意する

- 文字コードは UTF-8 BOM なし
- ヘッダーは Bean クラスのフィールドと同じにする

## 2. CSV ファイルを１行づつ読む

```java
Path csvPath = Path.of(Main.class.getClassLoader().getResource("新入生ゼミナール.csv").toURI());

FileReader fileReader = new FileReader(csvPath.toString(), StandardCharsets.UTF_8);
CSVReader csvReader = new CSVReader(fileReader);

String[] line;
while ((line = csvReader.readNext()) != null) {
    System.out.print(line.length + ":");
    for (String value : line) {
        System.out.print(value + " ");
    }
    System.out.println();
}
csvReader.close();
```

## 3. Bean クラスを用意する

- `@Builder`アノテーションを使わない
- 引数なしのコンストラクターを用意する

```java
package com.example.readcsv;

import lombok.Data;

@Data
public class DataBean {
    public DataBean () {}

    private String department;
    private String instructor;
    private String lectureName;
    private String timeTable;
    private int studentCount;
    private String code8;
    private String shimei;
    private String grade;
    private String note;
}
```

## 4. CSV ファイルを読んで Bean クラスのリストに変換する

```java
Path csvPath = Path.of(Main.class.getClassLoader().getResource("新入生ゼミナール.csv").toURI());
log.debug(csvPath.toString());

FileReader fileReader = new FileReader(csvPath.toString(), StandardCharsets.UTF_8);
CSVReader csvReader = new CSVReader(fileReader);

CsvToBean<DataBean> csvToBean = new CsvToBeanBuilder<DataBean>(csvReader).withType(DataBean.class).build();
List<DataBean> beanList = csvToBean.parse();

int i = 1;
for (DataBean bean : beanList) {
    log.info(String.format("%s: %s, %s, %s", i,
        bean.getCode8(), bean.getShimei(), bean.getGrade()));
    i++;
}
```
