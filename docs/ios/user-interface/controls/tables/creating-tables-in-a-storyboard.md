---
title: iOS Designer에서 테이블 작업
description: 이전 섹션에서는 테이블을 사용 하 여 개발 하는 방법을 살펴보았습니다. 5 번째 및 마지막 섹션에서는 지금까지 배운 내용을 집계 하 고 스토리 보드를 사용 하 여 기본 Chore List 응용 프로그램을 만듭니다.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: e73695046786e4d9949fd46bdbba665ff4f6cc72
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645170"
---
# <a name="working-with-tables-in-the-ios-designer"></a>iOS Designer에서 테이블 작업

스토리 보드는 iOS 응용 프로그램을 만드는 WYSIWYG 방식 이며, Mac 및 Windows의 Visual Studio 내에서 지원 됩니다. Storyboard에 대 한 자세한 내용은 [Storyboard 소개](~/ios/user-interface/storyboards/index.md) 문서를 참조 하세요. 또한 storyboard를 사용 하면 테이블 및 셀을 사용 하 여 개발을 간소화 하는 테이블 *의* 셀 레이아웃을 편집할 수 있습니다.

IOS 디자이너에서 테이블 뷰의 속성을 구성할 때 선택할 수 있는 셀 내용에는 두 가지 유형이 있습니다. **동적** 또는 **정적** 프로토타입 콘텐츠입니다.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>동적 프로토타입 콘텐츠

프로토타입 `UITableView` 콘텐츠가 포함 된은 일반적으로 목록에서 각 항목에 대해 프로토타입 셀 (또는 둘 이상의 셀을 정의할 수 있는 셀)이 다시 사용 되는 데이터 목록을 표시 하기 위한 것입니다. 셀을 인스턴스화할 필요가 없는 경우의 `GetView` `DequeueReusableCell` `UITableViewSource`메서드를 호출 하 여 메서드에서 가져옵니다.

 <a name="Static_Content" />


## <a name="static-content"></a>정적 콘텐츠

`UITableView`정적 콘텐츠가 있는 s를 사용 하면 디자인 화면에서 바로 테이블을 디자인할 수 있습니다. 셀은 테이블로 끌고 속성을 변경 하 고 컨트롤을 추가 하 여 사용자 지정할 수 있습니다.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>스토리 보드 기반 앱 만들기

