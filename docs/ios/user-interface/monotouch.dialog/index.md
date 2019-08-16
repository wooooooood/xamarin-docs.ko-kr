---
title: Xamarin.ios에 대 한 Monotouch.dialog 소개
description: 이 문서에서는 Monotouch.dialog (MT. D). Xamarin.ios를 사용 하 여 신속 하 고 선언적 UI를 개발 하기 위한 프레임 워크입니다. Monotouch.dialog Api를 사용 하 여 코드 또는 JSON에서 인터페이스를 만들고, 끌어오기-새로 고침, 검색, 배경 이미지 로드 등과 같은 기능을 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: cc7d966e7dd0371e967775d112789af9f0ad7c59
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528519"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Xamarin.ios에 대 한 Monotouch.dialog 소개

Monotouch.dialog. 대화 상자를 MT 라고 합니다. D의 경우는 개발자가 뷰 컨트롤러, 테이블 등의 번거로움을 아니라 정보를 사용 하 여 응용 프로그램 화면과 탐색을 빌드할 수 있게 해 주는 신속한 UI 개발 도구 키트입니다. 따라서 UI 개발 및 코드 감소를 매우 단순화 합니다. 예를 들어 다음 스크린샷에서를 살펴보세요.

 [![](images/image1.png "예를 들어이 스크린샷")](images/image1.png#lightbox)

다음 코드는이 전체 화면을 정의 하는 데 사용 되었습니다.

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

IOS에서 테이블을 사용 하는 경우 종종 반복적인 코드가 있습니다.
예를 들어 테이블이 필요할 때마다 해당 테이블을 채워야 하는 데이터 원본이 필요 합니다. 탐색 컨트롤러를 통해 연결 된 두 개의 테이블 기반 화면이 있는 응용 프로그램에서 각 화면은 동일한 코드를 많이 공유 합니다.

휴먼. D는 테이블을 만들기 위해 모든 코드를 일반 API에 캡슐화 하 여이를 단순화 합니다. 그런 다음 해당 API 위에 추상화를 제공 하 여 선언적 개체 바인딩 구문을 사용 하 여 훨씬 더 쉽게 만들 수 있도록 합니다. 따라서 MT에서 사용할 수 있는 두 가지 Api가 있습니다. 2

- **하위 수준 요소 api** – *elements api* 는 화면과 해당 구성 요소를 나타내는 요소의 계층적 트리를 만드는 방법을 기반으로 합니다. 요소 API를 사용 하면 개발자는 Ui를 만들 때 가장 유연 하 고 유연 하 게 제어할 수 있습니다. 또한 Elements API에는 JSON을 통한 선언적 정의에 대 한 고급 지원이 포함 되어 있으므로 서버에서 동적 UI를 생성 하는 것 뿐만 아니라 매우 빠른 선언을 모두 사용할 수 있습니다. 
- **높은 수준의 리플렉션 API** – 라고도 합니다 *바인딩*  *API* , UI 힌트와 다음 산 주석이 지정 된 클래스에서 D 화면 개체를 기반으로 자동으로 만들고 간에 새로운 바인딩을 표시 (및 필요에 따라 편집) 화면에서 및 백업 원본 개체를 제공 합니다. 위의 예제에서는 리플렉션 API를 사용 하는 방법을 보여 줍니다. 이 API는 요소 API에서 수행 하는 세분화 된 제어를 제공 하지 않지만 클래스 특성을 기반으로 요소 계층 구조를 자동으로 작성 하 여 복잡성을 더욱 줄입니다. 


휴먼. D는 화면 생성을 위해 기본 제공 되는 다양 한 UI 요소 집합을 제공 하지만 사용자 지정 된 요소 및 고급 화면 레이아웃에 대 한 필요성도 인식 합니다. 따라서 확장성은 API에 대 한 최고 수준의 주요 구운입니다. 개발자는 기존 요소를 확장 하거나 새 요소를 만든 다음 원활 하 게 통합할 수 있습니다.

또한 MT. D에는 "끌어오기-새로 고침" 지원, 비동기 이미지 로드, 검색 지원 등의 다양 한 일반 iOS UX 기능이 내장 되어 있습니다.

이 문서는 MT로 작업 하는 방법에 대해 자세히 설명 합니다. D, 다음을 포함 합니다.

- **MT. D 구성 요소** – MT를 구성 하는 클래스를 이해 하는 데 중점을 둡니다. D를 사용 하면 신속 하 게 속도를 높일 수 있습니다. 
- **요소 참조** – MT의 기본 제공 요소에 대 한 포괄적인 목록입니다. 2. 
- **고급 사용** – 끌어오기-새로 고침, 검색, 백그라운드 이미지 로드, LINQ를 사용 하 여 요소 계층 구조 작성, MT에서 사용할 사용자 지정 요소, 셀 및 컨트롤러 만들기와 같은 고급 기능을 다룹니다. 2. 

## <a name="setting-up-mtd"></a>MT를 설정 합니다. 2

휴먼. D는 Xamarin.ios를 사용 하 여 배포 됩니다. 이를 사용 하려면 Visual Studio 2017 또는 Mac용 Visual Studio에서 Xamarin.ios 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **monotouch.dialog** 어셈블리에 대 한 참조를 추가 합니다. 그런 다음 필요 `using MonoTouch.Dialog` 에 따라 소스 코드에서 문을 추가 합니다.

## <a name="understanding-the-pieces-of-mtd"></a>MT의 피스를 이해 합니다. 2

리플렉션 API를 사용 하는 경우에도 MT. D는 요소 API를 통해 직접 생성 된 것 처럼 내부적으로 요소 계층 구조를 만듭니다. 또한 이전 섹션에서 언급 한 JSON 지원도 요소를 만듭니다. 이러한 이유로 MT의 구성 요소를 기본적으로 이해 하는 것이 중요 합니다. 2.

휴먼. D는 다음 네 부분을 사용 하 여 화면을 빌드합니다.

- **DialogViewController**
- **RootElement**
- **섹션**
- **요소**


### <a name="dialogviewcontroller"></a>DialogViewController

*Dialogviewcontroller*또는 short에 대 한 *dvc* 는에서 `UITableViewController` 상속 되므로 테이블이 있는 화면을 나타냅니다. DVCs는 일반 UITableViewController와 마찬가지로 탐색 컨트롤러에 푸시할 수 있습니다.

### <a name="rootelement"></a>RootElement

*Rootelement* 는 dvc로 이동 하는 항목에 대 한 최상위 컨테이너입니다. 여기에는 요소가 포함 될 수 있는 섹션이 포함 되어 있습니다. RootElements는 렌더링 되지 않습니다. 대신 실제로 렌더링 되는 항목에 대 한 컨테이너입니다. RootElement는 DVC에 할당 된 다음 DVC가 해당 자식을 렌더링 합니다.

### <a name="section"></a>섹션

섹션은 테이블의 셀 그룹입니다. 일반 테이블 섹션과 마찬가지로, 다음 스크린샷에 나와 있는 것 처럼 텍스트 또는 사용자 지정 보기를 포함할 수 있는 머리글 및 바닥글을 선택적으로 포함할 수 있습니다.

 [![](images/image2.png "일반 테이블 섹션과 마찬가지로,이 스크린샷에서와 같이 텍스트 또는 사용자 지정 보기를 포함할 수 있는 머리글 및 바닥글을 선택적으로 포함할 수 있습니다.")](images/image2.png#lightbox)

### <a name="element"></a>요소

요소는 테이블의 실제 셀을 나타냅니다. 휴먼. D는 다양 한 데이터 형식 또는 다른 입력을 나타내는 다양 한 요소로 압축 됩니다. 예를 들어 다음 스크린샷에서는 사용 가능한 몇 가지 요소를 보여 줍니다.

 [![](images/image3.png "예를 들어이 스크린샷에서는 사용 가능한 몇 가지 요소를 보여 줍니다.")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>섹션과 RootElements에 대 한 추가 정보

이제 RootElements와 섹션에 대해 더 자세히 설명 하겠습니다.

### <a name="rootelements"></a>RootElements

Monotouch.dialog 프로세스를 시작 하려면 하나 이상의 RootElement가 필요 합니다.

RootElement가 섹션/요소 값을 사용 하 여 초기화 되는 경우이 값은 표시의 오른쪽에 렌더링 되는 구성에 대 한 요약을 제공 하는 자식 요소를 찾는 데 사용 됩니다. 예를 들어 아래 스크린샷에서는 왼쪽에 있는 테이블을 보여 줍니다. 여기에는 오른쪽의 세부 정보 화면 제목, "후 식", 선택한 사막 값이 포함 된 셀이 있습니다.

 [![](images/image4.png "이 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽 후 식, 선택한 desert의 값과 함께 제목이 포함 된 셀이 있는")](images/image4.png#lightbox) [![](images/image5.png "이 아래 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽의 후 식, 선택한 desert의 값과 함께 제목의 포함 된 셀을 사용 하 여")](images/image5.png#lightbox)

위에 표시 된 것 처럼 루트 요소를 섹션 내에서 사용 하 여 새 중첩 구성 페이지 로드를 트리거할 수도 있습니다. 이 모드에서 사용 하는 경우 제공 된 캡션은 섹션 내에서 렌더링 되는 동안 사용 되며 하위 페이지의 제목으로도 사용 됩니다. 예를 들어:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner") {
        new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
            new Section () {
                new RadioElement ("Ice Cream", "dessert"),
                new RadioElement ("Milkshake", "dessert"),
                new RadioElement ("Chocolate Cake", "dessert")
            }
        }
    }
};
```

위의 예제에서 사용자가 "후 식"를 탭 하면 Monotouch.dialog는 새 페이지를 만들어 루트를 "후 식"로 이동 하 고 세 개의 값이 있는 라디오 그룹을 사용 합니다.

이 특정 샘플에서 "2" 값을 RadioGroup에 전달 했으므로 라디오 그룹은 "후 식" 섹션에서 "초콜릿 케이크"를 선택 합니다. 즉, 목록에서 세 번째 항목 (인덱스 0)을 선택 합니다.

Add 메서드를 호출 하거나 C# 4 개의 이니셜라이저 구문을 사용 하 여 섹션을 추가 합니다.
애니메이션을 사용 하 여 섹션을 삽입 하기 위해 Insert 메서드가 제공 됩니다.

RadioGroup 대신 그룹 인스턴스를 사용 하 여 RootElement를 만드는 경우 섹션에 표시 될 때 RootElement의 요약 값은 BooleanElements와 동일한 키를 가진 모든 및 CheckboxElements의 누적 개수가 됩니다.

### <a name="sections"></a>섹션

섹션은 화면에서 요소를 그룹화 하는 데 사용 되며,이 요소는 RootElement의 유일한 유효한 직계 자식입니다. 섹션에는 새 RootElements를 비롯 한 모든 표준 요소가 포함 될 수 있습니다.

섹션에 포함 된 RootElements는 새로운 더 깊은 수준으로 이동 하는 데 사용 됩니다.

섹션은 머리글과 바닥글을 문자열 또는 UIViews 포함할 수 있습니다.
일반적으로 문자열을 사용 하지만 사용자 지정 Ui를 만들려면 UIView를 머리글 또는 바닥글로 사용할 수 있습니다. 다음과 같이 문자열을 사용 하 여 만들 수 있습니다.

```csharp
var section = new Section ("Header", "Footer");
```

뷰를 사용 하려면 뷰를 생성자에 전달 하면 됩니다.

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header);
```

### <a name="getting-notified"></a>알림 받기

#### <a name="handling-nsaction"></a>NSAction 처리

휴먼. D는 `NSAction` 콜백을 처리 하기 위한 대리자로를 표시 합니다.
예를 들어 MT로 만든 테이블 셀에 대 한 터치 이벤트를 처리 하려는 경우를 가정해 보겠습니다. 2. MT를 사용 하 여 요소를 만드는 경우 D, 다음과 같이 콜백 함수를 제공 하기만 하면 됩니다.

```csharp
new Section () {
    new StringElement ("Demo Callback", delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>요소 값 검색

`Element.Value` 속성과 결합 된 콜백은 다른 요소에 설정 된 값을 검색할 수 있습니다. 예를 들어, 다음 코드를 고려하세요.

```csharp
var element = new EntryElement (task.Name, "Enter task description", task.Description);
                
var taskElement = new RootElement (task.Name) {
    new Section () { element },
    new Section () { new DateElement ("Due Date", task.DueDate) },
    new Section ("Demo Retrieving Element Value") {
        new StringElement ("Output Task Description", delegate { Console.WriteLine (element.Value); })
    }
};
```

이 코드는 아래와 같이 UI를 만듭니다. 이 예제에 대 한 전체 연습은 [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) 자습서를 참조 하세요.

 [![](images/image6.png "다른 요소에 설정 된 값을 가져올 수 있습니다. 값 속성과 함께 사용 됩니다.")](images/image6.png#lightbox)

사용자가 아래쪽 테이블 셀을 누르면 익명 함수의 코드가 실행 되어 `element` 인스턴스의 값이 Mac용 Visual Studio의 **응용 프로그램 출력** 패드에 기록 됩니다.

## <a name="built-in-elements"></a>기본 제공 요소

휴먼. D에는 요소 라고 하는 여러 기본 제공 테이블 셀 항목이 함께 제공 됩니다.
이러한 요소는 문자열, 부동 소수점, 날짜 및 심지어 이미지와 같은 테이블 셀에 다양 한 형식을 표시 하는 데 사용 됩니다. 각 요소는 데이터 형식을 적절 하 게 표시 합니다. 예를 들어 부울 요소는 해당 값을 설정/해제 하는 스위치를 표시 합니다. 마찬가지로 float 요소는 float 값을 변경 하는 슬라이더를 표시 합니다.

이미지 및 html과 같은 더 다양 한 데이터 형식을 지원 하기 위해 더욱 복잡 한 요소가 있습니다. 예를 들어, html 요소를 선택 하면 웹 페이지를 로드 하기 위해 UIWebView 페이지를 열 때 표 셀에 캡션이 표시 됩니다.

### <a name="working-with-element-values"></a>요소 값 작업

사용자 입력을 캡처하는 데 사용 되는 요소는 `Value` 언제 든 지 요소의 현재 값을 보유 하는 public 속성을 노출 합니다. 사용자가 응용 프로그램을 사용 하면 자동으로 업데이트 됩니다.

이는 Monotouch.dialog의 일부인 모든 요소에 대 한 동작 이지만 사용자가 만든 요소에는 필요 하지 않습니다.

### <a name="string-element"></a>String 요소

는 `StringElement` 표 셀의 왼쪽에 캡션을 표시 하 고 셀의 오른쪽에 문자열 값을 표시 합니다.

 [![](images/image7.png "StringElement는 표 셀의 왼쪽에 캡션을 표시 하 고 셀의 오른쪽에 있는 문자열 값을 표시 합니다.")](images/image7.png#lightbox)

를 `StringElement` 단추로 사용 하려면 대리자를 제공 합니다.

```csharp
new StringElement ("Click me", () => { 
    new UIAlertView("Tapped", "String Element Tapped", null, "ok", null).Show();
});
```

 [![](images/image8.png "StringElement를 단추로 사용 하려면 대리자를 제공 합니다.")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>스타일이 지정 된 문자열 요소

에서는 `StyledStringElement` 기본 제공 테이블 셀 스타일 또는 사용자 지정 서식 지정을 사용 하 여 문자열을 표시할 수 있습니다.

 [![](images/image9.png "StyledStringElement를 사용 하면 기본 제공 테이블 셀 스타일이 나 사용자 지정 서식 지정을 사용 하 여 문자열을 표시할 수 있습니다.")](images/image9.png#lightbox)

클래스 `StyledStringElement` 는에서 `StringElement`파생 되지만 개발자가 글꼴, 텍스트 색, 배경 셀 색, 줄 바꿈 모드, 표시할 줄 수 및 액세서리를 표시할지 여부와 같은 몇 가지 속성을 사용자 지정할 수 있습니다.

### <a name="multiline-element"></a>Multiline 요소

 [![](images/image10.png "Multiline 요소")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry 요소

이름 `EntryElement`에서 알 수 있듯이은 사용자 입력을 가져오는 데 사용 됩니다. 문자를 숨기는 일반 문자열 또는 암호를 지원 합니다.

 [![](images/image11.png "EntryElement는 사용자 입력을 가져오는 데 사용 됩니다.")](images/image11.png#lightbox)

다음 세 가지 값으로 초기화 됩니다.

- 사용자에 게 표시 되는 항목에 대 한 캡션입니다.
- 자리 표시자 텍스트 (사용자에 게 힌트를 제공 하는 회색으로 표시 된 텍스트)입니다. 
- 텍스트의 값입니다.


자리 표시자와 값은 null 일 수 있습니다. 그러나 캡션이 필요 합니다.

어떤 지점에서 든 해당 Value 속성에 액세스 하면의 `EntryElement`값을 검색할 수 있습니다.

또한 만든 시간에 속성을데이터입력에필요한키보드유형스타일로설정할수있습니다.`KeyboardType` 아래에 나열 된 값 `UIKeyboardType` 을 사용 하 여 키보드를 구성 하는 데 사용할 수 있습니다.

- 숫자
- 전화
- URL
- EMail


### <a name="boolean-element"></a>부울 요소

 [![](images/image12.png "부울 요소")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Checkbox 요소

 [![](images/image13.png "Checkbox 요소")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radio 요소

를 `RadioElement` 사용`RootElement`하려면에를 지정 해야 합니다. `RadioGroup`

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0));
```

 [![](images/image14.png "RadioElement를 사용 하려면 RootElement에 RadioGroup을 지정 해야 합니다.")](images/image14.png#lightbox)

 `RootElements`는 라디오 요소를 조정 하는 데도 사용 됩니다. 멤버 `RadioElement` 는 여러 섹션에 걸쳐 있을 수 있습니다. 예를 들어 링 톤 선택기와 유사한 항목을 구현 하 고 시스템 벨 소리의 개별 사용자 지정 링 톤을 구현할 수 있습니다. 요약 보기에는 현재 선택 된 radio 요소가 표시 됩니다. 이를 사용 하려면 다음과 같이 `RootElement` group 생성자를 사용 하 여를 만듭니다.

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0));
```

의 `RadioGroup` 그룹 이름은 포함 하는 페이지 (있는 경우)에서 선택한 값을 표시 하는 데 사용 되 고,이 경우 0 인 값은 첫 번째 선택 된 항목의 인덱스입니다.

### <a name="badge-element"></a>배지 요소

 [![](images/image15.png "배지 요소")](images/image15.png#lightbox)

### <a name="float-element"></a>Float 요소

 [![](images/image16.png "Float 요소")](images/image16.png#lightbox)

### <a name="activity-element"></a>Activity 요소

 [![](images/image17.png "Activity 요소")](images/image17.png#lightbox)

### <a name="date-element"></a>Date 요소

 ![](images/image18.png "Date 요소")

DateElement에 해당 하는 셀을 선택 하면 날짜 선택이 다음과 같이 표시 됩니다.

 [![](images/image19.png "DateElement에 해당 하는 셀을 선택 하면 다음과 같이 날짜 선택이 표시 됩니다.")](images/image19.png#lightbox)

### <a name="time-element"></a>Time 요소

 [![](images/image20.png "Time 요소")](images/image20.png#lightbox)

TimeElement에 해당 하는 셀을 선택 하면 다음과 같이 시간 선택이 표시 됩니다.

 [![](images/image21.png "TimeElement에 해당 하는 셀을 선택 하면 다음과 같이 시간 선택이 표시 됩니다.")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime 요소

 [![](images/image22.png "DateTime 요소")](images/image22.png#lightbox)

DateTimeElement에 해당 하는 셀을 선택 하면 다음과 같이 datetime 선택이 표시 됩니다.

 [![](images/image23.png "DateTimeElement에 해당 하는 셀을 선택 하면 datetime 선택이 다음과 같이 표시 됩니다.")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML 요소

 [![](images/image24.png "HTML 요소")](images/image24.png#lightbox)

는 `HTMLElement` 테이블 셀에 `Caption` 속성의 값을 표시 합니다. 선택 선택 된 경우 `Url` 요소에 할당 된는 아래와 같이 `UIWebView` 컨트롤에 로드 됩니다.

 [![](images/image25.png "선택 선택 된 경우 요소에 할당 된 Url은 아래와 같이 UIWebView 보기 컨트롤에 로드 됩니다.")](images/image25.png#lightbox)

### <a name="message-element"></a>Message 요소

 [![](images/image26.png "Message 요소")](images/image26.png#lightbox)

### <a name="load-more-element"></a>추가 요소 로드

사용자가 목록에서 더 많은 항목을 로드할 수 있도록 하려면이 요소를 사용 합니다. 글꼴 및 텍스트 색 뿐만 아니라 표준 및 로딩 캡션을 사용자 지정할 수 있습니다.
표시기가 애니메이션 효과를 시작 하 고 사용자가 셀을 탭 한 다음 생성자에 전달 된 `NSAction` 가 실행 될 때 로드 캡션이 표시 됩니다. `UIActivity` 의 코드가 `NSAction` 완료 `UIActivity` 되 면 표시기가 애니메이션 효과를 중지 하 고 일반 캡션이 다시 표시 됩니다.

### <a name="uiview-element"></a>UIView 요소

또한를 `UIView` `UIViewElement`사용 하 여 모든 사용자 지정을 표시할 수 있습니다.

### <a name="owner-drawn-element"></a>소유자가 그린 요소

이 요소는 추상 클래스 이므로 서브클래싱 되어야 합니다. `Height(RectangleF bounds)` 요소의`Draw(RectangleF bounds, CGContext context, UIView view)` 높이를 반환 해야 하는 메서드와 컨텍스트 및 뷰 매개 변수를 사용 하 여 지정 된 범위 내에서 사용자 지정 된 모든 그리기를 수행 해야 하는 메서드를 재정의 해야 합니다. 이 요소는를 `UIView`서브클래싱 하 고 반환 될 셀에 배치 하 여 두 개의 간단한 재정의를 구현 하기만 하면 됩니다. `DemoOwnerDrawnElement.cs` 파일의 샘플 앱에서 더 나은 샘플 구현을 볼 수 있습니다.

클래스를 구현 하는 매우 간단한 예제는 다음과 같습니다.

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
{
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text { get; set; }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
}
```

