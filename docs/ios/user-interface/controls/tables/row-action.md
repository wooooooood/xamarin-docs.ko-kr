---
title: Xamarin.iOS에서 행 작업
description: 이 가이드에 UISwipeActionsConfiguration 또는 UITableViewRowAction를 사용 하 여 테이블 행에 대 한 사용자 지정 안쪽으로 살짝 밀어 작업을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/25/2017
ms.openlocfilehash: 6d41f37d4a63db710bb04e35e6e1a4be0dd4f7a4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408283"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin.iOS에서 행 작업

_이 가이드에 UISwipeActionsConfiguration 또는 UITableViewRowAction를 사용 하 여 테이블 행에 대 한 사용자 지정 안쪽으로 살짝 밀어 작업을 만드는 방법을 보여 줍니다._

![행에 대 한 안쪽으로 살짝 밀어 작업을 보여 주는](row-action-images/action02.png)

iOS는 테이블에서 작업을 수행 하려면 두 가지 방법을 제공 합니다. `UISwipeActionsConfiguration` 고 `UITableViewRowAction`입니다.

`UISwipeActionsConfiguration` iOS 11에서에서 도입 되었으며의 집합을 정의 하는 경우 수행할 작업을 배치 사용자 천공 기와 _어느 방향으로든에서_ 테이블 뷰의 행에. 이 동작의 네이티브 Mail.app 비슷합니다. 

`UITableViewRowAction` 클래스 사용자 천공 기와 표 뷰의 행에 가로 방향으로 남아 있는 경우 발생 하는 동작을 정의 하는 데 사용 됩니다.
예를 들어, 표시 되는 경우 편집 살짝 행에는 왼쪽 테이블을 **삭제** 기본 단추입니다. 여러 인스턴스를 연결 하 여는 `UITableViewRowAction` 클래스는 `UITableView`, 여러 사용자 지정 작업을 정의할 수 있습니다, 자체 텍스트, 서식 및 동작을 사용 하 여 각 합니다.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

사용 하 여 살짝 밀기 동작을 구현 하는 데 필요한 3 단계가 `UISwipeActionsConfiguration`:

1. 재정의 `GetLeadingSwipeActionsConfiguration` 및/또는 `GetTrailingSwipeActionsConfiguration` 메서드. 이러한 메서드는 반환 된 `UISwipeActionsConfiguration`합니다. 
2. 인스턴스화하는 `UISwipeActionsConfiguration` 반환할 합니다. 이 클래스는 배열을 `UIContextualAction`합니다.
3. `UIContextualAction`를 만듭니다.

이러한 경우 다음 섹션에 자세히 설명 되어 있습니다.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. SwipeActionsConfigurations 메서드 구현

`UITableViewController` (그리고 `UITableViewSource` 하 고 `UITableViewDelegate`) 두 메서드가 포함 되어: `GetLeadingSwipeActionsConfiguration` 및 `GetTrailingSwipeActionsConfiguration`, 테이블 뷰 행 안쪽으로 살짝 밀어 작업 집합을 구현 하는 데 사용 되는 합니다. 선행 살짝 밀기 동작 왼쪽-오른쪽 언어로 화면 왼쪽에서 오른쪽에서 왼쪽 언어에서 화면의 오른쪽을 살짝 밀기를 가리킵니다. 

