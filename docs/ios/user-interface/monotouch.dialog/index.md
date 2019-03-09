---
title: MonoTouch.Dialog Xamarin.iOS에 대 한 소개
description: '이 문서에서는 MonoTouch.Dialog (콜로라도 설명 D), Xamarin.iOS 사용 하 여 신속 하 게 하 고 선언적인 UI 개발을 위한 프레임 워크입니다. MonoTouch.Dialog Api를 사용 하 여 코드 또는 JSON에서 인터페이스를 만들고 끌어오기-새로 고침, 검색, 배경 이미지 로드 등과 같은 기능을 사용 하는 방법을 설명 합니다.'
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
---

# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>MonoTouch.Dialog Xamarin.iOS에 대 한 소개

MonoTouch.Dialog, MT와 참조 줄여서 D는 개발자가 응용 프로그램 화면 및 탐색 하는 번거로움을 뷰 컨트롤러, 테이블 등을 만드는 것이 아니라 정보를 사용 하 여 구축을 신속 하 게 UI 개발 도구 키트입니다. 따라서 UI 개발 및 코드 영역 축소의 중요 한 단순화를 제공합니다. 예를 들어 다음 스크린 샷에서를 것이 좋습니다.

 [![](images/image1.png "예를 들어이 스크린 샷")](images/image1.png#lightbox)

다음 코드는이 전체 화면을 정의에 사용 된:

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

IOS에서 테이블을 사용 하는 경우 경우가 있습니다 엄청난 양의 반복적인 코드를 합니다.
예를 들어 테이블 필요할 때마다 데이터 원본의 해당 테이블을 채우는 데 필요 합니다. 탐색 컨트롤러를 통해 연결 된 두 테이블 기반 화면에 있는 응용 프로그램에서 각 화면 동일한 코드를 많이 공유 합니다.

산 차원은 테이블 생성을 위한 제네릭 API로 모든 해당 코드를 캡슐화 하 여 하를 간소화 합니다. 그런 다음 바인딩도 쉽게 구문 선언적 개체를 허용 하는 해당 api 추상화를 제공 합니다. 따라서 두 가지 Api 산에서 사용할 수 있는 D:

-   **하위 수준 요소 API** – *요소 API* 화면 및 해당 구성 요소를 나타내는 요소의 계층적 트리를 만드는 기반으로 합니다. 요소 API를 최대한의 유연성 및 제어 Ui를 만드는 개발자에 게 제공 합니다. 또한 요소 API를 아주 빨리 선언인 뿐만 아니라 서버에서 동적 UI 생성 허용 하는 JSON을 통해 선언적 정의 대 한 지원을 발전 했습니다. 
-   **높은 수준의 리플렉션 API** – 라고도 합니다 *바인딩*  *API* , UI 힌트와 다음 산 주석이 지정 된 클래스에서 D 화면 개체를 기반으로 자동으로 만들고 간에 새로운 바인딩을 표시 (및 필요에 따라 편집) 화면에서 및 백업 원본 개체를 제공 합니다. 위의 예제에서는 리플렉션 API의 사용을 보여 줍니다. 이 API는 API 요소는 세분화 된 제어를 제공 하지 않습니다 하지만 자동으로 클래스 특성을 기반으로 요소 계층 구조를 구축 하 여 복잡성을 더욱 줄입니다. 


산 D는 큰 집합으로 압축 된 화면 만들기에 대 한 UI 요소에 기본 제공 하지만 것도 사용자 지정된 요소 및 고급 화면 레이아웃에 대 한 필요성을 인식 합니다. 따라서 확장성은 최우선 추천 API로 구운입니다. 개발자가 기존 요소를 확장 또는 새로 만들 고 원활 하 게 통합할 수 있습니다.

또한 산 D의 여러 가지 일반적인 iOS UX 기능 "끌어오기 새로 고침"을 지원 하 고 비동기 이미지 로드 및 검색 지원 같은 기본 제공 합니다.

이 문서에서는 산 사용에 대해 포괄적으로 걸립니다 합니다. D를 포함 합니다.

-   **산 D 구성 요소** –이 중점적으로 산을 구성 하는 클래스 이해 D 속도 빠르게 얻을 수 있도록 합니다. 
-   **요소 참조** – 산의 기본 제공 요소의 포괄적인 목록 4. 
-   **고급 사용법** –이 끌어오기-새로 고침, 검색, 배경 이미지 로드, LINQ를 사용 하 여 요소 계층을 구축 및 사용자 지정 요소에 셀을 만들기와 같은 고급 기능에 설명 하 고 컨트롤러에 대 한 산 사용 4. 

## <a name="setting-up-mtd"></a>산 설정 D

산 D는 Xamarin.iOS를 사용 하 여 배포 됩니다. 를 사용 하려면 마우스 오른쪽 단추로 클릭 합니다 **참조** Xamarin.iOS 노드의 Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트 및 참조를 추가 합니다 **MonoTouch.Dialog 1** 어셈블리. 그런 다음 추가 `using MonoTouch.Dialog` 필요에 따라 원본에서 문을 코딩 합니다.

## <a name="understanding-the-pieces-of-mtd"></a>산 가지 이해 D

리플렉션 API, 산을 사용 하는 경우에 D 것 처럼 요소 API를 통해 직접 생성 된 내부적으로 요소 계층 구조를 만듭니다. 또한 이전 섹션에서 언급 한 JSON 지원은도 요소가 생성 됩니다. 이러한 이유로 것에 산의 구성 요소 부분에 대 한 기본 이해 4.

산 D는 다음 네 가지 부분을 사용 하 여 화면을 빌드합니다.

-  **DialogViewController**
-  **RootElement**
-  **섹션**
-  **요소**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, 또는 *DVC* 줄여서에서 상속 `UITableViewController` 따라서 테이블을 사용 하 여 화면을 나타냅니다. DVCs 일반 UITableViewController 마찬가지로 탐색 컨트롤러에 푸시할 수 있습니다.

### <a name="rootelement"></a>RootElement

A *RootElement* 는 최상위 컨테이너를 DVC 속하게 될 항목입니다. 다음 요소를 포함할 수 있는 섹션을 포함 합니다. RootElements 렌더링 되지 않습니다. 대신 어떤 실제로 가져옵니다 렌더링에 대 한 컨테이너 하기만 하면 됩니다. RootElement DVC, 할당 되었고이 DVC 자식을 렌더링 합니다.

### <a name="section"></a>단원

섹션은 그룹 표의 셀입니다. 일반 테이블 섹션을 사용 하 여 필요에 따라은 텍스트 또는 다음 스크린샷과 같이 사용자 지정 뷰 헤더 및 바닥글을 저장할 수 있습니다 수 있습니다:

 [![](images/image2.png "머리글과 바닥글 중 하나를 수행할 수 있습니다. 있는 텍스트 또는이 스크린샷과 같이 사용자 지정 뷰 수 일반 테이블의 특정 섹션에 있는 필요에 따라 수 있습니다.")](images/image2.png#lightbox)

### <a name="element"></a>요소

요소는 실제 테이블 셀을 나타냅니다. 산 D는 다양 한 서로 다른 데이터 형식 또는 서로 다른 입력을 나타내는 요소를 사용 하 여 압축 되어 있습니다. 예를 들어 다음 스크린샷에서 몇을 가지 사용 가능한 요소를 보여 줍니다.

 [![](images/image3.png "예를 들어이 스크린샷에서 사용 가능한 요소 중 일부를 보여줍니다.")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>자세한 on 섹션과 RootElements

이제 살펴보겠습니다 RootElements 및 섹션에서 더 자세히 설명 합니다.

### <a name="rootelements"></a>RootElements

하나 이상의 RootElement MonoTouch.Dialog 프로세스를 시작 해야 합니다.

RootElement 섹션/요소 값을 사용 하 여 초기화 되 면이 값은 자식 디스플레이의 오른쪽에 렌더링 되는 구성의 요약을 제공 하는 요소를 찾으려고 사용 됩니다. 예를 들어 아래 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽의 "후 식"을 선택한 desert의 값과 함께 제목의 포함 된 셀을 사용 하 여 합니다.

 [![](images/image4.png "이 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽 후 식, 선택한 desert의 값과 함께 제목이 포함 된 셀이 있는")](images/image4.png#lightbox) [![](images/image5.png "이 아래 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽의 후 식, 선택한 desert의 값과 함께 제목의 포함 된 셀을 사용 하 여")](images/image5.png#lightbox)

루트 요소 데도 사용할 수 있습니다 섹션 내에서 중첩 된 구성 하는 새 페이지를 로드 트리거를 위와 같이 합니다. 이 모드에서 사용 하는 경우에 제공 캡션 섹션 내에서 렌더링 하는 동안 사용 되 고 하위 페이지에 대 한 제목으로도 사용 됩니다. 예를 들어:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

위의 예제에서는 사용자가 "후 식"를 누를 때 MonoTouch.Dialog 새 페이지를 만들고 "후 식" 되 고 세 개의 값을 사용 하 여 라디오 그룹을 유지 하는 루트로 이동 하 합니다.

이 특정 예제의 라디오 그룹 값 "2"는 RadioGroup를 전달 하기 때문에 "후 식" 단원의 "초콜릿 케이크"를 선택 합니다. 즉, (0 인덱스) 목록에서 세 번째 항목을 선택 합니다.

Add 메서드를 호출 또는 C# 4 이니셜라이저 구문을 사용 하 여 섹션을 추가 합니다.
애니메이션을 사용 하 여 섹션을 삽입 하려면 Insert 메서드를 제공 됩니다.

RootElement RadioGroup) (대신 그룹 인스턴스를 만드는 경우 요약 섹션에 표시 될 때 RootElement 변수의 모든 BooleanElements 및 Group.Key 값으로 동일한 키가 있는 CheckboxElements의 누적 개수 됩니다.

### <a name="sections"></a>섹션

화면에 요소를 그룹화 하는 데 사용 되며는 RootElement의 직계 자식만 유효 합니다. 섹션에서는 새 RootElements를 비롯 한 표준 요소를 포함할 수 있습니다.

RootElements 섹션에 포함 된 새로운 더 깊은 수준으로 이동 하는 데 사용 됩니다.

문자열 또는 UIViews 머리글 및 바닥글 섹션이 있을 수 있습니다.
일반적으로 사용 된 문자열에 있지만 사용자 지정 Ui를 만드는 모든 UIView 머리글 또는 바닥글으로 사용할 수 있습니다. 다음과 같이 만들려는 하거나 문자열을 사용할 수 있습니다.

```csharp
var section = new Section ("Header", "Footer")
```

뷰를 사용 하려면 생성자에 뷰를 전달 하기만 합니다.

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>알림 받기

#### <a name="handling-nsaction"></a>NSAction 처리

산 D 화면을 `NSAction` 콜백을 처리 하기 위한 대리자입니다.
예를 들어, 산에서 만든 테이블 셀에 대 한 터치 이벤트를 처리. 4. 산을 사용 하 여 요소를 만들 때 아래 표시 된 대로 D, 단순히 입력 콜백 함수:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>요소 값 검색

와 결합 합니다 `Element.Value` 속성 콜백에 다른 요소에 설정 된 값을 검색할 수 있습니다. 예를 들어, 다음 코드를 고려하세요.

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

이 코드는 아래와 같이 UI를 만듭니다. 이 예제의 전체 연습을 참조 하세요. 합니다 [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) 자습서입니다.

 [![](images/image6.png "Element.Value 속성와 함께 콜백을 검색할 수 있습니다 다른 요소에 설정 된 값")](images/image6.png#lightbox)

사용자 아래쪽 테이블 셀을 누르면 익명 함수에서 코드 실행에서 값을 쓰는 합니다 `element` 인스턴스를 **응용 프로그램 출력** mac 용 Visual Studio에서 패드

## <a name="built-in-elements"></a>기본 제공 요소

산 D 요소 라고 하는 기본 제공 테이블 셀 항목 수가 포함 되어 있습니다.
이러한 요소는 문자열, float, 날짜 및 이미지 라도 몇 이름을 같은 표 셀에 다양 한 종류를 표시 하는 데 사용 됩니다. 데이터 형식을 적절 하 게 표시 하는 각 요소를 담당 합니다. 예를 들어, 부울 요소에는 해당 값을 설정/해제 하는 스위치가 표시 됩니다. 마찬가지로, float 요소를 부동 소수점 값을 변경 하려면 슬라이더를 표시 합니다.

html 이미지 등 다양 한 데이터 형식을 지원 하기 위해 좀 더 복잡 한 요소가 있습니다. 예를 들어, 선택 하면 웹 페이지를 로드 하려면 UIWebView 열립니다는 html 요소를 사용 하는 테이블 셀의 캡션을 표시 합니다.

### <a name="working-with-element-values"></a>요소 값을 사용 하 여 작업

사용자 입력 캡처에 사용 되는 요소는 공개 노출 `Value` 언제 든 지 요소의 현재 값을 보유 하는 속성입니다. 사용자가 사용 하 여 응용 프로그램으로 자동으로 업데이트 됩니다.

모든 MonoTouch.Dialog, 포함 된 요소에 대 한 동작 이지만 사용자가 만든 요소에 대 한 필요 하지 않습니다.

### <a name="string-element"></a>문자열 요소

`StringElement` 캡션 테이블 셀을 셀의 오른쪽에 문자열 값을 왼쪽에 표시 합니다.

 [![](images/image7.png "테이블 셀을 셀의 오른쪽에 문자열 값을 왼쪽에는 StringElement 캡션을 보여 줍니다.")](images/image7.png#lightbox)

사용 하는 `StringElement` 을 button으로 대리자를 제공 합니다.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "StringElement 단추를 사용 하려면 대리자를 제공 합니다.")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>스타일이 적용 된 문자열 요소

`StyledStringElement` 허용 하거나 기본 제공 테이블 셀 스타일을 사용 하 여 표시 되는 문자열 또는 사용자 지정 서식 지정 합니다.

 [![](images/image9.png "StyledStringElement 허용 하거나 기본 제공 테이블 셀 스타일을 사용 하 여 표시 되는 문자열 또는 사용자 지정 서식 지정")](images/image9.png#lightbox)

합니다 `StyledStringElement` 클래스에서 파생 됩니다 `StringElement`, 하지만 있습니다 개발자 소수의 글꼴, 텍스트 색, 배경 셀 색, 줄 바꿈 모드를 표시 하는 줄 수와 같은 속성의 사용자 지정 및 액세서리를 표시할지 여부입니다.

### <a name="multiline-element"></a>여러 줄 요소

 [![](images/image10.png "여러 줄 요소")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry 요소

`EntryElement`의미 이름으로, 사용자 입력을 가져오는 데 사용 됩니다. 일반 문자열 또는 문자 숨겨져 있는 암호를 지원 합니다.

 [![](images/image11.png "EntryElement 사용자 입력을 가져오는 데 사용 됩니다.")](images/image11.png#lightbox)

세 가지 값으로 초기화 됩니다.

-  사용자에 게 표시 될 항목의 캡션.
-  자리 표시자 텍스트 (이 사용자에 게 힌트를 제공 하는 회색 텍스트)입니다. 
-  텍스트의 값입니다.


자리 표시자 및 값이 null 일 수 있습니다. 그러나 캡션이 필요합니다.

언제 든 지 값 속성 액세스 값을 검색할 수의 `EntryElement`합니다.

또한는 `KeyboardType` 데이터 항목에 대 한 원하는 키보드 형식 스타일으로 생성 시 속성을 설정할 수 있습니다. 이 값을 사용 하 여 키보드 구성에 사용할 수 있습니다 `UIKeyboardType` 아래에 나열 된:

-  Numeric
-  전화 번호
-  URL
-  메일


### <a name="boolean-element"></a>부울 요소

 [![](images/image12.png "부울 요소")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>확인란 요소

 [![](images/image13.png "확인란 요소")](images/image13.png#lightbox)

### <a name="radio-element"></a>라디오 요소

A `RadioElement` 필요를 `RadioGroup` 지정 되어야 합니다 `RootElement`합니다.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement는 RootElement에 지정할 RadioGroup 필요")](images/image14.png#lightbox)

 `RootElements` 라디오 요소를 조정 하도 사용 됩니다. `RadioElement` 멤버에는 여러 섹션 (예: 비슷하게 링 톤 선택기와 별도 사용자 지정 벨소리 시스템 벨 소리에서 구현) 걸쳐 있을 수 있습니다. 요약 뷰에서 현재 선택 된 라디오 요소를 표시 됩니다. 이 사용 하려면 만들기를 `RootElement` 같이 그룹 생성자를 사용 하 여:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

그룹의 이름을 `RadioGroup` (있는 경우)를 포함 하는 페이지에서 선택한 값이 0이 경우에 값 이며 처음 선택된 된 항목의 인덱스를 표시 하는 데 사용 됩니다.

### <a name="badge-element"></a>배지 요소

 [![](images/image15.png "배지 요소")](images/image15.png#lightbox)

### <a name="float-element"></a>Float 요소

 [![](images/image16.png "Float 요소")](images/image16.png#lightbox)

### <a name="activity-element"></a>작업 요소

 [![](images/image17.png "작업 요소")](images/image17.png#lightbox)

### <a name="date-element"></a>날짜 요소

 ![](images/image18.png "날짜 요소")

에 해당 하는 DateElement 셀을 선택 하면 날짜 선택은 아래와 같이 표시 됩니다.

 [![](images/image19.png "날짜 선택 표시 된 것 처럼 표시 됩니다는 DateElement에 해당 셀을 선택 하면")](images/image19.png#lightbox)

### <a name="time-element"></a>Time 요소

 [![](images/image20.png "Time 요소")](images/image20.png#lightbox)

에 해당 하는 TimeElement 셀을 선택 하면 아래와 같이 시간 선택이 표시 됩니다.

 [![](images/image21.png "시간 선택 표시 된 것 처럼 표시 됩니다는 TimeElement에 해당 셀을 선택 하면")](images/image21.png#lightbox)

### <a name="datetime-element"></a>날짜/시간 요소

 [![](images/image22.png "날짜/시간 요소")](images/image22.png#lightbox)

에 해당 하는 DateTimeElement 셀을 선택 하면 날짜/시간 선택기는 아래와 같이 표시 됩니다.

 [![](images/image23.png "날짜/시간 선택기와 같이 표시 됩니다는 DateTimeElement에 해당 셀을 선택 하면")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML 요소

 [![](images/image24.png "HTML 요소")](images/image24.png#lightbox)

합니다 `HTMLElement` 의 값을 표시 해당 `Caption` 테이블 셀의 속성입니다. 끄지 선택 합니다 `Url` 요소에 할당에서 로드 되는 `UIWebView` 아래와 같이 제어:

 [![](images/image25.png "아래와 같이 UIWebView 컨트롤에서 로드 되는 요소에 할당 된 Url을 끄지 선택")](images/image25.png#lightbox)

### <a name="message-element"></a>Message 요소

 [![](images/image26.png "Message 요소")](images/image26.png#lightbox)

### <a name="load-more-element"></a>자세한 요소를 로드 합니다.

이 요소를 사용 하 여 사용자가 목록에 더 많은 항목을 로드할 수 있도록 합니다. 일반 및 로드 캡션 뿐만 아니라 글꼴 및 텍스트 색을 사용자 지정할 수 있습니다.
`UIActivity` 표시기 애니메이션 효과 적용, 시작 되 고 사용자가 셀을 누를 때 로드 캡션이 표시 됩니다 차례로 `NSAction` 에 전달 된 생성자가 실행 됩니다. 한 번에 코드를 `NSAction` 가 완료 되 면는 `UIActivity` 표시기에 애니메이션 적용을 중지 하 고 일반 캡션 다시 표시 됩니다.

### <a name="uiview-element"></a>UIView 요소

또한 사용자 지정 `UIView` 를 사용 하 여 표시할 수는 `UIViewElement`합니다.

### <a name="owner-drawn-element"></a>소유자가 그린 요소

추상 클래스 이므로이 요소를 서브클래싱 해야 합니다. 재정의 해야 합니다 `Height(RectangleF bounds)` 요소의 높이 반환 해야 하는 방법 뿐만 `Draw(RectangleF bounds, CGContext context, UIView view)` 는 해야 할 지정된 된 범위 내 모든 사용자 지정된 그리기 컨텍스트 및 view 매개 변수를 사용 하 여에서. 이 요소는 하위 클래스는 힘든은 `UIView`, 및 반환 되도록 하려면 셀에 배치만 두 개의 간단한 재정의 구현 필요가 있습니다. 샘플 앱의 샘플 구현을 개선 보시는 `DemoOwnerDrawnElement.cs` 파일입니다.

클래스를 구현 하는 매우 간단한 예제는 다음과 같습니다.

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

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

`JsonElement` 서브 클래스입니다 `RootElement` 확장 하는 `RootElement` 로컬 또는 원격 url에서 중첩 자식 콘텐츠를 로드할 수 있어야 합니다.

`JsonElement` 되는 `RootElement` 두 가지 형태로 인스턴스화할 수 있습니다. 한 버전을 만듭니다는 `RootElement` 해당 주문형 콘텐츠를 로드 됩니다. 사용 하 여 만들어지며는 `JsonElement` 끝에서 내용을 로드 하려면 url 인수를 사용 하는 생성자.

```csharp
var je = new JsonElement ("Dynamic Data", "https://tirania.org/tmp/demo.json");
```

다른 폼에서 로컬 파일 또는 기존 데이터를 만듭니다 `System.Json.JsonObject` 이미 구문 분석:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

산을 사용 하 여 JSON을 사용 하 여 대 한 자세한 내용은 D, 참조를 [JSON 요소 연습](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) 자습서입니다.

## <a name="other-features"></a>기타 기능

### <a name="pull-to-refresh-support"></a>끌어오기-새로 고침 지원

 *끌어오기-에-* *새로 고침* 시각 효과 원래에서 발견 되는 *Tweetie2* 앱 많은 응용 프로그램 사이에서 인기 있는 적용 되었습니다.

자동 끌어오기-새로 고침 지원 프로그램 대화 상자에 추가할만 해야 할 두 가지: 사용자 데이터를 가져오는 경우 알림을 받을 수는 이벤트 처리기를 연결 하 고 알림는 `DialogViewController` 기본 상태로 다시 이동할 데이터가 로드 된 경우.

알림 후크 하는 것은 간단 합니다. 연결 하기만 합니다 `RefreshRequested` 이벤트에는 `DialogViewController`, 다음과 같은:

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

다음 방법을 `OnUserRequestedRefresh`를 일부 데이터 로드 큐, net에서 일부 데이터를 요청 하거나 데이터를 계산 하기 위한 스레드를 실행 합니다. 알려야 데이터가 로드 된 후의 `DialogViewController` 새 데이터에는 뷰를 기본 상태로 복원 하려면이 호출 하 여 수행 `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>검색 지원

검색을 지원 하도록 설정 합니다 `EnableSearch` 속성에 `DialogViewController`입니다. 설정할 수도 있습니다는 `SearchPlaceholder` 검색 표시줄에서 워터 마크 텍스트로 사용 하는 속성입니다.

검색 보기의 내용을 사용자 형식으로 변경 됩니다. 표시 되는 필드를 검색 하 고 해당 사용자에 게 표시 합니다. `DialogViewController` 프로그래밍 방식으로 시작, 종료 또는 결과에 새 필터 작업을 트리거하는 세 가지 메서드를 노출 합니다. 이러한 메서드는 다음과 같습니다.

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


시스템 확장 가능 하며 이므로 원하는 경우이 동작을 변경할 수 있습니다.

### <a name="background-image-loading"></a>배경 이미지 로드

MonoTouch.Dialog 통합 합니다 [TweetStation](https://github.com/migueldeicaza/TweetStation) 응용 프로그램의 이미지 로더. 이 이미지 로더가 캐싱 지원 백그라운드에서 이미지를 로드 하려면 사용할 수 있으며 이미지 로드 되었을 때 코드에 알릴 수 있습니다.

또한 나가는 네트워크 연결 수가 제한 됩니다.

이미지 로더가에서 구현 되는 `ImageLoader` 하기만 하면 클래스는 호출을 `DefaultRequestImage` 인스턴스의 뿐만 아니라 이미지를 로드 하려는 Uri를 제공 해야 메서드를는 `IImageUpdated` 될 인터페이스 될 때 호출 이미지 ha 가 로드 되었습니다.

다음 코드에 Url에서 이미지를 로드 하는 예를 들어 한 `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader 클래스의 현재 메모리에 캐시 된 이미지를 모두 해제 하려는 경우 호출할 수 있는 제거 메서드를 노출 합니다. 현재 코드에 50 이미지에 대 한 캐시 합니다. 경우 (예를 들어, 예상 되는 경우 50 이미지의 너무 많은 수는 너무 큰 이미지)는 다양 한 캐시 크기를 사용 하려는 방금 ImageLoader의 인스턴스를 만들 하 고 캐시에 유지 하려는 이미지의 수를 전달할 수 있습니다.

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ를 사용 하 여 요소 계층 구조를 만들려면

Clever LINQ 및 C#의 초기화 구문 사용을 통해 LINQ 요소 계층을 만들려면 사용할 수 있습니다. 예를 들어, 다음 코드는 화면 일부 문자열 배열에서 만들고 핸들 셀 선택 영역 각각에 전달 되는 익명 함수를 통해 `StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

이 XML 데이터 저장소에 거의 완전히 데이터로 복잡 한 응용 프로그램을 만들려면 데이터베이스에서에서 데이터를 쉽게 결합할 수 없습니다.

## <a name="extending-mtd"></a>산 확장 D

### <a name="creating-custom-elements"></a>사용자 지정 요소 만들기

기존 요소에서 상속 하 여 또는 루트 요소 클래스에서 파생 하 여 고유한 요소를 만들 수 있습니다.

고유한 요소를 만들려면 다음 메서드를 재정의 해야 합니다.

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

구현 해야 하는 요소는 변수 크기 수 있으면는 `IElementSizing` 메서드 하나를 포함 하는 인터페이스:

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

구현 하려는 경우에 `GetCell` 메서드를 호출 `base.GetCell(tv)` 반환된 된 셀을 사용자 지정도 재정의 해야 하 고는 `CellKey` 같이 요소를 고유 하 게 표시 되는 키를 반환할 속성:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

대부분의 요소에 대 한 작동 합니다 `StringElement` 고 `StyledStringElement` 다양 한 렌더링 시나리오에 대 한 고유한 키 집합을 사용 하는 것입니다. 이러한 클래스의 코드를 복제 해야 합니다.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

리플렉션 및 요소 API를 사용 하 여 동일한 `DialogViewController`입니다. 일부 기능을 사용 하려는 또는 보기의 모양을 사용자 지정할 수도 있습니다는 `UITableViewController` 는 Ui의 기본 만드는 것 이상으로 이동 합니다.

`DialogViewController` 단순히의 서브 클래스를 `UITableViewController` 에서 사용자 지정은 동일한 방식으로 사용자 지정할 수 있습니다는 `UITableViewController`.

예를 들어, 되려면 목록 스타일을 변경 하려는 경우 `Grouped` 또는 `Plain`, 다음과 같은 컨트롤러를 만들 때 속성을 변경 하 여이 값을 설정할 수 있습니다.

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

고급 사용자 지정을 `DialogViewController`, 배경, 설정 하는 등 경우 서브 클래스 하 고 적절 한 재정의 메서드를 아래 예와에서 같이:

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

다른 사용자 지정 점의 다음 가상 메서드는 `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

이 메서드는의 서브 클래스를 반환 해야 `DialogViewController.Source` 사례는 셀 크기가 균일, 또는의 서브 클래스에 대 한 `DialogViewController.SizingSource` 셀도 아닌 경우.

이 재정의 사용 하 여의 캡처하는 `UITableViewSource` 메서드. 예를 들어 [TweetStation](https://github.com/migueldeicaza/TweetStation) 이 사용 하 여 사용자 맨 위로 스크롤 하는 경우 추적을 읽지 않은 트 윗의 수를 적절 하 게 업데이트 합니다.

## <a name="validation"></a>유효성 검사

요소를 제공 하지 않고 유효성 검사 자체 웹 페이지에 적합 하 게 연결 된 모델로 데스크톱 응용 프로그램은 iPhone 상호 작용 모델에 직접 매핑되지 않습니다.

데이터 유효성 검사를 수행 하려는 경우 사용자 입력 된 데이터를 사용 하 여 작업을 트리거하는 경우이 수행 해야 합니다. 예를 들어를 <span class="ui">수행</span> 또는 <span class="ui">다음</span> 위쪽 도구 모음 또는 일부 단추 `StringElement` 다음 단계로 이동 하려면 단추를 사용 합니다.

이것이, 여기서 기본 입력된 유효성 검사를 수행 하 고 유효성 검사는 서버를 사용 하 여 사용자/암호 조합의 유효성 검사와 같은 복잡 한 것입니다.

오류가 사용자에 게 어떻게 하는 것은 특정 응용 프로그램입니다. 팝업 수는 `UIAlertView` 나 힌트를 표시 합니다.

## <a name="summary"></a>요약

이 문서에서는 많은 MonoTouch.Dialog에 대 한 정보를 설명 합니다. 이 설명 하는 방법의 기본적인 사항을 산 D 산을 구성 하는 다양 한 구성 요소를 설명 하며 4. 다양 한 요소 및 산에서 지 원하는 테이블 사용자 지정 하는 방법도 보여 줬습니다. D 하는 방법을 설명 하 고 산 D는 사용자 지정 요소를 사용 하 여 확장할 수 있습니다. 또한 산의 JSON 지원을 설명 했습니다. JSON에서 요소를 동적으로 만들 수 있는 차원입니다.


## <a name="related-links"></a>관련 링크

- [스크린 캐스트-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여는 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [스크린 캐스트-MonoTouch.Dialog를 사용 하 여 iOS 사용자 인터페이스를 쉽게 만들기](http://youtu.be/j7OC5r8ZkYg)
- [연습: 요소 API를 사용하여 애플리케이션 만들기](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [연습: 리플렉션 API를 사용하여 애플리케이션 만들기](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [연습: JSON 요소를 사용 하 여 사용자 인터페이스를 만들려면](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON 태그](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
