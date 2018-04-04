---
title: 대화 상자
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 모달 창 및 대화 상자를 사용 합니다. 표준 대화 상자를 사용 하 고 C# 코드에서 이러한 컨트롤와 상호 작용 Xcode 및 인터페이스 작성기에서 모달 창을 만드는 설명 합니다.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cd0b57cc40d3f7cffd4d5711ec04a7225be870e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="dialogs"></a>대화 상자

동일한 대화 상자 및 모달 창에 대 한 액세스를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 해야 하는 개발자의 작업 *Objective C* 및 *Xcode* 않습니다. Xamarin.Mac Xcode에 직접 통합을 사용 하면 Xcode의 _인터페이스 작성기_ 만들기 및 모달 창을 유지 관리 (또는 C# 코드에서 직접 만들 필요에 따라).

대화 상자가 사용자 작업에 대 한 응답에 표시 하 고 일반적으로 같은 방법으로 사용자가 작업을 완료할 수를 제공 합니다. 대화 상자 컴퓨터가 있어야 사용자의 응답을 닫을 수 있습니다.

Windows는 모덜리스 상태 (예: 여러 문서를 한 번에 열을 가질 수 있는 텍스트 편집기)에 사용 하거나 (예: 응용 프로그램을 계속 하려면 먼저 해제 해야 하는 내보내기 대화 상자) 모달 수 있습니다.

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)

이 문서는 기본적인 Xamarin.Mac 응용 프로그램에서 대화 상자 및 모달 창 작업 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>대화 상자에는 소개

대화 상자 (예: 파일을 저장 하는) 사용자 작업에 대 한 응답에 나타나고 사용자가 해당 작업을 완료할 수 있는 방법을 제공 합니다. 대화 상자 컴퓨터가 있어야 사용자의 응답을 닫을 수 있습니다.

Apple에 따라 대화 상자를 표시 하는 방법은 세 가지가 있습니다.

- **Modal 문서** -A 문서 모달 대화 상자에서 해제 될 때까지 지정된 된 문서 내에서 다른 작업을 실행 사용자 수 없습니다.
- **앱 모달** -An App 모달 대화 상자에서 해제 될 때까지 응용 프로그램이 상호 작용할 사용자 수 없습니다.
- **모덜리스** 는 모덜리스 대화 상자에서는 사용자가 여전히 문서 창과 상호 작용 하는 동안 대화 상자에서 설정을 변경할 수 있습니다.

### <a name="modal-window"></a>모달 창

모든 표준 `NSWindow` 모달 형식으로 표시 하 여 사용자 지정 된 대화도 사용할 수 있습니다.

