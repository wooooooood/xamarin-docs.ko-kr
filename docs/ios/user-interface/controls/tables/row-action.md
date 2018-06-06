---
title: Xamarin.iOS에 행 작업 사용
description: 이 가이드에 UISwipeActionsConfiguration 또는 UITableViewRowAction 테이블 행에 대 한 사용자 지정의 통과 작업을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/25/2017
ms.openlocfilehash: 4be8b6dc66c9c047e6662067e7e3ecf81ab22893
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789943"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin.iOS에 행 작업 사용

_이 가이드에 UISwipeActionsConfiguration 또는 UITableViewRowAction 테이블 행에 대 한 사용자 지정의 통과 작업을 만드는 방법을 보여 줍니다._

![행에서의 통과 작업을 보여 주는](row-action-images/action02.png)

테이블에서 작업을 수행 하는 두 가지를 제공 하는 iOS: `UISwipeActionsConfiguration` 및 `UITableViewRowAction`합니다.

`UISwipeActionsConfiguration` iOS 11에서에서 도입 되었으며 집합을 정의 하는 데 사용 되 곳에서 작업을 수행 해야 사용자 천공 기와 _어느 방향으로든에서_ 표 보기에서 한 행에 있습니다. 이 동작은 네이티브 Mail.app 유사 하지만 

`UITableViewRowAction` 클래스는 사용자 천공 기와 표 보기에서 한 행에 가로로 남아 있는 경우 발생 하는 동작을 정의 하는 데 사용 됩니다.
예를 들어 표시 될 때 행에 남아 있는 넘기기가 테이블 편집는 **삭제** 기본적으로는 단추입니다. 여러 인스턴스를 연결 하 여는 `UITableViewRowAction` 클래스는 `UITableView`, 여러 사용자 지정 동작을 정의 하려면 각각 자체 텍스트, 서식 및 동작 합니다.


## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

사용 하 여 동작의 통과 구현 하는 데 다음 세 단계 `UISwipeActionsConfiguration`:

1. 재정의 `GetLeadingSwipeActionsConfiguration` 및/또는 `GetTrailingSwipeActionsConfiguration` 메서드. 이러한 메서드는 반환 된 `UISwipeActionsConfiguration`합니다. 
2. 인스턴스화하는 `UISwipeActionsConfiguration` 반환 됩니다. 이 클래스는 배열을 사용 `UIContextualAction`합니다.
3. `UIContextualAction`를 만듭니다.

이러한 작업은 다음 섹션에서 자세히 설명 되어 있습니다.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. SwipeActionsConfigurations 메서드 구현

`UITableViewController` (그리고 `UITableViewSource` 및 `UITableViewDelegate`) 메서드가 2 개 포함: `GetLeadingSwipeActionsConfiguration` 및 `GetTrailingSwipeActionsConfiguration`, 표 보기 행에 살짝 작업 집합을 구현 하는 데 사용 되는 합니다. 화면 왼쪽에서 오른쪽 언어에서의 왼쪽 및 오른쪽에서 왼쪽 언어에서 화면 오른쪽에서는 살짝 선행 살짝 작업 참조합니다. 

다음 예제에서는 (에서 [TableSwipeActions](https://developer.xamarin.com/samples/monotouch/TableSwipeActions) 샘플) 선행 살짝 구성을 구현 하는 방법을 보여 줍니다. 다음 두 가지 동작 설명 하는 상황에 맞는 동작에서 만들어진 [아래](#create-uicontextualaction)합니다. 에 그런 다음 이러한 작업은 전달 하는 새롭게 초기화 된 [ `UISwipeActionsConfiguration` ](#create-uiswipeactionsconfigurations), 반환 값으로 사용 됩니다.


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

인스턴스화하는 `UISwipeActionsConfiguration` 를 사용 하 여는 `FromActions` 이루어진 새 배열을 추가 하는 방법을 `UIContextualAction`s, 다음 코드 조각에 나와 있는 것 처럼:

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

작업을 수행할 때 표시 되는 순서는 배열에 전달 되는 방법에 따라를 두는 것이 유용 합니다. 예를 들어 선행 천공 기와 대 한 위의 코드는 같이 작업 표시합니다.

![테이블 행에 표시 된 동작의 통과 앞에](row-action-images/action03.png)

천공 기와 후행에 대 한 작업은 다음 그림에 표시 된 것 처럼 표시 됩니다.

![테이블 행에 표시 된 동작의 통과 후행](row-action-images/action04.png)

이 코드 조각은 또한를 사용 하는 새 `PerformsFirstActionWithFullSwipe` 속성입니다. 기본적으로이 속성 설정 true, 즉 때 사용자 천공 기와 완전히 행에 대해 배열의 첫 번째 작업이 실행 됩니다. 삭제 되지 않은 작업을 사용 하는 경우 (예를 들어 "Delete" 이상적인 동작 하지 않을 하 고 따라서로 설정 해야 `false`합니다.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>만들기는 `UIContextualAction`

상황에 맞는 작업이 실제로 사용자 테이블 행 swipes 때 표시 되는 작업을 만들 것입니다.

제공 해야 하는 작업을 초기화 하는 `UIContextualActionStyle`, 제목 및 `UIContextualActionHandler`합니다. `UIContextualActionHandler` 세 개의 매개 변수: 작업, 작업에 표시 된 보기 및 완료 처리기:

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

배경색 또는 동작의 이미지와 같은 다양 한 시각적 속성을 편집할 수 있습니다. 위의 코드 조각 작업으로 이미지를 추가 하 고 배경색을 파란색 설정 하는 방법을 보여 줍니다.

초기화 하는 데 사용할 수는 상황에 맞는 작업을 만든 후의 `UISwipeActionsConfiguration` 에 `GetLeadingSwipeActionsConfiguration` 메서드.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

하나 이상의 행을 사용자 지정 동작을 정의할 수는 `UITableView`, 인스턴스의 생성 해야 합니다는 `UITableViewDelegate` 클래스 및 재정의 `EditActionsForRow` 메서드. 예를 들어:

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

정적 `UITableViewRowAction.Create` 새 메서드는 `UITableViewRowAction` 을 표시할는 **Hi** 사용자 천공 기와 왼쪽 테이블의 행에 가로로 단추입니다. 나중의 새 인스턴스는 `TableDelegate` 만들고 첨부 된 `UITableView`합니다. 예를 들어:

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

위의 코드를 실행 하 고 테이블 행에 남아 있는 사용자 천공 기와 **Hi** 대신 단추가 표시 됩니다는 **삭제** 기본적으로 표시 되는 단추:

[![](row-action-images/action01.png "삭제 단추 대신 표시 되 고 Hi 단추")](row-action-images/action01.png#lightbox)

사용자가 누를 경우는 **Hi** 단추를 `Hello World!` 작성 됩니다 Visual Studio에서 콘솔에 Mac 또는 Visual Studio에 대 한 응용 프로그램 디버그 모드에서 실행 됩니다.



## <a name="related-links"></a>관련 링크

- [TableSwipeActions (샘플)](https://developer.xamarin.com/samples/monotouch/TableSwipeActions)
- [WorkingWithTables (샘플)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
