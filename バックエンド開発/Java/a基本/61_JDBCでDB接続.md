# JDBC で DB 接続

作成日 2025/02/12

## pom.xml

```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.7.5</version>
</dependency>
```

## Main.java

```java
package com.example;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class Main {
    public static void main(String[] args) {
        // 環境変数を取得する
        String hostname = System.getenv("POSTGRES_HOSTNAME");
        String db = System.getenv("POSTGRES_DB");
        String user = System.getenv("POSTGRES_USER");
        String password = System.getenv("POSTGRES_PASSWORD");

        String url = String.format(
            "jdbc:postgresql://%s:5432/%s",hostname, db);

        System.out.println("url: " + url);
        System.out.println("user: " + user);
        System.out.println("password: " + password);

        Connection conn = null;
        Statement stmt = null;
        ResultSet rset = null;

        try{
            //PostgreSQLへ接続
            conn = DriverManager.getConnection(url, user, password);

            //SELECT文の実行
            stmt = conn.createStatement();
            String sql = "SELECT name FROM myfriends;";
            rset = stmt.executeQuery(sql);

            //SELECT結果の受け取り
            while(rset.next()){
                String col = rset.getString(1);
                System.out.println(col);
            }
        } catch (SQLException e){
            e.printStackTrace();
        } finally {
            try {
                if(rset != null)rset.close();
                if(stmt != null)stmt.close();
                if(conn != null)conn.close();
            }
            catch (SQLException e){
                e.printStackTrace();
            }
        }
    }
}
```
