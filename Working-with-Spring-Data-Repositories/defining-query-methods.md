### 4.4 定义查询方法
repository 代理有两种方法去查询。一种是根据方法名或者自定义查询，可用的选项取决于实际的商店。然而,根据相应的策略来决定实际SQL的创建，让我们看看选择项吧。

#### 4.4.1. 查询查找策略

以下策略可供查询库基础设施来解决。您可以配置策略名称空间通过 ```query-lookup-strategy```属性的XML配置或通过```queryLookupStrategy```启用的属性```${store}```库注释的Java配置。一些策略可能不支持特定的数据存储。

- ```create``` 试图构建一个能找到查询的查询方法名称。 通常的做法是把给定的一组注明前缀的方法名和解析的方法。

- ```USE_DECLARED_QUERY```试图找到一个声明查询并将抛出一个异常情况。查询可以定义注释上。

- ```CREATE_IF_NOT_FOUND```(默认)结合```CREATE```和```USE_DECLARED_QUERY```。 看起来一个声明查询第一,如果没有声明查询发现,它创建一个定制的基于名称的查询方法。这是默认查找策略,因此如果你不使用任何显式配置。 它允许快速查询定义的方法名,还custom-tuning这些查询通过引入需要查询。

#### 2.4.2  创建查询

query builder机制内置为构建约束查询库的实体。 带前缀的机制```findXXBy```,```readAXXBy```,```queryXXBy```,```countXXBy```, ```getXXBy```自动解析的其余部分。进一步引入子句可以包含表达式等```Distinct```设置不同的条件创建查询。 然而,第一个```By```作为分隔符来表示实际的标准的开始。 在一个非常基础的查询,可以定义条件```And```或者```Or```。

例 13. 根据方法名创建查询

``` java
public interface PersonRepository extends Repository<User, Long> {

  List<Person> findByEmailAddressAndLastname(EmailAddress emailAddress, String lastname);

  // Enables the distinct flag for the query
  List<Person> findDistinctPeopleByLastnameOrFirstname(String lastname, String firstname);
  List<Person> findPeopleDistinctByLastnameOrFirstname(String lastname, String firstname);

  // Enabling ignoring case for an individual property
  List<Person> findByLastnameIgnoreCase(String lastname);
  // Enabling ignoring case for all suitable properties
  List<Person> findByLastnameAndFirstnameAllIgnoreCase(String lastname, String firstname);

  // Enabling static ORDER BY for a query
  List<Person> findByLastnameOrderByFirstnameAsc(String lastname);
  List<Person> findByLastnameOrderByFirstnameDesc(String lastname);
}
```

实际结果的解析方法取决于你的持久性存储创建查询。-然而,也有一些一般要注意的事情。

- 遍历表达式通常结合运算符连接。您可以把表达式```And```和```Or```,```Between```,```LessThan```(不超过) , ```GreaterThan```,```Like```等运算符，这些操作对不同的数据库可能有所不同，具体参考各参考文档

The method parser supports setting an IgnoreCase flag for individual properties (for example, findByLastnameIgnoreCase(…)) or for all properties of a type that support ignoring case (usually String instances, for example, findByLastnameAndFirstnameAllIgnoreCase(…)). Whether ignoring cases is supported may vary by store, so consult the relevant sections in the reference documentation for the store-specific query method.

- 方法解析支持设置```IgnoreCase```在属性上面（如，```findByLastnameIgnoreCase(…)```),或者支持查询所有属性忽略大小写（如，```findByLastnameAndFirstnameAllIgnoreCase(…)```), 忽略大小写支持所有的数据库，其它的查询参考相关文档

- 您可以应用静态排序通过附加一个```OrderBy```基准进行排序 子句的查询方法,引用属性和方向提供了一个排序(```asc```或 ```Desc```)。 创建一个支持动态排序的查询方法,明白了 特殊参数处理 。














