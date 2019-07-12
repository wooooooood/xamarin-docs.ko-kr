---
title: Xamarin.Mac의 표준 컨트롤
description: 이 문서에서는 단추, 레이블, 텍스트 필드, 확인란 등 표준 AppKit 컨트롤을 사용 하 여 작업 내용을 다룹니다 및 Xamarin.Mac 응용 프로그램에서 컨트롤을 분할 합니다. Interface Builder를 사용 하 여 인터페이스를 추가 하 고 코드에서 상호 작용을 설명 합니다.
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 26ab7880b3c4b6176c806783fec7a499d68511c3
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831903"
---
# <a name="standard-controls-in-xamarinmac"></a>Xamarin.Mac의 표준 컨트롤

_이 문서에서는 단추, 레이블, 텍스트 필드, 확인란 등 표준 AppKit 컨트롤을 사용 하 여 작업 내용을 다룹니다 및 Xamarin.Mac 응용 프로그램에서 컨트롤을 분할 합니다. Interface Builder를 사용 하 여 인터페이스를 추가 하 고 코드에서 상호 작용을 설명 합니다._

작업할 때 C# 에 액세스할 수 있는 Xamarin.Mac 응용 프로그램에서.NET 및 동일한 AppKit 컨트롤에서 작업을 수행할 *Objective-c* 하 고 *Xcode* 않습니다. Xcode의 Xamarin.Mac이 Xcode와 직접 통합 되므로 사용할 수 있습니다 _Interface Builder_ 만들기 및 Appkit 컨트롤 유지 관리 (또는 필요에 따라 C# 코드에서 바로 작성).

AppKit 컨트롤은 Xamarin.Mac 응용 프로그램의 사용자 인터페이스를 만드는 데 사용 되는 UI 요소입니다. 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤 같은 요소로 구성 하며 사용자 조작 하는 경우 빠른 작업 또는 표시 결과 유발 합니다.

