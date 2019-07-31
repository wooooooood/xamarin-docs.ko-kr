---
title: 지문 인증 지침
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 08738a751fd630c6a413b1c7393f8007f5c97060
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68643554"
---
# <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

## <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

Android 6.0 지문 인증을 둘러싼 개념 및 Api를 살펴보았으므로 이제 지문 Api 사용에 대 한 몇 가지 일반적인 조언을 살펴보겠습니다.

1. **Android 지원 라이브러리 V4 호환성 Api 사용** &ndash; 이렇게 하면 코드에서 API 검사를 제거 하 여 응용 프로그램 코드를 간소화 하 고 응용 프로그램에서 가능한 대부분의 장치를 대상으로 지정할 수 있습니다.
2. **지문 인증에 대 한 대안 제공** &ndash; 지문 인증은 응용 프로그램이 사용자를 인증 하는 매우 빠른 방법 이지만 항상 작동 하거나 사용할 수 있는 것으로 간주할 수 없습니다. 지문 스캐너가 실패 하거나, 렌즈가 더티 이거나, 사용자가 지문 인증을 사용 하도록 장치를 구성 하지 않았거나, 지문이 누락 된 후에도이 문제가 발생할 수 있습니다. 사용자가 응용 프로그램에서 지문 인증을 사용 하지 않을 수도 있습니다. 이러한 이유로 Android 응용 프로그램은 사용자 이름 및 암호와 같은 대체 인증 프로세스를 제공 해야 합니다.
3. **Google의 지문 아이콘 사용** &ndash; 모든 응용 프로그램은 Google에서 제공 하는 것과 동일한 지문 아이콘을 사용 해야 합니다. 표준 아이콘을 사용 하면 Android 사용자가 앱에서 지문 인증을 사용 하는 위치를 쉽게 인식할 수 있습니다. 
    
    ![Android 지문 아이콘](summary-images/ic-fp-40px.png)
    
4. **사용자에 게 알림** &ndash; 응용 프로그램은 지문 스캐너가 활성 상태이 고 터치 또는 살짝 밀기를 대기 하는 사용자에 게 일종의 알림을 표시 해야 합니다. 

## <a name="summary"></a>요약

지문 인증을 사용 하면 Xamarin Android 응용 프로그램에서 사용자를 신속 하 게 확인 하 여 앱에서 바로 구매와 같은 중요 한 기능을 보다 쉽게 조작할 수 있습니다. 이 가이드에서는 Xamarin Android 응용 프로그램에 Android 6.0 지문 API를 통합 하는 데 필요한 개념 및 코드에 대해 설명 했습니다.

먼저 지문 API 자체 `FingerprintManager` (및 `FingerprintManagerCompat`)에 대해 설명 했습니다. 응용 프로그램에서 추상 `FingerprintManager.AuthenticationCallbacks` 클래스를 확장 하 고 지문 하드웨어와 응용 프로그램 자체 사이에 중개자로 사용 하는 방법을 살펴보았습니다. 그런 다음 Java `Cipher` 개체를 사용 하 여 지문 스캐너 결과의 무결성을 확인 하는 방법을 살펴보았습니다. 마지막으로, 장치에 지문을 등록 하 고 **adb** 를 사용 하 여 에뮬레이터에서 지문 살짝 밀기를 시뮬레이션 하는 방법을 설명 하는 테스트에 대해 약간의 노력을 했습니다. 

아직 수행 하지 않은 경우이 가이드와 함께 제공 되는 [샘플 응용 프로그램](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) 을 확인 해야 합니다. [지문 대화 상자 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) 은 Java에서 xamarin.ios로 이식 되었으며 android 응용 프로그램에 지문 인증을 추가 하는 방법에 대 한 또 다른 예제를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [지문 대화 상자 샘플](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [지문 아이콘](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
