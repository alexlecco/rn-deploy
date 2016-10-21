# rn-deploy
step-by-step guide to deploy react native app on android (in linux)

# 1. generar el keystore y la key desde el root

```bash
$ keytool -genkey -v -keystore my-release-key.keystore \
    -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000
```


# 2. mover el keystore a android/app/


# 3. en ~/gradle.properties, agregar

```bash
MYAPP_RELEASE_STORE_FILE=my-release-key.keystore
MYAPP_RELEASE_KEY_ALIAS=my-key-alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```
- cambiando los asteriscos por la password


# 4. en android/app/build.gradle, agregar

```
...
android {
  ...
  defaultConfig { ... }
  signingConfigs {
    release {
      storeFile file(MYAPP_RELEASE_STORE_FILE)
      storePassword MYAPP_RELEASE_STORE_PASSWORD
      keyAlias MYAPP_RELEASE_KEY_ALIAS
      keyPassword MYAPP_RELEASE_KEY_PASSWORD
    }
  }
  buildTypes {
    release {
      ...
      signingConfig signingConfigs.release
    }
  }
}
```

# 5. iniciar el RN packager desde el root

```bash
$ npm start
```


# 6. correr desde root

```bash
$ mkdir -p android/app/src/main/assets
$ curl \
"localhost:8081/index.android.bundle?platform=android&dev=false&minify=true" \
    -o "android/app/src/main/assets/index.android.bundle"
$ cd android  && ./gradlew assembleRelease
```


# 7. matar el RN packager y correr desde android/

```bash
./gradlew installRelease
```
