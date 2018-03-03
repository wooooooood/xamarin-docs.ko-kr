---
title: "연습-요소 API를 사용 하 여 응용 프로그램 만들기"
description: "이 문서 MonoTouch 대화 문서의에 제공 된 정보를 기준으로 작성 합니다. MonoTouch.Dialog (산 사용 하는 방법을 보여 주는 연습을 제공 D) 요소 API 빠르게 시작 하려면 산 사용한 응용 프로그램 빌드 4."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 19e20015d1872cbaea21dd8b8e5431981e463c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>연습-요소 API를 사용 하 여 응용 프로그램 만들기

_이 문서 MonoTouch 대화 문서의에 제공 된 정보를 기준으로 작성 합니다. MonoTouch.Dialog (산 사용 하는 방법을 보여 주는 연습을 제공 D) 요소 API 빠르게 시작 하려면 산 사용한 응용 프로그램 빌드 4._

이 연습에서 산 사용 작업 목록을 표시 하는 응용 프로그램의 마스터-세부 스타일을 만들려면 D 요소 API입니다. 사용자가 선택할 때의 <span class="ui"> + </span> 단추 탐색 모음에서 새 행의 작업에 대 한 테이블에 추가 됩니다. 행을 선택 하는 아래 그림과 같이 작업 설명 및 만료 날짜를 업데이트할 수 있는 세부 정보 화면으로 이동 됩니다.

 [ ![](elements-api-walkthrough-images/01-task-list-app.png "작업 설명 및 기한을 업데이트할 수 있는 세부 정보 화면으로 이동 되는 행을 선택 하면")](elements-api-walkthrough-images/01-task-list-app.png)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>요소 API 연습

에 [MonoTouch 대화 소개](~/ios/user-interface/monotouch.dialog/index.md) 산의 여러 부분을 확실 하 게 이해 여 얻은 문서 4. 응용 프로그램으로 모두 함께 배치할지 요소 API를 사용 하겠습니다.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>다중 화면 응용 프로그램 설정

화면 만들기 프로세스를 시작 하려면 MonoTouch.Dialog 만듭니다는 `DialogViewController`, 다음 추가 `RootElement`합니다.

MonoTouch.Dialog를 사용 하 여 다중 화면 응용 프로그램을 만들려면 해야 합니다.

1.  만들기는  `UINavigationController.`
1.  만들기는  `DialogViewController.`
1.  추가 된 `DialogViewController` 의 루트로  `UINavigationController.` 
1.  추가 `RootElement` 에  `DialogViewController.`
1.  추가 `Sections` 및 `Elements` 에  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>UINavigationController를 사용 하 여

탐색 스타일 응용 프로그램을 만들려면 만들려는 필요는 `UINavigationController`, 다음으로 추가 하 고는 `RootViewController` 에 `FinishedLaunching` 의 메서드는 `AppDelegate`합니다. 있도록는 `UINavigationController` 추가 MonoTouch.Dialog, 작동 한 `DialogViewController` 에 `UINavigationController` 아래와 같이:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
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

위의 코드의 인스턴스를 만듭니다.는 `RootElement` 에 전달 하 고는 `DialogViewController`합니다. `DialogViewController` 항상 한 `RootElement` 해당 계층 구조의 맨 위에 있는 합니다. 이 예제는 `RootElement` "할 일 목록," 탐색 컨트롤러의 탐색 모음에서 제목으로 사용 되는 문자열을 사용 하 여 만들어집니다. 이 시점에서 응용 프로그램을 실행 아래에 표시 된 화면을 제공 합니다.

 [ ![](elements-api-walkthrough-images/02-to-do-list-screen-.png "여기에 표시 된 화면을 제시 합니다 응용 프로그램 실행")](elements-api-walkthrough-images/02-to-do-list-screen-.png)

MonoTouch.Dialog의 계층 구조를 사용 하는 방법을 알아보겠습니다 `Sections` 및 `Elements` 자세한 화면을 추가 하 합니다.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>대화 상자 화면 만들기