다음 예제에서는 (에서 합니다 [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) 샘플) 선행 안쪽으로 살짝 밀어 구성을 구현 하는 방법을 보여 줍니다. 두 작업은 상황에 맞는 작업에서 만들어진 [아래](#create-uicontextualaction)합니다. 이러한 작업은 다음에 전달 새로 초기화 [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), 반환 값으로 사용 됩니다.


```csharp
public override UISwipeActionsConfiguration GetLeadingSwipeActionsConfiguration(UITableView tableView, NSIndexPath indexPath)
{
    //UIContextualActions
    var definitionAction = ContextualDefinitionAction(indexPath.Row);
    var flagAction = ContextualFlagAction(indexPath.Row);

    //UISwipeActionsConfiguration
    var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction });
    
    leadingSwipe.PerformsFirstActionWithFullSwipe = false;
    
    return leadingSwipe;
}  
```

<a name="create-uiswipeactionsconfigurations" />

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. 인스턴스화하는 `UISwipeActionsConfiguration`

인스턴스화하는 `UISwipeActionsConfiguration` 를 사용 하 여는 `FromActions` 의 새 배열을 추가 하는 방법 `UIContextualAction`s, 다음 코드 조각과에서 같이:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

배열에 전달 되는 방식을에서 종속 작업의 표시 순서는 중요 한 것입니다. 예를 들어, 선행 천공 기와 대 한 위의 코드는 따라서으로 작업을 표시합니다.

![테이블 행에 표시 된 선행 살짝 밀기 동작](row-action-images/action03.png)

천공 기와 후행에 대 한 작업은 다음 이미지와 같이 표시 됩니다.

![테이블 행에 표시 된 후행 살짝 밀기 동작](row-action-images/action04.png)

이 코드 조각은 새 사용도 `PerformsFirstActionWithFullSwipe` 속성입니다. 기본적으로이 속성 설정 되는 배열의 첫 번째 작업 동작이 발생 하는지에 사용자 천공 기와 완전히 행 즉 true입니다. 삭제 된 작업을 사용 하는 경우 (예를 들어 "삭제" 또는 적합 하지 않을 하 고, 따라서로 설정 해야 `false`합니다.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>만들기를 `UIContextualAction`

상황에 맞는 작업이 실제로 사용자 swipes 테이블 행을 때 표시 되는 작업을 만듭니다.

제공 해야 하는 작업을 초기화 하는 `UIContextualActionStyle`, 제목 및 `UIContextualActionHandler`합니다. `UIContextualActionHandler` 3 개의 매개 변수: 작업, 작업, 표시 된 보기 및 완료 처리기:

```csharp
public UIContextualAction ContextualFlagAction(int row)
{
    var action = UIContextualAction.FromContextualActionStyle
                    (UIContextualActionStyle.Normal,
                        "Flag",
                        (FlagAction, view, success) => {
                            var alertController = UIAlertController.Create($"Report {words[row]}?", "", UIAlertControllerStyle.Alert);
                            alertController.AddAction(UIAlertAction.Create("Cancel", UIAlertActionStyle.Cancel, null)); 
                            alertController.AddAction(UIAlertAction.Create("Yes", UIAlertActionStyle.Destructive, null));
                            PresentViewController(alertController, true, null);
                            
                            success(true);
                        });

    action.Image = UIImage.FromFile("feedback.png");
    action.BackgroundColor = UIColor.Blue;

    return action;
}
```

배경색 또는 이미지 작업 등의 다양 한 시각적 속성을 편집할 수 있습니다. 위의 코드 조각 작업에 이미지를 추가 하 고 해당 배경 색을 파랑으로 설정 하는 방법을 보여 줍니다.

상황에 맞는 작업을 만든 후 초기화 하는 데 사용할 수는 `UISwipeActionsConfiguration` 에 `GetLeadingSwipeActionsConfiguration` 메서드.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

하나 이상의 행을 사용자 지정 동작을 정의할 수는 `UITableView`, 인스턴스의 생성 해야 합니다는 `UITableViewDelegate` 클래스를 재정의 합니다 `EditActionsForRow` 메서드. 예를 들어:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using Foundation;
using UIKit;

namespace BasicTable
{
    public class TableDelegate : UITableViewDelegate
    {
        #region Constructors
        public TableDelegate ()
        {
        }

        public TableDelegate (IntPtr handle) : base (handle)
        {
        }

        public TableDelegate (NSObjectFlag t) : base (t)
        {
        }

        #endregion

        #region Override Methods
        public override UITableViewRowAction[] EditActionsForRow (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewRowAction hiButton = UITableViewRowAction.Create (
                UITableViewRowActionStyle.Default,
                "Hi",
                delegate {
                    Console.WriteLine ("Hello World!");
                });
            return new UITableViewRowAction[] { hiButton };
        }
        #endregion
    }
}
```

정적 `UITableViewRowAction.Create` 메서드는 새 데 `UITableViewRowAction` 표시할를 **Hi** 사용자 천공 기와 테이블의 행에 대해 가로로 왼쪽 단추입니다. 나중의 새 인스턴스를 `TableDelegate` 만들어지고 연결 된 `UITableView`. 예를 들어:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

위의 코드에서 실행 되 고 사용자 천공 기와 테이블의 행에 남아 있는 경우는 **Hi** 단추가 대신 표시 됩니다는 **삭제** 기본적으로 표시 되는 단추:

[![](row-action-images/action01.png "삭제 단추 대신 표시 되 고 Hi 단추")](row-action-images/action01.png#lightbox)

사용자가 누를 경우 합니다 **Hi** 단추를 `Hello World!` 작성 됩니다 Visual Studio에서 콘솔에 Mac 또는 Visual Studio에 대 한 응용 프로그램 디버그 모드에서 실행 될 때입니다.



## <a name="related-links"></a>관련 링크

- [TableSwipeActions (샘플)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
