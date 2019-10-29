---
title: Xamarin.ios에서 행 작업 사용
description: 이 가이드에서는 UISwipeActionsConfiguration 또는 UITableViewRowAction을 사용 하 여 테이블 행에 대해 사용자 지정 살짝 밀기 동작을 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 340FB633-0C46-40AA-9963-FF17D7CA6858
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/25/2017
ms.openlocfilehash: 542ae6696bae8fccfa6d5ed9842bce126760da37
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021862"
---
# <a name="working-with-row-actions-in-xamarinios"></a>Xamarin.ios에서 행 작업 사용

_이 가이드에서는 UISwipeActionsConfiguration 또는 UITableViewRowAction을 사용 하 여 테이블 행에 대해 사용자 지정 살짝 밀기 동작을 만드는 방법을 보여 줍니다._

![행에 대 한 살짝 밀기 동작 시연](row-action-images/action02.png)

iOS에서는 테이블에 대 한 작업을 수행 하는 두 가지 방법, 즉 `UISwipeActionsConfiguration` 및 `UITableViewRowAction`를 제공 합니다.

`UISwipeActionsConfiguration`은 iOS 11에서 도입 되었으며 사용자가 테이블 뷰의 행에 대해 _어느 방향으로든_ swipes 때 발생 해야 하는 작업 집합을 정의 하는 데 사용 됩니다. 이 동작은 네이티브 메일 앱과 유사 합니다.

`UITableViewRowAction` 클래스는 사용자가 테이블 뷰의 행에 가로 방향으로 swipes 때 수행 되는 동작을 정의 하는 데 사용 됩니다.
예를 들어 테이블을 편집 하는 경우 행을 왼쪽으로 살짝 밀어 놓으면 **삭제** 단추가 기본적으로 표시 됩니다. `UITableViewRowAction` 클래스의 여러 인스턴스를 `UITableView`에 연결 하 여 각각 고유한 텍스트, 서식 지정 및 동작을 사용 하 여 여러 사용자 지정 작업을 정의할 수 있습니다.

## <a name="uiswipeactionsconfiguration"></a>UISwipeActionsConfiguration

`UISwipeActionsConfiguration`를 사용 하 여 살짝 밀기 작업을 구현 하는 데 필요한 세 가지 단계가 있습니다.

1. `GetLeadingSwipeActionsConfiguration` 및/또는 `GetTrailingSwipeActionsConfiguration` 메서드를 재정의 합니다. 이러한 메서드는 `UISwipeActionsConfiguration`반환 합니다.
2. 반환할 `UISwipeActionsConfiguration`를 인스턴스화합니다. 이 클래스는 `UIContextualAction`배열을 사용 합니다.
3. `UIContextualAction`를 만듭니다.

이러한 내용은 다음 섹션에서 더 자세히 설명 합니다.

### <a name="1-implementing-the-swipeactionsconfigurations-methods"></a>1. SwipeActionsConfigurations 메서드 구현

`UITableViewController` (및 `UITableViewSource` 및 `UITableViewDelegate`)에는 테이블 뷰 행에 대해 살짝 밀기 동작 집합을 구현 하는 데 사용 되는 두 가지 메서드인 `GetLeadingSwipeActionsConfiguration` 및 `GetTrailingSwipeActionsConfiguration`가 포함 되어 있습니다. 선행 살짝 밀기 동작은 화면 왼쪽에서 오른쪽으로, 오른쪽에서 왼쪽으로 진행 되는 언어의 화면 오른쪽에서 살짝 밀기를 나타냅니다.