A `DialogViewController` 는 `UITableViewController` MonoTouch.Dialog 사용 하 여 화면을 추가 하는 하위 클래스입니다. MonoTouch.Dialog 추가 하 여 화면을 만듭니다는 `RootElement` 에 `DialogViewController`위에서 살펴본 것 처럼, 합니다. `RootElement` 점이 `Section` 테이블의 섹션을 나타내는 경우에 합니다.
섹션으로 이루어져 있습니다 요소, 섹션 또는 다른에 다른 `RootElements`합니다. 중첩으로 `RootElements`, 볼 수 있겠지만, 다음 MonoTouch.Dialog는 탐색 스타일 응용 프로그램을 자동으로 만듭니다.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>DialogViewController를 사용 하 여

`DialogViewController`, 되는 `UITableViewController` 하위 클래스에는 `UITableView` 를 뷰로 합니다. 이 예제에서는 될 때마다 테이블에 항목을 추가 하려는 <span class="ui"> + </span> 단추 탭 됩니다. 이후는 `DialogViewController` 에 추가 된는 `UINavigationController`, 사용할 수는 `NavigationItem`의 `RightBarButton` 추가할 속성의 <span class="ui"> + </span> 아래와 같이 단추:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

만들 때의 `RootElement` 앞에서 우리 전달 단일 `Section` 요소를 추가할 수 있도록 인스턴스는 <span class="ui"> + </span> 단추는 사용자가 탭 합니다. 이 이벤트 단추에 대 한 처리기를 다음 코드를 사용 했습니다.

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

이 코드에서는 새 `Task` 개체 될 때마다 단추 탭 됩니다. 다음의 간단한 구현을 보여 줍니다.는 `Task` 클래스:

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

 []()

작업의 `Name` 속성 만드는 데 사용 되는 `RootElement`의 명명 된 카운터 변수 함께 캡션 `n` 새로운 각 작업에 대해 증가 하는 합니다. MonoTouch.Dialog 바뀝니다 행에 추가 하는 요소는 `TableView` 때 각 `taskElement` 추가 됩니다.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>표시 및 대화 상자 화면 관리

사용 하는 `RootElement` MonoTouch.Dialog 자동으로 각 작업의 세부 정보에 대 한 새 화면을 만들고 행을 선택 하는 경우 탐색할 수 있도록 합니다.

두 개의 섹션; 자체 작업 세부 정보 화면으로 구성 된 이러한 섹션의 각 단일 요소를 포함합니다. 만든 첫 번째 요소는 `EntryElement` 작업에 대 한 편집 가능한 행 수 있도록 `Description` 속성입니다. 요소를 선택할 때 텍스트 편집 바로 아래와 같이 표시 됩니다.

 [ ![](elements-api-walkthrough-images/03-create-task.png "텍스트 편집 바로 표시 된 것 처럼 표시 됩니다는 요소를 선택할 때")](elements-api-walkthrough-images/03-create-task.png)

두 번째 섹션에 포함 되어는 `DateElement` 우리는 작업을 관리할 수 있는 `DueDate` 속성입니다. 날짜를 선택 하면 자동으로 표시 된 것 처럼 날짜 선택을 로드 합니다.

 [ ![](elements-api-walkthrough-images/04-date-picker.png "날짜 선택으로 로드 날짜를 선택 하면 자동으로")](elements-api-walkthrough-images/04-date-picker.png)

모두에 `EntryElement` 및 `DateElement` 의 경우 (또는 MonoTouch.Dialog에 있는 모든 데이터 입력 요소에 대 한) 값의 변경 내용은 자동으로 유지 됩니다. 날짜를 편집 및 루트 화면 및 세부 정보 화면에서 값의 유지 여기서 다양 한 작업 세부 정보, 간에 앞뒤로 탐색 하 여이 보여줄 수 있습니다.

 <a name="Summary" />


## <a name="summary"></a>요약

이 문서 MonoTouch.Dialog 요소 API를 사용 하는 방법을 보여 주는 연습을 제공 합니다. 산와 다중 화면 응용 프로그램을 만드는 기본 단계를 설명 사용 하는 방법을 포함 하 여 D는 `DialogViewController` 및 화면을 만들기 요소 및 섹션을 추가 하는 방법입니다. 또한 산 사용 하는 방법에 알아보았습니다. 와 함께에서 D는 `UINavigationController`합니다.


## <a name="related-links"></a>관련 링크

- [MTDWalkthrough (샘플)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [동영상 가이드-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [동영상 가이드-MonoTouch.Dialog와 iOS 사용자 인터페이스를 쉽게 만들](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