[![](standard-controls-images/intro01.png "예제 앱 주 화면")](standard-controls-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 AppKit 컨트롤을 사용 하 여 작업의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>컨트롤 및 보기 소개

macOS (Mac OS X 이전의 함) AppKit 프레임 워크를 통해 사용자 인터페이스 컨트롤의 표준 집합을 제공 합니다. 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤 같은 요소로 구성 하며 사용자 조작 하는 경우 빠른 작업 또는 표시 결과 유발 합니다.

대부분의 사용에 대 한 적절 한 수 있는 표준, 기본 제공 모양을 갖도록 모든 AppKit 컨트롤, 창 프레임 영역 또는 사용에 대 한 대체 모양을 지정 일부는 _활기 효과_ 또는 세로 막대와 같은 컨텍스트를 알림 센터 위젯입니다.

Apple는 AppKit 컨트롤을 사용 하는 경우 다음 지침을 제안 합니다.

- 동일한 보기에 컨트롤 크기를 혼합 하지 마세요.
- 일반적으로 컨트롤의 세로 크기 조정 하지 마십시오.
- 시스템 글꼴 및 컨트롤 내에서 적절 한 텍스트 크기를 사용 합니다.
- 컨트롤 사이의 적절 한 간격을 사용 합니다.

자세한 내용은 pleas 합니다 [컨트롤에 대 한 뷰와](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다.

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>창 프레임에서 컨트롤 사용

창 프레임 영역에 포함 될 수 있도록 표시 스타일을 포함 하는 AppKit 컨트롤의 하위 집합을 있습니다. 예를 들어 메일 앱의 도구 모음을 참조 하세요.

[![](standard-controls-images/mailapp.png "Mac 창 프레임")](standard-controls-images/mailapp.png#lightbox)

- **질감 단추 반올림** - `NSButton` 스타일이 `NSTexturedRoundedBezelStyle`합니다.
- **분할 된 제어 반올림 질감** - `NSSegmentedControl` 스타일이 `NSSegmentStyleTexturedRounded`합니다.
- **분할 된 제어 반올림 질감** - `NSSegmentedControl` 스타일이 `NSSegmentStyleSeparated`합니다.
- **팝업 메뉴 질감 반올림** - `NSPopUpButton` 스타일이 `NSTexturedRoundedBezelStyle`합니다.
- **드롭다운 메뉴 질감 반올림** - `NSPopUpButton` 스타일이 `NSTexturedRoundedBezelStyle`합니다.
- **검색 표시줄** - `NSSearchField`합니다.

Apple 창 프레임에 대 한 AppKit 컨트롤을 사용 하는 경우 다음 지침을 제안 합니다.

- 창 본문에 특정 컨트롤 스타일 창 프레임을 사용 하지 마세요.
- 창 프레임에 창을 본문 컨트롤 또는 스타일을 사용 하지 마세요.

자세한 내용은 pleas 합니다 [컨트롤에 대 한 뷰와](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다.

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>Interface Builder에서 사용자 인터페이스 만들기

새 Xamarin.Mac Cocoa 응용 프로그램을 만들면 기본적으로 표준 비어 창을 가져옵니다. 이 windows에 정의 된를 `.storyboard` 파일 프로젝트에 자동으로 포함 합니다. windows 디자인에 맞게 편집 하는 **솔루션 탐색기**를 두 번 클릭 합니다 `Main.storyboard` 파일:

[![](standard-controls-images/edit01.png "솔루션 탐색기에서 주 스토리 보드 선택")](standard-controls-images/edit01.png#lightbox)

Xcode의 Interface Builder에서 창 디자인을 엽니다.

[![](standard-controls-images/edit02.png "Xcode에서 스토리 보드를 편집합니다.")](standard-controls-images/edit02.png#lightbox)

사용자 인터페이스를 만들려면 끌면 UI 요소 (AppKit 컨트롤)에서 **라이브러리 검사기** 에 **인터페이스 편집기** Interface Builder에서. 아래 예제에서는 **수직 분할 뷰** 컨트롤에서 약품 되었습니다를 **라이브러리 검사기** 창에 배치 하 고는 **인터페이스 편집기**:

[![](standard-controls-images/edit03.png "라이브러리에서 분할 보기를 선택합니다.")](standard-controls-images/edit03.png#lightbox)

Interface Builder에서 사용자 인터페이스를 만드는 방법에 대 한 자세한 내용은 참조 하십시오 우리의 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 설명서.

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>크기 조정 및 위치 지정

사용자 인터페이스에 포함 된 컨트롤을 사용 합니다 **제약 조건 편집기** 값을 수동으로 입력 하 여 해당 위치와 크기를 설정 하 고 컨트롤을 자동으로 배치 하 고 때 크기를 조정 하는 방법을 제어 하려면 부모 창 또는 보기 크기가 조정 됩니다.

[![](standard-controls-images/edit04.png "제약 조건 설정")](standard-controls-images/edit04.png#lightbox)

사용 하 여는 **빨간색 I-빔** 바깥쪽 둘레를 **Autoresizing** 상자 _스틱_ (x, y) 지정된 된 위치에 컨트롤입니다. 예: 

[![](standard-controls-images/edit05.png "제약 조건 편집")](standard-controls-images/edit05.png#lightbox)

지정 선택한 컨트롤 (에 **계층 구조 보기** & **인터페이스 편집기**)를 조정 하거나 이동 하는 대로 창 또는 보기의 위쪽 및 오른쪽 위치를 멈출 수 있습니다. 

편집기의 다른 요소 높이 및 너비 같은 속성을 제어 합니다.

[![](standard-controls-images/edit06.png "높이 설정합니다.")](standard-controls-images/edit06.png#lightbox)

사용 하 여 제약 조건이 있는 요소의 맞춤을 제어할 수도 있습니다는 **맞춤 편집기**:

[![](standard-controls-images/edit07.png "맞춤 편집기")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> IOS와 달리 위치 (0, 0) 위에 macOS (0, 0)에서 화면의 왼쪽 위 모퉁이 왼쪽 아래 모서리입니다. MacOS는 수학 좌표계를 사용 하 여 위쪽 및 오른쪽 값 증가 숫자 값을 사용 하 여 때문입니다. 사용자 인터페이스에 대 한 AppKit 컨트롤을 배치 하는 경우 고려해 야이 해야 합니다.

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>사용자 지정 클래스를 설정합니다.

때 AppKit 컨트롤을 사용는 서브 클래스를 기존 컨트롤 및 해당 클래스의 고유한 사용자 지정 버전을 만들어야 하는 경우가 있습니다. 예를 들어, 원본 목록에서 사용자 지정 버전을 정의:

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

여기서는 `[Register("SourceListView")]` 명령 노출 된 `SourceListView` Interface Builder에서 이것이 objective-c 클래스를 사용할 수 있습니다. 자세한 내용은 참조 하십시오는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 부분을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 문서에 설명를 `Register` 및 `Export` 사용 되는 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소입니다.

위의 코드를 사용 하 여 AppKit 컨트롤을 확장 하는 기본 형식의 디자인 화면으로 끌어 수 있습니다 (아래 예제에서는 **원본 목록**), 전환할를 **검사기** 설정 합니다 **사용자 지정 클래스** Objective-c에 노출 하는 이름 (예 `SourceListView`):

[![](standard-controls-images/edit10.png "Xcode에서 사용자 지정 클래스를 설정합니다.")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>출 선 및 작업을 노출합니다.

중으로 표시 되기 위해 필요한 AppKit 컨트롤을 C# 코드에서 액세스할 수 있습니다, 전에 **출 선** 또는 및 **동작**합니다. 지정된 된 컨트롤에서 선택이 작업을 수행 하는 **인터페이스 계층 구조** 또는 **인터페이스 편집기** 전환 하는 **도우미 보기** (했는지를 `.h`편집을 위해 선택한 창의):

[![](standard-controls-images/edit11.png "편집 하려면 올바른 파일 선택")](standard-controls-images/edit11.png#lightbox)

에 지정 된 AppKit 컨트롤에서 컨트롤 끌기 `.h` 만들기를 시작 하려면 파일을 **출 선** 또는 **동작**:

[![](standard-controls-images/edit12.png "출 선 또는 작업을 만드는 끌어")](standard-controls-images/edit12.png#lightbox)

만들고 지정에 대 한 노출의 유형을 선택 합니다 **출 선** 또는 **동작** 는 **이름**: 

[![](standard-controls-images/edit13.png "출 선 또는 동작을 구성합니다.")](standard-controls-images/edit13.png#lightbox)


작업에 대 한 자세한 내용은 **출 선** 및 **작업**를 참조 하세요 합니다 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 부분 우리의 [Xcode 및 Interface 소개 작성기](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 설명서.

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode와 변경 내용 동기화

에서 전환 하면 Visual Studio로 다시 Mac 용 Xcode, Xcode에서 변경한 내용은 자동으로 Xamarin.Mac 프로젝트와 동기화 됩니다.

선택 하는 경우는 `SplitViewController.designer.cs` 에 **솔루션 탐색기** 를 볼 수 있습니다 하는 방법에 **출 선** 및 **작업** C# 코드에 확보 된:

[![](standard-controls-images/sync01.png "Xcode와 변경 내용 동기화")](standard-controls-images/sync01.png#lightbox)

알림 방법의 정의 `SplitViewController.designer.cs` 파일:

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

정의 사용 하 여 줄 위로 `MainWindow.h` Xcode에서 파일:

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

Mac 용 Visual Studio 변경 내용을 수신 대기 하는 알 수 있듯이 합니다 `.h` 파일을 선택한 후에 해당 변경 사항을 자동으로 동기화 `.designer.cs` 파일을 응용 프로그램에 노출 합니다. 또한 알 수 있습니다 `SplitViewController.designer.cs` Mac 용 Visual Studio를 수정 하지 않아도 되도록 부분 클래스는 `SplitViewController.cs` 에서는 클래스에 대 한 모든 변경 내용을 덮어쓰는 것입니다.

일반적으로 필요가 엽니다는 `SplitViewController.designer.cs` 직접 보여드린 것 여기 교육 목적에 대 한 합니다.

> [!IMPORTANT]
> 대부분의 경우 Mac 용 Visual Studio 자동으로 Xcode에서 변경 된 내용을 참조 하 고 Xamarin.Mac 프로젝트에 동기화 합니다. 동기화가 자동으로 수행되지 않는 경우 Xcode로 돌아가서 다시 Mac용 Visual Studio로 돌아갑니다. 대부분 이렇게 하면 동기화 주기가 시작됩니다.

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>단추를 사용 하 여 작업

AppKit 몇 가지 유형의 사용자 인터페이스 디자인에 사용할 수 있는 단추를 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [단추](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/buttons01.png "여러 가지 단추 종류의 예")](standard-controls-images/buttons01.png#lightbox)

단추를 통해 노출 된 경우는 **출 선**, 다음 코드를 누르는 것에 응답 합니다.

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

통해 노출 되었지만 단추 **작업**, `public partial` 메서드는 자동으로 생성 Xcode에서 선택한 이름입니다. 응답할를 **작업**, 클래스의 부분 메서드를 완료 하는 합니다 **작업** 에 정의 된 합니다. 예를 들어:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

단추 상태에 대 한 (같은 **에** 및 **해제**), 상태를 확인 또는 사용 하 여 설정할 수는 `State` 속성에 대해는 `NSCellStateValue` 열거형입니다. 예를 들어:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

여기서 `NSCellStateValue` 될 수 있습니다.

- **온** -단추의 하거나 (예: 확인란의 확인) 컨트롤을 선택 합니다.
- **해제** -단추 푸시되 지 않습니다 또는 컨트롤을 선택 하지 않습니다.
- **혼합** -조합 **에** 하 고 **해제** 상태입니다.

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>해당 키를 설정 하 고 기본 단추를 표시 합니다.

사용자 인터페이스 디자인에 추가 하는 모든 단추는 단추를 표시할 수 있습니다 합니다 _기본_ 단추를 누를 때 활성화 되는 합니다 **반환/Enter** 키보드의 키입니다. MacOS에서이 단추는 기본적으로 배경색을 파란색을 받게 됩니다.

기본적으로 단추를 설정 하려면 Xcode의 Interface Builder에서 선택 합니다. 다음으로 **특성 검사기**를 선택 합니다 **해당 키** 필드 및 키를 누릅니다를 **반환/Enter** 키:

[![](standard-controls-images/buttons03.png "키에 해당 하는 편집")](standard-controls-images/buttons03.png#lightbox)

마찬가지로 마우스 대신 키보드를 사용 하 여 단추를 활성화 하는 데 사용할 수 있는 모든 키 시퀀스를 할당할 수 있습니다. 예를 들어 위의 이미지에서 누르면 명령 + C 키를 여 합니다.

때 앱이 실행 되는 단추가 있는 창 키 고 집중, 사용자가 명령 + C를 누르면 단추에 대 한 작업 활성화 됩니다 (으로-사용자가 단추를 클릭 한).

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>확인란 및 라디오 단추를 사용 하 여 작업

AppKit 몇 가지 유형의 확인란 및 사용자 인터페이스 디자인에 사용할 수 있는 라디오 단추 그룹을 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [단추](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/buttons02.png "사용 가능한 확인란을 선택 하는 형식의 예")](standard-controls-images/buttons02.png#lightbox)


확인란 및 라디오 단추 (을 통해 노출 **출 선**) 상태 (같은 **에** 및 **해제**), 상태 확인 또는 사용 하 여 설정할 수는 `State` 속성에 대해는 `NSCellStateValue` 열거형입니다. 예를 들어:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

여기서 `NSCellStateValue` 될 수 있습니다.

- **온** -단추의 하거나 (예: 확인란의 확인) 컨트롤을 선택 합니다.
- **해제** -단추 푸시되 지 않습니다 또는 컨트롤을 선택 하지 않습니다.
- **혼합** -조합 **에** 하 고 **해제** 상태입니다.

라디오 단추 그룹의 단추를 선택 하려면이 라디오 단추를 선택 표시는 **출 선** 설정 하 고 해당 `State` 속성입니다. 예를 들면 다음과 같습니다.

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

라디오 단추는 그룹으로 작동 하 고 자동으로 선택된 된 상태를 처리 하는 컬렉션을 가져오려면를 만듭니다 **동작** 및 그룹의 모든 단추에 연결:

![](standard-controls-images/buttons04.png "새 동작 만들기")

다음으로, 고유한 할당 `Tag` 각 라디오 단추에는 **특성 검사기**:

![](standard-controls-images/buttons05.png "라디오 단추 태그 편집")

변경 내용을 저장 하 고 Mac 용 Visual Studio로 돌아가서, 처리 하는 코드를 추가 합니다 **동작** 에 연결 된 모든 라디오 단추:

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

사용할 수는 `Tag` 속성을 선택 된 라디오 단추를 참조 하세요.

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>메뉴 컨트롤 작업

AppKit 여러 유형의 사용자 인터페이스 디자인에 사용할 수 있는 메뉴 컨트롤을 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [메뉴 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/menu01.png "예제 메뉴 컨트롤")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>메뉴 컨트롤 데이터를 제공합니다.

MacOS에서 사용할 수 있는 메뉴 컨트롤 (또는 수 있는 Interface Builder에서 미리 정의 된 코드를 통해 채울) 내부 목록에서 드롭다운 목록을 채우는 데 설정할 수 있습니다 하거나 사용자 고유의 사용자 지정, 외부 데이터 소스를 제공 합니다.

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>내부 데이터 작업

Interface Builder를 메뉴 컨트롤의 항목을 정의 하는 것 외에도 (같은 `NSComboBox`), 추가, 편집 또는 유지 관리 하는 내부 목록에서 항목을 삭제할 수 있도록 하는 방법의 전체 집합을 제공 합니다.

- `Add` -새 항목이 목록 끝에 추가합니다.
- `GetItem` -지정된 된 인덱스에서 항목을 반환합니다.
- `Insert` -지정된 된 위치에 있는 목록에서 새 항목을 삽입 합니다.
- `IndexOf` -지정된 된 항목의 인덱스를 반환 합니다.
- `Remove` -지정된 된 항목 목록에서 제거합니다.
- `RemoveAll` -목록에서 모든 항목을 제거합니다.
- `RemoveAt` -지정된 된 인덱스에서 항목을 제거 합니다.
- `Count` -목록의 항목 개수를 반환합니다.

> [!IMPORTANT]
> Extern 데이터 소스를 사용 하는 경우 (`UsesDataSource = true`), 위에서 설명한 방법 중 하나를 호출 하면 예외가 throw 됩니다.

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>외부 데이터 원본 사용

메뉴 컨트롤에 대 한 행 수 있도록 기본 제공 내부 데이터를 사용 하는 대신 외부 데이터 원본을 사용 하 고 항목 (예: SQLite 데이터베이스)에 대 한 사용자 고유의 백업 저장소를 제공 합니다. 필요에 따라 수 있습니다.

외부 데이터 소스를 사용 하려면 메뉴 컨트롤의 데이터 원본 인스턴스를 만들 수 (`NSComboBoxDataSource` 예를 들어) 하 고 필요한 데이터를 제공 하는 여러 메서드를 재정의 합니다.

- `ItemCount` -목록의 항목 개수를 반환합니다.
- `ObjectValueForItem` -지정된 된 인덱스에 대 한 항목의 값을 반환 합니다.
- `IndexOfItem` -제공 항목 값의 인덱스를 반환 합니다.
- `CompletedString` -부분적으로 형식화 된 항목 값에 대 한 첫 번째 일치 항목 값을 반환합니다. 이 메서드는 자동 완성 기능을 사용할 수 있는 경우에 호출 됩니다 (`Completes = true`).

참조 하세요 합니다 [데이터베이스 및 콤보 상자](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes) 섹션을 [데이터베이스 작업](~/mac/app-fundamentals/databases.md) 문서에 대 한 자세한.

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>목록의 모양을 조정합니다.

다음 메서드는 메뉴 컨트롤의 모양을 조정할 수 있습니다.

- `HasVerticalScroller` - `true`를 컨트롤에 세로 스크롤 막대가 표시 됩니다. 
- `VisibleItems` -컨트롤 열릴 때 표시 되는 항목 수를 조정 합니다. 기본값은 5 (5).
- `IntercellSpacing` -함으로써 지정된 된 항목 둘레의 공간 크기를 조정 합니다.는 `NSSize` 여기서는 `Width` 왼쪽 및 오른쪽 여백을 지정 하는 및 `Height` 항목 전후 공간을 지정 합니다.
- `ItemHeight` -목록의 각 항목의 높이 지정합니다.

드롭다운 목록 유형의 `NSPopupButtons`, 첫 번째 메뉴 항목 컨트롤의 제목을 제공 합니다. 예를 들면 다음과 같습니다. 

[![](standard-controls-images/menu02.png "예제 메뉴 컨트롤")](standard-controls-images/menu02.png#lightbox)

제목을 변경 하려면이 항목을 노출 된 **출 선** 다음과 같은 코드를 사용 하 여:

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>선택한 항목을 조작

다음 메서드 및 속성을 통해 메뉴 컨트롤의 목록에서 선택한 항목을 조작할 수 있습니다.

- `SelectItem` -지정된 된 인덱스에서 항목을 선택 합니다.
- `Select` -지정 된 항목 값을 선택 합니다.
- `DeselectItem` -지정된 된 인덱스에서 항목을 선택 취소 합니다.
- `SelectedIndex` -현재 선택한 항목의 인덱스를 반환 합니다.
- `SelectedValue` -현재 선택한 항목의 값을 반환 합니다.

사용 하 여는 `ScrollItemAtIndexToTop` 목록의 맨 위에 있는 지정된 된 인덱스에서 항목을 표시 하 고 `ScrollItemAtIndexToVisible` 지정된 된 인덱스에 있는 항목 표시 될 때까지 목록을 스크롤합니다.

<a name="Responding to Events" />

### <a name="responding-to-events"></a>이벤트에 응답

메뉴 컨트롤이 사용자 상호 작용에 응답할 다음 이벤트를 제공 합니다.

- `SelectionChanged` -사용자가 목록에서 값을 선택한 경우 호출 됩니다.
- `SelectionIsChanging` -새 사용자가 선택한 항목 활성화 된 선택 되기 전에 호출 합니다.
- `WillPopup` -항목의 드롭다운 목록을 표시 되기 전에 호출 됩니다.
- `WillDismiss` -항목의 드롭다운 목록 닫히기 전에 호출 됩니다.

에 대 한 `NSComboBox` 컨트롤, 이들은 모두 동일한 이벤트를 합니다 `NSTextField`와 같은 `Changed` 사용자가 콤보 상자에서 텍스트의 값을 편집할 때마다 호출 되는 이벤트입니다.

응답할 수 있습니다는 항목을 연결 하 여 선택한 Interface Builder에서 내부 데이터 메뉴 항목을 정의 **동작** 다음과 같은 코드를 사용 하 여 응답할 **작업** 되 고 사용자가 트리거됩니다.

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

메뉴 및 메뉴 컨트롤 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [메뉴](~/mac/user-interface/menu.md) 하 고 [단추를 팝업 하 고 풀 다운 나열](~/mac/user-interface/menu.md) 설명서.

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>선택 컨트롤 사용

AppKit 여러 유형의 사용자 인터페이스 디자인에 사용할 수 있는 선택 컨트롤을 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [선택 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/select01.png "선택 컨트롤 예")](standard-controls-images/select01.png#lightbox)

선택 컨트롤으로 노출 하 여 사용자 상호 작용에 추적 하는 방법은 두 가지는 **동작**합니다. 예:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

또는 연결 하 여는 **대리자** 에 `Activated` 이벤트입니다. 예:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

를 설정 하거나 선택 컨트롤의 값을 읽고 여 `IntValue` 속성입니다. 예를 들어:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

특수 컨트롤 (예: 색 및 이미지도)는 해당 값 형식에 대 한 특정 속성이 있습니다. 예를 들면 다음과 같습니다.

```csharp
ColorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker` 날짜 및 시간에 직접 사용 하기 위한 다음 속성이 있습니다.

- **DateValue** -현재 날짜 및 시간 값을 `NSDate`입니다.
- **로컬** -사용자의 위치는 `NSLocal`합니다.
- **TimeInterval** -시간 값으로는 `Double`합니다.
- **표준 시간대** -사용자의 표준 시간대를 `NSTimeZone`입니다.

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>표시기 컨트롤 사용

AppKit 여러 유형의 사용자 인터페이스 디자인에 사용할 수 있는 표시기 컨트롤을 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [표시기 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/level01.png "표시기 예제 컨트롤")](standard-controls-images/level01.png#lightbox)

표시기 컨트롤에 그대로 노출 하거나 사용자 상호 작용을 추적 하는 방법은 두 가지는 **작업** 요소나 **출 선** 및 연결을 **대리자** 하는 `Activated`이벤트입니다. 예를 들어:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

읽거나 표시기 컨트롤의 값을 설정 하려면 사용 된 `DoubleValue` 속성입니다. 예:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

비활성 및 비동기 진행률 표시기를 애니메이션 효과 적용할 때 표시 합니다. 사용 된 `StartAnimation` 표시 될 때 애니메이션을 시작 하는 방법입니다. 예:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

호출 된 `StopAnimation` 메서드는 애니메이션을 중지 합니다.

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>텍스트 컨트롤 사용

AppKit 여러 유형의 사용자 인터페이스 디자인에 사용할 수 있는 텍스트 컨트롤을 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [텍스트 컨트롤](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다. 

[![](standard-controls-images/text01.png "예제에서는 텍스트 컨트롤")](standard-controls-images/text01.png#lightbox)

텍스트 필드 (`NSTextField`), 사용자 상호 작용을 추적 하는 다음 이벤트를 사용할 수 있습니다.

- **변경** -필드의 값을 변경할 때마다 발생 합니다. 예를 들어, 모든 문자에 다음을 입력 합니다.
- **EditingBegan** -사용자가 편집 필드를 선택할 때 발생 합니다.
- **EditingEnded** -사용자가 필드에서 Enter 키를 누르거나 필드를 유지 하는 경우.

사용 된 `StringValue` 속성을 읽거나 필드의 값을 설정 합니다. 예를 들어:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

표시 하거나 숫자 값을 편집 하는 필드를 사용할 수 있습니다는 `IntValue` 속성입니다. 예:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView` 완전 한 기능 갖춘된 텍스트를 제공 합니다. 편집 하 고 기본 서식을 사용 하 여 영역을 표시 합니다. 같은 `NSTextField`를 사용 하 여는 `StringValue` 속성을 읽거나은 영역 값을 설정 합니다.

Xamarin.Mac 앱에서 텍스트 뷰를 사용 하 여 작업의 복잡 한 예의 예제를 참조 하십시오 합니다 [SourceWriter 샘플 앱](https://developer.xamarin.com/samples/mac/SourceWriter/)합니다. SourceWriter는 코드 완성 및 간단한 구문 강조 기능을 제공하는 간단한 소스 코드 편집기입니다.

SourceWriter 코드는 완벽하게 주석 처리되어 있으며, 가능한 경우 Xamarin.Mac 지침 설명서에 핵심 기술 또는 메서드부터 관련 정보까지 다양한 링크가 제공됩니다.

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>콘텐츠 보기 작업

AppKit 몇 가지 유형의 사용자 인터페이스 디자인에 사용할 수 있는 콘텐츠 뷰를 제공 합니다. 자세한 내용은 참조 하십시오 합니다 [콘텐츠 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple의 섹션 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)합니다.

[![](standard-controls-images/content01.png "예제 콘텐츠 보기")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

팝 오버는 특정 컨트롤 또는 화면 영역에 직접 관련 된 기능을 제공 하는 일시적인 UI 요소입니다. 팝 오버 제어 나, 관련 된 영역에 포함 된 창 위에 배치 하 고 테두리를 부상 하는 지점을 나타내는 화살표를 포함 합니다.

팝 오버를 만들려면 다음을 수행 합니다.

1. 엽니다는 `.storyboard` 창에 팝 오버에서 두 번 클릭 하 여 추가 하려는 파일을 **솔루션 탐색기**
2. 끌어서를 **컨트롤러를 보려면** 에서 합니다 **라이브러리 검사기** 에 **인터페이스 편집기**: 

    [![](standard-controls-images/content02.png "라이브러리에서 뷰 컨트롤러를 선택 하면")](standard-controls-images/content02.png#lightbox)
4. 크기 및 레이아웃을 정의 합니다 **사용자 지정 보기**: 

    [![](standard-controls-images/content04.png "레이아웃 편집")](standard-controls-images/content04.png#lightbox)
5. 컨트롤 클릭 하 고 팝업에 원본의 드래그 합니다 **뷰 컨트롤러**: 

    [![](standard-controls-images/content05.png "끌어서 segue를 만들기")](standard-controls-images/content05.png#lightbox)
6. 선택 **팝 오버** 팝업 메뉴에서: 

    [![](standard-controls-images/content06.png "Segue 형식 설정")](standard-controls-images/content06.png#lightbox)
7. 변경 내용을 저장 하 고 Xcode와 동기화 하는 Mac 용 Visual Studio로 돌아갑니다.

<a name="Tab_Views" />

### <a name="tab-views"></a>탭 뷰

탭 뷰 (어떤 유사한 분할 된 제어를) 호출 되는 뷰 집합을 함께 탭 목록으로 이루어집니다 _창이_합니다. 사용자가 새 탭을 선택 하면 연결 된 창 표시 됩니다. 각 창에는 자체 컨트롤 집합이 포함 됩니다.

Xcode의 Interface Builder에서 탭 뷰를 사용할 때 사용 합니다 **특성 검사기** 탭의 개수를 설정 하려면:

[![](standard-controls-images/content08.png "탭의 개수를 편집합니다.")](standard-controls-images/content08.png#lightbox)

각 탭을 선택 합니다 **인터페이스 계층 구조** 설정 하려면 해당 **Title** UI 요소를 추가 하 고 해당 **창**:

[![](standard-controls-images/content09.png "Xcode에서 탭 편집")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>데이터 바인딩 AppKit 컨트롤

Xamarin.Mac 응용 프로그램에서 데이터 바인딩 및 키-값 코딩 기술을 사용 하 여 작성 하 고 채우는 UI 요소를 사용 하 여 작업을 유지 해야 하는 코드의 양을 크게 줄일 수 있습니다. 추가 백업 데이터를 분리의 장점은 수도 있습니다 (_데이터 모델_)에 처음부터 사용자 인터페이스를 종료 합니다 (_모델-뷰-컨트롤러_)를 더 쉬워진 유지 관리를 보다 융통성 있는 응용 프로그램 디자인 합니다.

키-값 코딩 (KVC)는 개체의 속성을 직접 액세스, 인스턴스 변수를 통해 액세스 하는 대신 속성을 식별 (특별 한 형식의 문자열) 키 또는 접근자 메서드를 사용 하는 메커니즘 (`get/set`). Xamarin.Mac 응용 프로그램에서 키-값 코딩 규격 접근자를 구현, 키-값을 관찰 (KVO), 데이터 바인딩, 핵심 데이터, Cocoa 바인딩 및 스크립팅 등과 같은 다른 macOS 기능에 대 한 액세스를 얻을 수 있습니다.

자세한 내용은 참조 하십시오 합니다 [단순 데이터 바인딩](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding) 섹션의 우리의 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 설명서.


<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 단추, 레이블, 텍스트 필드, 확인란 및 분할 된 컨트롤 같은 표준 AppKit 컨트롤을 사용 하 여 작업을 자세히 살펴보려면을 걸렸습니다. 이 설명 Xcode의 Interface Builder에서 사용자 인터페이스 디자인에 추가 출 선 및 작업을 통해 코드에 노출 하 고 C# 코드에서 AppKit 컨트롤을 사용 합니다.

## <a name="related-links"></a>관련 링크

- [MacControls (샘플)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [컨트롤 및 보기](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
