---
title: 다중 코어 디바이스 및 Xamarin.Android
description: Android는 여러 컴퓨터 아키텍처에서 실행할 수 있습니다. 이 문서에서는 Xamarin.Android 애플리케이션에 대해 사용할 수 있는 여러 CPU 아키텍처에 설명합니다. 이 문서에서는 또한 여러 CPU 아키텍처를 지원하도록 Android 애플리케이션을 패키징하는 방법도 설명합니다. ABI(애플리케이션 이진 인터페이스)를 소개하고, Xamarin.Android 애플리케이션에서 사용할 ABI에 대한 지침을 제공합니다.
ms.prod: xamarin
ms.assetid: D812883C-A14A-E74B-0F72-E50071E96328
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2019
ms.openlocfilehash: 1141b96151df0adda755b7c6d60019c18825cc76
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028023"
---
# <a name="multi-core-devices--xamarinandroid"></a>다중 코어 디바이스 및 Xamarin.Android

_Android는 여러 컴퓨터 아키텍처에서 실행할 수 있습니다. 이 문서에서는 Xamarin.Android 애플리케이션에 대해 사용할 수 있는 여러 CPU 아키텍처에 설명합니다. 이 문서에서는 또한 여러 CPU 아키텍처를 지원하도록 Android 애플리케이션을 패키징하는 방법도 설명합니다. ABI(애플리케이션 이진 인터페이스)를 소개하고, Xamarin.Android 애플리케이션에서 사용할 ABI에 대한 지침을 제공합니다._

## <a name="overview"></a>개요

Android는 여러 CPU 아키텍처를 지원하는 컴퓨터 코드가 포함된 단일 `.apk` 파일인 "패트 이진"의 생성을 지원합니다. 이는 *애플리케이션 이진 인터페이스*로 컴퓨터 코드의 각 조각을 연결하여 만들 수 있습니다. ABI는 특정 하드웨어 디바이스에서 실행될 컴퓨터 코드를 제어하는 데 사용됩니다. 예를 들어 x86 디바이스에서 실행되는 Android 애플리케이션의 경우 애플리케이션을 컴파일할 때 x86 ABI 지원을 포함해야 합니다.

특히 각 Android 애플리케이션은 EABI(*포함된 애플리케이션 이진 인터페이스*)를 하나 이상 지원하게 됩니다. EABI는 포함된 소프트웨어 프로그램과 관련된 규칙입니다. 일반적인 EABI는 다음과 같은 항목을 설명합니다.

- CPU 명령 집합.

- 런타임 시 메모리 저장 및 로드의 엔디언.

- 개체 파일 및 프로그램 라이브러리의 이진 형식과 이러한 파일 및 라이브러리에서 허용 또는 지원되는 콘텐츠 형식

- 애플리케이션 코드와 시스템 간에 데이터를 전달하는 데 사용되는 다양한 규칙(예: 함수가 호출될 때 레지스터 및/또는 스택이 사용되는 방법, 맞춤 제약 조건 등)

- 열거형 형식, 구조체, 필드 및 배열에 대한 맞춤 및 크기 제약 조건

- 런타임 시 머신 코드에 사용할 수 있는 함수 기호 목록(일반적으로 구체적으로 선택된 라이브러리 집합에서 가져옴)

### <a name="armeabi-and-thread-safety"></a>armeabi 및 스레드 보안

애플리케이션 이진 인터페이스는 아래에서 자세히 살펴보겠지만, Xamarin.Android에서 사용하는 `armeabi` 런타임은 *스레드로부터 안전하지 않다는 것*을 유념하시기 바랍니다. `armeabi` 지원이 포함된 애플리케이션이 `armeabi-v7a` 디바이스에 배포될 경우 이상하고 말로 설명할 수 없는 예외가 많이 발생합니다.

