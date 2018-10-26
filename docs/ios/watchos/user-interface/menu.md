---
title: watchOS에서 Xamarin 메뉴 컨트롤 (Force Touch)
description: 이 문서에서는 Xamarin에서 watchOS force 터치 제스처를 사용 하는 방법을 설명 합니다. Force 터치에 응답 하는 방법에 설명 추가 메뉴 및 메뉴 항목을 변경 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 7696c820ab6fdf19bdef46db31061fb5914e6cf4
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109759"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS에서 Xamarin 메뉴 컨트롤 (Force Touch)

조사식 키트 watch 앱 화면에서 구현 되는 경우 메뉴를 트리거하는 Force 터치 제스처를 제공 합니다.

![](menu-images/menu.png "메뉴를 표시 하는 Apple Watch")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force 터치에 응답

경우는 `Menu` 사용자 메뉴를 표시할 Force Touch 수행 하는 경우는 인터페이스 컨트롤러에 대 한 구현 되었습니다. 화면 간단 하 게 애니메이션 효과가 없음 메뉴에 구현 된 경우는 다른 작업에서 발생 합니다.

Force 터치 화면에서 특정 요소와 연관 되지 않습니다. 인터페이스 컨트롤러를 연결할 수 있습니다만 하나의 메뉴 및 Force Touch 눌러 화면에서 발생 한 위치에 관계 없이 표시 됩니다.

1 및 4 메뉴 사이는 옵션을 표시할 수 있습니다.


## <a name="adding-a-menu"></a>메뉴 추가

A `Menu` 에 추가 되어야 합니다는 `InterfaceController` 디자인 타임에 스토리 보드에서. 인터페이스 컨트롤러에 메뉴 컨트롤을 끌 때 표시가 없습니다 visual 스토리 보드 미리 보기에 있지만 **메뉴** 에 표시 되는 **문서 개요** 패드:

![](menu-images/menu-action.png "디자인 타임에 메뉴 편집")

최대 네 가지 메뉴 항목이 메뉴 컨트롤에 추가할 수 있습니다. 구성할 수 있습니다 합니다 **속성** 채움 합니다. 다음 특성을 설정할 수 있습니다.

- 제목 및
- 사용자 지정 이미지 또는
- 시스템 이미지: 허용, 블록, 추가, 거부, 정보를 더 음소거, 일시 중지, 재생, 반복, 다시 시작 "," 공유 "," Shuffle "," 스피커 "," 휴지통입니다.

만들기는 `Action` 선택 하 여는 **이벤트** 의 섹션을 **속성** 패드 및 동작 메서드에 대 한 이름을 입력 합니다. 부분 메서드에 코드를 다음과 같이 하는 인터페이스 컨트롤러 클래스에서 구현 될 수 있음에 만들어집니다.

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>사용자 지정 이미지

IOS에서 탭 이미지와 마찬가지로 메뉴 항목 이미지는 불투명 패턴 배경을 통해 볼 수 있도록 하는 알파 채널을 사용 하 여 필요 합니다.

최상의 성능을 위해 watch 앱 프로젝트 (watch 앱 확장 프로젝트 아님)에 메뉴에 사용 되는 이미지를 추가 해야 합니다.


## <a name="changing-the-menu-items"></a>메뉴 항목을 변경합니다.

<!--
### Design Time Items

Menu items added the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>런타임에 추가

할 수 없습니다는 `Menu` 하지만 런타임 시 인터페이스 컨트롤러를 추가할 수의 컬렉션 `MenuItem`s *수* 프로그래밍 방식으로 변경할 수 있습니다.
사용 된 `AddMenuItem` 표시 된 것 처럼 메서드:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Xamarin.iOS 조사식 키트 API에서 현재 요구를 `selector` 에 대 한는 `AdMenuItem` 메서드를 다음과 같이 선언할 수:

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>런타임 시 제거

합니다 `ClearAllMenuItems` 모두 제거 하는 메서드를 호출할 수 있습니다 *프로그래밍 방식으로 추가* 메뉴 항목입니다.

스토리 보드에 구성 된 메뉴 항목을 지울 수 없습니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple 메뉴 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
