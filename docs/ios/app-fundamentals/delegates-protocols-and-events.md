---
title: "이벤트, 프로토콜 및 대리자"
description: "이 문서에서는 콜백을 받으려면 하 고 사용자 인터페이스 컨트롤을 데이터로 채우는 데 사용 되는 키 iOS 기술을 제공 합니다. 이러한 기술에는 이벤트, 프로토콜 및 대리자입니다. 이 문서에 대해 설명 이러한 각 이며, 각 어떻게 사용 되는지 C#에서. Xamarin.iOS 이벤트를 노출할 친숙 한.NET도 Xamarin.iOS 프로토콜, 대리자 등 Objective-c 개념에 대 한 지원을 제공 하는 방법에 따라 iOS 컨트롤을 사용 하는 방법을 보여 줍니다 (Objective-c 대리자 혼동 해서는 안 C# 대리자와 함께). 또한이 문서는 프로토콜을 사용 – 모두 Objective-c 대리자에 대 한 비 대리자 시나리오에서의 기초로 방식을 보여 주는 예제를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 69296992c503d536a4160f172022c7ce5578812f
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="events-protocols-and-delegates"></a>이벤트, 프로토콜 및 대리자

_이 문서에서는 콜백을 받으려면 하 고 사용자 인터페이스 컨트롤을 데이터로 채우는 데 사용 되는 키 iOS 기술을 제공 합니다. 이러한 기술에는 이벤트, 프로토콜 및 대리자입니다. 이 문서에 대해 설명 이러한 각 이며, 각 어떻게 사용 되는지 C#에서. Xamarin.iOS 이벤트를 노출할 친숙 한.NET도 Xamarin.iOS 프로토콜, 대리자 등 Objective-c 개념에 대 한 지원을 제공 하는 방법에 따라 iOS 컨트롤을 사용 하는 방법을 보여 줍니다 (Objective-c 대리자 혼동 해서는 안 C# 대리자와 함께). 또한이 문서는 프로토콜을 사용 – 모두 Objective-c 대리자에 대 한 비 대리자 시나리오에서의 기초로 방식을 보여 주는 예제를 제공 합니다._

Xamarin.iOS 대부분 사용자 상호 작용에 대 한 이벤트를 노출 하도록 컨트롤을 사용 합니다.
Xamarin.iOS 응용 프로그램으로 기존.NET 응용 프로그램 거의 동일한 방법으로 이러한 이벤트를 사용 합니다. 예를 들어 Xamarin.iOS UIButton 클래스 TouchUpInside 라는 이벤트가 있습니다 하 고 하면.NET 응용 프로그램에서이 클래스와 이벤트 것 처럼이 이벤트를 사용 합니다.

이.NET 방법 외에도 Xamarin.iOS 더 복잡 한 상호 작용와 데이터 바인딩을 사용할 수 있는 다른 모델을 노출 합니다. 이 방법으로 Apple 위임과 프로토콜 호출 사용 합니다. 대리자에서 C#의 경우 대리자에 개념이 유사 하지만 Objective C의 대리자는 프로토콜을 따르는 전체 클래스 정의 단일 메서드를 호출 하는 대신 합니다. 해당 메서드를 선택적 될 수 있다는 점이 프로토콜은 C#에서 인터페이스와 유사 합니다. 예를 들어 데이터로 UITableView를 채우려면이 자체를 채우는 데는 UITableView을 호출 하는 UITableViewDataSource 프로토콜에 정의 된 메서드를 구현 하는 대리자 클래스를 만듭니다.

이 문서에서는 이러한 모든 항목에 대 한 알아봅니다 제공 견고한 토대를 Xamarin.iOS에 콜백 시나리오를 처리 하기 위한 포함 하 여:

-  **이벤트** – UIKit 컨트롤과 함께 사용 하 여.NET 이벤트입니다.
-  **프로토콜** – 되는 정보와 사용 방법을 프로토콜 기능을 학습 하 고 지도 주석에 대 한 데이터를 제공 하는 예제를 만듭니다.
-  **대리자** – 강력 하 고 약한 대리자와 이러한 각를 사용 하는 경우의 차이점을 학습 한 다음 주석을 포함 하는 사용자 상호 작용을 처리 하는 지도 예제 확장 하 여 Objective-c 대리자에 대 한 학습 합니다.


