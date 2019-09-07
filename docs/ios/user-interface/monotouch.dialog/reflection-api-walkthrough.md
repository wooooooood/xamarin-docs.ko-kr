---
title: 리플렉션 API를 사용 하 여 Xamarin.ios 응용 프로그램 만들기
description: 이 문서에서는 특성을 사용 하 여 데코레이팅된 클래스를 기반으로 UI를 만드는 Monotouch.dialog 특성 기반 리플렉션 API에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: conceptdev
ms.author: crdun
ms.openlocfilehash: 7acd43597d033b4c6daac59016a9bdf41ade6f68
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768137"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>리플렉션 API를 사용 하 여 Xamarin.ios 응용 프로그램 만들기

MT입니다. D 리플렉션 API를 사용 하면 클래스를 MT 인 특성으로 데코레이팅 할 수 있습니다. D는를 사용 하 여 화면을 자동으로 만듭니다. 리플렉션 API는 이러한 클래스와 화면에 표시 되는 항목 간의 바인딩을 제공 합니다. 이 API는 요소 API에서 사용 하는 세분화 된 제어를 제공 하지 않지만 클래스 장식을 기반으로 요소 계층 구조를 자동으로 작성 하 여 복잡성을 줄입니다.

## <a name="setting-up-mtd"></a>MT를 설정 합니다. 2

휴먼. D는 Xamarin.ios를 사용 하 여 배포 됩니다. 이를 사용 하려면 Visual Studio 2017 또는 Mac용 Visual Studio에서 Xamarin.ios 프로젝트의 **참조** 노드를 마우스 오른쪽 단추로 클릭 하 고 **monotouch.dialog** 어셈블리에 대 한 참조를 추가 합니다. 그런 다음 필요 `using MonoTouch.Dialog` 에 따라 소스 코드에서 문을 추가 합니다.

## <a name="getting-started-with-the-reflection-api"></a>리플렉션 API 시작

리플렉션 API를 사용 하는 것은 다음과 같이 간단 합니다.

1. MT로 데코레이팅된 클래스 만들기 D 특성.
1. 인스턴스를 `BindingContext` 만들고 위의 클래스의 인스턴스를 전달 합니다. 
1. 를 만들고를 전달 `BindingContext’s` `RootElement` 합니다. `DialogViewController` 