### <a name="json-element"></a>JSON 요소

는 로컬 또는 원격 url `RootElement` 에서 중첩 된 `RootElement` 자식의 콘텐츠를 로드할 수 있도록를 확장 하는의 서브 클래스입니다. `JsonElement`

는 두 가지 형식으로 인스턴스화할 수 있는입니다.`RootElement` `JsonElement` 한 버전은 요청 `RootElement` 시 콘텐츠를 로드 하는을 만듭니다. 이러한 매개 변수는 끝 부분 `JsonElement` 에서 추가 인수를 사용 하는 생성자를 사용 하 여 만들고, 콘텐츠를 로드 하는 데 사용 되는 url입니다.

```csharp
var je = new JsonElement ("Dynamic Data", "https://tirania.org/tmp/demo.json");
```

다른 양식은 이미 구문 분석 한 로컬 파일 또는 기존 `System.Json.JsonObject` 에서 데이터를 만듭니다.

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

With MT에 JSON을 사용 하는 방법에 대 한 자세한 내용입니다. D. [JSON 요소 연습](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) 자습서를 참조 하세요.

## <a name="other-features"></a>기타 기능

### <a name="pull-to-refresh-support"></a>끌어오기-새로 고침 지원

 *풀 대* *새로 고침은* *Tweetie2* 앱에서 처음 발견 되는 시각적 효과 이며, 많은 응용 프로그램에서 널리 사용 되 고 있습니다.

