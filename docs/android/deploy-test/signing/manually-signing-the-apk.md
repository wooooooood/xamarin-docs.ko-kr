---
title: 수동으로 APK 서명
ms.prod: xamarin
ms.assetid: 08549E1C-7F04-4D20-9E7A-794B9D09FD12
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d20ec990253ff86e7b426baad8da5a919a91ef6c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69525015"
---
# <a name="manually-signing-the-apk"></a>수동으로 APK 서명


애플리케이션이 릴리스용으로 빌드된 후 배포 전에 APK에 서명해야 Android 디바이스에서 실행할 수 있습니다. 이 프로세스는 일반적으로 IDE로 처리되지만 명령줄에서 수동으로 APK에 서명하기 위해 필요한 경우가 있습니다. APK 서명과 관련된 단계는 다음과 같습니다.

1. **개인 키 만들기** &ndash; 이 단계는 한 번만 수행해야 합니다. 개인 키는 APK에 디지털 방식으로 서명하는 데 필요합니다.
    개인 키가 준비되면 이후 릴리스 빌드에서 이 단계를 건너뛸 수 있습니다.

2. **APK Zipalign**&ndash;*Zipalign*은 애플리케이션에서 수행되는 최적화 프로세스입니다. 런타임에 Android가 APK와 보다 효율적으로 상호 작용할 수 있게 해줍니다. Xamarin.Android는 런타임에 검사를 수행하고 APK가 Zipalign 되지 않은 경우 애플리케이션 실행을 허용하지 않습니다.

3. **APK 서명** &ndash; 이 단계에서는 Android SDK의 **apksigner** 유틸리티를 사용하고, 이전 단계에서 생성된 개인 키로 APK에 서명합니다. v24.0.3 이전 버전의 오래된 Android SDK 빌드 도구를 사용하여 개발된 애플리케이션은 JDK의 **jarsigner** 앱을 사용합니다. 이러한 도구는 아래에서 자세히 설명합니다. 

단계의 순서는 중요하며 APK에 서명하는 데 사용되는 도구에 따라 달라집니다. **apksigner**를 사용할 경우 먼저 애플리케이션 **zipalign**을 수행한 후 **apksigner**로 서명하는 것이 중요합니다.  **jarsigner**를 사용하여 APK에 서명해야 하며, 먼저 APK에 서명한 후 **zipalign**을 실행하는 것이 중요합니다. 



## <a name="prerequisites"></a>전제 조건

이 가이드에서는 Android SDK 빌드 도구 v24.0.3 이상 버전의 **apksigner**를 사용하는 방법을 중점적으로 설명합니다. APK가 이미 빌드되었다고 가정합니다.

