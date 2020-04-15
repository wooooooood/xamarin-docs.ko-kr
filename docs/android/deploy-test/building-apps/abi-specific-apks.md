---
title: ABI 관련 APK 빌드
description: 이 문서에서는 Xamarin.Android를 사용하여 단일 ABI를 대상으로 하는 APK를 빌드하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/15/2018
ms.openlocfilehash: 0520439b89458b7f73a025cd8d6b2cf8fc41dac0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940631"
---
# <a name="building-abi-specific-apks"></a>ABI 관련 APK 빌드

_이 문서에서는 Xamarin.Android를 사용하여 단일 ABI를 대상으로 하는 APK를 빌드하는 방법을 설명합니다._

## <a name="overview"></a>개요

애플리케이션에 여러 APK를 포함하는 것이 유리한 경우가 있습니다. 각 APK는 동일한 키 스토리지를 사용하여 서명되고 동일한 패키지 이름을 공유하지만 특정 디바이스나 Android 구성을 위해 컴파일됩니다. 이는 권장되는 방법이 아닙니다. 여러 디바이스와 구성을 지원할 수 있는 APK 하나를 포함하는 것이 훨씬 더 간단합니다. 다음과 같이 여러 APK를 만드는 것이 유용한 경우도 있습니다.

- **APK 크기 축소** - Google Play는 APK 파일에 100MB 크기 제한을 적용합니다. 디바이스별 APK를 만들면 애플리케이션의 자산 및 리소스를 일부만 제공하면 되므로 APK의 크기를 줄일 수 있습니다.

- **여러 CPU 아키텍처 지원** - 애플리케이션에 특정 CPU에 대한 공유 라이브러리가 있을 경우 해당 CPU에 대한 공유 라이브러리만 배포할 수 있습니다.

