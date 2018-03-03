---
title: "연습: 리플렉션 API를 사용 하 여 응용 프로그램 만들기"
description: "요소 API MonoTouch.Dialog (산 뿐만 아니라 특성 기반 리플렉션 API를 D)도 포함 됩니다. 리플렉션 API 산 만드는 화면에는 D 클래스에 특성으로도 그만큼 용이 합니다. 이 문서에서는 리플렉션 API를 사용 하 여 응용 프로그램을 만드는 방법을 보여 주는 살펴봅니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 44df6fce4ec6d667c096da01cfc339ec2afdb077
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>연습: 리플렉션 API를 사용 하 여 응용 프로그램 만들기

_요소 API MonoTouch.Dialog (산 뿐만 아니라 특성 기반 리플렉션 API를 D)도 포함 됩니다. 리플렉션 API 산 만드는 화면에는 D 클래스에 특성으로도 그만큼 용이 합니다. 이 문서에서는 리플렉션 API를 사용 하 여 응용 프로그램을 만드는 방법을 보여 주는 살펴봅니다._


산 D 리플렉션 API 되도록 클래스를 통해 해당 산 특성으로 데코레이팅 D 화면을 자동으로 만들기 위해 사용 합니다. 리플렉션 API에는 이러한 클래스와 화면에 표시 되는 내용 간에 바인딩을 제공 합니다. 이 API는 API 요소는 한 세분화 된 제어를 제공 하지 않습니다, 있지만 복잡성 장식 클래스에 따라 요소 계층으로 자동으로 작성 하 여 감소 합니다.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>리플렉션 API 시작

리플렉션 API를 사용 하는 것은 것과 마찬가지로 간단 합니다.

1.  산으로 데코 레이트 된 클래스 만들기 D 특성입니다.
1.  만들기는 `BindingContext` 인스턴스를 위 클래스의 인스턴스를 전달 합니다. 
1.  만들기는 `DialogViewController` , 전달 된 `BindingContext’s` `RootElement` 합니다. 


리플렉션 API를 사용 하는 방법을 설명 하는 예제를 살펴 보겠습니다. 이 예제에서는 아래와 같이 간단한 데이터 입력 화면을 빌드합니다.

 [ ![](reflection-api-walkthrough-images/01-expense-entry.png "이 예제에서는 작성할 것 다음과 같이 간단한 데이터 입력 화면")](reflection-api-walkthrough-images/01-expense-entry.png)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>산을 사용 하 여 클래스 만들기 D 특성

가장 먼저 리플렉션 API를 사용 해야 하는 클래스를 특성으로 데코레이팅입니다. 이러한 특성 산 사용 됩니다. 만들려는 내부적으로 D 요소 API에서 개체입니다. 예를 들어, 다음 클래스 정을 살펴보겠습니다.

```csharp
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
}
```

`SectionAttribute` 의 섹션에서 발생 합니다는 `UITableView` 섹션의 헤더를 채우는 데 사용 되는 문자열 인수를 사용 하 여, 작성 합니다. 섹션 선언 되 면 그 다음에 오는 모든 필드 다른 섹션이 선언 된 될 때까지 해당 섹션에 포함 됩니다.
필드에 대해 만든 사용자 인터페이스 요소 유형의 필드의 형식과 산에 따라 달라 집니다. 데코레이팅하 D 특성입니다.

예를 들어는 `Name` 필드는 한 `string` 으로 데코레이팅 되었으므로 및는 `EntryAttribute`합니다. 이 인해 텍스트 입력 필드와 지정한 캡션을 포함 하는 테이블에 추가 되 고 행. 마찬가지로,는 `IsApproved` 필드는 한 `bool` 와 `CheckboxAttribute`, 테이블 셀의 오른쪽에 있는 확인란와 테이블 행의 결과입니다. 산 D 필드 이름으로 사용 된 공간을 자동으로 추가 특성에 지정 하지 않으면 하므로 경우 캡션을 만드는 데 합니다.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>BindingContext 추가

사용 하 여 `Expense` 클래스를 만들어야 한다는 `BindingContext`합니다. A `BindingContext` 바인딩되는 데이터 요소의 계층을 만들려면 특성 사용된 클래스에서 클래스입니다. 하나, 우리 단순히, 인스턴스화할를 만든 특성 사용된 클래스의 인스턴스를 생성자에 전달 합니다.

예를 들어, UI를 사용 하 여 선언 했으므로 추가 하려면 특성에 `Expense` 클래스에 다음 코드를 포함 합니다는 `FinishedLaunching` 의 메서드는 `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

UI를 만들기 위해 수행 해야이 추가 `BindingContext` 에 `DialogViewController` 으로 설정 하는 `RootViewController` 아래와 같이 창:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

표시 되 고 위에 표시 된 화면으로 인해 응용 프로그램을 지금 실행 됩니다.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>UINavigationController 추가

그러나는 제목이 "만들기" 작업에 전달 우리는 `BindingContext` 표시 되지 않습니다. 때문에 이것이 `DialogViewController` 은의 일부가 아닌는 `UINavigatonController`합니다. 추가 하는 코드를 변경해 보겠습니다는 `UINavigationController` 창으로 `RootViewController,` 추가 `DialogViewController` 의 루트로 `UINavigationController` 아래와 같이:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

에 제목이 표시 응용 프로그램을 실행 하면 이제는 `UINavigationController’s` 다음 스크린 샷은로 탐색 모음:

 [ ![](reflection-api-walkthrough-images/02-create-task.png "이제 응용 프로그램을 실행 하면에 제목이 나타납니다 UINavigationControllers 탐색 모음")](reflection-api-walkthrough-images/02-create-task.png)

포함 하 여 한 `UINavigationController`, 이제 산의 다른 기능을 수행할 수 있습니다 D 탐색이 필요 합니다. 예를 들어 하는 열거형을 추가할 수 있습니다는 `Expense` 비용과 산에 대 한 범주를 정의 하는 클래스 D 선택 화면이 자동으로 만듭니다. 을 설명 하기 위해 수정 된 `Expense` 포함 하도록 클래스는 `ExpenseCategory` 다음과 같이 필드:

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

응용 프로그램을 지금 실행 결과 범주에 대 한 테이블에 새 행 표시 된 것 처럼 됩니다.

 [ ![](reflection-api-walkthrough-images/03-set-details.png "표시 된 것 처럼 범주에 대 한 테이블에 새 행으로 인해 응용 프로그램을 지금 실행")](reflection-api-walkthrough-images/03-set-details.png)

행을 선택 하는 응용 프로그램 화면 이동 하는 새 행에 해당 하는 열거형과 함께 아래와 같이 발생 합니다.

 [ ![](reflection-api-walkthrough-images/04-set-category.png "열거형에 해당 하는 행이 포함 된 새로운 화면으로 응용 프로그램 이동으로 인해 행을 선택")](reflection-api-walkthrough-images/04-set-category.png)

 <a name="Summary" />


## <a name="summary"></a>요약

이 문서는 리플렉션 API의 연습을 제공합니다. 표시 되는 항목을 제어 하는 클래스에 특성을 추가 하는 방법을 살펴보았습니다. 또한 사용 하는 방법을 설명한는 `BindingContext` 산 사용 하는 방법 뿐만 아니라 생성 된 요소 계층에는 클래스에서 데이터를 바인딩할 와 D는 `UINavigationController`합니다.


## <a name="related-links"></a>관련 링크

- [MTDReflectionWalkthrough (샘플)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [동영상 가이드-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [동영상 가이드-MonoTouch.Dialog와 iOS 사용자 인터페이스를 쉽게 만들](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch 대화 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
