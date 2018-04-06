---
title: 시스템 앱으로 Xamarin.Android 설치
description: 이 가이드에서는 시스템 앱과 사용자 앱의 차이점, 그리고 Xamarin.Android 응용 프로그램을 시스템 응용 프로그램으로 설치하는 방법을 설명합니다. 이 가이드는 사용자 지정 Android ROM 이미지 작성자에게 적용됩니다. 사용자 지정 ROM을 만드는 방법은 설명하지 않습니다.
ms.prod: xamarin
ms.assetid: 0113143B-7D8D-4C4C-B2F5-B966A2E7CE1F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 94f2108a55cea520782aa5eac959195be09929b5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="installing-xamarinandroid-as-a-system-app"></a>시스템 앱으로 Xamarin.Android 설치

_이 가이드에서는 시스템 앱과 사용자 앱의 차이점, 그리고 Xamarin.Android 응용 프로그램을 시스템 응용 프로그램으로 설치하는 방법을 설명합니다. 이 가이드는 사용자 지정 Android ROM 이미지 작성자에게 적용됩니다. 사용자 지정 ROM을 만드는 방법은 설명하지 않습니다._

## <a name="system-app"></a>시스템 앱

사용자 지정 Android ROM 이미지 작성자나 Android 장치 제조업체는 ROM 또는 장치를 배포할 때 Xamarin.Android 응용 프로그램을 _시스템 앱_으로 포함하는 것이 좋습니다. 시스템 앱은 사용자 지정 ROM 작성자가 항상 사용하기를 원하는 기능을 제공하거나 장치 작동에 중요하다고 간주되는 앱입니다.

시스템 앱은 **/system/app/** 폴더(파일 시스템의 읽기 전용 디렉터리)에 설치되며, 루트 액세스 권한이 없는 사용자는 삭제하거나 이동할 수 없습니다. 반면, 사용자가 (일반적으로 Google Play를 사용하거나 앱을 사이드로드하여) 설치한 응용 프로그램은 _사용자 앱_이라고 합니다. 사용자 앱은 사용자가 삭제할 수 있으며, 대부분의 경우 장치의 다른 위치로 이동할 수 있습니다(예: 외부 저장소).

시스템 앱은 사용자 앱과 똑같이 동작할 수 있지만 다음과 같은 주목할 만한 예외가 있습니다.

- 시스템 앱은 일반 _사용자 앱_처럼 업그레이드할 수 있습니다. 그러나 앱의 사본이 항상 **/system/app/**에 있으므로 응용 프로그램을 원래 버전으로 항상 롤백할 수 있습니다.

- 시스템 앱에는 사용자 앱에 제공되지 않는 특정 시스템 전용 권한이 부여될 수 있습니다. 시스템 전용 권한의 예로 사용자 상호 작용 없이 응용 프로그램을 Bluetooth 장치와 연결할 수 있게 해주는 [`BLUETOOTH_PRIVILEGED`](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)가 있습니다.

Xamarin.Android 앱은 시스템 응용 프로그램으로 배포할 수 있습니다. APK를 사용자 지정 ROM에 제공하는 것 외에도, **libmonodroid.so** 및 **libmonosgen-2.0.so**라는 두 가지 공유 라이브러리를 APK에서 ROM 이미지의 파일 시스템으로 수동 복사해야 합니다. 이 가이드에서는 관련 단계를 설명합니다.

## <a name="restrictions"></a>제한

이 가이드는 사용자 지정 Android ROM 이미지 작성자에게 적용됩니다. 사용자 지정 ROM을 만드는 방법은 설명하지 않습니다.

이 가이드에서는 [Xamarin.Android용 릴리스 APK 패키징](~/android/deploy-test/publishing/index.md)에 익숙하며 Android 응용 프로그램의 [CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)를 잘 이해하고 있다고 가정합니다.

## <a name="install-a-xamarinandroid-app-as-a-system-app"></a>시스템 앱으로 Xamarin.Android 앱 설치

다음 단계에는 시스템 앱으로 Xamarin.Android 앱을 설치하는 방법을 설명합니다.

1. **Xamarin.Android 앱의 릴리스 APK 패키징** &ndash; 이는 [응용 프로그램 게시](~/android/deploy-test/publishing/index.md) 가이드에 자세히 설명되어 있습니다.

2. **APK에서 공유 라이브러리 추출** &ndash; ZIP 유틸리티 프로그램을 사용하여 APK 파일을 열고 **/lib/** 폴더의 콘텐츠를 살펴봅니다. 이 폴더에는 응용 프로그램에서 지원하는 각 ABI(_응용 프로그램 이진 인터페이스_)의 하위 디렉터리가 있습니다. 이 폴더의 콘텐츠에는 특정 ABI의 앱에 필요한 모든 공유 라이브러리가 포함됩니다.

    ![taskypro.zip의 armeabi-v7a 폴더에 있는 .so 파일의 스크린샷](install-system-app-images/install-system-app-01.png)

   이전 스크린샷에는 앱에 필요한 두 가지 **.so** 파일이 저장되는 지원 ABI(**armeabi-v7a**)가 하나뿐이었습니다. 이는 장치나 장치 ROM의 대상 아키텍처에 적합한 ABI 파일을 추출하는 데만 필요합니다. 따라서 **x86** 폴더의 **.so** 파일을 **armeabi-v7a** 장치나 ROM으로 복사하지 마세요.

3. **.so 파일을 /system/lib에 복사** &ndash; 이전 단계의 APK에서 추출된 **.so** 파일을 사용자 지정 ROM의 **/system/lib/** 폴더로 복사합니다.

4. **APK 파일을 /system/app에 복사** &ndash; 마지막 단계는 APK 파일을 ROM의 **/system/app** 폴더로 복사하는 것입니다.


## <a name="summary"></a>요약

이 가이드에서는 _시스템 앱_과 _사용자 앱_의 차이점, 그리고 Xamarin.Android 응용 프로그램을 시스템 앱으로 설치하는 방법을 설명했습니다.



## <a name="related-links"></a>관련 링크

- [응용 프로그램 게시](~/android/deploy-test/publishing/index.md)
- [CPU 아키텍처](~/android/app-fundamentals/cpu-architectures.md)
- [BLUETOOTH_PRIVILEGED](https://developer.android.com/reference/android/Manifest.permission.html#BLUETOOTH_PRIVILEGED)
- [ABI 관리](https://developer.android.com/ndk~/abis.html)
