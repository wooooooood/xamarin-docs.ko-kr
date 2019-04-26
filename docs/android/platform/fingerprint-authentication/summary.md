---
title: 지문 인증 지침
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 535eabe07cb4f4d36e6a6f918b5717efcc99185d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61023559"
---
# <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

## <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

Android 6.0 관련 Api 지문 인증 개념을 살펴본를 지문 Api 사용에 대 한 몇 가지 일반적인 조언을 살펴보겠습니다.

1. **Android 지원 라이브러리 v4 호환성 Api를 사용 하 여** &ndash; 을 코드에서 API 검사를 제거 하 여 응용 프로그램 코드를 단순화 하 고 응용 프로그램 대상 장치 가장을 허용 합니다.
2. **그러나 지문 인증에 대 한 대안을 제공** &ndash; 지문 authentication는 사용자를 인증 하는 응용 프로그램에 대 한 훌륭한 빠른 방법, 가정할 수 없습니다는 것은 항상 작동 또는 사용할 수 있습니다. 지문 스캐너를 실패할 수 있습니다, 렌즈 아마도 더티 수, 사용자 장치가 지문 인증을 사용 하도록 구성 되지 않을 수 있습니다 하거나 지문이 이후 가능성이 있습니다. 사용자 응용 프로그램을 사용 하 여 지문 인증을 사용 하도록 하지 않을 수 이기도 합니다. 이러한 이유로 Android 응용 프로그램에 사용자 이름 및 암호와 같은 대체 인증 프로세스를 제공 해야 합니다.
3. **Google의 지문 아이콘을 사용 하 여** &ndash; 모든 응용 프로그램에는 Google에서 제공 되는 동일한 지문 아이콘을 사용 해야 합니다. 표준 아이콘을 사용 하면 Android 사용자가 앱에서 지문 인증 사용 되는 위치 인식 하기 쉬운: 
    
    ![Android 지문 아이콘](summary-images/ic-fp-40px.png)
    
4. **사용자에 게 알릴** &ndash; 응용 프로그램에는 일종의 지문 스캐너를 활성화 되어 있음을 사용자에 게 알림 표시 해야을 터치 하거나 안쪽으로 살짝 밀어 기다립니다. 

## <a name="summary"></a>요약

지문 인증에는 신속 하 게 쉽게 앱에서 바로 구매와 같은 중요 한 기능을 사용 하 여 상호 작용 하는 사용자에 대 한 사용자를 확인 하려면 Xamarin.Android 응용 프로그램을 허용 하는 좋은 방법입니다. 이 가이드에는 개념 및 API의 Xamarin.Android 응용 프로그램에 Android 6.0 지문을 통합 하는 데 필요한 코드를 설명 합니다.

API 자체의 지문을 먼저 설명한 `FingerprintManager` (및 `FingerprintManagerCompat`). 검사할 방법을 `FingerprintManager.AuthenticationCallbacks` 추상 클래스를 응용 프로그램에서 확장 하 고 지문 하드웨어 및 자체 응용 프로그램 간의 중개자로 사용 해야 합니다. Java를 사용 하 여 지문 스캐너 결과의 무결성을 확인 하는 방법을 검사한 다음 `Cipher` 개체입니다. 마지막으로 했습니다 touched 약간 지문을 장치를 등록 하는 방법을 설명 하 고 사용 하 여 테스트에 대 **adb** 지문 에뮬레이터에서 안쪽으로 살짝 밀어를 시뮬레이션할 수 있습니다. 

이미 않았다면 자세히 살펴보아야 합니다 [샘플 응용 프로그램](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) 이 가이드와 함께 제공 되는 합니다. 합니다 [지문 대화 샘플](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) xamarin.android Java에서 이식 되었습니다 및 지문 인증 Android 응용 프로그램을 추가 하는 방법은 다른 예제를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 앱](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [지문 대화 샘플](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [지문 아이콘](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)