대화 상자에 자동으로 끌어오기-새로 고침 지원을 추가 하려면 다음 두 가지 작업을 수행 해야 합니다. 즉, 사용자가 데이터를 끌어올 때 알림을 받도록 이벤트 처리기를 연결 하 고 데이터 `DialogViewController` 를 로드 하 여 기본 상태로 되돌릴 때에 게 알립니다.

알림을 후크 하는 작업은 간단 합니다. 다음과 같이의 `RefreshRequested` 이벤트 `DialogViewController`에 연결 하기만 하면 됩니다.

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

그런 다음 메서드에서 `OnUserRequestedRefresh`일부 데이터 로드를 큐에 대기 시키고, net에서 일부 데이터를 요청 하거나, 데이터를 계산 하기 위해 스레드를 회전 합니다. 데이터가 로드 되 면에 새 데이터가 `DialogViewController` 있음을 알리고 다음을 호출 `ReloadComplete`하 여 뷰를 기본 상태로 복원 해야 합니다.

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>검색 지원

검색을 지원 하려면 `EnableSearch` `DialogViewController`에서 속성을 설정 합니다. 검색 표시줄의 워터 마크 `SearchPlaceholder` 텍스트로 사용 하도록 속성을 설정할 수도 있습니다.

검색 하면 보기의 내용이 사용자 형식으로 변경 됩니다. 표시 되는 필드를 검색 하 고 사용자에 게 표시 합니다. 는 `DialogViewController` 결과에 대 한 새 필터 작업을 프로그래밍 방식으로 시작, 종료 또는 트리거하는 세 가지 메서드를 노출 합니다. 이러한 메서드는 다음과 같습니다.

