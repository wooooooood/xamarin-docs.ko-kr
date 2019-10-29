---
title: IOS에서 JSON을 사용 하 여 사용자 인터페이스 만들기
description: Monotouch.dialog (MT. D)에는 JSON 데이터를 통한 동적 UI 생성 지원이 포함 됩니다. 이 자습서에서는 JSONElement를 사용 하 여 응용 프로그램에 포함 되거나 원격 Url에서 로드 된 JSON에서 사용자 인터페이스를 만드는 방법을 안내 합니다.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: ad2386d912dba28041c02c4fb4a8046d341a85ed
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002260"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>IOS에서 JSON을 사용 하 여 사용자 인터페이스 만들기

_Monotouch.dialog (MT. D)에는 JSON 데이터를 통한 동적 UI 생성 지원이 포함 됩니다. 이 자습서에서는 JSONElement를 사용 하 여 응용 프로그램에 포함 되거나 원격 Url에서 로드 된 JSON에서 사용자 인터페이스를 만드는 방법을 안내 합니다._

휴먼. D에서는 JSON에 선언 된 사용자 인터페이스를 만들 수 있습니다. 요소가 JSON, MT를 사용 하 여 선언 된 경우 D는 자동으로 연결 된 요소를 만듭니다. JSON은 로컬 파일, 구문 분석 된 `JsonObject` 인스턴스 또는 원격 Url에서 로드할 수 있습니다.

휴먼. D는 JSON을 사용할 때 Elements API에서 사용할 수 있는 모든 범위의 기능을 지원 합니다. 예를 들어 다음 스크린샷에서 응용 프로그램은 JSON을 사용 하 여 완전히 선언 됩니다.

