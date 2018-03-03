---
title: "IOS 용 Xamarin 디자이너에서 사용자 지정 컨트롤"
description: "IOS 용 Xamarin 디자이너 프로젝트에서 생성 되거나 Xamarin 구성 요소 저장소와 같은 외부 소스에서 참조 된 사용자 지정 컨트롤 렌더링을 지원 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 83ec11ab6a17717dd9556122745afc8d87959186
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>IOS 용 Xamarin 디자이너에서 사용자 지정 컨트롤

_IOS 용 Xamarin 디자이너 프로젝트에서 생성 되거나 Xamarin 구성 요소 저장소와 같은 외부 소스에서 참조 된 사용자 지정 컨트롤 렌더링을 지원 합니다._

IOS 용 Xamarin 디자이너 응용 프로그램의 사용자 인터페이스를 시각화에 대 한 강력한 도구 이며 대부분 iOS 뷰 및 컨트롤러 보기에 대 한 지원 편집 WYSIWYG를 제공 합니다. 응용 프로그램에서 iOS에 기본 제공 되는 것을 확장 하는 사용자 지정 컨트롤을 포함할 수도 있습니다. 염두에서 몇 가지 지침을 사용 하 여 이러한 사용자 지정 컨트롤 작성을 하는 경우도 렌더링할 수 있도록 iOS 디자이너, 한도 보다 풍부한 편집 환경을 제공 하 여 합니다. 이 문서는 이러한 지침을 참조 합니다.

## <a name="requirements"></a>요구 사항

다음 요구 사항을 모두 만족 하는 컨트롤 디자인 화면에 렌더링 됩니다.

1.  직접 또는 간접 하위 클래스는 [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) 또는 [UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller)합니다. 다른 [NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/) 하위 클래스는 디자인 화면에 아이콘으로 표시 됩니다.
2.  에 [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) 없습니다. 목표에 공개할 수
3.  있기 [필요한 IntPtr 생성자](~/ios/internals/api-design/index.md)합니다.
4.  구현 중 하나는 [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) 되었거나 인터페이스는 [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/) True로 설정 합니다.

위의 요구 사항을 충족 하는 코드에 정의 된 컨트롤은 시뮬레이터에 대 한 포함 하는 해당 프로젝트를 컴파일할 때 디자이너에 표시 됩니다. 기본적으로 모든 사용자 지정 컨트롤에 표시 됩니다는 **사용자 지정 구성 요소** 의 섹션은 **도구 상자**합니다. 그러나 [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/) 다른 섹션을 지정 하려면 사용자 지정 컨트롤의 클래스에 적용할 수 있습니다.

디자이너에서 타사 Objective C 라이브러리를 로드 하는 것을 지원 하지 않습니다.

## <a name="custom-properties"></a>사용자 지정 속성

사용자 지정 컨트롤에 의해 선언 된 속성은 다음 조건이 충족 될 경우 속성 패널에 표시 됩니다.

1.  속성에 공용 getter 및 setter에 있습니다.
1.  속성에는 [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/) 및 [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/) True로 설정 합니다.
1.  속성 형식이 숫자 형식, 열거형 형식, string, bool, [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/), [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/), 또는 [UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/)합니다. 이 목록은 지원 되는 형식 앞으로 확장 될 수 있습니다.


속성으로도 데코레이팅 할 수 있습니다는 [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/) 에 대 한 속성 패널에 표시 되는 레이블을 지정할 수 있습니다.

## <a name="initialization"></a>초기화

에 대 한 `UIViewController` 서브 클래스를 사용할지는 [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/) 디자이너에서 만든 뷰에 종속 되는 코드에 대 한 메서드.

에 대 한 `UIView` 및 기타 `NSObject` 하위 클래스는 [AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/) 메서드는 레이아웃 파일에서 로드 된 후 사용자 지정 컨트롤의 초기화를 수행 하기 가장 좋은 위치입니다. 컨트롤의 생성자를 실행 하기 전에 설정 됩니다 때 속성 패널에서 설정 하는 모든 사용자 지정 속성이 설정 되지 것입니다 때문에 이것이 `AwakeFromNib` 호출 됩니다.


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

컨트롤을 코드에서 직접 만들어야 설계 되어, 라면 다음과 같은 일반적인 초기화 코드를 있는 메서드를 만들려면:

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

언제, 어디에 덮어쓰지 iOS 디자이너 내 설정 된 값에 대 한 사용자 지정 구성 요소에서 디자인할 수 있는 속성을 초기화에 주의 해야 합니다. 예를 들어, 다음 코드를 수행 합니다.

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

