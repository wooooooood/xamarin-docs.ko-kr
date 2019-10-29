---
title: Elements API를 사용 하 여 Xamarin.ios 응용 프로그램 만들기
description: 이 문서는 Monotouch.dialog 소개 대화 상자에 제공 된 정보를 기반으로 합니다. Monotouch.dialog (MT. 대화 상자를 사용 하는 방법을 보여 주는 연습을 제공 합니다. D) 요소 API를 사용 하 여 MT로 응용 프로그램을 빠르게 빌드할 수 있습니다. 2.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 2258b2e8451f896aff59c89478833976ef7086e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002376"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Elements API를 사용 하 여 Xamarin.ios 응용 프로그램 만들기

_이 문서는 Monotouch.dialog 소개 대화 상자에 제공 된 정보를 기반으로 합니다. Monotouch.dialog (MT. 대화 상자를 사용 하는 방법을 보여 주는 연습을 제공 합니다. D) 요소 API를 사용 하 여 MT로 응용 프로그램을 빠르게 빌드할 수 있습니다. 2._

이 연습에서는 MT를 사용 합니다. D Elements API를 통해 작업 목록을 표시 하는 응용 프로그램의 마스터-세부 스타일을 만들 수 있습니다. 사용자가 탐색 모음에서 **+** 단추를 선택 하면 태스크에 대 한 테이블에 새 행이 추가 됩니다. 행을 선택 하면 아래 그림과 같이 작업 설명과 기한을 업데이트할 수 있는 세부 정보 화면으로 이동 합니다.

