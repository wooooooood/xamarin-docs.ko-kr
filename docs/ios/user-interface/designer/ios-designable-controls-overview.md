---
title: IOS 용 Xamarin 디자이너에서 사용자 지정 컨트롤
description: IOS 용 Xamarin 디자이너 프로젝트에서 생성 하거나 참조 Xamarin Component Store와 같은 외부 원본에서 사용자 지정 컨트롤 렌더링을 지원 합니다.
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: c409fcc018379401c1ab40573495da12a8220c5a
ms.sourcegitcommit: a1a58afea68912c79d16a3f64de9a0c1feb2aeb4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55233668"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>IOS 용 Xamarin 디자이너에서 사용자 지정 컨트롤

_IOS 용 Xamarin 디자이너 프로젝트에서 생성 하거나 참조 Xamarin Component Store와 같은 외부 원본에서 사용자 지정 컨트롤 렌더링을 지원 합니다._

IOS 용 Xamarin 디자이너 응용 프로그램의 사용자 인터페이스 시각화를 위한 강력한 도구 이며 WYSIWYG 편집 대부분의 iOS 뷰와 뷰 컨트롤러에 대 한 지원을 제공 합니다. 앱에서 iOS에 내장 된 것을 확장 하는 사용자 지정 컨트롤을 포함할 수도 있습니다. 이러한 사용자 지정 컨트롤 몇 가지 지침을 염두에서에 두고 작성 된, 경우도 렌더링할 수 있도록 ios 디자이너, 풍부한 편집 환경을 제공 합니다. 이 문서에서는 이러한 지침에 살펴봅니다.

## <a name="requirements"></a>요구 사항

다음 요구 사항을 모두 충족 하는 컨트롤이 디자인 화면에서 렌더링 될:

1.  직접 또는 간접 하위 클래스는 [UIView](xref:UIKit.UIView) 하거나 [UIViewController](xref:UIKit.UIViewController)합니다. 다른 [NSObject](xref:Foundation.NSObject) 서브 클래스는 디자인 화면에서 아이콘으로 표시 됩니다.
2.  에 [RegisterAttribute](xref:Foundation.RegisterAttribute) 목표 C에 노출 하려면
3.  있기 [필요한 IntPtr 생성자](~/ios/internals/api-design/index.md)합니다.
4.  하거나 구현 합니다 [IComponent](xref:System.ComponentModel.IComponent) 되었거나 인터페이스는 [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute) True로 설정 합니다.

시뮬레이터가 포함 된 프로젝트를 컴파일할 때 위의 요구 사항을 충족 하는 코드에 정의 된 컨트롤 디자이너에 표시 됩니다. 기본적으로 모든 사용자 지정 컨트롤에 표시 됩니다는 **사용자 지정 구성 요소** 섹션을 **도구 상자**합니다. 그러나 [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute) 다른 섹션을 지정 하는 사용자 지정 컨트롤의 클래스에 적용할 수 있습니다.

디자이너에서 타사 Objective-c 라이브러리를 로드 하는 것을 지원 하지 않습니다.

## <a name="custom-properties"></a>사용자 지정 속성

다음 조건이 충족 될 경우 사용자 지정 컨트롤에서 선언 된 속성 [속성] 패널에 표시 됩니다.

1.  속성이 공용 getter 및 setter.
1.  속성에는 [ExportAttribute](xref:Foundation.ExportAttribute) 뿐만 [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute) True로 설정 합니다.
1.  속성 형식은 숫자 형식, 열거형, string, bool [SizeF](xref:System.Drawing.SizeF)를 [UIColor](xref:UIKit.UIColor), 또는 [UIImage](xref:UIKit.UIImage)합니다. 이 목록은 지원 되는 형식 나중에 확장할 수 있습니다.


속성 지정 될 수 있습니다는 [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute) 에 대 한 [속성] 패널에 표시 되는 레이블을 지정 합니다.

## <a name="initialization"></a>초기화

에 대 한 `UIViewController` 서브 클래스를 사용할지를 [ViewDidLoad](xref:UIKit.UIViewController.ViewDidLoad) 디자이너에서 만든 보기에 종속 된 코드가 메서드.

에 대 한 `UIView` 및 기타 `NSObject` 하위 클래스는 [AwakeFromNib](xref:Foundation.NSObject.AwakeFromNib) 메서드는 레이아웃 파일에서 로드 된 후 사용자 지정 컨트롤의 초기화를 수행 하는 데 좋은 위치입니다. 컨트롤의 생성자를 실행 하기 전에 설정할 수는 경우 속성 패널에서 설정 된 사용자 지정 속성을 설정할 수는 이것이 `AwakeFromNib` 라고 합니다.


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

또한 코드에서 직접 만들 디자인 된, 하는 경우에 다음과 같은 일반 초기화 코드에서 포함 된 메서드를 만들려면는 것이 좋습니다.

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

