---
title: "IOS의 사용자 인터페이스"
description: "Xamarin.iOS 앱의 iOS 사용자 인터페이스 작업을 다룹니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f456b54180d50cfc4b6b98ed8f3d4118c8397b37
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="user-interface-in-ios"></a>IOS의 사용자 인터페이스

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[모양 API](introduction-to-the-appearance-api.md)

iOS는 UIAppearance Api를 사용 하 여 테마를 적용할 수를 사용자 인터페이스 컨트롤의 시각적 특성을 많은 수 있습니다.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[사용자 인터페이스 개체 만들기](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple 그룹 관련 부분에 "프레임 워크" Xamarin.iOS 네임 스페이스에 있는 기능을 합니다. `UIKit` iOS에 대 한 모든 사용자 인터페이스 컨트롤이 포함 된 네임 스페이스가입니다.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[레이아웃 옵션](~/ios/user-interface/ios-ui/layout-options.md)

보기를 조정 하거나 회전할 때 레이아웃을 제어 하기 위한 두 가지 다른 메커니즘을 없는: 자동 레이아웃 및 크기 자동 조정 합니다.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)

이 문서에서는 새로운 유형의 haptic 피드백 Xamarin.iOS에 구현 하는 방법과 iOS 10에서 사용할 수 있는 설명 합니다.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)

코드에는 사용자 인터페이스 컨트롤의 주에서 (또는 UI)에 대 한 변경 스레드만 수행 해야 합니다. UI 업데이트 (예: 콜백 또는 백그라운드 스레드)는 다른 스레드에서 발생 하는 화면에 렌더링 되지 않을 수 있습니다 또는 충돌이 발생할 수 있습니다.