Android 4.0.0, 4.0.1, 4.0.2 및 4.0.3의 버그로 인해 `armeabi-v7a` 디렉터리가 있고 디바이스가 `armeabi-v7a` 디바이스이더라도 네이티브 라이브러리는 `armeabi` 디렉터리에서 선택됩니다.

> [!NOTE]
> Xamarin.Android는 `.so`가 올바른 순서로 APK에 추가되도록 합니다. 이 버그는 Xamarin.Android의 사용자에게 문제가 되지 않습니다.

### <a name="abi-descriptions"></a>ABI 설명

Android에서 지원하는 각 ABI는 고유한 이름으로 식별됩니다.

#### <a name="armeabi"></a>armeabi

이는 하나 이상의 ARMv5TE 명령 집합을 지원하는 ARM 기반 CPU의 EABI 이름입니다. Android는 little-endian ARM GNU/Linux ABI를 따릅니다. 이 ABI는 하드웨어 기반 부동 소수점 계산을 지원하지 않습니다. 모든 FP 작업은 컴파일러의 `libgcc.a` 정적 라이브러리에서 제공하는 소프트웨어 도우미 함수에 의해 수행됩니다. `armeabi`는 SMP 디바이스를 지원하지 않습니다.

> [!IMPORTANT]
> Xamarin.Android의 `armeabi` 코드는 스레드로부터 안전하지 않으며 다중 CPU `armeabi-v7a` 디바이스에 사용해서는 안 됩니다(아래 설명 참조). 단일 코어 `armeabi-v7a` 디바이스에서 `armeabi` 코드를 사용하는 것은 안전합니다.

#### <a name="armeabi-v7a"></a>armeabi-v7a

이는 위에 설명된 `armeabi` EABI를 확장하는 또 다른 ARM 기반 CPU 명령 집합입니다. 이 `armeabi-v7a` EABI는 하드웨어 부동 소수점 연산 및 다중 CPU(SMP) 디바이스를 지원합니다. `armeabi-v7a` EABI를 사용하는 애플리케이션은 `armeabi`를 사용하는 애플리케이션보다 상당한 성능 개선 효과를 기대할 수 있습니다.

> [!NOTE]
> `armeabi-v7a` 기계어 코드는 ARMv5 디바이스에서 실행되지 않습니다.

#### <a name="arm64-v8a"></a>arm64-v8a

이는 ARMv8 CPU 아키텍처에 기반을 둔 64비트 명령 집합입니다. 이 아키텍처는 *Nexus 9*에서 사용됩니다.
Xamarin.Android 5.1이 이 아키텍처에 대한 지원을 도입했습니다. 자세한 내용은 [64비트 런타임 지원](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)을 확인하세요.

#### <a name="x86"></a>x86

이는 명령 집합을 지원하는 CPU의 ABI 이름이며, 일반적으로 *x86* 또는 *IA-32*라고 합니다. 이 ABI는 MMX, SSE, SSE2 및 SSE3 명령 집합을 포함한 Pentium Pro 명령 집합의 명령에 해당합니다. 다음과 같은 다른 선택적 IA-32 명령 집합은 포함하지 않습니다.

- MOVBE 명령.
- 보조 SSE3 확장(SSSE3).
- SSE4 변형.

> [!NOTE]
> Google TV는 x86에서 실행되기는 하지만 Android의 NDK에서 지원되지 않습니다.

#### <a name="x86_64"></a>x86_64

