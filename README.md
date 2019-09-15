# MeKV
Key-value

MeKV 是 Android  Key-Value 管理框架，为了解决 Android 平台下各种繁琐的配置类代码

使用了 https://github.com/Tencent/MMKV 来存储数据,支持自定义存储数据

## 配置

```gradle
dependencies {
    implementation 'vip.ruoyun.mekv:core:1.0.0'
    annotationProcessor 'vip.ruoyun.mekv:mekv-compiler:1.0.0'
}
```

## 使用

初始化
```java
MeKV.init(new MMKVStrategy(this));
```

两种模式
- model 模式
- key-value 模式

### model 模式
支持的类型
- Parcelable

```java
@MeKV
public class User implements Parcelable {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(final String name) {
        this.name = name;
    }

    //...Parcelable code
}
```

自定义 key
```java
@MeKV(key = "CustomKeyUserKey")
public class User implements Parcelable {
    ...
}
```
生成的代码
![-w1093](https://github.com/bugyun/MeKV/blob/faa4cf16786f2a28963ff7a43fb674ae40535cd1/art/15685305431100.jpg?raw=true)


使用
```java
User user = UserMeKV.getUser();
if (user != null) {
    user.setName("");
    user.getName();
}
UserMeKV.saveUser(user);
```

### key-value 模式
支持的类型
- String
- float
- boolean
- double
- long
- int
- byte[]
- ```Set<String>```
- Parcelable

不支持包装类型

```java
@MeKV(isModel = false)
public class People {
    //不需要 get/set 方法

    @Ignore//不会自动生成属性代码
    private String name;

    private boolean isOld;

    private int age;

    private double doubleMoney;

    private long longMoney;

    private byte[] mBytes;

    private User user;//对象 必须 implements Parcelable

    private Set<String> setString;
}
```

使用
```java
PeopleMeKV.getName();
PeopleMeKV.getUser();
PeopleMeKV.saveUser(new User());
```

生成的代码
![-w1093](https://github.com/bugyun/MeKV/blob/faa4cf16786f2a28963ff7a43fb674ae40535cd1/art/15685307213473.jpg?raw=true)

## 高级玩法
#### 自定义 MMKV https://github.com/Tencent/MMKV/wiki/android_advance_cn
```java
MMKV mmkv = MMKV.defaultMMKV();
...code//MMKV高级特性
MeKV.init(new MMKVStrategy(mmkv));
```

#### 去除不需要的 so 文件
MMKV 默认提供各种版本的 so文件,可以进行配置

![-w1093](https://github.com/bugyun/MeKV/blob/613362cb49656866f957d09d2cadc4de97326bfa/art/15685301150300.jpg?raw=true)

```gradle
android {
    compileSdkVersion 28
    minSdkVersion 16 //MMKV 要求最低版本
    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a"
        }
    }
}
```