이전 버전의 Android SDK 빌드 도구를 사용하여 빌드된 애플리케이션은 아래의 [jarsigner로 APK 서명](#Sign_the_APK_with_jarsigner)에 설명된 대로 **jarsigner**를 사용해야 합니다.



## <a name="create-a-private-keystore"></a>개인 키 저장소 만들기

*키 저장소*는 Java SDK의 [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html) 프로그램을 사용하여 생성되는 보안 인증서 데이터베이스입니다. Android는 디지털 방식으로 서명되지 않은 애플리케이션을 실행하지 않기 때문에 키 저장소는 Xamarin.Android 애플리케이션을 게시하는 데 중요합니다.

개발 중에 Xamarin.Android는 디버그 키 스토리지를 사용하여 애플리케이션에 서명하므로 애플리케이션을 에뮬레이터나 디버그 가능한 애플리케이션을 사용하도록 구성된 디바이스에 바로 배포할 수 있습니다.
하지만 이러한 키 저장소는 애플리케이션 배포를 위한 유효한 키 저장소로 인식되지 않습니다.

이러한 이유로 개인 키 저장소를 만들고 애플리케이션 서명에 사용해야 합니다. 이 단계는 한 번만 수행하면 됩니다. 업데이트를 게시할 때 동일한 키가 사용되며, 다른 애플리케이션에 서명하는 데도 동일한 키를 사용할 수 있기 때문입니다.

이 키 저장소를 보호하는 것은 중요합니다. 이 키 저장소를 분실할 경우 Google Play에서 애플리케이션에 업데이트를 게시할 수 없기 때문입니다.
키 저장소 분실로 인해 발생하는 문제를 해결하는 유일한 방법은 새 키 저장소를 만들고, 새 키로 APK에 다시 서명한 후, 새 애플리케이션을 제출하는 것입니다. 그런 다음, 기존 애플리케이션을 Google Play에서 제거해야 합니다. 마찬가지로, 이 새 키 스토리지가 손상되거나 공개적으로 배포될 경우 애플리케이션의 비공식 또는 악성 버전이 배포될 가능성이 있습니다.



### <a name="create-a-new-keystore"></a>새 키 저장소 만들기

새 키 저장소를 만들려면 Java SDK의 명령줄 도구 [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)이 필요합니다. 다음 코드 조각은 **keytool**을 사용하는 방법의 예입니다(`<my-filename>`은 키 저장소의 파일 이름으로, `<key-name>`은 키 저장소 내 키 이름으로 바꾸세요).

```shell
$ keytool -genkeypair -v -keystore <filename>.keystore -alias <key-name> -keyalg RSA \
          -keysize 2048 -validity 10000
```

**keytool**이 가장 먼저 요청하는 것은 키 저장소의 암호입니다. 그런 다음, 키 생성에 도움이 되는 몇 가지 정보를 요청합니다. 다음 코드 조각은 `xample.keystore` 파일에 저장될 `publishingdoc`라는 새 키를 만드는 예입니다.

```shell
$ keytool -genkeypair -v -keystore xample.keystore -alias publishingdoc -keyalg RSA -keysize 2048 -validity 10000
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  Ham Chimpanze
What is the name of your organizational unit?
  [Unknown]:  NASA
What is the name of your organization?
  [Unknown]:  NASA
What is the name of your City or Locality?
  [Unknown]:  Cape Canaveral
What is the name of your State or Province?
  [Unknown]:  Florida
What is the two-letter country code for this unit?
  [Unknown]:  US
Is CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US correct?
  [no]:  yes

Generating 2,048 bit RSA key pair and self-signed certificate (SHA1withRSA) with a validity of 10,000 days
        for: CN=Ham Chimpanze, OU=NASA, O=NASA, L=Cape Canaveral, ST=Florida, C=US
Enter key password for <publishingdoc>
        (RETURN if same as keystore password):
Re-enter new password:
[Storing xample.keystore]
```

키 저장소에 저장된 키를 나열하려면 **keytool**과 &ndash; `list` 옵션을 사용하세요.

```shell
$ keytool -list -keystore xample.keystore
```


## <a name="zipalign-the-apk"></a>APK Zipalign

**apksigner**를 사용하여 APK에 서명하기 전에 먼저 Android SDK의 **zipalign** 도구를 사용하여 파일을 최적화하는 것이 중요합니다. **zipalign**은 4바이트 경계를 따라 APK의 리소스를 재구성합니다. 이러한 정렬 방법은 Android가 APK에서 리소스를 빠르게 로드하여 애플리케이션의 성능을 높이고 메모리 사용량을 줄일 수 있게 해줍니다. Xamarin.Android는 런타임 검사를 수행하여 APK가 zipalign 되었는지 확인합니다. APK가 zipalign 되지 않은 경우 애플리케이션이 실행되지 않습니다.

다음 명령은 서명된 APK를 사용하고 배포 준비가 된 **helloworld.apk**라는 서명되고 zipalign 된 APK를 생성합니다.

```shell
$ zipalign -f -v 4 mono.samples.helloworld-unsigned.apk helloworld.apk
```


## <a name="sign-the-apk"></a>APK 서명

APK를 zipalign 한 후에는 키 저장소를 사용하여 서명해야 합니다. 이는 해당 버전의 SDK 빌드 도구에서 **build-tools** 디렉터리에 있는 **apksigner** 도구를 사용하여 수행합니다.  예를 들어 Android SDK 빌드 도구 v25.0.3이 설치된 경우 디렉터리에 **apksigner**가 있을 수 있습니다.

```bash
$ ls $ANDROID_HOME/build-tools/25.0.3/apksigner
/Users/tom/android-sdk-macosx/build-tools/25.0.3/apksigner*
```

다음 코드 조각은 `PATH` 환경 변수로 **apksigner**에 액세스할 수 있다고 가정합니다. **xample.keystore** 파일에 포함된 키 별칭 `publishingdoc`를 사용하여 APK에 서명합니다.

```shell
$ apksigner sign --ks xample.keystore --ks-key-alias publishingdoc mono.samples.helloworld.apk
```

이 명령을 실행하면 **apksigner**에서 필요할 경우 키 저장소의 암호를 요청합니다.

**apksigner** 사용법에 대한 자세한 내용은 [Google의 설명서](https://developer.android.com/studio/command-line/apksigner.html)를 참조하세요.

> [!NOTE]
> [Google 문제 62696222](https://issuetracker.google.com/issues/62696222)에 따르면 Android SDK에 **apksigner**가 "없습니다". 이 문제를 해결하는 방법은 Android SDK 빌드 도구 v25.0.3을 설치하고 해당 버전의 **apksigner**를 사용하는 것입니다.  


<a name="Sign_the_APK_with_jarsigner" />

### <a name="sign-the-apk-with-jarsigner"></a>jarsigner를 사용하여 APK에 서명

> [!WARNING]
> 이 섹션은 **jarsigner** 유틸리티를 사용하여 APK에 서명해야 하는 경우에만 적용됩니다. 개발자는 **apksigner**를 사용하여 APK에 서명하는 것이 좋습니다.

이 기법을 사용하려면 Java SDK의 **[jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)** 명령을 사용하여 APK 파일에 서명해야 합니다.  **jarsigner** 도구는 Java SDK에서 제공합니다. 

다음은 **xample.keystore**라는 키 저장소 파일에 포함된 `publishingdoc` 키와 **jarsigner**를 사용하여 APK에 서명하는 방법을 보여줍니다.

```shell
$ jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore xample.keystore mono.samples.helloworld.apk publishingdoc
```

> [!NOTE]
> **jarsigner**를 사용하는 경우 APK에 _먼저_ 서명한 후 **zipalign**을 사용하는 것이 중요합니다.  



## <a name="related-links"></a>관련 링크

- [애플리케이션 서명](https://source.android.com/security/apksigning/)
- [Java JAR 서명](https://docs.oracle.com/javase/8/docs/technotes~/jar/jar.html#Signed_JAR_File)
- [jarsigner](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jarsigner.html)
- [keytool](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/keytool.html)
- [zipalign](https://developer.android.com/studio/command-line/zipalign.html)
- [빌드 도구 26.0.0 - apksigner는 어디에 있습니까?](https://issuetracker.google.com/issues/62696222)
