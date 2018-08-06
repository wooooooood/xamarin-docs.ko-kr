---
title: JSON을 사용 하 여 Xamarin.iOS에서 사용자 인터페이스를 만들려면
description: MonoTouch.Dialog (콜로라도 D) JSON 데이터를 통해 동적 UI 생성에 대 한 지원이 포함 되어 있습니다. 이 자습서에서는 응용 프로그램에 포함 되거나 원격 Url에서 로드 되는 JSON에서 사용자 인터페이스를 만들 수는 JSONElement를 사용 하는 방법을 살펴봅니다.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 94cef78bb7eedc03192071f17af765ebb702e260
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038497"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>JSON을 사용 하 여 Xamarin.iOS에서 사용자 인터페이스를 만들려면

_MonoTouch.Dialog (콜로라도 D) JSON 데이터를 통해 동적 UI 생성에 대 한 지원이 포함 되어 있습니다. 이 자습서에서는 응용 프로그램에 포함 되거나 원격 Url에서 로드 되는 JSON에서 사용자 인터페이스를 만들 수는 JSONElement를 사용 하는 방법을 살펴봅니다._

산 D는 JSON에 선언 된 사용자 인터페이스 만들기를 지원 합니다. 요소는 JSON, 산을 사용 하 여 선언 된 경우 차원에서 관련된 요소를 자동으로 만듭니다. 로컬 파일을 구문 분석 된에서 JSON을 로드할 수 있습니다 `JsonObject` 인스턴스 또는 원격 Url입니다.

산 D 전체 다양 한 JSON을 사용 하는 경우 요소 API에서 사용할 수 있는 기능을 지원 합니다. 예를 들어 다음 스크린샷은 응용 프로그램은 JSON을 사용 하 여을 선언 완전히 됩니다.

[![](json-element-walkthrough-images/01-load-from-file.png "예를 들어이 스크린샷에서 응용 프로그램은 완전히 사용 하 여 선언 JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [![](json-element-walkthrough-images/01-load-from-file.png "예를 들어이 스크린샷에서 응용 프로그램은 완전히 사용 하 여 선언 JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

예를 다시 합니다 [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) JSON을 사용 하 여 작업 세부 정보 화면을 추가 하는 방법을 보여 주는 자습서입니다.

## <a name="setting-up-mtd"></a>산 설정 D

산 D는 Xamarin.iOS를 사용 하 여 배포 됩니다. 를 사용 하려면 마우스 오른쪽 단추로 클릭 합니다 **참조** Xamarin.iOS 노드의 Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트 및 참조를 추가 합니다 **MonoTouch.Dialog 1** 어셈블리. 그런 다음 추가 `using MonoTouch.Dialog` 필요에 따라 원본에서 문을 코딩 합니다.

## <a name="json-walkthrough"></a>JSON 연습

이 연습에 대 한 예제는 작업을을 만들 수 있습니다. 첫 번째 화면에서 작업을 선택 하면 세부 정보 화면에 표시 된 것 처럼 표시 됩니다.

 [![](json-element-walkthrough-images/03-task-list.png "세부 정보 화면을 표시 된 것 처럼 표시 됩니다 첫 번째 화면에서 작업을 선택한 경우")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>JSON 만들기

예를 들어 프로젝트 파일에서 JSON 로드에서는 `task.json`합니다. 산 D는 JSON 요소 API를 반영 하는 구문에 맞게 필요 합니다. 코드에서 요소 API를 사용 하는 것 처럼 섹션 JSON을 사용할 경우 선언 하 고 해당 섹션 내에서 요소를 추가 했습니다. 섹션 및 json에서 요소를 선언 하려면 사용 문자열 "섹션" 및 "요소"를 각각의 키와 합니다. 각 요소에 대해 연결 된 요소 형식을 사용 하 여 설정 되는 `type` 키입니다. 다른 모든 요소 속성은 속성 이름을 사용 하 여 키로 설정 됩니다.

예를 들어 다음 JSON 섹션 및 작업 세부 정보에 대 한 요소를 설명합니다.

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

위의 JSON 알림 각 요소에 대 한 id를 포함합니다. 모든 요소에는 런타임 시 참조 id를 포함할 수 있습니다. 코드에서 JSON을 로드 하는 방법을 알아보겠습니다 경우 잠시에서 어떻게 사용 되는지 살펴보겠습니다.

## <a name="loading-the-json-in-code"></a>코드에서 JSON 로드

산으로 로드 하려고 하는 JSON을 정의한 후에 D를 사용 하 여 `JsonElement` 클래스입니다. 위에서 만든 JSON 파일을 가정 하 고 이름 sample.json 사용 하 여 프로젝트에 추가 되어 콘텐츠를 로드 빌드 작업을 지정 합니다 `JsonElement` 간단 하 게 코드의 다음 줄을 호출 합니다.

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

추가 하므로이 요청 될 때마다 작업을 만들 시, 이전 요소 API 예제에서 다음과 같이 클릭 한 단추의 수정할 수 있습니다.

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>런타임 시 요소에 액세스

JSON 파일에 선언 했습니다 하는 경우 두 요소에 id를 추가 했습니다을 기억 하십시오. 코드에서 해당 속성을 수정 하려면 런타임에 각 요소에 액세스 하는 id 속성을 사용할 수 있습니다. 예를 들어, 다음 코드는 작업 개체에서 값을 설정 하는 항목 및 날짜 요소를 참조 합니다.

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

산 D는 단순히 Url의 생성자에 전달 하 여 외부 Url에서 JSON을 동적으로 로드도 지원 합니다 `JsonElement`합니다. 산 D에서는 화면 간 탐색 하는 동안 요청 시 JSON에 선언 된 계층을 확장 합니다. 예를 들어 아래와 같은 JSON 파일을 로컬 웹 서버의 루트에 있는:

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

런타임 시 파일을 검색 하 고 산에서 구문 분석 아래 스크린샷에 표시 된 것 처럼 사용자가 두 번째 뷰를 이동할 때 D:

 [![](json-element-walkthrough-images/04-json-web-example.png "파일을 검색 하 고 산에서 구문 분석 사용자가 두 번째 보기를 탐색할 때 D")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>요약

이 문서에서는 사용 하 여를 만드는 방법을 보여 주었습니다. 산을 사용 하 여 인터페이스 JSON에서 D입니다. 원격 Url 뿐 아니라 응용 프로그램을 사용 하 여 파일에 포함 된 JSON을 로드 하는 방법에 알아보았습니다. 또한 런타임에 JSON에 설명 된 요소에 액세스 하는 방법에 알아보았습니다.

## <a name="related-links"></a>관련 링크

- [MTDJsonDemo (샘플)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [스크린 캐스트-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여는 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [스크린 캐스트-MonoTouch.Dialog를 사용 하 여 iOS 사용자 인터페이스를 쉽게 만들기](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch.Dialog 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [리플렉션 API 연습](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
