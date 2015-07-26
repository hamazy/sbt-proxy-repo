# Artfactory

## インストール

適当な inventory.txt を用意して、ansible を実行。

~~~
$ ansible-playbook -i inventory.txt artifactory.yml
~~~

## 起動．停止

`service` コマンドが使える。

~~~
$ sudo service artifactory start
~~~

## 管理画面へのアクセス

* http://localhost:8081/
* Log In から admin でログイン
    * 初期パスワードは password。User Profile のページで変更する。

リモートからアクセスする場合は localhost:8081 を適宜置き換える。

## ログ

`/var/opt/jfrog/artifactory/logs`下にある。
詳しくは http://www.jfrog.com/confluence/display/RTF/Artifactory+Log+Files 参照。

## リポジトリの設定

### リモートリポジトリの設定

Remote Repositories に下記を定義する。

* maven-central
  * https://repo1.maven.org/maven2/
  * maven-default
* ~~javanet-maven1~~ (Not Found)
  * http://download.java.net/maven/1/
  * maven-1-default -> maven-2-default
* sonatype-public
  * https://oss.sonatype.org/content/repositories/public
  * maven-default
* typesafe-releases
  * https://repo.typesafe.com/typesafe/releases
  * maven-default
* typesafe-ivy-releases
  * https://repo.typesafe.com/typesafe/ivy-releases/
  * ivy-default
* sbt-plugin-releases
  * https://repo.scala-sbt.org/scalasbt/sbt-plugin-releases
  * ivy-default
* jcenter
  * https://jcenter.bintray.com/
  * maven-default

### 仮想リポジトリの設定

Virtual Repositories に下記を定義する。

* remote-maven-repo
* remote-ivy-repo

remote-maven-repo には layout が maven のリモートリポジトリを、remote-ivy-repo には ivy のリモートリポジトリを追加する。

* remote-maven-repo
  * jcenter
  * maven-central
  * sonatype-public
  * typesafe-releases
* remote-ivy-repo
  * typesafe-ivy-releases
  * sbt-plugin-releases

## SBT の設定

ファイル `~/.sbt/repositories ` を作成し、以下の内容にする。

~~~
[repositories]
local
scalakb-ivy-proxy-releases: http://localhost:8081/artifactory/remote-ivy-repo/, [organization]/[module]/(scala_[scalaVersion]/)(sbt_[sbtVersion]/)[revision]/[type]s/[artifact](-[classifier]).[ext]
scalakb-maven-proxy-releases: http://localhost:8081/artifactory/remote-maven-repo
~~~

リモートからアクセスする場合は localhost:8081 を適宜置き換える。
