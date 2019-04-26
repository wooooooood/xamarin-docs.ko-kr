---
title: WatchOS에서 Xamarin 탐색 사용
description: 이 문서에서는 watchOS 응용 프로그램에서 탐색을 사용 하는 방법을 설명 합니다. 모달 인터페이스, 계층적 탐색 및 페이지 기반 인터페이스에 설명 합니다.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 0f087e4ce8fac2d86d45b6a27dc00c3fe4ad18db
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61412746"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>WatchOS에서 Xamarin 탐색 사용

시계에서 사용할 수 있는 가장 간단한 탐색 옵션은 단순 [모달 팝업](#modal) 현재 장면 위에 표시 되는 합니다.

다중 장면 watch 앱에 있는 사용 가능한 두 가지 탐색 패러다임은:

- [계층적 탐색](#Hierarchical_Navigation)
- [페이지 기반 인터페이스](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>모달 인터페이스

사용 된 `PresentController` 인터페이스 컨트롤러를 모달 형식으로 열 수입니다. 인터페이스 컨트롤러에 이미 정의 되어 있어야 합니다 **Interface.storyboard**합니다.

```csharp
PresentController ("pageController","some context info");
```

컨트롤러 모달 형식으로 표시 되는 전체 화면 (이전 장면 포함)를 사용 합니다. 기본적으로 제목 설정 됩니다 **취소** 컨트롤러 해제를 탭 하 고 있습니다.

모달 형식으로 제공 된 컨트롤러를 프로그래밍 방식으로 닫으려면 호출 `DismissController`합니다.

```csharp
DismissController();
```

단일 장면 또는 페이지 기반 레이아웃을 사용 하 여 모달 화면 수 있습니다.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>계층적 탐색

통해 다시 탐색, 방식과 유사 하 게 될 수 있는 스택 처럼 장면 표시 `UINavigationController` iOS에서 작동 합니다. 백그라운드에서 탐색 스택으로 푸시되 고 (프로그래밍 방식으로 또는 사용자 선택) 팝 수 수 있습니다.

![](navigation-images/hierarchy-1.png "장면 탐색 스택으로 푸시 될 수 있는") ![](navigation-images/hierarchy-2.png "장면 탐색 스택에서 팝 될 수 있습니다")

IOS의 경우와 마찬가지로 왼쪽 가장자리 살짝 계층 탐색 스택 부모 컨트롤러를 다시 이동 합니다.

모두를 [WatchKitCatalog](https://developer.xamarin.com/samples/WatchKitCatalog) 하 고 [WatchTables](https://developer.xamarin.com/samples/WatchTables) 샘플 계층적 탐색이 포함 합니다.

### <a name="pushing-and-popping-in-code"></a>푸시 및 팝 코드

키트에는 과도 하 게 총비율 "탐색 컨트롤러" 필요 하지 않습니다 시청 iOS 않습니다-처럼 만들 푸시할 사용 하 여 컨트롤러를 `PushController` 메서드 및 탐색 스택에서 자동으로 생성 됩니다.

```csharp
PushController("secondPageController","some context info");
```

시계의 화면 포함 됩니다는 **다시** 사용 하 여 탐색 스택의 맨 왼쪽에 단추 장면을 프로그래밍 방식으로 제거할 수 있습니다 `PopController`합니다.

```csharp
PopController();
```

IOS를 사용 하 여 이기도 하므로 사용 하 여 탐색 스택의 루트를 반환할 수 `PopToRootController`입니다.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Segue를 사용 하 여

Segue 계층적 탐색을 정의 하는 스토리 보드의 내부 간에 만들 수 있습니다. 운영 체제 호출 대상 장면에 대 한 컨텍스트를 가져오려고 `GetContextForSegue` 새 인터페이스 컨트롤러를 초기화 합니다.

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

페이지 기반 인터페이스 살짝 왼쪽-오른쪽, 방식과 유사 하 게 `UIPageViewController` iOS에서 작동 합니다. 표시기 점은 표시할 현재 페이지를 표시할 화면 아래쪽을 따라 표시 됩니다.

![](navigation-images/paged-1.png "샘플의 첫 번째 페이지") ![](navigation-images/paged-2.png "샘플 두 번째 페이지") ![](navigation-images/paged-5.png "샘플 다섯 번째 페이지")


페이지 기반 인터페이스를 watch 앱에 대 한 기본 UI를 사용 하 여 `ReloadRootControllers` 배열 인터페이스 컨트롤러 및 컨텍스트를 사용 하 여:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

루트 없는 페이지 기반 컨트롤러를 제공할 수도 있습니다를 사용 하 여 `PresentController` 앱의 다른 내부 중입니다.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```



## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (샘플)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)
