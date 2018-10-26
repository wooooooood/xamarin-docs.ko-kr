---
title: 리플렉션 API를 사용 하 여 Xamarin.iOS 응용 프로그램 만들기
description: 이 문서는 MonoTouch.Dialog 특성 기반 리플렉션 API에 설명 특성으로 데코 레이트 하는 클래스를 기반으로 하는 UI를 만듭니다.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.openlocfilehash: bbc49e5f830a8711e5ead90fe02113e654d5da93
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107783"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>리플렉션 API를 사용 하 여 Xamarin.iOS 응용 프로그램 만들기

Mt를 D 리플렉션 API 클래스를 사용 하면 해당 산 특성으로 데코레이팅 D 화면을 자동으로 만들려면 사용 합니다. 리플렉션 API는 이러한 클래스 및 화면에 표시 되는 항목 간의 바인딩을 제공 합니다. 이 API는 API 요소는 세분화 된 제어를 제공 하지 않습니다, 하지만 자동으로 클래스 장식을 기반으로 하는 요소 계층 구조를 구축 하 여 복잡성을 감소 합니다.

## <a name="setting-up-mtd"></a>산 설정 D

산 D는 Xamarin.iOS를 사용 하 여 배포 됩니다. 를 사용 하려면 마우스 오른쪽 단추로 클릭 합니다 **참조** Xamarin.iOS 노드의 Mac 용 Visual Studio 2017 또는 Visual Studio에서 프로젝트 및 참조를 추가 합니다 **MonoTouch.Dialog 1** 어셈블리. 그런 다음 추가 `using MonoTouch.Dialog` 필요에 따라 원본에서 문을 코딩 합니다.

## <a name="getting-started-with-the-reflection-api"></a>리플렉션 API 시작

리플렉션 API를 사용 하는 것 만큼 간단 합니다.

1.  산을 사용 하 여 데코 레이트 된 클래스 만들기 차원 특성입니다.
1.  만들기는 `BindingContext` 인스턴스를 위 클래스의 인스턴스를 전달 합니다. 
1.  만들기는 `DialogViewController` 에 전달 합니다 `BindingContext’s` `RootElement` 합니다. 