[![](dialog-images/modal01.png "예제 모달 창")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>문서 모달 대화 상자 시트

A _시트_ 사용자 대화 상자를 닫고 될 때까지 창와 상호 작용 하는 것을 방지 하는 특정된 문서 창에 연결 하는 모달 대화 상자. 시트는 스레드가 나옵니다 하 고 한 번에 하나만 시트는 창에 대 한 열 수 있는 창에 연결 합니다.

[![](dialog-images/sheet08.png "예 모달 시트")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Windows 기본 설정

기본 설정은 사용자는 자주 변경 하는 응용 프로그램의 설정을 포함 하는 모덜리스 대화 상자. 기본 Windows에는 주로 다른 설정 그룹을 전환할 수 있도록 하는 도구 모음이 포함 됩니다.

[![](dialog-images/dialog02.png "예제에서는 기본 설정 창")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>열기 대화 상자

열기 대화 상자를 찾아서 응용 프로그램에서 항목을 열려면 일관 된 방식으로 사용자를 제공 합니다.

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>인쇄 및 페이지 설정 대화 상자

사용 하는 모든 응용 프로그램에서 경험 설정 대화 상자의 페이지 응용 프로그램은 사용자가 일관 된 인쇄를 가질 수 있도록 표시할 수 있는 및 macOS 표준 인쇄를 제공 합니다.

인쇄 대화 상자를 무료로 부동 대화 상자로 표시할 수 있습니다.

[![](dialog-images/print01.png "인쇄 대화 상자")](dialog-images/print01.png#lightbox)

또는 시트로 표시할 수 있습니다.

[![](dialog-images/print02.png "인쇄 시트")](dialog-images/print02.png#lightbox)

페이지 설정 대화 상자를 무료로 부동 대화 상자로 표시할 수 있습니다.

[![](dialog-images/print03.png "페이지 설정 대화 상자")](dialog-images/print03.png#lightbox)

또는 시트로 표시할 수 있습니다.

[![](dialog-images/print04.png "페이지 설정 시트")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>저장 대화 상자

저장 대화 상자는 사용자에 게 응용 프로그램에 항목을 저장 하는 일관 된 방법을 제공 합니다. 저장 대화 상자에 두 개의 상태: **최소** (축소 된 라고도 함):

[![](dialog-images/save01.png "저장 대화")](dialog-images/save01.png#lightbox)

및 **넓게** 상태:

[![](dialog-images/save02.png "확장 된 저장 대화 상자")](dialog-images/save02.png#lightbox)

**최소** 저장 대화 상자를 시트로 표시 될 수도 있습니다.

[![](dialog-images/save03.png "최소 시트 저장")](dialog-images/save03.png#lightbox)

마찬가지로 **넓게** 저장 대화 상자:

[![](dialog-images/save04.png "확장 된 시트 저장")](dialog-images/save04.png#lightbox)

자세한 내용은 참조는 [대화 상자의](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Apple의 섹션 [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>프로젝트에 모달 창 추가

기본 문서 창 외 Xamarin.Mac 응용 프로그램 기본 설정 또는 검사기 패널 등 사용자에 게 다른 유형의 windows 표시를 해야 합니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**열고는 `Main.storyboard` Xcode의 인터페이스 작성기에서 편집을 위해 파일입니다.
2. 새 끌어 **뷰-컨트롤러** 디자인 화면에:

    [![](dialog-images/new01.png "라이브러리에서 보기 컨트롤러를 선택 하면")](dialog-images/new01.png#lightbox)
3. 에 **Identity 관리자**, 입력 `CustomDialogController` 에 대 한는 **클래스 이름**: 

    [![](dialog-images/new02.png "설정 클래스 이름")](dialog-images/new02.png#lightbox)
4. Mac 용 Visual Studio로 다시 전환, xcode를 동기화 하 고 만들 수 있도록 허용 된 `CustomDialogController.h` 파일입니다.
5. Xcode 돌아가서 사용자 인터페이스를 디자인 합니다. 

    [![](dialog-images/new03.png "Xcode에서 UI 디자인")](dialog-images/new03.png#lightbox)
6. 만들기는 **모달 Segue** 대화는 대화 상자 창에 열립니다 UI 요소에서 컨트롤을 끌어 새 보기 컨트롤러에 응용 프로그램의 주 창에서. 할당 된 **식별자** `ModalSegue`: 

    [![](dialog-images/new06.png "모달 segue")](dialog-images/new06.png#lightbox)
6. 모든 통신 업 **동작** 및 **콘센트**: 

    [![](dialog-images/new04.png "작업 구성")](dialog-images/new04.png#lightbox)
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

확인 된 `CustomDialogController.cs` 다음과 같은 파일 보기:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

이 코드는 제목 및 대화 상자에 대 한 설명을 설정 하려면 몇 가지 속성 및 취소 되 고 또는 허용 되는 대화 상자에 대응 하는 몇 가지 이벤트를 노출 합니다.

다음에 편집는 `ViewController.cs` 파일, 재정의 `PrepareForSegue` 메서드 다음과 비슷하게 표시 및:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

이 코드는 Xcode의 인터페이스 작성기에서이 대화 상자를 정의 하는 segue를 초기화 하 고 제목 및 설명을 설정 합니다. 또한 선택 대화 상자에서 사용자가을 처리 합니다.

응용 프로그램을 실행 하 고 사용자 지정 대화 상자를 표시할 수 있습니다.

[![](dialog-images/new05.png "예제 대화 상자")](dialog-images/new05.png#lightbox)

Xamarin.Mac 응용 프로그램에서 기간을 사용 하는 방법에 대 한 자세한 내용은 참조 하십시오 우리의 [창 작업](~/mac/user-interface/window.md) 설명서입니다.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>사용자 지정 시트 만들기

A _시트_ 사용자 대화 상자를 닫고 될 때까지 창와 상호 작용 하는 것을 방지 하는 특정된 문서 창에 연결 하는 모달 대화 상자. 시트는 스레드가 나옵니다 하 고 한 번에 하나만 시트는 창에 대 한 열 수 있는 창에 연결 합니다. 

사용자 지정 시트 Xamarin.Mac를 만들려면 다음을 수행 하겠습니다.

1. 에 **솔루션 탐색기**열고는 `Main.storyboard` Xcode의 인터페이스 작성기에서 편집을 위해 파일입니다.
2. 새 끌어 **뷰-컨트롤러** 디자인 화면에:

    [![](dialog-images/new01.png "라이브러리에서 보기 컨트롤러를 선택 하면")](dialog-images/new01.png#lightbox)
2. 사용자 인터페이스 디자인:

    [![](dialog-images/sheet01.png "UI 디자인")](dialog-images/sheet01.png#lightbox)
3. 만들기는 **시트 Segue** 새 뷰 컨트롤러 프로그램 주 창에서: 

    [![](dialog-images/sheet02.png "시트 segue 유형 선택")](dialog-images/sheet02.png#lightbox)
4. 에 **Identity 관리자**, 보기 컨트롤러 이름을 **클래스** `SheetViewController`: 

    [![](dialog-images/sheet03.png "설정 클래스 이름")](dialog-images/sheet03.png#lightbox)
5. 필요한 모든 정의 **콘센트** 및 **동작**: 

    [![](dialog-images/sheet04.png "필요한 콘센트 및 작업 정의")](dialog-images/sheet04.png#lightbox)
6. 변경 내용을 저장 하 고 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.

다음에 편집는 `SheetViewController.cs` 파일을 다음과 같이 되도록 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

다음에 편집는 `ViewController.cs` 파일을 편집는 `PrepareForSegue` 메서드 다음과 같이 만듭니다.

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

응용 프로그램을 실행 하 고 시트를 열고, 창에 연결 됩니다.

[![](dialog-images/sheet08.png "예 시트")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>기본 설정 대화 상자 만들기

인터페이스 작성기의 기본 설정 보기를 레이아웃에서는 전에 기본 설정으로의 전환을 처리 하는 사용자 지정 segue 종류를 추가 해야 합니다. 프로젝트에 새 클래스를 추가 하 고 호출할 `ReplaceViewSeque `합니다. 클래스를 편집 하 고 다음과 비슷하게 표시 합니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

만든 사용자 지정 segue, 우리의 기본 설정을 처리 하는 Xcode의 인터페이스 작성기에서 새 창을 추가할 수 있습니다.

새 창에 추가 하려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**열고는 `Main.storyboard` Xcode의 인터페이스 작성기에서 편집을 위해 파일입니다.
2. 새 끌어 **창 컨트롤러** 디자인 화면에:

    [![](dialog-images/pref01.png "라이브러리에서 창 컨트롤러를 선택 합니다.")](dialog-images/pref01.png#lightbox)
3. 근처 창 배치는 **메뉴 모음** 디자이너:

    [![](dialog-images/pref02.png "새 창 추가")](dialog-images/pref02.png#lightbox)
4. 기본 설정 보기에서 탭 있을 연결 된 뷰 컨트롤러의 복사본을 만듭니다.

    [![](dialog-images/pref03.png "필요한 보기 컨트롤러 추가")](dialog-images/pref03.png#lightbox)
5. 새 끌어 **도구 모음 컨트롤러** 에서 **라이브러리**:

    [![](dialog-images/pref04.png "라이브러리에서 도구 모음 컨트롤러를 선택 합니다.")](dialog-images/pref04.png#lightbox)
6. 및의 창 디자인 화면에 놓습니다.

    [![](dialog-images/pref05.png "새 도구 모음 컨트롤러 추가")](dialog-images/pref05.png#lightbox)
7. 레이아웃 도구 모음 디자인:

    [![](dialog-images/pref06.png "레이아웃 도구 모음")](dialog-images/pref06.png#lightbox)
8. 컨트롤을 클릭 하 고 각에서 끌어 **도구 모음 단추** 위에서 만든 보기에 있습니다. 선택 된 **사용자 지정** segue 유형:

    [![](dialog-images/pref07.png "설정 segue 유형")](dialog-images/pref07.png#lightbox)
9. 새 Segue를 선택 하 고 설정 된 **클래스** 를 `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Segue 클래스를 설정합니다.")](dialog-images/pref08.png#lightbox)
10. 에 **Menubar 디자이너** 디자인 화면에서 응용 프로그램 메뉴에서 선택 **기본 설정 중...** control 클릭 하 고 만드는 기본 설정 창으로 끌어는 **표시** segue:

    [![](dialog-images/pref09.png "설정 segue 유형")](dialog-images/pref09.png#lightbox)
11. 변경 내용을 저장 하 고 동기화 하는 Mac에 대 한 Visual Studio로 돌아갑니다.

코드를 실행 하 고 선택 하는 경우는 **기본 설정 중...**  에서 **응용 프로그램 메뉴**, 창이 표시 됩니다.

[![](dialog-images/pref10.png "예제에서는 기본 설정 창")](dialog-images/pref10.png#lightbox)

작업 창 및 도구 모음에 대 한 자세한 내용은 참조 하십시오 우리의 [Windows](~/mac/user-interface/window.md) 및 [도구 모음](~/mac/user-interface/toolbar.md) 설명서입니다.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>저장 하 고 기본 설정을 로드 합니다.

일반적인 macOS 응용 프로그램에서에서 사용자가을 응용 프로그램의 사용자 기본 설정 중 하나를 변경할 때 해당 변경 내용이 자동으로 저장 됩니다. Xamarin.Mac 응용 프로그램에서이 처리 하는 가장 쉬운 방법은 시스템 수준의 모든 사용자의 기본 설정을 관리 하 고 공유 하는 단일 클래스를 만드는 것입니다.

먼저 추가 하는 새 `AppPreferences` 프로젝트에 클래스에서 상속 하 고 `NSObject`합니다. 기본 설정을 사용 하도록 디자인 됩니다 [데이터 바인딩 및 키-값 코딩](~/mac/app-fundamentals/databinding.md) 를 만드는 과정 하면 및 보다 간단 하 게 구성 된 기본 설정을 유지 관리 합니다. 기본 설정을 적은 양의 단순 데이터 형식으로 구성 됩니다, 이후 기본 제공 사용 `NSUserDefaults` 저장 하 고 값을 검색 합니다.

편집 된 `AppPreferences.cs` 파일을 다음과 같이 표시 하 게 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

이 클래스와 같은 몇 가지 도우미 루틴을 포함 `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, 작업할 수 있게 하는 등 `NSUserDefaults` 더 쉽습니다. 또한 `NSUserDefaults` 처리 하는 기본 제공 방법이 없는 `NSColors`, `NSColorToHexString` 및 `NSColorFromHexString` 메서드는 색 웹 기반 16 진수 문자열을 변환 하는 데 사용 됩니다 (`#RRGGBBAA` 여기서 `AA` 알파 투명도) 일 수 있는 쉽게 저장 하 고 검색 합니다.

에 `AppDelegate.cs` 파일의 인스턴스를 만들고,이 **AppPreferences** 응용 프로그램 수준의 사용 되는 개체:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>기본 설정 보기에 배선 기본 설정

그런 다음 위에서 만든 뷰와 기본 설정 창에서 UI 요소에 기본 클래스를 연결 합니다. 인터페이스 작성기에서 기본 설정 보기 컨트롤러를 선택 하 고로 전환는 **Identity 관리자**를 컨트롤러에 대 한 사용자 지정 클래스를 만듭니다. 

[![](dialog-images/prefs12.png "Identity 관리자")](dialog-images/prefs12.png#lightbox)

변경 내용을 동기화 하는 새로 만든된 클래스 편집을 위해 열 Mac 용 Visual Studio로 다시 전환 합니다. 다음과 같은 클래스를 확인 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

이 클래스가 여기에 두 가지 작업을 수행 했음을 확인: 먼저 도우미는 `App` 속성을 액세스 하는 **AppDelegate** 쉽게 합니다. 두 번째는 `Preferences` 전역 속성을 노출 **AppPreferences** 이 보기에 UI 컨트롤에서 데이터 바인딩 배치에 대 한 클래스입니다.

다음으로 두 번 스토리 보드 파일 다시 인터페이스 작성기에서 열 (및 방금 변경한 위 참조)을 클릭 합니다. 보기에는 기본 인터페이스를 빌드하는 데 필요한 모든 UI 컨트롤을 끕니다. 각 컨트롤에 대 한 전환 하는 **바인딩 검사기** 고의 각 속성에 바인딩하는 **AppPreference** 클래스:

[![](dialog-images/prefs13.png "바인딩 관리자")](dialog-images/prefs13.png#lightbox)

모든 패널 (뷰 컨트롤러)에 대해 위의 단계를 반복 하 고 필요한 기본 설정 속성.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>열려 있는 모든 창을 변경 기본 설정을 적용 합니다.

위에서 설명한 대로 응용 프로그램에 사용자가 해당 변경 내용이 응용 프로그램의 사용자 기본 설정 중 하나를 변경할 때 자동으로 저장 되어 있는 모든 창을에 적용 하는 일반적인 macOS에서, 사용자가 응용 프로그램에서 열려 있습니다.

신중 하 게 계획 하 고 응용 프로그램의 기본 설정 및 windows의 디자인이이 프로세스를 원활 하 고 투명 하 게 한 최소의 작업을 코딩 하 여 최종 사용자에 게 발생 하면 합니다.

모든 창을 사용할 응용 프로그램 기본 설정에 대해 다음 도우미 속성에 액세스할 수 있도록 해당 콘텐츠 보기 컨트롤러에 추가 우리의 **AppDelegate** 쉽게:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

다음으로, 내용이 나 사용자의 기본 설정에 따라 동작을 구성 하는 클래스를 추가 합니다.

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

사용자의 기본 설정에를 준수 하는지 확인 하는 창이 처음 열릴 때 구성 메서드를 호출 해야 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

다음으로 편집 된 `AppDelegate.cs` 파일 선택한 열려 있는 모든 창을 변경 모든 기본 설정을 적용 하기 위해 다음 메서드 추가:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

다음으로 추가 된 `PreferenceWindowDelegate` 프로젝트에 클래스와 다음과 비슷하게 표시:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

이렇게 하면 기본 창이 닫힙니다 때 열려 있는 모든 창에 전송 될 기본 설정을 변경 합니다.

마지막으로, 기본 설정 창 컨트롤러 편집을 위에서 만든 대리자를 추가 합니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

위치에서 이러한 모든 변화를 통해 사용자를 응용 프로그램의 기본 설정을 편집 하 고 기본 설정 창을 닫는 경우 변경 내용은에 적용될지 열려 있는 모든 창을:

[![](dialog-images/prefs14.png "예제에서는 기본 설정 창")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>열기 대화 상자

열기 대화 상자를 찾아서 응용 프로그램에서 항목을 열려면 일관 된 방식으로 사용자를 수 있습니다. 열기 대화 Xamarin.Mac 응용 프로그램에서을 표시 하려면 다음 코드를 사용 합니다.

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

위의 코드는 파일의 내용을 표시 하는 새 문서 창을 열으십시오. 이 대체 해야 응용 프로그램에 필요한 기능을 사용 하 여 코드입니다.

작업할 때 다음과 같은 속성을 사용할 수는 `NSOpenPanel`:

- **CanChooseFiles** -경우 `true` 사용자 파일을 선택할 수 있습니다.
- **CanChooseDirectories** -경우 `true` 사용자 디렉터리를 선택할 수 있습니다.
- **AllowsMultipleSelection** -경우 `true` 사용자는 한 번에 둘 이상의 파일을 선택할 수 있습니다.
- **ResolveAliases** -경우 `true` 선택 하 고 별칭을 원래 파일의 경로를 확인 합니다.
- **AllowedFileTypes** -은 사용자가 어느 한 확장으로 선택할 수 있는 파일 형식의 문자열 배열 또는 _유틸리티_합니다. 기본값은 `null`, 모든 파일을 열 수 있습니다.

`RunModal ()` 메서드 열기 대화 상자를 표시 하 고 사용자가 파일 또는 디렉터리 (지정 된 대로 속성에 의해)를 선택 하도록 허용 및 반환 `1` 클릭 하면는 **열려** 단추입니다.

Url로 사용자의 선택한 파일이 나 디렉터리를 반환 하는 열기 대화 상자는 `URL` 속성입니다.

프로그램을 실행 하 고 선택 하는 경우는 **열기...**  에서 항목의 **파일** 다음 메뉴가 표시 됩니다. 

[![](dialog-images/dialog03.png "열기 대화 상자")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>인쇄 및 페이지 설정 대화 상자

사용 하는 모든 응용 프로그램에서 경험 설정 대화 상자의 페이지 응용 프로그램은 사용자가 일관 된 인쇄를 가질 수 있도록 표시할 수 있는 및 macOS 표준 인쇄를 제공 합니다.

다음 코드는 표준 인쇄 대화 상자를 표시 됩니다.

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

설정 하는 경우는 `ShowPrintAsSheet` 속성을 `false`, 응용 프로그램을 실행 하 고 인쇄 대화 상자를 표시, 다음 표시 됩니다.

[![](dialog-images/print01.png "인쇄 대화 상자")](dialog-images/print01.png#lightbox)

경우 설정의 `ShowPrintAsSheet` 속성을 `true`, 응용 프로그램을 실행 하 고 인쇄 대화 상자를 표시, 다음 표시 됩니다.

[![](dialog-images/print02.png "인쇄 시트")](dialog-images/print02.png#lightbox)

다음 코드는 페이지 레이아웃 대화 상자를 표시 합니다.

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

설정 하는 경우는 `ShowPrintAsSheet` 속성을 `false`, 응용 프로그램을 실행 하 고 인쇄 모양 대화 상자를 표시, 다음 표시 됩니다.

[![](dialog-images/print03.png "페이지 설정 대화 상자")](dialog-images/print03.png#lightbox)

경우 설정의 `ShowPrintAsSheet` 속성을 `true`, 응용 프로그램을 실행 하 고 인쇄 모양 대화 상자를 표시, 다음 표시 됩니다.

[![](dialog-images/print04.png "페이지 설정 시트")](dialog-images/print04.png#lightbox)

작업에서 인쇄 및 설정 대화 상자의 페이지에 대 한 자세한 내용은 Apple의를 참조 하십시오 [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) 및 [인쇄 소개](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) 설명서입니다.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>저장 대화 상자

저장 대화 상자는 사용자에 게 응용 프로그램에 항목을 저장 하는 일관 된 방법을 제공 합니다.

다음 코드는 표준 저장 대화 상자를 표시 됩니다.

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` 속성은 사용자로 파일을 저장 하도록 선택할 수 있는 파일 형식의 문자열 배열입니다. 파일 형식 중 하나를 확장으로 지정할 수 있습니다 또는 _유틸리티_합니다. 기본값은 `null`, 모든 파일 형식을 사용할 수 있습니다.

설정 하는 경우는 `ShowSaveAsSheet` 속성을 `false`, 응용 프로그램을 실행 하 고 선택 **다른 이름으로 저장...**  에서 **파일** 메뉴에서 다음을 표시 됩니다.

[![](dialog-images/save01.png "A 저장 대화 상자")](dialog-images/save01.png#lightbox)

사용자는 대화 상자를 확장할 수 있습니다.

[![](dialog-images/save02.png "확장 된 저장 대화 상자")](dialog-images/save02.png#lightbox)

설정 하는 경우는 `ShowSaveAsSheet` 속성을 `true`, 응용 프로그램을 실행 하 고 선택 **다른 이름으로 저장...**  에서 **파일** 메뉴에서 다음을 표시 됩니다.

[![](dialog-images/save03.png "시트 저장")](dialog-images/save03.png#lightbox)

사용자는 대화 상자를 확장할 수 있습니다.

[![](dialog-images/save04.png "확장 된 시트 저장")](dialog-images/save04.png#lightbox)

작업 저장 대화 상자에 대 한 더 자세한 내용은 Apple의를 참조 하십시오 [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) 설명서입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에 자세히 살펴보고 모달 창, 시트 및 Xamarin.Mac 응용 프로그램에서 표준 시스템 대화 상자 작업을 수행 했습니다. 다양 한 유형 및 모달 창, 시트 및 대화 상자를 사용 하 여 살펴본를 만들어 모달 창 및 시트 Xcode에서 유지 관리의 인터페이스 작성기와 모달 창을 사용 하는 방법, 시트 및 C# 코드에서 대화 상자.

## <a name="related-links"></a>관련 링크

- [MacWindows (샘플)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [메뉴](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [도구 모음](~/mac/user-interface/toolbar.md)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [시트 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [인쇄 작업 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