- `StartSearch`
- `FinishSearch`
- `PerformFilter`


시스템이 확장 가능 하므로 원하는 경우이 동작을 변경할 수 있습니다.

### <a name="background-image-loading"></a>배경 이미지 로드

Monotouch.dialog는 [TweetStation](https://github.com/migueldeicaza/TweetStation) 응용 프로그램의 이미지 로더를 통합 합니다. 이 이미지 로더는 배경에서 이미지를 로드 하는 데 사용할 수 있으며, 캐싱을 지원 하 고 이미지가 로드 될 때 코드를 알릴 수 있습니다.

또한 나가는 네트워크 연결 수를 제한 합니다.

이미지 로더는 `ImageLoader` 클래스에서 구현 되며, `DefaultRequestImage` 메서드를 호출 하기만 하면 로드 하려는 이미지에 대 한 Uri를 제공 하 고 이미지 ha를 수행할 때 호출 되는 `IImageUpdated` 인터페이스의 인스턴스를 제공 해야 합니다. s가 로드 되었습니다.

예를 들어 다음 코드는 Url에서로 `BadgeElement`이미지를 로드 합니다.

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
    new Section() {
        new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
    }
};
```

ImageLoader 클래스는 현재 메모리에 캐시 된 모든 이미지를 해제 하려는 경우 호출할 수 있는 제거 메서드를 노출 합니다. 현재 코드에 50 이미지용 캐시가 있습니다. 다른 캐시 크기를 사용 하려는 경우 (예를 들어 이미지가 너무 커서 50 이미지가 너무 많은 경우) ImageLoader의 인스턴스를 만들고 캐시에 보관 하려는 이미지의 수를 전달할 수 있습니다.

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ를 사용 하 여 요소 계층 구조 만들기

Linq를 사용 하 여 LINQ 및 C#의 초기화 구문을 보다 유용 하 게 사용 하 여 요소 계층 구조를 만들 수 있습니다. 예를 들어 다음 코드는 일부 문자열 배열에서 화면을 만들고 각각 `StringElement`에 전달 되는 익명 함수를 통해 셀 선택을 처리 합니다.

```csharp
var rootElement = new RootElement ("LINQ root element") {
    from x in new string [] { "one", "two", "three" }
    select new Section (x) {
        from y in "Hello:World".Split (':')
        select (Element) new StringElement (y, delegate { Debug.WriteLine("cell tapped"); })
    }
};
```

이를 쉽게 XML 데이터 저장소 또는 데이터베이스의 데이터와 결합 하 여 복잡 한 응용 프로그램을 완전히 데이터 로부터 만들 수 있습니다.

## <a name="extending-mtd"></a>MT 확장. 2

### <a name="creating-custom-elements"></a>사용자 지정 요소 만들기

기존 요소 또는 루트 클래스 요소에서 파생 하 여를 상속 하 여 고유한 요소를 만들 수 있습니다.

고유한 요소를 만들려면 다음 메서드를 재정의 해야 합니다.

```csharp
// To release any heavy resources that you might have
void Dispose (bool disposing);

