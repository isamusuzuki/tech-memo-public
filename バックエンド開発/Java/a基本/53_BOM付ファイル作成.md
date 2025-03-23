# Java で BOM 付きの UTF-8 のファイルを作成する

作成日 2025/01/14

## 参照サイト 1

[Java UTF-8 のテキストファイルを BOM 付きで作成する](https://www.javalife.jp/2018/02/26/post-441/)

```java
public class Utf8BomTest {

    public static void main(String[] args) {

        try(FileOutputStream fos = new FileOutputStream("c:\\test\\test.txt");
            OutputStreamWriter osw = new OutputStreamWriter(fos, "UTF-8");
            BufferedWriter bw = new BufferedWriter(osw)) {

            //BOM付与
            fos.write(0xef);
            fos.write(0xbb);
            fos.write(0xbf);

            bw.write("カキカキ...( ..);φメモメモ");
            bw.flush();

        }catch(Exception e) {
            e.printStackTrace();
        }
    }
}
```

> UTF-8 の BOM は、ファイルの先頭 3byte に持っています。
> よって、先頭 3byte に、「0xef」、「0xbb」、「0xbf」を埋め込んであげることで、UTF-8 BOM 付きファイルができあがります。
> 以降は、いつもどおり、普通に中身を書いてあげれば OK です。

## 参照サイト 2

[logback を使って Excel で読める CSV ファイルをログ出力する](https://qiita.com/hideaki-otsuka/items/165e58aab3af66719181)

```java
try {
    OutputStream outputStream = super.getOutputStream();
    // BOMを出力
    outputStream.write(0xef);
    outputStream.write(0xbb);
    outputStream.write(0xbf);
    // 固定文言（CSVヘッダ）を出力
    outputStream.write("操作日付,操作時刻,ユーザID,ユーザ名,操作内容,操作対象者ID・・・\n".getBytes());
    if (super.isImmediateFlush()) {
        outputStream.flush();
    }
} finally {
    lock.unlock();
}
```

## 自分で書いたサンプルコード

```java
@RestController
public class ZuluController {

    @GetMapping("/zulu/download")
    public void download(HttpServletResponse response) throws IOException {
        response.addHeader("Content-Disposition", "attachment; filename=\"data.txt\"");

        try (OutputStream os = response.getOutputStream()) {
            //BOM付与
            os.write(0xef);
            os.write(0xbb);
            os.write(0xbf);

            StringBuilder sb = new StringBuilder();
            sb.append("おはようございます。\n");
            sb.append("こんにちは。\n");
            sb.append("こんばんは。\n");

            os.write(sb.toString().getBytes(StandardCharsets.UTF_8));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
