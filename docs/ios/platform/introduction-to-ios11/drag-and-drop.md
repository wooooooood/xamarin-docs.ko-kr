---
title: Xamarin.ios에서 끌어서 놓기
description: 이 문서에서는 iOS 11에 도입 된 Api를 사용 하 여 Xamarin.ios 앱에서 끌어서 놓기를 구현 하는 방법을 설명 합니다. 특히 UITableView에서 끌어서 놓기를 사용 하도록 설정 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/05/2017
ms.openlocfilehash: 928936815c89dd74d0ad3775f59ea210702c8857
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306166"
---
# <a name="drag-and-drop-in-xamarinios"></a>Xamarin.ios에서 끌어서 놓기

_IOS 11에 대 한 끌어서 놓기 구현_

iOS 11에는 iPad의 응용 프로그램 간에 데이터를 복사 하기 위한 끌어서 놓기 지원이 포함 됩니다. 사용자는 나란히 배치 된 앱에서 모든 형식의 콘텐츠를 선택 하 고 끌어서 놓을 수 있습니다. 그러면 앱 아이콘을 끌어 앱을 열고 데이터를 삭제할 수 있습니다.

![사용자 지정 앱에서 Notes 앱으로 예제 끌어서 놓기](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> 끌어서 놓기는 iPhone의 동일한 앱 내 에서만 사용할 수 있습니다.

모든 콘텐츠를 만들거나 편집할 수 있는 끌어서 놓기 작업 지원 고려:

- 텍스트 컨트롤은 추가 작업 없이 iOS 11에 대해 빌드된 모든 앱에 대 한 끌어서 놓기를 지원 합니다.
- 테이블 보기 및 컬렉션 뷰에는 끌어서 놓기 동작 추가를 간소화 하는 iOS 11의 향상 된 기능이 포함 되어 있습니다.
- 다른 모든 보기는 추가 사용자 지정을 통해 끌어서 놓기를 지원 하도록 만들 수 있습니다.

앱에 끌어서 놓기 지원을 추가 하는 경우 다양 한 수준의 콘텐츠 충실도를 제공할 수 있습니다. 예를 들어, 데이터의 서식 있는 텍스트와 일반 텍스트 버전을 모두 제공 하 여 수신 앱이 끌기 대상에 가장 적합 한 데이터를 선택할 수 있습니다. 또한 끌기 시각화를 사용자 지정 하 고 한 번에 여러 항목을 끌 수 있습니다.

## <a name="drag-and-drop-with-text-controls"></a>텍스트 컨트롤을 사용 하 여 끌어서 놓기

`UITextView` 및 `UITextField`에서는 선택한 텍스트를 끌어와에서 텍스트 내용을 끌어 놓는 기능을 자동으로 지원 합니다.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>UITableView를 사용 하 여 끌어서 놓기

`UITableView`에는 테이블 행과의 끌어서 놓기 상호 작용에 대 한 기본 제공 처리가 있으므로 몇 가지 메서드만 있으면 기본 동작을 사용 하도록 설정할 수 있습니다.

관련 된 두 가지 인터페이스가 있습니다.

- `IUITableViewDragDelegate` – 테이블 뷰에서 끌기를 시작할 때 정보를 패키지 합니다.
- `IUITableViewDropDelegate` – 놓기를 시도 하 고 완료할 때 정보를 처리 합니다.

[DragAndDropTableView 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview) 에서는이 두 인터페이스가 모두 `UITableViewController` 클래스에서 구현 되며 대리자 및 데이터 소스와 함께 구현 됩니다. `ViewDidLoad` 메서드에서 할당 됩니다.

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

이러한 두 인터페이스에 필요한 최소한의 코드는 아래에 설명 되어 있습니다.

### <a name="table-view-drag-delegate"></a>테이블 뷰 끌기 대리자

테이블 뷰에서 행 끌기를 지 원하는 데 _필요한_ 유일한 방법은 `GetItemsForBeginningDragSession`입니다. 사용자가 행을 끌기 시작 하면이 메서드가 호출 됩니다.

구현은 다음과 같습니다. 끌어 온 행과 연결 된 데이터를 검색 하 고 인코딩한 다음, 응용 프로그램에서 작업의 "삭제" 부분을 처리 하는 방법을 결정 하는 `NSItemProvider` 구성 합니다. 예를 들어이 예제에서는 데이터 형식을 처리할 수 있는지 여부와 같은 작업을 `PlainText`합니다.

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

끌기 대리자에는 끌기 동작을 사용자 지정 하기 위해 구현할 수 있는 여러 가지 선택적 메서드가 있습니다. 예를 들어 서식 있는 텍스트 뿐만 아니라 일반 텍스트, 벡터 및와 같은 여러 데이터 표현을 제공 합니다. 그리기의 비트맵 버전입니다. 동일한 앱 내에서 끌어서 놓을 때 사용할 사용자 지정 데이터 표현을 제공할 수도 있습니다.

### <a name="table-view-drop-delegate"></a>테이블 뷰 삭제 대리자

Drop 대리자의 메서드는 테이블 뷰에서 끌기 작업이 발생 하거나 위에서 완료 될 때 호출 됩니다. 필요한 메서드는 데이터를 삭제할 수 있는지 여부와 삭제가 완료 된 경우 수행할 작업을 결정 합니다.

- `CanHandleDropSession` – 끌기가 진행 중이 고 잠재적으로 응용 프로그램에서 삭제 되는 동안이 메서드는 끌고 있는 데이터를 삭제할 수 있는지 여부를 확인 합니다.
- `DropSessionDidUpdate` – 끌기가 진행 되는 동안이 메서드를 호출 하 여 의도 한 작업을 결정 합니다. 끌기 세션 및 가능한 인덱스 경로를 통해 끌고 있는 테이블 뷰의 정보는 사용자에 게 제공 되는 동작과 시각적 피드백을 확인 하는 데 모두 사용할 수 있습니다.
- `PerformDrop` – 사용자가 손가락을 떼는 방법으로 삭제를 완료 하면이 메서드는 끌고 있는 데이터를 추출 하 고 테이블 뷰를 수정 하 여 새 행 (또는 행)에 데이터를 추가 합니다.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` 테이블 뷰에서 끌고 있는 데이터를 허용할 수 있는지 여부를 나타냅니다. 이 코드 조각에서 `CanLoadObjects`를 사용 하 여이 테이블 뷰에서 문자열 데이터를 사용할 수 있는지 확인 합니다.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` 메서드는 끌기 작업이 진행 되는 동안 사용자에 게 시각적 신호를 제공 하는 동안 반복적으로 호출 됩니다.

아래 코드에서 `HasActiveDrag`를 사용 하 여 작업이 현재 테이블 뷰에서 생성 되었는지 여부를 확인 합니다. 그렇다면 단일 행만 이동할 수 있습니다.
다른 원본에서 끌어온 경우 복사 작업이 표시 됩니다.

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Drop 작업은 `Cancel`, `Move`또는 `Copy`중 하나일 수 있습니다.

Drop 의도는 새 행을 삽입 하거나 기존 행에 데이터를 추가/추가할 수 있습니다.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` 메서드는 사용자가 작업을 완료할 때 호출 되며, 삭제 된 데이터를 반영 하도록 테이블 뷰와 데이터 원본을 수정 합니다.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

추가 코드를 추가 하 여 대량 데이터 개체를 비동기적으로 로드할 수 있습니다.

### <a name="testing-drag-and-drop"></a>끌어서 놓기 테스트

[예제](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)를 테스트 하려면 iPad를 사용 해야 합니다.
다른 앱 (예: 메모)과 함께 샘플을 열고 행과 텍스트를 서로 끌어 옵니다.

![진행 중인 끌기 작업 스크린샷](drag-and-drop-images/01-sml.png)

## <a name="related-links"></a>관련 링크

- [Apple (인간 인터페이스 지침) 끌어서 놓기](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [테이블 뷰 끌어서 놓기 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [컬렉션 뷰 끌어서 놓기 샘플](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [끌어서 놓기 소개 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [컬렉션 및 테이블 뷰 (WWDC)를 사용 하 여 끌어서 놓기 (비디오)](https://developer.apple.com/videos/play/wwdc2017/223/)
