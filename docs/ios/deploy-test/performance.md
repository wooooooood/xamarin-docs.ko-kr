---
title: Xamarin.iOS 성능
description: 이 문서에서는 Xamarin.iOS 응용 프로그램에서 성능 및 메모리 사용을 개선하기 위해 사용할 수 있는 기술을 설명합니다.
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 01/29/2016
ms.openlocfilehash: afff9d3924c673edc363292efa1a9b7df43a9218
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinios-performance"></a>Xamarin.iOS 성능

낮은 응용 프로그램 성능은 여러 가지 방법으로 나타납니다. 이 경우에 응용 프로그램이 응답하지 않는 것처럼 보이고, 스크롤 속도가 느려지고, 배터리 수명이 줄어들 수 있습니다. 그러나 성능을 최적화하려면 효율적인 코드를 구현하는 것 이상이 필요합니다. 응용 프로그램 성능에 대한 사용자 환경도 고려해야 합니다. 예를 들어 사용자가 다른 활동을 수행하지 못하도록 차단하지 않고 작업을 실행하면 사용자 환경을 향상시키는 데 도움이 될 수 있습니다. 

이 문서에서는 Xamarin.iOS 응용 프로그램에서 성능 및 메모리 사용을 개선하기 위해 사용할 수 있는 기술을 설명합니다.

> [!NOTE]
> 이 문서를 읽기 전에 먼저 Xamarin 플랫폼을 사용하여 빌드된 응용 프로그램의 메모리 사용 및 성능을 향상시키기 위한 비플랫폼 특정 기술에 대해 설명하는 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)을 참조해야 합니다.

## <a name="avoid-strong-circular-references"></a>강력한 순환 참조 방지

