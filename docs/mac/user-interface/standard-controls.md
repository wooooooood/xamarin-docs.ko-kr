---
title: Xamarin.ios의 표준 컨트롤
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤과 같은 표준 AppKit 컨트롤을 사용 하는 방법을 설명 합니다. Interface Builder를 사용 하 여 인터페이스에 추가 하 고 코드에서 상호 작용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 9a6e302885570f35bb8323a5504cc9a4d8256ac1
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572106"
---
# <a name="standard-controls-in-xamarinmac"></a>Xamarin.ios의 표준 컨트롤

_이 문서에서는 Xamarin.ios 응용 프로그램에서 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤과 같은 표준 AppKit 컨트롤을 사용 하는 방법을 설명 합니다. Interface Builder를 사용 하 여 인터페이스에 추가 하 고 코드에서 상호 작용 하는 방법을 설명 합니다._

Xamarin.ios 응용 프로그램에서 c # 및 .NET으로 작업 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자가 동일한 appkit 컨트롤에 액세스할 수 있습니다. Xamarin.ios는 Xcode와 직접 통합 되므로 Xcode의 _Interface Builder_ 를 사용 하 여 Appkit 컨트롤을 만들고 유지 관리 하거나 (필요에 따라 c # 코드에서 직접 만들 수 있습니다.)

AppKit 컨트롤은 Xamarin.ios 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 UI 요소입니다. 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤과 같은 요소로 구성 되며 사용자가 조작할 때 인스턴트 작업 또는 표시 되는 결과가 발생 합니다.

