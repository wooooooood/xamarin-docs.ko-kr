---
title: IOS의 사용자 인터페이스
description: 이 문서는 Xamarin.iOS 앱에 대 한 사용자 인터페이스를 작성 하는 방법에 설명 하는 지침에 연결 합니다. 연결 된 가이드 모양을 API를 만들고 사용자 인터페이스 개체를 레이아웃 옵션을 자세히 설명 합니다.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: efb88ada8a4b4c36dd49de137eb64acd63552968
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115681"
---
# <a name="user-interfaces-in-ios"></a>IOS의 사용자 인터페이스

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[모양 API](introduction-to-the-appearance-api.md)

iOS는 UIAppearance Api를 사용 하 여 테마를 적용할 사용자 인터페이스 컨트롤의 시각적 특성을 많은 수 있습니다.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[사용자 인터페이스 개체 만들기](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple 그룹 관련된 부분 "프레임 워크" Xamarin.iOS 네임 스페이스에 상응 하는 기능입니다. `UIKit` iOS에 대 한 모든 사용자 인터페이스 컨트롤이 포함 된 네임 스페이스가입니다.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[레이아웃 옵션](~/ios/user-interface/ios-ui/layout-options.md)

메커니즘은 두 가지 다른 보기를 조정 하거나 회전 하는 경우 레이아웃을 제어 하기 위한: 자동 크기 조정 및 자동 레이아웃 합니다.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[햅틱 피드백 제공](~/ios/user-interface/ios-ui/haptic-feedback.md)

이 문서에서는 새로운 종류의 햅 틱 피드백 iOS 10과 Xamarin.iOS에서 구현 하는 방법에서 사용할 수 있습니다.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[UI 스레드 작업](~/ios/user-interface/ios-ui/ui-thread.md)

코드 변경 내용을 사용자 인터페이스 컨트롤 기본에서 (또는 UI) 스레드를 확인만 해야 합니다. UI 업데이트 (예: 콜백 또는 백그라운드 스레드) 다른 스레드에서 발생 하는 화면에 렌더링 되지 않을 수 있습니다 또는 충돌이 발생할 수 있습니다.




