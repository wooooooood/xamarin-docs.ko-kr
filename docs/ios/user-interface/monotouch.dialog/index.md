---
title: "MonoTouch.Dialog 소개"
description: "(산 MonoTouch.Dialog D) toolkit은 신속한 응용 프로그램 UI 개발 Xamarin.iOS에 대 한 필수적인 프레임입니다. 산 D를 사용 하면 빠르고 쉽게 복잡 한 응용 프로그램 탐색 컨트롤러, 테이블 등의 덜어 보다는 선언적 방식을 사용 하 여 UI를 정의할 수 있습니다. 또한 산 D 유연한 전체 컨트롤이 나 넘깁니다 접근 방식으로 끌어오기-새로 고침, 배경 이미지를 로드 하는 등의 추가 기능을 개발자에 게 제공, 지원 및 JSON 데이터를 통해 동적 UI 생성을 검색 하는 Api 집합을 있습니다. 이 가이드에서는 산 사용 하는 여러 가지 방법을 소개합니다 D 및 고급 사용 심층적으로 소개 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b279f3e643e008e88b8ad086c400d992427c6df4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-monotouchdialog"></a>MonoTouch.Dialog 소개

_(산 MonoTouch.Dialog D) toolkit은 신속한 응용 프로그램 UI 개발 Xamarin.iOS에 대 한 필수적인 프레임입니다. 산 D를 사용 하면 빠르고 쉽게 복잡 한 응용 프로그램 탐색 컨트롤러, 테이블 등의 덜어 보다는 선언적 방식을 사용 하 여 UI를 정의할 수 있습니다. 또한 산 D 유연한 전체 컨트롤이 나 넘깁니다 접근 방식으로 끌어오기-새로 고침, 배경 이미지를 로드 하는 등의 추가 기능을 개발자에 게 제공, 지원 및 JSON 데이터를 통해 동적 UI 생성을 검색 하는 Api 집합을 있습니다. 이 가이드에서는 산 사용 하는 여러 가지 방법을 소개합니다 D 및 고급 사용 심층적으로 소개 합니다._


산 라고 MonoTouch.Dialog 줄여서 D 응용 프로그램 화면 및 덜어 컨트롤러 보기, 테이블 등을 만드는 것이 아니라 정보를 사용 하 여 탐색을 빌드하기 위해 개발자가 사용할 수 있는 신속한 UI 개발 도구 키트입니다. 따라서 UI 코드와 개발 감소의 중요 한 단순화를 제공합니다. 예를 들어 다음 스크린 샷에서를 것이 좋습니다.

 [ ![](images/image1.png "예를 들어이 스크린 샷")](images/image1.png)

다음 코드는이 전체 화면 정의에 사용 된:

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

IOS에서 테이블을 사용 하는 경우 있습니다 엄청난 양의 반복적인 코드입니다.
예를 들어 테이블 필요할 때마다 데이터 원본은 해당 테이블을 채우는 데 필요 합니다. 탐색 컨트롤러를 통해 연결 되어 있는 두 테이블 기반 화면에 있는 응용 프로그램에서 각 화면 동일한 코드를 많이 공유 합니다.

산 D 간소화 하는 해당 코드의 모든 테이블 생성을 위한 제네릭 API를 캡슐화 합니다. 그런 다음 바인딩 보다 쉽게 구문 선언적 개체에 대 한 허용 하는 API 기반으로 추상화를 제공 합니다. 따라서 두 가지 Api 산에서 사용할 수 있는 D:

