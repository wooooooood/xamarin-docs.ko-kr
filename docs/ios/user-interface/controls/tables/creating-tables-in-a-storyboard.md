---
title: IOS 디자이너에서에서 테이블 작업
description: 이전 섹션에서 테이블을 사용 하 여 개발 하는 것이 살펴보았습니다. 이 다섯 번째이자 마지막 섹션 내용에서는 지금까지 배운 집계 하 고 스토리 보드를 사용 하 여 기본 작업 목록 응용 프로그램을 만듭니다.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d161a267c8ffa5040327db8e6e4f867a324b04f2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105809"
---
# <a name="working-with-tables-in-the-ios-designer"></a>IOS 디자이너에서에서 테이블 작업

스토리 보드는 wysiwyg (위지윅) iOS 응용 프로그램을 만드는 방법 및 Mac 및 Windows에서 Visual Studio 내에서 지원 됩니다. 스토리 보드에 대 한 자세한 내용은 참조는 [소개에 스토리 보드](~/ios/user-interface/storyboards/index.md) 문서. 스토리 보드도 편집할 수 있도록 셀 레이아웃 *에서* 테이블 및 셀을 사용 하 여 개발을 간소화 하는 테이블

IOS 디자이너에서에서 테이블 보기의 속성을 구성할 때 두 가지가 셀 내용을 선택할 수 있습니다: **동적** 하거나 **정적** 프로토타입 콘텐츠입니다.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>동적 프로토타입 콘텐츠