시기를 덮어쓰지 iOS 디자이너 내에서 설정 된 값에 대 한 사용자 지정 구성 요소에서 디자인할 수 있는 속성을 초기화할 수 있는 위치에 주의 해야 합니다. 예를 들어 다음 코드를 수행 합니다.

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

`CustomView` 구성 요소에서 노출 된 `Counter` iOS 디자이너 내에서 개발자가 설정할 수 있는 속성입니다. 그러나 변수의 디자이너 내에서 어떤 값이 설정에 관계 없이는 `Counter` 속성 값은 항상 0 (0). 이유는 다음과 같습니다.

-  인스턴스는 `CustomControl` 스토리 보드 파일에서을 확장 합니다.
-  IOS 디자이너에서 수정한 모든 속성은 설정 됩니다 (값을 설정 하는 등의 `Counter` 에 2 (2), 예를 들어).
-  합니다 `AwakeFromNib` 메서드를 실행 하 고 구성 요소로 호출 `Initialize` 메서드.
-  내 `Initialize` 의 값을 `Counter` 속성 영 (0)으로 다시 설정 됩니다.


위의 문제를 해결 하려면 초기화 하거나 합니다 `Counter` 속성이 다른 곳 (예: 구성 요소의 생성자) 재정의 하지 않는 또는 합니다 `AwakeFromNib` 메서드를 호출 `Initialize` 구성 요소 항목 외부의 추가 초기화가 필요한 경우 생성자에서 현재 처리 중인 됩니다.

## <a name="design-mode"></a>디자인 모드

디자인 화면에서 사용자 지정 컨트롤을 몇 가지 제한 사항을 준수 해야 합니다.

-  앱 번들 리소스는 디자인 모드에서 사용할 수 없습니다. 통해 로드 된 경우에 사용할 수 있는 이미지가 [UIImage 메서드](xref:UIKit.UIImage) 합니다.
-  디자인 모드에서 웹 요청과 같은 비동기 작업을 수행 되어야 합니다. 디자인 화면 애니메이션 또는 컨트롤의 UI 다른 비동기 업데이트를 지원 하지 않습니다.


사용자 지정 컨트롤을 구현할 수 있습니다 [IComponent](xref:System.ComponentModel.IComponent) 사용 하는 [서](xref:System.ComponentModel.ISite.DesignMode) 속성을 디자인 화면의 인지 확인 합니다. 이 예제에서는 레이블이 런타임에 런타임은 고 디자인 화면에서 "디자인 모드"가 표시 됩니다.

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
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

항상 확인 해야 합니다 `Site` 속성에 대 한 `null` 해당 멤버 중 하나에 액세스해보십시오. 하는 경우 `Site` 는 `null`, 안전 컨트롤 디자이너에 실행 되지 않는 것으로 가정 합니다.
디자인 모드로 `Site` 컨트롤의 생성자가 실행 된 후 및 하기 전에 설정할 `AwakeFromNib` 라고 합니다.

## <a name="debugging"></a>디버깅

위의 요구 사항을 충족 하는 컨트롤을 도구 상자에서 표시 하 고 화면에 렌더링 됩니다.
컨트롤 렌더링 되지 않습니다 하는 경우 컨트롤이 나 해당 종속성 중 하나에서 버그 확인 합니다.

디자인 화면 계속 다른 컨트롤을 렌더링 하는 동안 개별 컨트롤에서 throw 된 예외를 catch 종종 수 있습니다. 잘못 된 컨트롤 빨간색 자리 표시자로 바뀌고 느낌표 아이콘을 클릭 하 여 예외 추적을 볼 수 있습니다.

 ![](ios-designable-controls-overview-images/exception-box.png "빨간색 자리 표시자 및 예외 세부 사항으로는 결함이 있는 컨트롤")

디버그 기호를 컨트롤에 대해 사용할 수 있는 파일 이름과 줄 번호를 추적 해야 합니다. 두 번 스택 추적의 줄을 클릭 하면 소스 코드에서 해당 줄으로 이동 합니다.

디자이너는 잘못 된 컨트롤을 격리할 수를 디자인 화면의 맨 위에 있는 경고 메시지가 표시 됩니다.

 ![](ios-designable-controls-overview-images/info-bar.png "디자인 화면의 맨 위에 있는 경고 메시지")

전체 렌더링에는 잘못 된 컨트롤을 고정 하거나 디자인 화면에서 제거 하면 다시 시작 됩니다.

## <a name="summary"></a>요약

이 문서 작성 및 iOS 디자이너에서 사용자 지정 컨트롤의 응용 프로그램을 도입 했습니다. 먼저 컨트롤 디자인 화면에 렌더링 되 고 [속성] 패널에서 사용자 지정 속성을 노출 하기 위해 충족 해야 하는 요구 사항을 설명 합니다. 다음 코드 숨김-서 속성을 컨트롤의 초기화에서 조회 합니다. 어떤 일이 생기 설명 마지막으로 예외 throw 되는 시기 및이 문제를 해결 하는 방법입니다.