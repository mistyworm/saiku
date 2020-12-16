<a href="#readme"></a>

***

## Setup

### Build Instructions

```sh
mvn clean install -DskipTests

mvn clean clover2:setup test clover2:aggregate clover2:clover
```

### Update project version

To update the pom versions run:

```sh
mvn versions:set -DnewVersion=3.x.x
```

Then remove the backups with:

```sh
find . -name "*.versionsBackup" -type f -delete
```

## 增加其它数据库

### ClickHouse

新建一个pom工程，引用clickhouse-jdbc

```xml
        <dependency>
            <groupId>ru.yandex.clickhouse</groupId>
            <artifactId>clickhouse-jdbc</artifactId>
            <version>0.2.4</version>
        </dependency>
```

新建artifacts，将引用的类库一起打包为一个jar，然后将该jar放到打包后的saiku-server/target/dist/saiku-server/tomcat/webapps/saiku/WEB-INF/lib/中

添加clickhouse时有依赖冲突，删掉saiku/WEB-INF/lib/的httpcore-4.3-alpha1.jar和httpclient-4.2.5.jar就可以了。

## 启动

编译完后，在saiku-server/target/dist/saiku-server中会自动生成可执行的文件。

因为使用的是jdk 1.7里的一些组件，如果运行环境用的是jdk 1.8，需要修改start-saiku.sh

```sh
-Dorg.xml.sax.parser=\"com.sun.org.apache.xerces.internal.parsers.SAXParser\" -Djavax.xml.parsers.DocumentBuilderFactory=\"com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderFactoryImpl\" -Djavax.xml.parsers.SAXParserFactory=\"com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl\"
```

如果要支持IDE远程调试，可加上

```sh
-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8787
```

修改后的例子：

```sh
 export CATALINA_OPTS="-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8787 -Xms256m -Xmx768m -XX:MaxPermSize=256m -Dfile.encoding=UTF-8 -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true -Djava.awt.headless=true -Dorg.xml.sax.parser=\"com.sun.org.apache.xerces.internal.parsers.SAXParser\" -Djavax.xml.parsers.DocumentBuilderFactory=\"com.sun.org.apache.xerces.internal.jaxp.DocumentBuilderFactoryImpl\" -Djavax.xml.parsers.SAXParserFactory=\"com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl\""
```

现在可使用以下命令启动程序了。

```bash
./start-saiku.sh
```

默认使用http://localhost:8080/测试程序
