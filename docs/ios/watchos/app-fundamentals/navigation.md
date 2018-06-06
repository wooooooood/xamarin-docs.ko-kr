---
title: WatchOS Xamarin에서 탐색 사용
description: 이 문서에서는 탐색 watchOS 응용 프로그램에서 작업 하는 방법을 설명 합니다. 모달 인터페이스, 계층적 탐색 및 페이지 기반 인터페이스에 설명 합니다.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: c9bcfc388164060549ca7010d11671abfa8230ac
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790642"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>WatchOS Xamarin에서 탐색 사용

시계에 사용할 수 있는 가장 간단한 탐색 옵션은 단순 [모달 팝업](#modal) 현재 장면 맨 위에 나타나는 합니다.

다중 장면 watch 앱 있습니다 사용할 수 있는 두 가지 탐색 패러다임은 다음과 같습니다.

- [계층적 탐색](#Hierarchical_Navigation)
- [페이지 기반 인터페이스](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>모달 인터페이스

사용 하 여는 `PresentController` 인터페이스 컨트롤러 모달 형식으로 열 수 있습니다. 인터페이스 컨트롤러에 이미 정의 되어 있어야는 **Interface.storyboard**합니다.

```csharp
PresentController ("pageController","some context info");
```

모달 형식으로 제공 된 컨트롤러 (이전 장면 담당할)는 전체 화면을 사용 합니다. 기본적으로 제목 설정 되어 **취소** 눌러서 컨트롤러를 해제 하 고 있습니다.

프로그래밍 방식으로 모달 형식으로 제공 된 컨트롤러를 종료 하려면 호출 `DismissController`합니다.

```csharp
DismissController();
```

모달 화면 단일 장면 또는 페이지 기반 레이아웃을 사용할 수 있습니다.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>계층적 탐색

표시를 통해 다시 탐색, 방식과 유사 하 게 될 수 있는 스택 처럼 장면 `UINavigationController` iOS에서 작동 합니다. 장면 수 탐색 스택에 밀어 넣은 있고 (프로그래밍 방식으로 또는 사용자 선택 하 여) 팝 합니다.

![](navigation-images/hierarchy-1.png "장면 탐색 스택으로 푸시 될 수 있는") ![ ] (navigation-images/hierarchy-2.png "장면 탐색 스택에서 팝 될 수 있습니다")

IOS의 경우와 마찬가지로 왼쪽 가장자리 살짝 계층 탐색 스택의 상위 컨트롤러를 다시 탐색 합니다.

두는 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) 및 [WatchTables](https://developer.xamarin.com/samples/WatchTables) 계층 탐색이 샘플에 포함 되어 있습니다.

### <a name="pushing-and-popping-in-code"></a>삽입 또는 코드에서 제거

키트에는 과도 하 게 설명할 "탐색 컨트롤러" 필요 하지 않습니다 시청 같이 iOS-만들 밀어 넣으면 사용 하 여 컨트롤러는 `PushController` 메서드 및 탐색 스택의 자동으로 만들어집니다.

```csharp
PushController("secondPageController","some context info");
```

시계의 화면 포함 됩니다는 **다시** 왼쪽, 위쪽에 단추 또한 프로그래밍 방식으로 사용 하 여 탐색 스택을에서 장면이 제거 `PopController`합니다.

```csharp
PopController();
```

와 마찬가지로 iOS, 또한를 사용 하 여 탐색 스택을의 루트에 반환할 수 `PopToRootController`합니다.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>사용 하 여 Segues

Segues 내부에서 계층적 탐색을 정의 하려면 스토리 보드 간에 만들 수 있습니다. 운영 체제 호출 대상 장면에 대 한 컨텍스트를 `GetContextForSegue` 새 인터페이스 컨트롤러를 초기화 합니다.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```
<a name="Page-Based_Interfaces"/>

## <a name="page-based-interfaces"></a>페이지 기반 인터페이스

페이지 기반 인터페이스 살짝 왼쪽-오른쪽, 방식과 유사 하 게 `UIPageViewController` iOS에서 작동 합니다. 표시기 점 현재 표시 되는 페이지를 표시 하려면 화면 아래쪽을 따라 표시 됩니다.

![](navigation-images/paged-1.png "샘플의 첫 번째 페이지") ![ ] (navigation-images/paged-2.png "샘플 두 번째 페이지") ![ ] (navigation-images/paged-5.png "샘플 다섯 번째 페이지")


페이지 기반 인터페이스를 watch 앱에 대 한 기본 UI를 만들려면 사용 `ReloadRootControllers` 인터페이스 컨트롤러 및 컨텍스트 배열을 사용 하 여:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

루트가 아닌 하는 페이지 기반 컨트롤러를 제공할 수도 있습니다를 사용 하 여 `PresentController` 응용 프로그램의 다른 장면이 중 하나입니다.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WatchTables (샘플)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchTables/)
