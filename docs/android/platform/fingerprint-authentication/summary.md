---
title: 지문 인증 지침
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e955d4f96724bd5682e7d0e6db2c36fa1b7810f4
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027425"
---
# <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

## <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

지금까지 Android 6.0 지문 인증과 관련된 개념 및 API를 살펴보았습니다. 이번에는 지문 API를 사용하기 위한 몇 가지 일반적인 조언을 살펴보겠습니다.

1. **Android 지원 라이브러리 v4 호환성 API 사용** &ndash; 이렇게 하면 코드에서 API 검사를 제거하여 애플리케이션 코드를 간소화하고 애플리케이션이 가능한 최대의 디바이스를 대상으로 설정하도록 허용할 수 있습니다.
2. **지문 인증의 대안 제공** &ndash; 지문 인증은 애플리케이션이 사용자를 인증하는 빠르고 우수한 방법이긴 하나, 항상 작동할 것이라고 또는 항상 사용할 수 있다고 가정할 수 없습니다. 지문 스캐너가 작동하지 않거나 렌즈가 더러울 수 있고, 사용자가 지문 인증을 사용하도록 디바이스를 구성하지 않았을 수 있으며, 구성된 지문이 사라졌을 수도 있습니다. 사용자가 애플리케이션에서 지문 인증을 사용하지 않고 싶어 할 수도 있습니다. 해당 이유로 인해 Android 애플리케이션에서는 사용자 이름 및 암호와 같은 대안 인증 프로세스를 제공해야 합니다.
3. **Google의 지문 아이콘 사용** &ndash; 모든 애플리케이션에서는 Google에서 제공하는 동일한 지문 아이콘을 사용해야 합니다. 표준 아이콘을 사용하면 Android 사용자가 앱의 어디에서 지문 인증이 사용되는지 손쉽게 인식할 수 있습니다. 
    
    ![Android 지문 아이콘](summary-images/ic-fp-40px.png)
    
4. **사용자에게 안내** &ndash; 애플리케이션에서 지문 스캐너가 활성 상태이고 터치 또는 스와이프를 기다리고 있음을 안내하는 알림을 사용자에게 표시해야 합니다. 

## <a name="summary"></a>요약

지문 인증은 Xamarin.Android 애플리케이션이 사용자를 빠르게 확인하도록 허용하고 사용자가 앱 내 구매와 같은 민감한 기능을 손쉽게 상호 작용할 수 있도록 지원하는 좋은 방법입니다. 이 가이드에서는 Xamarin.Android 애플리케이션에 Android 6.0 지문 API를 통합하는 데 필요한 개념과 코드를 살펴보았습니다.

먼저 지문 API `FingerprintManager`(및 `FingerprintManagerCompat`)를 살펴보았습니다. 애플리케이션에서 `FingerprintManager.AuthenticationCallbacks` 추상 클래스를 확장하여 지문 하드웨어와 애플리케이션 사이의 중간 매체로 사용해야 한다는 사실을 살펴보았습니다. 이후 Java `Cipher` 개체를 사용하여 지문 스캐너의 무결성을 확인하는 방법을 알아보았습니다. 마지막으로, 디바이스에 지문을 등록하는 방법과 **adb**를 사용하여 에뮬레이터에서 지문을 시뮬레이션하는 방법을 설명함으로써 테스트에 관한 주제를 간략하게 살펴보았습니다. 

이 가이드와 함께 제공되는 [애플리케이션 예제](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)도 살펴보시기 바랍니다. [지문 대화 상자 예제](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)은 Java에서 Xamarin.Android로 이식되었으며, Android 애플리케이션에 지문 인증을 추가하는 또 다른 예제를 제공합니다.

## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [지문 대화 상자 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [지문 아이콘](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