리플렉션 API를 사용 하는 방법을 설명 하는 예제를 살펴보겠습니다. 이 예제에서는 아래와 같이 간단한 데이터 입력 화면을 빌드 해 보겠습니다.

 [![](reflection-api-walkthrough-images/01-expense-entry.png "이 예제에서는 빌드 해 보겠습니다 다음과 같이 간단한 데이터 입력 화면")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>산을 사용 하 여 클래스 만들기 차원 특성

먼저 리플렉션 API를 사용 하는 특성으로 데코레이팅된 클래스입니다. 이러한 특성 산 사용 됩니다. 요소 API에서 만들려는 내부적으로 차원 개체입니다. 예를 들어, 다음 클래스 정을 것이 좋습니다.

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

`SectionAttribute` 부분에 하면를 `UITableView` 섹션의 헤더를 채우는 데 사용 되는 문자열 인수를 사용 하 여, 작성 합니다. 섹션을 선언 하 고, 다른 섹션은 선언 될 때까지 오는 모든 필드 섹션에 포함 됩니다.
필드에 대해 만든 사용자 인터페이스 요소의 형식 필드의 형식과 MT를 따라 달라 집니다. 지정 하는 차원 특성입니다.

예를 들어를 `Name` 필드를 `string` 으로 데코 레이트 된 및는 `EntryAttribute`합니다. 이 인해 텍스트 입력 필드와 지정한 캡션이 포함 된 테이블에 추가할 행입니다. 마찬가지로, 합니다 `IsApproved` 필드는를 `bool` 사용 하 여를 `CheckboxAttribute`테이블 셀의 오른쪽에서 확인란을 사용 하 여 테이블 행의 결과, 합니다. 산 D에서는 필드 이름으로 자동으로 공백을 추가 특성에 지정 되지 않은 있으므로 예제의 경우 캡션을 만드는 데 합니다.

## <a name="adding-the-bindingcontext"></a>BindingContext를 추가합니다.

사용 하 여 `Expense` 클래스를 생성 해야를 `BindingContext`합니다. `BindingContext` 요소의 계층을 만들려면 특성 사용된 클래스에서 데이터를 바인딩하는 클래스입니다. 하나를 만들려면 간단히 인스턴스화할 하 고 특성 사용된 클래스의 인스턴스를 생성자에 전달 합니다.

예를 들어, UI를 사용 하 여 선언 된 것을 추가 하려면 특성을 `Expense` 클래스에 다음 코드를 포함 합니다는 `FinishedLaunching` 메서드를 `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

추가 UI를 만드는 작업을 수행 해야 하는 모든는 `BindingContext` 에 `DialogViewController` 으로 설정 하는 `RootViewController` 아래와 같이 창:

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

표시 되 고 위에 표시 된 화면에서 결과 이제 응용 프로그램을 실행 합니다.

### <a name="adding-a-uinavigationcontroller"></a>UINavigationController 추가

그러나는 제목을 "만들기" 작업에 전달 하는 것을 `BindingContext` 표시 되지 않습니다. 때문에 이것이 합니다 `DialogViewController` 은의 일부가 아닌를 `UINavigatonController`. 추가 하는 코드를 변경해 보겠습니다를 `UINavigationController` 창으로 `RootViewController,` 추가 된 `DialogViewController` 의 루트로 `UINavigationController` 아래와 같이:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

제목에 표시 되는 응용 프로그램을 실행 하면 이제는 `UINavigationController’s` 탐색 모음으로 보여 주는 스크린샷입니다.

 [![](reflection-api-walkthrough-images/02-create-task.png "이제 응용 프로그램을 실행 하면 제목 표시줄에 나타납니다 UINavigationControllers 탐색")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

포함 하 여를 `UINavigationController`, 우리가 지금 활용할 수 산의 다른 기능 D 탐색이 필요 합니다. 예를 들어 하는 열거형을 추가할 수 있습니다는 `Expense` 비용과 산에 대 한 범주를 정의 하는 클래스 D 선택 화면을 자동으로 만듭니다 됩니다. 을 설명 하기 위해 수정 합니다 `Expense` 를 포함 하도록 클래스는 `ExpenseCategory` 다음과 같이 필드:

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

이제 응용 프로그램을 실행 결과의 범주에 대 한 테이블에 새 행을 표시 된 것 처럼 됩니다.

 [![](reflection-api-walkthrough-images/03-set-details.png "표시 된 것 처럼 범주에 대 한 테이블에 새 행을 결과 이제 응용 프로그램 실행")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

행을 선택 하는 응용 프로그램 화면을 이동 새 행에 해당 하는 열거형을 사용 하 여 아래와 같이 발생 합니다.

 [![](reflection-api-walkthrough-images/04-set-category.png "행을 선택 하면 행에 해당 하는 열거형을 사용 하 여 새로운 화면으로 응용 프로그램 탐색")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>요약

이 문서에서는 리플렉션 API의 연습을 제공 합니다. 표시 되는 항목을 제어 하는 클래스에 특성을 추가 하는 방법을 살펴보았습니다. 또한 사용 하는 방법을 설명한는 `BindingContext` 바인딩할 데이터 클래스에서 사용 하 여 산 방법 뿐만 아니라 생성 되는 요소 계층 구조 사용 하 여 D는 `UINavigationController`합니다.


## <a name="related-links"></a>관련 링크

- [MTDReflectionWalkthrough (샘플)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [스크린 캐스트-Miguel de Icaza는 MonoTouch.Dialog를 사용 하 여는 iOS 로그인 화면을 만듭니다.](http://youtu.be/3butqB1EG0c)
- [스크린 캐스트-MonoTouch.Dialog를 사용 하 여 iOS 사용자 인터페이스를 쉽게 만들기](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch 대화 소개](~/ios/user-interface/monotouch.dialog/index.md)
- [요소 API 연습](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 요소 연습](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github에서 MonoTouch 대화 상자](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation 응용 프로그램](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController 클래스 참조](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController 클래스 참조](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
