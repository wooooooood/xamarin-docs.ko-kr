---
title: Xamarin.iOS에 backgrounding
description: 백그라운드 처리 또는 backgrounding은 응용 프로그램이 전경에 다른 응용 프로그램을 실행 하는 동안 백그라운드에서 작업을 수행할 수 있도록 하는 과정입니다. 이 가이드는 백그라운드 처리에 iOS에 대 한 기본적인 사용 됩니다.
ms.prod: xamarin
ms.assetid: F377440C-C5D9-4267-85D8-2C816E3A0300
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b22f3ef3276129f7f46c23cc1d06666f151f5ac4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783544"
---
# <a name="backgrounding-in-xamarinios"></a>Xamarin.iOS에 backgrounding

_백그라운드 처리 또는 backgrounding은 응용 프로그램이 전경에 다른 응용 프로그램을 실행 하는 동안 백그라운드에서 작업을 수행할 수 있도록 하는 과정입니다. 이 가이드는 백그라운드 처리에 iOS에 대 한 기본적인 사용 됩니다._

모바일 응용 프로그램에서 backgrounding 하는 것은 멀티태스킹 바탕 화면에서의 일반적인 개념 근본적으로 다릅니다. 데스크톱 컴퓨터에서 다양 한 화면 자원을, 기능 및 메모리를 포함 하 여 응용 프로그램에 사용할 수 있는 리소스 했습니다. 응용 프로그램은-나란히 실행 하 고 성능이 뛰어난 유지 수 및 사용할 수 있습니다. 모바일 장치에서 리소스는 훨씬 더 제한 되어 있습니다. 작은 화면에 둘 이상의 응용 프로그램을 표시 하는 것이 어렵습니다 및는 배터리 소비는 최고 속도로 여러 응용 프로그램을 실행 합니다. Backgrounding 상수 간 절충안입니다 응용 프로그램 리소스, 잘 수행 하는 데 필요한 백그라운드 작업을 실행할 수 발전 하 고 장치와 foregrounded 응용 프로그램 응답 성능을 유지 합니다. IOS 및 Android backgrounding를 나타내려면 프로 비전이 필요 하지만 매우 다른 방식으로 처리 될 합니다.

IOS에서 응용 프로그램 상태 backgrounding가 인식 하 고는 응용 프로그램의 성능과 사용자 동작에 따라 백그라운드 상태 내부 및 외부로 이동 하는 응용 프로그램. iOS는 또한 알려진된 배경 필요한 응용 프로그램의 유형으로 작동 하는 중요 한 작업을 완료 하는 시간에 대 한 OS 요청을 포함 하 여 백그라운드에서 실행 되도록 응용 프로그램 연결에 대 한 몇 가지 옵션을 제공 하 고 지정 된 응용 프로그램의 콘텐츠를 새로 고치 간격입니다.

이 가이드 및 약관과 연습에서는 백그라운드에서 응용 프로그램 작업을 수행 하는 방법을 배울 것 이며 합니다. 주요 개념 및 모범 사례를 설명 하 고 백그라운드에서 위치 업데이트를 수신 하는 실제 응용 프로그램을 만드는 과정 다음 단계를 진행 합니다.

## <a name="contents"></a>목차

1.  [iOS의 Backgrounding 소개](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md)
1.  [응용 프로그램 수명 주기 데모](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md)
1.  [iOS Backgrounding 기술](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md)
1.  [연습: iOS의 Backgrounding](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
1.  [iOS Backgrounding 지침](~/ios/app-fundamentals/backgrounding/ios-backgrounding-guidance.md)

## <a name="summary"></a>요약

이 가이드에서는 iOS에 대 한 백그라운드 처리를 수행 하는 데 필요한 여러 가지를 도입 되었습니다. IOS 응용 프로그램 상태를 검사 하 고 iOS 응용 프로그램 수명 주기에서 backgrounding 역할을 검사 합니다. 또한 개별 태스크 또는 iOS에 백그라운드에서 작동 하는 전체 응용 프로그램 등록 수 방법을 배웠습니다. 마지막으로 백그라운드에서 업데이트를 수행 하는 응용 프로그램을 구축 하 여 iOS에서 backgrounding 살펴본을 강화 했습니다.



## <a name="related-links"></a>관련 링크

- [Android에서 backgrounding](~/android/app-fundamentals/services/index.md)
- [LifecycleDemo (샘플)](https://developer.xamarin.com/samples/monotouch/LifecycleDemo/)
- [위치 (샘플)](https://developer.xamarin.com/samples/monotouch/Location/)
- [간단한 백그라운드 전송 (샘플)](https://developer.xamarin.com/samples/monotouch/SimpleBackgroundTransfer/)
- [iOS 백그라운드 실행](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html)
