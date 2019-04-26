---
title: Xamarin.iOS에서 끌어서 놓기
description: 이 문서에 끌어서 구현 및 iOS 11에에서 도입 된 Api를 사용 하 여 Xamarin.iOS 앱에 삭제 하는 방법을 설명 합니다. 설정 설명 특히 UITableView에 놓습니다.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/05/2017
ms.openlocfilehash: aa93e015a399e733a2bb52f087a1e482bc23a00a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169671"
---
# <a name="drag-and-drop-in-xamarinios"></a>Xamarin.iOS에서 끌어서 놓기

_끌어서 놓기 ios 11의 구현_

iOS 11 포함 끌어서 놓기 지원을 iPad에서 응용 프로그램 간에 데이터를 복사 합니다. 사용자가 선택 하 고 배치 앱-side-by-side, 또는 데이터를 삭제할 수 있도록 하 여 앱을 트리거할 앱 아이콘 위로 드래그 하 여 모든 유형의 콘텐츠를 끌어 수 있습니다.

![메모 앱에 사용자 지정 앱에서 끌어서 놓기 예제](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> 끌어서 놓기는 iPhone에서 동일한 앱 내에서 사용할 수만 있습니다.

끌어서 지원 고려 하 고 어디서 나 콘텐츠 놓기 작업은 만들거나 편집할 수 있습니다.

- 텍스트 컨트롤 추가 작업 없이 iOS 11에 대해 빌드된 모든 앱에 대 한 끌어서 놓기 지원 합니다.
- 테이블 뷰와 컬렉션 뷰 간소화 추가 끌어서 놓기 동작을 iOS 11의에서 향상 된 기능을 포함 합니다.
- 끌어서 지원 추가 사용자 지정을 사용 하 여 삭제를 다른 뷰를 만들 수 있습니다.

앱에 추가 하는 끌어서 놓기 지원, 경우에 서로 다른 수준의 콘텐츠 충실도;을 제공할 수 있습니다. 예를 들어 수신 앱을 끌기 대상으로 가장 적합 한는 선택할 수 있도록 서식 있는 텍스트 및 데이터의 일반 텍스트 버전을 모두 제공할 수 있습니다. 끌어서 시각화를 사용자 지정할 수 있으며 한 번에 여러 항목을 끌어 올 수 있도록 이기도 합니다.

## <a name="drag-and-drop-with-text-controls"></a>텍스트 컨트롤을 사용한 끌어서 놓기

`UITextView` 및 `UITextField` 자동으로, 선택한 텍스트를 놓는 텍스트 콘텐츠를 지원 합니다.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>UITableView 사용한 끌어서 놓기

`UITableView` 끌어서 놓기 테이블 행을 기본 동작을 사용 하도록 설정 하려면 몇 가지 방법만 요구와 상호 작용에 대 한 기본 제공 처리를 있습니다.

두 개의 인터페이스가 포함 됩니다.

- `IUITableViewDragDelegate` – 테이블 보기의 끌기 시작할 때 패키지 정보입니다.
- `IUITableViewDropDelegate` – 저장 될 때 정보를 처리 하려고 하 고 완료 합니다.

에 [DragAndDropTableView 샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) 두 인터페이스 모두에서 구현 됩니다는 `UITableViewController` 클래스, 대리자 및 데이터 원본입니다. 에 할당 된 `ViewDidLoad` 메서드:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

이러한 두 인터페이스에 대 한 최소한의 필요한 코드를 아래 설명 되어 있습니다.

### <a name="table-view-drag-delegate"></a>테이블 보기 끌어서 대리자

유일한 방법은 _필요_ 표 보기에서 행을 끌어 지원 하기 위해 `GetItemsForBeginningDragSession`합니다. 사용자가 행을 끌기 시작 되 면이 메서드를 호출 합니다.

구현은 다음과 같습니다. 끌어 온된 행과 연결 된 데이터를 검색, 인코드, 하 고 구성를 `NSItemProvider` 응용 프로그램 작업의 "drop" 부분을 처리 하는 방법을 결정 하는 (예를 들어 여부 처리할 수 있도록 데이터 형식, `PlainText`, 예제에서):

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

대상 앱에서 활용 수행할 수 있는 여러 데이터 표현을 제공 하는 등의 끌기 동작을 사용자 지정할 구현할 수 있는 끌어서 대리자에 다양 한 선택적 메서드 (서식 있는 텍스트는 일반 텍스트 또는 벡터와 같은 및 비트맵의 버전은 그리기)입니다. 동일한 앱 내에서 끌어서 놓아 때 사용 되는 사용자 지정 데이터 표현을 제공할 수도 있습니다.

### <a name="table-view-drop-delegate"></a>테이블 보기 놓기 대리자

Drop 대리자에서 메서드는 끌기 작업 테이블 뷰를 통해 수행 되거나 이보다 완료 될 때 호출 됩니다. 필요한 방법 드롭다운 완료 되 면 수행 되는 작업 및 데이터를 삭제 하도록 허용 되는지 여부를 결정 합니다.

- `CanHandleDropSession` – 끌기 in progress, 그리고 잠재적으로 응용 프로그램 삭제 중 상태인 동안이 메서드는 끌고 있는 데이터는 삭제할 수 있는지 여부를 결정 합니다.
- `DropSessionDidUpdate` – 끌기 중에서 상태인 동안 작업 것을 확인 하려면이 메서드 호출 됩니다. 위로 끌고 테이블 보기, 끌어서 세션 및 가능한 인덱스 경로 정보를 사용할 수 있습니다 모든 동작 및 사용자에 게 제공 하는 시각적 피드백을 확인 하려면.
- `PerformDrop` – 끝나면 사용자 드롭다운 (해당 손가락 들어올려),이 메서드는 끌고 있는 데이터를 추출 하 고 새 행 또는 (행)의 데이터를 추가 하려면 테이블 뷰를 수정 합니다.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` 테이블 보기 끌고 있는 데이터를 사용할 수 있는지 여부를 나타냅니다. 이 코드 조각에서는 `CanLoadObjects` 이 테이블 뷰의 문자열 데이터를 사용할 수 있는 확인 하는 데 사용 됩니다.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

`DropSessionDidUpdate` 메서드는 끌기 작업 진행 중, 사용자에 게 시각 신호를 제공 하는 동안 반복적으로 호출 됩니다.

아래 코드에서 `HasActiveDrag` 현재 테이블 보기의 작업을 시작 하는지 여부를 결정 하는 데 사용 됩니다. 그렇다면 단일 행만 이동할 수 있습니다.
다른 소스에서 끌기 인 경우 복사 작업을 표시 됩니다.

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

Drop 작업 중 하나일 수 있습니다 `Cancel`하십시오 `Move`, 또는 `Copy`합니다.

Drop 의도 새 행을 삽입 하거나 기존 행에 데이터 추가/추가 될 수 있습니다.

#### <a name="performdrop"></a>PerformDrop

`PerformDrop` 메서드는 사용자 작업을 완료 하 고 끌어 놓은 데이터를 반영 하도록 테이블 뷰 및 데이터 소스를 수정 합니다.

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

큰 데이터 개체를 비동기적으로 로드 하려면 추가 코드를 추가할 수 있습니다.

### <a name="testing-drag-and-drop"></a>테스트 끌어서 놓기

IPad를 사용 하 여 테스트 해야 합니다 [샘플](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)합니다.
다른 앱 (예: 정보)와 함께 샘플을 열고 서로 행 및 텍스트를 끕니다.

![끌기 작업 중에서의 스크린샷](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>관련 링크

- [끌어서 놓기 휴먼 인터페이스 지침 (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [끌어서 놓기 샘플 테이블 보기](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [끌어서 놓기 샘플 컬렉션 보기](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [끌어서 놓기 (WWDC) (비디오)를 소개합니다.](https://developer.apple.com/videos/play/wwdc2017/203/)
- [컬렉션 및 테이블 뷰 (WWDC) (비디오)를 사용한 끌어서 놓기](https://developer.apple.com/videos/play/wwdc2017/223/)