프로토콜 및 대리자를 보여 주기 위해 다음 그림과 같이 지도에 주석을 추가 하는 단순 맵 응용 프로그램을 빌드합니다.

 [![](delegates-protocols-and-events-images/01-map.png "지도에 주석을 추가 하는 단순 맵 응용 프로그램의 예로") ](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ] (delegates-protocols-and-events-images/04-annotation-with-callout.png "지도에 추가 하는 예 주석")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

이 앱을 수행 하는 작업량과, 하기 전에 시작 하겠습니다.NET 이벤트는 UIKit 아래를 살펴보면 합니다.

 <a name=".NET_Events_with_UIKit" />


## <a name="net-events-with-uikit"></a>UIKit 사용 하 여.NET 이벤트

Xamarin.iOS는 UIKit 컨트롤에 대해.NET 이벤트를 노출합니다. 예를 들어 UIButton C# 람다 식을 사용 하는 다음 코드와 같이.net에서는 일반적인 방법으로 처리 하는 TouchUpInside 이벤트를 있습니다.

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

C# 2.0 스타일 익명 메서드에서이 이와 같은 구현할 수 있습니다.

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

위의 코드는 UIViewContoller의 ViewDidLoad 메서드에 연결 됩니다. aButton 변수가 참조 하는 단추는 iOS 디자이너 또는 코드를 추가할 수 있습니다. 다음 그림에는이 문서를 함께 제공 되는 샘플에서 가져온 iOS 디자이너에에서 추가 될 때이 단추를 보여 줍니다.

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "IOS 디자이너에에서 추가 된 단추")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

또한 Xamarin.iOS 컨트롤과 함께 발생 하는 상호 작용에 코드를 연결할 때의 대상 동작 스타일을 지원 합니다. Hello 단추에 대 한 대상 작업을 만들려면 두 번 클릭 디자이너는 ios입니다. UIViewController의 코드 숨김 파일이 표시 되 고 개발자를 연결 하는 메서드를 삽입할 위치를 선택 하 라는 메시지가 표시 됩니다.

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "UIViewControllers 코드 숨김 파일")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

위치 선택 된 후 새 메서드가 만들고 컨트롤에 대 한 유선 대부분이 합니다. 단추를 클릭할 때 메시지 다음 예제에서는 콘솔에 기록 됩니다.

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "메시지는 단추를 클릭할 때 콘솔에 기록 됩니다.")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS 대상 작업 패턴에 대 한 자세한 내용은의 대상 작업 섹션을 참조 하십시오. " [iOS에 대 한 핵심 응용 프로그램 경쟁력](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)" Apple iOS 개발자 라이브러리에서에서.

Xamarin.iOS 함께 iOS 디자이너를 사용 하는 방법에 대 한 자세한 내용은 참조는 " [디자이너 개요 iOS](~/ios/user-interface/designer/index.md)" 설명서입니다.

 <a name="Events" />


## <a name="events"></a>이벤트

다양 한 옵션 있는 UIControl에서 이벤트를 가로챌 하려는 경우: C# 람다 식 및 하위 수준 Objective-c Api를 사용 하 여 대리자 함수를 사용 하 여 합니다.

다음 섹션에서는 필요한 제어 정도 따라 단추에 TouchDown 이벤트 캡처는 방법을 보여 줍니다.

 <a name="C#_Style" />


## <a name="c-style"></a>C# 스타일

대리자 구문을 사용 하 여:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

마음에 람다 대신:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

포함 하려는 경우 여러 단추 동일한 처리기를 사용 하 여 동일한 코드를 공유:

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>여러 종류의 이벤트를 모니터링합니다.

UIControlEvent 플래그에 대 한 C# 이벤트에는 개별 플래그는 일대일 매핑을 포함 합니다. 두 개 이상의 이벤트를 처리, 사용 하 여 코드의 동일한 부분이 할는 `UIControl.AddTarget` 메서드:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

람다 구문 사용:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

연결에 특정 개체 인스턴스를 특정 선택기를 호출 하는 방식 처럼 Objective C의 하위 수준 기능을 사용 하려면:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

