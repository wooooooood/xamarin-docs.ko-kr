---
title: TvOS 탐색 및 Xamarin에서 포커스 사용
description: 이 문서에서는 tvOS 앱 내부에서의 탐색을 제공 하 고 처리 하는 데 중점의 개념과 사용 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 69886a0da53d419a0c40bdf34f91d301c9efe504
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573718"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>TvOS 탐색 및 Xamarin에서 포커스 사용

_이 문서에서는 tvOS 앱 내부에서의 탐색을 제공 하 고 처리 하는 데 중점의 개념과 사용 방법을 설명 합니다._

이 문서에서는 tvOS 앱의 사용자 인터페이스에서 [탐색](#Navigation) 을 처리 하는 데 [중점](#Focus-and-Selection) 의 개념과이를 사용 하는 방법을 설명 합니다. 기본 제공 tvOS 탐색 컨트롤이 포커스, 강조 표시 및 선택 항목을 사용 하 여 tvOS 앱의 사용자 인터페이스 탐색을 제공 하는 방법을 살펴보겠습니다.

[![](navigation-focus-images/intro01.png "tvOS apps User Interface Navigation")](navigation-focus-images/intro01.png#lightbox)

다음으로, [시차](#Focus-and-Parallax) 및 *계층화 된 이미지* 에서 포커스를 사용 하 여 최종 사용자에 게 현재 탐색 상태를 시각적으로 파악할 수 있는 방법을 살펴보겠습니다.

마지막으로, 포커스, [포커스 업데이트](#Working-with-Focus-Updates), [포커스 가이드](#Working-with-Focus-Guides), [컬렉션에 포커스](#Working-with-Focus-in-Collections) [를 사용 하](#Working-with-Focus)고 TvOS 앱에서 이미지 보기에 대 한 [시차를 사용 하도록 설정](#enabling-parallax) 하는 방법을 살펴보겠습니다.

<a name="Navigation"></a>

## <a name="navigation"></a>탐색

TvOS 앱의 사용자는 iOS를 사용 하 여 장치 화면에서 이미지를 탭 하지만 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)을 사용 하 여 대화방에서 간접적으로 상호 작용 하지 않습니다. 앱의 사용자 인터페이스를 설계할 때 자연스럽 게 흐를 수 있도록 앱의 사용자 인터페이스를 설계할 때이를 염두에 두어야 하지만, 사용자가 Apple TV 환경에서 사용을 유지 합니다.

성공적인 tvOS 앱은 탐색 자체를 호출 하지 않고 응용 프로그램에서 제공 하는 데이터의 구조와 앱의 용도를 원활 하 게 지 원하는 방식으로 탐색을 구현 합니다. 사용자 인터페이스를 dominating 하거나 콘텐츠와 앱 사용자 환경에서 포커스를 그리지 않고도 자연스럽 고 친숙 하도록 탐색을 디자인 합니다.

[![](navigation-focus-images/nav01.png "The tvOS settings app")](navigation-focus-images/nav01.png#lightbox)

Apple TV를 사용 하는 동안 사용자는 일반적으로 지정 된 콘텐츠 집합을 제시 하는 누적 된 화면 집합을 탐색 합니다. 그리고 모든 새 화면에는 [단추](~/ios/tvos/user-interface/buttons.md), [탭 모음](~/ios/tvos/user-interface/tab-bars.md), 테이블, [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 또는 [분할 뷰와](~/ios/tvos/user-interface/split-views.md)같은 표준 UI 컨트롤을 사용 하 여 하나 이상의 콘텐츠 하위 화면이 나타날 수 있습니다.

사용자는 새로운 각 데이터 화면을 사용 하 여이 화면 스택으로 더 자세히 탐색 합니다. Siri 원격의 **메뉴** 단추를 사용 하면 스택에서 역방향으로 이동 하 여 이전 화면 또는 주 메뉴로 돌아갈 수 있습니다.

Apple은 tvOS 앱에 대 한 탐색을 설계할 때 다음 사항을 염두에 두어야 합니다.

- **콘텐츠를 빠르고 쉽게 찾을 수 있도록 탐색을 레이아웃** 합니다. 사용자는 최대한 적은 수의 탭, 클릭, swipes 앱 내에서 콘텐츠에 액세스 하려고 합니다. 탐색을 간소화 하 고 최소 개수의 화면으로 콘텐츠를 구성 합니다.
- **터치를 사용 하 여 유체 인터페이스 만들기** -가능한 적은 수의 제스처를 사용 하 여 사용자가 _포커스 가능한 항목_ 간을 최소한의 마찰으로 이동할 수 있는지 확인 합니다.
- **초점을 맞춘 디자인** -사용자가 대화방에서 콘텐츠를 상호 작용 하므로 Siri 원격을 사용 하 여 상호 작용 하기 전에 사용자 인터페이스 항목으로 포커스를 이동 해야 합니다. 사용자는 자신의 목표 달성을 위해 너무 많은 제스처를 필요로 하는 경우 앱을 사용 하지 않아도 됩니다.
- **메뉴 단추를 통해 역방향 탐색을 제공** 하 여 사용자가 Siri 원격의 **메뉴** 단추를 사용 하 여 뒤로 탐색할 수 있도록 하는 친숙 하 고 친숙 한 환경을 만듭니다. **메뉴** 단추를 누르면 항상 이전 화면으로 돌아가거나 앱의 주 메뉴로 돌아갑니다. 앱의 최상위 수준에서 **메뉴** 단추를 누르면 Apple TV 홈 화면으로 돌아옵니다.
- **일반적으로 뒤로 단추를 표시 하지 않습니다** . Siri 원격에서 **메뉴** 단추를 누를 때 화면 스택이 뒤로 이동 하기 때문에이 동작을 복제 하는 추가 컨트롤이 표시 되지 않기 때문입니다. 이 규칙의 예외는 **취소** 단추를 표시 해야 하는 소거식 작업 (예: 콘텐츠 삭제)이 포함 된 화면 또는 화면을 구매 하는 것입니다.
- **많은 컬렉션을 단일 화면에 표시** 합니다. 즉, Siri 원격은 제스처를 사용 하 여 많은 양의 콘텐츠 컬렉션을 빠르고 쉽게 이동할 수 있도록 설계 되었습니다. 앱이 포커스를 받을 수 있는 항목의 큰 컬렉션을 사용 하는 경우 사용자의 파트를 더 많이 탐색 해야 하는 여러 화면으로 분리 하는 대신 단일 화면으로 유지 하는 것이 좋습니다.
- **탐색에 표준 컨트롤** 을 다시 사용 하 여 가능한 한 간편 하 고 친숙 한 사용자 환경을 만들 수 있습니다 `UIKit` . 페이지 컨트롤, 탭 모음, 분할 된 컨트롤, 테이블 뷰, 컬렉션 보기, 응용 프로그램 탐색을 위한 분할 보기 등의 기본 제공 컨트롤을 사용할 수 있습니다. 사용자는 이러한 요소를 이미 잘 알고 있으므로 사용자는 앱을 탐색할 수 있습니다.
- **가로 콘텐츠 탐색 우선** -Apple TV의 특성으로 인해 Siri 원격의 왼쪽에서 오른쪽으로 살짝 밀기는 위쪽 및 아래쪽 보다 더 자연스럽 게 작동 합니다. 앱에 대 한 콘텐츠 레이아웃을 디자인할 때이 옵션을 고려 합니다.

<a name="Focus-and-Selection"></a>

## <a name="focus-and-selection"></a>포커스 및 선택

Apple TV에서 이미지, 단추 또는 기타 UI 요소는 현재 탐색의 대상인 경우 _포커스에 있는_ 것으로 간주 됩니다.

[![](navigation-focus-images/focus01.png "Focus and Selection example")](navigation-focus-images/focus01.png#lightbox)

사용자가 장치 터치 스크린의 요소와 직접 상호 작용 하는 iOS 장치와 달리 사용자는 Siri 원격을 사용 하 여 실내에서 tvOS 요소와 상호 작용 합니다. 이 사용자 상호 작용을 제공 하 고 처리 하기 위해 Apple TV는 _포커스_ 기반 모델을 사용 합니다.

[Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)에서 제스처 및 단추 누름을 사용 하면 사용자가 한 UI 요소에서 다른 UI 요소로 포커스를 이동 합니다. 포커스가 원하는 요소로 이동 하 고 나면 사용자가 클릭 하 여 콘텐츠를 선택 하거나 해당 요소로 표시 되는 작업을 활성화 합니다.

포커스가 변경 되 면 미세한 애니메이션 및 효과 (예: 이미지에 대 한 시차 효과)를 사용 하 여 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 합니다.

Apple에는 포커스 및 선택 작업에 대 한 다음과 같은 제안이 있습니다.

- **동작 효과를 위해 기본 제공 UI 컨트롤 사용** -및 포커스 API를 사용 하 여 `UIKit` 사용자 인터페이스에서 포커스 모델은 기본 동작 및 시각적 효과를 UI 요소에 자동으로 적용 합니다. 이를 통해 앱은 Apple TV 플랫폼의 사용자에 게 친숙 하 고 친숙 하며, 포커스를 받을 수 있는 항목 간에 유체 및 직관적인 이동이 가능 합니다.
- **예상 방향으로 포커스 이동** -Apple TV에서 거의 모든 요소에 간접 조작이 사용 됩니다. 예를 들어 사용자는 Siri 원격을 사용 하 여 포커스를 이동 하 고 시스템은 인터페이스를 자동으로 스크롤하여 현재 포커스가 있는 항목을 표시 합니다. 앱이 이러한 유형의 상호 작용을 구현 하는 경우 포커스는 사용자 제스처의 방향으로 이동 해야 합니다. 따라서 사용자가 Siri 원격 포커스에서 오른쪽으로 이동 해야 하는 경우에는 화면을 왼쪽으로 스크롤할 수 있는 오른쪽으로 이동 해야 합니다. 이 규칙의 한 가지 예외는 직접 조작을 사용 하는 전체 화면 항목입니다 (위로 살짝 밀거나 요소를 위로 이동).
- **포커스가 있는 항목이 분명 한지 확인** 합니다. 사용자가 AFARŠTINA에서 UI 요소와 상호 작용 하기 때문에 현재 포커스가 있는 항목의 의미는 중요 합니다. 일반적으로이는 기본 제공 요소에 의해 자동으로 처리 됩니다 `UIKit` . 사용자 지정 컨트롤의 경우 항목 크기 또는 그림자와 같은 기능을 사용 하 여 포커스를 표시 합니다.
- **시차를 사용 하** 여 집중 된 항목을 gentle에 대 한 실시간 이동의 Siri 원격 결과에 작은 원형 제스처로 만듭니다. 이 [시차 효과](#Focus-and-Parallax) 는 계층화 된 이미지에 기본 제공 되므로 사용자에 게 `UIKit` _Layered Images_ 포커스가 있는 항목에 대 한 연결을 제공 합니다.
- **적절 한 크기의 적절 한 크기의 항목을 만듭니다** . 큰 항목의 간격이 있는 큰 항목은 더 작은 항목 보다 더 쉽게 선택 하 고 탐색할 수 있습니다.
- **포커스 또는 포커스가 없는를 보이는 UI 요소 디자인** -일반적으로 Apple TV는 크기를 늘려서 포커스가 있는 항목을 나타냅니다. 앱의 UI 요소가 모든 프레젠테이션 크기에서 제대로 보이는지 확인 하 고 필요한 경우 더 큰 크기의 요소에 대 한 자산을 제공 합니다.
- **포커스 변경 내용 표시 유동적으로** 애니메이션을 사용 하 여 **집중** 된 항목과 **포커스가 없는** 상태 사이에서 부드러운 페이드를 표시 하 여 전환이 jarring 되지 않도록 합니다.
- 커서를 **표시 하지** 않습니다. 사용자가 화면 주위에서 커서를 이동 하지 않고 포커스를 사용 하 여 앱의 UI를 탐색 하는 것이 좋습니다. 사용자 인터페이스는 항상 포커스 모델을 사용 하 여 일관 된 사용자 환경을 제공 해야 합니다.

<a name="Working-with-Focus"></a>

### <a name="working-with-focus"></a>포커스 작업

포커스를 받을 수 있는 항목이 될 수 있는 사용자 지정 컨트롤을 만들려고 할 수 있습니다. 이 경우 속성을 재정의 `CanBecomeFocused` 하 고 `true` 를 반환 합니다. 그렇지 않으면을 반환 `false` 합니다. 예를 들면 다음과 같습니다.

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

언제 든 지 `Focused` 컨트롤의 속성을 사용 하 여 `UIKit` 현재 항목 인지 확인할 수 있습니다. `true`현재 UI 항목에 포커스가 있는 경우에는 그렇지 않습니다. 예를 들면 다음과 같습니다.

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

코드를 통해 포커스를 다른 UI 요소로 직접 이동할 수는 없지만 해당 속성을로 설정 하 여 화면이 로드 될 때 먼저 포커스를 가져오는 UI 요소를 지정할 수 있습니다 `PreferredFocusedView` `true` . 예를 들면 다음과 같습니다.

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates"></a>

### <a name="working-with-focus-updates"></a>포커스 업데이트 작업 

사용자가 한 UI 요소에서 다른 UI 요소 (예: Siri 원격의 제스처에 대 한 응답)로 이동 하는 경우 포커스 _업데이트 이벤트_ 는 포커스를 잃은 항목 및 포커스를 얻는 항목 모두에 전송 됩니다.

또는에서 상속 되는 사용자 지정 요소의 경우 `UIView` `UIViewController` , 포커스 업데이트 이벤트를 사용 하도록 여러 메서드를 재정의할 수 있습니다.

- **DidUpdateFocus** -이 메서드는 뷰가 포커스를 얻거나 잃을 때 언제 든 지 호출 됩니다.
- **Shouldupdatefocus** -이 방법을 사용 하 여 포커스를 이동할 수 있는 위치를 정의 합니다.

포커스 엔진이 포커스를 UI 요소로 다시 이동 하도록 요청 하려면 `PreferredFocusedView` `SetNeedsUpdateFocus` 뷰 컨트롤러의 메서드를 호출 합니다.

> [!IMPORTANT]
> 호출 `SetNeedsUpdateFocus` 중인 뷰 컨트롤러에 현재 포커스가 있는 뷰가 포함 된 경우에만를 호출 하면 효과가 있습니다.

<a name="Working-with-Focus-Guides"></a>

### <a name="working-with-focus-guides"></a>포커스 가이드 작업

TvOS에 기본 제공 되는 포커스 엔진은 가로 및 세로 모눈에 속하는 항목 간의 움직임을 처리 하는 데 유용 합니다. 일반적으로 인터페이스 디자인에 UI 요소를 추가 하는 것 보다 더 많은 작업을 수행 해야 하며, 포커스 엔진은 개발자 개입 없이 이러한 요소 간의 움직임을 자동으로 처리 합니다.

그러나 ui 디자인의 필요성 UI 요소는 가로 및 세로 모눈에 속하지 않으며 서로 대각선으로 표시 되기 때문에 액세스할 수 없는 경우가 있습니다. 이는 포커스 엔진이 UI 항목 간의 간단한 위쪽, 아래쪽, 왼쪽 및 오른쪽 이동을 처리 하도록 디자인 되었기 때문에 발생 합니다.

예를 들어 다음 UI 레이아웃을 사용 합니다.

 [![](navigation-focus-images/guide01.png "Working with Focus Guides example")](navigation-focus-images/guide01.png#lightbox)

**추가 정보** 단추는 **구매** 단추를 사용 하 여 가로 및 세로 모눈에 속하지 않으므로 사용자는 액세스할 수 없습니다. 그러나 포커스 엔진에 이동 힌트를 제공 하기 위해 _포커스 가이드_ 를 사용 하 여 쉽게 수정할 수 있습니다. 

포커스 가이드 ()는 포커스 엔진에 포커스를 이동할 수 있는 `UIFocusGuide` 뷰의 보이지 않는 영역을 노출 하므로 포커스를 다른 뷰로 리디렉션할 수 있습니다.

따라서 위에 제공 된 예제를 해결 하기 위해 다음 코드를 보기 컨트롤러에 추가 하 여 추가 **정보** 및 **구입** 단추 사이에 포커스 가이드를 만들 수 있습니다.

```csharp
public UIFocusGuide FocusGuide = new UIFocusGuide ();
...

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Add Focus Guide to layout
    View.AddLayoutGuide (FocusGuide);

    // Define Focus Guide that will allow the user to move
    // between the More Info and Buy buttons.
    FocusGuide.LeftAnchor.ConstraintEqualTo (BuyButton.LeftAnchor).Active = true;
    FocusGuide.TopAnchor.ConstraintEqualTo (MoreInfoButton.TopAnchor).Active = true;
    FocusGuide.WidthAnchor.ConstraintEqualTo (BuyButton.WidthAnchor).Active = true;
    FocusGuide.HeightAnchor.ConstraintEqualTo (MoreInfoButton.HeightAnchor).Active = true;
}
```

먼저 `UIFocusGuide` 메서드를 사용 하 여 새를 만들고 뷰의 레이아웃 안내선 컬렉션에 추가 합니다 `AddLayoutGuide` .

그런 다음, 포커스 가이드의 위쪽, 왼쪽, 너비 및 높이 앵커는 **추가 정보** 및 **구입** 단추를 기준으로 조정 되어 서로 배치 됩니다. 참조

[![](navigation-focus-images/guide02.png "Example Focus Guide")](navigation-focus-images/guide02.png#lightbox)

또한 속성을로 설정 하 여 새 제약 조건이 생성 될 때 활성화 되 고 있다는 점에 유의 해야 합니다 `Active` `true` .

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

새 포커스 가이드를 설정 하 고 뷰에 추가 하면 뷰 컨트롤러의 `DidUpdateFocus` 메서드를 재정의 하 고 다음 코드를 추가 하 여 추가 **정보** 및 **구입** 단추 사이를 이동할 수 있습니다.

```csharp
public override void DidUpdateFocus (UIFocusUpdateContext context, UIFocusAnimationCoordinator coordinator)
{
    base.DidUpdateFocus (context, coordinator);

    // Get next focusable item from context
    var nextFocusableItem = context.NextFocusedView;

    // Anything to process?
    if (nextFocusableItem == null) return;

    // Decide the next focusable item based on the current
    // item with focus
    if (nextFocusableItem == MoreInfoButton) {
        // Move from the More Info to Buy button
        FocusGuide.PreferredFocusedView = BuyButton;
    } else if (nextFocusableItem == BuyButton) {
        // Move from the Buy to the More Info button
        FocusGuide.PreferredFocusedView = MoreInfoButton;
    } else {
        // No valid move
        FocusGuide.PreferredFocusedView = null;
    }
}
```

첫째,이 코드는 `NextFocusedView` `UIFocusUpdateContext` ()에 전달 된의을 가져옵니다 `context` . 이 뷰가 이면 `null` 처리가 필요 하지 않으며 메서드가 종료 됩니다.

그런 다음 `nextFocusableItem` 이 평가 됩니다. **추가 정보** 또는 **구입** 단추와 일치 하는 경우 포커스 가이드의 속성을 사용 하 여 반대 단추에 포커스를 보냅니다 `PreferredFocusedView` . 예를 들면 다음과 같습니다.

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

두 단추가 모두 포커스 이동의 소스가 아닌 경우에는 `PreferredFocusedView` 속성이 지워집니다.

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections"></a>

### <a name="working-with-focus-in-collections"></a>컬렉션에서 포커스 사용

개별 항목을 또는에서 포커스를 받을 수 있는지 여부를 결정할 때 `UICollectionView` `UITableView` 또는의 메서드를 `UICollectionViewDelegate` 각각 재정의 `UITableViewDelegate` 합니다. 예를 들면 다음과 같습니다.

```csharp
public class CardHandDelegate : UICollectionViewDelegateFlowLayout
{
    ...
    public override bool CanFocusItem (UICollectionView collectionView, NSIndexPath indexPath)
    {
        if (indexPath == null) {
            return false;
        } else {
            var controller = collectionView as CardHandViewController;
            return !controller.Hand [indexPath.Row].IsFaceDown;
        }
    }
}
```

`CanFocusItem`메서드는 `true` 현재 항목이 포커스를 받을 수 있는 경우를 반환 하 고, 그렇지 않으면를 반환 `false` 합니다.

`UICollectionView`또는가 `UITableView` 포커스를 잃거나 다시 얻으면 마지막 항목으로 포커스를 복원 하도록 하려면 `RemembersLastFocusedIndexPath` 속성을로 설정 `true` 합니다.

<a name="Focus-and-Parallax"></a>

## <a name="focus-and-parallax"></a>포커스 및 시차

위에서 설명한 것 처럼 사용자 인터페이스 요소는 탐색 이벤트 중에 현재 항목 일 때 포커스를가지고 _있는_ 것으로 간주 됩니다. 일반적으로 항목에 포커스가 있으면 크기는 약간 늘어나고 그림자를 사용 하 여 시각적으로 상승 됩니다. 

사용자가 Siri 원격에서 천천히 원형 제스처를 수행 하면 포커스가 있는 항목이이 이동에 대 한 응답으로 실시간으로 설정 됩니다. Sway가 발생 하면 표면이 빛이 보이도록 표시 되는 이미지에 불 광택이 적용 됩니다. 지정 된 비활성 시간이 지나면 포커스를 벗어난 콘텐츠가 희미 하 고 포커스가 있는 항목이 훨씬 커집니다. 

이러한 효과는 함께 작동 하 여 TV 화면의 콘텐츠와 사용자가 소파에서 Apple TV와 상호 작용 하는 멘 탈 연결을 제공 합니다.

Apple TV에서이 시차 효과는 시스템 전체에 걸쳐 중심 항목에 3D 깊이 및 dynamics를 전달 하는 데 사용 됩니다. tvOS는 이러한 효과를 만들기 위해 이동 하 고 크기를 동적으로 조정 하는 일련의 투명 한 [계층화 된 이미지](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) 를 사용 합니다.

계층화 된 이미지는 tvOS 앱의 아이콘에 필요 하며 동적 인기 선반 콘텐츠에 대해 지원 됩니다. 필수는 아니지만 Apple은 앱 UI의 다른 포커스 가능 항목에서 계층화 된 이미지를 구현 하는 것을 강력 하 게 제안 합니다.

### <a name="enabling-parallax"></a>시차 사용

`UIImageView`컨트롤 (또는에서 상속 하는 모든 컨트롤 `UIImageView` )은 시차 효과를 자동으로 지원 합니다. 기본적으로이 지원은 사용 하지 않도록 설정 되어 있으므로 다음 코드를 사용 합니다.

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

이 속성을로 설정 하면 `true` 사용자가 선택 하 고 포커스를 선택할 때 이미지 뷰가 시차 효과를 자동으로 가져옵니다.

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 tvOS 앱의 사용자 인터페이스에서 탐색을 처리 하는 데 사용 되는 방법 및 포커스의 개념에 대해 설명 했습니다. 기본 제공 tvOS 탐색 컨트롤이 포커스, 강조 표시 및 선택 영역을 사용 하 여 탐색을 제공 하는 방법을 검토 합니다. 다음으로, 시차 및 계층화 된 이미지에서 포커스를 사용 하 여 최종 사용자에 게 현재 탐색 상태를 시각적으로 파악할 수 있는 방법을 살펴보았습니다. 마지막으로, 포커스, 포커스 업데이트, 컬렉션에 포커스를 사용 하 고 시차를 사용 하도록 설정 하는 작업을 검사 했습니다.

## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 가이드](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