상황에 따라 개체에서 가비지 수집기를 통해 메모리를 회수하지 못하도록 방지할 수 있는 강력한 참조 순환을 만들 수도 있습니다. 예를 들어 [`UIView`](https://developer.xamarin.com/api/type/UIKit.UIView/)에서 상속되는 클래스와 같은 [`NSObject`](https://developer.xamarin.com/api/type/Foundation.NSObject/) 파생 하위 클래스가 `NSObject` 파생 컨테이너에 추가되고, 다음 코드 예제와 같이 Objective-C에서 강력하게 참조되는 경우를 살펴보겠습니다.

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

이 코드에서 `Container` 인스턴스를 만들면 C# 개체가 Objective-C 개체에 대한 강력한 참조를 갖게 됩니다. 마찬가지로, `MyView` 인스턴스도 Objective-C 개체에 대한 강력한 참조를 갖게 됩니다.

또한 `container.AddSubview`를 호출하면 관리되지 않는 `MyView` 인스턴스의 참조 횟수도 증가합니다. 이 경우 관리 개체가 해당 개체에 대한 참조를 유지한다는 보장이 없기 때문에 Xamarin.iOS 런타임은 `MyView` 개체를 관리 코드에서 활성 상태로 유지하는 `GCHandle` 인스턴스를 만듭니다. 관리 코드 관점에서 [`AddSubview`](https://developer.xamarin.com/api/member/UIKit.UIView.AddSubview/p/UIKit.UIView/) 호출이 `GCHandle`에 대한 호출이 아닌 경우 `MyView` 개체가 회수됩니다.

관리되지 않는 `MyView` 개체에는 *강력한 링크*로 알려진 관리 개체를 가리키는 `GCHandle`이 있습니다. 관리 개체에는 `Container` 인스턴스에 대한 참조가 포함됩니다. 이에 따라 `Container` 인스턴스는 `MyView` 개체에 대한 관리 참조를 갖게 됩니다.

포함된 개체가 해당 컨테이너에 대한 링크를 유지하는 경우에는 순환 참조를 처리할 수 있는 몇 가지 옵션이 있습니다.

-  컨테이너에 대한 링크를 `null`로 설정하여 순환을 수동으로 끊습니다.
-  컨테이너에서 포함된 개체를 수동으로 제거합니다.
-  개체에 대해 `Dispose`를 호출합니다.
-  컨테이너에 대한 약한 참조를 유지하는 순환 참조를 방지합니다. 약한 참조에 대한 자세한 내용은 다음 섹션을 참조하세요.

### <a name="using-weakreferences"></a>WeakReferences 사용

순환을 방지하기 위해 한 가지 방법은 자식에서 부모까지 약한 참조를 사용하는 것입니다. 예를 들어 위의 코드는 다음과 같이 작성할 수 있습니다.

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

여기에서 포함된 개체는 부모를 활성 상태로 유지합니다. 그러나 부모는 `container.AddSubView`에 수행한 호출을 통해 자식을 활성 상태로 유지합니다.

또한 이는 대리자 또는 데이터 원본 패턴을 사용하는 iOS API에서도 발생합니다. 예를 들어 [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/) 클래스에서 [`Delegate`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.Delegate/) 속성 또는 [`DataSource`](https://developer.xamarin.com/api/property/MonoTouch.UIKit.UITableView.DataSource/)를 설정할 때 피어 클래스에 구현이 포함됩니다.

하위 클래스를 만드는 대신 수행할 수 있는 작업인 [`IUITableViewDataSource`](https://developer.xamarin.com/api/type/MonoTouch.UIKit.IUITableViewDataSource/)와 같이 프로토콜을 구현하기 위해 순전히 만들어진 클래스의 경우, 클래스에서 인터페이스를 구현하고, 메서드를 재정의하고, `DataSource` 속성을 `this`에 할당할 수 있습니다.

#### <a name="weak-attribute"></a>약한 특성

[Xamarin.iOS 11.10](https://developer.xamarin.com/releases/ios/xamarin.ios_11/xamarin.ios_11.10/#WeakAttribute)에서는 `[Weak]` 특성이 도입되었습니다. `WeakReference <T>`과 마찬가지로 더 적은 코드로 [강력한 순환 참조](https://docs.microsoft.com/en-us/xamarin/ios/deploy-test/performance#avoid-strong-circular-references)를 중단하기 위해 `[Weak]`를 사용할 수 있습니다.

`WeakReference <T>`를 사용하는 다음 코드를 살펴봅니다.

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

`[Weak]`를 사용하는 해당 코드는 훨씬 더 간결합니다.

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

다음은 [위임](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html) 패턴의 컨텍스트에서 `[Weak]`를 사용하는 또 다른 예제입니다.

```csharp
public class MyViewController : UIViewController 
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate 
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>강력한 참조가 있는 개체 삭제

강력한 참조가 있고 종속성을 제거하기 어려우면 `Dispose` 메서드에서 부모 포인터를 제거합니다.

컨테이너의 경우 포함된 개체를 제거하려면 다음 코드 예제와 같이 `Dispose` 메서드를 재정의합니다.

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

부모에 대한 강력한 참조를 유지하는 자식 개체의 경우 `Dispose` 구현에서 부모에 대한 참조를 제거합니다.

```csharp
class MyChild : UIView 
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

강력한 참조를 해제하는 방법에 대한 자세한 내용은 [IDisposable 리소스 해제](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable)를 참조하세요.
또한 [Xamarin.iOS, 가비지 수집기 및 나](http://krumelur.me/2015/04/27/xamarin-ios-the-garbage-collector-and-me/) 블로그 게시물에도 훌륭한 토론이 있습니다.

### <a name="more-information"></a>추가 정보

자세한 내용은 [순환 유지를 방지하는 규칙](http://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html)(Cocoa With Love 제공), [MonoTouch GC에 있는 버그인가요?](http://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc)(StackOverflow 제공) 및 [MonoTouch GC에서 refcount가 1보다 큰 관리 개체를 종료할 수 없는 이유는 무엇인가요?](http://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1)(StackOverflow 제공)를 참조하세요

## <a name="optimize-table-views"></a>테이블 보기 최적화

사용자에게는 [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/) 인스턴스에 대한 부드러운 스크롤 및 빠른 로드 시간이 필요합니다. 그러나 셀에 깊게 중첩된 뷰 계층 구조 또는 복잡한 레이아웃이 포함되어 있으면 스크롤 성능이 저하될 수 있습니다. 그러나 `UITableView` 성능이 저하되지 않도록 방지하는 데 사용할 수 있는 방법이 있습니다.

- 셀을 다시 사용합니다. 자세한 내용은 [셀 재사용](#reusecells)을 참조하세요.
- 하위 뷰의 수를 줄입니다.
- 웹 서비스에서 검색된 셀 내용을 캐시합니다.
- 동일하지 않은 경우 모든 행의 높이를 캐시합니다.
- 셀 및 다른 뷰를 불투명하게 만듭니다.
- 이미지 크기 조정 및 그라데이션을 사용하지 않습니다.

이러한 기술은 전체적으로 [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/) 인스턴스 스크롤을 부드럽게 유지하는 데 도움이 될 수 있습니다.

### <a name="reuse-cells"></a>셀 다시 사용

[`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)에 수백 개의 행을 표시하는 경우 작은 수의 [`UITableViewCell`](https://developer.xamarin.com/api/type/UIKit.UITableViewCell/) 개체만 한 번에 화면에 표시할 때 이 개체를 수백 개 만드는 것은 메모리 낭비입니다. 대신, 화면에 표시되는 셀만 메모리에 로드될 수 있으며, 이 경우 **콘텐츠**가 다시 사용된 셀에 로드됩니다. 이렇게 하면 수백 개의 추가 개체를 인스턴스화하지 않으므로 시간과 메모리가 절약됩니다.

따라서 셀이 화면에서 사라지면 다음 코드 예제와 같이 뷰를 큐에 배치하여 다시 사용할 수 있습니다.

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

사용자가 스크롤하면 새 뷰가 표시되도록 요청하기 위해 [`UITableView`](https://developer.xamarin.com/api/type/UIKit.UITableView/)에서 `GetCell` 재정의를 호출합니다. 그런 다음, 이 재정의는 [`DequeueReusableCell`](https://developer.xamarin.com/api/member/UIKit.UITableView.DequeueReusableCell/p/Foundation.NSString/) 메서드를 호출하고, 셀을 다시 사용할 수 있으면 해당 셀을 반환합니다.

자세한 내용은 [데이터로 테이블 채우기](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)의 [셀 재사용](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)을 참조하세요.

## <a name="use-opaque-views"></a>불투명 보기 사용

투명도가 정의되지 않은 뷰에는 [`Opaque`](https://developer.xamarin.com/api/property/UIKit.UIView.Opaque/) 속성 집합이 있어야 합니다. 이렇게 하면 그리기 시스템에서 뷰가 최적으로 렌더링됩니다. 뷰가 [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/)에 포함되거나 복잡한 애니메이션의 일부일 때 특히 중요합니다. 그렇지 않으면 그리기 시스템에서 다른 콘텐츠와 뷰를 합성하여 성능에 큰 영향을 줄 수 있습니다.

## <a name="avoid-fat-xibs"></a>FAT XIB 방지

XIB가 대부분 스토리보드로 대체되었지만, XIB를 여전히 사용할 수 있는 몇 가지 상황이 있습니다. XIB가 메모리에 로드되면 이미지를 포함하여 모든 콘텐츠가 메모리에 로드됩니다. XIB에 즉시 사용되지 않는 뷰가 포함되어 있으면 메모리가 낭비됩니다. 따라서 XIB를 사용하는 경우 뷰 컨트롤러당 하나의 XIB만 있고, 가능하면 뷰 컨트롤러의 뷰 계층 구조를 별도의 XIB로 분리합니다.

## <a name="optimize-image-resources"></a>이미지 리소스 최적화

이미지는 응용 프로그램에서 사용하는 가장 비싼 리소스 중 일부이며, 고해상도로 캡처되는 경우가 많습니다. 따라서 앱의 번들 이미지를 [`UIImageView`](https://developer.xamarin.com/api/type/UIKit.UIImageView/)에 표시하는 경우 이미지와 `UIImageView`의 크기가 동일해야 합니다. 런타임에서 이미지 크기를 조정하는 경우, 특히 `UIImageView`가 [`UIScrollView`](https://developer.xamarin.com/api/type/UIKit.UIScrollView/)에 포함된 경우 비용이 많이 드는 작업이 될 수 있습니다.

자세한 내용은 [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md) 가이드의 [이미지 리소스 최적화](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)를 참조하세요.

## <a name="test-on-devices"></a>장치 테스트

가능한 한 빨리 물리적 장치에서 응용 프로그램 배포 및 테스트를 시작합니다. 시뮬레이터는 장치의 동작과 제한을 완벽하게 일치시키지 않으므로 가능한 한 빨리 물리적 장치 시나리오에서 테스트해야 중요합니다.

특히 시뮬레이터는 어떠한 방식으로든 물리적 장치의 메모리 또는 CPU 제한을 시뮬레이션하지 않습니다.

## <a name="synchronize-animations-with-the-display-refresh"></a>디스플레이 새로 고침과 애니메이션 동기화

게임에는 게임 논리를 실행하고 화면을 업데이트하기 위해 긴밀한 루프가 사용되는 경향이 있습니다. 일반적인 프레임 속도의 범위는 초당 30-60회 프레임입니다. 일부 개발자는 게임 시뮬레이션을 화면 업데이트와 결합하여 가능한 한 많은 초당 횟수로 화면을 업데이트해야 하고, 초당 60회 프레임을 초과하도록 유혹받을 수 있다고 느낍니다.

그러나 디스플레이 서버는 초당 60회 상한에서 화면 업데이트를 수행합니다. 따라서 이 제한보다 더 빠르게 화면을 업데이트하려고 하면 화면이 찢기거나 미세한 끊김 현상이 발생할 수 있습니다. 화면 업데이트가 디스플레이 업데이트와 동기화되도록 코드를 구조화하는 것이 가장 좋습니다. 이 작업은 시각화에 적합한 타이머인 [`CoreAnimation.CADisplayLink`](https://developer.xamarin.com/api/type/CoreAnimation.CADisplayLink/) 클래스와 초당 60회 프레임으로 실행되는 게임을 사용하여 수행할 수 있습니다.

## <a name="avoid-core-animation-transparency"></a>코어 애니메이션 투명도 방지

코어 애니메이션 투명도를 사용하지 않으면 비트맵 합성 성능이 향상됩니다. 일반적으로 가능한 한 투명한 레이어 및 흐리게 표시되는 테두리를 사용하지 않도록 방지합니다.

## <a name="avoid-code-generation"></a>코드 생성 방지

iOS 커널에서 동적 코드를 실행하지 못하도록 방지하므로 `System.Reflection.Emit` 또는 *동적 언어 런타임*을 사용하여 코드를 동적으로 생성해야 합니다.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.iOS로 빌드된 응용 프로그램의 성능을 높이는 기술에 대해 설명했습니다. 이러한 기술은 전체적으로 CPU에서 수행하는 작업의 양과 응용 프로그램에서 소비하는 메모리의 양을 크게 줄일 수 있습니다.

## <a name="related-links"></a>관련 링크

- [플랫폼 간 성능](~/cross-platform/deploy-test/memory-perf-best-practices.md)