참고, 상속된 된 기본 클래스의 인스턴스 메서드를 구현 하는 경우 공용 메서드의 이어야 합니다.

 <a name="Protocols" />


## <a name="protocols"></a>프로토콜

프로토콜은 메서드 선언의 목록을 제공 하는 Objective C 언어 기능. 프로토콜을 선택적 메서드를 사용할 수 있다는 C#에서 중요 한 차이점 인터페이스에 비슷한 목적을 갖습니다. 프로토콜을 채택 하는 클래스에서 구현 하지 않는 경우에 선택적 메서드 호출 되지 않습니다. 또한 Objective C의 단일 클래스 C# 클래스에는 여러 인터페이스를 구현할 수와 마찬가지로 여러 프로토콜을 구현할 수입니다.

Apple â C# 인터페이스와 동일 하 게 작동 하므로 호출자에서 구현 하는 클래스를 도외시 동안 채택 하는 클래스에 대 한 계약을 정의 iOS 전체 ´ ù. 프로토콜을 사용 하 대리자가 아닌 시나리오에서 둘 다 (같은와 `MKAnnotation` 다음 표시 된 예), 및 대리자 (대리자 섹션에서이 문서의 뒷부분에 표시)으로 합니다.

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Xamarin.ios 있는 프로토콜

Xamarin.iOS에서는 Objective-c 프로토콜을 사용 하는 예제를 살펴보겠습니다. 이 예에서는 `MKAnnotation` 참가 하는 프로토콜의는 `MapKit` 프레임 워크입니다. `MKAnnotation` 지도에 추가할 수 있는 주석에 대 한 정보를 제공할 것을 채택 하는 모든 개체를 허용 하는 프로토콜이입니다. 예를 들어, 구현 하는 개체 `MKAnnotation` 주석이 연결 된 제목 위치를 제공 합니다.

이러한 방식으로 `MKAnnotation` 프로토콜은 주석을 함께 제공 되는 관련 데이터를 제공 하는 데 사용 됩니다. 주석 자체에 대 한 실제 보기에는 적용 하는 개체의 데이터에서 작성 되는 `MKAnnotation` 프로토콜입니다. 설명선 (아래 스크린샷에 나와 있는 것) 처럼 사용자가 주석을 누를 때 표시 되는 텍스트에서 제공 하는 예를 들어는 `Title` 프로토콜을 구현 하는 클래스의 속성:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "사용자가 주석을 누르면 설명선에 대 한 예제 텍스트")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

프로토콜 심층 분석 다음 섹션에 설명 된 대로 Xamarin.iOS 프로토콜 추상 클래스에 바인딩합니다. 에 대 한는 `MKAnnotation` 프로토콜을 바인딩된 C# 클래스 이름은 `MKAnnotation` 는 프로토콜의 이름과 모방 하기 위해의 서브 클래스는 `NSObject`, CocoaTouch에 대 한 루트 기본 클래스입니다. Getter 및 setter 좌표;에 대 한 구현 하는 프로토콜 요구 그러나 제목 및 부제목은 선택적입니다. 따라서는 `MKAnnotation` 클래스는 `Coordinate` 속성은 *추상*, 구현 요구 및 `Title` 및 `Subtitle` 속성 표시 된 *가상* 아래와 같이 선택적 있게:

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

모든 클래스에서 파생 단순히 주석 데이터를 제공할 수 있습니다 `MKAnnotation`같으면 적어도 `Coordinate` 속성 구현 됩니다. 예를 들어 다음은 생성자에 시작 하는 좌표를 사용 하 고 제목에 대 한 문자열을 반환 하는 예제 클래스가입니다.

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

서브클래싱하는 모든 클래스에 바인딩된 프로토콜을 통해 `MKAnnotation` 주석 보기를 만들 때 맵에서 사용할 수 있는 관련 데이터를 제공할 수 있습니다. 지도 주석을 추가 하려면를 호출 하기만 하면는 `AddAnnotation` 의 메서드는 `MKMapView` 다음 코드에 표시 된 인스턴스:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

여기에 지도 변수는의 인스턴스는 `MKMapView`,이 지도 자체를 나타내는 클래스입니다. `MKMapView` ´ ֲ는 `Coordinate` 에서 파생 된 데이터는 `SampleMapAnnotation` 주석 보기 지도에서 위치를 지정 하는 인스턴스.

