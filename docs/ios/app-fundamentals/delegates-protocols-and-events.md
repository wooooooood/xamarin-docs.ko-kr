---
title: Xamarin.ios의 이벤트, 프로토콜 및 대리자
description: 이 문서에서는 Xamarin.ios에서 이벤트, 프로토콜 및 대리자를 사용 하는 방법을 설명 합니다. 이러한 기본 개념은 Xamarin.ios 개발에서 사용할 수 있습니다.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2017
ms.openlocfilehash: b63d5dcd8ac1a82c1f120cc5a690985557f7e68f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004404"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Xamarin.ios의 이벤트, 프로토콜 및 대리자

Xamarin.ios는 컨트롤을 사용 하 여 대부분의 사용자 상호 작용에 대 한 이벤트를 노출 합니다.
Xamarin.ios 응용 프로그램은 기존 .NET 응용 프로그램을 사용 하는 것과 거의 동일한 방식으로 이러한 이벤트를 사용 합니다. 예를 들어 Xamarin.ios UIButton 클래스에는 TouchUpInside 라는 이벤트가 있으며이 클래스와 이벤트가 .NET 앱에 있는 것 처럼이 이벤트를 사용 합니다.

이 .NET 접근 방식 외에 Xamarin.ios는 보다 복잡 한 상호 작용 및 데이터 바인딩에 사용할 수 있는 다른 모델을 노출 합니다. 이 방법론은 Apple에서 대리자와 프로토콜을 호출 하는 것을 사용 합니다. 대리자는의 C#대리자와 비슷하지만, 단일 메서드를 정의 하 고 호출 하는 대신, 목표 C의 대리자는 프로토콜을 따르는 전체 클래스입니다. 프로토콜은의 C#인터페이스와 유사 합니다. 단, 해당 메서드는 선택 사항 일 수 있습니다. 예를 들어 UITableView를 데이터로 채우려면 UITableView에서 자신을 채우기 위해 호출 하는 UITableViewDataSource 프로토콜에 정의 된 메서드를 구현 하는 대리자 클래스를 만들 수 있습니다.

이 문서에서는 다음을 포함 하 여 Xamarin.ios에서 콜백 시나리오를 처리 하기 위한 견고한 토대를 제공 하는 모든 항목에 대해 알아봅니다.

- **이벤트** – uikit 컨트롤과 함께 .Net 이벤트 사용.
- **프로토콜** – 프로토콜 및 사용 방법에 대해 배우고 맵 주석에 대 한 데이터를 제공 하는 예제를 만듭니다.
- **대리자** – 지도 예제를 확장 하 여 주석을 포함 하는 사용자 상호 작용을 처리 하 고, 강력 하 고 약한 대리자와 이러한 각 대리자를 사용 하는 경우의 차이를 파악 하 여 목표-C 대리자에 대해 학습 합니다.

프로토콜과 대리자를 설명 하기 위해 다음과 같이 맵에 주석을 추가 하는 간단한 map 응용 프로그램을 빌드합니다.

