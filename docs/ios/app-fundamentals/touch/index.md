---
title: Xamarin.ios 앱에서 터치 처리
description: 이 문서는 Xamarin.ios 앱에서 터치, 멀티 터치, 제스처 및 3D 터치를 사용 하는 방법을 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: E3904713-6018-4755-A315-EB045DFB3500
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/23/2017
ms.openlocfilehash: 7aac9a3e2a86f5cdcfa2d6ab27961dd83998bb0f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009356"
---
# <a name="handling-touch-in-xamarinios-apps"></a>Xamarin.ios 앱에서 터치 처리

다른 모바일 플랫폼과 마찬가지로 iOS에는 터치를 처리 하는 다양 한 방법이 있습니다. 멀티 터치를 지원할 수 있습니다. 즉, 화면에서 많은 연락처 지점과 복잡 한 제스처가 있습니다. 이 가이드에서는 몇 가지 개념을 소개 하 고 iOS에서 터치 및 제스처를 구현 하는 particularities 합니다.

iOS는 일련의 `UIResponder` 메서드를 통해 응용 프로그램에서 사용할 수 있는 `UITouch` 클래스의 터치 데이터를 캡슐화 합니다. 응용 프로그램은 `UIView` 및 `UIViewController`의 서브 클래스에서 이러한 메서드를 재정의할 수 있으며, 둘 다 `UIResponder`에서 상속 됩니다.

IOS는 터치 데이터를 캡처할 뿐만 아니라 터치의 패턴을 제스처로 해석 하는 수단을 제공 합니다. 이러한 제스처 인식기를 사용 하 여 이미지 회전 또는 페이지 전환 같은 응용 프로그램 관련 명령을 해석할 수 있습니다. iOS는 최소한의 추가 코드를 사용 하 여 일반적인 제스처를 처리 하는 다양 한 클래스 컬렉션을 제공 합니다.

접촉 및 제스처 인식기 중에서 선택 하는 것이 혼동 될 수 있습니다. 이 가이드에서는 일반적으로 제스처 인식기에 대 한 기본 설정을 지정 하는 것이 좋습니다. 제스처 인식기는 중요 한 분리와 더 나은 캡슐화를 제공 하는 불연속 클래스로 구현 됩니다. 이렇게 하면 작성 되는 코드의 양을 최소화 하면서 여러 뷰 간에 논리를 쉽게 공유할 수 있습니다.

그러나 낮은 수준의 터치 처리를 사용 하 고 여러 손가락을 추적 해야 하는 경우가 있습니다. 예를 들어 손가락 그리기 프로그램을 만들어야 합니다.

## <a name="sections"></a>섹션

- [iOS의 터치](touch-in-ios.md)
- [연습: iOS에서 터치 사용](ios-touch-walkthrough.md)
- [멀티 터치 추적](touch-tracking.md)

이 가이드는 iOS의 터치에 대 한 소개를 제공 합니다. Ios에서 3D 터치 및 햅 피드백을 사용 하는 방법에 대 한 자세한 내용은 각각 iOS 9 및 10에 소개 되어 있습니다. 아래 특정 가이드를 참조 하세요.

- [3D Touch](~/ios/platform/3d-touch.md)
- [햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)

## <a name="related-links"></a>관련 링크

- [iOS Touch 시작 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-start)
- [iOS Touch Final (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
- [FingerPaint (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-fingerpaint)