`MKAnnotation` 프로토콜은 알려진된 집합을 제공 하기를 구현 하는 모든 개체에서 기능을 소비자 (이 경우 맵) 없이 알 필요가 구현 세부 사항에 대 한 합니다. 이 간소화 가능한 주석의 다양 한 지도에 추가 합니다.

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>프로토콜에 대 한 고찰 정보

C#의 인터페이스 선택적 메서드를 지원 하지 않으므로 Xamarin.iOS 프로토콜을 추상 클래스에 매핑합니다. 따라서 채택 Objective C의 프로토콜에에서 따라 이루어집니다 Xamarin.iOS 프로토콜에 바인딩되는 추상 클래스에서 파생 되 고 필요한 메서드를 구현 합니다. 이러한 메서드는 클래스의 추상 메서드로 노출 됩니다. 선택적 메서드는 프로토콜에서 C# 클래스의 가상 메서드에 바인딩됩니다.

예를 들어 다음은의 일부는 `UITableViewDataSource` 에 바인딩된 Xamarin.iOS 변수로 프로토콜:

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

참고 추상 클래스입니다. Xamarin.iOS는 프로토콜에서 필수/선택적 메서드를 지원 하기 위해 추상 클래스를 만듭니다.
그러나 Objective-c 프로토콜 (또는 C#의 인터페이스)와 달리 C# 클래스 다중 상속을 지원 하지 않습니다. 이 프로토콜을 사용 하 고 일반적으로 중첩된 클래스에 연결 하는 C# 코드의 디자인을 영향을 줍니다. 더이 문제에 대 한 대리자 섹션에서이 문서의 뒷부분에 설명 됩니다.

 `GetCell(…)` 추상 메서드이며, Objective C에 바인딩된 *선택기*, `tableView:cellForRowAtIndexPath:`의 필요한 방법인는 `UITableViewDataSource` 프로토콜입니다. 선택기는 Objective-c 용어 메서드 이름입니다. 필요에 따라 메서드를 적용 하려면 Xamarin.iOS abstract로 선언 합니다. 다른 방법은 `NumberOfSections(…)`에 바인딩된 `numberOfSectionsInTableview:`합니다. 이 메서드는 프로토콜의 경우 선택 사항 이므로 Xamarin.iOS 선언한 가상으로 C#의 재정의 여부.

Xamarin.iOS 있습니다에 대 한 모든 iOS 바인딩을 처리합니다. 그러나 경우 Objective C에서 프로토콜을 수동으로 바인딩해야 해야 있습니다 그렇게 할 수 있는 클래스를 데코레이팅하는 `ExportAttribute`합니다. 이것이 Xamarin.iOS 자체에서 사용 하는 동일한 방법입니다.

Objective C 형식 Xamarin.iOS에 바인딩하는 방법에 대 한 자세한 내용은 문서 참조 [바인딩 Objective C 형식](~/ios/platform/binding-objective-c/index.md)합니다.

노력을 통해 프로토콜 아직 하지만 합니다. 또한 사용 iOS에서의 기준으로 다음 섹션의 항목은 인 Objective-c 대리자에 대 한 합니다.

 <a name="Delegates" />


## <a name="delegates"></a>대리자

iOS는 Objective-c 대리자를 사용 하 여 한 개체가 다른 작업 전달는 위임 패턴을 구현. 작업을 수행 하는 개체는 첫 번째 개체의 대리자. 개체 특정 상황이 발생한 후 메시지를 전송 하 여 작업을 수행할 수 대리자를 알려줍니다. Objective C의 다음과 같은 메시지를 보내는 하는 것은 기능적으로 C#에서 메서드를 호출 합니다. 대리자 이러한 호출에 대 한 응답에서 메서드를 구현 하 고 있으므로 응용 프로그램에 기능을 제공 합니다.

대리자를 사용 하면 클래스의 동작을 서브 클래스를 만들 필요 없이 확장할 수 있습니다. 하나의 클래스를 호출할 때 다시 간에 중요 한 동작에서 발생 한 후 iOS에서 응용 프로그램에서 자주 대리자를 사용 합니다. 예를 들어는 `MKMapView` 대리자 클래스의 작성자 응용 프로그램 내에 응답할 수 있도록 사용자가 지도 대 한 주석이 누르면 해당 대리자를 다시 호출 클래스입니다. Xamarin.iOS 사용 하 여 대리자를 이러한 종류의 예제를 사용 하 여이 문서의 뒷부분에 나오는 대리자 사용의 예를 통해 사용할 수 있습니다.

이 시점에서 할까요 클래스에서 해당 대리자를 호출 하는 방법을 결정 하는 방법입니다. 프로토콜을 사용 하는 다른 자리입니다. 대리자에서 사용할 수 있는 메서드는 일반적으로 채택할 프로토콜에서 제공 됩니다.

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>대리자에 프로토콜이 사용 되는 방법

프로토콜 지도에 주석을 추가 지 원하는 데 사용 되는 어떻게 이전에 대해 살펴보았습니다.
프로토콜도 특정 이벤트 발생 등 지도 대 한 주석 사용자 탭 한 후 또는 테이블의 셀을 선택 하는 후에 호출 하는 클래스에 대 한 메서드의 알려진된 집합 제공 하기 위해 사용 됩니다. 이러한 메서드를 구현 하는 클래스를 호출 하는 클래스의 대리자도 알려져 있습니다.

위임을 지 원하는 클래스를 대리자를 구현 하는 클래스가 할당 된 대리자 속성을 노출 하 여 그렇게 합니다. 대리자에 대 한 구현 하는 메서드는 특정 대리자를 채택 하는 프로토콜에 따라 달라 집니다. 에 대 한는 `UITableView` 메서드를 구현 하는 `UITableViewDelegate` 프로토콜을에 대 한는 `UIAccelerometer` 구현 하는 메서드를 `UIAccelerometerDelegate`대리자를 노출 하는 iOS 사용할 수 전체에서 다른 클래스에 대 한 등.

`MKMapView` 이전 예제에서 살펴본 클래스 라는 대리자를 호출 하는 속성에 다양 한 이벤트 발생 후 합니다. 에 대 한 대리자 `MKMapView` 유형의 `MKMapViewDelegate`합니다.
이 곧에서 사용 하지만 첫 번째는 선택 된 후에 주석이에 응답 하는 예제 보겠습니다 강력 하 고 약한 대리자 간의 차이점에 설명 합니다.

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>강력한 대리자 vs입니다. 약한 대리자

알아보았습니다 지금까지 대리자는 강력한 형식 의미 하는 강력한 대리자입니다. Xamarin.iOS 바인딩 iOS에서 모든 대리자 프로토콜에 대해 강력한 형식의 클래스와 함께 제공 됩니다. 그러나 iOS 약한 대리자의 개념을 있습니다. 특정 대리자의 Objective-c 프로토콜에 바인딩된 클래스 서브클래싱, 대신 iOS 수도 있습니다 원하는 ExportAttribute로 메서드를 데코레이팅하 NSObject에서 파생 된 클래스에 직접 프로토콜 메서드를 바인딩 하기로 결정 한 다음 적절 한 선택기를 제공 합니다.
이 방법을 사용 하는 경우에 클래스의 인스턴스는 WeakDelegate 속성 대신 대리자 속성에 할당 합니다. 약한 대리자는 다른 상속 계층 구조 아래로 대리자 클래스를 사용할 수 있는 유연성을 제공 합니다. 강력 하 고 약한 대리자를 사용 하는 Xamarin.iOS 예제를 살펴 보겠습니다.

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin.iOS와 대리자를 사용 하는 예

서브 클래스 예제에서 주석을 눌러 사용자에 대 한 응답에서 코드를 실행 하려면 원하므로 `MKMapViewDelegate` 인스턴스를 할당 하 고는 `MKMapView`의 `Delegate` 속성입니다. `MKMapViewDelegate` 프로토콜만 선택적 메서드를 포함 합니다.
따라서 모든 메서드는 가상의 Xamarin.iOS에서이 프로토콜에 바인딩된 `MKMapViewDelegate` 클래스입니다. 사용자가 주석을 선택는 `MKMapView` 인스턴스 송신할는 `mapView:didSelectAnnotationView:` 해당 대리자에는 메시지입니다. 이 처리 하기에 Xamarin.iOS 재정의 해야는 `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` 다음과 같이 MKMapViewDelegate 하위 클래스에서 메서드:

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

위에 표시 된 SampleMapDelegate 클래스가 포함 하는 컨트롤러에서 중첩 된 클래스로 구현 되는 `MKMapView` 인스턴스. Objective C의 채택 하는 클래스 내에서 직접 여러 프로토콜 컨트롤러를 자주 표시 됩니다. 그러나 프로토콜 Xamarin.iOS의 클래스에 바인딩된 후 강력한 형식의 대리자를 구현 하는 클래스 중첩된 클래스도 일반적으로 포함 됩니다.

대리자 클래스 구현을 준비에서 하면에 할당 하는 컨트롤러에서 대리자의 인스턴스를 인스턴스화하는 `MKMapView`의 `Delegate` 다음과 같이 속성:

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

파생 된 클래스에 직접 메서드를 바인딩 약한 대리자를 같은 작업을 수행 하기 위해 사용 해야 `NSObject` 에 할당 하는 `WeakDelegate` 의 속성은 `MKMapView`합니다. 이후는 `UIViewController` 궁극적으로에서 파생 `NSObject` (예: Objective C의에서 모든 클래스 CocoaTouch), 우리 단순히 메서드를 구현할 수는에 바인딩된 `mapView:didSelectAnnotationView:` 컨트롤러에 직접 컨트롤러를 할당 하 고 `MKMapView`의 `WeakDelegate`, extra 중첩 된 클래스에 대 한 필요성을 방지 합니다. 아래 코드에서는이 방법을 보여 줍니다.

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

이 코드를 실행 하는 경우 응용 프로그램 대리자 강력한 형식의 버전을 실행 하는 경우와 동일 하 게 작동 합니다. 이 코드에서 기능은 약한 대리자가 강력한 형식의 대리자를 사용 하는 것을 때 생성 된 추가 클래스를 만드는 필요 하지 않습니다. 그러나이 형식 안전성 저하 됩니다. 에 전달 된 선택기에 실수 하는 경우는 `ExportAttribute`를 런타임까지 확인할가 있습니다.

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>이벤트 및 대리자

대리자는.NET 이벤트를 사용 하는 방식 유사 하 게 iOS에서 콜백에 대 한 사용 됩니다. IOS를 Api와 Objective-c 대리자를 사용 하는 방법은 더 것 처럼 보일.NET, Xamarin.iOS iOS에서 대리자는 사용 하는 여러 위치에서.NET 이벤트를 노출 합니다.

이전 구현에서는 예를 들어 여기서는 `MKMapViewDelegate` 에 응답 한 선택한 주석이.NET 이벤트를 사용 하 여 구현할 Xamarin.iOS에서 수도 있습니다. 이 이벤트는에 정의 된 경우 `MKMapView` 호출 `DidSelectAnnotationView`합니다. 했을 때는 `EventArgs` 형식의 서브 클래스 `MKMapViewAnnotationEventsArgs`합니다. `View` 속성 `MKMapViewAnnotationEventsArgs` 동일 하 게 구현 진행할 수는에서 열었던 앞서 설명한 것 처럼 여기를 주석 보기에 대 한 참조를 제공 합니다.

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>요약

이 문서에서 Xamarin.iOS 이벤트, 프로토콜 및 대리자를 사용 하는 방법을 다룹니다. Xamarin.iOS 컨트롤에 대 한 일반.NET 스타일 이벤트를 노출 하는 방법에 대해 살펴보았습니다.
그런 다음 이전에 배운 것 Objective-c 프로토콜 C#의 인터페이스와에서 다른 지 Xamarin.iOS 사용 하 여 방법에 대 한 합니다. 마지막으로, Xamarin.iOS 관점에서 Objective-c 대리자 검사 했습니다. Xamarin.iOS을 지 원하는 방법 모두 강력 하 게 하 고 약한 형식의 대리자.NET 이벤트 대리자 메서드를 바인딩하는 방법에 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [프로토콜, 대리자 및 이벤트 (샘플)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [바인딩 Objective C 형식](~/ios/platform/binding-objective-c/index.md)
- [Objective C 프로그래밍 언어](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4에서에서 사용자 인터페이스 디자인](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS 용 핵심 응용 프로그램 경쟁력](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
