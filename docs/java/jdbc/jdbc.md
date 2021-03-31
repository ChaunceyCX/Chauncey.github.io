# JDBC数据库链接池

## JDBC链接4种方式

四种类型的JDBC驱动程序：
- 1: JDBC-ODBC BRIDGE 　　在JDBC出现的初期，JDBC-ODBC桥显然是非常有实用意义的，通过JDBC-ODBC桥，开发人员可以使用JDBC来存取ODBC数据源。不足的是，他需要在客户端安装ODBC驱动程序，换句话说，必须安装MICROSOFT WINDOWS的某个版本。使用这一类型你需要牺牲JDBC的平***立性。另外，ODBC驱动程序还需要具有客户端的控制权限。
- 2: JDBC-NATIVE DRIVER BRIDGE 　　JDBC本地驱动程序桥提供了一种JDBC接口，它建立在本地数据库驱动程序的顶层，而不需要使用ODBC。 JDBC驱动程序将对数据库的API从标准的JDBC调用转换为本地调用。使用此类型需要牺牲JDBC的平***立性，还要求在客户端安装一些本地代码。
- 3: JDBC-NETWORK BRIDGE 　　JDBC网络桥驱动程序不再需要客户端数据库驱动程序。它使用网络上的中间服务器来存取数据库。这种应用使得以下技术的实现有了可能，这些技术包括负载均衡、连接缓冲池和数据缓存等。由于第3种类型往往只需要相对更少的下载时间，具有平***立性，而且不需要在客户端安装并取得控制权，所以很适合于INTERNET上的应用。
- 4: PURE JAVA DRIVER 　　第4种类型通过使用一个纯JAVA数据库驱动程序来执行数据库的直接访问。此类型实际上在客户端实现了2层结构。要在N-层结构中应用，一个更好的做法是编写一个EJB，让它包含存取代码并提供一个对客户端具有数据库独立性的服务。 　　WEBLOGIC服务器为一些通常的数据库提供了JDBC驱动程序，包括ORACLE, SYBASE, MICROSOFT SQL SERVER以及INFORMIX。它也带有一种JDBC驱动程序用于CLOUDSCAPE，这是一种纯JAVA的DBMS，WEBLOGIC服务器中带有该数据库的评估版本。