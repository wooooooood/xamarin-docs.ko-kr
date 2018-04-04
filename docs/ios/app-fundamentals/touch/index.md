---
title: 터치
description: 여러 오늘날의 장치에 터치 스크린이 빠르고 효율적으로 상호 작용할 수 장치와 자연스럽 고 직관적인 방식입니다. 이 상호 작용 간단한 터치 검색만 제한 되지 않습니다. – 제스처도 사용할 수 있습니다. 예를 들어 핀치 확대/축소 제스처는 두 손가락 사용자 수 확대 / 축소 된 화면의 일부를 모으는으로 매우 일반적인 예 –이입니다. 이 가이드는 터치 및 제스처 iOS에서 검사합니다.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/23/2017
ms.openlocfilehash: f34b502e3c0d67f33d41bc489f7ec1d93356af99
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="touch"></a>터치

_여러 오늘날의 장치에 터치 스크린이 빠르고 효율적으로 상호 작용할 수 장치와 자연스럽 고 직관적인 방식입니다. 이 상호 작용 간단한 터치 검색만 제한 되지 않습니다. – 제스처도 사용할 수 있습니다. 예를 들어 핀치 확대/축소 제스처는 두 손가락 사용자 수 확대 / 축소 된 화면의 일부를 모으는으로 매우 일반적인 예 –이입니다. 이 가이드는 터치 및 제스처 iOS에서 검사합니다._


다른 모바일 플랫폼에 같은 iOS 다양 한 터치를 처리 하는 방법에 있습니다. 멀티 터치를 지원할 수-화면에서 연락처의 점수-및 복잡 한 제스처입니다. 이 가이드는 개념 뿐 아니라 iOS 터치 및 제스처 구현의 particularities 소개 합니다.

iOS에 대 한 터치 데이터를 캡슐화는 `UITouch` 일련의 응용 프로그램에 사용할 수 있는 클래스 `UIResponder` 메서드. 응용 프로그램의 서브 클래스에서 이러한 메서드를 재정의할 수 `UIView` 및 `UIViewController`, 둘 다에서 상속 `UIResponder`합니다.

IOS는 터치 데이터를 캡처할 뿐만 아니라 터치 제스처에의 패턴을 해석 하기 위한 방법을 제공 합니다. 이러한 제스처 인식기 이미지의 회전 또는 페이지의 순서 대로 같은 응용 프로그램별 명령을 해석 하 다시 사용할 수 있습니다. iOS 다양 한 최소 추가 된 코드와 함께 일반적인 제스처를 처리 하는 클래스를 제공 합니다.

터치 및 제스처 인식기 간의 선택 혼란 스러운 일 수 있습니다. 이 가이드는 일반적으로 기본 설정에 부여 해야 제스처 인식기 하는 것이 좋습니다. 제스처 인식기 고려 사항 및 효율적으로 캡슐화 separation 제공 하는 개별 클래스에 구현 됩니다. 이것은 작성 된 코드의 양을 최소화 하는 여러 뷰 간의 논리를 공유할 간단 하 게 합니다.

그러나 하위 수준 터치 처리를 사용 하 고도 finger-paint 프로그램을 만드는 예를 들어 여러 손가락을 추적 해야 하는 경우가 있습니다.

## <a name="sections"></a>섹션

-  [iOS의 터치](touch-in-ios.md)
-  [연습: iOS에서 터치 사용](ios-touch-walkthrough.md)
-  [멀티 터치 추적](touch-tracking.md)

이 가이드에서 iOS 터치 소개로 사용 됩니다. 3D 터치 및 Haptic 피드백을 사용 하 여 iOS에 대 한 자세한 내용은 iOS 9 및 10 각각 아래의 특정 가이드를 참조 하십시오에 도입 된입니다.

* [3D Touch](~/ios/platform/3d-touch.md)
* [햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)



## <a name="related-links"></a>관련 링크

- [iOS (샘플)를 시작 터치](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS 최종 터치 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
