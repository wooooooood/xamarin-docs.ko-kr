---
title: 키 저장소의 서명 찾기
ms.prod: xamarin
ms.assetid: 1b511fec-e6f6-453e-89c8-810aafb02b77
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 46b43e6689f751c4fac1e8668234fce7f953521e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="finding-your-keystores-signature"></a>키 저장소의 서명 찾기

Xamarin.Android 앱의 MD5 또는 SHA1 서명은 APK 서명에 사용된 **.keystore** 파일에 따라 달라집니다. 일반적으로 디버그 빌드에서는 릴리스 빌드와 다른 **.keystore** 파일을 사용합니다.

## <a name="for-debug--non-custom-signed-builds"></a>디버그/사용자 지정되지 않은 서명된 빌드용

Xamarin.Android는 동일한 **debug.keystore** 파일을 사용하여 모든 디버그 빌드에 서명합니다. 이 파일은 Xamarin.Android가 처음 설치될 때 생성됩니다. 아래 단계에는 기본 Xamarin.Android **debug.keystore** 파일의 MD5 또는 SHA1 서명을 찾는 과정이 자세히 설명되어 있습니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

앱에 서명하는 데 사용되는 Xamarin **debug.keystore** 파일을 찾습니다. 기본적으로 다음 위치에서 Xamarin.Android 응용 프로그램의 디버그 버전에 서명하는 데 사용되는 키 저장소를 찾을 수 있습니다.

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\debug.keystore**

JDK에서 `keytool.exe` 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 다음 위치에 있습니다.

**C:\\Program Files (x86)\\Java\\jdk*VERSION*\\bin\\keytool.exe**

**keytool.exe**를 포함하는 디렉터리를 `PATH` 환경 변수에 추가합니다.
**명령 프롬프트**를 열고 다음 명령을 사용하여 `keytool.exe`를 실행합니다.

```cmd
keytool.exe -list -v -keystore "%LocalAppData%\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

실행 시 **keytool.exe**는 다음 텍스트를 출력해야 합니다. **MD5:** 및 **SHA1:** 레이블은 해당 서명을 식별합니다.

```cmd
Alias name: androiddebugkey
Creation date: Aug 19, 2014
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 53f3b126
Valid from: Tue Aug 19 13:18:46 PDT 2014 until: Sun Nov 15 12:18:46 PST 2043
Certificate fingerprints:
         MD5:  27:78:7C:31:64:C2:79:C6:ED:E5:80:51:33:9C:03:57
         SHA1: 00:E5:8B:DA:29:49:9D:FC:1D:DA:E7:EE:EE:1A:8A:C7:85:E7:31:23
         SHA256: 21:0D:73:90:1D:D6:3D:AB:4C:80:4E:C4:A9:CB:97:FF:34:DD:B4:42:FC:
08:13:E0:49:51:65:A6:7C:7C:90:45
         Signature algorithm name: SHA1withRSA
         Version: 3
```


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

앱에 서명하는 데 사용되는 Xamarin **debug.keystore** 파일을 찾습니다. 기본적으로 다음 위치에서 Xamarin.Android 응용 프로그램의 디버그 버전에 서명하는 데 사용되는 키 저장소를 찾을 수 있습니다.

**~/.local/share/Xamarin/Mono for Android/debug.keystore**


JDK에서 **keytool** 명령을 실행하면 키 저장소에 대한 정보를 가져올 수 있습니다. 이 도구는 일반적으로 다음 위치에 있습니다.

**/System/Library/Java/JavaVirtualMachines/*VERSION*.jdk/Contents/Home/bin/keytool**

**keytool**을 포함하는 디렉터리를 **PATH** 환경 변수에 추가합니다.
**터미널**을 열고 다음 명령을 사용하여 **keytool**을 실행합니다.

```bash
$ keytool -list -v -keystore ~/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

실행 시 **keytool**은 다음 텍스트를 출력해야 합니다. **MD5:** 및 **SHA1:** 레이블은 해당 서명을 식별합니다.

```bash
Alias name: androiddebugkey
Creation date: Apr 16, 2015
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 76afa120
Valid from: Thu Apr 16 10:52:27 PDT 2015 until: Sat Apr 08 10:52:27 PDT 2045
Certificate fingerprints:
     MD5:  0A:D3:7E:80:3D:40:2A:23:89:B9:AB:9C:4B:B6:63:36
     SHA1: 89:33:8F:F2:C5:0C:91:08:4A:CF:04:A5:EC:4A:31:80:84:18:0D:D4
     SHA256: 91:AC:3E:2F:CB:EF:50:07:2B:E0:D9:8D:8B:C2:42:87:6A:85:02:86:EB:44:84:10:34:02:ED:35:CE:C6:38:47
     Signature algorithm name: SHA256withRSA
     Version: 3

Extensions:

#1: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 65 6C FE 67 8E CD CB 9A   01 CD 9F E8 25 9C A4 A3  el.g........%...
0010: 4F 4E CF 17                                        ON..
]
]
```

-----

## <a name="for-release--custom-signed-builds"></a>릴리스/서명된 사용자 지정 빌드용

사용자 지정 **.keystore** 파일을 사용하여 서명된 릴리스 빌드의 프로세스는 위와 동일하며, 릴리스 **.keystore** 파일이 Xamarin.Android에서 사용하는 **debug.keystore** 파일을 대체합니다. 릴리스 키 저장소 파일이 생성된 경우 키 저장소 암호와 별칭 이름의 고유한 값을 대체합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio **배포** 마법사를 사용하여 Xamarin.Android 앱에 서명한 경우 결과 키 저장소는 다음 위치에 있습니다.

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\alias\\alias.keystore**

예를 들어 [새 인증서 만들기](~/android/deploy-test/signing/index.md#newcertvs)의 단계를 따라 새 서명 키를 만든 경우 결과 예제 키 저장소는 다음 위치에 있습니다.

**C:\\Users\\*USERNAME*\\AppData\\Local\\Xamarin\\Mono for Android\\chimp\\chimp.keystore**

Xamarin.Android 앱에 서명하는 방법에 대한 자세한 내용은 [Android 응용 프로그램 패키지에 서명](~/android/deploy-test/signing/index.md)을 참조하세요.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac용 Visual Studio **서명 및 배포...** 마법사를 사용하여 앱에 서명한 경우 결과 키 저장소는 다음 위치에 있습니다.

**~/Library/Developer/Xamarin/Keystore/*alias*/*alias*.keystore**

예를 들어 [새 인증서 만들기](~/android/deploy-test/signing/index.md#newcertxs)의 단계를 따라 새 서명 키를 만든 경우 결과 예제 키 저장소는 다음 위치에 있습니다.

**~/Library/Developer/Xamarin/Keystore/chimp/chimp.keystore**

Xamarin.Android 앱에 서명하는 방법에 대한 자세한 내용은 [Android 응용 프로그램 패키지에 서명](~/android/deploy-test/signing/index.md)을 참조하세요.


-----
