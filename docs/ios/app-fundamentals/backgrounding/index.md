---
title: Xamarin.ios의 Backgrounding
description: 백그라운드 처리 또는 backgrounding 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 응용 프로그램이 백그라운드에서 작업을 수행할 수 있도록 하는 프로세스입니다. 이 가이드는 iOS의 백그라운드 처리를 소개 하는 역할을 합니다.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: ee96288cee83e3a073da4e12aaa4332e38beb804
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649264"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.ios의 Backgrounding

_백그라운드 처리 또는 backgrounding 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 응용 프로그램이 백그라운드에서 작업을 수행할 수 있도록 하는 프로세스입니다. 이 가이드는 iOS의 백그라운드 처리를 소개 하는 역할을 합니다._

모바일 응용 프로그램의 Backgrounding는 데스크톱의 일반적인 멀티태스킹 개념과 근본적으로 다릅니다. 데스크톱 컴퓨터에는 화면 부동산, 전원 및 메모리를 포함 하 여 응용 프로그램에 사용할 수 있는 다양 한 리소스가 있습니다. 응용 프로그램은 동시에 실행 될 수 있으며 성능이 뛰어나고 사용할 수 있습니다. 모바일 장치에서 리소스는 훨씬 더 제한적입니다. 작은 화면에는 응용 프로그램을 두 개 이상 표시 하는 것이 어렵고 여러 응용 프로그램을 최대 속도로 실행 하면 배터리가 방전 됩니다. Backgrounding는 응용 프로그램에 필요한 백그라운드 작업을 실행 하는 데 필요한 리소스를 응용 프로그램에 제공 하는 것과 포그라운드 응용 프로그램 및 장치 응답성을 유지 하는 것 사이의 일관 된 절충입니다. IOS와 Android 모두 backgrounding에 대 한 규정이 있지만 매우 다양 한 방식으로 처리 합니다.

IOS에서 backgrounding는 응용 프로그램 상태로 인식 되며 앱과 사용자의 동작에 따라 백그라운드 상태에서 앱이 이동 합니다. 또한 iOS는 중요 한 작업을 완료 하 고, 알려진 백그라운드 필수 응용 프로그램의 형식으로 작동 하 고, 지정 된 응용 프로그램의 콘텐츠를 새로 고칠 수 있도록 OS에 요청을 포함 하 여 백그라운드에서 실행 되도록 앱을 연결 하는 몇 가지 옵션을 제공 합니다. 구간.

이 가이드와 함께 제공 되는 연습에서는 백그라운드에서 응용 프로그램 작업을 수행 하는 방법을 배울 예정입니다. 주요 개념 및 모범 사례를 살펴본 다음 백그라운드에서 위치 업데이트를 수신 하는 실제 앱을 만드는 과정을 단계별로 안내 합니다.

## <a name="contents"></a>목차

1.  [iOS의 Backgrounding 소개](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [애플리케이션 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS Backgrounding 기술](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [연습: iOS의 Backgrounding](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Backgrounding 지침](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>요약

이 가이드에서는 iOS에서 백그라운드 처리를 수행 하는 다양 한 방법을 소개 했습니다. Ios 응용 프로그램 상태를 검사 하 고 iOS 응용 프로그램 수명 주기에 backgrounding가 재생 되는 역할을 검사 했습니다. 또한 iOS의 백그라운드에서 작동 하도록 개별 작업 또는 전체 응용 프로그램을 등록 하는 방법을 배웠습니다. 마지막으로, 백그라운드에서 업데이트를 수행 하는 응용 프로그램을 빌드하여 iOS에서 backgrounding에 대 한 이해를 강화 합니다.



## <a name="related-links"></a>관련 링크

- [Android의 Backgrounding](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/lifecycledemo)
- [Location (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/location)
- [간단한 백그라운드 전송 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/simplebackgroundtransfer)
- [iOS 백그라운드 실행](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