-   **하위 수준 요소 API** – *요소 API* 화면 및 해당 구성 요소를 나타내는 요소의 계층적 트리를 만드는 방법에 기반 합니다. 요소 API는 가장 뛰어난 유연성 및 제어 Ui를 만드는 개발자에 게 제공 합니다. 또한, 요소 API는 선언적 정의 대 한 지원으로 아주 빨리 선언 하는 서버에서 동적 UI 생성을 허용 하는 JSON을 통해 고급 합니다. 
-   **수준의 리플렉션 API** – 라고도 *바인딩**API* , 클래스 주석을 지정 하 여 UI 힌트와 산에서 D 화면에서 개체를 기반으로 자동으로 만들고 백업 원본 개체와 내용 간의 바인딩을 표시 (및 필요에 따라 편집) 화면에서 제공 합니다.   위의 예제에서는 리플렉션 API의 사용을 방법을 보여 줍니다. 이 API는 API 요소 않는 세분화 된 제어를 제공 하지 않습니다 하지만 클래스 특성을 기반으로 요소 계층으로 자동으로 작성 하 여 복잡성을 더욱 줄입니다. 


산 D 제공 큰 집합으로 압축으로 화면 만들기에 대 한 UI 요소에 기본 제공 되지만 사용자 지정된 요소와 고급 화면 레이아웃에 대 한 필요성도 인식 합니다. 따라서 확장성은 첫 번째 클래스 갖춘 API에는 기본 제공 됩니다. 개발자 기존 요소를 확장할 수 있습니다 또는 새로 만들와 완벽 하 게 통합 합니다.

또한 산 D에 다양 한 "끌어오기 새로 고침" 지원, 비동기 이미지 로드 및 지원 검색와 같은 기본 제공 하는 일반적인 iOS UX 기능이 있습니다.

이 문서에는 산 사용에 대 한 포괄적인 개요를 걸립니다. D, 포함 하 여:

-   **산 구성 요소 D** – 이해 산 구성 하는 클래스에 대해 중점적으로이 속도 신속 하 게 가져오는 사용할 수 있도록 D입니다. 
-   **요소 참조** – 전체 목록이 산의 기본 제공 요소 4. 
-   **고급 사용** –이 끌어오기-새로 고침, 검색, 배경 이미지 로드, 요소 계층을 빌드하기 위해 LINQ를 사용 하 여 및 사용자 지정 요소를 셀을 만드는 등의 고급 기능에 설명 하 고 컨트롤러에 대 한 산 사용 4. 

## <a name="understanding-the-pieces-of-mtd"></a>산 가지 이해 D

리플렉션 API, 산 사용 하는 경우에 D 경우 직접 요소 API를 통해 생성 된 것 처럼 내부적으로 요소 계층 구조를 만듭니다. 또한 이전 섹션에서 언급 한 JSON 지원 요소도으로 만듭니다. 이 이유로 것이 중요 한지 산의 구성 부분에 대 한 기본적인 이해 4.

산 D 다음 네 부분을 사용 하 여 화면을 빌드합니다.

-  **DialogViewController**
-  **RootElement**
-  **섹션**
-  **요소**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*, 또는 *DVC* 줄여서에서 상속 `UITableViewController` 따라서 테이블에는 화면을 나타냅니다. DVCs 일반 UITableViewController와 동일 하 게 탐색 컨트롤러에 푸시할 수 있습니다.

### <a name="rootelement"></a>RootElement

A *RootElement* 는 DVC 속하게 될 항목에 대 한 최상위 컨테이너입니다. 다음 요소를 포함할 수 있는 섹션을 포함 합니다. RootElements 렌더링 되지 않습니다. 대신 단순히 컨테이너 렌더링할 실제로 기능에 대 한 일입니다. RootElement DVC, 할당 되었고이 DVC 자식을 렌더링 합니다.

### <a name="section"></a>단원

섹션은 테이블에 있는 셀의 그룹입니다. 일반 테이블 섹션으로이 발생할 수 있으므로 필요에 따라는 머리글 및 바닥글 유지할 수 있습니다. 있는 텍스트 또는 다음 스크린 샷에서 같이 사용자 지정 보기 수 있습니다:

 [ ![](images/image2.png "머리글 및 바닥글을 유지할 수 될 텍스트 또는이 스크린샷에서 같이 사용자 지정 뷰를 일반 테이블 섹션으로이 발생할 수 있으므로 필요에 따라")](images/image2.png)

### <a name="element"></a>요소

