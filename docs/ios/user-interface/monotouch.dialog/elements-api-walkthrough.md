---
title: 요소 API를 사용 하 여 Xamarin.iOS 응용 프로그램 만들기
description: 이 문서에서는 MonoTouch 대화 문서에 대 한 소개에서 정보를 구축 합니다. MonoTouch.Dialog (MT를 사용 하는 방법을 보여 주는 연습 표시 D) 요소 API 빠르게 산을 사용 하 여 응용 프로그램 빌드 4.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 1b4263e37e6d95c03e88905319cfe0ee167cb30b
ms.sourcegitcommit: 85c45dc28ab3625321c271804768d8e4fce62faf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67039699"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>요소 API를 사용 하 여 Xamarin.iOS 응용 프로그램 만들기

_이 문서에서는 MonoTouch 대화 문서에 대 한 소개에서 정보를 구축 합니다. MonoTouch.Dialog (MT를 사용 하는 방법을 보여 주는 연습 표시 D) 요소 API 빠르게 산을 사용 하 여 응용 프로그램 빌드 4._

이 연습에서는 MT를 작업 목록을 표시 하는 응용 프로그램의 마스터-세부 스타일을 만들려면 D 요소 API입니다. 사용자가 선택 하는 경우는 <span class="ui"> + </span> 단추 탐색 모음에서 새 행이 작업에 대 한 테이블에 추가 됩니다. 아래 그림과 같이 작업 설명 및 지불 하기로 한 날짜를 업데이트할 수 있도록 하는 세부 정보 화면으로 이동 됩니다 행을 선택 합니다.

 [![](elements-api-walkthrough-images/01-task-list-app.png "작업 설명과 지불 하기로 한 날짜를 업데이트할 수 있는 세부 정보 화면으로 이동 됩니다 행을 선택")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>산 설정 D

산 D는 Xamarin.iOS를 사용 하 여 배포 됩니다. 를 사용 하려면 마우스 오른쪽 단추로 클릭 합니다 **참조** Xamarin.iOS 노드의 Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트 및 참조를 추가 합니다 **MonoTouch.Dialog 1** 어셈블리. 그런 다음 추가 `using MonoTouch.Dialog` 필요에 따라 원본에서 문을 코딩 합니다.

## <a name="elements-api-walkthrough"></a>요소 API 연습

에 [MonoTouch 대화 소개](~/ios/user-interface/monotouch.dialog/index.md) 문서 얻은 산의 여러 부분을 확실 하 게 이해 4. 응용 프로그램에 함께 배치 하는 요소 API를 사용해 보겠습니다.

## <a name="setting-up-the-multi-screen-application"></a>다중 화면 응용 프로그램 설정

화면 만들기 프로세스를 시작 하려면 MonoTouch.Dialog 만듭니다는 `DialogViewController`, 다음 추가 `RootElement`.

MonoTouch.Dialog를 사용 하 여 다중 화면 응용 프로그램을 만들려고 해야 합니다.

1.  만들기를 `UINavigationController.`
1.  만들기를 `DialogViewController.`
1.  추가 된 `DialogViewController` 의 루트로 합니다  `UINavigationController.` 
1.  추가 된 `RootElement` 에  `DialogViewController.`
1.  추가 `Sections` 고 `Elements` 에  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>UINavigationController를 사용 하 여

탐색 스타일 응용 프로그램을 만들려면 생성 해야는 `UINavigationController`로 추가한를 `RootViewController` 에 `FinishedLaunching` 메서드의 `AppDelegate`합니다. 있도록를 `UINavigationController` MonoTouch.Dialog, 추가 사용을 `DialogViewController` 에 `UINavigationController` 아래와 같이:

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    _rootElement = new RootElement ("To Do List"){new Section ()};

    // code to create screens with MT.D will go here …

    _rootVC = new DialogViewController (_rootElement);
    _nav = new UINavigationController (_rootVC);
    _window.RootViewController = _nav;
    _window.MakeKeyAndVisible ();
            
    return true;
}
```

위 코드의 인스턴스를 만듭니다는 `RootElement` 에 전달 된 `DialogViewController`. 합니다 `DialogViewController` 항상를 `RootElement` 계층 구조의 맨 위에 있는 합니다. 이 예제는 `RootElement` "할 일 모음" 탐색 컨트롤러의 탐색 모음에서 제목으로 사용 되는 문자열을 사용 하 여 만들어집니다. 이 시점에서 응용 프로그램을 실행 초래 아래에 표시 된 화면:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "응용 프로그램을 실행 여기에 표시 된 화면이 나타납니다.")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

MonoTouch.Dialog의 계층 구조를 사용 하는 방법을 알아보겠습니다 `Sections` 고 `Elements` 자세한 화면을 추가 합니다.

### <a name="creating-the-dialog-screens"></a>대화 상자 화면 만들기

A `DialogViewController` 되는 `UITableViewController` MonoTouch.Dialog 화면을 추가 하는 데 사용 하는 하위 클래스입니다. MonoTouch.Dialog 추가 하 여 화면을 만듭니다는 `RootElement` 에 `DialogViewController`위에서 말한 것 처럼, 합니다. 합니다 `RootElement` 있습니다 `Section` 테이블의 섹션을 나타내는 인스턴스.
섹션으로 이루어져 있습니다 요소, 섹션 또는 다른에 다른 `RootElements`합니다. 중첩 하 여 `RootElements`, 앞으로 살펴보겠지만 다음 자동으로 MonoTouch.Dialog 탐색 스타일 응용 프로그램을 만듭니다.

### <a name="using-dialogviewcontroller"></a>DialogViewController를 사용 하 여

`DialogViewController`, 되는 `UITableViewController` 하위 클래스에는 `UITableView` 해당 뷰로 합니다. 이 예제에서는 때마다 테이블에 항목을 추가 하려고 합니다 <span class="ui"> + </span> 단추를 탭 할 합니다. 하므로 `DialogViewController` 에 추가 된를 `UINavigationController`, 사용할 수 있습니다는 `NavigationItem`의 `RightBarButton` 추가할 속성을 <span class="ui"> + </span> 단추를 아래와 같이:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

만들 때 합니다 `RootElement` 이전에 전달 하는 단일 `Section` 요소를 추가할 수 있도록 인스턴스를 <span class="ui"> + </span> 사용자가 단추를 탭 할 합니다. 에서는 단추에 대 한 처리기를 수행 하려면 다음 코드를 따르면 됩니다.

```csharp
_addButton.Clicked += (sender, e) => {                
    ++n;
                
    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
    var taskElement = new RootElement (task.Name) {
        new Section () {
            new EntryElement (task.Name, "Enter task description", task.Description)
        },
        new Section () {
            new DateElement ("Due Date", task.DueDate)
        }
    };
    _rootElement [0].Add (taskElement);
};
```

이 코드에서는 새 `Task` 단추를 탭 할 때마다 개체입니다. 다음의 간단한 구현을 보여 줍니다는 `Task` 클래스:

```csharp
public class Task
{   
    public Task ()
    {
    }
      
