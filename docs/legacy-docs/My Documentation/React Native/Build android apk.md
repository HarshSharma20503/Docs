1. Generate a keystore using the following command `keytool -genkeypair -v -storetype PKCS12 -keystore my-upload-key.keystore -alias my-key-alias -keyalg RSA -keysize 2048 -validity 10000`
2. follow the instruction given here - https://reactnative.dev/docs/signed-apk-android till Adding signing config to your app's Gradle config
3. Now go to android folder and run the following command `./gradlew assembleRelease`.
4. You can find the apk in _android/app/build/outputs/apk/app-release.apk