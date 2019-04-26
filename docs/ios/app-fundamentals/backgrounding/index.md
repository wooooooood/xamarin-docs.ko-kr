---
title: Xamarin.iOS의 backgrounding
description: 백그라운드 처리 또는 backgrounding은 응용 프로그램이 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 백그라운드에서 작업을 수행할 수 있도록 프로세스입니다. 이 가이드는 iOS에서 처리 하는 백그라운드 소개 됩니다.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2018
ms.openlocfilehash: a4f5112b6e77ab6e00453c19c766d1e905df1144
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60946659"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS의 backgrounding

_백그라운드 처리 또는 backgrounding은 응용 프로그램이 다른 응용 프로그램이 포그라운드에서 실행 되는 동안 백그라운드에서 작업을 수행할 수 있도록 프로세스입니다. 이 가이드는 iOS에서 처리 하는 백그라운드 소개 됩니다._

모바일 응용 프로그램의 backgrounding 바탕 화면에서 멀티태스킹의 일반적인 개념 보다 근본적으로 다릅니다. 데스크톱 컴퓨터에 다양 한 리소스 화면 부동산, 기능 및 메모리를 포함 하 여 응용 프로그램에 사용할 수 있습니다. 응용 프로그램이 side-by-side-를 실행 하 고 고성능을 유지 하는 일을 할 되어 사용할 수 있습니다. 모바일 장치에서 리소스는 훨씬 더 제한 됩니다. 작은 화면에 둘 이상의 응용 프로그램을 표시 하기 어렵습니다 및 배터리를 소모는 최고 속도로 여러 응용 프로그램을 실행 합니다. Backgrounding 상수 사이의 타협점입니다 응용 프로그램 리소스를 잘 수행 하는 데 필요한 백그라운드 작업 실행을 제공 하 고 응답성이 뛰어난 foregrounded 응용 프로그램 및 장치를 유지 합니다. IOS 및 Android backgrounding에 대 한 프로 비전 없지만 매우 다른 방식으로 처리할 것입니다.

IOS의 backgrounding가 응용 프로그램 상태를 인식 및 앱의 앱 및 사용자 동작에 따라 백그라운드 상태 내부 및 외부로 이동 됩니다. iOS는 또한 라는 백그라운드 필요한 응용 프로그램을 알려진된 형식으로 운영 중요 한 작업을 완료 시간에 대 한 OS를 포함 하 여 백그라운드에서 실행 되도록 앱을 연결 하는 것에 대 한 몇 가지 옵션을 제공 및 지정 된 응용 프로그램의 콘텐츠를 새로 고침 간격입니다.

이 가이드 및 연습을 함께 제공 되는 경우 하겠습니다 백그라운드에서 응용 프로그램 작업을 수행 하는 방법을 알아봅니다. 주요 개념 및 모범 사례를 설명 하 고 백그라운드에서 위치 업데이트를 수신 하는 실제 앱을 만드는 안내 합니다.

## <a name="contents"></a>목차

1.  [iOS의 Backgrounding 소개](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [애플리케이션 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS Backgrounding 기술](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [연습: iOS의 Backgrounding](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Backgrounding 지침](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>요약

이 가이드에서는 iOS에서 백그라운드 처리를 수행 하는 여러 가지를 도입 했습니다. IOS 응용 프로그램 상태를 설명 하 고 iOS 응용 프로그램 수명 주기에서에서 재생 backgrounding 역할을 검사 합니다. 또한 개별 태스크 또는 전체 응용 프로그램이 iOS 백그라운드에서 작동할 수 등록 수 하는 방법을 알게 되었습니다. 마지막으로 백그라운드에서 업데이트를 수행 하는 응용 프로그램을 작성 하 여 iOS의 backgrounding의 이해를 강화 했습니다.



## <a name="related-links"></a>관련 링크

- [Android에서 backgrounding](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (샘플)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [위치 (샘플)](https://developer.xamarin.com/samples/monotouch/Location/)
- [간단한 백그라운드 전송 (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS 백그라운드 실행](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
