---
title: JSON을 사용 하 여 Xamarin.iOS의 사용자 인터페이스를 만들 수
description: MonoTouch.Dialog (산 D) JSON 데이터를 통해 동적 UI 생성에 대 한 지원이 포함 되어 있습니다. 이 자습서에서는 응용 프로그램에 포함 된 되거나 원격 Url에서 로드 되는 JSON에서 사용자 인터페이스를 만들 수는 JSONElement를 사용 하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f9ba2cce1650260aa889e8282c091012ef8bbddc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790655"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>JSON을 사용 하 여 Xamarin.iOS의 사용자 인터페이스를 만들 수

_MonoTouch.Dialog (산 D) JSON 데이터를 통해 동적 UI 생성에 대 한 지원이 포함 되어 있습니다. 이 자습서에서는 응용 프로그램에 포함 된 되거나 원격 Url에서 로드 되는 JSON에서 사용자 인터페이스를 만들 수는 JSONElement를 사용 하는 방법을 살펴봅니다._

산 D는 JSON에 선언 된 사용자 인터페이스 만들기를 지원 합니다. 요소는 JSON, 산을 사용 하 여 선언 된 경우 D 관련된 요소를 자동으로 만듭니다. 구문 분석 된 로컬 파일에서 JSON을 로드할 수 있습니다 `JsonObject` 인스턴스나 원격 Url도 합니다.

산 D 모든 범위의 JSON을 사용 하는 경우 요소 API에서 사용할 수 있는 기능을 지원 합니다. 예를 들어 다음 스크린 샷에 응용 프로그램은 JSON을 사용 하 여을 선언 완전히 됩니다.

[![](json-element-walkthrough-images/01-load-from-file.png "예를 들어 왼쪽 스크린샷에서 응용 프로그램은 완전히 사용 하 여 선언 JSON") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "예를 들어 왼쪽 스크린샷에서 응용 프로그램은 완전히 사용 하 여 선언 JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

예를 다시 확인 하겠습니다는 [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) JSON을 사용 하 여 작업 세부 정보 화면을 추가 하는 방법을 보여 주는 자습서입니다.

## <a name="json-walkthrough"></a>JSON 연습

이 연습에 대 한 예제를 통해 작업을 만들어야 합니다. 첫 번째 화면에서 작업을 선택 하면 세부 정보 화면에 표시 된 것 처럼 표시 됩니다.

 [![](json-element-walkthrough-images/03-task-list.png "세부 정보 화면 표시 된 것 처럼 표시 됩니다는 작업을 첫 번째 화면에서 선택")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON 만들기

이 예에서는 이라는 프로젝트에 있는 파일에서 JSON를 로드 합니다 `task.json`합니다. 산 D 요소 API를 반영 하는 구문에 맞게 JSON이 필요 합니다. 코드에서 요소 API를 사용 하는 것 처럼 섹션 JSON을 사용할 경우 선언 하 고 요소를 추가 했습니다 이러한 섹션 내 합니다. 섹션 및 JSON의 요소를 선언 하려면 사용 됨 문자열 "섹션" 및 "요소" 각각 키입니다. 각 요소에 대해 연결 된 요소 형식을 사용 하 여 설정 되는 `type` 키입니다. 다른 모든 요소 속성은 속성 이름으로 키로 설정 됩니다.

예를 들어 다음 JSON에서는 섹션 및 작업 세부 정보에 대 한 요소를 설명합니다.

```csharp
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

위의 JSON 공지 각 요소에 대 한 id를 포함합니다. 모든 요소는 런타임에 참조 하는 id를 포함할 수 있습니다. 코드에서 JSON을 로드 하는 방법을 알아보겠습니다 경우 잠시에서 어떻게 사용 되는지 살펴보겠습니다.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>코드에 JSON 로드

산에 로드 해야 JSON 정의 되 면 D를 사용 하 여 `JsonElement` 클래스입니다. 이름 sample.json 사용 하 여 프로젝트에 추가 되었으며 내용, 로드의 빌드 작업을 지정 합니다. 위에서 만든 json 파일의 `JsonElement` 작업은 다음 코드 줄 호출을 합니다.

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

이 작업을 만들 때마다 요청 시 추가 하 고, 이후 다음과 같은 API 요소 앞의 예제에서 클릭 한 단추의 수정할 수 있을:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>런타임 시 요소에 액세스

JSON 파일에 선언 했으므로 때 id 두 요소에 추가한 점에 유의 하세요. 코드에서 해당 속성을 수정 하는 런타임 시 각 요소에 액세스 하는 id 속성을 사용할 수 있습니다. 예를 들어 다음 코드는 작업 개체에서 값을 설정 하는 항목 및 날짜 요소를 참조 합니다.

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

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Url에서 JSON 로드

산 D 단순히 Url의 생성자에 전달 하 여 외부 Url에서 JSON을 동적으로 로드도 지원는 `JsonElement`합니다. 산 D 화면 간 탐색 하는 동안 필요에 따라 JSON에 선언 된 계층 구조를 확장 합니다. 예를 들어 아래와 같은 JSON 파일의 로컬 웹 서버 루트에 있는:

```csharp
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

로드할 수 있습니다이 사용 하 여 `JsonElement` 다음 코드 에서처럼:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

런타임 시 파일이 검색 하 고 산에서 구문 분석 아래 스크린샷에 표시 된 것 처럼 사용자가 두 번째 보기를 탐색할 때 D:

 [![](json-element-walkthrough-images/04-json-web-example.png "파일 검색 하 고 산에서 구문 분석 사용자가 두 번째 보기를 탐색할 때 D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>요약

이 문서를 사용 하 여 만드는 방법에 살펴보았습니다 산 인터페이스 JSON에서 D입니다. 원격 Url에서 뿐만 아니라 응용 프로그램과 파일에 포함 된 JSON을 로드 하는 방법에 알아보았습니다. 또한 런타임 시 JSON에 설명 된 요소에 액세스 하는 방법을 배웠습니다.


## <a name="related-links"></a>관련 링크

- [MTDJsonDemo (샘플)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [동영상 가이드-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [동영상 가이드-MonoTouch.Dialog와 iOS 사용자 인터페이스를 쉽게 만들](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
