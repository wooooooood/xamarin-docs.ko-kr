---
title: 이벤트, 프로토콜 및 Xamarin.iOS에서 대리자
description: 이 문서는 이벤트, 프로토콜을 사용 하는 방법에 설명 하 고 Xamarin.iOS에서 위임. 이러한 기본 개념은 Xamarin.iOS 개발에서.
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/17/2017
ms.openlocfilehash: 5ccefdb5e527e67338714896905734c74278d00a
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57671882"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>이벤트, 프로토콜 및 Xamarin.iOS에서 대리자

Xamarin.iOS는 대부분의 사용자 상호 작용에 대 한 이벤트를 노출 하려면 컨트롤을 사용 합니다.
기존.NET 응용 프로그램으로 동일한 방식으로 이러한 이벤트를 사용 하는 Xamarin.iOS 응용 프로그램. 예를 들어 Xamarin.iOS UIButton 클래스 TouchUpInside 라는 이벤트가 있고 것 처럼이 클래스와 이벤트.NET 앱에서이 이벤트를 사용 합니다.

Xamarin.iOS는.NET 이렇게 외에도 더 복잡 한 상호 작용 및 데이터 바인딩을 사용할 수 있는 다른 모델을 표시 합니다. 이 방법론 Apple 대리자 및 프로토콜 호출을 사용 합니다. 대리자는 대리자를 개념상 비슷하지만 C#, 하지만 Objective C에서 대리자는 프로토콜을 준수 하는 전체 클래스를 정의 하 고 단일 메서드를 호출 하는 대신 합니다. 프로토콜에서 인터페이스에 비슷합니다. C#에 해당 메서드는 선택 사항일 수 있습니다. 예를 들어 데이터를 사용 하 여 UITableView를 채우기 위해 자체를 채우는 데는 UITableView 호출 UITableViewDataSource 프로토콜에 정의 된 메서드를 구현 하는 대리자 클래스를 만듭니다.

이러한 모든 항목에 대해 알아봅니다이 문서에서는 제공 견고한 Xamarin.iOS에서 콜백 시나리오를 처리 하는 것에 대 한 포함:

- **이벤트** – UIKit 컨트롤을 사용 하 여 사용 하 여.NET 이벤트입니다.
- **프로토콜** – 새로운 학습 프로토콜은 및 사용 방법에 및 맵 주석에 대 한 데이터를 제공 하는 예제를 만들어야 합니다.
- **대리자** – 주석을 포함 하는 사용자 상호 작용을 처리 하 여 map 예를 확장 한 다음 강력한 / 취약 한 대리자와 이러한 각를 사용 하는 경우의 차이점을 학습 하 여 Objective-c 대리자에 대 한 학습 합니다.

프로토콜 및 대리자를 보여 주기 위해 여기에 표시 된 맵에 주석을 추가 하는 단순 맵 응용 프로그램을 빌드 해 보겠습니다.