[![](delegates-protocols-and-events-images/01-map-sml.png "An example of a simple map application that adds an annotation to a map")](delegates-protocols-and-events-images/01-map.png#lightbox)
[![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "An example annotation added to a map")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

이 앱을 주요 당면 하기 전에 UIKit에서 .NET 이벤트를 살펴보면 시작 하겠습니다.

## <a name="net-events-with-uikit"></a>UIKit를 사용 하는 .NET 이벤트

Xamarin.ios는 UIKit 컨트롤에서 .NET 이벤트를 노출 합니다. 예를 들어 UIButton에는 C# 람다 식을 사용 하는 다음 코드와 같이 일반적으로 .net에서와 같이 처리 하는 TouchUpInside 이벤트가 있습니다.

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

다음과 같이 2.0 스타일의 C# 무명 메서드를 사용 하 여이를 구현할 수도 있습니다.

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

위의 코드는 UIViewController의 `ViewDidLoad` 메서드에 연결 되어 있습니다. `aButton` 변수는 iOS 디자이너 또는 코드를 사용 하 여 추가할 수 있는 단추를 참조 합니다. 다음 그림은 iOS 디자이너에 추가 된 단추를 보여 줍니다.

[![](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png "A button added in iOS Designer")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

또한 xamarin.ios는 컨트롤과 함께 발생 하는 상호 작용에 코드를 연결 하는 대상 작업 스타일을 지원 합니다. **Hello** 단추의 대상 작업을 만들려면 iOS 디자이너에서 해당 작업을 두 번 클릭 합니다. UIViewController의 코드 숨겨진 파일이 표시 되 고 개발자는 연결 방법을 삽입할 위치를 선택 하 라는 메시지가 표시 됩니다.

[![](delegates-protocols-and-events-images/03-interface-builder-action-sml.png "The UIViewControllers code-behind file")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

위치를 선택 하면 새 메서드가 생성 되 고 컨트롤에 연결 됩니다. 다음 예제에서는 단추를 클릭 하면 콘솔에 메시지가 기록 됩니다.

[![](delegates-protocols-and-events-images/05-interface-builder-action-sml.png "A message will be written to the console when the button is clicked")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS 대상 작업 패턴에 대 한 자세한 내용은 Apple iOS 개발자 라이브러리의 [ios에 대 한 핵심 응용 프로그램](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) 역량의 대상-작업 섹션을 참조 하세요.

Xamarin.ios에서 iOS Designer를 사용 하는 방법에 대 한 자세한 내용은 [Ios 디자이너 개요](~/ios/user-interface/designer/index.md) 설명서를 참조 하세요.

## <a name="events"></a>이벤트

UIControl에서 이벤트를 가로채는 경우 C# 람다 및 대리자 함수를 사용 하 여 하위 수준 목표-C api를 사용 하는 등의 다양 한 옵션을 사용할 수 있습니다.

다음 섹션에서는 필요한 컨트롤의 양에 따라 단추에서 System.windows.uielement.touchdown> 이벤트를 캡처하는 방법을 보여 줍니다.

## <a name="c-style"></a>C#Style

대리자 구문 사용:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

대신 람다를 사용할 경우:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

여러 단추가 동일한 처리기를 사용 하 여 동일한 코드를 공유 하도록 하려면 다음을 수행 합니다.

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

## <a name="monitoring-more-than-one-kind-of-event"></a>여러 종류의 이벤트 모니터링

UIControlEvent C# 플래그에 대 한 이벤트는 개별 플래그에 일 대 일 매핑을 포함 합니다. 동일한 코드 조각을 사용 하 여 두 개 이상의 이벤트를 처리 하려는 경우 `UIControl.AddTarget` 메서드를 사용 합니다.

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

람다 구문 사용:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

특정 개체 인스턴스에 연결 하 고 특정 선택기를 호출 하는 것 처럼 목표-C의 하위 수준 기능을 사용 해야 하는 경우 다음을 수행 합니다.

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

상속 된 기본 클래스에서 인스턴스 메서드를 구현 하는 경우 공용 메서드 여야 합니다.

## <a name="protocols"></a>프로토콜

프로토콜은 메서드 선언 목록을 제공 하는 객관적인 C 언어 기능입니다. 의 C#인터페이스와 비슷한 용도를 제공 합니다. 주요 차이점은 프로토콜이 선택적 메서드를 가질 수 있다는 것입니다. 프로토콜을 채택 하는 클래스가 이러한 메서드를 구현 하지 않는 경우에는 선택적 메서드가 호출 되지 않습니다. 또한, 목표 C의 단일 클래스는 C# 클래스가 여러 인터페이스를 구현할 수 있는 것 처럼 여러 프로토콜을 구현할 수 있습니다.

Apple은 iOS 전체에서 프로토콜을 사용 하 여 클래스에 대 한 계약을 정의 하는 동시에 호출자의 구현 클래스를 추상화 하므로 C# 인터페이스 처럼 작동 합니다. 프로토콜은 비 대리자 시나리오 (예: 다음에 표시 되는 `MKAnnotation` 예제)에서 사용 되며 대리자 (이 문서의 뒷부분에 나오는 대리자 섹션)에서 사용 됩니다.

### <a name="protocols-with-xamarinios"></a>Xamarin.ios를 사용 하는 프로토콜

Xamarin.ios에서 목표-C 프로토콜을 사용 하는 예를 살펴보겠습니다. 이 예제에서는 `MapKit` 프레임 워크의 일부인 `MKAnnotation` 프로토콜을 사용 합니다. `MKAnnotation`는이를 채택 하는 모든 개체가 맵에 추가할 수 있는 주석에 대 한 정보를 제공 하는 데 사용할 수 있는 프로토콜입니다. 예를 들어 `MKAnnotation`를 구현 하는 개체는 주석의 위치와 연결 된 제목을 제공 합니다.

이러한 방식으로 `MKAnnotation` 프로토콜을 사용 하 여 주석과 함께 제공 되는 관련 데이터를 제공 합니다. 주석 자체의 실제 뷰는 `MKAnnotation` 프로토콜을 채택 하는 개체의 데이터로 빌드됩니다. 예를 들어 아래 스크린샷에서와 같이 사용자가 주석에서 탭 할 때 표시 되는 설명선의 텍스트는 프로토콜을 구현 하는 클래스의 `Title` 속성에서 제공 됩니다.

 [![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "Example text for the callout when the user taps on the annotation")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

다음 섹션에 설명 된 대로 [프로토콜 심층](#protocols-deep-dive)이해, xamarin.ios는 프로토콜을 추상 클래스에 바인딩합니다. `MKAnnotation` 프로토콜의 경우 바인딩된 C# 클래스는 프로토콜의 이름을 모방 하기 위해`MKAnnotation`이름이 지정 되며 CocoaTouch에 대 한 루트 기본 클래스인`NSObject`의 서브 클래스입니다. 이 프로토콜을 사용 하려면 좌표에 대해 getter 및 setter를 구현 해야 합니다. 그러나 제목과 부제목은 선택 사항입니다. 따라서 `MKAnnotation` 클래스에서 `Coordinate` 속성은 *abstract*로 설정 되어야 하며,이 속성을 구현 하 고 `Title` 및 `Subtitle` 속성을 *가상*으로 표시 하 여 아래와 같이 옵션을 선택 합니다.

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

`Coordinate` 속성이 하나 이상 구현 되어 있으면 모든 클래스는 `MKAnnotation`에서 파생 하 여 주석 데이터를 제공할 수 있습니다. 예를 들어 다음은 생성자의 좌표를 사용 하 고 제목에 대 한 문자열을 반환 하는 샘플 클래스입니다.

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return title;
        }
    }
}
```

클래스가 바인딩되는 프로토콜을 통해, 서브 클래스 `MKAnnotation`는 주석의 뷰를 만들 때 맵에서 사용할 관련 데이터를 제공할 수 있습니다. 맵에 주석을 추가 하려면 다음 코드와 같이 `MKMapView` 인스턴스의 `AddAnnotation` 메서드를 호출 하면 됩니다.

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

여기서 map 변수는 맵 자체를 나타내는 클래스인 `MKMapView` 인스턴스입니다. `MKMapView`는 `SampleMapAnnotation` 인스턴스에서 파생 된 `Coordinate` 데이터를 사용 하 여 주석 보기를 맵에 배치 합니다.

`MKAnnotation` 프로토콜은 구현 세부 정보를 알아야 하는 소비자 (이 경우 map) 없이 구현 하는 모든 개체에 대해 알려진 기능 집합을 제공 합니다. 이렇게 하면 다양 한 주석을 맵에 쉽게 추가할 수 있습니다.

### <a name="protocols-deep-dive"></a>프로토콜 심층 살펴보기

인터페이스 C# 는 선택적 메서드를 지원 하지 않으므로 xamarin.ios는 프로토콜을 추상 클래스에 매핑합니다. 따라서 목표-C에서 프로토콜을 채택 하는 것은 프로토콜에 바인딩되고 필요한 메서드를 구현 하는 추상 클래스에서 파생 하 여 Xamarin.ios에서 수행 됩니다. 이러한 메서드는 클래스에서 추상 메서드로 노출 됩니다. 프로토콜의 선택적 메서드는 C# 클래스의 가상 메서드에 바인딩됩니다.

예를 들어 다음은 Xamarin.ios에서 바인딩되는 `UITableViewDataSource` 프로토콜의 일부입니다.

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

클래스는 추상 클래스입니다. Xamarin.ios를 사용 하면 클래스를 추상화 하 여 프로토콜에서 선택적/필수 메서드를 지원할 수 있습니다.
그러나 목적과 C# 인터페이스와 달리 클래스는 C# 다중 상속을 지원 하지 않습니다. 이는 프로토콜을 사용 C# 하는 코드 디자인에 영향을 주며 일반적으로 중첩 된 클래스를 사용 합니다. 이 문제에 대 한 자세한 내용은이 문서의 뒷부분에 있는 대리자 섹션에서 설명 합니다.

 `GetCell(…)`는 `UITableViewDataSource` 프로토콜의 필수 메서드인 `tableView:cellForRowAtIndexPath:` 목표-C *선택기*에 바인딩된 추상 메서드입니다. Selector는 메서드 이름에 대 한 목표-C 용어입니다. 필요에 따라 메서드를 적용 하기 위해 Xamarin.ios는 추상으로 선언 합니다. 다른 메서드인 `NumberOfSections(…)`은 `numberOfSectionsInTableview:`에 바인딩됩니다. 이 메서드는 프로토콜에서 선택 사항 이므로 Xamarin.ios는 가상으로 선언 하므로에서 C#재정의할 수 있습니다.

Xamarin.ios는 모든 iOS 바인딩을 처리 합니다. 그러나 목표-C에서 수동으로 프로토콜을 바인딩해야 하는 경우에는 클래스를 `ExportAttribute`로 데코레이팅하 여 수행할 수 있습니다. 이는 Xamarin.ios 자체에서 사용 되는 방법과 동일 합니다.

Xamarin.ios에서 목표-C 형식을 바인딩하는 방법에 대 한 자세한 내용은 [바인딩 목표-c 형식](~/ios/platform/binding-objective-c/index.md)문서를 참조 하세요.

그러나 아직 프로토콜을 사용 하는 것은 아닙니다. 또한이는 다음 섹션의 항목인 목표-C 대리자의 기반으로 iOS에서 사용 됩니다.

## <a name="delegates"></a>대리자

iOS는 목표-C 대리자를 사용 하 여 한 개체가 다른 개체와 작업을 전달 하는 위임 패턴을 구현 합니다. 작업을 수행 하는 개체는 첫 번째 개체의 대리자입니다. 개체는 특정 상황이 발생 한 후 메시지를 전송 하 여 해당 대리자에 게 작업을 수행 하도록 지시 합니다. 목표 C에서와 같이 메시지를 보내는 것은에서 C#메서드를 호출 하는 것과 기능적으로 동일 합니다. 대리자는 이러한 호출에 대 한 응답으로 메서드를 구현 하므로 응용 프로그램에 기능을 제공 합니다.

대리자를 사용 하면 서브 클래스를 만들 필요 없이 클래스의 동작을 확장할 수 있습니다. IOS의 응용 프로그램은 중요 한 작업이 발생 한 후 한 클래스가 다른 클래스를 다시 호출 하는 경우 대리자를 사용 하는 경우가 많습니다. 예를 들어 사용자가 맵에 대 한 주석을 탭 할 때 `MKMapView` 클래스가 대리자를 다시 호출 하 여 대리자 클래스의 작성자에 게 응용 프로그램 내에서 응답할 수 있는 기회를 제공 합니다. 이 문서의 뒷부분에 나오는 이러한 형식의 대리자 사용에 대 한 예제를 통해 작업할 수 있습니다. 예를 들어 Xamarin.ios에서 대리자를 사용 합니다.

이 시점에서 클래스가 대리자에 대해 호출할 메서드를 결정 하는 방법을 궁금할 수 있습니다. 프로토콜을 사용 하는 다른 위치입니다. 일반적으로 대리자에 사용할 수 있는 메서드는 채택 하는 프로토콜에서 제공 됩니다.

### <a name="how-protocols-are-used-with-delegates"></a>대리자에 프로토콜을 사용 하는 방법

이전에는 프로토콜을 사용 하 여 맵에 주석을 추가 하는 방법을 살펴보았습니다.
프로토콜은 사용자가 맵에 대 한 주석을 탭 하거나 테이블의 셀을 선택 하는 경우와 같이 특정 이벤트가 발생 한 후에 호출 하는 클래스에 대 한 알려진 메서드 집합을 제공 하는 데도 사용 됩니다. 이러한 메서드를 구현 하는 클래스를 호출 하는 클래스의 대리자 라고 합니다.

위임을 지 원하는 클래스는 대리자를 구현 하는 클래스에 할당 되는 대리자 속성을 노출 하 여이 작업을 수행 합니다. 대리자에 대해 구현 하는 메서드는 특정 대리자가 적용 하는 프로토콜에 따라 달라 집니다. `UITableView` 메서드의 경우 `UITableViewDelegate` 프로토콜을 구현 합니다. `UIAccelerometer` 메서드에 대해 `UIAccelerometerDelegate`를 구현 하 고,이를 통해 모든 iOS에서 대리자를 노출할 다른 모든 클래스에 대해를 구현할 수 있습니다.

이전 예제에서 본 `MKMapView` 클래스에는 다양 한 이벤트가 발생 한 후에 호출 하는 Delegate 라는 속성이 있습니다. `MKMapView`에 대 한 대리자는 `MKMapViewDelegate`형식입니다.
이를 사용 하 여 선택한 후 주석에 응답 하는 것이 좋습니다. 하지만 먼저 강력 하 고 약한 대리자 간의 차이점에 대해 살펴보겠습니다.

### <a name="strong-delegates-vs-weak-delegates"></a>강력한 대리자 및 약한 대리자

지금까지 살펴본 대리자는 강력 하 게 형식화 된 대리자입니다. Xamarin.ios 바인딩은 iOS의 모든 대리자 프로토콜에 대 한 강력한 형식의 클래스와 함께 제공 됩니다. 그러나 iOS에는 약한 대리자의 개념도 있습니다. 특정 대리자에 대 한 목표-C 프로토콜에 바인딩된 클래스를 서브클래싱 하는 대신, iOS를 사용 하 여 NSObject에서 파생 되는 모든 클래스에서 직접 프로토콜 메서드를 바인딩하고, 메서드를 ExportAttribute로 데코레이팅하는 것을 선택할 수도 있습니다. 적절 한 선택기를 제공 합니다.
이 방법을 사용 하는 경우 대리자 속성이 아닌 WeakDelegate 속성에 클래스의 인스턴스를 할당 합니다. 약한 대리자는 대리자 클래스를 다른 상속 계층 구조에서 사용할 수 있는 유연성을 제공 합니다. 강력 하 고 약한 대리자를 모두 사용 하는 Xamarin.ios 예를 살펴보겠습니다.

### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin.ios에서 대리자를 사용 하는 예제

예제에서 주석을 탭 하 여 사용자에 대 한 응답으로 코드를 실행 하기 위해 `MKMapViewDelegate`를 서브클래싱하 고 `MKMapView`의 `Delegate` 속성에 인스턴스를 할당할 수 있습니다. `MKMapViewDelegate` 프로토콜에는 선택적 메서드만 포함 됩니다.
따라서 모든 메서드는 Xamarin.ios `MKMapViewDelegate` 클래스에서이 프로토콜에 바인딩된 가상입니다. 사용자가 주석을 선택 하면 `MKMapView` 인스턴스가 대리자에 게 `mapView:didSelectAnnotationView:` 메시지를 보냅니다. Xamarin.ios에서이를 처리 하려면 다음과 같이 MKMapViewDelegate 하위 클래스의 `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` 메서드를 재정의 해야 합니다.

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

위에 표시 된 SampleMapDelegate 클래스는 `MKMapView` 인스턴스를 포함 하는 컨트롤러에서 중첩 된 클래스로 구현 됩니다. 목표-C에서는 컨트롤러가 클래스 내에서 직접 여러 프로토콜을 채택 하는 것을 볼 수 있습니다. 그러나 프로토콜은 Xamarin.ios의 클래스에 바인딩되므로 강력한 형식의 대리자를 구현 하는 클래스는 일반적으로 중첩 클래스로 포함 됩니다.

대리자 클래스 구현을 구현 하면 컨트롤러에서 대리자의 인스턴스를 인스턴스화하고 다음과 같이 `MKMapView`의 `Delegate` 속성에 할당 하기만 하면 됩니다.

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

Weak 대리자를 사용 하 여 동일한 작업을 수행 하려면 `NSObject`에서 파생 되는 클래스에서 직접 메서드를 바인딩하고 `MKMapView`의 `WeakDelegate` 속성에 할당 해야 합니다. `UIViewController` 클래스는 궁극적으로는 `NSObject`에서 파생 되므로 (CocoaTouch의 모든 목표-C 클래스와 같이) 컨트롤러에서 직접 `mapView:didSelectAnnotationView:`에 바인딩된 메서드를 구현 하 고 컨트롤러를 `MKMapView`의 `WeakDelegate`에 할당 하 여 추가 중첩 클래스입니다. 아래 코드에서는이 방법을 보여 줍니다.

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

이 코드를 실행 하는 경우 응용 프로그램은 강력한 형식의 대리자 버전을 실행할 때와 정확히 동일 하 게 동작 합니다. 이 코드의 혜택은 약한 대리자가 강력한 형식의 대리자를 사용할 때 생성 된 추가 클래스를 만들 필요가 없다는 것입니다. 그러나이는 형식 안전성을 위해 제공 됩니다. `ExportAttribute`전달 된 선택기에서 실수를 수행 하는 경우 런타임이 될 때까지 찾을 수 없습니다.

### <a name="events-and-delegates"></a>이벤트 및 대리자

대리자는 .NET에서 이벤트를 사용 하는 방식과 유사 하 게 iOS의 콜백에 사용 됩니다. IOS Api와 이러한 대리자를 사용 하는 방식이 .NET 처럼 보이는 것 처럼 보일 때 Xamarin.ios는 iOS에서 대리자를 사용 하는 여러 위치에 .NET 이벤트를 노출 합니다.

예를 들어, 선택 된 주석에 응답 하는 `MKMapViewDelegate`에 대 한 이전 구현은 .NET 이벤트를 사용 하 여 Xamarin.ios에서 구현 될 수도 있습니다. 이 경우 이벤트는 `MKMapView` 정의 되 고 `DidSelectAnnotationView` 이라고 합니다. `MKMapViewAnnotationEventsArgs`형식의 `EventArgs` 서브 클래스가 있습니다. `MKMapViewAnnotationEventsArgs`의 `View` 속성은 다음과 같이 앞에서 설명한 것과 동일한 구현으로 진행할 수 있는 주석 보기에 대 한 참조를 제공 합니다.

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

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios에서 이벤트, 프로토콜 및 대리자를 사용 하는 방법을 설명 했습니다. Xamarin.ios가 컨트롤에 대 한 일반적인 .NET 스타일 이벤트를 노출 하는 방법을 살펴보았습니다.
다음에는 C# 인터페이스와 다른 방법 및 xamarin.ios를 사용 하는 방법을 포함 하 여 목표 C 프로토콜에 대해 알아보았습니다. 마지막으로, Xamarin.ios 관점에서 목표-C 대리자를 검사 했습니다. Xamarin.ios에서 강력 하 고 약한 형식의 대리자를 지 원하는 방법과 .NET 이벤트를 대리자 메서드에 바인딩하는 방법에 대해 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [프로토콜, 대리자 및 이벤트 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/protocols-delegates-events)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [바인딩 목표-C 형식](~/ios/platform/binding-objective-c/index.md)
- [목표-C 프로그래밍 언어](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4에서 사용자 인터페이스 디자인](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS에 대 한 핵심 응용 프로그램 역량](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
