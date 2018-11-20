---
title: 服务端_SPRING_BOOT_KOTLIN_MYBATIS_COMMON_SQL_PROVIDER
date: 2018-11-19 18:29:49
categories: [技术]
tags: [服务端,SPRING-BOOT]
---

### 公共接口
发现书写 mapper对数据库进行增删改查 以及 controller 都是重复性操作，才有了本编文章，主要是利用反射自动生成 SQL 增删改查语句

### 代码
```
import org.apache.ibatis.annotations.DeleteProvider
import org.apache.ibatis.annotations.InsertProvider
import org.apache.ibatis.annotations.Options
import org.apache.ibatis.annotations.UpdateProvider

interface HKBaseMapper<T> {
    @Options(useGeneratedKeys = true)
    @InsertProvider(type = HKSqlProvider::class, method = "insert")
    fun insert(tableModel: T): Int

    @DeleteProvider(type = HKSqlProvider::class, method = "delete")
    fun delete(tableModel: T): Int

    @UpdateProvider(type = HKSqlProvider::class, method = "update")
    fun update(tableModel: T): Int

    @SelecProvider(type = HKSqlProvider::class, method = "select")
    fun select(tableModel: T): MutableList<T>
}

```
```
import org.apache.ibatis.annotations.Mapper

@Mapper
interface AdMapper : HKBaseMapper<AdModel>


```
```
import org.apache.logging.log4j.LogManager
import org.apache.logging.log4j.Logger

@Suppress("unused")
class HKSqlProvider {
    private val logger: Logger = LogManager.getLogger(UserController::class.java.name)

    fun insert(tableModel: Any?): String {
        val tableName = tableModel?.javaClass?.simpleName?.replace("Model", "")?.toLowerUnderScoreFromUpperCamel()
        val modelFields = HKReflectUtil.getFields(tableModel?.javaClass)
        val valueNameList = ArrayList<String>()
        val sqlString = StringBuilder("insert into $tableName (")
        modelFields.forEach { field -> HKReflectUtil.getValue(field, tableModel)?.let { valueNameList.add(field.name) } }
        valueNameList.forEachIndexed { index, valueName -> sqlString.append(valueName).append(if (index != valueNameList.size - 1) "," else ") values(") }
        valueNameList.forEachIndexed { index, valueName -> sqlString.append("#{$valueName}").append(if (index != valueNameList.size - 1) "," else ")") }

        logger.warn("""
            ---------------------------------
            mybatis-buildSql-insert -->
            tableModel: $tableModel
                   sql: $sqlString
            ---------------------------------
        """)
        return sqlString.toString()
    }

    fun update(tableModel: Any?): String {
        val tableName = tableModel?.javaClass?.simpleName?.replace("Model", "")?.toLowerUnderScoreFromUpperCamel()
        val modelFields = HKReflectUtil.getFields(tableModel?.javaClass)
        val valueNameList = ArrayList<String>()
        val sqlString = StringBuilder("update $tableName set ")
        modelFields.forEach { field -> HKReflectUtil.getValue(field, tableModel)?.let { valueNameList.add(field.name) } }
        valueNameList.forEachIndexed { index, valueName -> sqlString.append("$valueName=#{$valueName}").append(if (index != valueNameList.size - 1) "," else "") }

        logger.warn("""
            ---------------------------------
            mybatis-buildSql-update -->
            tableModel: $tableModel
                   sql: $sqlString
            ---------------------------------
        """)
        return sqlString.toString()
    }

    fun delete(tableModel: Any?): String {
        val tableName = tableModel?.javaClass?.simpleName?.replace("Model", "")?.toLowerUnderScoreFromUpperCamel()
        val modelFields = HKReflectUtil.getFields(tableModel?.javaClass)
        val valueNameList = ArrayList<String>()
        val sqlString = StringBuilder("delete from $tableName where ")
        modelFields.forEach { field -> HKReflectUtil.getValue(field, tableModel)?.let { valueNameList.add(field.name) } }
        valueNameList.forEachIndexed { index, valueName -> sqlString.append("$valueName=#{$valueName}").append(if (index != valueNameList.size - 1) " and " else "") }

        logger.warn("""
            ---------------------------------
            mybatis-buildSql-delete -->
            tableModel: $tableModel
                   sql: $sqlString
            ---------------------------------
        """)
        return sqlString.toString()
    }

    fun select(tableModel: Any?): String {
        val tableName = tableModel?.javaClass?.simpleName?.replace("Model", "")?.toLowerUnderScoreFromUpperCamel()
        val modelFields = HKReflectUtil.getFields(tableModel?.javaClass)
        val valueNameList = ArrayList<String>()
        val sqlString = StringBuilder("select * from $tableName where ")
        modelFields.forEach { field -> HKReflectUtil.getValue(field, tableModel)?.let { valueNameList.add(field.name) } }
        valueNameList.forEachIndexed { index, valueName -> sqlString.append("$valueName=#{$valueName}").append(if (index != valueNameList.size - 1) " and " else "") }

        logger.warn("""
            ---------------------------------
            mybatis-buildSql-select -->
            tableModel: $tableModel
                   sql: $sqlString
            ---------------------------------
        """)
        return sqlString.toString()
    }
}

```