[![](delegates-protocols-and-events-images/01-map-sml.png "지도에 주석을 추가 하는 단순 맵 응용 프로그램의 예로")](delegates-protocols-and-events-images/01-map.png#lightbox)
[![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "예제 주석의 맵에 추가")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

이 앱에 들어가기에 시작.NET 이벤트는 UIKit 아래를 보면 됩니다.

## <a name="net-events-with-uikit"></a>UIKit 사용 하 여.NET 이벤트

Xamarin.iOS는 UIKit 컨트롤에 대 한.NET 이벤트를 노출합니다. 예를 들어 UIButton에 TouchUpInside 이벤트를 사용 하는 다음 코드와 같이.net에서 일반적인 방법으로 처리는 C# 람다 식:

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

이를 구현할 수도 있습니다는 C# 2.0 스타일의 무명 메서드는 다음과 같은:

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

위의 코드 연결 되는 `ViewDidLoad` 메서드는 UIViewController의 합니다. `aButton` 변수 참조 단추가 iOS 디자이너 또는 코드를 사용 하 여 추가할 수 있습니다. 다음 그림 iOS 디자이너에에서 추가한 단추를 보여 줍니다.

[![](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png "IOS 디자이너에에서 추가 단추")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS는 또한 컨트롤을 사용 하 여 발생 하는 상호 작용에 코드를 연결 하는 대상 동작 스타일을 지원 합니다. 대상 작업을 만드는 합니다 **Hello** 단추를 두 번 클릭 하 여 iOS 디자이너에서에서 합니다. UIViewController의 코드 숨김 파일이 표시 되 고 개발자에서 연결 메서드를 삽입할 위치를 선택 하 라는 메시지가 표시 됩니다.

[![](delegates-protocols-and-events-images/03-interface-builder-action-sml.png "UIViewControllers 코드 숨김 파일")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

위치를 선택한 후 새 메서드 생성 되어 컨트롤에 유선 접속 합니다. 다음 예제에서는 단추를 클릭할 때 메시지를 콘솔에 기록 수 됩니다.

[![](delegates-protocols-and-events-images/05-interface-builder-action-sml.png "단추를 클릭할 때 메시지를 콘솔에 기록 됩니다.")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS 대상 동작 패턴에 대 한 자세한 내용은의 대상 작업 섹션을 참조 하세요 [iOS 용 핵심 응용 프로그램 역량](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) Apple iOS 개발자 라이브러리에서에서.

Xamarin.iOS를 사용 하 여 iOS 디자이너를 사용 하는 방법에 대 한 자세한 내용은 참조는 [iOS 디자이너 개요](~/ios/user-interface/designer/index.md) 설명서.

## <a name="events"></a>이벤트

UIControl에서 이벤트를 가로채 고 원하는 경우 다양 한 옵션: 사용 하 여는 C# 람다 식 및 하위 수준 Objective C Api를 사용 하 여 대리자 함수입니다.

다음 섹션에서는 얼마나 많이 제어 필요에 따라 단추, TouchDown 이벤트를 캡처할는 방법을 보여 줍니다.

## <a name="c-style"></a>C#스타일

대리자 구문을 사용합니다.

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

사용 하려는 경우 람다 대신:

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

원할 경우 여러 단추 동일한 코드를 공유할 동일한 처리기를 사용 합니다.

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

## <a name="monitoring-more-than-one-kind-of-event"></a>둘 이상의 유형의 이벤트를 모니터링합니다.

C# 이벤트 UIControlEvent 플래그에 대 한 개별 플래그 한 일 매핑이 있어야 합니다. 두 개 이상의 이벤트를 처리, 사용 하려는 코드의 동일한 부분이 있는 경우는 `UIControl.AddTarget` 메서드:

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

람다 구문을 사용합니다.

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

특정 선택기를 호출 하는 특정 개체 인스턴스를 후크 같은 Objective C의 하위 수준 기능을 사용 하는 경우:

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

참고, 상속된 된 기본 클래스의 인스턴스 메서드를 구현 하는 경우 공용 메서드가 이어야 합니다.

## <a name="protocols"></a>프로토콜

프로토콜에는 메서드 선언의 목록을 제공 하는 Objective C 언어 기능입니다. 인터페이스에 비슷한 용도로 사용 C#, 기본적으로 차이가 프로토콜이 선택적 메서드를 사용할 수 있습니다. 프로토콜을 채택 하는 클래스에서 구현 하지 않는 경우에 선택적 메서드 호출 되지 않습니다. Objective C에서 단일 클래스 처럼는 여러 프로토콜을 구현할 수 또한는 C# 클래스는 여러 인터페이스를 구현할 수 있습니다.

Apple iOS 전체 프로토콜을 사용 하 여 클래스와 동일 하 게 작동 하므로 호출자에서 구현 하는 클래스를 추상화 하는 동안 채택에 대 한 계약을 정의 하는 C# 인터페이스입니다. 프로토콜을 사용 둘 다 대리자가 아닌 시나리오에서 (같은 사용 하 여를 `MKAnnotation` 다음에 표시 된 예제), 대리자 (대리자 섹션에서이 문서의 뒷부분에 표시)으로 사용 하 여 합니다.

### <a name="protocols-with-xamarinios"></a>Xamarin.ios 사용 하 여 프로토콜

Xamarin.iOS에서 Objective-c 프로토콜을 사용 하는 예제를 살펴보겠습니다. 사용 예를 들어 합니다 `MKAnnotation` 참가 하는 프로토콜의는 `MapKit` 프레임 워크입니다. `MKAnnotation` 맵에 추가할 수 있는 주석에 대 한 정보를 제공 하 고 채택 하는 개체를 허용 하는 프로토콜입니다. 예를 들어, 구현 하는 개체 `MKAnnotation` 주석 및 연결 된 제목 위치를 제공 합니다.

이러한 방식으로 `MKAnnotation` 프로토콜을 사용 하 여 주석을 함께 제공 되는 관련 데이터를 제공할 수 있습니다. 자체 주석에 대 한 실제 보기 채택 하는 개체의 데이터에서 빌드되는 `MKAnnotation` 프로토콜입니다. 아래 스크린샷에 같이 사용자가 주석을 누를 때 표시 되는 설명선 텍스트에서 제공 하는 예를 들어를 `Title` 프로토콜을 구현 하는 클래스의 속성:

 [![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "사용자 주석을 누르면 설명선 텍스트 예제")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

다음 섹션에 설명 된 대로 [프로토콜에 대 한 심층 정보](#protocols-deep-dive), Xamarin.iOS 추상 클래스에 프로토콜을 바인딩합니다. 에 대 한 합니다 `MKAnnotation` 프로토콜, 바인딩된 C# 클래스 이름은 `MKAnnotation` 프로토콜의 이름을 모방 하기 위해의 서브 클래스는 `NSObject`, CocoaTouch에 대 한 루트 기본 클래스입니다. Getter 및 setter 좌표;에 대해 구현 해야 하는 프로토콜 필요 그러나 제목 및 부제목은 선택적입니다. 따라서 합니다 `MKAnnotation` 클래스를 `Coordinate` 속성은 *추상*를 구현 해야 한다는 및 `Title` 및 `Subtitle` 속성은 표시 됩니다 *가상* 를 아래와 같이 선택적 있도록 합니다.

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

모든 클래스에서 파생 하기만 하 여 주석 데이터를 제공할 수 있습니다 `MKAnnotation`같으면 적어도 `Coordinate` 속성 구현 됩니다. 예를 들어, 다음은 생성자에서 시작 하는 좌표를 사용 하 고 제목에 대 한 문자열을 반환 하는 샘플 클래스가입니다.

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

모든 클래스에 바인딩되어 프로토콜을 통해 `MKAnnotation` 주석의 보기를 만들 때 맵에서 사용 되는 관련 데이터를 제공할 수 있습니다. 주석의 지도를 추가 하려면 호출 하기만 하면 됩니다는 `AddAnnotation` 메서드는 `MKMapView` 다음 코드에 표시 된 대로 인스턴스:

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

여기에 맵 변수의 인스턴스가 `MKMapView`, 맵 자체를 나타내는 클래스인 합니다. `MKMapView` 사용할지 합니다 `Coordinate` 에서 파생 된 데이터는 `SampleMapAnnotation` 맵에서 주석 보기를 배치 하는 인스턴스.

`MKAnnotation` 알려진된 집합을 제공 하는 프로토콜 기능의 구현 하는 모든 개체는 소비자가 (이 경우 맵)에 대해 알 필요가 구현 세부 정보입니다. 이 간소화 가능한 주석의 다양 한 지도에 추가 합니다.

### <a name="protocols-deep-dive"></a>프로토콜에 대 한 심층 정보

이후 C# 인터페이스는 선택적 메서드를 지원 하지 않습니다, Xamarin.iOS 추상 클래스에 프로토콜을 매핑합니다. 따라서 Objective-c에서 프로토콜을 채택 하 여 수행 됩니다 Xamarin.iOS에서 프로토콜에 바인딩되는 추상 클래스에서 파생 하 고 필요한 메서드를 구현. 이러한 메서드는 클래스에서 추상 메서드로 노출 됩니다. 프로토콜에서 선택적 메서드는 바인딩됩니다의 가상 메서드는 C# 클래스입니다.

예를 들어, 부분 같습니다는 `UITableViewDataSource` 에 바인딩된 Xamarin.iOS와 프로토콜:

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

클래스는 추상 임을 유의 합니다. Xamarin.iOS는 프로토콜에서 필수/선택적 메서드를 지원 하기 위해 추상 클래스를 만듭니다.
Objective-c 프로토콜 달리 (또는 C# 인터페이스), C# 클래스는 다중 상속을 지원 하지 않습니다. 디자인을 미치면 C# 프로토콜을 사용 하 고 일반적으로 코드를 중첩 클래스입니다. 더이 문제에 대 한 대리자 섹션에서이 문서의 뒷부분에 설명 됩니다.

 `GetCell(…)` 추상 메서드를 Objective-c로 바인딩할 *선택기*, `tableView:cellForRowAtIndexPath:`의 필수 메서드는는 `UITableViewDataSource` 프로토콜입니다. 선택기는 Objective-c 용어 메서드 이름입니다. 필요에 따라 메서드를 적용할 Xamarin.iOS abstract로 선언 합니다. 다른 메서드를 `NumberOfSections(…)`에 바인딩된 `numberOfSectionsInTableview:`합니다. 이 메서드는 Xamarin.iOS 선언 가상으로 쉽게에서 재정의 하는 선택 사항 이므로 프로토콜에서는 선택적 C#입니다.

Xamarin.iOS는 수에 대 한 모든 iOS 바인딩을 처리합니다. 그러나 Objective-c에서 프로토콜을 수동으로 바인딩할 해야 하는 경우 있습니다 이렇게으로 클래스 데코레이팅는 `ExportAttribute`합니다. 이것이 자체 Xamarin.iOS에서 사용한 동일한 메서드를입니다.

Objective-c 형식 Xamarin.iOS에서 바인딩하는 방법에 대 한 자세한 내용은 문서를 참조 [Objective-c 바인딩 형식](~/ios/platform/binding-objective-c/index.md)합니다.

우리 하지 통해 프로토콜을 사용 하 여 아직 하지만 합니다. 데도 사용 iOS에서 기준으로 다음 섹션의 항목에는 Objective-c 대리자에 대 한 합니다.

## <a name="delegates"></a>대리자

iOS는 Objective-c 대리자를 사용 하 여 한 개체가 다른 작업 전달 하는 위임 패턴을 구현. 개체 작업을 수행 하는 첫 번째 개체의 대리자입니다. 개체는 특정 상황이 발생 한 후 메시지를 전송 하 여 작업 대리자를 나타냅니다. 기능적에서 메서드를 호출 하는 Objective C에서 다음과 같은 메시지를 보내는 C#입니다. 대리자 이러한 호출에 대 한 응답에서 메서드를 구현 하 고 있으므로 응용 프로그램에 기능을 제공 합니다.

대리자를 사용 하면 하위 클래스를 만들 필요 없이 클래스의 동작을 확장할 수 있습니다. 하나의 클래스를 호출할 때 다시 다른 중요 한 작업에서 발생 한 후 ios에서 응용 프로그램 대리자를 종종 사용 합니다. 예를 들어를 `MKMapView` 사용자가 지도 대 한 주석이 대리자 클래스의 작성자에 게 응용 프로그램 내에 응답 기회를 제공 하는 경우 클래스가 해당 대리자를 다시 호출 합니다. Xamarin.iOS 사용 하 여 대리자를 대리자 사용 예제를 사용 하 여이 문서의 뒷부분에이 유형의 예제를 통해 사용할 수 있습니다.

이 시점에서 궁금할 클래스에서 해당 대리자를 호출 하는 방법을 결정 하는 방법입니다. 프로토콜을 사용 하는 다른 위치입니다. 일반적으로 대리자를 사용할 수 있는 방법을 채택할 프로토콜에서 제공 됩니다.

### <a name="how-protocols-are-used-with-delegates"></a>대리자를 사용 하 여 프로토콜 사용 하는 방법

프로토콜 맵에 주석을 추가 지원 하기 위해 사용 하는 이전 방법에 대해 살펴보았습니다.
특정 이벤트 발생, 같은 지도 대 한 주석이 사용자 탭 한 후 또는 테이블의 셀을 선택 하는 후에 호출 하는 클래스에 대 한 메서드 집합을 알된 수 있도록 프로토콜도 사용 됩니다. 이러한 메서드를 구현 하는 클래스를 호출 하는 클래스의 대리자 라고 합니다.

위임을 지 원하는 클래스가 할당 되는 대리자를 구현 하는 클래스, 대리자 속성을 노출 하 여 수행 합니다. 대리자에 대 한 구현 하는 메서드는 특정 대리자 채택 하는 프로토콜에 따라 달라 집니다. 에 대 한는 `UITableView` 메서드를 구현 하는 `UITableViewDelegate` 프로토콜을에 대 한는 `UIAccelerometer` 구현 하는 메서드를 `UIAccelerometerDelegate`등 다른 클래스는 하려는 iOS 전체 노출 대리자에 대 한 합니다.

`MKMapView` 이전 예제에서 살펴본 클래스 라는 대리자를 호출 하는 속성에 다양 한 이벤트가 발생 한 후입니다. 에 대 한 대리자 `MKMapView` 유형의 `MKMapViewDelegate`합니다.
사용 하 여이 선택한 후 주석에 응답할 예로 사용 하지만 첫 번째에서 곧 보겠습니다 강력한 / 취약 한 대리자 간의 차이 설명 합니다.

### <a name="strong-delegates-vs-weak-delegates"></a>강력한 대리자 vs입니다. 약한 대리자

살펴보았습니다 지금 대리자는 강력한 대리자, 강력한 형식으로 의미 합니다. IOS에서 모든 대리자 프로토콜에 대 한 강력한 형식의 클래스를 사용 하 여 Xamarin.iOS 바인딩을 제공 합니다. 그러나 iOS에 약한 대리자의 개념이 있습니다. 특정 대리자의 Objective-c 프로토콜에 바인딩된 클래스를 하위 클래스 지정 대신 iOS 선택할 수도 있습니다 원하는 NSObject을 ExportAttribute로 메서드를 데코레이팅에서 파생 되는 모든 클래스에서 직접 프로토콜 메서드를 바인딩하려면 다음 적절 한 선택기를 제공 합니다.
이 방법을 사용 하는 경우 대리자 속성 대신 WeakDelegate 속성에 클래스의 인스턴스를 할당 합니다. 약한 대리자 다른 상속 계층을 다운 대리자 클래스를 사용할 수 있는 유연성을 제공 합니다. 강력한 / 취약 한 대리자를 사용 하는 Xamarin.iOS 예제를 살펴보겠습니다.

### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin.iOS를 사용 하 여 대리자를 사용 하는 예제

예제에서 주석을 탭 사용자에 대 한 응답에서 코드를 실행 하려면에서는 서브클래싱 할 수 있습니다 `MKMapViewDelegate` 인스턴스를 할당 합니다 `MKMapView`의 `Delegate` 속성입니다. `MKMapViewDelegate` 프로토콜 선택적 메서드만 포함 합니다.
따라서 모든 메서드는 가상의 Xamarin.iOS에서이 프로토콜에 바인딩되어 있는 `MKMapViewDelegate` 클래스입니다. 주석의 선택 하면를 `MKMapView` 인스턴스에 보냅니다는 `mapView:didSelectAnnotationView:` 해당 대리자에는 메시지. 이 처리 하기 Xamarin.iOS에서 재정의 해야 합니다 `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` 같이 MKMapViewDelegate 서브 클래스에서 메서드:

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

위에 표시 된 SampleMapDelegate 클래스가 들어 있는 컨트롤러의 중첩 클래스로 구현 되는 `MKMapView` 인스턴스. Objective-c에서 자주 볼 수는 컨트롤러 클래스 내에서 직접 여러 프로토콜을 채택 합니다. 그러나 프로토콜 Xamarin.iOS에서 클래스에 바인딩되어 있는 하므로 강력한 형식의 대리자를 구현 하는 클래스 중첩 클래스로 일반적으로 포함 됩니다.

현재 위치에서 대리자 클래스 구현에서는 하기만 하면 컨트롤러에서 대리자의 인스턴스를 인스턴스화하고에 할당 합니다 `MKMapView`의 `Delegate` 다음과 같이 속성:

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

약한 대리자에 동일한 작업을 수행 하는 데 사용 하려면 클래스에서 파생 되는 직접 메서드를 바인딩하고 해야 `NSObject` 에 할당 하는 `WeakDelegate` 의 속성을 `MKMapView`. 이후 합니다 `UIViewController` 클래스에서 최종적으로 파생 됩니다 `NSObject` (예: Objective-c의에서 모든 클래스 CocoaTouch), 간단히 바인딩된 메서드를 구현할 수 있습니다 `mapView:didSelectAnnotationView:` 컨트롤러에서 직접 컨트롤러를 할당 하 고 `MKMapView`의 `WeakDelegate`, 추가 중첩된 클래스에 대 한 필요성을 방지 합니다. 아래 코드에는이 방법을 보여 줍니다.

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

이 코드를 실행 하는 경우 응용 프로그램에 강력한 형식의 대리자 버전을 실행 하는 경우와 동일 하 게 동작 합니다. 이 코드에서 장점은 약한 대리자가 강력한 형식의 대리자를 사용 했을 때 생성 된 추가 클래스를 만드는 필요 하지 않습니다. 그러나이 형식 안전성 저하 됩니다. 에 전달 된 선택기를 실수를 하는 경우는 `ExportAttribute`, 런타임 시까지 확인 하지 않습니다.

### <a name="events-and-delegates"></a>이벤트 및 대리자

대리자는.NET 이벤트를 사용 하는 방식와 유사 하 게 iOS에서 콜백에 대 한 사용 됩니다. IOS 수 있도록 Api 및 Objective-c 대리자를 사용 하는 방법 보일 더.NET Xamarin.iOS 대리자 iOS에서 사용 되는 여러 위치에서.NET 이벤트를 노출 합니다.

이전 구현 예를 들어 여기서는 `MKMapViewDelegate` 응답할 선택한 주석이 또한 Xamarin.iOS에서.NET 이벤트를 사용 하 여 구현할 수입니다. 이벤트는 정의 하는 경우 `MKMapView` 호출 `DidSelectAnnotationView`합니다. 했을 때를 `EventArgs` 형식의 서브 클래스 `MKMapViewAnnotationEventsArgs`합니다. 합니다 `View` 속성의 `MKMapViewAnnotationEventsArgs` 주석 보기에 대 한 참조를 제공, 동일 하 게 구현 계속할 수 없습니다는에서 사용 했던 앞서 설명한 것 처럼 여기:

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

이 문서에서는 Xamarin.iOS에서 이벤트, 프로토콜 및 대리자를 사용 하는 방법을 설명 합니다. Xamarin.iOS는 컨트롤에 대 한 일반.NET 스타일 이벤트를 노출 하는 방법에 대해 살펴보았습니다.
Objective-c 프로토콜을 포함 하는 방법을 다른 알아본 다음 C# 인터페이스 및 Xamarin.iOS 하을 사용 하는 방법입니다. 마지막으로, Xamarin.iOS 관점에서 Objective-c 대리자 검사 했습니다. Xamarin.iOS에서 지 원하는 방법 모두 강력한 및 약하게 형식화 된 대리자 및.NET 이벤트 대리자 메서드를 바인딩하는 방법에 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [프로토콜, 대리자 및 이벤트 (샘플)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Objective-c 형식 바인딩](~/ios/platform/binding-objective-c/index.md)
- [Objective C 프로그래밍 언어](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4에서에서 사용자 인터페이스 디자인](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS에 대 한 핵심 응용 프로그램 역량](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