StoryboardTable 예제에는 스토리 보드에서 두 가지 유형의 UITableView를 사용 하는 간단한 마스터-세부 앱이 포함 되어 있습니다. 이 섹션의 나머지 부분에서는 완료 시 다음과 같이 표시 되는 작은 할 일 목록 예제를 빌드하는 방법에 대해 설명 합니다.

 [![예제 화면](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

사용자 인터페이스는 storyboard를 사용 하 여 작성 되 고 두 화면에서는 UITableView를 사용 합니다. 주 화면에서는 *프로토타입 콘텐츠* 를 사용 하 여 행을 레이아웃 하 고 세부 정보 화면에서는 *정적 콘텐츠* 를 사용 하 여 사용자 지정 셀 레이아웃을 사용 하 여 데이터 입력 폼을 만듭니다.

## <a name="walkthrough"></a>연습

**(만들기) 새 프로젝트를 사용 하 여 Visual Studio에서 새 솔루션을 만듭니다. 단일 뷰 앱 (C#)을 >** 하 고 _StoryboardTables_를 호출 합니다.

 [![새 프로젝트 만들기 대화 상자](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

솔루션이 열리고 C# `Main.storyboard` 파일이 이미 생성 됩니다. IOS 디자이너에서 `Main.storyboard` 파일을 두 번 클릭 하 여 엽니다.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>스토리 보드 수정

스토리 보드는 다음 세 단계로 편집 됩니다.

-  먼저 필요한 뷰 컨트롤러를 레이아웃 하 고 해당 속성을 설정 합니다.
-  둘째, 개체를 뷰에서 끌어서 놓아 UI를 만듭니다.
-  마지막으로, 각 뷰에 필요한 UIKit 클래스를 추가 하 고 코드에서 참조할 수 있도록 여러 컨트롤의 이름을 지정 합니다.


스토리 보드가 완료 되 면 코드를 추가 하 여 모든 작업을 수행할 수 있습니다.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>뷰 컨트롤러 레이아웃

스토리 보드의 첫 번째 변경 내용은 기존 세부 정보 보기를 삭제 하 고 UITableViewController로 바꾸는 것입니다. 다음 단계를 수행하십시오.

1.  뷰 컨트롤러의 맨 아래에 있는 막대를 선택 하 고 삭제 합니다.
2.  도구 상자에서 **탐색 컨트롤러** 와 **테이블 뷰 컨트롤러** 를 스토리 보드로 끕니다. 
3.  루트 뷰 컨트롤러에서 방금 추가 된 두 번째 테이블 뷰 컨트롤러로 segue를 만듭니다. Segue를 만들려면 *정보 셀에서* 새로 추가 된 UITableViewController로 컨트롤을 끌어 놓습니다. **Segue 선택**아래에서 **표시** 옵션을 선택 합니다. 
4.  만든 새 segue를 선택 하 고 코드에서이 segue를 참조 하는 식별자를 제공 합니다. Segue을 클릭 하 고 다음과 `TaskSegue` 같이 **Properties Pad**에 **식별자** 를 입력 합니다.    
  [![속성 패널의 이름 지정 segue](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. 그런 다음 두 테이블 뷰를 선택 하 고 Properties Pad 사용 하 여 구성 합니다. 보기를 선택 하 고 보기 컨트롤러를 선택 하지 마십시오. 문서 개요를 사용 하 여 선택 항목에 대 한 도움말을 볼 수 있습니다.

6.  루트 뷰 컨트롤러 **를 콘텐츠로 변경 합니다. 동적 프로토타입** (Design Surface에 대 한 뷰에는 **프로토타입 콘텐츠가** 레이블이 표시 됨):

    [![Content 속성을 동적 프로토타입으로 설정](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  새 **uitableviewcontroller** 를 **콘텐츠로 변경 합니다. 정적 셀**. 


8. 새 UITableViewController에는 클래스 이름과 식별자가 설정 되어 있어야 합니다. 뷰 컨트롤러를 선택 하 고 **Properties Pad** **클래스** 에 대해 _TaskDetailViewController_ 를 입력 합니다. 이렇게 하면 Solution Pad에서 `TaskDetailViewController.cs` 새 파일이 만들어집니다. 아래 예제에 나와 있는 것 처럼 **StoryboardID** 를 _세부 정보_로 입력 합니다. 나중에 코드에서 C# 이 뷰를 로드 하는 데 사용 됩니다.  

    [![스토리 보드 ID 설정](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. 이제 스토리 보드 디자인 화면이 다음과 같이 표시 됩니다. 루트 뷰 컨트롤러의 탐색 항목 제목이 "Chore Board"로 변경 되었습니다.

    [![디자인 화면](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>UI 만들기

이제 뷰와 segue를 구성 했으므로 사용자 인터페이스 요소를 추가 해야 합니다.

#### <a name="root-view-controller"></a>루트 뷰 컨트롤러

먼저 아래 그림과 같이 마스터 뷰 컨트롤러에서 프로토타입 셀을 선택 하 고 **id** 를 _taskcell_으로 설정 합니다. 이는 나중에 코드에서이 UITableViewCell의 인스턴스를 검색 하는 데 사용 됩니다.

 [![셀 식별자 설정](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

다음으로, 아래 그림과 같이 새 작업을 추가 하는 단추를 만들어야 합니다.

[![탐색 모음의 막대 단추 항목](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

다음을 수행합니다. 

-  도구 상자에서 _탐색 모음의 오른쪽_으로 **막대 단추 항목** 을 끕니다.
-  **Properties Pad**의 **표시줄 단추 항목** 에서 식별자를 선택 **합니다. 를** 추가 하 여 단추를 *+* 추가 합니다. 
-  이후 단계에서 코드에서 식별할 수 있도록 이름을 지정 합니다. 사용자가 막대 단추 항목의 이름을 설정할 수 있도록 루트 뷰 컨트롤러에 클래스 이름 (예: **Itemviewcontroller**)을 지정 해야 합니다.


#### <a name="taskdetail-view-controller"></a>TaskDetail 뷰 컨트롤러

자세히 보기에는 많은 작업이 필요 합니다. 테이블 뷰 셀을 뷰로 끌어온 다음 레이블, 텍스트 보기 및 단추를 사용 하 여 채워야 합니다. 아래 스크린샷에서는 두 개의 섹션이 있는 완성 된 UI를 보여 줍니다. 한 섹션에는 세 개의 셀, 세 개의 레이블, 두 개의 텍스트 필드와 하나의 스위치가 있으며 두 번째 섹션에는 두 개의 단추가 있는 하나의 셀이 있습니다.

 [![세부 정보 보기 레이아웃](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

전체 레이아웃을 작성 하는 단계는 다음과 같습니다.

테이블 뷰를 선택 하 고 **속성 패드**를 엽니다. 다음 속성을 업데이트 합니다.

-  **섹션**: _sr-2_ 
-  **스타일**: _속성별로_
-  **구분 기호**: _없음_
-  **선택**: _선택 항목 없음_

아래 그림과 같이 위쪽 섹션을 선택 하 고 **속성 > 테이블 뷰 섹션** 에서 **행** 을 _3_으로 변경 합니다.


 [![위쪽 섹션을 세 개의 행으로 설정](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

각 셀에 대해 **Properties Pad** 을 열고 다음을 설정 합니다.

-  **스타일**:  _사용자 지정_
-  **식별자**: 각 셀에 대 한 고유 식별자를 선택 합니다 (예: "_제목_", "_메모_", "_완료_").
-  필요한 컨트롤을 끌어 스크린샷에 표시 된 레이아웃 ( **UILabel**, **uitextfield** 및 **UISwitch** 를 올바른 셀에 생성 하 고 레이블을 적절 하 게 설정 합니다. 제목, 메모 및 완료).


두 번째 섹션에서 **행** 을 _1_ 로 설정 하 고 셀의 아래쪽 크기 조정 핸들을 사용 하 여 셀을 더 길게 만듭니다.

-  **식별자**를 고유한 값으로 설정 합니다 (예: "저장"). 
-  **배경 설정**:  _색의 선택을 취소_ 합니다.
-  아래 그림과 같이 두 단추를 셀로 끌고 해당 제목을 적절 하 게 설정 합니다 (예: _저장_ 및 _삭제_).

   [![아래쪽 섹션에서 두 단추 설정](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

이때 셀과 컨트롤에 대 한 제약 조건을 설정 하 여 적응 레이아웃을 확인할 수도 있습니다.

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit 클래스 및 명명 컨트롤 추가

스토리 보드를 만드는 마지막 단계는 몇 가지입니다. 먼저 코드에서 나중에 사용할 수 있도록 **id > name** 아래에 있는 각 컨트롤의 이름을 지정 해야 합니다. 다음과 같이 이름을로 합니다.

-  **제목 UITextField** : _TitleText_
-  **참고 UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **UIButton 삭제** : _DeleteButton_
-  **UIButton 저장** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>코드 추가

나머지 작업은 Mac의 Visual Studio 또는의 Windows에서 수행 됩니다 C#. 코드에 사용 되는 속성 이름은 위의 연습에서 설정한 속성을 반영 합니다.

먼저 응용 프로그램 전체에서 해당 `Chores` 값을 사용할 수 있도록 ID, 이름, 메모 및 완료 부울 값을 가져오고 설정 하는 방법을 제공 하는 클래스를 만들려고 합니다.

`Chores` 클래스에서 다음 코드를 추가 합니다.

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

다음으로에서 `UITableViewSource`상속 `RootTableSource` 되는 클래스를 만듭니다. 

이와 비 Storyboard 테이블 뷰의 차이점은 `GetView` 메서드가 `theDequeueReusableCell` 셀을 인스턴스화할 필요가 없다는 것입니다. 메서드는 항상 프로토타입 셀 (일치 하는 식별자로)의 인스턴스를 반환 합니다.

아래 코드는 `RootTableSource.cs` 파일에서 가져온 것입니다.

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

`RootTableSource` 클래스를 사용 하려면 `ItemViewController`의 생성자에 새 컬렉션을 만듭니다.

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

에서 `ViewWillAppear` 컬렉션을 원본에 전달 하 고 테이블 뷰에 할당 합니다.

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

이제 앱을 실행 하면 주 화면이 로드 되어 두 작업의 목록이 표시 됩니다. 작업이 수행 되 면 storyboard에 의해 정의 된 segue가 세부 정보 화면을 표시 하지만 현재 데이터를 표시 하지 않습니다.

Segue의 ' 매개 변수 보내기 '로 `PrepareForSegue` 메서드를 재정의 하 고 `DestinationViewController` ( `TaskDetailViewController` 이 예제에서는)에서 속성을 설정 합니다. 대상 뷰 컨트롤러 클래스가 인스턴스화되어 사용자에 게 아직 표시 되지 않습니다. 즉, 클래스에서 속성을 설정할 수 있지만 UI 컨트롤을 수정할 수는 없습니다.

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

메서드에서 `TaskDetailViewController` 는`SetTask` ViewWillAppear에서 참조할 수 있도록 매개 변수를 속성에 할당 합니다. 가 호출 될 때 `SetTask` `PrepareForSegue` 가 없을 수 있으므로에서 컨트롤 속성을 수정할 수 없습니다.

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

이제 segue가 세부 정보 화면을 열고 선택한 작업 정보를 표시 합니다. 그러나 **저장** 및 **삭제** 단추에 대 한 구현은 없습니다. 단추를 구현 하기 전에 다음 메서드를 **ItemViewController.cs** 에 추가 하 여 기본 데이터를 업데이트 하 고 세부 정보 화면을 닫습니다.

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

다음으로 `TouchUpInside` **TaskDetailViewController.cs**의 `ViewDidLoad` 메서드에 단추의 이벤트 처리기를 추가 해야 합니다. 에 대 한 속성 참조는 `Delegate` 특별히 `SaveTask` 생성 `DeleteTask`되었으므로및를 호출 하 여 작업의 일부로이 뷰를 닫을 수 있습니다. `ItemViewController`

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

빌드에 대 한 마지막 남은 기능은 새 작업을 만드는 것입니다. **ItemViewController.cs** 에서 새 작업을 만들고 자세히 보기를 여는 메서드를 추가 합니다. 스토리 보드에서 뷰를 인스턴스화하려면 해당 보기 `InstantiateViewController` `Identifier` 에 대 한와 함께 메서드를 사용 합니다 .이 예제에서는 ' 세부 정보 '가 됩니다.

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

마지막으로 **ItemViewController.cs**의 `ViewDidLoad` 메서드에서 탐색 모음의 단추를 연결 하 여 호출 합니다.

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

스토리 보드 예제를 완료 합니다. 완성 된 앱은 다음과 같습니다.

[![완성 된 앱](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

예제에서는 다음을 보여 줍니다.

-  데이터 목록을 표시 하기 위해 다시 사용 하도록 셀이 정의 된 프로토타입 콘텐츠가 있는 테이블을 만듭니다. 
-  정적 콘텐츠를 사용 하 여 테이블을 만들어 입력 폼을 작성 합니다. 여기에는 테이블 스타일 변경 및 섹션, 셀 및 UI 컨트롤 추가가 포함 됩니다. 
-  Segue를 만들고 메서드를 `PrepareForSegue` 재정의 하 여 필요한 매개 변수의 대상 뷰에 알리도록 하는 방법입니다. 
-  `Storyboard.InstantiateViewController` 메서드를 사용 하 여 스토리 보드 뷰를 직접 로드 합니다.



## <a name="related-links"></a>관련 링크

- [StoryboardTable (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