요소는 테이블에는 실제 셀을 나타냅니다. 산 D 압축는 다양 한 데이터 형식이 나 다른 서로 다른 입력을 나타내는 요소와 함께 제공 됩니다. 예를 들어 다음 스크린샷에서 사용 가능한 요소 중 일부를 보여 줍니다.

 [ ![](images/image3.png "이 스크린샷을 사용할 수 있는 요소 중 일부를 설명 하는 예를 들어")](images/image3.png)

## <a name="more-on-sections-and-rootelements"></a>자세한 on 섹션 및 RootElements

이제 살펴보겠습니다 RootElements 및 섹션 보다 자세히 설명에서 합니다.

### <a name="rootelements"></a>RootElements

하나 이상의 RootElement MonoTouch.Dialog 프로세스를 시작 해야 합니다.

RootElement 섹션/요소 값으로 초기화 되 면이 값은 자식 디스플레이의 오른쪽에 렌더링 되는 구성의 요약을 제공 하는 요소를 찾으려고 사용 됩니다. 예를 들어, 아래 스크린샷에서 테이블을 보여 줍니다 왼쪽에 셀의 오른쪽에 "후 식" 선택한 사막 값과 함께 세부 정보 화면 제목을 포함 합니다.

 [ ![](images/image4.png "이 스크린샷은 테이블 왼쪽의 세부 정보 화면 오른쪽의 후 식, 선택한 desert 값과 함께 제목이 포함 된 셀과") ](images/image4.png) [ ![ ] (images/image5.png "이 아래 스크린샷에서 세부 정보 화면 오른쪽의 후 식, 선택한 사막 값과 함께 제목이 포함 된 셀과 표를 왼쪽에 표시")](images/image5.png)

루트 요소 데도 사용할 수 있습니다 섹션 내 새 중첩 된 구성 페이지를 로드할 트리거할 위와 같이 합니다. 이 모드에서 사용 되는 캡션을 제공 섹션 내에서 렌더링 중에 사용 하 고 하위 페이지에 제목으로도 사용 됩니다. 예:

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

위의 예제에서는 사용자가 "후 식"를 누를 때 MonoTouch.Dialog 새 페이지를 만들는 "후 식" 되 고 3 개의 값이 포함 된 라디오 그룹 있으면 루트와 함께 이동 합니다.

이 샘플에서 라디오 그룹이 RadioGroup에 "2" 값을 전달 했습니다 때문에 "후 식" 단원의 "초콜릿 케이크"를 선택 합니다. 즉, 세 번째 항목 목록 (0 인덱스)를 선택 합니다.

Add 메서드를 호출 또는 C# 4 이니셜라이저 구문을 사용 하 여 섹션을 추가 합니다.
애니메이션 하는 섹션을 삽입 하는 삽입 메서드 제공 됩니다.

