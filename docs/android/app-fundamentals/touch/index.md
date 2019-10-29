---
title: Xamarin Android의 터치 및 제스처
description: 오늘날의 많은 장치에서 터치 스크린을 사용 하면 사용자가 쉽고 직관적인 방법으로 장치를 빠르고 효율적으로 조작할 수 있습니다. 이 상호 작용은 간단한 터치 검색 으로만 제한 되지 않으며 제스처도 사용할 수 있습니다. 예를 들어 확대/축소 제스처는 화면의 일부를 사용자가 확대 하거나 축소할 수 있는 두 손가락으로 집기 하 여이에 대 한 매우 일반적인 예입니다. 이 가이드는 Android의 터치 및 제스처를 검사 합니다.
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 43637d8592631b2732e5922544f52d91947dd3bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024290"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Xamarin Android의 터치 및 제스처

_오늘날의 많은 장치에서 터치 스크린을 사용 하면 사용자가 쉽고 직관적인 방법으로 장치를 빠르고 효율적으로 조작할 수 있습니다. 이 상호 작용은 간단한 터치 검색 으로만 제한 되지 않으며 제스처도 사용할 수 있습니다. 예를 들어 확대/축소 제스처는 화면의 일부를 사용자가 확대 하거나 축소할 수 있는 두 손가락으로 집기 하 여이에 대 한 매우 일반적인 예입니다. 이 가이드는 Android의 터치 및 제스처를 검사 합니다._

## <a name="touch-overview"></a>터치 개요

iOS와 Android는 터치를 처리 하는 방법과 비슷합니다. 둘 다 화면 및 복잡 한 제스처에서 여러 터치 접점을 지원할 수 있습니다. 이 가이드에서는 개념의 일부 유사성을 소개 하 고 두 플랫폼에서 터치 및 제스처를 구현 하는 particularities 합니다.

Android는 `MotionEvent` 개체를 사용 하 여 터치 데이터를 캡슐화 하 고 뷰 개체의 메서드를 사용 하 여 터치를 수신 대기 합니다.

터치 데이터를 캡처하는 것 외에도 iOS와 Android 모두 제스처의 패턴을 해석 하는 수단을 제공 합니다. 이러한 제스처 인식기를 사용 하 여 이미지 회전 또는 페이지 전환 같은 응용 프로그램 관련 명령을 해석할 수 있습니다. Android는 복잡 한 사용자 지정 제스처를 쉽게 추가할 수 있는 리소스 뿐만 아니라 지원 되는 몇 가지 제스처를 제공 합니다.

Android 또는 iOS에 대 한 작업을 수행 하는 경우 터치 및 제스처 인식기 중에서 선택 하는 것이 혼란 스 러 울 수 있습니다. 이 가이드에서는 일반적으로 제스처 인식기에 대 한 기본 설정을 지정 하는 것이 좋습니다. 제스처 인식기는 중요 한 분리와 더 나은 캡슐화를 제공 하는 불연속 클래스로 구현 됩니다. 이렇게 하면 작성 되는 코드의 양을 최소화 하면서 여러 보기 간에 논리를 쉽게 공유할 수 있습니다.

이 가이드는 각 운영 체제에 대해 비슷한 형식을 따릅니다. 먼저 플랫폼의 터치 Api가 터치 상호 작용을 빌드하는 기반 이므로 소개 하 고 설명 합니다. 그런 다음 몇 가지 일반적인 제스처를 탐색 하 고 응용 프로그램에 대 한 사용자 지정 제스처를 만들어 마무리 하는 제스처 인식기의 세계에 대해 알아봅니다. 마지막으로, 낮은 수준의 터치 추적을 사용 하 여 손가락 그리기 프로그램을 만드는 개별 손가락을 추적 하는 방법을 알아봅니다.

## <a name="sections"></a>섹션

- [Android 터치](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [연습: Android에서 Touch 사용](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [멀티 터치 추적](touch-tracking.md)

## <a name="summary"></a>요약

이 가이드에서는 Android에서 touch를 검사 했습니다. 두 운영 체제의 경우 터치를 사용 하도록 설정 하는 방법과 터치 이벤트에 응답 하는 방법을 배웠습니다. 다음으로 몇 가지 일반적인 시나리오를 처리 하기 위해 Android 및 iOS에서 제공 하는 제스처와 제스처에 대해 알아보았습니다. 사용자 지정 제스처를 만들어 응용 프로그램에서 구현 하는 방법을 살펴보았습니다. 연습에서는 각 운영 체제의 동작에 대 한 개념 및 Api를 보여 주고 개별 손가락을 추적 하는 방법도 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [Android Touch 시작 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Android Touch Final (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
- [FingerPaint (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)