[![](json-element-walkthrough-images/01-load-from-file.png "예를 들어이 스크린샷에서 응용 프로그램은 JSON을 사용 하 여 완전히 선언 됩니다.")](json-element-walkthrough-images/01-load-from-file.png#lightbox)[![](json-element-walkthrough-images/01-load-from-file.png "예를 들어이 스크린샷에서 응용 프로그램은 JSON을 사용 하 여 완전히 선언 됩니다.")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

JSON을 사용 하 여 작업 세부 정보 화면을 추가 하는 방법을 보여 주는 [ELEMENTS API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) 자습서의 예제를 다시 살펴보겠습니다.

## <a name="setting-up-mtd"></a>MT를 설정 합니다. 2

휴먼. D는 Xamarin.ios를 사용 하 여 배포 됩니다. 이를 사용 하려면 Visual Studio 2017 또는 Mac용 Visual Studio에서 Xamarin.ios 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **monotouch.dialog** 어셈블리에 대 한 참조를 추가 합니다. 그런 다음 필요에 따라 소스 코드에 `using MonoTouch.Dialog` 문을 추가 합니다.

## <a name="json-walkthrough"></a>JSON 연습

이 연습에 대 한 예제에서는 작업을 만들 수 있습니다. 첫 번째 화면에서 작업을 선택 하면 세부 정보 화면이 다음과 같이 표시 됩니다.

 [![](json-element-walkthrough-images/03-task-list.png "When a task is selected on the first screen, a detail screen is presented as shown")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON 만들기

이 예제에서는 `task.json`이라는 프로젝트의 파일에서 JSON을 로드 합니다. 휴먼. D에서는 JSON이 Elements API를 미러링 하는 구문을 준수 해야 합니다. 코드에서 Elements API를 사용 하는 것과 마찬가지로, JSON을 사용 하는 경우 섹션을 선언 하 고 해당 섹션 내에서 요소를 추가 합니다. JSON에서 섹션과 요소를 선언 하려면 "sections" 및 "elements" 문자열을 각각 키로 사용 합니다. 각 요소에 대해 연결 된 요소 형식은 `type` 키를 사용 하 여 설정 됩니다. 다른 모든 elements 속성은 속성 이름을 키로 사용 하 여 설정 됩니다.

예를 들어 다음 JSON은 작업 세부 정보에 대 한 섹션과 요소를 설명 합니다.

```json
{
    "title": "Task",
    "sections": [
        {
            "elements" : [
                {
                    "id" : "task-description",
                    "type": "entry",
                    "placeholder": "Enter task description"
                },
                {
                    "id" : "task-duedate",
                    "type": "date",
                    "caption": "Due Date",
                    "value": "00:00"
                }
            ]
        }
    ]
}
```

위의 JSON에는 각 요소에 대 한 id가 포함 되어 있습니다. 모든 요소에는 런타임에 참조할 수 있는 id가 포함 될 수 있습니다. 이는 코드에서 JSON을 로드 하는 방법을 보여 주는 순간에 사용 되는 방법을 살펴보겠습니다.

## <a name="loading-the-json-in-code"></a>코드로 JSON 로드

JSON을 정의한 후에는 MT로 로드 해야 합니다. D `JsonElement` 클래스를 사용 합니다. 위에서 만든 JSON을 사용 하 여 파일을 프로젝트에 추가 하 고, 콘텐츠 빌드 작업을 지정 하 고, `JsonElement`를 로드 하는 것은 다음 코드 줄을 호출 하는 것 만큼 간단 하다 고 가정 합니다.

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

작업을 만들 때마다 요청 시이 작업을 추가 하기 때문에 이전 Elements API 예제에서 클릭 한 단추를 다음과 같이 수정할 수 있습니다.

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>런타임에 요소 액세스

JSON 파일에 선언 된 경우 두 요소에 id를 추가 했습니다. Id 속성을 사용 하 여 런타임에 각 요소에 액세스 하 여 코드에서 해당 속성을 수정할 수 있습니다. 예를 들어 다음 코드는 entry 및 date 요소를 참조 하 여 task 개체에서 값을 설정 합니다.

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    taskElement.Caption = task.Name;

    var description = taskElement ["task-description"] as EntryElement;

    if (description != null) {
        description.Caption = task.Name;
        description.Value = task.Description;       
    }

    var duedate = taskElement ["task-duedate"] as DateElement;

    if (duedate != null) {                
        duedate.DateValue = task.DueDate;
    }
    _rootElement [0].Add (taskElement);
};
```

## <a name="loading-json-from-a-url"></a>Url에서 JSON 로드

휴먼. D에서는 단순히 Url을 `JsonElement`생성자에 전달 하 여 외부 Url에서 JSON을 동적으로 로드할 수 있습니다. 휴먼. D는 화면 간을 이동할 때 주문형 JSON에 선언 된 계층 구조를 확장 합니다. 예를 들어 아래에는 로컬 웹 서버의 루트에 있는 것과 같은 JSON 파일이 있다고 가정 합니다.

```json
{
    "type": "root",
    "title": "home",
    "sections": [
        {
            "header": "Nested view!",
            "elements": [
                {
                    "type": "boolean",
                    "caption": "Just a boolean",
                    "id": "first-boolean",
                    "value": false
                },
                {
                    "type": "string",
                    "caption": "Welcome to the nested controller"
                }
            ]
        }
    ]
}
```

다음 코드와 같이 `JsonElement`를 사용 하 여이를 로드할 수 있습니다.

```csharp
_rootElement = new RootElement ("Json Example") {
    new Section ("") {
        new JsonElement ("Load from url", "http://localhost/sample.json")
    }
};
```

런타임에 파일이 MT로 검색 되 고 구문 분석 됩니다. D 아래 스크린샷에 표시 된 것 처럼 사용자가 두 번째 뷰로 이동 합니다.

 [![](json-element-walkthrough-images/04-json-web-example.png "The file will be retrieved and parsed by MT.D when the user navigates to the second view")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 MT를 사용 하 여 인터페이스를 만드는 방법을 살펴보았습니다. JSON에서 D. 응용 프로그램 뿐만 아니라 원격 Url을 사용 하 여 파일에 포함 된 JSON을 로드 하는 방법을 살펴보았습니다. 또한 런타임에 JSON에 설명 된 요소에 액세스 하는 방법도 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MTDJsonDemo (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdjsondemo)
- [Monotouch.dialog 소개. 대화 상자](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github의 Monotouch.dialog 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
