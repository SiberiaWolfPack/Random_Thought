### BasicDao
1. 问题
* 持久化框架和数据库连接池简化了jdbc开发，但还有不足：
	+ SQL语句是固定的，不能通过参数传入，通用性不好。
	+ 对于select操作，返回类型不确定，需要使用泛型。
	+ 将来的表很多，业务需求复杂，不可能只靠一个Java类完成。可能会有很多专门的类，比如ActorDAO，GoodsDAO...专门处理每个表。
2. BasicDAO
* BasicDAO就是把各个DAO共有的部分放到这里来，类似于一个工具类（高内聚低耦合）。
* 这样就形成了表，实例类，操作DAO，BasicDAO，应用类五位一体，这就是所谓数据层。
* DAO：数据访问对象。