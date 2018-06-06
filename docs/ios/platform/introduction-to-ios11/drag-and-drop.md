---
title: 끌어서 놓기 Xamarin.iOS에
description: 이 문서에서는 끌어서를 구현 하 고 iOS 11에에서 도입 된 Api를 사용 하 여 Xamarin.iOS 앱에서 삭제 하는 방법을 설명 합니다. 사용 설명 특히 UITableView에 놓습니다.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: 7c41f96dae88047e64ec1e74838e3efab55958cc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786966"
---
# <a name="drag-and-drop-in-xamarinios"></a>끌어서 놓기 Xamarin.iOS에

_끌어서 놓기 iOS 11에 대 한 구현_

iOS 11 포함 끌어서 놓기 지원을 iPad에 있는 응용 프로그램 간에 데이터를 복사 합니다. 사용자가 선택한 배치 앱--나란히 하거나 앱을 열고 데이터를 삭제할 수 있도록 수 트리거되고 앱 아이콘 위에 끌어서 모든 유형의 콘텐츠를 끌어 수 있습니다.

![메모 응용 프로그램에 사용자 지정 응용 프로그램에서 끌어서 놓기 예제](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> 끌어서 놓기는 iPhone에서 동일한 응용 프로그램 내에서 사용할 수만 있습니다.

놓기 작업 아무 곳 이나 콘텐츠 생성 또는 편집할 수 있으며 끌어서를 지 원하는 보십시오.

- 텍스트 컨트롤 추가 작업 없이 iOS 11에 대해 작성 된 모든 앱에 대 한 끌어서 놓기 지원 합니다.
- 테이블 뷰와 컬렉션 뷰 11 시키고 Io를 단순화 추가 끌어서 놓기 동작에 향상 된 기능을 포함 합니다.
- 추가 사용자 지정과 놓습니다 끌어서 원하는 다른 보기를 만들 수 있습니다.

앱에 추가 끌어서 놓기 지원, 서로 다른 수준의 콘텐츠 충실도; 구현할 수 있습니다. 예를 들어 수신 응용 프로그램을 끌기 대상에 대 한 가장 잘 맞는 선택할 수 있도록 서식 있는 텍스트 및 데이터의 일반 텍스트 버전을 모두 제공할 수 있습니다. 끌어서 시각화를 사용자 지정 하 고 한 번에 여러 항목을 끌어 사용 가능한 이기도 합니다.

## <a name="drag-and-drop-with-text-controls"></a>드래그 앤 드롭 텍스트 컨트롤

`UITextView` 및 `UITextField` 자동으로 선택 된 텍스트 출력을 텍스트 끌어다 놓기에 콘텐츠를 지원 합니다.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>드래그 앤 드롭 UITableView와

`UITableView` 끌어서 놓기 테이블 행을 기본 동작을 사용 하도록 설정 하려면 몇 가지 방법만 요구와 상호 작용에 대 한 기본 제공 처리를 있습니다.

두 개의 인터페이스 포함 됩니다.

- `IUITableViewDragDelegate` – 테이블 보기에서 끌기 시작 되 면 패키지 정보입니다.
- `IUITableViewDropDelegate` – 삭제 중인 경우 정보를 처리 하려고 하 고 완료 합니다.

에 [DragAndDropTableView 샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) 두 인터페이스는 둘 다에 구현 됩니다는 `UITableViewController` 클래스, 대리자 및 데이터 원본입니다. 할당 하는 `ViewDidLoad` 메서드:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

이러한 두 인터페이스에 대 한 필요한 최소한의 코드는 아래 설명 되어 있습니다.

### <a name="table-view-drag-delegate"></a>테이블 보기 끌어서 대리자

유일한 방법은 _필요한_ 표 보기에서 한 행을 끌어 지원 하기 위해 `GetItemsForBeginningDragSession`합니다. 사용자가 끌어 행을 시작 하는 경우이 메서드가 호출 됩니다.

구현은 다음과 같습니다. 끌어 온된 행과 연결 된 데이터를 검색, 인코딩합니다, 구성 및는 `NSItemProvider` 응용 프로그램에서 작업의 "drop" 부분을 처리 하는 방법을 결정 하는 (예를 들어 여부 처리할 수 있도록 데이터 형식을 `PlainText`, 예제에서):

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

대상 앱에 이점이 취할 수 있는 여러 데이터 표현을 제공 하는 등 끌어서 동작을 사용자 지정을 구현할 수 있는 끌어서 대리자에 대해 선택적 가지가 많은 (서식 있는 텍스트와 일반 텍스트 또는 벡터와 같은 및 비트맵의 버전은 드로잉)입니다. 또한 동일한 응용 프로그램 내에서 끌어서 때 사용 되는 사용자 지정 데이터 표현을 제공할 수 있습니다.

### <a name="table-view-drop-delegate"></a>테이블 보기 놓기 대리자

Drop 대리자에서 메서드는 끌기 작업 테이블 뷰를 통해 발생 하거나 그 위에 완료 될 때 호출 됩니다. 필요한 메서드는 삭제가 완료 되 면 수행 되는 작업 및 데이터가 삭제 될 수 있는지 여부를 결정 합니다.

- `CanHandleDropSession` -진행률 및 잠재적으로에 끌어 놓는 응용 프로그램에서의 끌어서 이지만,이 메서드는 끌고 있는 데이터가 삭제 될 수 있는지 여부를 결정 합니다.
- `DropSessionDidUpdate` – 끌기가 진행 중인 동안 어떤 작업을 위한 것이 확인 하려면이 메서드 호출 됩니다. 테이블 뷰 위로 끄는, 끌기 세션 및 가능한 인덱스 경로 정보를 모두 사용할 수 동작 및 사용자에 게 제공 하는 시각적 피드백을 결정 하 합니다.
- `PerformDrop` – 사용자 (해당 손가락 들어올려) 삭제가 완료 되 면 하는 경우이 메서드 끌고 있는 데이터를 추출 하 고 새 행 (또는 행)에 데이터를 추가 하려면 테이블 뷰를 수정 합니다.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` 테이블 뷰 끌고 있는 데이터를 사용할 수 있는지 여부를 나타냅니다. 이 코드 조각에서는 `CanLoadObjects` 이 테이블 뷰의 문자열 데이터를 수락할 수 있음을 확인 하는 데 사용 됩니다.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` 끌기 작업이 진행 중 사용자에 게 시각적 표시를 제공 하는 동안 메서드가 반복적으로 호출 됩니다.

아래 코드에서 `HasActiveDrag` 현재 테이블 보기에서 작업을 발생 하는지 여부를 결정 하는 데 사용 됩니다. 이 경우 단일 행만 이동할 수 있습니다.
다른 소스에서 끌기 경우 복사 작업을 표시 됩니다.

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

Drop 작업 중 하나일 수 있습니다 `Cancel`, `Move`, 또는 `Copy`합니다.

Drop 의도를 새 행을 삽입 하거나 기존 행에 데이터 추가/추가할 수 있습니다.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` 메서드는 사용자는 작업을 완료 하 고 삭제 된 데이터를 반영 하도록 테이블 뷰 및 데이터 소스를 수정 합니다.

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

큰 데이터 개체를 비동기적으로 로드 추가 코드를 추가할 수 있습니다.

### <a name="testing-drag-and-drop"></a>테스트 끌어서 놓기

IPad를 사용 하 여 테스트 해야 합니다는 [샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)합니다.
다른 응용 프로그램 (예: 메모)와 함께 샘플을 열고 서로 행과 텍스트를 끕니다.

![끌기 작업 중에서의 스크린 샷](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>관련 링크

- [끌어서 놓기 휴먼 인터페이스 지침 (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [끌어서 놓기 테이블 보기 샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [끌어서 놓기 컬렉션 뷰 샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [끌어서 놓기 (WWDC) (비디오)를 소개합니다.](https://developer.apple.com/videos/play/wwdc2017/203/)
- [끌어서 놓기 컬렉션 및 테이블 보기 (WWDC) (비디오)](https://developer.apple.com/videos/play/wwdc2017/223/)