[![](elements-api-walkthrough-images/01-task-list-app.png "Selecting the row will navigate to the detail screen that allows us to update the task description and the due date")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

## <a name="setting-up-mtd"></a>MT를 설정 합니다. 2

휴먼. D는 Xamarin.ios를 사용 하 여 배포 됩니다. 이를 사용 하려면 Visual Studio 2017 또는 Mac용 Visual Studio에서 Xamarin.ios 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **monotouch.dialog** 어셈블리에 대 한 참조를 추가 합니다. 그런 다음 필요에 따라 소스 코드에 `using MonoTouch.Dialog` 문을 추가 합니다.

## <a name="elements-api-walkthrough"></a>요소 API 연습

[Monotouch.dialog 소개 대화 상자](~/ios/user-interface/monotouch.dialog/index.md) 에서 MT의 여러 부분에 대 한 확실 한 이해를 얻었습니다. 2. Elements API를 사용 하 여 응용 프로그램에 모두 함께 배치 해 보겠습니다.

## <a name="setting-up-the-multi-screen-application"></a>다중 화면 응용 프로그램 설정

화면 만들기 프로세스를 시작 하려면 Monotouch.dialog를 `DialogViewController`만든 다음 `RootElement`를 추가 합니다.

Monotouch.dialog를 사용 하 여 다중 화면 응용 프로그램을 만들려면 다음을 수행 해야 합니다.

1. `UINavigationController.` 만들기
1. `DialogViewController.` 만들기
1. `DialogViewController`을 `UINavigationController.` 루트로 추가 합니다. 
1. `DialogViewController.`에 `RootElement` 추가
1. `Sections` 및 `Elements`를 `RootElement.`에 추가 

### <a name="using-a-uinavigationcontroller"></a>UINavigationController 사용

탐색 스타일의 응용 프로그램을 만들려면 `UINavigationController`를 만든 다음 `AppDelegate`의 `FinishedLaunching` 메서드에서 `RootViewController`로 추가 해야 합니다. Monotouch.dialog를 사용 하 여 `UINavigationController` 작업을 수행 하려면 아래와 같이 `UINavigationController`에 `DialogViewController`를 추가 합니다.

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

위의 코드는 `RootElement`의 인스턴스를 만들고 `DialogViewController`에 전달 합니다. `DialogViewController`는 항상 해당 계층의 맨 위에 `RootElement` 있습니다. 이 예제에서는 탐색 컨트롤러의 탐색 모음에서 제목 역할을 하는 "할 일 목록" 문자열을 사용 하 여 `RootElement`를 만듭니다. 이 시점에서 응용 프로그램을 실행 하면 다음과 같은 화면이 표시 됩니다.

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Running the application will present the screen shown here")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Monotouch.dialog의 `Sections` 및 `Elements`의 계층 구조를 사용 하 여 더 많은 화면을 추가 하는 방법을 알아보겠습니다.

### <a name="creating-the-dialog-screens"></a>대화 상자 화면 만들기

`DialogViewController`는 Monotouch.dialog에서 화면을 추가 하는 데 사용 하는 `UITableViewController` 하위 클래스입니다. Monotouch.dialog `DialogViewController`에 `RootElement`를 추가 하 여 화면을 만듭니다. `RootElement`에는 테이블의 섹션을 나타내는 `Section` 인스턴스가 있을 수 있습니다.
섹션은 요소, 다른 섹션 또는 기타 `RootElements`구성 됩니다. `RootElements`를 중첩 하면 Monotouch.dialog는 다음에 표시 되는 것 처럼 탐색 스타일 응용 프로그램을 자동으로 만듭니다.

### <a name="using-dialogviewcontroller"></a>DialogViewController 사용

`UITableViewController` 하위 클래스가 되는 `DialogViewController`에는 `UITableView` 있는 뷰로 표시 됩니다. 이 예에서는 **+** 단추를 누를 때마다 테이블에 항목을 추가 하려고 합니다. `DialogViewController` `UINavigationController`에 추가 되었으므로 `NavigationItem`의 `RightBarButton` 속성을 사용 하 여 아래와 같이 **+** 단추를 추가할 수 있습니다.

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

이전에 `RootElement`를 만들 때 사용자가 **+** 단추를 탭 할 때 요소를 추가할 수 있도록 단일 `Section` 인스턴스로 전달 했습니다. 단추에 대 한 이벤트 처리기에서 다음 코드를 사용 하 여이 작업을 수행할 수 있습니다.

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

이 코드는 단추를 누를 때마다 새 `Task` 개체를 만듭니다. 다음은 `Task` 클래스의 간단한 구현을 보여 줍니다.

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

작업의 `Name` 속성은 새 작업 마다 증가 하는 `n` 이라는 카운터 변수와 함께 `RootElement`캡션을 만드는 데 사용 됩니다. Monotouch.dialog는 각 `taskElement` 추가 될 때 `TableView`에 추가 되는 행으로 요소를 전환 합니다.

## <a name="presenting-and-managing-dialog-screens"></a>대화 상자 화면 표시 및 관리

Monotouch.dialog를 사용 하 여 각 작업의 세부 정보에 대 한 새 화면을 자동으로 만들고 행을 선택할 때이 화면으로 이동 하도록 `RootElement`를 사용 했습니다.

작업 세부 정보 화면 자체는 두 개의 섹션으로 구성 됩니다. 이러한 각 섹션에는 단일 요소가 포함 됩니다. 첫 번째 요소는 작업의 `Description` 속성에 편집할 수 있는 행을 제공 하기 위해 `EntryElement`에서 생성 됩니다. 요소를 선택 하면 아래와 같이 텍스트 편집용 키보드가 제공 됩니다.

 [![](elements-api-walkthrough-images/03-create-task.png "When the element is selected, a keyboard for text editing is presented as shown")](elements-api-walkthrough-images/03-create-task.png#lightbox)

두 번째 섹션에는 작업의 `DueDate` 속성을 관리 하는 데 사용할 수 있는 `DateElement` 포함 되어 있습니다. 날짜를 선택 하면 다음과 같이 날짜 선택이 자동으로 로드 됩니다.

 [![](elements-api-walkthrough-images/04-date-picker.png "Selecting the date automatically loads a date picker as")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

`EntryElement` 및 `DateElement`의 경우 또는 Monotouch.dialog의 모든 데이터 입력 요소에서 값에 대 한 변경 내용은 자동으로 유지 됩니다. 날짜를 편집한 다음 루트 화면과 다양 한 작업 세부 정보 화면의 값이 유지 되는 다양 한 작업 세부 정보를 탐색 하 여이를 설명할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Monotouch.dialog Elements API를 사용 하는 방법을 보여 주는 연습을 제공 했습니다. MT를 사용 하 여 다중 화면 응용 프로그램을 만드는 기본 단계에 대해 설명 했습니다. D. `DialogViewController` 사용 방법 및 화면을 만드는 데 요소와 섹션을 추가 하는 방법을 포함 합니다. 또한 MT를 사용 하는 방법을 살펴보았습니다. D `UINavigationController`와 함께 사용 됩니다.

## <a name="related-links"></a>관련 링크

- [Mtd 연습 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdwalkthrough)
- [Monotouch.dialog 소개. 대화 상자](~/ios/user-interface/monotouch.dialog/index.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Github의 Monotouch.dialog 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