```
import io.swagger.annotations.ApiOperation
import org.springframework.web.bind.annotation.PostMapping
import org.springframework.web.bind.annotation.RequestBody
import org.springframework.web.bind.annotation.RequestMapping
import org.springframework.web.bind.annotation.RestController


@RestController
@RequestMapping("/ad")
class AdController(val mapper: AdMapper) {

    @ApiOperation("增加", notes = "返回 ID")
    @PostMapping("/insert")
    fun insert(@RequestBody request: HKRequest<AdModel>): HKResponse<HKIdData> {
        if (request.data == null) return HKCode.ERROR_PARAMS.response()
        return HKResponse(HKIdData(mapper.insert(request.data!!)))
    }

    @ApiOperation("删除", notes = "返回 受影响的行数")
    @PostMapping("/delete")
    fun delete(@RequestBody request: HKRequest<AdModel>): HKResponse<HKColumnData> {
        if (request.data == null) return HKCode.ERROR_PARAMS.response(HKColumnData(-1))
        return HKResponse(HKColumnData(mapper.delete(request.data!!)))
    }

    @ApiOperation("修改", notes = "返回 受影响的行数")
    @PostMapping("/update")
    fun update(@RequestBody request: HKRequest<AdModel>): HKResponse<HKColumnData> {
        if (request.data == null) return HKCode.ERROR_PARAMS.response()
        return HKResponse(HKColumnData(mapper.update(request.data!!)))
    }

    @ApiOperation("查询")
    @PostMapping("/select")
    fun select(@RequestBody request: HKRequest<AdModel>): HKResponse<List<AdModel>> {
        if (request.data == null || request.data == null) return HKCode.ERROR_PARAMS.response()
        return HKResponse(mapper.select(request.data!!))
    }

}

```

### 反射工具类
```
import java.lang.reflect.Field
import java.lang.reflect.InvocationTargetException
import kotlin.reflect.KClass
import kotlin.reflect.full.companionObject
import kotlin.reflect.full.companionObjectInstance
import kotlin.reflect.full.declaredFunctions

@Suppress("unused")
object HKReflectUtil {
    private val TAG = "[reflect]"

    /**
     * 根据方法的名字调用方法，适合 object 定义的单例静态方法
     */
    @Throws(RuntimeException::class, IllegalAccessException::class, IllegalArgumentException::class, InvocationTargetException::class, NullPointerException::class, ExceptionInInitializerError::class)
    fun invoke(clazz: KClass<*>?, methodName: String?, vararg params: Any?): Any? {
        if (clazz == null || methodName.isNullOrBlank()) throw RuntimeException("${TAG} clazz:$clazz or methodName:$methodName is null")
        val methods = clazz.java.kotlin.companionObject?.declaredFunctions?.filter { it.name == methodName && it.parameters.size - 1 == params.size }
        if (methods?.size ?: 0 <= 0) throw RuntimeException("[callNativeMethod] the invoked method dose not exist :$methodName")
        return methods!![0].call(clazz.companionObjectInstance, *params)
    }

    fun getFields(objectClass: Class<*>?): MutableList<Field> {
        var fieldList: MutableList<Field>? = null
        try {
            fieldList = objectClass?.declaredFields?.toMutableList()
            objectClass?.superclass?.declaredFields.let { if (it != null) fieldList?.addAll(it) }
        } catch (ignore: Exception) {
        }
        return fieldList ?: arrayListOf()
    }

    fun getValue(field: Field?, fromObject: Any?): Any? = try {
        field?.isAccessible = true
        field?.get(fromObject)
    } catch (ignore: Exception) {
        null
    }
}

```

### 预览
![swagger2](http://upload-images.jianshu.io/upload_images/2062311-b06f057d9274c144.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
