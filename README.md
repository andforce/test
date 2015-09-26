开发中,Apk签名是一件不大也不小的事,如果你做过`微博`相关的开发,你可能遇到如下情景:

> 用Eclipse或者AS直接点run生成的apk跑在手机上根本不能授权成功,每次需要导出release版本,很烦.

如果你基于系统源码开发或者二次开发系统预置App,你的流程可能是:
> mm 或者 mmm 编译出apk,之后再push 到/system/app/ 下,或者用adb install -r 安装到手机.
> 否根本无法安装,因为签名不一样.

因此我搜集整理了如下内容,解决了以上两个难题,任何app都可以直接再IDE中直接run,并debug.

# 日常开发
---

## 修改Eclips或者Android Studio的默认签名文件

> 通常情况下,我们debug版本的apk和正式release的apk签名是不一样.特别开发一些有签名限制
> 的App的时候,就造成了调试上的不方便.

### 如何修改?
+ Eclipse
> Eclips -> Window -> Preference -> Androi -> Buil.
> 在 `Custom debug keystore` 指定自己的 `debug.keystore` 就可以了.(注意签名文件的名字
不必是`debug.keystore`).

+ Androi的 Studio
``` script
android {
    signingConfigs {
        debug {
            storeFile file("debug.keystore")
        }
    }
}
```

### 疑问?
> 如何创建自己的`debug.keystore`?

``` script
keytool -genkey -v -keystore debug.keystore -alias androiddebugkey -storepass android -keypass android -keyalg RSA -validity 14000
// 这样你就创建了一个 debug.keystore
```

### 如何把现有签名文件,转换成Eclipse或Android Studio可用的`debug.keystore`?

> 假如你现在的签名文件是:my.keystore.

+ 修改`my.keystore` 的密码为:`android`

``` script
keytool -storepasswd -keystore my.keystore
```
+ 修改`my.keystore` 的`alias`为:`androiddebugkey`

``` script
keytool -changealias -keystore my.keystore -alias my_name -destalias androiddebugkey
```
+ 修改`alias` 的密码为:`android`

``` script
keytool -keypasswd -keystore my.keystore -alias androiddebugkey
```
执行完上面三步,你的`debug.keystore` 就创建完成了,替换上去就行.

# 系统源码开发
---

## 源码签名文件在哪?
> SOURCE_CODE/build/target/product/security/

``` script
$:ls
media.pk8       platform.pk8       README    shared.pk8       signapk.jar  testkey.x509.pem
media.x509.pem  platform.x509.pem  Root.zip  shared.x509.pem  testkey.pk8  update.zip
```

## 但如果你在一家手机厂商工作,厂商一般不会直接用而是会进行定制:
> SOURCE_CODE/vendor/CUSTOM_VENTER_NAME/security/

``` script
$: ls
media.pk8       platform.pk8       releasekey.pk8       shared.pk8       testkey.pk8
media.x509.pem  platform.x509.pem  releasekey.x509.pem  shared.x509.pem  testkey.x509.pem
```

## 如何用系统签名文件签名apk?
``` script
java -jar platform.x509.pem platform.pk8 UnSign.apk Signed.apk
```

## zipalign优化apk
``` script
zipalign -v 4 MyDemo_signed.apk MyDemo_new.apk
```

## 是否用zipalign优化过
``` script
zipalign -c -v 4 MyDemo.apk
```

## 如何把`platform.x509.pem platform.pk8` 转成`debug.keystore`?
+ 生成shared.priv.pem

``` script
openssl pkcs8 -in platform.pk8 -inform DER -outform PEM -out shared.priv.pem -nocrypt
```
+ 生成pkcs12

``` script
openssl pkcs12 -export -in platform.x509.pem -inkey shared.priv.pem -out shared.pk12 -name androiddebugkey
// 如果生成的`debug.keystore`要用在Eclipse中使用,请输入密码:`android`,否则请输入自己的签名即可.
```

+ 生成`debug.keystore`

``` script
keytool -importkeystore -deststorepass android -destkeypass android -destkeystore debug.keystore -srckeystore shared.pk12 -srcstoretype PKCS12 -srcstorepass android -alias androiddebugkey
```