여러 APK가 있으면 배포가 복잡해질 수 있음 - Google Play에서 해결한 문제입니다. Google Play는 **AndroidManifest.XML**에 포함된 애플리케이션의 버전 코드 및 기타 메타데이터에 따라 디바이스에 올바른 APK가 제공되도록 합니다. Google Play에서 애플리케이션에 여러 APK를 지원하는 방식에 대한 자세한 내용과 제한 사항은 [여러 APK 지원에 대한 Google 설명서](https://developer.android.com/google/play/publishing/multiple-apks.html)를 참조하세요.

이 가이드에서는 Xamarin.Android 애플리케이션을 위해 각 APK가 특정 ABI를 대상으로 하는 여러 APK 빌드를 스크립팅하는 방법을 설명합니다. 다음 토픽을 살펴봅니다.

1. APK의 고유한 *버전 코드*를 만듭니다.
1. 이 APK에 사용할 임시 버전의 **AndroidManifest.XML**을 만듭니다.
1. 이전 단계의 **AndroidManifest.XML**을 사용하여 애플리케이션을 빌드합니다.
1. APK에 서명하고 zipalign하여 릴리스를 준비합니다.

이 가이드의 뒷부분에 [Rake](https://martinfowler.com/articles/rake.html)를 사용하여 이러한 단계를 스크립팅하는 방법을 보여주는 연습이 있습니다.

### <a name="creating-the-version-code-for-the-apk"></a>APK의 버전 코드 만들기

Google에서는 7자리 버전 코드를 사용하는 버전 코드에 특정 알고리즘을 권장합니다([여러 APK 지원 문서](https://developer.android.com/google/play/publishing/multiple-apks.html)에서 *버전 코드 구성표 사용* 섹션 참조).
이 버전 코드 구성표를 8자리로 확장하면 일부 ABI 정보를 버전 코드에 포함하여 Google Play에서 디바이스에 올바른 APK를 배포하도록 할 수 있습니다. 다음 목록에서는 이 8자리 버전 코드 형식을 설명합니다(왼쪽에서 오른쪽으로 인덱싱됨).

- **인덱스 0**(아래 다이어그램에서 빨간색) &ndash; ABI의 정수:
  - 1 &ndash; `armeabi`
  - 2 &ndash; `armeabi-v7a`
  - 6 &ndash; `x86`

- **인덱스 1-2**(아래 다이어그램에서 주황색) &ndash; 애플리케이션에서 지원하는 최소 API 수준입니다.

- **인덱스 3-4**(아래 다이어그램에서 파란색) &ndash; 지원되는 화면 크기:
  - 1 &ndash; 소형
  - 2 &ndash; 표준
  - 3 &ndash; 대형
  - 4 &ndash; 초대형

- **인덱스 5-7**(아래 다이어그램에서 녹색) &ndash; 버전 코드의 고유 번호입니다. 
    이 항목은 개발자가 설정합니다. 애플리케이션의 각 공용 릴리스마다 증가해야 합니다.

다음 다이어그램에서는 위 목록에 설명된 각 코드의 위치를 보여줍니다.

[![다이어그램의 8자리 버전 코드 형식의 다이어그램(색상으로 구분)](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)

Google Play에서는 `versionCode` 및 APK 구성에 따라 디바이스에 올바른 APK가 제공되도록 합니다. 가장 높은 버전 코드를 가진 APK가 디바이스에 제공됩니다. 예를 들어 애플리케이션에 다음 버전 코드를 갖는 세 APK가 있을 수 있습니다.

- 11413456 - ABI가 `armeabi`이고, API 수준 14, 소형~대형 화면을 대상으로 하며, 버전 번호는 456입니다.
- 21423456 - ABI가 `armeabi-v7a`이고, API 수준 14, 표준 및 대형 화면을 대상으로 하며, 버전 번호는 456입니다.
- 61423456 - ABI가 `x86`이고, API 수준 14, 표준 및 대형 화면을 대상으로 하며, 버전 번호는 456입니다.

이 예제로 계속 진행하려면 `armeabi-v7a`와 관련된 버그가 해결되었다고 간주하세요. 앱 버전이 457로 증가하고, `android:versionCode`를 21423457로 설정하여 새 APK가 빌드됩니다. `armeabi` 및 `x86` 버전의 versionCodes는 동일하게 유지됩니다.

x86 버전이 최신 API(API 수준 19)를 대상으로 하는 업데이트나 버그 수정을 받아서 앱의 버전이 500이 된다고 가정해 보겠습니다. 새 `versionCode`는 61923500으로 변경되고, armeabi/armeabi-v7a는 그대로 유지됩니다. 이때 버전 코드는 다음과 같습니다.

- 11413456 - ABI가 `armeabi`이고, API 수준 14, 소형~대형 화면을 대상으로 하며, 버전 이름은 456입니다.
- 21423457 - ABI가 `armeabi-v7a`이고, API 수준 14, 소형~대형 화면을 대상으로 하며, 버전 이름은 457입니다.
- 61923500 - ABI가 `x86`이고, API 수준 19, 소형~대형 화면을 대상으로 하며, 버전 이름은 500입니다.

이러한 버전 코드를 수동으로 유지 관리하는 것은 개발자에게 큰 부담이 될 수 있습니다. 정확한 `android:versionCode`를 계산한 후 APK를 빌드하는 프로세스는 자동화되어야 합니다.
이를 수행하는 방법에 대한 예는 이 문서의 뒷부분에 나오는 연습에서 설명합니다.

### <a name="create-a-temporary-androidmanifestxml"></a>임시 AndroidManifest.XML 만들기

꼭 필요하지는 않지만 각 ABI에 대한 임시 **AndroidManifest.XML**을 만들면 한 APK에서 다른 APK로 정보가 유출되면서 발생하는 문제를 방지할 수 있습니다. 예를 들어 `android:versionCode` 특성은 각 APK에 대해 고유해야 합니다.

이를 수행하는 방법은 관련 스크립팅 시스템에 따라 다르지만 일반적으로 개발 중에 사용한 Android 매니페스트의 사본을 만들고, 수정한 후, 빌드 프로세스 중에 그 수정된 매니페스트를 사용하는 것입니다.

### <a name="compiling-the-apk"></a>APK 컴파일

ABI별 APK를 빌드할 때는 다음 샘플 명령줄에 나와 있는 것처럼 `xbuild` 또는 `msbuild`를 사용하는 것이 가장 좋습니다.

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

다음 목록에서는 각 명령줄 매개 변수를 설명합니다.

- `/t:Package` &ndash; 디버그 키 저장소를 사용하여 서명된 Android APK를 만듭니다.

- `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; 대상으로 할 ABI입니다. `armeabi`, `armeabi-v7a` 또는 `x86` 중 하나여야 합니다.

- `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; 빌드의 일부로 생성된 중간 파일을 저장할 디렉터리입니다. Xamarin.Android는 필요할 경우 ABI의 이름을 딴 디렉터리를 만듭니다(예: `obj.armeabi-v7a`). 한 빌드에서 다른 빌드로 파일이 "유출"되는 결과를 가져오는 문제를 방지할 수 있으므로 각 ABI당 하나의 폴더를 사용하는 것이 좋습니다. 이 값은 디렉터리 구분 기호(OS X의 경우 `/`)를 사용하여 종결됩니다.

- `/p:AndroidManifest` &ndash; 이 속성은 빌드 중에 사용되는 **AndroidManifest.XML** 파일의 경로를 지정합니다.

- `/p:OutputPath=bin.<TARGET_ABI>` &ndash; 최종 APK가 저장될 디렉터리입니다. Xamarin.Android는 ABI의 이름을 딴 디렉터리를 만듭니다(예: `bin.armeabi-v7a`).

- `/p:Configuration=Release` &ndash; APK의 릴리스 빌드를 수행합니다. 디버그 빌드는 Google Play에 업로드하지 못할 수 있습니다.

- `<CS_PROJ FILE>` &ndash; Xamarin.Android 프로젝트에 대한 `.csproj` 파일의 경로입니다.

### <a name="sign-and-zipalign-the-apk"></a>APK 서명 및 Zipalign

APK에 서명해야 Google Play를 통해 배포할 수 있습니다. 이는 Java 개발자 키트에 포함된 `jarsigner` 애플리케이션을 사용하여 수행할 수 있습니다. 다음 명령은 명령줄에서 `jarsigner`를 사용하는 방법을 보여줍니다.

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

모든 Xamarin.Android 애플리케이션은 zipalign되어야 디바이스에서 실행될 수 있습니다. 사용할 명령줄의 형식은 다음과 같습니다.

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```

## <a name="automating-apk-creation-with-rake"></a>Rake를 사용한 APK 만들기 자동화

샘플 프로젝트 [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK)는 ABI별 버전 번호를 계산하고 다음과 같은 각 ABI별로 별도의 APK 3개를 빌드하는 방법을 보여 주는 간단한 Android 프로젝트입니다.

- armeabi
- armeabi-v7a
- x86

샘플 프로젝트의 [rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb)은 이전 섹션에 설명된 각 단계를 수행합니다.

1. APK에 대한 [android:versionCode를 만듭니다](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30).

1. 해당 APK의 사용자 지정 **AndroidManifest.XML**에 [android:versionCode를 작성합니다](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55).

1. 이전 단계에서 생성된 **AndroidManifest.XML**을 사용하여 ABI를 개별 대상으로 하는 Xamarin.Android 프로젝트의 [릴리스 빌드를 컴파일합니다](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63).

1. 프로덕션 키 저장소를 사용하여 [APK에 서명합니다](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66).

1. APK [zipalign](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67)을 수행합니다.

애플리케이션의 모든 APK를 빌드하려면 명령줄에서 `build` Rake 작업을 실행합니다.

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Rake 작업이 완료되면 `xamarin.helloworld.apk` 파일이 있는 `bin` 폴더 3개가 있을 것입니다. 다음 스크린샷은 이러한 각각의 폴더와 그 내용을 보여줍니다.

[![xamarin.helloworld.apk를 포함하는 플랫폼별 폴더의 위치](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)

> [!NOTE]
> 이 가이드에 설명된 빌드 프로세스는 여러 다른 빌드 시스템 중 하나에서 구현될 수 있습니다. 미리 작성된 예제는 없지만 [Powershell](https://technet.microsoft.com/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) 또는 [Fake](https://fsharp.github.io/FAKE/)를 사용하면 가능합니다.

## <a name="summary"></a>요약

이 가이드에서는 특정 ABI를 대상으로 하는 Android APK를 만드는 방법과 관련된 몇 가지 제안 사항을 제공했습니다. 또한 APK가 대상으로 하는 CPU 아키텍처를 식별하는 `android:versionCodes`를 만들기 위한 하나의 구성표도 설명했습니다. 연습에서는 Rake를 사용하여 빌드를 스크립팅한 샘플 프로젝트를 제공했습니다.

## <a name="related-links"></a>관련 링크

- [OneABIPerAPK(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/oneabiperapk)
- [애플리케이션 게시](~/android/deploy-test/publishing/index.md)
- [Google Play에 대한 여러 APK 지원](https://developer.android.com/google/play/publishing/multiple-apks.html)
