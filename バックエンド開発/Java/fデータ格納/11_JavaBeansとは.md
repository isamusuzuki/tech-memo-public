# JavaBeans 概要

作成日 2025/01/24

参照サイト => [JavaBeans とは？Beans を支える 3 つのルールと考え方](https://www.bold.ne.jp/engineer-club/java-beans)

## JavaBeans とは

データを納める倉庫のような役割のクラス

## JavaBeans 3 つのルール

1. プロパティへの getter/setter メソッドを持つ
1. 引数のないコンストラクタを持つ
1. java.io.Serializable を実装している

## 利点1: 具体的なクラスを知らなくてもプロパティが使える

```java
String property = "name";

Object bean = new JavaBeansSample();
Class<?> clazz = bean.getClass();

String setterName = "set" + property.substring(0, 1).toUpperCase()
    + property.substring(1);

Method setter = clazz.getDeclaredMethod(setterName, String.class);
setter.invoke(bean, "TEST");

String getterName = "get" + property.substring(0, 1).toUpperCase()
    + property.substring(1);

Method getter = clazz.getDeclaredMethod(setterName);
String ret = (String)getter.invoke(bean);
System.out.println(ret); // => TEST
```

## 利点2: 具体的なクラスを知らなくてもインスタンスの生成を生成できる

```java
Class<?> clazz = Class.forName("JavaBeansSample");
Object obj = clazz.getDeclearedConstructor().newInstance();
JavaBeanSample bean = (JavaBeansSample) obj;
bean.setName("TEST");
System.out.println(bean.getName()); // => TEST
```