[![](standard-controls-images/intro01.png "The example app main screen")](standard-controls-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 AppKit 컨트롤 작업의 기본 사항을 다룹니다. 이 문서에서 사용할 주요 개념 및 기술에 대해 설명 하는 대로 [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 소개 하 고 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하는 것이 좋습니다.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서에서 c [# 클래스/메서드를 목표로](~/mac/internals/how-it-works.md) 표시 하는 방법에 대해 살펴볼 수 있습니다 `Register` . c `Export` # 클래스를 객관적인 개체 및 UI 요소에 연결 하는 데 사용 되는 및 명령에 대해서도 설명 합니다.

<a name="Introduction_to_Controls_and_Views"></a>

## <a name="introduction-to-controls-and-views"></a>컨트롤 및 뷰 소개

macOS (이전의 Mac OS X)는 AppKit 프레임 워크를 통해 사용자 인터페이스 컨트롤의 표준 집합을 제공 합니다. 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤과 같은 요소로 구성 되며 사용자가 조작할 때 인스턴트 작업 또는 표시 되는 결과가 발생 합니다.

모든 AppKit 컨트롤에는 대부분의 용도에 적합 한 표준 기본 제공 모양이 있습니다. 일부 경우에는 창 프레임 영역에서 사용할 대체 모양이 나 사이드바 영역 또는 알림 센터 위젯에 같은 _Vibrance 효과_ 컨텍스트를 지정 합니다.

Apple AppKit 컨트롤을 사용할 때 다음 지침을 제안 합니다.

- 동일한 뷰에서 컨트롤 크기를 혼합 하지 마세요.
- 일반적으로 컨트롤의 크기를 세로로 조정 하지 마십시오.
- 컨트롤 내에서 시스템 글꼴과 적절 한 텍스트 크기를 사용 합니다.
- 컨트롤 사이에 적절 한 간격을 사용 합니다.

자세한 내용은 Apple의 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [컨트롤 및 뷰 정보](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) 섹션을 참조 하세요.

<a name="Using_Controls_in_a_Window_Frame"></a>

### <a name="using-controls-in-a-window-frame"></a>창 프레임에서 컨트롤 사용

Windows 프레임 영역에 포함할 수 있도록 하는 표시 스타일을 포함 하는 AppKit 컨트롤의 하위 집합이 있습니다. 예를 들어 메일 응용 프로그램의 도구 모음을 참조 하세요.

[![](standard-controls-images/mailapp.png "A Mac Window frame")](standard-controls-images/mailapp.png#lightbox)

- 스타일을 사용 하는 **원형 질감 단추** `NSButton` `NSTexturedRoundedBezelStyle` 입니다.
- 스타일이 인 **질감으로 분할 된 분할 된 컨트롤** `NSSegmentedControl` `NSSegmentStyleTexturedRounded` 입니다.
- 스타일이 인 **질감으로 분할 된 분할 된 컨트롤** `NSSegmentedControl` `NSSegmentStyleSeparated` 입니다.
- 의 스타일을 사용 하는 **원형 질감 팝업 메뉴** `NSPopUpButton` `NSTexturedRoundedBezelStyle` 입니다.
- **원형 질감 드롭다운 메뉴** - `NSPopUpButton` 스타일을 사용 하는 `NSTexturedRoundedBezelStyle` 입니다.
- **검색 표시줄** -A `NSSearchField` .

Apple 창 프레임에서 AppKit 컨트롤을 사용할 때 다음 지침을 제안 합니다.

- 창 본문에서 창 프레임 특정 컨트롤 스타일을 사용 하지 않습니다.
- 창 프레임에서 창 본문 컨트롤이 나 스타일을 사용 하지 않습니다.

자세한 내용은 Apple의 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [컨트롤 및 뷰 정보](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) 섹션을 참조 하세요.

<a name="Creating_a_User_Interface_in_Interface_Builder"></a>

## <a name="creating-a-user-interface-in-interface-builder"></a>Interface Builder에서 사용자 인터페이스 만들기

새 Xamarin.ios Cocoa 응용 프로그램을 만들면 기본적으로 표준 빈 창이 표시 됩니다. 이 창은 `.storyboard` 프로젝트에 자동으로 포함 되는 파일에 정의 됩니다. Windows 디자인을 편집 하려면 **솔루션 탐색기**에서 파일을 두 번 클릭 합니다 `Main.storyboard` .

[![](standard-controls-images/edit01.png "Selecting the Main Storyboard in the Solution Explorer")](standard-controls-images/edit01.png#lightbox)

이렇게 하면 Xcode의 Interface Builder에서 창 디자인이 열립니다.

[![](standard-controls-images/edit02.png "Editing the storyboard in Xcode")](standard-controls-images/edit02.png#lightbox)

사용자 인터페이스를 만들려면 **라이브러리 검사기** 에서 UI 요소 (Appkit 컨트롤)를 Interface Builder의 **인터페이스 편집기** 로 끌어 옵니다. 아래 예제에서는 **세로 분할 뷰** 컨트롤이 **라이브러리 검사자** 에서 마약 적용 되 고 **인터페이스 편집기**의 창에 배치 되었습니다.

[![](standard-controls-images/edit03.png "Selecting a Split View from the Library")](standard-controls-images/edit03.png#lightbox)

Interface Builder에서 사용자 인터페이스를 만드는 방법에 대 한 자세한 내용은 [Xcode 및 Interface Builder 소개 문서를](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 참조 하세요.

<a name="Sizing_and_Positioning"></a>

### <a name="sizing-and-positioning"></a>크기 조정 및 위치 지정

컨트롤이 사용자 인터페이스에 포함 된 후에는 **제약 조건 편집기** 를 사용 하 여 값을 수동으로 입력 하 여 위치와 크기를 설정 하 고 부모 창이 나 뷰의 크기를 조정할 때 컨트롤의 위치와 크기를 자동으로 조정 하는 방법을 제어 합니다.

[![](standard-controls-images/edit04.png "Setting the constraints")](standard-controls-images/edit04.png#lightbox)

**Autoresizing** 상자 외부의 **빨간색 빔** _을 사용 하 여_ 지정 된 (x, y) 위치에 컨트롤을 고정 합니다. 예를 들면 다음과 같습니다. 

[![](standard-controls-images/edit05.png "Editing a constraint")](standard-controls-images/edit05.png#lightbox)

**계층 구조 뷰**  &  **인터페이스 편집기**에서 선택 된 컨트롤이 크기가 조정 되거나 이동 될 때 창이 나 뷰의 위쪽 및 오른쪽 위치에 멈춰 있음을 지정 합니다. 

높이 및 너비와 같은 편집기 컨트롤 속성의 다른 요소:

[![](standard-controls-images/edit06.png "Setting the height")](standard-controls-images/edit06.png#lightbox)

**맞춤 편집기**를 사용 하 여 제약 조건이 있는 요소의 맞춤을 제어할 수도 있습니다.

[![](standard-controls-images/edit07.png "The Alignment Editor")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> IOS와 달리 (0, 0)은 화면의 왼쪽 위 모퉁이가 고 macOS (0, 0)는 왼쪽 아래 모퉁이입니다. MacOS는 숫자 값이 위쪽 및 오른쪽으로 증가 하는 수치 좌표 시스템을 사용 하기 때문입니다. 사용자 인터페이스에 AppKit 컨트롤을 배치할 때이를 고려해 야 합니다.

<a name="Setting_a_Custom_Class"></a>

### <a name="setting-a-custom-class"></a>사용자 지정 클래스 설정

AppKit 컨트롤을 사용 하 여 작업 하 고 기존 컨트롤을 서브클래싱하 며 해당 클래스의 사용자 지정 버전을 만들어야 하는 경우가 있습니다. 예를 들어, 원본 목록의 사용자 지정 버전을 정의 합니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

여기서 `[Register("SourceListView")]` 명령은 `SourceListView` 클래스를 목적-C에 노출 하므로를 Interface Builder에서 사용할 수 있습니다. 자세한 내용은 [Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서에서 c [# 클래스/메서드를 목표로](~/mac/internals/how-it-works.md) 표시 섹션을 참조 하세요. c `Register` `Export` # 클래스를 객관적인 개체 및 UI 요소에 연결 하는 데 사용 되는 및 명령을 설명 합니다.

위의 코드를 사용 하 여 확장 하는 기본 형식의 AppKit 컨트롤을 디자인 화면으로 끌어 올 수 있습니다 (아래 예제에서는 **원본 목록**). **Identity Inspector** 로 전환 하 고 **사용자 지정 클래스** 를 목표-C에 노출 한 이름 (예:)으로 설정 합니다 `SourceListView` .

[![](standard-controls-images/edit10.png "Setting a custom class in Xcode")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions"></a>

### <a name="exposing-outlets-and-actions"></a>콘센트 및 작업 노출

AppKit 컨트롤은 c # 코드에서 액세스할 수 있기 전에 **콘센트** 또는 and **작업**으로 노출 되어야 합니다. 이렇게 하려면 **인터페이스 계층 구조** 또는 **인터페이스 편집기** 에서 지정 된 컨트롤을 선택 하 고 **길잡이 뷰로** 전환 합니다 (편집용으로 창의을 선택 했는지 확인 `.h` ).

[![](standard-controls-images/edit11.png "Selecting the correct file to edit")](standard-controls-images/edit11.png#lightbox)

제어-AppKit 컨트롤에서 전달 파일로 끌어 `.h` **콘센트가** 나 **작업**만들기를 시작 합니다.

[![](standard-controls-images/edit12.png "Dragging to create an Outlet or Action")](standard-controls-images/edit12.png#lightbox)

만들 노출 유형을 선택 하 고 **콘센트가** 나 **작업** 에 **이름을**지정 합니다. 

[![](standard-controls-images/edit13.png "Configuring the Outlet or Action")](standard-controls-images/edit13.png#lightbox)

**콘센트** 및 **작업**으로 작업 하는 방법에 대 한 자세한 내용은 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 설명서의 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 참조 하세요.

<a name="Synchronizing_Changes_with_Xcode"></a>

### <a name="synchronizing-changes-with-xcode"></a>Xcode와 변경 내용 동기화

Xcode에서 Mac용 Visual Studio로 다시 전환 하면 Xcode에서 변경한 내용이 자동으로 Xamarin.ios 프로젝트와 동기화 됩니다.

솔루션 탐색기에서를 선택 하면 `SplitViewController.designer.cs` c **Solution Explorer** # 코드에서 **유출** 및 **작업이** 유선으로 연결 된 방식을 확인할 수 있습니다.

[![](standard-controls-images/sync01.png "Synchronizing Changes with Xcode")](standard-controls-images/sync01.png#lightbox)

파일의 정의가 다음과 같이 표시 되는지 확인 합니다 `SplitViewController.designer.cs` .

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Xcode의 파일에 있는 정의를 사용 하 여 줄을 설정 합니다 `MainWindow.h` .

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

여기에서 볼 수 있듯이 Mac용 Visual Studio는 파일에 대 한 변경 내용을 수신 하 `.h` 고 해당 파일의 변경 내용을 자동으로 동기화 `.designer.cs` 하 여 응용 프로그램에 노출 합니다. 는 `SplitViewController.designer.cs` partial 클래스 이므로 Mac용 Visual Studio는 `SplitViewController.cs` 클래스에 대 한 변경 내용을 덮어쓸 수 있는 수정이 필요 하지 않습니다.

일반적으로 사용자가 직접 열 필요가 없습니다 `SplitViewController.designer.cs` . 여기에는 교육용 으로만 제공 됩니다.

> [!IMPORTANT]
> 대부분의 경우 Mac용 Visual Studio는 Xcode에서 변경 된 내용을 자동으로 확인 하 고 Xamarin.ios 프로젝트와 동기화 합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.

<a name="Working_with_Buttons"></a>

## <a name="working-with-buttons"></a>단추 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 유형의 단추를 제공 합니다. 자세한 내용은 Apple의 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [단추](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/buttons01.png "An example of the different button types")](standard-controls-images/buttons01.png#lightbox)

**콘센트**를 통해 단추를 표시 한 경우 다음 코드는 눌러져 있는 것에 응답 합니다.

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

**작업**을 통해 노출 된 단추의 경우 `public partial` Xcode에서 사용자가 선택한 이름을 사용 하 여 자동으로 메서드가 생성 됩니다. **작업**에 응답 하려면 **동작이** 정의 된 클래스에서 부분 메서드를 완료 합니다. 예를 들면 다음과 같습니다.

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

상태 (예: **켜기** 및 **끄기**)를 사용 하는 단추의 경우 상태를 확인 하거나 열거형에 대해 속성을 사용 하 여 설정할 수 있습니다 `State` `NSCellStateValue` . 예를 들면 다음과 같습니다.

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

위치는 `NSCellStateValue` 다음과 같습니다.

- **On** -단추가 푸시되 고 컨트롤이 선택 됩니다 (예: 확인란 체크 상자).
- **Off** -단추가 푸시되 지 않거나 컨트롤이 선택 되지 않았습니다.
- **Mixed** - **설정** 및 **해제** 상태의 조합입니다.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent"></a>

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>단추를 기본값으로 표시 및 동일한 키 설정

사용자 인터페이스 디자인에 추가한 모든 단추에 대해 키보드에서 **Return/Enter** 키를 누를 때 활성화 되는 _기본_ 단추로 해당 단추를 표시할 수 있습니다. MacOS에서이 단추는 기본적으로 파란색 배경색을 받습니다.

단추를 기본값으로 설정 하려면 Xcode의 Interface Builder에서 선택 합니다. 그런 다음, **특성 검사자**에서 **키에 해당 하** 는 필드를 선택 하 고 **Return/enter** 키를 누릅니다.

[![](standard-controls-images/buttons03.png "Editing the Key Equivalent")](standard-controls-images/buttons03.png#lightbox)

마찬가지로 마우스 대신 키보드를 사용 하 여 단추를 활성화 하는 데 사용할 수 있는 키 시퀀스를 할당할 수 있습니다. 예를 들어 위의 이미지에서 Command-C 키를 누릅니다.

앱이 실행 되 고 단추가 있는 창이 키 이며 포커스가 있는 경우 사용자가 단추를 누르면 단추에 대 한 작업이 활성화 됩니다 (사용자가 단추를 클릭 한 경우).

<a name="Working_with_Checkboxes_and_Radio_Buttons"></a>

## <a name="working-with-checkboxes-and-radio-buttons"></a>확인란 및 라디오 단추 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 유형의 확인란 및 라디오 단추 그룹을 제공 합니다. 자세한 내용은 Apple의 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [단추](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/buttons02.png "An example of the available checkbox types")](standard-controls-images/buttons02.png#lightbox)

확인란 및 라디오 단추 ( **콘센트**를 통해 노출 됨)의 상태 (예: **설정** 및 **해제**)를 사용 하 여 상태를 확인 하거나 `State` 열거형에 대해 속성을 사용 하 여 설정할 수 있습니다 `NSCellStateValue` . 예를 들면 다음과 같습니다.

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

위치는 `NSCellStateValue` 다음과 같습니다.

- **On** -단추가 푸시되 고 컨트롤이 선택 됩니다 (예: 확인란 체크 상자).
- **Off** -단추가 푸시되 지 않거나 컨트롤이 선택 되지 않았습니다.
- **Mixed** - **설정** 및 **해제** 상태의 조합입니다.

라디오 단추 그룹의 단추를 선택 하려면 라디오 단추를 표시 하 여 **콘센트** 를 선택 하 고 해당 속성을 설정 `State` 합니다. 예를 들면 다음과 같습니다.

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

그룹 역할을 하는 라디오 단추의 컬렉션을 가져오고 선택 된 상태를 자동으로 처리 하려면 새 **작업** 을 만들고 그룹의 모든 단추를 연결 합니다.

![](standard-controls-images/buttons04.png "Creating a new Action")

그런 다음 `Tag` **특성 검사자**에서 각 라디오 단추에 고유한를 할당 합니다.

![](standard-controls-images/buttons05.png "Editing a radio button tag")

변경 내용을 저장 하 고 Mac용 Visual Studio로 돌아가서 모든 라디오 단추가 연결 된 **작업** 을 처리 하는 코드를 추가 합니다.

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

속성을 사용 하 여 `Tag` 선택한 라디오 단추를 확인할 수 있습니다.

<a name="Working_with_Menu_Controls"></a>

## <a name="working-with-menu-controls"></a>메뉴 컨트롤 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 가지 유형의 메뉴 컨트롤을 제공 합니다. 자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [메뉴 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/menu01.png "Example menu controls")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data"></a>

### <a name="providing-menu-control-data"></a>메뉴 컨트롤 데이터 제공

MacOS에서 사용할 수 있는 메뉴 컨트롤은 내부 목록 (Interface Builder에서 미리 정의 되거나 코드를 통해 채워질 수 있음) 또는 고유한 사용자 지정 외부 데이터 원본을 제공 하 여 드롭다운 목록을 채우도록 설정할 수 있습니다.

<a name="Working-with-Internal-Data"></a>

#### <a name="working-with-internal-data"></a>내부 데이터 작업

메뉴 컨트롤 (예:)은 Interface Builder 항목을 정의 하는 것 외에도 `NSComboBox` 유지 관리 하는 내부 목록에서 항목을 추가, 편집 또는 삭제할 수 있는 전체 메서드 집합을 제공 합니다.

- `Add`-목록의 끝에 새 항목을 추가 합니다.
- `GetItem`-지정 된 인덱스에 있는 항목을 반환 합니다.
- `Insert`-목록에서 지정 된 위치의 새 항목을 삽입 합니다.
- `IndexOf`-지정 된 항목의 인덱스를 반환 합니다.
- `Remove`-목록에서 지정 된 항목을 제거 합니다.
- `RemoveAll`-목록에서 모든 항목을 제거 합니다.
- `RemoveAt`-지정 된 인덱스에서 항목을 제거 합니다.
- `Count`-목록의 항목 수를 반환 합니다.

> [!IMPORTANT]
> Extern 데이터 소스 ()를 사용 하는 경우 `UsesDataSource = true` 위의 메서드를 호출 하면 예외가 throw 됩니다.

<a name="Working-with-an-External-Data-Source"></a>

#### <a name="working-with-an-external-data-source"></a>외부 데이터 원본 작업

기본 제공 내부 데이터를 사용 하 여 메뉴 컨트롤에 대 한 행을 제공 하는 대신 필요에 따라 외부 데이터 원본을 사용 하 고 항목에 대 한 고유한 백업 저장소 (예: SQLite 데이터베이스)를 제공할 수 있습니다.

외부 데이터 원본으로 작업 하려면 메뉴 컨트롤의 데이터 소스 인스턴스를 만들고 ( `NSComboBoxDataSource` 예:), 필요한 데이터를 제공 하는 여러 메서드를 재정의 합니다.

- `ItemCount`-목록의 항목 수를 반환 합니다.
- `ObjectValueForItem`-지정 된 인덱스에 대 한 항목의 값을 반환 합니다.
- `IndexOfItem`-항목 값 제공에 대 한 인덱스를 반환 합니다.
- `CompletedString`-부분적으로 형식화 된 항목 값에 대해 일치 하는 첫 번째 항목 값을 반환 합니다. 이 메서드는 자동 완성 기능을 사용 하도록 설정한 경우에만 호출 됩니다 ( `Completes = true` ).

자세한 내용은 [데이터베이스 작업](~/mac/app-fundamentals/databases.md) 문서의 [데이터베이스 및 ComboBoxes](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) 섹션을 참조 하세요.

<a name="Adjusting-the-Lists-Appearance"></a>

### <a name="adjusting-the-lists-appearance"></a>목록의 모양 조정

다음 메서드를 사용 하 여 메뉴 컨트롤의 모양을 조정할 수 있습니다.

- `HasVerticalScroller`-이면 `true` 컨트롤에 세로 스크롤 막대가 표시 됩니다. 
- `VisibleItems`-컨트롤을 열 때 표시 되는 항목 수를 조정 합니다. 기본값은 5입니다.
- `IntercellSpacing`-가 `NSSize` `Width` 왼쪽 및 오른쪽 여백을 지정 하 고에서 `Height` 항목 앞뒤의 공백을 지정 하는을 제공 하 여 지정 된 항목 주위의 공간 크기를 조정 합니다.
- `ItemHeight`-목록에 있는 각 항목의 높이를 지정 합니다.

드롭다운 형식의 경우 `NSPopupButtons` 첫 번째 메뉴 항목이 컨트롤의 제목을 제공 합니다. 예를 들면 다음과 같습니다. 

[![](standard-controls-images/menu02.png "An example menu control")](standard-controls-images/menu02.png#lightbox)

제목을 변경 하려면이 항목을 **콘센트** 로 노출 하 고 다음과 같은 코드를 사용 합니다.

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items"></a>

### <a name="manipulating-the-selected-items"></a>선택한 항목 조작

다음 메서드 및 속성을 사용 하 여 메뉴 컨트롤의 목록에서 선택한 항목을 조작할 수 있습니다.

- `SelectItem`-지정 된 인덱스에서 항목을 선택 합니다.
- `Select`-지정 된 항목 값을 선택 합니다.
- `DeselectItem`-지정 된 인덱스에서 항목을 선택 취소 합니다.
- `SelectedIndex`-현재 선택 된 항목의 인덱스를 반환 합니다.
- `SelectedValue`-현재 선택 된 항목의 값을 반환 합니다.

를 사용 `ScrollItemAtIndexToTop` 하 여 목록의 맨 위에 있는 지정 된 인덱스에 항목을 표시 하 고,를 사용 하 여 `ScrollItemAtIndexToVisible` 지정 된 인덱스의 항목이 표시 될 때까지 목록으로 스크롤합니다.

<a name="Responding to Events"></a>

### <a name="responding-to-events"></a>이벤트에 응답

메뉴 컨트롤은 사용자 상호 작용에 응답 하는 다음과 같은 이벤트를 제공 합니다.

- `SelectionChanged`-사용자가 목록에서 값을 선택 하면 호출 됩니다.
- `SelectionIsChanging`-새 사용자가 선택한 항목이 활성 선택이 되기 전에 호출 됩니다.
- `WillPopup`-항목의 드롭다운 목록이 표시 되기 전에 호출 됩니다.
- `WillDismiss`-항목의 드롭다운 목록이 종결 되기 전에 호출 됩니다.

컨트롤의 경우 `NSComboBox` `NSTextField` `Changed` 사용자가 콤보 상자의 텍스트 값을 편집할 때마다 호출 되는 이벤트와 같은 이벤트를 모두 포함 합니다.

필요에 따라 **작업** 에 항목을 연결 하 여 선택 하는 Interface Builder에 정의 된 내부 데이터 메뉴 항목에 응답 하 고 다음과 같은 코드를 사용 하 여 사용자가 트리거하는 **작업** 에 응답할 수 있습니다.

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

메뉴 및 메뉴 컨트롤을 사용 하는 방법에 대 한 자세한 내용은 [메뉴](~/mac/user-interface/menu.md) 및 [팝업 단추와 풀 다운 목록](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

<a name="Working_with_Selection_Controls"></a>

## <a name="working-with-selection-controls"></a>선택 컨트롤 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 가지 유형의 선택 컨트롤을 제공 합니다. 자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [선택 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/select01.png "Example selection controls")](standard-controls-images/select01.png#lightbox)

선택 컨트롤에 사용자 상호 작용이 있는 경우를 **동작**으로 노출 하 여이를 추적 하는 방법에는 두 가지가 있습니다. 예를 들면 다음과 같습니다.

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

또는 **대리자** 를 이벤트에 연결 `Activated` 합니다. 예를 들면 다음과 같습니다.

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

선택 컨트롤의 값을 설정 하거나 읽으려면 속성을 사용 `IntValue` 합니다. 예를 들면 다음과 같습니다.

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

전문 컨트롤 (예: 색 웰 및 이미지 웰)에는 해당 값 형식에 대 한 특정 속성이 있습니다. 예를 들면 다음과 같습니다.

```csharp
ColorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

에는 `NSDatePicker` 날짜 및 시간을 직접 사용 하기 위한 다음과 같은 속성이 있습니다.

- **DateValue** -현재 날짜 및 시간 값을 나타내는 `NSDate` 입니다.
- **Local** -사용자의 위치 () `NSLocal` 입니다.
- **Timeinterval** -시간 값을 나타내는 값 `Double` 입니다.
- **TimeZone** -사용자의 표준 시간대 `NSTimeZone` 입니다.

<a name="Working_with_Indicator_Controls"></a>

## <a name="working-with-indicator-controls"></a>표시기 컨트롤 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 유형의 표시기 컨트롤을 제공 합니다. 자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [지표 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/level01.png "Example indicator controls")](standard-controls-images/level01.png#lightbox)

표시기 컨트롤이 사용자 상호 작용을 **수행** 하는 **시기를 추적** 하는 방법에는 두 가지가 있습니다 **Delegate** `Activated` . 예를 들면 다음과 같습니다.

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

표시기 컨트롤의 값을 읽거나 설정 하려면 속성을 사용 `DoubleValue` 합니다. 예를 들면 다음과 같습니다.

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

표시 되지 않는 경우 미정 및 비동기 진행률 표시기를 애니메이션으로 적용 해야 합니다. 애니메이션을 `StartAnimation` 표시할 때 애니메이션을 시작 하려면 메서드를 사용 합니다. 예를 들면 다음과 같습니다.

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

메서드를 호출 하면 `StopAnimation` 애니메이션이 중지 됩니다.

<a name="Working_with_Text_Controls"></a>

## <a name="working-with-text-controls"></a>텍스트 컨트롤 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 형식의 텍스트 컨트롤을 제공 합니다. 자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [Text Controls](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) 섹션을 참조 하세요. 

[![](standard-controls-images/text01.png "Example text controls")](standard-controls-images/text01.png#lightbox)

텍스트 필드 ()의 경우 `NSTextField` 다음 이벤트를 사용 하 여 사용자 상호 작용을 추적할 수 있습니다.

- **변경 됨** -사용자가 필드 값을 변경할 때마다 발생 합니다. 예를 들어, 모든 문자를 입력 합니다.
- **EditingBegan** -사용자가 편집할 필드를 선택 하면 발생 합니다.
- **EditingEnded** -사용자가 필드에서 Enter 키를 누르거나 필드를 벗어날 때

`StringValue`필드의 값을 읽거나 설정 하려면 속성을 사용 합니다. 예를 들면 다음과 같습니다.

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

숫자 값을 표시 하거나 편집 하는 필드의 경우 속성을 사용할 수 있습니다 `IntValue` . 예를 들면 다음과 같습니다.

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

는 `NSTextView` 기본 제공 서식 지정을 사용 하 여 완전 한 기능을 갖춘 텍스트 편집 및 표시 영역을 제공 합니다. 와 마찬가지로 `NSTextField` 속성을 사용 `StringValue` 하 여 영역의 값을 읽거나 설정 합니다.

Xamarin.ios 앱에서 텍스트 보기를 사용 하는 복잡 한 예제에 대 한 예제는 [Sourcewriter 샘플 앱](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)을 참조 하세요. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

<a name="Working_with_Content_Views"></a>

## <a name="working-with-content-views"></a>콘텐츠 뷰 작업

AppKit는 사용자 인터페이스 디자인에 사용할 수 있는 여러 유형의 콘텐츠 보기를 제공 합니다. 자세한 내용은 Apple [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)의 [콘텐츠 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) 섹션을 참조 하세요.

[![](standard-controls-images/content01.png "An example content view")](standard-controls-images/content01.png#lightbox)

<a name="Popovers"></a>

### <a name="popovers"></a>Popovers

팝 오버은 특정 컨트롤 또는 화면 영역과 직접 관련 된 기능을 제공 하는 임시 UI 요소입니다. 창 위에 있는 팝 오버 float는 관련 된 컨트롤이 나 영역을 포함 하 고 해당 테두리에는 표시 되는 점을 나타내는 화살표가 포함 됩니다.

팝 오버을 만들려면 다음을 수행 합니다.

1. `.storyboard` **솔루션 탐색기** 를 두 번 클릭 하 여 팝 오버를 추가 하려는 창의 파일을 엽니다.
2. **라이브러리 검사기** 의 **뷰 컨트롤러** 를 **인터페이스 편집기**로 끌어 옵니다. 

    [![](standard-controls-images/content02.png "Selecting a View Controller from the Library")](standard-controls-images/content02.png#lightbox)
3. **사용자 지정 보기**의 크기와 레이아웃을 정의 합니다. 

    [![](standard-controls-images/content04.png "Editing the layout")](standard-controls-images/content04.png#lightbox)
4. 컨트롤을 클릭 하 고 팝업 소스에서 **뷰 컨트롤러로**끕니다. 

    [![](standard-controls-images/content05.png "Dragging to create a segue")](standard-controls-images/content05.png#lightbox)
5. 팝업 메뉴에서 **팝 오버** 를 선택 합니다. 

    [![](standard-controls-images/content06.png "Setting the segue type")](standard-controls-images/content06.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

<a name="Tab_Views"></a>

### <a name="tab-views"></a>탭 뷰

탭 뷰는 _창_이라고 하는 뷰 집합과 결합 된 탭 목록 (분할 된 컨트롤과 유사 함)으로 구성 됩니다. 사용자가 새 탭을 선택 하면 해당 탭에 연결 된 창이 표시 됩니다. 각 창에는 고유한 컨트롤 집합이 포함 되어 있습니다.

Xcode의 Interface Builder에서 탭 뷰로 작업 하는 경우 **특성 검사자** 를 사용 하 여 탭 수를 설정 합니다.

[![](standard-controls-images/content08.png "Editing the number of tabs")](standard-controls-images/content08.png#lightbox)

**인터페이스 계층 구조** 에서 각 탭을 선택 하 여 **제목을** 설정 하 고 UI 요소를 해당 **창**에 추가 합니다.

[![](standard-controls-images/content09.png "Editing the tabs in Xcode")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls"></a>

## <a name="data-binding-appkit-controls"></a>데이터 바인딩 AppKit 컨트롤

Xamarin.ios 응용 프로그램에서 키-값 코딩 및 데이터 바인딩 기술을 사용 하 여 UI 요소를 채우고 사용 하기 위해 작성 하 고 유지 관리 해야 하는 코드의 양을 크게 줄일 수 있습니다. 또한 프런트 엔드 사용자 인터페이스 (_모델-뷰-컨트롤러_)에서 지원 데이터 (_데이터 모델_)를 추가로 분리 하 여 더 쉽게 유지 관리 하 고 더욱 유연한 응용 프로그램을 디자인할 수 있는 이점을 누릴 수 있습니다.

KVC (키-값 코딩)는 키 (특수 형식의 문자열)를 사용 하 여 개체의 속성에 간접적으로 액세스 하 고 인스턴스 변수 또는 접근자 메서드 ()를 통해 액세스 하는 대신 속성을 식별 하는 메커니즘입니다 `get/set` . Xamarin.ios 응용 프로그램에서 키-값 코딩 규격 접근자를 구현 하 여 키-값 관찰 (KVO), 데이터 바인딩, 코어 데이터, Cocoa 바인딩, scriptability 등의 다른 macOS 기능에 액세스할 수 있습니다.

자세한 내용은 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서의 [단순 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) 섹션을 참조 하세요.

<a name="Summary"></a>

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤과 같은 표준 AppKit 컨트롤을 사용 하는 방법에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 사용자 인터페이스 디자인에 추가 하 여 콘센트 및 작업을 통해 코드에 노출 하 고 c # 코드에서 AppKit 컨트롤을 사용 하는 것에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [MacControls (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccontrols)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [컨트롤 및 뷰 정보](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
