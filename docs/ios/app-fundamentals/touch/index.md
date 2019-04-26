---
title: Xamarin.iOS 앱에 터치를 처리합니다.
description: 이 문서는 터치, 멀티 터치, 제스처 및 Xamarin.iOS 앱에서 3D 터치를 사용 하는 방법을 설명 하는 가이드에 연결 합니다.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/23/2017
ms.openlocfilehash: 5aabc3a3c2ffbcffc0e12379989f7eb43b03a902
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61399293"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin.iOS 앱에 터치를 처리합니다.

다른 모바일 플랫폼을 같은 iOS에 터치를 처리 하는 방법의 수 멀티 터치를 지원할 수 있습니다-화면에서 연락처의 점수-및 복잡 한 제스처입니다. 이 가이드에는 개념 중 일부와 iOS에서 터치 및 제스처를 구현 하는 particularities 도입 되었습니다.

iOS에서 터치 데이터를 캡슐화 합니다 `UITouch` 일련의 응용 프로그램에 사용할 수 있는 클래스 `UIResponder` 메서드. 응용 프로그램의 서브 클래스에서 이러한 메서드를 재정의할 수 있습니다 `UIView` 하 고 `UIViewController`를 둘 다에서 상속 `UIResponder`합니다.

터치 데이터를 캡처할 뿐만 아니라 iOS의 터치 제스처에 대 한 패턴을 해석 하기 위한 수단을 제공 합니다. 이러한 제스처 인식기를 이미지의 회전을 페이지의 설정 등의 응용 프로그램별 명령을 해석에 사용할 수 있습니다. iOS 최소 추가한 코드를 사용 하 여 일반적인 제스처를 처리 하는 클래스의 다양 한 컬렉션을 제공 합니다.

터치 및 제스처 인식기를 선택할은 혼동 될 수 있습니다. 이 가이드는 일반적으로 기본 설정에 부여 해야 제스처 인식기 하는 것이 좋습니다. 제스처 인식기는 큰 영역 분리 및 캡슐화를 효율적으로 제공 하는 별도 클래스로 구현 됩니다. 이렇게 하면 간단 하 게 작성 된 코드의 양을 최소화 하는 다른 뷰 간에 논리를 공유 합니다.

그러나 하위 수준 터치 처리를 사용 하 고도 finger-paint 프로그램을 만드는 예를 들어 여러 손가락을 추적 해야 하는 경우가 있습니다.

## <a name="sections"></a>섹션

-  [iOS의 터치](touch-in-ios.md)
-  [연습: iOS에서 터치 사용](ios-touch-walkthrough.md)
-  [멀티 터치 추적](touch-tracking.md)

이 가이드는 iOS의 터치 소개 됩니다. 3D 터치 및 햅 틱 피드백을 사용 하 여 iOS에 대 한 자세한 내용은 iOS 9 및 10 각각 아래 제품별 지침을 참조 하십시오에 도입 된입니다.

* [3D Touch](~/ios/platform/3d-touch.md)
* [햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>관련 링크

- [iOS (샘플)를 시작 터치](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_start)
- [iOS 최종 Touch (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/Touch_final)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monotouch/ApplicationFundamentals/FingerPaint)