// To retrieve the UITableViewCell for your element
// you would need to prepare the cell to be reused, in the
// same way that UITableView expects reusable cells to work
UITableViewCell GetCell (UITableView tv);

// To retrieve a "summary" that can be used with
// a root element to render a summary one level up.  
string Summary ();

// To detect when the user has tapped on the cell
void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path);

// If you support search, to probe if the cell matches the user input
bool Matches (string text);
```

요소에 가변 크기를 사용할 수 있는 경우 하나의 메서드를 포함 하 `IElementSizing` 는 인터페이스를 구현 해야 합니다.

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

반환 된 셀을 호출 `GetCell` `base.GetCell(tv)` 하 고 사용자 지정 하 여 메서드를 구현 하려는 경우에는 다음과 같이 요소에 `CellKey` 고유한 키를 반환 하도록 속성을 재정의 해야 합니다.

```csharp
static NSString MyKey = new NSString ("MyKey");
protected override NSString CellKey {
    get {
        return MyKey;
    }
}
```

이는 대부분의 요소에 대해 작동 하지만 및 `StringElement` `StyledStringElement` 의 경우에는 작동 하지 않으며 다양 한 렌더링 시나리오에 고유한 키 집합을 사용 합니다. 이러한 클래스에서 코드를 복제 해야 합니다.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

Reflection과 Elements API는 모두 동일한 `DialogViewController`를 사용 합니다. 보기의 모양을 사용자 지정 하려는 경우 또는 ui의 기본 생성을 벗어나는의 `UITableViewController` 일부 기능을 사용 하려는 경우가 있습니다.

는 단순히의 서브 클래스 `UITableViewController` 이며,를사용자지정하는것과같은방법으로사용자지정할수있습니다.`UITableViewController` `DialogViewController`

예를 들어 목록 스타일을 `Grouped` 또는 `Plain`로 변경 하려는 경우 다음과 같이 컨트롤러를 만들 때 속성을 변경 하 여이 값을 설정할 수 있습니다.

```csharp
var myController = new DialogViewController (root, true) {
    Style = UITableViewStyle.Grouped;
}
```

배경을 설정 하는 등의 `DialogViewController`고급 사용자 지정 작업을 수행 하려면 아래 예제에 표시 된 것 처럼 해당 메서드를 하위 클래스로 지정 하 고 적절 한 메서드를 재정의 합니다.

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

또 다른 사용자 지정 지점은의 다음 가상 메서드입니다 `DialogViewController`.

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

이 메서드는 셀이 균일 하 `DialogViewController.Source` 게 크기가 지정 된 경우의 서브 클래스를 반환 하거나 셀이 `DialogViewController.SizingSource` 불균형 하지 않은 경우의 서브 클래스를 반환 해야 합니다.

이 재정의를 사용 하 여 `UITableViewSource` 메서드를 캡처할 수 있습니다. 예를 들어 [TweetStation](https://github.com/migueldeicaza/TweetStation) 는이를 사용 하 여 사용자가 위쪽으로 스크롤 한 시기를 추적 하 고 읽지 않은 트 윗의 수를 적절 하 게 업데이트 합니다.

## <a name="validation"></a>유효성 검사

웹 페이지 및 데스크톱 응용 프로그램에 적합 한 모델이 iPhone 상호 작용 모델에 직접 매핑되지 않으므로 요소는 유효성 검사를 자체적으로 제공 하지 않습니다.

데이터 유효성 검사를 수행 하려면 사용자가 입력 한 데이터를 사용 하 여 작업을 트리거할 때이 작업을 수행 해야 합니다. 예를 들어 맨 위 도구 모음에 있는 **완료** 또는 **다음** 단추 또는 `StringElement` 다음 단계로 이동 하는 단추로 사용할 수 있습니다.

여기서는 기본 입력 유효성 검사를 수행 하 고 서버와의 사용자/암호 조합 유효성 검사와 같은 보다 복잡 한 유효성 검사를 수행 합니다.

사용자에 게 오류를 알리는 방법은 응용 프로그램 마다 다릅니다. 를 `UIAlertView` 팝 하거나 힌트를 표시할 수 있습니다.

## <a name="summary"></a>요약

이 문서에서는 Monotouch.dialog에 대 한 많은 정보를 설명 했습니다. 여기서는 MT의 기본 사항에 대해 설명 했습니다. D는 MT를 구성 하는 다양 한 구성 요소에 적용 됩니다. 2. 또한 MT에서 지 원하는 다양 한 요소 및 테이블 사용자 지정을 보여 줍니다. D. MT에 대해 설명 했습니다. D는 사용자 지정 요소로 확장할 수 있습니다. 또한 MT에서 JSON 지원에 대해 설명 했습니다. D를 사용 하 여 JSON에서 동적으로 요소를 만들 수 있습니다.


## <a name="related-links"></a>관련 링크

- [연습: 요소 API를 사용하여 애플리케이션 만들기](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [연습: 리플렉션 API를 사용하여 애플리케이션 만들기](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [연습: JSON 요소를 사용 하 여 사용자 인터페이스 만들기](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Monotouch.dialog JSON 태그](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github의 Monotouch.dialog 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
