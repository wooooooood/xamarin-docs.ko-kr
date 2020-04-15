---
title: Xamarin.Forms 제스처
description: 이 가이드에서는 Xamarin.Forms 제스처 인식기를 사용하여 Xamarin.Forms 애플리케이션 보기와의 사용자 상호 작용을 감지하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 0E197A51-2304-4C09-A710-C7FF24A89F15
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2018
ms.openlocfilehash: 33968fb935e8b69736ac338bfa0479e4f278e64a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "61184575"
---
# <a name="xamarinforms-gestures"></a>Xamarin.Forms 제스처

_제스처 인식기를 사용하여 Xamarin.Forms 애플리케이션 보기와의 사용자 상호 작용을 감지할 수 있습니다._

Xamarin.Forms [`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) 클래스는 [`View`](xref:Xamarin.Forms.View) 인스턴스에서 탭하기, 손가락 모으기, 이동하기 및 살짝 밀기 제스처를 지원합니다.

## <a name="adding-a-tap-gesture-recognizer"></a>[탭 제스처 인식기 추가](tap.md)

탭 제스처는 탭을 감지하는 데 사용되며 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) 클래스로 인식됩니다.

## <a name="adding-a-pinch-gesture-recognizer"></a>[손가락 모으기 제스처 인식기 추가](pinch.md)

손가락 모으기 제스처는 대화형 확대/축소 작업을 수행하는 데 사용되며 [`PinchGestureRecognizer`](xref:Xamarin.Forms.PinchGestureRecognizer) 클래스로 인식됩니다.

## <a name="adding-a-pan-gesture-recognizer"></a>[이동 제스처 인식기 추가](pan.md)

이동 제스처는 화면에서 손가락의 움직임을 감지하고 이 움직임을 콘텐츠에 적용하는 데 사용되며 [`PanGestureRecognizer`](xref:Xamarin.Forms.PanGestureRecognizer) 클래스로 인식됩니다.

## <a name="adding-a-swipe-gesture-recognizer"></a>[살짝 밀기 제스처 인식기 추가](swipe.md)

살짝 밀기 제스처는 화면에서 손가락이 가로나 세로 방향으로 이동하는 경우 발생하며 콘텐츠에 대한 탐색을 시작하는 데 종종 사용됩니다. 살짝 밀기 제스처는 [`SwipeGestureRecognizer`](xref:Xamarin.Forms.SwipeGestureRecognizer) 클래스로 인식됩니다.
