---
title: 지문 인증 지침
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2b66c3660f6d8af9217089a7615784957fcc6ed7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

## <a name="fingerprint-authentication-guidance"></a>지문 인증 지침

개념을 살펴본 것 하 고 Android 6.0 둘러싼 Api 인증 지문, 지문 Api 사용에 대 한 몇 가지 일반적인 조언을 살펴보겠습니다.

1. **Android 지원 라이브러리 v4 호환성 Api를 사용 하 여** &ndash; 코드에서 API 검사를 제거 하 여 응용 프로그램 코드를 단순화 되 고 응용 프로그램이 가장 많은 장치 가능한 대상으로 하도록 허용 합니다.
2. **하지만 지문 인증에 대 한 대안을 제공** &ndash; 지문 인증은 사용자를 인증 하는 응용 프로그램에 대 한 빠른는 훌륭한 방법, 없습니다 가정를 항상 작동 아니면 사용할 수 있습니다. 지문 스캐너 실패할 수 있습니다, 렌즈 미정 더티 수, 사용자 장치 지문 인증을 사용 하도록 구성 되지 않을 수 있습니다 되었거나 지문이 모두 누락 수행한 이후 같습니다. 사용자 응용 프로그램과 함께 지문 인증을 사용 하도록 하지 않을 수 이기도 합니다. 이러한 이유로, Android 응용 프로그램 사용자 이름 및 암호와 같은 대체 인증 프로세스를 제공 해야 합니다.
3. **Google의 지문 아이콘을 사용 하 여** &ndash; 모든 응용 프로그램에는 Google에서 제공 하는 동일한 지문 아이콘 사용 해야 합니다. 표준 아이콘을 사용 하면 Android 사용자가 앱에서 지문 인증이 사용 되는 위치를 인식 합니다. 
    
    ![Android 지문 아이콘](summary-images/ic-fp-40px.png)
    
4. **사용자에 게** &ndash; 응용 프로그램에 일종의 지문 스캐너 활성 상태 인지, 사용자에 게 알림 표시 되어야 하 고 터치 또는 살짝 대기 합니다. 

## <a name="summary"></a>요약

지문 인증은 신속 하 게 확인할 사용자, 사용자가 앱에서 바로 구매와 같은 중요 한 기능을 사용 하도록 쉽게 Xamarin.Android 응용 프로그램이 허용 하는 좋은 방법입니다. 이 가이드의 개념 및 Android 6.0 지문 Xamarin.Android 응용 프로그램에서 API를 통합 하는 데 필요한 코드를 설명 합니다.

API 자체의 지문 먼저 설명한 `FingerprintManager` (및 `FingerprintManagerCompat`). 검사 했습니다 방법을 `FingerprintManager.AuthenticationCallbacks` 추상 클래스를 응용 프로그램에 의해 확장 하 고 지문 하드웨어와 자체 응용 프로그램 간의 중개자로 사용 해야 합니다. Java를 사용 하 여 지문 스캐너 결과의 무결성을 확인 하는 방법을 검사 했습니다 다음 `Cipher` 개체입니다. 마지막으로, 우리 다루었습니다 약간 지문 장치에 등록 하는 방법을 설명 하 고 사용 하 여 테스트 **adb** 에뮬레이터 지문 살짝을 시뮬레이션할 수 있습니다. 

살펴봐야 그렇게 이미 하지 않은 경우는 [샘플 응용 프로그램](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide) 이 가이드와 함께 제공 되는 합니다. [지문 대화 상자 샘플](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/) Xamarin.Android을 Java에서 이식 하 고 지문 인증 Android 응용 프로그램을 추가 하는 방법에 또 다른 예를 제공 합니다.



## <a name="related-links"></a>관련 링크

- [지문 가이드 샘플 응용 프로그램](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [지문 대화 샘플](https://developer.xamarin.com/samples/monodroid/android-m/FingerprintDialog/)
- [지문 아이콘](https://developer.android.comhttps://developer.xamarin.com/samples/FingerprintDialog/res/drawable-hdpi/ic_fp_40px.html)
