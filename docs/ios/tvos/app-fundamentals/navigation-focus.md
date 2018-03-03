---
title: "탐색 및 포커스 작업"
description: "이 문서에서는 포커스 및을 나타내고 Xamarin.tvOS 앱 내에서 탐색을 처리 하는 방법의 개념을 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: DD72E95F-AE9B-47D2-B132-5FA5FBD8026E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 82151599b92094b816f4763c533ed7746db37920
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-navigation-and-focus"></a>탐색 및 포커스 작업

_이 문서에서는 포커스 및을 나타내고 Xamarin.tvOS 앱 내에서 탐색을 처리 하는 방법의 개념을 설명 합니다._


이 문서에서는의 개념을 설명 [포커스](#Focus-and-Selection) 처리 하는 방법 및 [탐색](#Navigation) Xamarin.tvOS 응용 프로그램의 사용자 인터페이스에 있습니다. 기본 제공 tvOS 탐색 컨트롤 Xamarin.tvOS 앱의 사용자 인터페이스 탐색을 제공 포커스, 강조 표시 및 선택 영역을 사용 방법을 검토 합니다.

[ ![](navigation-focus-images/intro01.png "사용자 인터페이스 탐색 tvOS 앱")](navigation-focus-images/intro01.png)

으로 포커스를 사용할 수 있는 방법을 살펴보겠습니다 다음에 살펴보겠습니다 [시차](#Focus-and-Parallax) 및 *계층화 된 이미지* 최종 사용자에 게 현재 탐색 상태에 대 한 시각적 단서 수 있도록 합니다.

작업에서 살펴보게 마지막으로, [포커스](#Working-with-Focus), [포커스 업데이트](#Working-with-Focus-Updates), [포커스 가이드](#Working-with-Focus-Guides), [컬렉션에 포커스를](#Working-with-Focus-in-Collections) 및 [ 시차를 사용 하도록 설정](#Enabling-Parallax) Xamarin.tvOS 앱의 이미지 뷰에 있습니다.

<a name="Navigation" />

## <a name="navigation"></a>탐색

Xamarin.tvOS 응용 프로그램의 사용자의 인터페이스를 직접으로 ios 여기서은 탭 하지 장치의 화면에 나타나는 이미지 하지만 직접에서 사용 하 여 대화방에서 상호 작용 하지 것입니다는 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote)합니다. 당연히 흐르는 것 아직 사용자가 Apple TV 경험에서 immersed 유지 되도록 응용 프로그램의 사용자 인터페이스를 디자인할 때이 점을 명심 해야 합니다.

성공적인 tvOS 앱 자체 탐색에 주의 호출 하지 않고 표시 하는 데이터의 구조와 응용 프로그램의 용도 원활 하 게 지원 되는 방식에서 탐색을 구현 합니다. 콘텐츠 및 응용 프로그램 사용자 경험에서 포커스를 그리거나 원형이 사용자 인터페이스 없이 자연스럽 고 친숙 한 느낌 되도록 탐색을 디자인 합니다.

[ ![](navigation-focus-images/nav01.png "TvOS 설정 앱")](navigation-focus-images/nav01.png)

하지만 일반적으로 Apple TV, 사용자를 사용 하 여 탐색 집합 화면을 통해 각 지정된 된 집합의 콘텐츠를 제공 합니다. 하나 이상의 하위 화면와 같은 표준 UI 컨트롤을 사용 하는 콘텐츠의 모든 새 화면 차례로 발생할 수 있습니다 [단추](~/ios/tvos/user-interface/buttons.md), [탭 막대](~/ios/tvos/user-interface/tab-bars.md), 테이블, [컬렉션 뷰](~/ios/tvos/user-interface/collection-views.md) 또는 [ 분할 뷰](~/ios/tvos/user-interface/split-views.md)합니다.

사용자는 데이터의 각 새로운 화면으로 화면이 스택에 깊은 탐색합니다. 사용 하 여는 **메뉴** 단추 Siri 원격 이동할 수 뒤로 돌아가서 이전 화면 하거나 주 메뉴 스택을 통해 합니다.

Apple 앱 tvOS에 대 한 탐색을 설계할 때 다음 염두에서에 두고 제안:

- **레이아웃 확인 찾기 콘텐츠 빠르고 편리한을 탐색** -탭, 가장 적은 수의 앱 내에서 콘텐츠에 액세스 하려면 사용자가을 최대한 천공 기와 합니다. 탐색을 단순화 하 고 화면의 최소 번호에 대 한 콘텐츠를 구성 합니다.
- **유체 인터페이스를 사용 하 여 Touch 만들기** -사용자 간에 이동할 수 있는지 확인 _포커스 항목_ 최소 마찰 적은 수의 가능한 제스처를 사용 하 여 사용 합니다.
- **염두에 포커스가 있는 디자인** -Siri 원격을 사용 하 여 상호 작용 하기 전에 사용자 인터페이스 항목으로 포커스를 이동 해야 하므로 사용자가 간에 콘텐츠 상호 작용 합니다. 목표 달성 하기가 너무 많은 제스처를 요구 하는 경우에 사용자 응용 프로그램으로 생각 수신 됩니다.
- **뒤로 탐색 메뉴 단추를 통해 제공** -사용자가 이전 버전과 Siri 원격을 사용 하 여 탐색할 수를 허용 하는 쉽고 친숙 한 환경을 제공 하도록 **메뉴** 단추입니다. 키를 누르면는 **메뉴** 단추 해야 항상 이전 화면으로 돌아가거나 응용 프로그램의 주 메뉴에 반환 합니다. 키를 눌러 응용 프로그램의 최상위 수준에 **메뉴** 단추 Apple TV 홈 화면으로 반환 해야 합니다.
- **뒤로 단추를 표시 하지 말고, 일반적으로** -키를 누르면는 **메뉴** Siri 원격 단추 화면 스택을 통해 뒤로 탐색,이 동작을 복제 하는 추가 컨트롤을 표시 하지 않도록 합니다. 이 규칙에 예외는 화면 또는 화면 (예: 콘텐츠를 삭제) 파괴적인 동작이 포함 된 구매에 대 한 여기서는 **취소** 단추가 표시 됩니다.
- **대신 대부분의 단일 화면에 광범위 한 모음을 표시** -Siri 원격 빠른 콘텐츠의 대규모 컬렉션을 통해 이동 하 고 제스처를 사용 하 여 손쉽게 하도록 설계 되었습니다. 앱 대규모 포커스를 받을 수 있는 항목의 컬렉션을 사용 하는 경우에 사용자의 부분에 더 많은 탐색 해야 하는 많은 화면으로 해제 하는 대신 단일 화면에 보관 하는 것이 좋습니다.
- **표준 컨트롤을 사용 하 여 탐색을 위한** -를 가능한 된 친숙 하 고 간편 사용자 경험을 만들려는 사용 하 여 기본 제공 `UIKit` 페이지 컨트롤, 탭 막대, 세그먼트 컨트롤, 표 보기, 컬렉션 뷰 및 분할 등의 컨트롤 응용 프로그램의 탐색에 대 한 보기를 제공 합니다. 사용자의 이러한 요소에 잘 알고 이미 있으므로 앱을 탐색할 수 직관적으로.
- **가로 콘텐츠 탐색을 우선시** -Apple TV의 특성상 넘기기가 Siri 원격 왼쪽에서 오른쪽이 더 자연 보다 및 축소 합니다. 앱에 대 한 콘텐츠 레이아웃을 디자인 하는 경우이 옵션을 고려 합니다.

<a name="Focus-and-Selection" />

## <a name="focus-and-selection"></a>포커스 및 선택

Apple tv, 이미지, 단추 또는 다른 UI 요소 것으로 간주 됩니다 _포커스가_ 때 현재 탐색의 대상입니다.

[ ![](navigation-focus-images/focus01.png "포커스 및 선택 예")](navigation-focus-images/focus01.png)

여기서는 사용자가 직접 장치의 터치 스크린에 요소가 상호 작용 하는 iOS 장치와 달리 사용자 간에 Siri 원격을 사용 하 여에서 tvOS 요소와 상호 작용 합니다. Apple TV 시키며이 사용자 상호 작용 처리를 사용 하 여 한 _포커스_ 기반 모델입니다.

누르는 제스처 및 단추를 사용 하는 [Siri 원격](~/ios/tvos/platform/remote-bluetooth.md#The-Siri-Remote), 사용자를 다른 UI 요소에서 포커스를 이동 합니다. 포커스에서 원하는 요소를 이동한, 되 면 사용자가 콘텐츠를 선택 하거나 해당 요소에서 나타내는 작업을 활성화 하려면 클릭 합니다.

포커스 변경에 따라 미묘한 애니메이션 및 효과 (예: 이미지에 시차 효과) 현재 포커스가 있는 사용자 인터페이스 항목을 명확 하 게 식별 하는 데 사용 됩니다.

Apple에 포커스 및 선택 영역을 사용 하려면 다음 제안 사항이 있습니다.

- **이동 효과 대 한 기본 제공 UI 컨트롤을 사용 하 여** -를 사용 하 여 `UIKit` 있고 사용자 인터페이스를 포커스 모델의 포커스가 API에서 자동으로 적용할 기본 동작 및 시각 효과 UI 요소에 있습니다. 이 네이티브 및 Apple TV 플랫폼의 사용자에 게 익숙한 생각 될 응용 프로그램을 사용 하면 고 포커스를 받을 수 있는 항목 간의 유동성 및 직관적인 이동을 허용 합니다.
- **예상 방향으로 포커스를 이동** -Apple TV에 거의 모든 요소에 간접 조작을 사용 하 여 합니다. 예를 들어, 포커스를 이동 하려면 Siri 원격을 사용 하 고 시스템을 계속 현재 포커스가 있는 항목을 표시 하려면 인터페이스를 자동으로 스크롤됩니다. 앱이이 종류의 상호 작용을 구현 하는 경우 사용자의 제스처의 방향으로 포커스가 이동 있는지 확인 합니다. 따라서 사용자에서는 오른쪽에서 안쪽으로 살짝 Siri 원격 포커스 (초래할 수 있습니다는 왼쪽으로 스크롤하여 화면) 오른쪽으로 이동 해야 합니다. 이 규칙의 한 가지 예외는 (여기서 넘기기가를 요소 위로 이동)을 직접 조작에 사용 하는 전체 화면 항목입니다.
- **포커스가 있는 항목 Obvious 인지 확인** -하므로 사용자가 이제부터 afar에 UI 요소와 상호 작용 하는 것이 현재 포커스가 있는 항목 둘러 중요 합니다. 일반적으로이에서 처리 되는 자동으로 기본 제공 `UIKit` 요소입니다. 사용자 지정 컨트롤에 대 한 항목 크기 또는 그림자 등의 기능 사용 하 여 포커스를 표시 합니다.
- **시차 확인 포커스가 있는 항목 응답을 사용 하 여** 크기가 작거나,-포커스가 있는 항목의 완만 걸쳐 실시간 이동에서 Siri 원격에 순환 제스처 발생 합니다. 이 [시차 효과](#Focus-and-Parallax) 변환은에 `UIKit` _계층화 이미지_ 의미 포커스가 있는 항목에는 연결의 사용자에 게 합니다.
- **적절 한 크기의 포커스를 받을 수 항목을 만들** -충분 한 간격으로 큰 항목을 선택 하 고 더 작은 항목 보다 탐색 하기가 보다 쉽습니다.
- **UI 요소를 찾을 것 중 하나 초점을 맞춘 또는 Unfocused 디자인** -일반적으로 Apple TV의 크기를 늘려 포커스가 있는 항목을 나타냅니다. 확인 앱의 UI 요소 프레젠테이션 크기로 훌륭해, 필요한 경우 더 큰 크기의 요소에 대 한 자산 제공.
- **유동적으로 변경 내용을 포커스를 나타내는** -원활 하는 항목 간의 페이드 애니메이션을 사용 하 여 **Focused** 및 **Unfocused** 되 고 전환을 jarring 유지 하는 상태입니다.
- **커서를 표시 하지 않습니다** -사용자가 포커스를 사용 하 여 응용 프로그램의 UI를 이동 하려는 화면 커서를 이동 하 여 되지 않습니다. 사용자 인터페이스 항상 일관 된 사용자 환경을 제공 포커스 모델을 사용 해야 합니다.

<a name="Working-with-Focus" />

### <a name="working-with-focus"></a>포커스가 있는 작업

포커스를 받을 수는 항목을 지정할 수 있는 사용자 지정 컨트롤을 만들려는 경우가 있을 수 있습니다. 경우 지금 재정의 `CanBecomeFocused` 속성 및 반환 `true`, 그렇지 않으면 반환 `false`합니다. 예:

```csharp
public class myView : UIView
{
    public override bool CanBecomeFocused {
        get {return true;}
    }
}
```

언제 든 지 사용할 수 있습니다는 `Focused` 의 속성을 `UIKit` 컨트롤을 현재 항목 인지 확인 합니다. 경우 `true` UI 항목은 현재 포커스를가지고, 그렇지 않으면 다른 합니다. 예:

```csharp
// Is my view in focus?
if (myView.Focused) {
    // Do something
    ...
}
```

직접 코드를 통해 다른 UI 요소에 포커스를 이동할 수 없습니다, 동안 어떤 UI 요소가 먼저 포커스 화면 설정 하 여 로드 될 때 지정할 수 있습니다는 `PreferredFocusedView` 속성을 `true`합니다. 예:

```csharp
// Make the play button the starting focus item
playButton.PreferredFocusedView = true;
```

<a name="Working-with-Focus-Updates" />

### <a name="working-with-focus-updates"></a>포커스 업데이트 작업 

사용자 (예를 들어 Siri 원격 제스처에 응답)에서 다른 UI 요소에서 이동할 포커스를 다시 하는 경우는 _포커스 업데이트 이벤트_ 포커스를 잃을 항목 및 포커스가 항목 모두에 전송 됩니다.

상속 하는 사용자 지정 요소에 대 한 `UIView` 또는 `UIViewController`, 포커스 업데이트 이벤트를 사용 하는 여러 메서드를 재정의할 수 있습니다.

- **DidUpdateFocus** -보기를 얻거나 포커스를 잃을 언제 든 지가이 메서드가 호출 됩니다.
- **ShouldUpdateFocus** -포커스를 이동할 수를 정의 하려면이 방법을 사용 합니다.

포커스 백업할 포커스 엔진 이동 하는 것을 요청 하는 `PreferredFocusedView` UI 요소를 호출은 `SetNeedsUpdateFocus` 보기 컨트롤러의 메서드.

> [!IMPORTANT]
> **참고:** 호출 `SetNeedsUpdateFocus` 뷰 컨트롤러에 대해 호출 되는 현재 포커스가 있는 보기를 포함 하는 경우에 영향을 주지 합니다.




<a name="Working-with-Focus-Guides" />

### <a name="working-with-focus-guides"></a>포커스 가이드로 작업

TvOS에 기본 제공 포커스 엔진은 처리 움직임 가로 및 세로 모눈에 있는 항목 사이 훌륭한입니다. 일반적으로 UI 요소를 사용자 인터페이스 디자인 및 포커스 엔진 개발자의 개입 없이 요소 간에 이동 처리할 자동으로 추가 하는 것 이상의 아무 작업도 수행 해야 합니다.

그러나 있을 수 있습니다 번 UI 디자인의 필요성 때문에 UI 요소 가로 및 세로 모눈에 해당 하지 않는 및 액세스할 수 없는 서로 대각선 되기 때문에 때. 이 포커스 엔진, 아래, 왼쪽 및 오른쪽으로 이동만 UI 항목 간의 간단한 처리 하도록 설계 된 때문에 발생 합니다.

예를 보려면 다음 UI 레이아웃을 수행 합니다.

 [ ![](navigation-focus-images/guide01.png "포커스 가이드 예제 사용")](navigation-focus-images/guide01.png)
 
때문에 **추가 정보** 단추와 가로 및 세로 모눈에 속하지는 **구입** 단추는 것이 사용자에 게 액세스할 수 있습니다. 그러나이 쉽게 해결할 수를 사용 하는 _포커스 가이드_ 포커스 엔진에 이동 힌트를 제공할 수 있습니다. 

포커스 가이드 (`UIFocusGuide`)으로 포커스를 다른 보기로 리디렉션할 수 있도록 포커스 엔진에 Focusable 보기의 표시 되지 않는 영역을 노출 합니다.

따라서 위의 예제를 해결 하기 위해 다음 코드 수에 추가할 간에 포커스 지침을 만드는 뷰 컨트롤러는 **추가 정보** 및 **구입** 단추:

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

먼저, 새 `UIFocusGuide` 를 만들고 사용 하 여 해당 뷰의 레이아웃 가이드 컬렉션에 추가 된 `AddLayoutGuide` 메서드.

포커스 가이드의 위쪽, 왼쪽, 너비 및 높이 앵커 기준으로 조정 하는 다음으로 **추가 정보** 및 **구입** 서로 배치 하는 단추입니다. 참조

[ ![](navigation-focus-images/guide02.png "예제 포커스 가이드")](navigation-focus-images/guide02.png)

설정 하 여 만들어지는 새 제약 조건을 활성화 해야 것도 해당 `Active` 속성을 `true`:

```csharp
FocusGuide.LeftAnchor.ConstraintEqualTo (...).Active = true;
```

설정 되 고 보기에 추가, 보기 컨트롤러의 새 포커스 가이드와 함께 `DidUpdateFocus` 사이 이동 하려면 다음 코드를 추가 하 고 메서드를 재정의할 수는 **추가 정보** 및 **구입** 단추:

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

먼저,이 코드 get는 `NextFocusedView` 에서 `UIFocusUpdateContext` 에 전달 되었습니다 (`context`). 이 보기는 `null`다음 없는 처리가 필요 하다 고 메서드 종료 되었습니다.

다음으로 `nextFocusableItem` 평가 됩니다. 일치 하는 **추가 정보** 또는 **구입** 단추, 포커스 포커스 가이드를 사용 하 여 다른 단추에 보내집니다 `PreferredFocusedView` 속성입니다. 예:

```csharp
// Move from the More Info to Buy button
FocusGuide.PreferredFocusedView = BuyButton;
```

에 두 단추 포커스가 shift의 원본인는 `PreferredFocusedView` 속성이 선택 취소 되어 있습니다.

```csharp
// No valid move
FocusGuide.PreferredFocusedView = null;
```

<a name="Working-with-Focus-in-Collections" />

### <a name="working-with-focus-in-collections"></a>컬렉션에 포커스가 있는 작업

개별 항목에 포커스를 받을 수 수 있는지 여부를 결정할 때는 `UICollectionView` 또는 `UITableView`의 메서드를 재정의 합니다는 `UICollectionViewDelegate` 또는 `UITableViewDelegate` 각각. 예:

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

`CanFocusItem` 메서드 반환 `true` 현재 항목에 포커스가 있을 수 있습니다, 그렇지 않으면 반환 `false`합니다.

원하는 경우는 `UICollectionView` 또는 `UITableView` 기억 하기 포커스 마지막 항목 때 복원 잃고 포커스를 다시 설정 된 `RemembersLastFocusedIndexPath` 속성을 `true`합니다.

<a name="Focus-and-Parallax" />

## <a name="focus-and-parallax"></a>포커스 및 시차

사용자 인터페이스 요소 것으로 간주 됩니다 위에서 설명한 대로 _포커스가_ 때 현재 항목 탐색 이벤트 중입니다. 크기가 약간 증가 하 고 시각적으로 권한 상승 그림자를 사용 하 여 항목에 포커스를 되 면 일반적으로. 

사용자 Siri 원격 느린, 순환 제스처를 만드는 경우 포커스가 있는 항목이 이동이에 대 한 응답으로 실시간 라 합니다. 거주 발생 조명된 광택을 표시 하는 화면을 만드는 해당 이미지에 적용 됩니다. 비활성가 주어진된 시간 후 포커스를 벗어난 내용이 흐리게 표시 하 고 Focused 항목도 커집니다. 

이러한 효과 TV 화면의 콘텐츠를와 소파에서 Apple TV의 사용자를 생각해 연결 제공 하기 위해 함께 작동 합니다.

Apple TV에 3D 깊이 및 포커스에 있는 항목을 dynamics 의미를 전달 하기 위해이 시차 효과 시스템 전체에서 사용 됩니다. 투명 하 게 사용 하는 tvOS [계층화 된 이미지](~/ios/tvos/app-fundamentals/icons-images.md#Layered-Images) 를 이동 하 고 이러한 효과를 만들려면 동적으로 조정 합니다.

계층화 된 이미지 tvOS 앱의 아이콘에 대 한 필요 하며 동적 위쪽 선반 콘텐츠에 대 한 지원입니다. 필수 하는 동안 Apple 것을 강력히 권장 응용 프로그램의 UI에 포커스를 받을 수 다른 항목의 계층화 된 이미지를 구현 합니다.

### <a name="enabling-parallax"></a>시차를 사용 하도록 설정

`UIImageView` 컨트롤 (또는 컨트롤에서 상속 되는 `UIImageView`) 시차 효과 자동으로 지원 합니다. 기본적으로이 지원은 사용 하도록 설정 하려면 다음 코드를 사용 하 여 해제 됩니다.

```csharp
myImageView.AdjustsImageWhenAncestorFocused = true;
```

이 속성 설정 하 여 `true`, 포커스 및 사용자가을 선택 하면 이미지 보기 시차 효과 자동으로 부여 됩니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 포커스 및 Xamarin.tvOS 응용 프로그램의 사용자 인터페이스에 대 한 탐색을 처리 하는 방법의 개념을 검사가 수행 됩니다. 기본 제공 tvOS 탐색 컨트롤 탐색을 제공 포커스, 강조 표시 및 선택 영역을 사용 방법을 검사 합니다. 다음으로, 그 최종 사용자에 게 현재 탐색 상태에 대 한 시각적 단서를 제공 하 시차 및 이미지 계층을 포커스에 사용할 수는 방법을 살펴보았습니다. 마지막으로, 작업에 포커스를 포커스 업데이트, 컬렉션 및 시차 활성화에 포커스를 검사합니다.




## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS 휴먼 인터페이스 지침](https://developer.apple.com/tvos/human-interface-guidelines/)
- [TvOS에 대 한 응용 프로그램 프로그래밍 가이드](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
