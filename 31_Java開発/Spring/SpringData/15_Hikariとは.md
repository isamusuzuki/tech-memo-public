# Hikari とは

作成日 2025/02/18

参照書籍 => P.627 "13.3.4 コネクションプールライブラリの変更" from 「Spring 徹底入門 第 2 版」

> Spring Boot では DataSource を定義する必要はなく、自動で生成されます。コネクションプーリングの仕組みも自動で決まり、以下のライブラリのうちクラスパス上にあるものが使用されます。優先順位は以下の順番どおりです。
>
> - HikariCP
> - Tomcat JDBC
> - Commons DBCP2
> - Oracle UDP
>
> spring-boot-starter-jdbc や spring-boot-starter-data-jpa の依存関係を pom.xml に追加した場合はデフォルトで HikariCP の依存関係も追加されます。したがって HikariCP は設定なしで利用できます。
