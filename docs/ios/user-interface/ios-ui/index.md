---
title: iOS의 사용자 인터페이스
description: 이 문서는 Xamarin.ios 앱에서 사용자 인터페이스를 빌드하는 방법을 설명 하는 가이드로 연결 됩니다. 연결 된 안내선은 모양 API, 사용자 인터페이스 개체 만들기, 레이아웃 옵션 등을 다룹니다.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/14/2017
ms.openlocfilehash: 954e3b8f612fd710dd178cfc296889c9da372183
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768315"
---
# <a name="user-interfaces-in-ios"></a>iOS의 사용자 인터페이스

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[모양 API](introduction-to-the-appearance-api.md)

iOS에서는 UIAppearance Api를 사용 하 여 사용자 인터페이스 컨트롤의 많은 시각적 특성에 테마를 적용할 수 있습니다.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[사용자 인터페이스 개체 만들기](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple에서는 관련 기능을 Xamarin.ios 네임 스페이스와 관련 된 "프레임 워크"로 그룹화 합니다. `UIKit`는 iOS의 모든 사용자 인터페이스 컨트롤을 포함 하는 네임 스페이스입니다.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[레이아웃 옵션](~/ios/user-interface/ios-ui/layout-options.md)

뷰의 크기를 조정 하거나 회전할 때 레이아웃을 제어 하는 두 가지 메커니즘이 있습니다. 자동 크기 조정 및 자동 레이아웃.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)

이 문서에서는 iOS 10에서 제공 되는 새로운 유형의 햅 피드백 및 Xamarin.ios에서 구현 하는 방법에 대해 설명 합니다.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)

코드는 주 (또는 UI) 스레드의 사용자 인터페이스 컨트롤만 변경 해야 합니다. 다른 스레드에서 발생 한 모든 UI 업데이트 (예: 콜백 또는 백그라운드 스레드)가 화면에 렌더링 되지 않거나 충돌을 일으킬 수 있습니다.
