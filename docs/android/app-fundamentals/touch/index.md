---
title: 터치 및 제스처 Xamarin.Android에
description: 여러 오늘날의 장치에 터치 스크린이 빠르고 효율적으로 상호 작용할 수 장치와 자연스럽 고 직관적인 방식입니다. 이 상호 작용 간단한 터치 검색만 제한 되지 않습니다.-제스처도 사용할 수 있습니다. 예를 들어 핀치 확대/축소 제스처는 두 손가락 사용자 수 확대 / 축소 된 화면의 일부를 모으는 하 여이 매우 일반적인 예제입니다. 이 가이드를 터치를 검사 하 고 Android에서 제스처입니다.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3700f9c73fb58fefcdba7987c9931e385cd52d38
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/03/2018
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>터치 및 제스처 Xamarin.Android에

_여러 오늘날의 장치에 터치 스크린이 빠르고 효율적으로 상호 작용할 수 장치와 자연스럽 고 직관적인 방식입니다. 이 상호 작용 간단한 터치 검색만 제한 되지 않습니다.-제스처도 사용할 수 있습니다. 예를 들어 핀치 확대/축소 제스처는 두 손가락 사용자 수 확대 / 축소 된 화면의 일부를 모으는 하 여이 매우 일반적인 예제입니다. 이 가이드를 터치를 검사 하 고 Android에서 제스처입니다._

## <a name="touch-overview"></a>터치 개요

iOS 및 Android 터치를 처리 하는 방법에서 비슷합니다. 둘 다-화면에서 연락처의 점수-멀티 터치 및 제스처를 복잡 한 지원할 수 있습니다. 이 가이드는 일부의 개념을 유사성 뿐만 아니라 터치 구현의 particularities를 소개 하 고 두 플랫폼 모두에서 제스처입니다.

Android 사용 하 여 한 `MotionEvent` 터치 데이터 및 작업에 대 한 수신 대기 하도록 뷰 개체에 대 한 메서드를 캡슐화 하는 개체입니다.

터치 데이터를 캡처할 뿐만 아니라 iOS 및 Android 터치 제스처에의 패턴을 해석 하기 위한 방법을 제공 합니다. 이러한 제스처 인식기 이미지의 회전 또는 페이지의 순서 대로 같은 응용 프로그램별 명령을 해석 하 다시 사용할 수 있습니다. Android는 소수의으로 지원 되는 제스처를 손쉽게 쉬운 복잡 한 사용자 지정 제스처 추가 리소스를 제공 합니다.

Android 또는 iOS에서 작업 하는 복잡 한 터치 및 제스처 인식기 간의 선택 될 수 있습니다. 이 가이드는 일반적으로 기본 설정에 부여 해야 제스처 인식기 하는 것이 좋습니다. 제스처 인식기 고려 사항 및 효율적으로 캡슐화 separation 제공 하는 개별 클래스에 구현 됩니다. 따라서 쉽게 작성 된 코드의 양을 최소화 하는 서로 다른 뷰 간의 논리를 공유할 수 있습니다.

이 가이드에는 각 운영 체제에 대 한 유사한 형식을 따릅니다: 먼저, 플랫폼의 터치 Api 도입 되었으며 어떤 터치 상호 작용을 빌드할 foundation이 설명 됩니다. 그런 다음에 대해 살펴보기 제스처 인식기 – 세계 먼저 몇 가지 일반적인 제스처를 탐색 하 고 응용 프로그램에 대 한 사용자 지정 제스처 만들기 작업을 마무리 하 여 합니다. 마지막으로, 낮은 수준의 터치 추적 finger-paint 프로그램을 만드는 데 사용 하 여 개별 손가락을 추적 하는 방법을 볼 수 있습니다.

## <a name="sections"></a>섹션

-  [Android 터치](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [연습: Android에서 터치 사용](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [멀티 터치 추적](touch-tracking.md)

## <a name="summary"></a>요약

이 가이드에는 Android에서 터치를 검사 했습니다. 두 운영 체제에 대 한 터치 사용 하는 방법과 터치 이벤트에 응답 하는 방법을 배웠습니다 했습니다. 다음으로, 이전에 배운 것 제스처 및 제스처 인식기 중 일부에 대 한 몇 가지 일반적인 시나리오를 처리 하기 Android 및 iOS를 모두 제공 합니다. 사용자 지정 제스처를 만들고 응용 프로그램에서이 구현 하는 방법을 검사 했습니다. 작업에서 각 운영 체제에 대 한 개념 및 Api를 설명 하는 연습 하 고 개별 손가락을 추적 하는 방법도 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플)를 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