리플렉션 API를 사용 하는 방법을 보여 주는 예를 살펴보겠습니다. 이 예에서는 아래와 같이 간단한 데이터 입력 화면을 작성 합니다.

 [![](reflection-api-walkthrough-images/01-expense-entry.png "이 예제에서는 다음과 같이 간단한 데이터 입력 화면을 작성 합니다.")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>MT를 사용 하 여 클래스 만들기 D 특성

리플렉션 API를 사용 해야 하는 첫 번째 작업은 특성을 사용 하 여 데코레이팅된 클래스입니다. 이러한 특성은 MT에서 사용 됩니다. D는 내부적으로 요소 API에서 개체를 만듭니다. 예를 들어 다음과 같은 클래스 정의를 살펴보겠습니다.

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

는 `SectionAttribute` 섹션의 헤더를 채우는 데 `UITableView` 사용 되는 문자열 인수를 사용 하 여가 생성 되는 섹션을 생성 합니다. 섹션이 선언 되 면 다른 섹션이 선언 될 때까지 뒤에 오는 모든 필드가 해당 섹션에 포함 됩니다.
필드에 대해 생성 되는 사용자 인터페이스 요소의 형식은 필드의 형식과 MT에 따라 달라 집니다. D 특성 데코레이팅

예 `Name` 를 들어 필드 `string` 는 이며로 `EntryAttribute`데코 레이트 됩니다. 그러면 텍스트 입력 필드와 지정 된 캡션이 있는 행이 테이블에 추가 됩니다. 마찬가지로 필드는 `IsApproved` 를 `bool` 포함 `CheckboxAttribute`하는입니다. 그러면 테이블 셀의 오른쪽에 확인란을 포함 하는 테이블 행이 생성 됩니다. 휴먼. D는 특성에 지정 되지 않았으므로 필드 이름을 사용 하 여 자동으로 공백을 추가 하 고이 경우에 캡션을 만듭니다.

## <a name="adding-the-bindingcontext"></a>BindingContext 추가

`Expense` 클래스를 사용 하려면을 `BindingContext`만들어야 합니다. 는 `BindingContext` 특성을 사용 하는 클래스의 데이터를 바인딩하여 요소의 계층 구조를 만드는 클래스입니다. 이를 만들려면 단순히 인스턴스화하고 특성 사용 클래스의 인스턴스를 생성자에 전달 합니다.

예를 들어 `Expense` 클래스에서 특성을 사용 하 여 선언한 UI를 추가 하려면의 `FinishedLaunching` `AppDelegate`메서드에 다음 코드를 포함 합니다.

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

그런 다음 UI를 만들려면에를 추가 `BindingContext` `DialogViewController` 하 `RootViewController` 고 아래와 같이 창의로 설정 하기만 하면 됩니다.

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
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

이제 응용 프로그램을 실행 하면 위에 표시 된 화면이 표시 됩니다.

### <a name="adding-a-uinavigationcontroller"></a>UINavigationController 추가

그러나에 전달 `BindingContext` 된 "작업 만들기" 제목은 표시 되지 않습니다. `DialogViewController` 가`UINavigatonController`의 일부가 아니기 때문입니다. 를 `UINavigationController` `DialogViewController` 창으로`UINavigationController` 추가 하는 코드를 변경 하 고아래와같이을의루트로추가해보겠습니다.`RootViewController,`

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

이제 응용 프로그램을 실행 하면 아래 스크린샷에 표시 된 대로 제목이 `UINavigationController’s` 탐색 모음에 표시 됩니다.

 [![](reflection-api-walkthrough-images/02-create-task.png "이제 응용 프로그램을 실행할 때 제목이 UINavigationControllers 탐색 모음에 표시 됩니다.")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

이제를 포함 `UINavigationController`하 여 MT의 다른 기능을 활용할 수 있습니다. 해당 탐색이 필요한 D입니다. 예를 들어, `Expense` 클래스에 열거형을 추가 하 여 경비 및 MT의 범주를 정의할 수 있습니다. D는 자동으로 선택 화면을 만듭니다. 이를 보여 주기 위해 `Expense` 다음과 같이 `ExpenseCategory` 필드를 포함 하도록 클래스를 수정 합니다.

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

이제 응용 프로그램을 실행 하면 다음과 같이 범주에 대 한 테이블에 새 행이 생성 됩니다.

 [![](reflection-api-walkthrough-images/03-set-details.png "이제 응용 프로그램을 실행 하면 다음과 같이 범주에 대 한 테이블에 새 행이 생성 됩니다.")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

행을 선택 하면 응용 프로그램에서 아래와 같이 열거형에 해당 하는 행이 있는 새 화면으로 이동 합니다.

 [![](reflection-api-walkthrough-images/04-set-category.png "행을 선택 하면 응용 프로그램에서 열거형에 해당 하는 행이 있는 새 화면으로 이동 합니다.")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 리플렉션 API에 대 한 연습을 제공 했습니다. 클래스에 특성을 추가 하 여 표시 되는 항목을 제어 하는 방법을 살펴보았습니다. 또한를 `BindingContext` 사용 하 여 클래스의 데이터를 만들어진 요소 계층 구조와 MT를 사용 하는 방법에 바인딩하는 방법에 대해 설명 했습니다. D가 있는 `UINavigationController`D입니다.

## <a name="related-links"></a>관련 링크

- [MTDReflectionWalkthrough (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Monotouch.dialog 대화 상자 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github의 Monotouch.dialog 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
