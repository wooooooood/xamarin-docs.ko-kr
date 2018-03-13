---
title: "IOS 디자이너에서에서 테이블 작업"
description: "이전 섹션에서 테이블을 사용 하 여 개발을 탐색 합니다. 이 경우의 다섯 번째 및 마지막 섹션에서 지금까지 학습 한 내용 집계 하 고 합니다 스토리 보드를 사용 하 여 기본 작업 목록 응용 프로그램을 만듭니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: e46038b21327fe8847d2c04ee1ba16960f6a059b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>IOS 디자이너에서에서 테이블 작업

스토리 보드는 WYSIWYG iOS 응용 프로그램을 만드는 방법 및 Mac 및 Windows에서 Visual Studio 내에서 지원 됩니다. 스토리 보드에 대 한 자세한 내용은 참조는 [소개 스토리 보드](~/ios/user-interface/storyboards/index.md) 문서. 스토리 보드도 편집할 수 있도록 하는 셀 레이아웃 *에* 테이블 및 셀을 사용 하 여 개발을 간소화 하는 테이블

두 가지 유형의 셀 내용에서 선택할 수 없는 iOS 디자이너에서에서 테이블 보기의 속성을 구성할 때: **동적** 또는 **정적** 프로토타입 콘텐츠입니다.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>동적 프로토타입 콘텐츠

A `UITableView` 프로토타입으로 콘텐츠는 일반적으로 사용 데이터의 목록을 표시 하려면 여기서 프로토타입 셀 (또는 셀 있습니다을 정의할 수 여러 개) 목록의 각 항목에 대해 다시 사용 되 고 있습니다. 셀 인스턴스화할 필요 하지 않습니다에서 가져온는 `GetView` 메서드를 호출 하 여는 `DequeueReusableCell` 방식의 해당 `UITableViewSource`합니다.

 <a name="Static_Content" />


## <a name="static-content"></a>정적 콘텐츠

`UITableView`으로 정적 콘텐츠가 담긴 테이블 디자인 화면에서 오른쪽 디자인을 사용 합니다. 셀 끌어서 놓을 테이블에 속성을 변경 하 고 컨트롤을 추가 하 여 사용자 지정할 수 있습니다.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>스토리 보드 기반 응용 프로그램 만들기

