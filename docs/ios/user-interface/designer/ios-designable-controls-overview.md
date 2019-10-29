---
title: Xamarin Designer for iOS의 사용자 지정 컨트롤
description: Xamarin Designer for iOS는 프로젝트에서 만들었거나 Xamarin 구성 요소 저장소와 같은 외부 소스에서 참조 되는 사용자 지정 컨트롤 렌더링을 지원 합니다.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: e8c38ec407d13a99e2990a6d4cf39b5a23728b1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003972"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>Xamarin Designer for iOS의 사용자 지정 컨트롤

_Xamarin Designer for iOS는 프로젝트에서 만들었거나 Xamarin 구성 요소 저장소와 같은 외부 소스에서 참조 되는 사용자 지정 컨트롤 렌더링을 지원 합니다._

Xamarin Designer for iOS는 응용 프로그램의 사용자 인터페이스를 시각화 하 고 대부분의 iOS 뷰 및 뷰 컨트롤러에 대 한 WYSIWYG 편집 지원을 제공 하기 위한 강력한 도구입니다. 앱에는 iOS로 빌드된 항목을 확장 하는 사용자 지정 컨트롤도 포함 될 수 있습니다. 이러한 사용자 지정 컨트롤에 대 한 지침을 염두에 두면 iOS 디자이너에서 렌더링 하 여 훨씬 더 풍부한 편집 환경을 제공할 수도 있습니다. 이 문서에서는 이러한 지침을 살펴봅니다.

## <a name="requirements"></a>요구 사항

다음 요구 사항을 모두 충족 하는 컨트롤이 디자인 화면에 렌더링 됩니다.

1. [Uiview](xref:UIKit.UIView) 또는 [uiviewcontroller](xref:UIKit.UIViewController)의 직접 또는 간접 서브 클래스입니다. 다른 [Nsobject](xref:Foundation.NSObject) 서브 클래스는 디자인 화면에 아이콘으로 표시 됩니다.
2. 이 클래스에는 목표-C에 노출 하는 [Registerattribute](xref:Foundation.RegisterAttribute) 가 있습니다.
3. [필요한 IntPtr 생성자](~/ios/internals/api-design/index.md)가 있습니다.
4. [IComponent](xref:System.ComponentModel.IComponent) 인터페이스를 구현 하거나 [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) 를 True로 설정 합니다.

위의 요구 사항을 충족 하는 코드에 정의 된 컨트롤은 해당 프로젝트를 시뮬레이터에 대해 컴파일할 때 디자이너에 표시 됩니다. 기본적으로 모든 사용자 지정 컨트롤은 **도구 상자**의 **사용자 지정 구성 요소** 섹션에 표시 됩니다. 그러나 [Categoryattribute](xref:System.ComponentModel.CategoryAttribute) 를 사용자 지정 컨트롤의 클래스에 적용 하 여 다른 섹션을 지정할 수 있습니다.

디자이너는 타사 목표-C 라이브러리 로드를 지원 하지 않습니다.

## <a name="custom-properties"></a>사용자 지정 속성

다음 조건이 충족 되 면 사용자 지정 컨트롤에 의해 선언 된 속성이 속성 패널에 표시 됩니다.

1. 속성에는 public getter 및 setter가 있습니다.
1. 속성에는 [Exportattribute](xref:Foundation.ExportAttribute) 뿐만 아니라 [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) 가 True로 설정 되어 있습니다.
1. 속성 형식은 숫자 형식, 열거형 형식, 문자열, bool, [SizeF](xref:System.Drawing.SizeF), [UIColor](xref:UIKit.UIColor)또는 [uiimage](xref:UIKit.UIImage)입니다. 지원 되는 형식 목록은 나중에 확장할 수 있습니다.

속성은 속성 패널에서 표시 되는 레이블을 지정 하기 위해 [Displaynameattribute](xref:System.ComponentModel.DisplayNameAttribute) 로 데코레이팅 될 수도 있습니다.

## <a name="initialization"></a>초기화

`UIViewController` 서브 클래스의 경우 디자이너에서 만든 뷰에 따라 달라 지는 코드에 [ViewDidLoad](xref:UIKit.UIViewController.ViewDidLoad) 메서드를 사용 해야 합니다.

`UIView` 및 기타 `NSObject` 서브 클래스의 경우, [AwakeFromNib](xref:Foundation.NSObject.AwakeFromNib) 메서드는 레이아웃 파일에서 로드 된 후 사용자 지정 컨트롤의 초기화를 수행 하는 데 권장 되는 장소입니다. 이는 컨트롤의 생성자가 실행 될 때 속성 패널에 설정 된 사용자 지정 속성은 설정 되지 않지만 `AwakeFromNib`가 호출 되기 전에 설정 되기 때문입니다.

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

또한 컨트롤을 코드에서 직접 만들 수 있도록 디자인 하는 경우 다음과 같이 일반적인 초기화 코드를 포함 하는 메서드를 만들 수 있습니다.

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>속성 초기화 및 AwakeFromNib

