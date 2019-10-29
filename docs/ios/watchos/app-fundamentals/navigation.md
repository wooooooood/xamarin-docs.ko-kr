---
title: Xamarin에서 watchOS 탐색 사용
description: 이 문서에서는 watchOS 응용 프로그램에서 탐색 작업을 수행 하는 방법을 설명 합니다. 모달 인터페이스, 계층적 탐색 및 페이지 기반 인터페이스에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: d3565e359ccbad9f7b779969f4273a8cbae4d438
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021741"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Xamarin에서 watchOS 탐색 사용

Watch에서 사용할 수 있는 가장 간단한 탐색 옵션은 현재 장면 위에 표시 되는 간단한 [모달 팝업](#modal) 입니다.

다중 장면 조사식 응용 프로그램의 경우 다음과 같은 두 가지 탐색 패러다임을 사용할 수 있습니다.

- [계층적 탐색](#Hierarchical_Navigation)
- [페이지 기반 인터페이스](#Page-Based_Interfaces)

<a name="modal"/>

## <a name="modal-interfaces"></a>모달 인터페이스

인터페이스 컨트롤러를 모달로 열려면 `PresentController` 메서드를 사용 합니다. 인터페이스 컨트롤러는 이미 **인터페이스. storyboard**에 정의 되어 있어야 합니다.

```csharp
PresentController ("pageController","some context info");
```

모달로 제공 되는 컨트롤러는 전체 화면 (이전 장면 포함)을 사용 합니다. 기본적으로 제목은 **취소** 로 설정 되 고 탭을 누르면 컨트롤러가 해제 됩니다.

모달로 제공 되는 컨트롤러를 프로그래밍 방식으로 닫으려면 `DismissController`를 호출 합니다.

```csharp
DismissController();
```

모달 화면은 단일 장면 이거나 페이지 기반 레이아웃을 사용할 수 있습니다.

<a name="Hierarchical_Navigation"/>

## <a name="hierarchical-navigation"></a>계층적 탐색

는 iOS에서 `UINavigationController` 작동 하는 방식과 비슷하게 다시 탐색할 수 있는 스택 처럼 장면을 표시 합니다. 장면을 탐색 스택으로 푸시되 고, 프로그래밍 방식으로 또는 사용자 선택으로 팝 될 수 있습니다.

![](navigation-images/hierarchy-1.png "장면을 탐색 스택으로 푸시할 수 있습니다.") ![](navigation-images/hierarchy-2.png "탐색 스택에서 장면을 팝 할 수 있습니다.")

IOS와 마찬가지로 왼쪽에서 오른쪽으로 살짝 밀기는 계층 탐색 스택의 부모 컨트롤러로 다시 이동 합니다.

[WatchKitCatalog](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) 및 [WatchTables](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchtables) 샘플에는 모두 계층적 탐색이 포함 됩니다.

### <a name="pushing-and-popping-in-code"></a>코드 푸시 및 팝

Arching 메서드 `PushController`를 사용 하 여 컨트롤러를 푸시하는 것과 같이 감시 키트에는 오버 "탐색 컨트롤러"를 만들 필요가 없으며, 탐색 스택이 자동으로 만들어집니다.

```csharp
PushController("secondPageController","some context info");
```

조사식 화면의 왼쪽 위에 **뒤로** 단추가 포함 되지만 `PopController`를 사용 하 여 탐색 스택에서 장면을 프로그래밍 방식으로 제거할 수도 있습니다.

```csharp
PopController();
```

IOS와 마찬가지로 `PopToRootController`를 사용 하 여 탐색 스택의 루트로 돌아갈 수 있습니다.

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Segue 사용

Segue는 스토리 보드의 장면 사이에서 계층적 탐색을 정의 하는 데 사용할 수 있습니다. 대상 장면의 컨텍스트를 가져오기 위해 운영 체제는 `GetContextForSegue`를 호출 하 여 새 인터페이스 컨트롤러를 초기화 합니다.

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

페이지 기반 인터페이스는 `UIPageViewController` iOS에서 작동 하는 방식과 비슷하게 왼쪽에서 오른쪽으로 살짝 밉니다. 표시기 점이 화면 아래쪽에 표시 되어 현재 표시 되는 페이지를 표시 합니다.

![](navigation-images/paged-1.png "샘플 첫 페이지")![](navigation-images/paged-2.png "샘플 초 페이지")![](navigation-images/paged-5.png "샘플 5 페이지")

페이지 기반 인터페이스를 watch 앱에 대 한 기본 UI로 설정 하려면 인터페이스 컨트롤러 및 컨텍스트의 배열과 함께 `ReloadRootControllers`를 사용 합니다.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

응용 프로그램의 다른 장면 중 하나에서 `PresentController`를 사용 하 여 루트가 아닌 페이지 기반 컨트롤러를 제공할 수도 있습니다.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```

## <a name="related-links"></a>관련 링크

- [WatchKitCatalog (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WatchTables (샘플)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)