    public string Name { get; set; }
        
    public string Description { get; set; }

    public DateTime DueDate { get; set; }
}
```

작업의 `Name` 속성은 만드는 데 합니다 `RootElement`의 라는 카운터 변수 함께 캡션 `n` 각 새 태스크에 대해 증가 되 합니다. MonoTouch.Dialog에 추가 되는 행에 요소를 설정 합니다 `TableView` 때 각 `taskElement` 추가 됩니다.

## <a name="presenting-and-managing-dialog-screens"></a>표시 및 관리 대화 상자 화면

사용 된 `RootElement` MonoTouch.Dialog 자동으로 각 작업의 세부 정보에 대 한 새 화면을 만들고 행을 선택 하는 경우를 탐색할 수 있도록 합니다.

두 섹션;으로 구성 된 자체 작업 세부 정보 화면 이러한 각 섹션에는 단일 요소를 포함 합니다. 첫 번째 요소에서 생성 되는 `EntryElement` 작업의 행을 편집할 수 있도록 `Description` 속성입니다. 요소를 선택 하면 텍스트 편집에 대 한 키보드는 아래와 같이 표시 됩니다.

 [![](elements-api-walkthrough-images/03-create-task.png "요소를 선택 하는 경우 텍스트 편집에 대 한 키보드와 같이 표시 됩니다.")](elements-api-walkthrough-images/03-create-task.png#lightbox)

두 번째 섹션에서는 `DateElement` 태스크의 관리를 수 있게 해 주는 `DueDate` 속성입니다. 날짜를 선택 하면 자동으로 표시 된 대로 날짜 선택기를 로드 합니다.

 [![](elements-api-walkthrough-images/04-date-picker.png "날짜 선택기로 로드 날짜를 선택 하면 자동으로")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

둘 다에 `EntryElement` 및 `DateElement` 사례, MonoTouch.Dialog에 있는 모든 데이터 입력 요소에 대 한 값으로 변경 내용을 자동으로 유지 됩니다. 날짜를 편집 하 고 루트 화면 및 세부 정보 화면에 있는 값은 유지 하는 위치를 다양 한 작업 세부 정보 간에 앞뒤로 탐색 하 여이 보여줄 수 했습니다.

## <a name="summary"></a>요약

이 문서에서는 MonoTouch.Dialog 요소 API를 사용 하는 방법을 보여 주는 연습을 제공 합니다. 산을 사용 하 여 다중 화면 응용 프로그램을 만드는 기본 단계에 설명 했습니다. D를 사용 하는 방법 등을 `DialogViewController` 및 화면을 만드는 요소 및 섹션을 추가 하는 방법입니다. 또한 산을 사용 하는 방법에 알아보았습니다. 와 함께에서 D는 `UINavigationController`합니다.

## <a name="related-links"></a>관련 링크

- [MTDWalkthrough (샘플)](https://developer.xamarin.com/samples/monotouch/MTDWalkthrough/)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