이는 64비트 x86 명령 집합을 지원하는 CPU의 ABI입니다(*x64* 또는 *AMD64*라고도 함). Xamarin.Android 5.1이 이 아키텍처에 대한 지원을 도입했습니다. 자세한 내용은 [64비트 런타임 지원](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1/index.md#64-bit-runtime-support)을 확인하세요.

#### <a name="apk-file-format"></a>APK 파일 형식

Android 애플리케이션 패키지는 Android 애플리케이션에 필요한 모든 코드, 자산, 리소스 및 인증서를 저장하는 파일 형식입니다. `.zip` 파일 이지만 `.apk` 파일 이름 확장명을 사용합니다. 확장할 경우 Xamarin.Android에서 만든 `.apk` 콘텐츠를 아래 스크린샷에서 볼 수 있습니다.

[![.apk의 콘텐츠](multicore-devices-images/00.png)](multicore-devices-images/00.png#lightbox)

`.apk` 파일의 콘텐츠에 대한 간단한 설명:

- **AndroidManifest.xml** &ndash; 이는 이진 XML 형식의 `AndroidManifest.xml` 파일입니다.

- **classes.dex**&ndash; 여기에는 Android 런타임 VM에서 사용하는 `dex` 파일 형식으로 컴파일된 애플리케이션 코드가 포함됩니다.

- **resources.arsc**&ndash; 이 파일에는 애플리케이션의 미리 컴파일된 리소스가 모두 포함됩니다.

- **lib** &ndash; 이 디렉터리에는 각 ABI의 컴파일된 코드가 저장됩니다. 이전 섹션에서 설명된 각 ABI당 하나의 하위 폴더가 포함됩니다. 위의 스크린샷에서 해당 `.apk`에는 `armeabi-v7a` 및 `x86` 모두를 위한 네이티브 라이브러리가 있습니다.

- **META-INF** &ndash; 이 디렉터리(있는 경우)는 서명 정보, 패키지 및 확장 구성 데이터를 저장하는 데 사용됩니다.

- **res** &ndash; 이 디렉터리는 `resources.arsc`로 컴파일되지 않은 리소스를 저장합니다.

> [!NOTE]
> `libmonodroid.so` 파일은 모든 Xamarin.Android 애플리케이션에 필요한 네이티브 라이브러리입니다.

#### <a name="android-device-abi-support"></a>Android 디바이스 ABI 지원

각 Android 디바이스는 최대 두 개의 ABI에서 네이티브 코드 실행을 지원합니다.

- **“주” ABI** &ndash; 이는 시스템 이미지에서 사용되는 머신 코드에 해당합니다.

- **“보조” ABI** &ndash; 이는 시스템 이미지에서도 지원되는 선택적 ABI입니다.

예를 들어 일반적인 ARMv5TE 디바이스는 `armeabi`의 주 ABI만 있지만, ARMv7 디바이스는 `armeabi-v7a`의 주 ABI와 `armeabi`의 보조 ABI를 지정합니다. 일반적인 x86 디바이스는 `x86`의 주 ABI만 지정합니다.

### <a name="android-native-library-installation"></a>Android 네이티브 라이브러리 설치

패키지 설치 시 `.apk` 내 네이티브 라이브러리가 앱의 네이티브 라이브러리 디렉터리(일반적으로 `/data/data/<package-name>/lib`)로 추출되고, 그 이후에는 `$APP/lib`라 불립니다.

Android의 네이티브 라이브러리 설치 동작은 Android 버전에 따라 큰 차이가 있습니다.

#### <a name="installing-native-libraries-pre-android-40"></a>네이티브 라이브러리 설치: Android 4.0 이전

Android 4.0 Ice Cream Sandwich 이전 버전은 `.apk` 내 *단일 ABI*에서만 네이티브 라이브러리를 추출합니다. 이 버전의 Android 앱은 먼저 주 ABI의 모든 네이티브 라이브러리 추출을 시도하고, 이러한 라이브러리가 없을 경우 보조 ABI의 모든 네이티브 라이브러리를 추출합니다. "병합"은 수행되지 않습니다.

예를 들어 `armeabi-v7a` 디바이스에 애플리케이션이 설치된 상황을 생각해 보세요. `armeabi` 및 `armeabi-v7a`를 모두 지원하는 `.apk,`에는 다음과 같은 ABI `lib` 디렉터리와 파일이 포함되어 있습니다.

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

설치 후 네이티브 라이브러리 디렉터리에는 다음이 포함됩니다.

```shell
$APP/lib/libtwo.so # from the armeabi-v7a directory in the apk
```

즉, `libone.so`는 설치되지 않습니다. 그러면 문제가 발생합니다. 애플리케이션이 런타임에 로드할 `libone.so`가 없기 때문입니다. 이 동작은 예상하지 못한 것이지만 버그로 기록되었고 "[예상대로 작동](https://code.google.com/p/android/issues/detail?id=9089)"으로 재분류되었습니다.

따라서 Android 4.0 이전 버전을 대상으로 하는 경우 애플리케이션이 지원할 *각* ABI의 *모든* 네이티브 라이브러리를 제공해야 합니다. 즉, `.apk`는 다음을 포함해야 합니다.

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libone.so
lib/armeabi-v7a/libtwo.so
```

#### <a name="installing-native-libraries-android-40-ndash-android-403"></a>네이티브 라이브러리 설치: Android 4.0 &ndash; Android 4.0.3

Android 4.0 Ice Cream Sandwich는 추출 논리를 변경합니다. 모든 네이티브 라이브러리를 열거하고, 파일의 기본 이름이 이미 추출되었는지, 다음 조건이 둘 다 충족되었는지 확인한 후 라이브러리를 추출합니다.

- 아직 추출되지 않았습니다.

- 네이티브 라이브러리의 ABI가 대상의 주 또는 보조 ABI와 일치합니다.

이러한 조건을 충족하면(즉, 다음과 같은 콘텐츠가 포함된 `.apk`가 있다면) "병합" 동작이 가능합니다.

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

그러면 설치 후 네이티브 라이브러리 디렉터리에 다음이 포함됩니다.

```shell
$APP/lib/libone.so
$APP/lib/libtwo.so
```

불행히도 이 동작은 다음 문서 [문제 24321: apk에 armeabi와 armeabi-v7a가 둘 다 포함된 경우 Galaxy Nexus 4.0.2가 armeabi 네이티브 코드 사용](https://code.google.com/p/android/issues/detail?id=25321)에 설명된 대로 순서에 따라 달라집니다.

네이티브 라이브러리는 "순서대로"(예를 들어 unzip에 나열된 대로) 처리되고, *처음 일치* 항목이 추출됩니다. `.apk`에는 `armeabi` 및 `armeabi-v7a` 버전의 `libtwo.so`가 포함되어 있고 `armeabi`가 먼저 나열되므로 `armeabi-v7a` 버전이 *아닌* `armeabi` 버전이 추출됩니다.

```shell
$APP/lib/libone.so # armeabi
$APP/lib/libtwo.so # armeabi, NOT armeabi-v7a!
```

또한 Xamarin.Android는 (아래의 *지원되는 ABI 선언* 섹션에 설명된 대로) `armeabi` 및 `armeabi-v7a` ABI가 둘 다 지정되어 있더라도 다음에 다음과 같은 요소를 만듭니다.
`csproj`:

```xml
<AndroidSupportedAbis>armeabi,armeabi-v7a</AndroidSupportedAbis>
```

따라서 `armeabi-v7a` `libmonodroid.so`가 있고 대상에 최적화되어 있더라도 `.apk` 내에서 `armeabi` `libmonodroid.so`가 먼저 발견되고, `armeabi` `libmonodroid.so`가 추출됩니다. 또한 `armeabi`는 SMP로부터 안전하지 않으므로 이로 인해 모호한 런타임 오류가 발생할 수도 있습니다.

##### <a name="installing-native-libraries-android-404-and-later"></a>네이티브 라이브러리 설치: Android 4.0.4 이상

Android 4.0.4는 추출 논리를 변경합니다. 모든 네이티브 라이브러리를 열거하고, 파일의 기본 이름을 읽은 후, 주 ABI 버전(있는 경우) 또는 보조 ABI(있는 경우)를 추출합니다. 따라서 다음과 같은 콘텐츠가 포함된 `.apk`가 있다면 "병합" 동작이 가능합니다.

```shell
lib/armeabi/libone.so
lib/armeabi/libtwo.so
lib/armeabi-v7a/libtwo.so
```

그러면 설치 후 네이티브 라이브러리 디렉터리에 다음이 포함됩니다.

```shell
$APP/lib/libone.so # from armeabi
$APP/lib/libtwo.so # from armeabi-v7a
```

### <a name="xamarinandroid-and-abis"></a>Xamarin.Android 및 ABI

Xamarin.Android는 다음과 같은 _64비트_ 아키텍처를 지원합니다.

- `arm64-v8a`
- `x86_64`

> [!NOTE]
> 2018년 8월부터 새 앱은 API 레벨 26을 대상으로 해야 하고, 2019년 8월부터 앱은 32비트 버전 외에 [‘64비트 버전을 제공’](https://android-developers.googleblog.com/2017/12/improving-app-security-and-performance.html)해야 합니다.

Xamarin.Android는 다음 32비트 아키텍처를 지원합니다.

- `armeabi` ^
- `armeabi-v7a`
- `x86`

> [!NOTE]
> **^** [Xamarin.Android 9.2](https://docs.microsoft.com/xamarin/android/release-notes/9/9.2#removal-of-support-for-armeabi-cpu-architecture)부터는 `armeabi`가 더이상 지원되지 않습니다.

Xamarin.Android는 현재 `mips`에 대한 지원을 제공하지 않습니다.

### <a name="declaring-supported-abis"></a>지원되는 ABI 선언

기본적으로 Xamarin.Android는 **릴리스** 빌드의 경우 `armeabi-v7a`로, **디버그** 빌드의 경우 `armeabi-v7a` 및 `x86`으로 지정됩니다. 다양한 ABI에 대한 지원은 Xamarin.Android 프로젝트의 프로젝트 옵션을 통해 설정할 수 있습니다. Visual Studio에서는 다음 스크린샷에서처럼 **고급** 탭에서 프로젝트 **속성**의 **Android 옵션** 페이지에서 이를 설정할 수 있습니다.

![Android 옵션 고급 속성](multicore-devices-images/vs-abi-selections.png)

Mac용 Visual Studio에서는 다음 스크린샷에서처럼 **고급** 탭에서 **프로젝트 옵션**의 **Android 빌드** 페이지에서 지원되는 아키텍처를 선택할 수 있습니다.

[![Android 빌드 지원 ABI](multicore-devices-images/xs-abi-selections-sml.png)](multicore-devices-images/xs-abi-selections.png#lightbox)

다음과 같이 추가적인 ABI 지원을 선언해야 하는 경우가 있습니다.

- `x86` 디바이스에 애플리케이션 배포.

- 스레드 보안을 위해 `armeabi-v7a` 디바이스에 애플리케이션 배포

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Android 애플리케이션을 실행할 수 있는 여러 CPU 아키텍처를 설명했습니다. 애플리케이션 이진 인터페이스와 Android에서 이를 사용하여 개별 CPU 아키텍처를 지원하는 방법을 소개했습니다.
그런 다음, Xamarin.Android 애플리케이션에서 ABI 지원을 지정하는 방법을 설명하고, `armeabi` 전용으로 고안된 `armeabi-v7a` 디바이스에서 Xamarin.Android 애플리케이션을 사용할 경우 발생하는 문제를 중점적으로 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [ARM 아키텍처용 ABI(PDF)](http://infocenter.arm.com/help/topic/com.arm.doc.ihi0036b/IHI0036B_bsabi.pdf)
- [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html)
- [문제 9089: Nexus One - armeabi-v7a에 하나 이상의 라이브러리가 없을 경우 armeabi에서 네이티브 라이브러리를 로드하지 않음](https://code.google.com/p/android/issues/detail?id=9089)
- [문제 24321: apk에 armeabi와 armeabi-v7a가 둘 다 포함된 경우 Galaxy Nexus 4.0.2가 armeabi 네이티브 코드 사용](https://code.google.com/p/android/issues/detail?id=25321)
