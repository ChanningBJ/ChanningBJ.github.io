---
layout: post
category: Java
title: "MyBatis 使用annonation定义类型映射"
date: "2015-09-12"
---
介绍如何使用annonation的方式定义数据库字段到Java成员变量直接的映射关系，以及定义数据库表中的类型到Java类型的自定义转换

<!--more-->


关于如何配置MyBatis进行Java对象和Mysql表之间的映射可以参照 [这片文章](http://channingbj.github.io/java/2015/09/04/usemybits/)。

下面是一个最基础的映射关系配置：
    public interface SimpleMapper {
        @Select("select url from testdb.MY_BATIS_TEST;")
        Set<Record> selectRecords();
    }
在默认的配置中，mysqk表的字段会映射到同名字的java成员变量，这里我们看如何设置映射到其他的变量，以及如何进行类型转换

# 自定义映射关系

如果我们的DB中的一列名称是support_os_version类型是VARCHAR，但是在Java代码中对应的变量是 `String supportOSLevel`，可以进行如下定义：

    public interface SimpleMapper {
        @Select("select url from testdb.MY_BATIS_TEST;")
        @Results(value = {
                @Result(property = "support_os_version", column = "supportOSLevel"),
        })
        Set<Record> selectRecords();
    }


# 定义类型映射

如果我们的support_os_version列是一个使用逗号分割的版本号里表，
在Java中我们想映射到 `String[] supportOSLevel`, 可以通过定义 typeHandler 的方式进行映射。

首先定义一个从String到String[]的转换类：

    public class StringSplitHandler extends BaseTypeHandler<String[]> {

        private static final Joiner joiner = Joiner.on(",");

        @Override
        public void setNonNullParameter(PreparedStatement preparedStatement, int i, String[] strings, JdbcType jdbcType) throws SQLException {
            preparedStatement.setString(i, joiner.join(strings));
        }

        @Override
        public String[] getNullableResult(ResultSet resultSet, String columnName) throws SQLException {
            String data = resultSet.getString(columnName);
            if(StringUtils.isEmpty(data)) {
                return new String[0];
            } else {
                return data.split(",");
            }
        }

        @Override
        public String[] getNullableResult(ResultSet resultSet, int columnIndex) throws SQLException {
            String data = resultSet.getString(columnIndex);
            if(StringUtils.isEmpty(data)) {
                return new String[0];
            } else {
                return data.split(",");
            }
        }

        @Override
        public String[] getNullableResult(CallableStatement callableStatement, int columnIndex) throws SQLException {
            String data = callableStatement.getString(columnIndex);
            if(StringUtils.isEmpty(data)) {
                return new String[0];
            } else {
                return data.split(",");
            }
        }
    }


然后在映射关系的定义中指定这个类作为typeHandler

    public interface SimpleMapper {
        @Select("select url from testdb.MY_BATIS_TEST;")
        @Results(value = {
                @Result(property = "support_os_version", column = "supportOSLevel", typeHandler = StringSplitHandler.class),
        })
        Set<Record> selectRecords();
    }