`CustomView` 구성 요소에서 노출 한 `Counter` iOS 디자이너 내부 개발자가 설정 될 수 있는 속성입니다. 그러나 값 디자이너 내부에 어떤 값이 설정에 관계 없이는 `Counter` 속성 영 (0)은 항상 있습니다. 이유는 다음과 같습니다.

-  인스턴스는 `CustomControl` 스토리 보드 파일에서을 확장 합니다.
-  IOS 디자이너에서 수정 하는 모든 속성은 설정 됩니다 (의 값을 설정 하는 등의 `Counter` 를 2 (2), 예:).
-  `AwakeFromNib` 메서드를 실행 하 고 구성 요소의 호출 `Initialize` 메서드.
-  내부 `Initialize` 의 값은 `Counter` 속성 영 (0)으로 다시 설정 됩니다.


위의 문제를 해결 하려면 시작 하거나는 `Counter` 속성 다른 위치 (예: 구성 요소의 생성자에서와 같이) 재정의 하지 않는 또는 `AwakeFromNib` 메서드와 호출 `Initialize` 구성 요소가 무엇 외부에서 더 이상 없습니다 초기화 해야 하는 경우 해당 생성자에서 현재 처리 되는 합니다.

## <a name="design-mode"></a>디자인 모드

디자인 화면에서 사용자 지정 컨트롤 몇 가지 제한 사항을 준수 해야 합니다.

-  앱 번들 리소스는 디자인 모드에서 사용할 수 없습니다. 이미지를 통해 로드 될 때 사용할 [UIImage 메서드](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM) 합니다.
-  디자인 모드에서 웹 요청 등의 비동기 작업을 수행 되어야 합니다. 디자인 화면 애니메이션 또는 컨트롤의 UI 다른 비동기 업데이트를 지원 하지 않습니다.


사용자 지정 컨트롤을 구현할 수 [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/) 사용 및는 [서](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/) 속성을 디자인 화면에 있는지 확인 합니다. 이 예제에서는 레이블이 런타임 시의 디자인 화면에 "Runtime" "디자인 모드"가 표시 됩니다.

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

항상 확인 해야는 `Site` 속성에 대 한 `null` 해당 멤버가 액세스해보십시오. 경우 `Site` 은 `null`, 안전 컨트롤 디자이너에서 실행 되 고 있지 가정 합니다.
디자인 모드에서 `Site` 컨트롤의 생성자가 실행 된 후 및 하기 전에 설정 됩니다 `AwakeFromNib` 호출 됩니다.

## <a name="debugging"></a>디버깅

위의 요구 사항을 충족 하는 컨트롤을 도구 상자에 표시 하 고 화면에 렌더링 됩니다.
컨트롤 렌더링 되지 않습니다 컨트롤이 나 해당 종속성 중 하나에서 버그 확인 합니다.

디자인 화면 줄이면서 다른 컨트롤을 렌더링 하도록 개별 컨트롤에서 발생 한 예외를 catch 할 수 경우가 많습니다. 결함이 있는 컨트롤을 빨간색 자리 표시자로 바뀌고 느낌표 아이콘을 클릭 하 여 예외 추적을 볼 수 있습니다.

 ![](ios-designable-controls-overview-images/exception-box.png "빨간색 자리 표시자 및 예외 세부 사항으로는 결함이 있는 컨트롤")

디버그 기호를 컨트롤에 대해 사용할 수 있는 경우에 추적 파일 이름과 줄 번호 갖습니다. 두 번 스택 추적의 줄을 클릭 하면 해당 줄의 소스 코드에서으로 이동 합니다.

디자이너는 결함이 있는 컨트롤을 격리할 수 없습니다, 디자인 화면 위쪽에 경고 메시지가 표시 됩니다.

 ![](ios-designable-controls-overview-images/info-bar.png "디자인 화면 맨 위에 있는 경고 메시지")

전체 렌더링에 결함이 있는 컨트롤을 고정 하거나 디자인 화면에서 제거 하면 다시 시작 됩니다.

## <a name="summary"></a>요약

이 문서 작성 및 iOS 디자이너에서 사용자 지정 컨트롤의 응용 프로그램을 도입 했습니다. 먼저 컨트롤 디자인 화면에 렌더링 되어야 하 고 속성 패널에서 사용자 지정 속성을 노출 하기 위해 충족 해야 하는 요구 사항을 설명 합니다. 코드 숨김-서 속성 및 컨트롤의 초기화에 다음 검토 합니다. 어떤 일이 생기 설명 마지막으로 예외를 throw 하는 경우 및이 문제를 해결 하는 방법입니다.