다음 예제에서는 ( [TableSwipeActions](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions) 샘플에서) 선행 살짝 밀기 구성을 구현 하는 방법을 보여 줍니다. 두 작업은 [아래](#create-uicontextualaction)에서 설명 하는 상황별 동작에서 만들어집니다. 이러한 작업은 반환 값으로 사용 되는 새로 초기화 된 [`UISwipeActionsConfiguration`](#create-uiswipeactionsconfigurations)전달 됩니다.

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

### <a name="2-instantiate-a-uiswipeactionsconfiguration"></a>2. `UISwipeActionsConfiguration` 인스턴스화

다음 코드 조각과 같이 `FromActions` 메서드를 사용 하 여 `UIContextualAction`s의 새 배열을 추가 하 여 `UISwipeActionsConfiguration`를 인스턴스화합니다.

```csharp
var leadingSwipe = UISwipeActionsConfiguration.FromActions(new UIContextualAction[] { flagAction, definitionAction })

leadingSwipe.PerformsFirstActionWithFullSwipe = false;
```

작업이 표시 되는 순서는 배열에 전달 되는 방식에 따라 달라 집니다. 예를 들어 선행 swipes 위의 코드는 다음과 같이 작업을 표시 합니다.

![테이블 행에 표시 되는 선행 살짝 밀기 동작](row-action-images/action03.png)

후행 swipes의 경우 작업은 다음 이미지와 같이 표시 됩니다.

![테이블 행에 표시 되는 후행 살짝 밀기 동작](row-action-images/action04.png)

또한이 코드 조각은 새 `PerformsFirstActionWithFullSwipe` 속성을 사용 합니다. 기본적으로이 속성은 true로 설정 됩니다. 즉, 사용자가 행을 완전히 swipes 때 배열의 첫 번째 작업이 수행 됩니다. 소거식이 아닌 동작이 있는 경우 (예: "Delete")이는 이상적인 동작이 아닐 수 있으므로 `false`설정 해야 합니다.

<a name="create-uicontextualaction" />

### <a name="create-a-uicontextualaction"></a>`UIContextualAction` 만들기

컨텍스트 작업은 사용자가 테이블 행을 swipes 때 표시 되는 작업을 실제로 만드는 위치입니다.

작업을 초기화 하려면 `UIContextualActionStyle`, 제목 및 `UIContextualActionHandler`를 제공 해야 합니다. 이 `UIContextualActionHandler`는 작업, 작업을 표시 한 뷰 및 완료 처리기의 세 가지 매개 변수를 사용 합니다.

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

작업의 배경색 또는 이미지와 같은 다양 한 시각적 속성을 편집할 수 있습니다. 위의 코드 조각에서는 작업에 이미지를 추가 하 고 배경색을 파랑으로 설정 하는 방법을 보여 줍니다.

컨텍스트 작업을 만든 후에는를 사용 하 여 `GetLeadingSwipeActionsConfiguration` 메서드에서 `UISwipeActionsConfiguration`를 초기화할 수 있습니다.

## <a name="uitableviewrowaction"></a>UITableViewRowAction

`UITableView`에 대해 하나 이상의 사용자 지정 행 작업을 정의 하려면 `UITableViewDelegate` 클래스의 인스턴스를 만들고 `EditActionsForRow` 메서드를 재정의 해야 합니다. 예를 들면,

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

정적 `UITableViewRowAction.Create` 메서드를 사용 하 여 사용자가 테이블의 행에 가로 방향 **으로 swipes 때 단추를** 표시 하는 새 `UITableViewRowAction`을 만들 수 있습니다. 나중에 `TableDelegate`의 새 인스턴스가 만들어지고 `UITableView`에 연결 됩니다. 예를 들면,

```csharp
TableDelegate tableDelegate;
...

// Replace the standard delete button with a "Hi" button
tableDelegate = new TableDelegate ();
table.Delegate = tableDelegate;

```

위의 코드를 실행 하 고 사용자가 테이블 행에 swipes는 경우 기본적으로 표시 되는 **삭제** 단추 대신 **안녕하세요** 단추가 표시 됩니다.

[![](row-action-images/action01.png "The Hi button being displayed instead of the Delete button")](row-action-images/action01.png#lightbox)

사용자 **가 단추를** 탭 하면 응용 프로그램이 디버그 모드에서 실행 될 때 Mac용 Visual Studio 또는 Visual Studio에서 콘솔에 `Hello World!` 작성 됩니다.

## <a name="related-links"></a>관련 링크

- [TableSwipeActions (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/tableswipeactions)
- [WorkingWithTables (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