RootElement RadioGroup) (대신 그룹 인스턴스를 생성 한 경우 요약 값 한 섹션에 표시 될 때 RootElement의 모든 BooleanElements 및 Group.Key 값과 동일한 키가 들어 CheckboxElements의 누적 개수 됩니다.

### <a name="sections"></a>섹션

섹션은 화면에 요소를 그룹화 하는 데 사용 됩니다 하 고는 RootElement의 유일한 유효 직계 자식입니다. 섹션에 새 RootElements를 비롯 한 표준 요소를 포함 될 수 있습니다.

RootElements 섹션에 포함 된 새로운 깊은 수준으로 이동 하는 데 사용 됩니다.

문자열이 나 UIViews로 머리글 및 바닥글 섹션이 있을 수 있습니다.
일반적으로 사용 된 문자열에 있지만 사용자 지정 Ui를 만들 모든 UIView 머리글 또는 바닥글으로 사용할 수 있습니다. 다음과 같이 만들려고 하거나 문자열을 사용할 수 있습니다.

```csharp
var section = new Section ("Header", "Footer")
```

뷰를 사용 하려면 방금 뷰 생성자에 전달 합니다.

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>알림을 받는

#### <a name="handling-nsaction"></a>NSAction 처리

산 D 표면은 `NSAction` 콜백을 처리에 대 한 대리자입니다.
예를 들어, 산에서 만든 표 셀에 대 한 터치 이벤트를 처리. 4. 산으로 요소를 만들 때 D, 단순히 제공 콜백 함수를 다음과 같이:

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>요소 값 검색

와 결합 된 `Element.Value` 속성을 콜백 다른 요소에 설정 된 값을 검색할 수 있습니다. 예를 들어, 다음 코드를 고려하세요.

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

이 코드는 아래와 같이 UI를 만듭니다. 이 예제는 전체 연습은 대 한 참조는 [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) 자습서입니다.

 [ ![](images/image6.png "Element.Value 속성을 함께 콜백 값을 검색할 수는 다른 요소에서 설정")](images/image6.png)

아래쪽 테이블 셀을 누를 때는 익명 함수에 코드 실행에서 값을 쓰는 `element` 인스턴스는 **응용 프로그램 출력** Mac.에 대 한 Visual Studio에서 채움

## <a name="built-in-elements"></a>기본 제공 요소

산 D 요소 라고 하는 기본 제공 테이블 셀 항목 수가 포함 되어 있습니다.
이러한 요소는 서로 다른 형식의 다양 한 문자열, float, 날짜 및 이미지 라도 군에 같은 테이블 셀에 표시 하는 데 사용 됩니다. 각 요소는 데이터 형식을 적절 하 게 표시 처리 합니다. 예를 들어 부울 요소에는 해당 값을 설정/해제 하는 스위치가 표시 됩니다. 마찬가지로, float 요소는 float 값을 변경 하려면 슬라이더를 표시 합니다.

Html 및 이미지와 같은 다양 한 데이터 형식을 지원 하기는 훨씬 더 복잡 한 요소가 있습니다. 예를 들어 html 요소를 선택 하면 웹 페이지를 로드할 UIWebView 열리고, 테이블 셀의 캡션을 표시 합니다.

### <a name="working-with-element-values"></a>요소 값 작업

사용자 입력 캡처에 사용 되는 요소는 공개 노출 `Value` 언제 든 지 요소의 현재 값을 보유 하는 속성입니다. 사용자는 응용 프로그램을 사용 하는 대로 자동으로 업데이트 됩니다.

모든 MonoTouch.Dialog, 포함 된 요소에 대 한 동작 이지만 사용자가 만든 요소에 대 한 필요 하지 않습니다.

### <a name="string-element"></a>문자열 요소

A `StringElement` 캡션을 표 셀과 셀의 오른쪽에 문자열 값의 왼쪽에 표시 합니다.

 [ ![](images/image7.png "표 셀과 셀의 오른쪽에 문자열 값의 왼쪽에는 StringElement 캡션을 보여 줍니다.")](images/image7.png)

사용 하는 `StringElement` 단추와 대리자를 제공 합니다.

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [ ![](images/image8.png "StringElement 단추를 사용 하려면 한 대리자를 제공 합니다.")](images/image8.png)

### <a name="styled-string-element"></a>스타일이 적용 된 문자열 요소

A `StyledStringElement` 기본 표 셀 스타일 중 하나를 사용 하 여 표시 되는 문자열을 허용 하거나 사용자 지정 서식을 사용 하 여 합니다.

 [ ![](images/image9.png "StyledStringElement 기본 표 셀 스타일 중 하나를 사용 하 여 표시 되는 문자열을 허용 하거나 사용자 지정 서식을 지정 하 여")](images/image9.png)

`StyledStringElement` 클래스에서 파생 `StringElement`, 되지만 사용 하면 개발자가 사용자는 소수의 글꼴, 텍스트 색, 배경 셀 색, 줄 바꿈 모드, 개수의 줄을 표시 하려면 같은 속성을 지정 하 고는 액세서리를 표시할지 여부입니다.

### <a name="multiline-element"></a>여러 줄 요소

 [ ![](images/image10.png "여러 줄 요소")](images/image10.png)

### <a name="entry-element"></a>Entry 요소

`EntryElement`, 의미 이름으로, 사용자 입력을 가져오는 데 사용 됩니다. 일반 문자열 또는 문자 숨겨져 있습니다 암호를 지원 합니다.

 [ ![](images/image11.png "EntryElement 사용자 입력을 가져오는 데 사용 됩니다.")](images/image11.png)

세 가지 값으로 초기화 됩니다.

-  사용자에 게 표시 되는 항목에 대 한 캡션.
-  자리 표시자 텍스트 (이 사용자에 게 힌트를 제공 하는 회색으로 표시 아웃 텍스트)입니다. 
-  텍스트의 값입니다.


자리 표시자 및 값 null 일 수 있습니다. 그러나 캡션을 필요 합니다.

언제 든 지 Value 속성에 액세스할 수의 값을 검색 된 `EntryElement`합니다.

또한는 `KeyboardType` 데이터 항목에 대 한 필요한 키보드 형식 스타일을 만들 때 속성을 설정할 수 있습니다. 이 구성의 값을 사용 하 여 키보드를 사용할 수 있습니다 `UIKeyboardType` 아래와 같이:

-  Numeric
-  전화 번호
-  URL
-  메일


### <a name="boolean-element"></a>부울 요소

 [ ![](images/image12.png "부울 요소")](images/image12.png)

### <a name="checkbox-element"></a>확인란 요소

 [ ![](images/image13.png "확인란 요소")](images/image13.png)

### <a name="radio-element"></a>라디오 요소

A `RadioElement` 필요는 `RadioGroup` 를 지정 해야는 `RootElement`합니다.

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [ ![](images/image14.png "RadioElement는 RootElement 지정 되어 RadioGroup 필요")](images/image14.png)

 `RootElements` 라디오 요소 조정도 사용 됩니다. `RadioElement` 멤버 등 비슷한 링 톤 선택기와 별도 사용자 지정 벨소리 시스템 벨 소리 등에서 구현 하는 여러 섹션을 확장할 수 있습니다. 요약 보기에는 현재 선택 된 라디오 요소를 표시 됩니다. 이 사용 하려면 만듭니다는 `RootElement` 다음과 같이 그룹 생성자에서:

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

그룹의 이름을 `RadioGroup` (있는 경우) 포함 하는 페이지에서 선택한 값 및 값이 0 인이 경우, 해당 값은 첫 번째 선택된 항목의 인덱스를 표시 하는 데 사용 됩니다.

### <a name="badge-element"></a>배지 요소

 [ ![](images/image15.png "배지 요소")](images/image15.png)

### <a name="float-element"></a>Float 요소

 [ ![](images/image16.png "Float 요소")](images/image16.png)

### <a name="activity-element"></a>작업 요소

 [ ![](images/image17.png "작업 요소")](images/image17.png)

### <a name="date-element"></a>날짜 요소

 ![](images/image18.png "날짜 요소")

에 해당 하는 DateElement 셀을 선택 하면 아래와 같이 날짜 선택 표시 됩니다.

 [ ![](images/image19.png "날짜 선택 표시 된 것 처럼 표시 됩니다는 DateElement를 해당 셀을 선택 하면")](images/image19.png)

### <a name="time-element"></a>Time 요소

 [ ![](images/image20.png "Time 요소")](images/image20.png)

에 해당 하는 TimeElement 셀을 선택 하면 시간 선택은 아래와 같이 표시 됩니다.

 [ ![](images/image21.png "표시 된 것 처럼 시간 선택은 표시에 해당 하는 TimeElement 셀을 선택 하면")](images/image21.png)

### <a name="datetime-element"></a>날짜/시간 요소

 [ ![](images/image22.png "날짜/시간 요소")](images/image22.png)

에 해당 하는 DateTimeElement 셀을 선택 하면 날짜/시간 선택기는 아래와 같이 표시 됩니다.

 [ ![](images/image23.png "날짜/시간 선택기와 같이 표시 됩니다에 해당 하는 DateTimeElement 셀을 선택 하면")](images/image23.png)

### <a name="html-element"></a>HTML 요소

 [ ![](images/image24.png "HTML 요소")](images/image24.png)

`HTMLElement` 의 값을 표시 해당 `Caption` 테이블 셀의 속성입니다. 을 선택 하는 위치는 `Url` 요소에 할당에서 로드 되는 `UIWebView` 아래와 같이 제어:

 [ ![](images/image25.png "아래와 같이 UIWebView 컨트롤에 로드 되는 요소에 할당 된 Url을 끄지 선택")](images/image25.png)

### <a name="message-element"></a>Message 요소

 [ ![](images/image26.png "Message 요소")](images/image26.png)

### <a name="load-more-element"></a>더 많은 요소를 로드 합니다.

이 요소를 사용 하 여 사용자가 목록에 더 많은 항목을 로드할 수 있도록 합니다. 일반 및 로드 캡션 뿐 아니라 글꼴 및 텍스트 색을 사용자 지정할 수 있습니다.
`UIActivity` 표시기 애니메이션을 시작 하 고 사용자가 셀을 누를 때 로드 캡션이 표시 됩니다 차례로 `NSAction` 에 전달 된 생성자가 실행 됩니다. 코드에 한 번의 `NSAction` 가 완료 되 면는 `UIActivity` 표시기 애니메이션 적용을 중지 하 고 일반 캡션이 다시 표시 됩니다.

### <a name="uiview-element"></a>UIView 요소

또한 사용자 지정 `UIView` 를 사용 하 여 표시 될 수 있습니다는 `UIViewElement`합니다.

### <a name="owner-drawn-element"></a>소유자가 그린 요소

추상 클래스 이므로이 요소를 서브클래싱 합니다. 재정의 해야 하는 `Height(RectangleF bounds)` 메서드는 요소의 높이 반환 해야 하으로 `Draw(RectangleF bounds, CGContext context, UIView view)` 를 수행 해야 해당된 범위 내에서 모든 사용자 지정된 그리기 컨텍스트 및 view 매개 변수를 사용 하 여에 합니다. 이 요소는 하위 클래스의 중요 한 역할을 `UIView`, 반환 될 셀에 배치할 수만 두 개의 간단한 재정의 구현 해야 합니다. 샘플 앱에 더 나은 샘플 구현을 볼 수는 `DemoOwnerDrawnElement.cs` 파일입니다.

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

`JsonElement` 의 서브 클래스가 `RootElement` 확장 하는 `RootElement` 중첩 된 자식의 내용을 로컬 또는 원격 url에서 로드할 수 있습니다.

`JsonElement` 는 `RootElement` 두 가지 형태로 인스턴스화할 수 있습니다. 한 버전을 만듭니다는 `RootElement` 내용을 필요할 때 로드 됩니다. 사용 하 여 만들어집니다는 `JsonElement` 끝에서 내용을 로드 하려면 url 인수를 사용 하는 생성자:

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

다른 폼은 로컬 파일 또는 기존에서 데이터를 만듭니다 `System.Json.JsonObject` 이미 구문 분석:

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

산 JSON 사용에 대 한 자세한 내용은 D, 참조는 [JSON 요소 연습](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough) 자습서입니다.

## <a name="other-features"></a>기타 기능

### <a name="pull-to-refresh-support"></a>끌어오기-새로 고침 지원

 *끌어오기-에-* *새로 고침* 시각적 효과 원래에서 발견 되는 *Tweetie2* 여러 응용 프로그램에서 널리 사용 되는 영향을 주지 있으며 응용 프로그램입니다.

하려면 대화 상자에 자동 끌어오기-새로 고침 지원을 추가 하기만 하면 다음 두 가지 작업을 수행 하: 사용자 데이터를 끌어와서 때 알림을 받을 이벤트 처리기를 연결 하 고 알림는 `DialogViewController` 때 데이터가 로드 된 기본 상태로 다시 돌아가십시오.

알림 후크 하는 것은 간단 합니다. 에 연결 하기만 `RefreshRequested` 이벤트에는 `DialogViewController`, 다음과 같이 합니다.

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

어떤 방법을 다음 `OnUserRequestedRefresh`를 큐에 일부 데이터 로드, net에서 일부 데이터를 요청 하거나 데이터를 계산 하는 스레드를 실행 합니다. 알려야 데이터가 로드 되 면는 `DialogViewController` 이 다음에 새 데이터 및 보기를 기본 상태로 복원 하려면이 호출 하 여 수행 하는 `ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>검색 지원

검색을 지원 하도록 설정 된 `EnableSearch` 속성에 사용자 `DialogViewController`합니다. 설정할 수도 있습니다는 `SearchPlaceholder` 검색 창에서 워터 마크 텍스트로 사용 하는 속성입니다.

검색 보기의 내용을 사용자가 변경 됩니다. 표시 되는 필드를 검색 하 고 사용자에 게 하는 것을 보여 줍니다. `DialogViewController` 를 프로그래밍 방식으로 시작, 종료 또는 결과에 새 필터 작업을 트리거하는 세 가지 메서드를 노출 합니다. 이러한 메서드는 다음과 같습니다.

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


시스템을 확장할 수 이므로 하려는 경우이 동작을 변경할 수 있습니다.

### <a name="background-image-loading"></a>배경 이미지 로드

MonoTouch.Dialog 통합는 [TweetStation](https://github.com/migueldeicaza/TweetStation) 응용 프로그램의 이미지 로더가 합니다. 이 이미지 로더가 캐싱을 지원 백그라운드에서 이미지를 로드 사용할 수 있으며 이미지 로드 되었을 때 사용자 코드에 알릴 수 있습니다.

나가는 네트워크 연결 수를 제한할 수도 됩니다.

이미지 로더가에서 구현 되는 `ImageLoader` 하기만 하면 클래스는 호출의 `DefaultRequestImage` 의 인스턴스를 로드 하려는 이미지에 대 한 Uri를 제공 해야 할는 메서드를는 `IImageUpdated` 될 인터페이스 될 때 호출 이미지 ha s 로드 되었습니다.

다음 코드에 Url에서 이미지를 로드 하는 예를 들어 한 `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader 클래스는 현재 메모리에 캐시 되어 있는 이미지를 모두 해제 하려는 경우를 호출할 수 있는 Purge 메서드를 노출 합니다. 현재 코드 50 이미지에 대 한 캐시를 있습니다. 경우 (예를 들어 경우 너무 큰 50 이미지 수 있다고 너무 많은 것으로 이미지를 예상할 수)는 다양 한 캐시 크기를 사용 하려면, 방금 ImageLoader의 인스턴스를 만들 및 캐시에 유지 하려는 이미지의 수를 전달할 수 있습니다.

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ를 사용 하 여 요소 계층을 만들려면

LINQ 및 C#의 초기화 구문의 효과적인 사용을 통해 LINQ 요소 계층을 만들려면 사용할 수 있습니다. 예를 들어 다음 코드에서는 일부 문자열 배열에서 화면을 만듭니다 및 핸들 셀 각로 전달 되는 익명 함수를 통해 선택 `StringElement`:

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

XML 데이터 저장소 또는 데이터에서 거의 완전히 복잡 한 응용 프로그램을 만들 데이터베이스의 데이터와 쉽게 조합 수 없습니다.

## <a name="extending-mtd"></a>산 확장 D

### <a name="creating-custom-elements"></a>사용자 지정 요소 만들기

기존 요소에서 상속 하 여 또는 루트 요소 클래스에서 파생 하 여 사용자가 직접 요소를 만들 수 있습니다.

사용자가 직접 요소를 만들려면 다음 메서드를 재정의 하려는 됩니다.

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

구현 해야 하는 요소는 가변 크기 수 있으면는 `IElementSizing` 인터페이스에 메서드 하나를 포함 합니다.

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

구현 계획 이라면 프로그램 `GetCell` 메서드를 호출 하 여 `base.GetCell(tv)` 도 재정의 해야 반환된 된 셀을 사용자 지정 하는 `CellKey` 요소를 고유 하 게 표시 하는 키를 반환 하는 다음과 같이:

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

대부분의 요소에 대 한 작동는 `StringElement` 및 `StyledStringElement` 다양 한 렌더링 시나리오에 대 한 키의 자체 집합을 사용 하는 것입니다. 이러한 클래스에서 코드를 복제 해야 합니다.

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

반사와 요소 API를 모두 사용 하 여 동일한 `DialogViewController`합니다. 일부 기능을 사용 하려는 경우도 또는 보기의 모양을 사용자 지정할 수도 있습니다는 `UITableViewController` Ui의 기본 작성을 벗어나는입니다.

`DialogViewController` 단순히의 서브 클래스는 `UITableViewController` 사용자 지정 하는 동일한 방식으로 사용자 지정할 수도 있습니다는 `UITableViewController`합니다.

예를 들어, 중 목록 스타일을 변경 하려는 경우 `Grouped` 또는 `Plain`, 다음과 같은 컨트롤러를 만들 때 속성을 변경 하 여이 값을 설정할 수 있습니다.

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

고급 사용자 지정의는 `DialogViewController`, 배경, 설정 등과 같은 서브 클래스 하 고 적절 한 재정의 메서드를 아래 예에 나와 있는 것 처럼:

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

다른 사용자 지정 지점에는 다음과 같은 가상 메서드는는 `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

이 메서드는의 서브 클래스를 반환 해야 `DialogViewController.Source` 경우 여기서 해당 셀 같은 크기, 또는의 서브 클래스에 대 한 `DialogViewController.SizingSource` 셀 일정 하지 않은 경우.

이 재정의 사용 하 여 중 하나를 캡처하는 `UITableViewSource` 메서드. 예를 들어 [TweetStation](https://github.com/migueldeicaza/TweetStation) 사용 하 여이 사용자는 맨 위로 스크롤 하는 시기를 추적 하 고 읽지 않은 트 윗의 수에 따라 업데이트 합니다.

## <a name="validation"></a>유효성 검사

요소를 제공 하지 않고 유효성 검사 자체 웹 페이지에 매우 적합 모델로 데스크톱 응용 프로그램은 iPhone 상호 작용 모델에 직접 매핑되지 않습니다.

데이터 유효성 검사를 수행 하려는 경우 사용자 입력 된 데이터로 작업을 트리거할 때이 수행 해야 합니다. 예를 들어 한 <span class="ui">수행</span> 또는 <span class="ui">다음</span> 단추 맨 위의 도구 모음 또는 일부 `StringElement` 다음 단계로 이동 하는 단추로 사용 합니다.

이것이 기본 입력된 유효성 검사를 수행 하 고 유효성 검사 서버는 사용자/암호 조합의 유효성 검사와 같은 복잡 한 아마도 있는입니다.

오류가 사용자에 게 어떻게는 응용 프로그램입니다. 팝업 수는 `UIAlertView` 나 힌트를 표시 합니다.

## <a name="summary"></a>요약

이 문서는 많은 MonoTouch.Dialog에 대 한 정보를 다룹니다. 방법의 기본 개념을 설명 산 D 작동 하 고 산을 구성 하는 다양 한 구성 요소를 포함 합니다. 4. 다양 한 요소와 산에서 지 원하는 테이블 사용자 지정도 보여 주고 D 방법을 설명 하 고 산 사용자 지정 요소와 D는 확장할 수 있습니다. 또한 산의 JSON 지원을 설명 JSON에서 요소를 동적으로 만들 수 있도록 D입니다.


## <a name="related-links"></a>관련 링크

- [동영상 가이드-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [동영상 가이드-MonoTouch.Dialog와 iOS 사용자 인터페이스를 쉽게 만들](http://youtu.be/j7OC5r8ZkYg)
- [연습: 요소 API를 사용 하 여 응용 프로그램 만들기](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [연습: 리플렉션 API를 사용 하 여 응용 프로그램 만들기](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [연습: JSON 요소를 사용 하 여 사용자 인터페이스 만들기](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON 태그](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
