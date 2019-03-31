---
title: TvOS 탐색 및 Xamarin에서 포커스를 사용 하 여 작업
description: 이 문서에서는 포커스 및 있고 Xamarin.tvOS 앱 내에서 탐색을 처리 하는 방법의 개념에 설명 합니다.
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3cb8d1c1d92146e70056c6cf562f2fa1cb028e7c
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58677874"
---
# <a name="working-with-tvos-navigation-and-focus-in-xamarin"></a>TvOS 탐색 및 Xamarin에서 포커스를 사용 하 여 작업

_이 문서에서는 포커스 및 있고 Xamarin.tvOS 앱 내에서 탐색을 처리 하는 방법의 개념에 설명 합니다._


이 문서에서는의 개념을 다룹니다 [포커스](#Focus-and-Selection) 처리 하는 방법 및 [탐색](#Navigation) Xamarin.tvOS 앱의 사용자 인터페이스에 있습니다. 기본 제공 tvOS 탐색 컨트롤 Xamarin.tvOS 앱의 사용자 인터페이스 탐색을 제공 포커스, 강조 표시 및 선택을 사용 하는 방법을 살펴보겠습니다.

[![](navigation-focus-images/intro01.png "tvOS 앱 사용자 인터페이스 탐색")](navigation-focus-images/intro01.png#lightbox)

다음으로 사용 하 여 포커스를 사용할 수 있는 방법을 살펴보겠습니다를 알아보겠습니다 [시차](#Focus-and-Parallax) 하 고 *계층화 된 이미지* 최종 사용자에 게 현재 탐색 상태에 대 한 시각적 단서를 제공 합니다.

마지막으로, 작업할 살펴보도록 하겠습니다 [포커스](#Working-with-Focus), [포커스 업데이트](#Working-with-Focus-Updates)를 [포커스 가이드](#Working-with-Focus-Guides)를 [컬렉션 포커스](#Working-with-Focus-in-Collections) 및 [ 시차를 사용 하도록 설정 하면](#enabling-parallax) Xamarin.tvOS 앱의 이미지 뷰에 있습니다.

<a name="Navigation" />

## <a name="navigation"></a>탐색

Xamarin.tvOS 앱의 사용자가 해당 인터페이스로 직접 ios를 탭 할 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 사용 하 여 대화방에서 상호 작용 하지 합니다 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)합니다. 물론 흐름 아직 Apple TV 환경을 사용 하는 사용자를 유지 하는 앱의 사용자 인터페이스를 디자인할 때이 점을 명심 해야 합니다.

성공적인 tvOS 앱을 원활 하 게 앱의 용도 및 자체 탐색에 주의 호출 하지 않고 표시 하는 데이터의 구조를 지 원하는 방식으로 탐색을 구현 합니다. 사용자 인터페이스를 원형이 그리기 콘텐츠 및 앱 사용자 경험에서 포커스 없이 자연스럽 고 친숙 한 느낌 탐색을 디자인 합니다.

[![](navigation-focus-images/nav01.png "TvOS 설정 앱")](navigation-focus-images/nav01.png#lightbox)

하지만 일반적으로 Apple TV, 사용자를 사용 하 여 이동 화면의 누적된 집합을 통해 각 콘텐츠의 지정 된 집합을 제공 합니다. 하나 이상의 하위 화면 콘텐츠 등의 표준 UI 컨트롤을 사용 하 여 모든 새 화면 차례로 않을 [단추](~/ios/tvos/user-interface/buttons.md)를 [탭 표시줄](~/ios/tvos/user-interface/tab-bars.md), 테이블 [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 또는 [ 분할 뷰](~/ios/tvos/user-interface/split-views.md)합니다.

사용자는 데이터의 각 새 화면을 사용 하 여 화면의이 스택에 심층적인가 이동합니다. 사용 하 여 합니다 **메뉴** 단추 Siri 원격에 이동할 수 있습니다 이전 버전과 이전 화면 또는 주 메뉴에 반환할 스택을 통해.

Apple 염두 다음 tvOS 앱에 대 한 탐색을 디자인할 때 제안 합니다.

- **레이아웃 확인 찾기 콘텐츠 빠르고 간편한을 탐색** -탭, 가장 적은 수의 앱 내에서 콘텐츠에 액세스 하려면 사용자가 클릭 하 고 최대한 천공 기와 합니다. 탐색을 간소화 하 고 화면의 최소 번호에 대 한 콘텐츠를 구성 합니다.
- **유체 인터페이스를 사용 하 여 Touch 만들기** -사용자 간에 이동할 수 있는지 확인 하십시오 _포커스 항목_ 최대한 적은 개수의 가능한 제스처를 사용 하 여 최소한의 마찰을 사용 하 여 합니다.
- **에 포커스가 있는 디자인** -Siri 원격을 사용 하 여 상호 작용 하기 전에 사용자 인터페이스 항목에 포커스를 이동 해야 하므로 사용자가 대화방에서 콘텐츠 상호 작용 합니다. 너무 많은 제스처 목표를 달성 하기 위해 필요한 경우 사용자 앱을 사용 하 여 불편을 받게 됩니다.
- **이전 버전과 탐색 메뉴 단추를 통해 제공할** -Siri 원격을 사용 하 여 뒤로 이동할 수 있도록를 쉽고 친숙 한 환경을 만드세요 **메뉴** 단추입니다. 키를 눌러 합니다 **메뉴** 단추는 항상 이전 화면으로 돌아간 또는 앱의 주 메뉴로 돌아갑니다. 키를 눌러 앱의 최상위 수준에 **메뉴** 단추 Apple TV 홈 화면에 반환 해야 합니다.
- **뒤로 단추를 표시 하지 말고에 일반적으로** -키를 눌러 합니다 **메뉴** Siri 원격 단추 화면 스택을 통해 뒤로 탐색,이 동작을 복제 하는 추가 컨트롤을 표시 하지 않도록 합니다. 이 규칙에 예외는 화면 또는 삭제 작업 (예: 콘텐츠 삭제)를 사용 하 여 화면을 구매 하는 것에 대 한 위치를 **취소** 도 단추를 표시 해야 합니다.
- **대규모 컬렉션 대신 여러 단일 화면에 표시** -Siri 원격 콘텐츠 빠른 큰 컬렉션을 이동 및 제스처를 사용 하 여 손쉽게 하도록 설계 되었습니다. 앱이 대규모 포커스 가능 항목의 컬렉션을 사용 하 여 작동 하는 경우에 사용자 입장에서 자세한 탐색 해야 하는 많은 화면으로 분할 하는 대신 단일 화면에 유지 하는 것이 좋습니다.
- **표준 컨트롤을 사용 하 여 탐색을 위한** -마찬가지로 가능한를 쉽고 친숙 한 사용자 환경을 만들려면 기본 제공을 사용 하 여 `UIKit` 페이지 컨트롤, 탭 표시줄, 분할 된 컨트롤, 표 보기, 컬렉션 뷰 및 분할 등의 컨트롤 앱의 탐색에 대 한 보기를 제공 합니다. 사용자가 이러한 요소를 사용 하 여 이미 익숙한, 이후 앱을 이동할 수 직관적으로 합니다.
- **가로 콘텐츠 탐색을 우선시** -Apple TV의 특성상 Siri 원격 왼쪽에서 오른쪽으로 살짝이 더 자연 스럽습니다 보다 및 축소 합니다. 앱에 대 한 콘텐츠 레이아웃을 디자인할 때이 옵션을 고려 합니다.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>포커스 및 선택

Apple tv, 이미지, 단추 또는 다른 UI 요소 것으로 간주 됩니다 _포커스가_ 현재 탐색 대상의 경우.

[![](navigation-focus-images/focus01.png "포커스 및 선택 예")](navigation-focus-images/focus01.png#lightbox)

여기서는 사용자가 직접 장치의 터치 스크린에서 요소 상호 작용 하는 iOS 장치와 달리 사용자 Siri 원격을 사용 하 여 대화방에서 tvOS 요소와 상호 작용 합니다. Apple TV에서는 있고이 사용자 상호 작용을 처리 하는 _포커스_ 기반 모델입니다.

누르는 제스처와 단추를 사용 하 여 합니다 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), 사용자에서 다른 UI 요소에서 포커스를 이동 합니다. 원하는 요소를 포커스에 이동 되 면 콘텐츠를 선택 하거나 해당 요소에서 나타내는 작업을 활성화 하려면 클릭 합니다.

포커스가 변경 될 때 미묘한 애니메이션과 부작용 (예: 이미지에 시차 적용) 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 하는 데 사용 됩니다.

Apple에 포커스 및 선택 작업을 위한 다음 제안에 있습니다.

- **기본 제공 UI 컨트롤 이동 효과 대 한 사용** -를 사용 하 여 `UIKit` 포커스 모델 사용자 인터페이스에서 포커스 API에 자동으로 적용할 기본 동작 및 시각 효과 UI 요소 및 합니다. 이 네이티브 및 Apple TV 플랫폼의 사용자에 게 익숙한 것으로 생각 될 앱을 사용 하면 고 포커스를 받을 수 있는 항목 간의 유연 하 고 직관적인 이동을 허용 합니다.
- **예상 방향으로 포커스를 이동** -Apple TV에서 거의 모든 요소 간접 조작을 사용 합니다. 예를 들어, 포커스를 이동 하려면 Siri 원격을 사용 하 고 시스템 현재 포커스가 있는 항목이 계속 표시 하는 인터페이스를 자동으로 스크롤합니다. 앱이이 종류의 상호 작용을 구현 하는 경우 포커스 이동 방향으로 사용자 제스처는 확인 합니다. 따라서 사용자 오른쪽에서 안쪽으로 살짝 Siri 원격 포커스 (야기할 수 왼쪽으로 스크롤하여 화면) 오른쪽으로 이동 해야 합니다. 이 규칙의 한 가지 예외는 (여기서 살짝 요소 위로 이동)을 직접 조작에 사용 하는 전체 화면 항목입니다.
- **포커스가 있는 항목이 Obvious 임을 확인** -afar에서 UI 요소를 사용 하 여 사용자 상호 작용 하는, 이므로 현재 포커스가 있는 항목이 돋 중요 합니다. 일반적으로이에서 처리 되는 자동으로 기본 제공 `UIKit` 요소입니다. 사용자 지정 컨트롤에 대 한 포커스를 표시 하려면 항목 크기 또는 그림자와 같은 기능을 사용 합니다.
- **시차 확인 초점을 맞춘 항목 응답을 사용 하 여** 작은-Siri 원격에 순환 제스처 발생 포커스가 있는 항목의 살짝, 실시간으로 이동 합니다. 이렇게 [시차 효과](#Focus-and-Parallax) 에 기본 제공 되 `UIKit` _계층화 된 이미지_ 포커스가 있는 항목에는 연결의 사용자에 게 합니다.
- **적절 한 크기의 포커스 가능 항목을 만들** -충분 한 간격을 사용 하 여 많은 항목은 쉽게 선택 하 고 더 작은 항목 보다 이동 합니다.
- **확인 좋은 중 초점을 맞춘 또는 Unfocused UI 요소 디자인** -Apple TV 일반적으로 해당 크기를 늘려 포커스가 있는 항목을 나타냅니다. 확인 앱의 UI 요소 모든 프레젠테이션 크기로 훌륭해, 필요한 경우 더 큰 크기의 요소에 대 한 자산을 제공 합니다.
- **포커스 변경 그날 나타냅니다** -애니메이션을 사용 하 여 원활 하 게 항목이 간의 페이드 **Focused** 및 **Unfocused** 상태를에서 전환 jarring 되도록 합니다.
- **커서를 표시 하지 않습니다** -포커스를 사용 하 여 앱의 UI를 이동 하고자 하는 사용자가 화면에서 이리저리 커서를 이동 하 여 되지 않습니다. 항상 사용자 인터페이스 일관 된 사용자 환경을 제공 하는 포커스 모델을 사용 해야 합니다.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>포커스가 있는 작업

포커스 가능 항목을 될 수 있는 사용자 지정 컨트롤을 만들려고 할 수 있는 시간 있을 수 있습니다. 경우 재정의 되므로 합니다 `CanBecomeFocused` 속성과 반환 `true`, 그렇지 않으면 반환 `false`합니다. 예를 들어:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

언제 든 지 사용할 수 있습니다 합니다 `Focused` 의 속성을 `UIKit` 컨트롤을 현재 항목 인지 확인 합니다. 경우 `true` UI 항목은 현재 포커스를가지고, 하지 않는 다른 합니다. 예를 들어:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

직접 코드를 통해 다른 UI 요소에 포커스를 이동할 수 없습니다, 하는 동안 UI 요소는 먼저 포커스 화면을 설정 하 여 로드 될 때 지정할 수 있습니다 해당 `PreferredFocusedView` 속성을 `true`입니다. 예를 들어:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>포커스 업데이트 작업 

사용자 (예를 들어, Siri 원격 제스처에 대 한 응답)에서 다른 UI 요소에서 이동할 포커스 발생 하는 경우는 _포커스 업데이트 이벤트_ 포커스를 잃는 항목 및 포커스가 항목 모두에 전송 됩니다.

상속 되는 사용자 지정 요소에 대 한 `UIView` 또는 `UIViewController`, 포커스 업데이트 이벤트를 사용 하는 여러 메서드를 재정의할 수 있습니다.

- **DidUpdateFocus** -뷰에 포커스를 얻거나 잃을 때마다가이 메서드가 호출 됩니다.
- **ShouldUpdateFocus** -포커스를 이동할 수 있는 정의 하려면이 메서드를 사용 합니다.

포커스를 포커스 엔진 이동 하는 것을 요청 하는 `PreferredFocusedView` UI 요소를 호출 합니다 `SetNeedsUpdateFocus` 뷰 컨트롤러의 메서드.

> [!IMPORTANT]
> 호출 `SetNeedsUpdateFocus` 뷰 컨트롤러에 대해 호출 되는 현재 포커스가 있는 뷰를 포함 하는 경우에 효과가 있습니다.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>포커스 가이드를 사용 하 여 작업

TvOS 내장 포커스 엔진은 가로 및 세로 모눈에 속하는 항목 간 이동을 처리 하는 데 유용 합니다. 일반적으로 사용자 인터페이스 디자인 및 포커스 엔진 UI 요소는 개발자의 개입 없이 이러한 요소 간의 이동 자동으로 처리를 추가 하는 것 이상의 아무 작업도 수행 해야 합니다.

그러나 있을 시간, UI 디자인의 필요성 때문에 UI 요소를 가로 및 세로 모눈에 해당 하지 않습니다 하 고 액세스할 수 없는 서로 대각선 이기 때문에. 포커스 엔진 아래로 UI 항목만 간의 왼쪽 및 오른쪽 이동, 간단한 처리 하도록 설계 된이 발생 합니다.

예를 들어 다음 UI 레이아웃을 수행 합니다.

 [![](navigation-focus-images/guide01.png "포커스 가이드 예제 작업")](navigation-focus-images/guide01.png#lightbox)
 
때문에 **추가 정보** 단추 사용 하 여 가로 및 세로 모눈에 속하지 않습니다 합니다 **구입** 단추 사용자에 게 액세스할 수 있는 것입니다. 그러나이 쉽게 수정할 수를 사용 하는 _포커스 가이드_ 포커스 엔진에 이동 힌트를 제공할 수 있습니다. 

포커스 가이드 (`UIFocusGuide`)으로 포커스를 다른 보기로 리디렉션할 수 있으므로 포커스 엔진으로 Focusable 뷰의 표시 되지 않는 영역을 노출 합니다.

따라서 위의 예제를 해결 하려면 다음 코드를 추가할 수 간의 포커스 지침을 만드는 뷰 컨트롤러는 **추가 정보** 하 고 **구입** 단추:

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

먼저, 새 `UIFocusGuide` 만들어지고 사용 하 여 뷰의 레이아웃 안내선 컬렉션에 추가 된 `AddLayoutGuide` 메서드.

포커스 가이드의 위쪽, 왼쪽, 너비 및 높이 앵커에 상대적인 조정 됩니다는 다음으로 **추가 정보** 하 고 **구입** 서로 위치로 단추. 참조

[![](navigation-focus-images/guide02.png "예제 포커스 가이드")](navigation-focus-images/guide02.png#lightbox)

새 제약 조건을 설정 하 여 만들 때 활성화 되는 것을 알아야 것도 해당 `Active` 속성을 `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

설정 및 보기에 추가, 보기 컨트롤러의 새 포커스 가이드 `DidUpdateFocus` 메서드를 재정의할 수 있습니다 및 사이 이동 하려면 다음 코드를 추가 합니다 **추가 정보** 및 **구입** 단추:

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

첫째,이 코드 가져오기 합니다 `NextFocusedView` 에서 합니다 `UIFocusUpdateContext` 에 전달 하는 (`context`). 이 보기는 `null`, 없습니다 처리가 필요 하다 고 메서드를 종료 합니다.

다음으로 `nextFocusableItem` 평가 됩니다. 일치 하는 **추가 정보** 또는 **구입** 단추 포커스가 포커스 가이드를 사용 하 여 반대 단추에 보내집니다 `PreferredFocusedView` 속성입니다. 예를 들어:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

모두 단추는 포커스 shift의 소스는 `PreferredFocusedView` 속성이 지워집니다.

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>컬렉션에 포커스가 있는 작업

개별 항목에 포커스를 받을 수 수 있는지 여부를 결정 하는 경우는 `UICollectionView` 또는 `UITableView`의 메서드를 재정의할 수는 `UICollectionViewDelegate` 또는 `UITableViewDelegate` 각각. 예를 들어:

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

합니다 `CanFocusItem` 메서드가 반환 `true` 포커스가 현재 항목 수, 하는 경우 다른 반환 `false`합니다.

하려는 경우는 `UICollectionView` 또는 `UITableView` 기억 하 고 포커스를 복원 하는 마지막 항목 잃고 포커스를 다시 얻은 경우에 설정 된 `RemembersLastFocusedIndexPath` 속성을 `true`입니다.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>포커스 및 시차

위에서 설명한 대로 사용자 인터페이스 요소를 것으로 간주 됩니다 _포커스가_ 때 현재 항목 탐색 중입니다. 크기가 약간 증가 하 고 시각적으로 권한 상승 그림자를 사용 하 여 항목에 포커스를 가져오면 일반적으로 합니다. 

사용자가 느린, 순환 제스처 Siri 원격 경우 포커스가 있는 항목이 이동이에 대 한 응답으로 실시간 sway 됩니다. sway 발생을 표시등이 광택 우수한 성능을 나타내는 표시 화면을 만드는 이미지에 적용 됩니다. 지정 된 시간이 지나면 비활성, 포커스를 벗어난 내용을 흐리게 표시 및 Focused 항목은 더 커집니다. 

이러한 효과 함께 작동 하는 TV 화면의 콘텐츠 사이의 소파에서 Apple TV와 상호 작용 하는 사용자 멘 탈 연결을 제공 합니다.

Apple tv, 3D 깊이 및 dynamics 포커스 내 항목의 의미를 전달 하기 위해이 시차 효과 시스템 전체에서 사용 됩니다. 투명 하 게 사용 하는 tvOS [계층화 된 이미지](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) 이동 하 고이 효과를 만드는 동적으로 크기를 조정 하는 합니다.

계층화 된 이미지 tvOS 앱 아이콘에 대 한 필요 하며 동적 인기 코너 콘텐츠에 대 한 지원 됩니다. 필수는 동안 Apple 것을 강력히 권장 앱 UI에서 포커스를 받을 수 다른 항목에서 계층화 된 이미지를 구현 합니다.

### <a name="enabling-parallax"></a>시차를 사용 하도록 설정

합니다 `UIImageView` 컨트롤 (또는 컨트롤에서 상속 되는 `UIImageView`) 자동으로 시차 효과 지원 합니다. 기본적으로이 지원은 사용 하도록 설정 하려면 다음 코드를 사용 하 여 비활성화 됩니다.

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

이 속성 설정 하 여 `true`, 포커스 및 사용자가 선택 될 때 이미지 보기 시차 효과 자동으로 부여 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 포커스 및 Xamarin.tvOS 앱의 사용자 인터페이스에서 탐색을 처리 하는 방법의 개념을 설명 했습니다. 이 기본 tvOS 탐색 컨트롤 탐색을 제공 포커스, 강조 표시 및 선택 영역을 사용 하는 방법을 검사 합니다. 다음으로 해당 포커스 사용할 수 있는 방법을 Parallax 및 계층화 된 이미지를 사용 하 여 최종 사용자에 게 현재 탐색 상태에 대 한 시각적 단서를 제공 하려면 살펴보았습니다. 마지막으로 포커스를 포커스 업데이트, 컬렉션 및 사용 하도록 설정 하면 Parallax의 포커스를 사용 하 여 작업을 조사 하 고 있습니다.




## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS 앱 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