IOS 디자이너 내에서 설정 된 값을 덮어쓰지 않도록 사용자 지정 구성 요소에서 디자인 가능한 속성을 초기화 하는 시기와 위치에 대 한 주의를 기울여야 합니다. 예를 들어 다음 코드를 사용 합니다.

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView` 구성 요소는 개발자가 iOS 디자이너 내에서 설정할 수 있는 `Counter` 속성을 노출 합니다. 그러나 디자이너 내에 설정 된 값에 관계 없이 `Counter` 속성 값은 항상 0이 됩니다. 이유는 다음과 같습니다.

- `CustomControl` 인스턴스는 스토리 보드 파일에서 팽창 됩니다.
- IOS 디자이너에서 수정 된 모든 속성 (예: `Counter`의 값을 2로 설정)이 설정 됩니다 (예:).
- `AwakeFromNib` 메서드가 실행 되 고 구성 요소의 `Initialize` 메서드가 호출 됩니다.
- `Initialize` 내에서 `Counter` 속성의 값이 0으로 다시 설정 됩니다.

위의 상황을 해결 하려면 `Counter` 속성을 다른 곳에서 초기화 하거나 (예: 구성 요소 생성자에서) `AwakeFromNib` 메서드를 재정의 하지 않고 구성 요소에 현재 있는 것과 같은 추가 초기화가 필요 하지 않은 경우 `Initialize`를 호출 합니다. 생성자에 의해 처리 됩니다.

## <a name="design-mode"></a>디자인 모드

디자인 화면에서 사용자 지정 컨트롤은 몇 가지 제한 사항을 준수 해야 합니다.

- 응용 프로그램 번들 리소스는 디자인 모드에서 사용할 수 없습니다. [Uiimage 메서드](xref:UIKit.UIImage) 를 통해 로드할 때 이미지를 사용할 수 있습니다.
- 웹 요청과 같은 비동기 작업은 디자인 모드에서 수행 하면 안 됩니다. 디자인 화면은 컨트롤의 UI에 대 한 애니메이션 또는 기타 비동기 업데이트를 지원 하지 않습니다.

사용자 지정 컨트롤은 [IComponent](xref:System.ComponentModel.IComponent) 를 구현 하 고 [designmode](xref:System.ComponentModel.ISite.DesignMode) 속성을 사용 하 여 디자인 화면에 있는지 확인할 수 있습니다. 이 예제에서 레이블은 디자인 화면에 "디자인 모드"를 표시 하 고 런타임에는 "런타임"을 표시 합니다.

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null && Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

해당 멤버에 액세스 하기 전에 항상 `null`에 대 한 `Site` 속성을 확인 해야 합니다. `Site` `null`경우 디자이너에서 컨트롤이 실행 되지 않는다고 가정 하는 것이 안전 합니다.
디자인 모드에서 `Site`는 컨트롤의 생성자가 실행 된 후 `AwakeFromNib` 호출 되기 전에 설정 됩니다.

## <a name="debugging"></a>디버깅

위의 요구 사항을 충족 하는 컨트롤이 도구 상자에 표시 되 고 화면에 렌더링 됩니다.
컨트롤이 렌더링 되지 않는 경우 컨트롤 또는 해당 종속성 중 하나에서 버그를 확인 합니다.

디자인 화면에서는 다른 컨트롤을 계속 렌더링 하는 동안 개별 컨트롤에서 throw 된 예외를 종종 catch 할 수 있습니다. 잘못 된 컨트롤은 빨간색 자리 표시자로 바뀌고 느낌표 아이콘을 클릭 하 여 예외 추적을 볼 수 있습니다.

 ![](ios-designable-controls-overview-images/exception-box.png "A faulty control as red placeholder and the exception details")

컨트롤에 디버그 기호를 사용할 수 있는 경우 추적에는 파일 이름과 줄 번호가 있습니다.
스택 추적에서 줄을 두 번 클릭 하면 소스 코드에서 해당 줄로 이동 합니다.

디자이너에서 잘못 된 컨트롤을 격리할 수 없는 경우 디자인 화면 위쪽에 경고 메시지가 표시 됩니다.

 ![](ios-designable-controls-overview-images/info-bar.png "A warning message at the top of the design surface")

오류가 발생 한 컨트롤이 디자인 화면에서 수정 되거나 제거 되 면 전체 렌더링이 다시 시작 됩니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 디자이너에서 사용자 지정 컨트롤을 만들고 적용 하는 방법을 소개 했습니다. 먼저 컨트롤이 디자인 화면에서 렌더링 되어야 하는 요구 사항을 설명 하 고 속성 패널에서 사용자 지정 속성을 노출 합니다. 그런 다음 컨트롤의 코드를 초기화 하는 코드와 DesignMode 속성을 살펴보았습니다. 마지막으로 예외가 throw 될 때 발생 하는 상황과이를 해결 하는 방법을 설명 했습니다.