StoryboardTable 예제에서는 두 가지 유형의 UITableView 스토리 보드에서 사용 하는 간단한 마스터-세부 앱을 포함 합니다. 이 섹션의 나머지 부분에서는 완료 되 면 다음과 같이 표시 됩니다는 작은 할 일 목록 예제를 빌드하는 방법을 설명 합니다.

 [![예제에서는 화면](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

사용자 인터페이스에 스토리 만들어지고 두 화면은 UITableView를 사용 합니다. 주 화면에 사용 하 여 *프로토타입 콘텐츠* 화면 레이아웃 행 및 세부 정보를 사용 하 여 *정적 콘텐츠가 담긴* 셀 사용자 지정 레이아웃을 사용 하 여 데이터 입력 양식을 만들 수 있습니다.

## <a name="walkthrough"></a>연습

Visual Studio를 사용 하 여 새 솔루션을 만들어 **(만들기) 새 프로젝트... > 단일 보기 App(C#)**, 해당 버전을 호출 _StoryboardTables_합니다.

 [![새 프로젝트 대화 상자 만들기](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

일부 C# 파일 솔루션이 열리면서 및 `Main.storyboard` 이미 만든 파일입니다. 두 번 클릭은 `Main.storyboard` iOS 디자이너에서에서 열 파일입니다.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>스토리 보드를 수정합니다.

3 단계에 따라 스토리 보드를 편집할 수 됩니다.

-  첫째, 레이아웃 필요한 컨트롤러를 확인 하 고 속성을 설정 합니다.
-  둘째, UI를 끌어서 놓는 개체를 보기에 만들
-  마지막으로 각 보기에 필요한 UIKit 클래스를 추가 하 고 코드에서 참조할 수 있도록 다양 한 컨트롤에 이름을 지정 합니다.


스토리 보드 완료 되 면 작동 하는 모든 코드를 추가할 수 있습니다.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>레이아웃 뷰 컨트롤러

스토리 보드에 첫 번째 변경 사항을 기존 세부 정보 보기를 삭제 하 고는 UITableViewController로 대체 됩니다. 아래 단계를 수행합니다.

1.  보기 컨트롤러의 아래쪽 막대를 선택 하 여 삭제 합니다.
2.  끌어서는 **탐색 컨트롤러** 및 **테이블 뷰-컨트롤러** 도구 상자에서 스토리 보드에 있습니다. 
3.  루트 뷰 컨트롤러에서 방금 추가 된 두 번째 테이블 보기 컨트롤러에는 segue를 만듭니다. 컨트롤 segue 만들 + 끌어 *세부 셀에서* 새로 추가 된 UITableViewController 하 합니다. 옵션을 선택 **표시*** 아래 **Segue 선택**합니다. 
4.  새 사용자가 만든 segue 하 고 식별자 코드에서 segue이 참조를 선택 합니다. segue 클릭 하 고 입력 `TaskSegue` 에 대 한는 **식별자** 에 **속성 패드**, 다음과 같이:    
  [![속성 패널에서 segue 이름 지정](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. 다음을 선택 하 고 속성 패드를 사용 하 여 두 테이블 뷰를 구성 합니다. 뷰와 하지 뷰-컨트롤러 선택 되어 있는지 확인-문서 개요를 사용 하 여 선택 영역에 도움이 되도록 합니다.

6.  루트 뷰 컨트롤러 되도록 변경 **콘텐츠: 동적 프로토타입** (디자인 화면의 보기 라는 레이블이 지정 **프로토타입 콘텐츠** ):

    [![콘텐츠 속성을 동적 프로토타입을 설정](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  새 변경 **UITableViewController** 되도록 **콘텐츠: 정적 셀**합니다. 


8. 새 UITableViewController 클래스 이름 및 식별자 집합에 있어야 합니다. 뷰-컨트롤러와 유형을 선택 _TaskDetailViewController_ 에 대 한는 **클래스** 에 **속성 패드** – 새 만들어집니다 `TaskDetailViewController.cs` 솔루션의 파일 패드입니다. 입력은 **StoryboardID** 으로 _세부_아래 예에서 같이, 합니다. 이 나중에 C# 코드에서이 뷰를 로드할 수 있습니다:  

    [![스토리 보드 ID 설정](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. 스토리 보드 디자인 화면은 이제 다음과 같이 표시 됩니다 (루트 뷰 컨트롤러 탐색 항목 제목 변경 된 경우 "작업 보드"):

    [![디자인 화면](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>UI 만들기

이제는 뷰 segues 및는 구성, 사용자 인터페이스 요소를 추가 해야 합니다.

#### <a name="root-view-controller"></a>루트 뷰 컨트롤러

먼저 마스터 뷰 컨트롤러에서 프로토타입 셀을 선택 하 고 설정 된 **식별자** 으로 _taskcell_아래 그림과 같이, 합니다. 이 나중에 코드에서에서이 UITableViewCell의 인스턴스를 검색 합니다.

 [![셀 식별자를 설정합니다.](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

다음으로 아래 그림과 같이 새 작업을 추가 합니다 있는 단추를 만듭니다 해야 합니다.

[![막대 탐색 모음에서 단추 항목](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

다음을 수행합니다. 

-  끌어서는 **표시줄 단추 항목** 도구 상자에서는 _의 탐색 모음 오른쪽_합니다.
-  에 **속성 패드**아래 **표시줄 단추 항목** 선택 **식별자: 추가** (있도록는  *+*  더하기 단추). 
-  이후 단계에서 코드에서 식별할 수 있도록 이름을 지정 합니다. 참고 루트 뷰 컨트롤러 클래스 이름을 지정 해야 합니다 (예를 들어 **ItemViewController**) 막대 단추 항목의 이름을 설정할 수 있도록 합니다.


#### <a name="taskdetail-view-controller"></a>TaskDetail 뷰-컨트롤러

세부 정보 보기에는 더 많은 작업이 필요합니다. 표 보기 셀 뷰를 끌어 및 레이블, 텍스트 뷰 및 단추 채워진 다음 필요 합니다. 아래 스크린샷에서 두 섹션이 포함 된 완료 된 UI를 표시 합니다. 하나의 섹션에 세 개의 셀, 레이블을 3 개, 두 번째 섹션에 있는 두 개의 단추를 사용 하 여 한 셀 하는 동안 두 개의 텍스트 필드와 한 전환할:

 [![세부 정보 보기 레이아웃](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

전체 레이아웃 작성 하는 단계는:

테이블 뷰를 선택 하 고 엽니다는 **속성 패드**합니다. 다음 속성을 업데이트 합니다.

-  **섹션**: _2_ 
-  **스타일**: _그룹화_
-  **구분 기호**: _없음_
-  **선택 영역**: _선택_

맨 위 섹션에 선택 고 **속성 > 테이블 뷰 섹션** 변경 **행** 를 _3_아래 그림과 같이,:


 [![3 개의 행을 맨 위 섹션에 설정](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

열린 각 셀에 대 한는 **속성 패드** 설정:

-  **스타일**: _사용자 지정_
-  **식별자**: 않음 (예: 각 셀에 대 한 고유 식별자 선택 "_제목_","_메모_","_수행_").
-  스크린샷에 표시 된 레이아웃을 생성 하기 위해 필요한 컨트롤을 끌어 (배치 **UILabel**, **UITextField** 및 **UISwitch** 올바른 수의 셀에 레이블을 설정 하 고 적절 하 게 ie 합니다. 제목, 메모 수행).


두 번째 섹션에서는 설정 **행** 를 _1_ 높이를 셀의 아래쪽 크기 조정 핸들을 선택 하 고 있습니다.

-  **식별자를 설정**:을 고유한 값 (예:. "저장"). 
-  **배경 설정**: _색 지우기_ 합니다.
-  두 개의 단추 셀으로 끌어서 직함을 적절 하 게 설정 (예: _저장_ 및 _삭제_) 아래 그림과 같이,:

   [![아래쪽 섹션에 두 단추 설정](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

이 시점에서 셀과 적응 레이아웃을 확인 하는 컨트롤에 대 한 제약 조건을 설정할 수도 있습니다.

### <a name="adding-uikit-class-and-naming-controls"></a>UIKit 클래스를 추가 하 고 컨트롤 이름 지정

이 스토리 보드를 만드는 몇 가지 최종 단계가 있습니다. 먼저 우리 각 우리의 컨트롤에 이름을 지정 해야 아래 **Identity > 이름** 이므로 나중에 코드에서 사용할 수 있습니다. 이러한 이름을 다음과 같습니다.

-  **UITextField 제목** : _TitleText_
-  **UITextField 정보** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **UIButton 삭제** : _DeleteButton_
-  **UIButton 저장** : _저장 단추_


<a name="Adding_Code" />

## <a name="adding-code"></a>코드 추가

작업의 나머지 부분에서는 Mac 또는 C#으로 Windows에서 Visual Studio에서 수행 됩니다. Note 코드에서 사용 되는 속성 이름과 위의 연습에서 설정 하는 것을 반영 합니다.

먼저 만들려고는 `Chores` 클래스를 응용 프로그램 전반에서 해당 값을 사용할 수 있도록 가져오고 ID, 이름, 메모 및 수행 Boolean의 값을 설정 하는 방법을 제공 합니다.

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

이 스토리 보드 아닌 테이블 뷰 간의 차이 `GetView` 메서드 어떤 셀도 –를 인스턴스화할 필요가 없습니다 `theDequeueReusableCell` 메서드는 항상 (일치 하는 식별자)가 있는 프로토타입 셀의 인스턴스를 반환 합니다.

아래 코드에서이 `RootTableSource.cs` 파일:

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

`ViewWillAppear` 소스 컬렉션을 전달 하 고 테이블 뷰를 할당 합니다.

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

응용 프로그램을 실행 하는 경우 이제 기본 화면 그러면 이제 로드 되 고 두 작업의 목록을 표시 합니다. 작업에 접촉 될 때 스토리 보드에 정의 된 segue이 세부 정보 화면을 표시 하지만 현재 데이터가 표시 되지 않습니다.

'매개 변수 '는 segue에 보내려면 하려면 재정의 `PrepareForSegue` 메서드의 속성을 설정 하 고는 `DestinationViewController` (의 `TaskDetailViewController` 이 예에서). 대상 뷰-컨트롤러 클래스는 인스턴스화할 하지 않으면이 아직 – 사용자에 게 표시 합니다. 즉, 클래스에 속성을 설정할 수 있지만 UI 컨트롤을 수정 하지:

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

`TaskDetailViewController` 는 `SetTask` 메서드 매개 변수 속성 할당 ViewWillAppear에서 참조할 수 있습니다. 컨트롤 속성에서 수정할 수 없습니다 `SetTask` 때문에 없을 수 있습니다 때 `PrepareForSegue` 호출 됩니다.

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

segue 이제 세부 정보 화면 열리고 선택한 작업 정보를 표시 합니다. 하지만 구현은 없습니다에 대 한는 **저장** 및 **삭제** 단추입니다. 단추를 구현 하기 전에 이러한 메서드를 추가 **ItemViewController.cs** 기본 데이터를 업데이트 하는 세부 정보 화면을 닫습니다.

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

단추를 추가 해야 하는 다음으로 `TouchUpInside` 에 이벤트 처리기는 `ViewDidLoad` 메서드 **TaskDetailViewController.cs**합니다. `Delegate` 속성 참조를는 `ItemViewController` 호출할 수 있도록 특별히 만들어진 `SaveTask` 및 `DeleteTask`, 해당 작업의 일부로이 뷰를 닫아입니다:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

마지막으로 남은 빌드할 수 있는 기능 할 작업은 새 작업 만들기입니다. **ItemViewController.cs** 추가 새 만들어 작업 메서드와 세부 정보 보기를 엽니다. 사용 하는 스토리 보드에서 보기를 인스턴스화하는 `InstantiateViewController` 메서드는 `Identifier` '정보' 될이 예에서 해당 보기-:

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

탐색 모음에서 단추를 연결 하는 마지막으로, **ItemViewController.cs**의 `ViewDidLoad` 메서드를 호출할 수 있습니다.

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

스토리 보드 예제-는 완성 된 응용 프로그램은 다음과 같습니다 완료:

[![완성 된 응용 프로그램](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

예를 보여 줍니다.

-  셀 데이터의 목록을 표시 하려면 다시 사용할 정의 되어 있는 프로토타입 콘텐츠를 사용 하 여 테이블 만들기 
-  입력된 폼을 빌드하는 정적 콘텐츠를 사용 하 여 테이블 만들기 테이블 스타일을 변경 하 고 섹션, 셀 및 UI 컨트롤을 추가 합니다.이 포함 됩니다. 
-  segue 만들고 재정의 하는 방법의 `PrepareForSegue` 메서드 매개 변수가 필요한 모든 대상 보기에 알리기 위해. 
-  스토리 보드 보기와 직접 로드 하는 `Storyboard.InstantiateViewController` 메서드.



## <a name="related-links"></a>관련 링크

- [StoryboardTable (샘플)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Storyboards 소개](~/ios/user-interface/storyboards/index.md)
