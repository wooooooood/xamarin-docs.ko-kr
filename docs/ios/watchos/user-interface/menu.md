---
title: watchOS Xamarin에서 Menu 컨트롤 (강제 터치)
description: 이 문서에서는 Xamarin의 watchOS force 터치 제스처를 사용 하는 방법을 설명 합니다. Force 터치에 응답 하는 방법에 설명 추가 메뉴 및 메뉴 항목을 변경 하는 방법입니다.
ms.prod: xamarin
ms.assetid: 5A7F83FB-9BC4-4812-92C5-CEC8DAE8211E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 4b973b925b99189416087224644c376864c56871
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791347"
---
# <a name="watchos-menu-control-force-touch-in-xamarin"></a>watchOS Xamarin에서 Menu 컨트롤 (강제 터치)

조사식 키트 watch 앱 화면에서 구현 되는 경우 메뉴를 트리거하는 Force 터치 제스처를 제공 합니다.

![](menu-images/menu.png "Apple Watch 메뉴 표시")
<!-- watch image courtesy of http://infinitapps.com/bezel/ -->

## <a name="responding-to-force-touch"></a>Force 터치에 응답

경우는 `Menu` 는 메뉴가 표시 됩니다 터치 힘을 수행할 때 인터페이스 컨트롤러에 대 한 구현 되었습니다. 화면에 애니메이션을 간단 하 게은 메뉴가 나타나지 구현 된 경우는 다른 작업에서 발생 합니다.

Force 터치 화면에 특정 요소와 연결 됩니다. 하나의 메뉴 인터페이스 컨트롤러에 연결할 수, Force 터치 키를 눌러 화면에서 발생 하는 위치에 관계 없이 표시 됩니다.

1 및 4 개 메뉴 사이는 옵션을 표시할 수 있습니다.


## <a name="adding-a-menu"></a>메뉴 추가

A `Menu` 에 추가 되어야 합니다는 `InterfaceController` 디자인 타임에 스토리 보드에 있습니다. 인터페이스 컨트롤러에 메뉴 컨트롤을 끌어 오면는 없는 시각적 스토리 보드 미리 보기에는 있지만 **메뉴** 에 표시는 **문서 개요** 패드:

![](menu-images/menu-action.png "디자인 타임에 메뉴 편집")

최대 4 개의 메뉴 항목은 메뉴 컨트롤에 추가할 수 있습니다. 구성할 수 있습니다는 **속성** 패드 합니다. 다음과 같은 특성을 설정할 수 있습니다.

- 제목 및
- 사용자 지정 이미지 또는
- 시스템 이미지: 수락, 추가, 블록, 거부, 정보, 아마도, 더 음소거, 일시 중지, 재생, Resume, 공유, 순서 섞기, 스피커, 휴지통 반복 합니다.

만들기는 `Action` 선택 하 여는 **이벤트** 섹션은 **속성** 패드를 동작 메서드에 대 한 이름을 입력 합니다. 부분 메서드는 다음과 같은 인터페이스 컨트롤러 클래스에서 구현 해야 코드에서 만들어집니다.

```csharp
partial void MenuItemTapped ()
{
    Console.WriteLine ("A menu item was tapped.");
}
```

### <a name="custom-images"></a>사용자 지정 이미지

IOS에서 탭 이미지와 마찬가지로, 메뉴 항목 이미지 알파 채널을 통해 표시 하도록 배경을 허용 하는 불투명 패턴이 필요 합니다.

최상의 성능을 위해 조사식 응용 프로그램 프로젝트 (하지 조사식 응용 프로그램 확장 프로젝트)에 메뉴에 사용 된 이미지를 추가 해야 합니다.


## <a name="changing-the-menu-items"></a>메뉴 항목 변경

<!--
### Design Time Items

Menu items added the the storyboard can be shown and hidden programmatically.
-->

### <a name="adding-at-runtime"></a>런타임에 추가

초래할 수는 `Menu` 하지만 런타임 시 인터페이스 컨트롤러에 추가할의 컬렉션 `MenuItem`s *수* 프로그래밍 방식으로 변경할 수 있습니다.
사용 하 여 `AddMenuItem` 표시 된 것 처럼 메서드:

```csharp
AddMenuItem (WKMenuItemIcon.Accept, "Yes", new ObjCRuntime.Selector ("tapped"));
```

Xamarin.iOS 조사식 키트 API를 사용 하려면 현재는 `selector` 에 대 한는 `AdMenuItem` 메서드를 다음과 같이 선언 해야 합니다.

```csharp
[Export("tapped")]
void MenuItemTapped ()
{
    Console.WriteLine ("The dynamically added 'Yes' menu item was tapped.");
}
```

### <a name="removing-at-runtime"></a>런타임 시 제거

`ClearAllMenuItems` 모두 제거 하는 메서드를 호출할 수 있습니다 *프로그래밍 방식으로 추가 된* 메뉴 항목입니다.

스토리 보드에 구성 된 메뉴 항목을 지울 수 없습니다.



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [Apple의 메뉴 문서](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Menus.html)