`UITableView` 프로토타입으로 콘텐츠 데이터의 목록을 표시 하기 위해 일반적으로 여기서 프로토타입 셀 (또는 셀 수로 정의할 수 둘 이상의) 목록의 각 항목에 대 한 다시 사용 됩니다. 셀 인스턴스화할 필요가 없습니다를 얻을 수 있습니다. 이러한 합니다 `GetView` 메서드를 호출 합니다 `DequeueReusableCell` 메서드의 해당 `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>정적 콘텐츠

`UITableView`정적 콘텐츠를 사용 하 여 s 테이블 디자인 화면에서 바로 디자인을 허용 합니다. 셀 테이블로 끌어 하 고 속성을 변경 하 고 컨트롤을 추가 하 여 사용자 지정할 수 있습니다.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>스토리 보드 기반 앱 만들기

StoryboardTable 예제에서는 두 가지 유형의 UITableView 스토리 보드에 사용 하는 간단한 마스터-세부 앱을 포함 합니다. 이 섹션의 나머지 부분에는 완료 되 면 다음과 같이 표시 됩니다는 작은 할 일 목록 예제를 빌드하는 방법을 설명 합니다.

 [![예제 화면](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

사용자 인터페이스를 스토리 보드를 사용 하 여 빌드되고 두 화면을 UITableView를 사용 합니다. 사용 하 여 주 화면 *프로토타입 콘텐츠* 화면 레이아웃 행 및 세부 정보를 사용 하 여 *정적 콘텐츠* 사용자 지정 셀 레이아웃을 사용 하 여 데이터 입력 폼을 만들 합니다.

## <a name="walkthrough"></a>연습

사용 하 여 Visual Studio에서 새 솔루션을 만듭니다 **새 프로젝트... (만들기) > 단일 뷰 앱 (C#)** 를 호출 하 고 _StoryboardTables_.

 [![새 프로젝트 대화 상자 만들기](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

일부 솔루션 열립니다 C# 파일 및 `Main.storyboard` 이미 만든 파일입니다. 두 번 클릭 합니다 `Main.storyboard` 파일 iOS 디자이너에서에서 엽니다.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>스토리 보드를 수정합니다.

3 단계에 따라 스토리 보드를 편집할 수 됩니다.

-  첫째, 레이아웃 필요한 컨트롤러 보기 및 해당 속성을 설정 합니다.
-  둘째, 개체를 보기에 끌어서 놓아 UI 만들기
-  마지막으로 각 보기에 필요한 UIKit 클래스를 추가 하 고 코드에서 참조할 수 있도록 다양 한 컨트롤에 이름을 지정 합니다.


스토리 보드 완료 되 면 작동 하는 모든 코드를 추가할 수 있습니다.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>레이아웃 뷰 컨트롤러

스토리 보드에 첫 번째 변경 기존 세부 정보 보기를 삭제 하는 중 이며 UITableViewController을으로 바꿉니다. 아래 단계를 수행합니다.

1.  뷰 컨트롤러의 아래쪽 막대를 선택 하 여 삭제 합니다.
2.  끌어서를 **탐색 컨트롤러** 와 **테이블 뷰 컨트롤러** 도구 상자에서 스토리 보드에 있습니다. 
3.  방금 추가 된 두 번째 테이블 뷰 컨트롤러를 루트 뷰 컨트롤러에서 segue를 만듭니다. Segue를 컨트롤 만들기 + 드래그 *세부 정보 셀에서* 새로 추가 된 UITableViewController를 합니다. 옵션을 선택 **표시*** 아래 **Segue 선택**합니다. 
4.  선택 하면 새 사용자가 만든 segue 및 코드에서 segue이 참조에 식별자를 제공 합니다. Segue 클릭 하 고 입력 `TaskSegue` 에 대 한 합니다 **식별자** 에 **Properties Pad**, 다음과 같은:    
  [![속성 패널에서 segue 명명](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. 다음을 선택 하 고 속성 패드를 사용 하 여 두 테이블 뷰를 구성 합니다. 뷰와 뷰 컨트롤러가 아닌 선택 되어 있는지 확인 – 선택에 도움이 되도록 문서 개요를 사용할 수 있습니다.

6.  루트 뷰 컨트롤러에 액세스할 수 변경 **콘텐츠: 동적 프로토타입** (디자인 화면의 보기를 레이블이 지정 됩니다 **프로토타입 콘텐츠** ):

    [![콘텐츠 속성을 동적 프로토타입을 설정](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  새 변경 **UITableViewController** 되도록 **콘텐츠: 정적 셀**합니다. 


8. 클래스 이름 및 식별자를 설정할 새 UITableViewController 있어야 합니다. 뷰 컨트롤러와 유형을 선택 _TaskDetailViewController_ 에 대 한 합니다 **클래스** 에 **Properties Pad** – 새 만들어집니다 `TaskDetailViewController.cs` 솔루션의 파일 패드입니다. 입력 된 **StoryboardID** 으로 _세부 정보_아래 예제에 설명 된 대로, 합니다. 이 사용 됩니다 나중에이 보기를 로드 하려면 C# 코드:  

    [![Storyboard ID를 설정합니다.](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. 스토리 보드 디자인 화면에 이제이 처럼 보여야 (루트 뷰 컨트롤러의 탐색 항목 제목으로 변경 되었습니다 "번거로운 작업이 보드"):

    [![디자인 화면](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>UI 만들기

이제 뷰 segue 및는 구성, 사용자 인터페이스 요소를 추가 해야 합니다.

#### <a name="root-view-controller"></a>루트 뷰 컨트롤러

먼저 마스터 뷰 컨트롤러에서 프로토타입 셀을 선택 하 고 설정 합니다 **식별자** 으로 _taskcell_아래 그림과 같이 합니다. 이 나중에 코드에서에서이 UITableViewCell의 인스턴스를 검색 합니다.

 [![셀 식별자를 설정합니다.](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

그런 다음 아래 그림과 같이 새 태스크를 추가 하는 단추를 만들려면 해야 합니다.

[![탐색 모음에서 단추 항목 모음](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

다음을 수행합니다. 

-  끌어서를 **모음 단추 항목** 도구 상자에서를 _탐색 표시줄의 오른쪽_합니다.
-  에 **Properties Pad**아래에 있는 **표시줄 단추 항목** 선택 **식별자: 추가** (있도록를 *+* 단추와). 
-  이후 단계에서 코드에서 식별할 수 있도록 이름을 지정 합니다. 참고 루트 뷰 컨트롤러 클래스 이름을 지정 해야 합니다 (예를 들어 **ItemViewController**) 막대 단추 항목의 이름을 설정할 수 있도록 합니다.


#### <a name="taskdetail-view-controller"></a>TaskDetail 뷰 컨트롤러

세부 정보 보기에는 많은 작업이 필요합니다. 테이블 뷰 셀 뷰로 끌어서 레이블과 텍스트 뷰 단추를 사용 하 여 입력 한 다음 필요 합니다. 다음 스크린샷은 두 섹션을 사용 하 여 완성 된 UI를 보여 줍니다. 하나의 섹션에 있는 레이블 세 개 있는 세 개의 셀, 두 번째 섹션에는 두 개의 단추를 사용 하 여 하나의 셀에 있는 동안 두 텍스트 필드와 한 전환:

 [![세부 정보 보기 레이아웃](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

전체 레이아웃을 구축 하는 단계는 다음과 같습니다.

테이블 보기를 선택 하 고 엽니다는 **속성 패드**합니다. 다음 속성을 업데이트 합니다.

-  **섹션**: _2_ 
-  **스타일**: _그룹화_
-  **구분 기호**: _없음_
-  **선택 영역**: _선택 항목 없음_

맨 위 섹션을 선택 및 아래 **속성 > 테이블 뷰 섹션** 변경 **행** 하 _3_아래 그림과 같이:


 [![세 개의 행을 맨 위 섹션 설정](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

열린 각 셀에 대 한 합니다 **Properties Pad** 설정:

-  **스타일**: _사용자 지정_
-  **식별자**: (예: 각 셀에 대 한 고유 식별자를 선택 합니다. "_제목_","_노트_","_완료_").
-  스크린샷에 표시 된 레이아웃을 생성 하기 위해 필요한 컨트롤을 끌어 (배치 **UILabel**, **UITextField** 하 고 **UISwitch** 올바른 셀 레이블을 설정 적절 하 게 ie입니다. 제목, 메모 및 완료).


두 번째 섹션에서는 설정 **행** 하 _1_ 더 길게 셀의 아래쪽 크기 조정 핸들을 선택 하 고 있습니다.

-  **식별자를 설정할**: 고유한 값 (예: "저장"). 
-  **배경색을 설정**: _색 지우기_ 합니다.
-  두 개의 단추 셀으로 끌어서 직함을 적절 하 게 설정 (예: _저장_ 하 고 _삭제_) 아래 그림과 같이:

   [![아래쪽 섹션에서 두 개의 단추를 설정합니다.](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

이 시점에서 셀 및 적응 레이아웃을 확인 하는 컨트롤에 대 한 제약 조건을 설정할 수도 있습니다.

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit 클래스를 추가 하 고 컨트롤 이름 지정

이 스토리 보드를 만드는 마지막 단계는 몇 가지 있습니다. 먼저에서는 각 컨트롤이 여기에 이름을 지정 해야 아래 **Identity > 이름** 되므로 나중에 코드에서 사용할 수 있습니다. 다음과 같이 이름:

-  **제목 UITextField** : _TitleText_
-  **정보 UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **UIButton 삭제할** : _DeleteButton_
-  **저장 UIButton** : _저장 단추_


<a name="Adding_Code" />

## <a name="adding-code"></a>코드를 추가합니다.

작업의 나머지 부분을 사용 하 여 Windows 또는 Mac에서 Visual Studio에서 수행 됩니다 C#입니다. 위의 연습에서 설정 된 코드에서 사용 된 속성 이름이 반영 되도록 note 합니다.

먼저 만들려는 `Chores` 클래스에서는 응용 프로그램 전체에서 해당 값을 사용할 수 있도록 가져오고 ID, 이름, 메모 및 완료 Boolean의 값을 설정 하는 방법을 제공 합니다.

사용자 `Chores` 클래스에 다음 코드를 추가 합니다.

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

다음으로 만듭니다는 `RootTableSource` 클래스에서 상속 되는 `UITableViewSource`합니다. 

이 뷰와 Storyboard가 아닌 테이블 간의 차이점은는 `GetView` 메서드를 어떤 셀도 – 인스턴스화할 필요가 없습니다 `theDequeueReusableCell` 메서드는 항상 (일치 하는 식별자)가 있는 프로토타입 셀의 인스턴스를 반환 합니다.

아래 코드에서는 `RootTableSource.cs` 파일:

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

사용 하 여 `RootTableSource` 클래스, 새 컬렉션을 만들는 `ItemViewController`의 생성자:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

`ViewWillAppear` 컬렉션 원본에 전달 하 고 테이블 보기를 할당 합니다.

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

앱을 실행 하는 경우 이제 기본 화면 이제 로드를 두 작업의 목록을 표시 합니다. 작업을 연결 하는 경우 스토리 보드를 정의한 segue 세부 정보 화면을 표시 하면 있지만 지금은 모든 데이터가 표시 되지 않습니다.

' 매개 변수 '를 보내도록 segue를 재정의 합니다 `PrepareForSegue` 메서드에 속성을 설정 합니다 `DestinationViewController` (를 `TaskDetailViewController` 이 예제의). 대상 뷰 컨트롤러 클래스를 인스턴스화할 아닌 아직 클래스의 속성을 설정할 수 있지만 UI 컨트롤을 수정 하지 의미이 사용자에 게 표시 합니다.

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

`TaskDetailViewController` 는 `SetTask` 메서드 ViewWillAppear에서 참조할 수 있도록 속성에 해당 매개 변수를 할당 합니다. 컨트롤 속성에서 수정할 수 없습니다 `SetTask` 때문에 없을 때 `PrepareForSegue` 라고:

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

Segue 이제 세부 정보 화면 열리고 선택한 작업 정보를 표시 합니다. 방법이 구현 하지 않습니다 합니다 **저장** 하 고 **삭제** 단추입니다. 단추를 구현 하기 전에 이러한 메서드를 추가 **ItemViewController.cs** 기본 데이터를 업데이트 하 여 세부 정보 화면을 닫습니다.

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

단추를 추가 해야 하는 다음으로, `TouchUpInside` 이벤트 처리기는 `ViewDidLoad` 메서드의 **TaskDetailViewController.cs**합니다. 합니다 `Delegate` 속성에 대 한 참조를 `ItemViewController` 를 호출할 수 있도록 특별히 만들어진 `SaveTask` 및 `DeleteTask`, 해당 작업의 일부로이 뷰를 닫고는:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

마지막 남은 부분 빌드할 수 있는 기능 경우 새 작업 만들기 **ItemViewController.cs** 만드는 새 메서드를 작업 및 세부 정보 뷰를 추가 합니다. 스토리 보드를 사용 하는 뷰를 인스턴스화하는 `InstantiateViewController` 메서드는 `Identifier` '정보' 될이 예에서 해당 보기-:

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

마지막으로, 연결의 탐색 모음에서 단추 **ItemViewController.cs**의 `ViewDidLoad` 메서드를 호출 합니다.

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

스토리 보드 예제-이 완성 된 앱 같습니다 완료:

[![완성 된 앱](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

예를 보여 줍니다.

-  셀이 다시 사용 하 여 데이터의 목록을 표시 하는 것에 대 한 정의 되어 있는 프로토타입 콘텐츠를 사용 하 여 테이블을 만드는 중입니다. 
-  입력된 양식 작성 하는 정적 콘텐츠를 사용 하 여 테이블을 만드는 중입니다. 테이블 스타일을 변경 하 고 섹션, 셀 및 UI 컨트롤을 추가이 포함 되어 있습니다. 
-  Segue를 만들고를 재정의 하는 방법의 `PrepareForSegue` 매개 변수가 필요한 모든 대상 보기를 알리는 방법입니다. 
-  스토리 보드 뷰를 직접 로드 합니다 `Storyboard.InstantiateViewController` 메서드.



## <a name="related-links"></a>관련 링크

- [StoryboardTable (샘플)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
