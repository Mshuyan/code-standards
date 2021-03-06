# 代码规范

## 基本原则

+ 必须使用阿里规约插件

+ 每个文件右上角，必须显示如下图标

  ![image-20191216175724382](CodeStandards.assets/image-20191216175724382.png) 

## 错误

### 原则

坚决杜绝代码中出现错误

### 可忽略错误

> 随时补充

## 警告

### 原则

+ 避免代码中存在警告
+ 对于无法解决得警告，需要使用注释标明原因
+ [可压制警告](#可压制警告)中提到得警告，必须使用`@SuppressWarnings`注解进行压制，并且使用注释标明压制原因，其他警告不允许压制

### 可忽略警告

> 随时补充

+ **代码不能超过80行**

  尽量将可抽离的方法进行抽离，如果无法缩减到80行以内，可忽略

+ **未使用**

  对于预留的类、方法、属性，报未使用警告时，可以采取压制

## 参数校验

+ 注解校验

  对于接口接收的请求参数，如果前端传值错误会导致后端出错，则必须使用参数校验注解进行校验（参数校验使用参见：https://github.com/Mshuyan/validation）

+ 手动校验

  需要进行逻辑判断才能知道参数的限定范围时，需要在代码中自己对参数进行校验

## 单元测试

+ 编写代码时，尽量将每个方法都以功能模块方式编写
+ 推荐使用单元测试对方法进行测试，及早发现逻辑错误

## 约束

### MyBatisPlus

#### Mapper方法命名

`mapper`接口中定义的方法不允许与`mybatisplus`中提供的查询方法同名，`mybtis`是通过代理调用`sql`的，同名方法会导致调用`sql`出错

#### 分页

所有的分页查询，必须使用`PageQuery`对象传递分页参数，不可以使用传递查询条件的`query`继承`IPage`

#### QueryWrapper

+ 使用`QueryWrapper`时，尽量使用`lambda`表达式，避免使用魔法值

  ```java
  QueryWrapper<GsSysUserRole> userRoleQueryWrapper = new QueryWrapper<>();
  userRoleQueryWrapper.lambda().eq(GsSysUserRole::getPartyId,Dict111PartiesEnum.SELF.getCode()).eq(GsSysUserRole::getRoleCode, RoleCodeConstants.QC_ML_DIANWEN);
  List<GsSysUserRole> userRoles = gsSysUserRoleMapper.selectList(userRoleQueryWrapper);
  ```

### 时间类型

时间类型不使用`Date`、`LocalDateTime`，这两个时间类型不包含时区信息，需要使用`Instant`时间戳类型

### 返回响应

+ 返回响应必须使用`ApiResult`返回
+ service层不可以直接返回`ApiResult`，`ApiResult`需要在`Controlelr`层统一进行封装

### 数据库

+ 不允许存在超过3个表以上的表关联查询，再java代码中分步实现

## 数据库

+ 每张表必须有主键，并且主键字段仅作为唯一标识，没有其他含义
+ 所有字段不能使用`NULL`值，并且必须有默认值
+ 尽量遵守`数据库设计三范式`
+ 创建表字段时，如果该字段对应了字典表中的`code`，则这个字段以 `_value `结尾，需要返回该字段对应的字典值中的name时，字段名就是去掉 `_value`时的字段名

## swagger

### 参数说明

+ 所有接口参数必须使用`@ApiImplicitParam`和`@ApiModelProperty`标记参数的具体含义，不要词不达意

+ 对于必传、允许值等特性，需要使用注解得对应属性进行标记

+ 对于公用得实体类，需要在每个接口得`@ApiOperation`注解得`note`属性中明确指明该接口需要使用哪些请求参数

  ![image-20200407175211763](CodeStandards.assets/image-20200407175211763.png) 