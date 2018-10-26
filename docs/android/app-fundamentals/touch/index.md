---
title: 터치 및 제스처를 xamarin.android
description: 여러 오늘날의 장치에서 터치 스크린에 빠르고 효율적으로 자연스럽 고 직관적인 방식으로 장치와 상호 작용할 수가 있습니다. 이 상호 작용 바로 간단한 터치 감지에 제한 되지 않습니다.-제스처도 사용할 수 있습니다. 예를 들어 축소 확대/축소 제스처가이 매우 일반적인 예로 사용자 수 확대 또는 축소 하는 두 손가락을 사용 하 여 화면의 밀기입니다. 이 가이드는 터치를 검사 하 고 Android에서 제스처입니다.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 7f957c9ff5a0e7c3a0821978703860ed2f723a92
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123325"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>터치 및 제스처를 xamarin.android

_여러 오늘날의 장치에서 터치 스크린에 빠르고 효율적으로 자연스럽 고 직관적인 방식으로 장치와 상호 작용할 수가 있습니다. 이 상호 작용 바로 간단한 터치 감지에 제한 되지 않습니다.-제스처도 사용할 수 있습니다. 예를 들어 축소 확대/축소 제스처가이 매우 일반적인 예로 사용자 수 확대 또는 축소 하는 두 손가락을 사용 하 여 화면의 밀기입니다. 이 가이드는 터치를 검사 하 고 Android에서 제스처입니다._

## <a name="touch-overview"></a>터치 개요

iOS 및 Android 터치를 처리 하는 방법에서 비슷합니다. 모두-화면에서 연락처의 여러 지점-멀티 터치 및 제스처를 복잡 한 지원할 수 있습니다. 이 가이드는 일부의 개념을 유사점 뿐만 아니라 터치 구현의 particularities를 소개 하 고 두 플랫폼 모두에서 제스처입니다.

Android 사용을 `MotionEvent` 터치 데이터 및 터치에 대 한 수신 대기 하도록 뷰 개체의 메서드를 캡슐화 하는 개체입니다.

터치 데이터를 캡처할 뿐만 아니라 iOS와 Android 터치 제스처에 패턴을 해석 하기 위한 방법을 제공 합니다. 이러한 제스처 인식기를 이미지의 회전을 페이지의 설정 등의 응용 프로그램별 명령을 해석에 사용할 수 있습니다. Android에는 몇 가지 리소스를 쉽게 복잡 한 사용자 지정 제스처 추가 뿐만 아니라 지원 되는 제스처를 제공 합니다.

Android 또는 iOS에서 작업할 혼동 단일 터치 및 제스처 인식기 간의 선택 될 수 있습니다. 이 가이드는 일반적으로 기본 설정에 부여 해야 제스처 인식기 하는 것이 좋습니다. 제스처 인식기는 큰 영역 분리 및 캡슐화를 효율적으로 제공 하는 별도 클래스로 구현 됩니다. 따라서 쉽게 작성 된 코드의 양을 최소화 하는 다른 뷰 사이의 논리를 공유할 수 있습니다.

이 가이드에는 각 운영 체제에 대 한 유사한 형식을 따릅니다: 첫째, 플랫폼의 터치 Api 도입 하 고 설명에 터치 상호 작용을 빌드할 때 기초로 같습니다. 그런 다음에 대해 살펴보기 – 제스처 인식기의 전 세계 먼저 몇 가지 일반적인 제스처를 탐색 하 고 완료 하는 응용 프로그램에 대 한 사용자 지정 제스처를 생성 하 여 합니다. 마지막으로, 하위 수준의 터치 추적을 사용 하 여 finger-paint 프로그램을 만드는 각 손가락을 추적 하는 방법을 배웁니다.

## <a name="sections"></a>섹션

-  [Android 터치](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [연습: Android 터치 사용](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
-  [멀티 터치 추적](touch-tracking.md)

## <a name="summary"></a>요약

이 가이드의 Android에서 터치를 검사 했습니다. 운영 체제 모두에 대 한 터치를 사용 하는 방법 및 터치 이벤트에 응답 하는 방법 학습 했습니다. 다음으로 알게 제스처 및 제스처 인식기의 일부에 대 한 iOS 및 Android 모두 제공 몇 가지 일반적인 시나리오를 처리 하는 합니다. 사용자 지정 제스처를 만들고 응용 프로그램에서이 구현 하는 방법을 살펴보았습니다. 작업에서 각 운영 체제에 대 한 개념 및 Api를 설명 하는 연습 및 각 손가락을 추적 하는 방법도 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [Android 터치 (샘플) 시작](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_start)
- [Android 터치 최종 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/Touch_final)
- [핑거 페인트 (샘플)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/FingerPaint)
