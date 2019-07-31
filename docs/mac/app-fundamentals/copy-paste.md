---
title: Xamarin.ios에서 복사 하 여 붙여넣기
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 복사 및 붙여넣기 기능을 제공 하기 위해 대지의 작업을 설명 합니다. 여러 앱 간에 공유할 수 있는 표준 데이터 형식으로 작업 하는 방법과 지정 된 앱 내에서 사용자 지정 데이터를 지 원하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 61b9d84d6d5882d447a78e6583a399013f8919ef
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656556"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Xamarin.ios에서 복사 하 여 붙여넣기

_이 문서에서는 Xamarin.ios 응용 프로그램에서 복사 및 붙여넣기 기능을 제공 하기 위해 대지의 작업을 설명 합니다. 여러 앱 간에 공유할 수 있는 표준 데이터 형식으로 작업 하는 방법과 지정 된 앱 내에서 사용자 지정 데이터를 지 원하는 방법을 보여 줍니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 목표-C에서 작업 하는 개발자에 게는 동일한 대지의 (복사 및 붙여넣기) 지원에 대 한 액세스 권한이 있습니다.

이 문서에서는 Xamarin.ios 앱에서 대지의 사용을 위한 두 가지 주요 방법을 설명 합니다.

1. **표준 데이터 형식** -대/소문자 작업은 일반적으로 관련 되지 않은 두 앱 간에 수행 되므로 앱은 다른 응용 프로그램에서 지 원하는 데이터 형식을 인식 하지 못합니다. 공유 가능성을 최대화 하기 위해, 대/표시는 특정 항목에 대 한 여러 표시를 보유할 수 있습니다 (일반적인 데이터 형식의 표준 집합을 사용 하 여) .이를 통해 사용 중인 앱이 요구 사항에 가장 적합 한 버전을 선택할 수 있습니다.
2. **사용자 지정 데이터** -xamarin.ios 내에서 복잡 한 데이터의 복사 및 붙여넣기를 지원 하기 위해 대지의 영향을 받을 사용자 지정 데이터 형식을 정의할 수 있습니다. 예를 들어 사용자가 여러 데이터 형식 및 점으로 구성 된 복잡 한 도형을 복사 하 고 붙여 넣을 수 있도록 하는 벡터 그리기 앱입니다.

[![실행 중인 앱의 예](copy-paste-images/intro01.png "실행 중인 앱의 예")](copy-paste-images/intro01-large.png#lightbox)

이 문서에서는 복사 및 붙여넣기 작업을 지원 하기 위해 Xamarin.ios 응용 프로그램에서 대지의 사용에 대 한 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 특성 요소 Objective-C 개체 및 UI에 C# 클래스를 연결 하는 데 사용 합니다.

## <a name="getting-started-with-the-pasteboard"></a>대지의 시작

이 대지의는 지정 된 응용 프로그램 내에서 또는 응용 프로그램 간에 데이터를 교환 하는 표준화 된 메커니즘을 제공 합니다. Xamarin.ios 응용 프로그램에서 일반적으로 사용 하는 작업은 복사 및 붙여넣기 작업을 처리 하는 것 이지만 다른 많은 작업도 지원 됩니다 (예: 끌어서 & Drop 및 애플리케이션 서비스).

신속 하 게 시작 하려면 Xamarin.ios 앱에서 pasteboards를 사용 하는 방법에 대 한 간단 하 고 실용적인 소개로 시작 하겠습니다. 나중에 대지의 작동 원리와 사용 되는 방법에 대 한 자세한 설명을 제공 합니다.

이 예에서는 이미지 뷰가 포함 된 창을 관리 하는 간단한 문서 기반 응용 프로그램을 만들 예정입니다. 사용자는 앱의 문서 간에 이미지를 복사 하 여 붙여넣을 수 있으며 동일한 앱 내의 다른 앱 또는 여러 창에서 이미지를 복사 하 여 붙여넣을 수 있습니다.

### <a name="creating-the-xamarin-project"></a>Xamarin 프로젝트 만들기

먼저 복사 및 붙여넣기 지원을 추가 하 게 될 새 문서 기반의 Xamarin.ios 앱을 만듭니다.

다음을 수행합니다.

1. Mac용 Visual Studio를 시작 하 고 **새 프로젝트** ... 링크를 클릭 합니다.
2. **Mac** > **앱**cocoa 앱을 선택 하 고 다음 단추를 클릭 합니다. >  

    [![새 Cocoa 앱 프로젝트 만들기](copy-paste-images/sample01.png "새 Cocoa 앱 프로젝트 만들기")](copy-paste-images/sample01-large.png#lightbox)
3. `MacCopyPaste` **프로젝트 이름** 으로를 입력 하 고 다른 모든 항목을 기본값으로 유지 합니다. 다음을 클릭 합니다. 

    [![프로젝트 이름 설정](copy-paste-images/sample01a.png "프로젝트 이름 설정")](copy-paste-images/sample01a-large.png#lightbox)

4. **만들기** 단추를 클릭 합니다. 

    [![새 프로젝트 설정 확인](copy-paste-images/sample02.png "새 프로젝트 설정 확인")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>NSDocument 추가

다음으로 응용 프로그램의 `NSDocument` 사용자 인터페이스에 대 한 백그라운드 저장소 역할을 하는 사용자 지정 클래스를 추가 합니다. 여기에는 단일 이미지 뷰가 포함 되어 있으며, 뷰에서 이미지를 기본 대/표시로 복사 하는 방법과 기본 대지의 이미지를 가져와서 이미지 뷰에 표시 하는 방법을 알고 있습니다.

**Solution Pad** 에서 xamarin.ios 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.

![프로젝트에 NSDocument 추가](copy-paste-images/sample03.png "프로젝트에 NSDocument 추가")

**이름**에 대해 `ImageDocument`를 입력하고 **새로 만들기** 단추를 클릭합니다. **ImageDocument.cs** 클래스를 편집 하 고 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

아래에 자세히 설명 된 코드 중 일부를 살펴보겠습니다.

다음 코드에서는 이미지를 사용할 수 `true` `false`있는 경우 기본 대지의 이미지 데이터가 있는지 테스트 하는 속성을 제공 합니다.

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

다음 코드에서는 연결 된 이미지 보기의 이미지를 기본 대지의로 복사 합니다.

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

다음 코드는 기본 대지의 이미지를 붙여넣고 첨부 된 이미지 보기에 표시 합니다 (대지의 올바른 이미지를 포함 하는 경우).

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

이 문서를 사용 하 여 Xamarin.ios 앱에 대 한 사용자 인터페이스를 만듭니다.

### <a name="building-the-user-interface"></a>사용자 인터페이스 빌드

**주 storyboard** 파일을 두 번 클릭 하 여 Xcode에서 엽니다. 그런 다음 도구 모음과 이미지 웰을 추가 하 고 다음과 같이 구성 합니다.

[![도구 모음 편집](copy-paste-images/sample04.png "도구 모음 편집")](copy-paste-images/sample04-large.png#lightbox)

도구 모음 왼쪽에 이미지 복사 및 붙여넣기 **도구 모음 항목** 을 추가 합니다. 편집 메뉴에서 복사 및 붙여넣기를 위한 바로 가기로 사용할 예정입니다. 그런 다음 도구 모음 오른쪽에 4 개의 **이미지 도구 모음 항목** 을 추가 합니다. 이를 사용 하 여 몇 가지 기본 이미지로 이미지를 채웁니다.

도구 모음을 사용 하는 방법에 대 한 자세한 내용은 [도구 모음](~/mac/user-interface/toolbar.md) 설명서를 참조 하세요.

다음으로 도구 모음 항목과 이미지 웰에 대해 다음과 같은 출 선 및 작업을 제공 하겠습니다.

[![콘센트 및 작업 만들기](copy-paste-images/sample05.png "콘센트 및 작업 만들기")](copy-paste-images/sample05-large.png#lightbox)

콘센트 및 작업으로 작업 하는 방법에 대 한 자세한 내용은 [Hello, Mac](~/mac/get-started/hello-mac.md) 설명서의 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 참조 하세요.

### <a name="enabling-the-user-interface"></a>사용자 인터페이스 사용

Xcode에서 만든 사용자 인터페이스와 콘센트 및 작업을 통해 노출 되는 UI 요소를 사용 하 여 UI를 사용 하도록 설정 하는 코드를 추가 해야 합니다. **Solution Pad** **ImageWindow.cs** 파일을 두 번 클릭 하면 다음과 같이 표시 됩니다.

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

아래에 자세히 설명 되어 있는 코드를 살펴보겠습니다.

먼저 위에서 만든 `ImageDocument` 클래스의 인스턴스를 노출 합니다.

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

, `Export` `Document` 및 를`DidChangeValue`사용 하 여 Xcode에서 키-값 코딩 및 데이터 바인딩을 허용 하도록 속성을 설정 했습니다. `WillChangeValue`

또한 다음 속성을 사용 하 여 Xcode의 UI에 추가한 이미지의 이미지를 노출 합니다.

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

주 창이 로드 되 고 표시 되 면 `ImageDocument` 클래스의 인스턴스를 만들고 다음 코드를 사용 하 여 UI의 이미지를 연결 합니다.

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

마지막으로, 복사 하 여 붙여넣기 도구 모음 항목을 클릭 하는 사용자에 대 한 응답으로 `ImageDocument` 클래스의 인스턴스를 호출 하 여 실제 작업을 수행 합니다.

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>파일 및 편집 메뉴 사용

마지막으로 수행 해야 할 일은 **파일** 메뉴 (주 창의 새 인스턴스 만들기)에서 **새** 메뉴 항목을 사용 하도록 설정 하 고 **편집** 메뉴에서 **잘라내기**, **복사** 및 **붙여넣기** 메뉴 항목을 사용 하도록 설정 하는 것입니다.

**새** 메뉴 항목을 사용 하도록 설정 하려면 **AppDelegate.cs** 파일을 편집 하 고 다음 코드를 추가 합니다.

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

자세한 내용은 [windows](~/mac/user-interface/window.md) 설명서의 [여러 창으로 작업](~/mac/user-interface/window.md) 섹션을 참조 하세요.

**잘라내기**, **복사** 및 **붙여넣기** 메뉴 항목을 사용 하도록 설정 하려면 **AppDelegate.cs** 파일을 편집 하 고 다음 코드를 추가 합니다.

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

각 메뉴 항목에 대해 현재, 최상위, 키 창을 가져와 `ImageWindow` 클래스로 캐스팅 합니다.

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

여기에서 해당 창의 `ImageDocument` 클래스 인스턴스를 호출 하 여 복사 및 붙여넣기 작업을 처리 합니다. 예: 

```csharp
window.Document.CopyImage (sender);
```

기본 대지의 이미지 또는 현재 활성 창의 이미지에 이미지 데이터가 있는 경우에만 **잘라내기**, **복사** 및 **붙여넣기** 메뉴 항목에 액세스할 수 있습니다.

Xamarin.ios 프로젝트에 **EditMenuDelegate.cs** 파일을 추가 하 고 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

이번에는 현재 최상위 창을 가져오고 해당 `ImageDocument` 클래스 인스턴스를 사용 하 여 필요한 이미지 데이터가 있는지 확인 합니다. 그런 다음 메서드를 `MenuWillHighlightItem` 사용 하 여이 상태를 기반으로 각 항목을 활성화 하거나 비활성화 합니다.

**AppDelegate.cs** 파일을 편집 하 고 메서드 `DidFinishLaunching` 를 다음과 같이 만듭니다.
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

먼저 편집 메뉴에서 메뉴 항목의 자동 사용 및 사용 안 함을 사용 하지 않도록 설정 합니다. 다음으로 위에서 만든 `EditMenuDelegate` 클래스의 인스턴스를 연결 합니다.

자세한 내용은 [메뉴](~/mac/user-interface/menu.md) 설명서를 참조 하세요.

### <a name="testing-the-app"></a>앱 테스트

모든 것이 준비 되 면 응용 프로그램을 테스트할 준비가 된 것입니다. 앱을 빌드하고 실행 하면 기본 인터페이스가 표시 됩니다.

![응용 프로그램 실행](copy-paste-images/run01.png "응용 프로그램 실행")

편집 메뉴를 여는 경우 이미지의 이미지 또는 기본 대지의 이미지가 없기 때문에 **잘라내기**, **복사** 및 **붙여넣기** 를 사용할 수 없습니다.

![편집 메뉴 열기](copy-paste-images/run02.png "편집 메뉴 열기")

이미지를 이미지에 추가 하 고 편집 메뉴를 다시 열면 이제 항목이 활성화 됩니다.

![편집 메뉴 항목을 사용 하도록 설정 하는 것을 보여 줍니다] . (copy-paste-images/run03.png "편집 메뉴 항목을 사용 하도록 설정 하는 것을 보여 줍니다") .

이미지를 복사 하 고 파일 메뉴에서 **새로 만들기** 를 선택 하는 경우 해당 이미지를 새 창에 붙여 넣을 수 있습니다.

![새 창에 이미지 붙여넣기](copy-paste-images/run04.png "새 창에 이미지 붙여넣기")

다음 섹션에서는 Xamarin.ios 응용 프로그램에서 대지의 작업을 자세히 살펴보겠습니다.

## <a name="about-the-pasteboard"></a>대지의 정보

Macos (이전의 OS X)에서 대지의 (`NSPasteboard`)는 복사 & 붙여넣기, 끌어서 놓기 & 애플리케이션 서비스 등의 여러 서버 프로세스를 지원 합니다. 다음 섹션에서는 몇 가지 주요 대지의 개념을 자세히 살펴보겠습니다.

### <a name="what-is-a-pasteboard"></a>대지의 정의

클래스 `NSPasteboard` 는 응용 프로그램 간에 정보를 교환 하거나 지정 된 앱 내에서 정보를 교환 하기 위한 표준화 된 메커니즘을 제공 합니다. 대지의 주요 기능은 복사 및 붙여넣기 작업을 처리 하기 위한 것입니다.

1. 사용자가 앱에서 항목을 선택 하 고 **잘라내기** 또는 **복사** 메뉴 항목을 사용 하는 경우 선택한 항목에 대 한 하나 이상의 표현이 대지의 위에 배치 됩니다.
2. 사용자가 동일한 앱 또는 다른 앱 내에서 **붙여넣기** 메뉴 항목을 사용 하는 경우 해당 항목에서 처리할 수 있는 데이터 버전이 대지의 복사 되 고 앱에 추가 됩니다.

Less 명백한 대지의 경우에는 찾기, 끌기, 끌어서 놓기 및 응용 프로그램 서비스 작업이 포함 됩니다.

- 사용자가 끌기 작업을 시작 하면 끌기 데이터가 대지의에 복사 됩니다. 끌기 작업이 다른 앱에 대 한 drop을 사용 하 여 종료 되는 경우 해당 앱은 대지의 데이터를 복사 합니다.
- 번역 서비스의 경우에는 요청 하는 앱에서 번역할 데이터가 대 면에 복사 됩니다. 응용 프로그램 서비스는 대지의 데이터를 검색 하 고, 번역을 수행한 다음, 다시 설정에 데이터를 붙여 넣습니다.

가장 간단한 형태의 pasteboards는 지정 된 앱 내에서 또는 앱과 줄임으로써 사이에 있는 데이터를 이동 하는 데 사용 됩니다. Pasteboards의 개념은 쉽게 사용할 수 있지만 몇 가지 복잡 한 세부 정보를 고려해 야 합니다. 이러한 내용은 아래에서 자세히 설명 합니다.

### <a name="named-pasteboards"></a>명명 된 pasteboards

대/는 공개 또는 비공개 일 수 있으며, 응용 프로그램 내에서 또는 여러 앱에서 다양 한 용도로 사용할 수 있습니다. macOS는 다음과 같이 잘 정의 된 특정 사용을 포함 하는 몇 가지 표준 pasteboards를 제공 합니다.

- `NSGeneralPboard`- **잘라내기**, **복사** 및 **붙여넣기** 작업의 기본 기능입니다.
- `NSRulerPboard`- **눈금자**에서 **잘라내기**, **복사** 및 **붙여넣기** 작업을 지원 합니다.
- `NSFontPboard`-개체에 대 `NSFont` 한 **잘라내기**, **복사** 및 **붙여넣기** 작업을 지원 합니다.
- `NSFindPboard`-검색 텍스트를 공유할 수 있는 응용 프로그램별 찾기 패널을 지원 합니다.
- `NSDragPboard`- **끌기 & Drop** 작업을 지원 합니다.

대부분의 경우 시스템 정의 pasteboards 중 하나를 사용 합니다. 그러나 사용자 고유의 pasteboards를 만들어야 하는 경우가 있을 수 있습니다. 이러한 경우 `FromName (string name)` `NSPasteboard` 클래스의 메서드를 사용 하 여 지정 된 이름의 사용자 지정 대지의 이름을 만들 수 있습니다.

필요에 따라 `CreateWithUniqueName` `NSPasteboard` 클래스의 메서드를 호출 하 여 고유 하 게 명명 된 대지의를 만들 수 있습니다.

### <a name="pasteboard-items"></a>대지의 항목

응용 프로그램이 아트 보드에 기록 하는 각 데이터는 대지의 _항목_ 으로 간주 되 고, 대/는 여러 항목을 동시에 포함할 수 있습니다. 이러한 방식으로 앱은 여러 버전의 데이터 (예: 일반 텍스트 및 서식 있는 텍스트)로 복사 되는 데이터를 작성 하 고, 검색 하는 앱은 처리할 수 있는 데이터만 읽을 수 있습니다 (예: 일반 텍스트에만 해당).

### <a name="data-representations-and-uniform-type-identifiers"></a>데이터 표현 및 균일 한 형식 식별자

대지의 작업은 일반적으로 서로를 인식 하지 않는 둘 이상의 응용 프로그램 또는 각 응용 프로그램에서 처리할 수 있는 데이터 형식 사이에 발생 합니다. 위의 섹션에서 설명한 것 처럼 공유 정보에 대 한 잠재적 가능성을 최대화 하기 위해 대지의는 복사 하 여 붙여 넣은 데이터의 여러 표현을 포함할 수 있습니다.

각 표현은 표시 되는 날짜의 형식을 고유 하 게 식별 하는 단순한 문자열 보다는 UTI (Uniform Type Identifier)를 통해 식별 됩니다. 자세한 내용은 Apple의 [균일 한 형식 식별자 개요](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) 를 참조 하세요. 설명서). 

사용자 지정 데이터 형식 (예: 벡터 그리기 앱에서 그리기 개체)을 만드는 경우 복사 및 붙여넣기 작업에서 고유 하 게 식별할 수 있도록 고유한 UTI를 만들 수 있습니다.

응용 프로그램에서 복사 된 데이터를 붙여넣기 위해 준비 하는 경우 해당 기능에 가장 적합 한 표현을 찾아야 합니다 (있는 경우). 일반적으로이는 사용할 수 있는 가장 다양 한 형식 (예: 워드 프로세싱 앱에 대 한 서식 있는 텍스트)으로, 필요한 경우 사용할 수 있는 가장 간단한 형식 (간단한 텍스트 편집기의 경우 일반 텍스트)으로 대체 됩니다.

<a name="Promised_Data" />

### <a name="promised-data"></a>약속 한 데이터

일반적으로 응용 프로그램 간 공유를 최대화 하기 위해 가능한 한 많은 데이터 표현을 제공 해야 합니다. 그러나 시간 또는 메모리 제약 조건으로 인해 실제로는 각 데이터 형식을 대지의에 작성 하는 것이 실용적이 지 않을 수 있습니다.

이 경우에는 대/소문자에 첫 번째 데이터 표현을 저장할 수 있으며, 받는 앱은 붙여넣기 작업 바로 전에 즉시 생성 될 수 있는 다른 표현을 요청할 수 있습니다.

항목을 표시 하는 경우에는 `NSPasteboardItemDataProvider` 인터페이스를 준수 하는 개체에서 사용할 수 있는 하나 이상의 다른 표현을 제공 하도록 지정 합니다. 이러한 개체는 수신 앱에서 요청 하는 요청에 대 한 추가 표현을 제공 합니다.

### <a name="change-count"></a>변경 수

각 대지의 새 소유자가 선언 될 때마다 증가 하는 _변경 횟수_ 를 유지 합니다. 앱은 변경 개수 값을 확인 하 여 마지막으로 검사 한 후에 대지의 내용이 변경 되었는지 여부를 확인할 수 있습니다.

클래스의`NSPasteboard` 및 `ClearContents` 메서드를 사용 하 여 지정 된 대지의 변경 횟수를 수정 합니다. `ChangeCount`

## <a name="copying-data-to-a-pasteboard"></a>대지의 데이터 복사

처음에는 대/소문자에 액세스 하 고, 기존 콘텐츠를 지우고, 대지의에 필요한 만큼의 데이터 표현을 작성 하 여 복사 작업을 수행 합니다.

예:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

일반적으로 위의 예제에서 수행한 것 처럼 일반 대 면에만 씁니다. `WriteObjects` *메서드에 보내는* 모든 개체는 `INSPasteboardWriting` 인터페이스를 따라야 합니다. `NSString` ,`NSImage` ,,`NSPasteboardItem`, 및와 같은 몇 가지 기본 제공 클래스는이 인터페이스를 자동으로 준수 합니다. `NSURL` `NSColor` `NSAttributedString`

사용자 지정 데이터 클래스를 대지의에 작성 하는 경우에는 `INSPasteboardWriting` 인터페이스를 준수 하거나 `NSPasteboardItem` 클래스의 인스턴스에 래핑해야 합니다 (아래의 [사용자 지정 데이터 형식](#Custom_Data_Types) 섹션 참조).

## <a name="reading-data-from-a-pasteboard"></a>대지의 데이터 읽기

위에서 설명한 것 처럼 앱 간에 데이터를 공유할 수 있는 가능성을 최대화 하기 위해 복사 된 데이터에 대 한 여러 표현이 대지의에 기록 될 수 있습니다. 수신 앱은 해당 기능에 사용할 수 있는 가장 다양 한 버전 (있는 경우)을 선택 합니다.

### <a name="simple-paste-operation"></a>단순 붙여넣기 작업

메서드를 `ReadObjectsForClasses` 사용 하 여 대지의 데이터를 읽습니다. 두 매개 변수가 필요 합니다.

1. 대지의 읽기를 `NSObject` 원하는 기반 클래스 형식의 배열입니다. 가장 적합 한 데이터 형식으로 순서를 지정 해야 합니다.
2. 추가 제약 조건 (예: 특정 URL 콘텐츠 형식으로 제한)을 포함 하는 사전 또는 추가 제약 조건이 필요 하지 않은 경우 빈 사전입니다.

메서드는 전달 된 조건을 충족 하는 항목의 배열을 반환 하므로 요청 된 데이터 형식 수가 대부분 포함 됩니다. 요청 된 형식이 없고 빈 배열이 반환 될 수도 있습니다.

예를 들어 다음 코드는가 일반 대지의에 `NSImage` 있는지 확인 하 고,이 경우 이미지 웰에 표시 합니다.

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>여러 데이터 형식 요청

생성 되는 Xamarin.ios 응용 프로그램의 형식에 따라 붙여넣은 데이터의 여러 표현을 처리할 수 있습니다. 이러한 상황에서 데이터를 검색 하는 두 가지 시나리오는 다음과 같습니다.

1. `ReadObjectsForClasses` 메서드를 한 번 호출 하 고 원하는 모든 표현의 배열을 기본 순서로 제공 합니다.
2. 매번 다른 형식의 배열을 요청 `ReadObjectsForClasses` 하는 메서드를 여러 번 호출 합니다.

작업 보드에서 데이터를 검색 하는 방법에 대 한 자세한 내용은 위의 **단순 붙여넣기 작업** 섹션을 참조 하세요.

### <a name="checking-for-existing-data-types"></a>기존 데이터 형식 확인

올바른 데이터가 있는 경우에만 **붙여넣기** 메뉴 항목을 사용 하도록 설정 하는 것과 같이 대지의 데이터를 실제로 읽지 않고 대지의 특정 데이터 표현이 포함 되어 있는지 확인 하는 것이 좋습니다.

대지의 메서드 `CanReadObjectForClasses` 를 호출 하 여 지정 된 형식이 포함 되어 있는지 확인 합니다.

예를 들어 다음 코드는 일반 대지의 `NSImage` 인스턴스가 포함 되어 있는지 여부를 확인 합니다.

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>대지의 url 읽기

지정 된 Xamarin.ios 앱의 기능을 기반으로 하는 경우에는 지정 된 조건 집합을 충족 하는 경우에만 (예: 특정 데이터 형식의 파일이 나 Url을 가리키는 경우)에만 대지의 Url을 읽을 수 있습니다. 이 경우 `CanReadObjectForClasses` 또는 `ReadObjectsForClasses` 메서드의 두 번째 매개 변수를 사용 하 여 추가 검색 조건을 지정할 수 있습니다.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>사용자 지정 데이터 형식

Xamarin.ios 앱의 대지의에 사용자 지정 형식을 저장 해야 하는 경우가 있습니다. 예를 들어 사용자가 그리기 개체를 복사 하 고 붙여 넣을 수 있도록 하는 벡터 그리기 앱입니다.

이 경우에서 `NSObject` 상속 하 고 몇 가지 `INSPasteboardWriting` 인터페이스 (`INSCoding`및 `INSPasteboardReading`)를 준수 하도록 데이터 사용자 지정 클래스를 디자인 해야 합니다. 필요에 따라를 사용 `NSPasteboardItem` 하 여 복사 하거나 붙여넣을 데이터를 캡슐화 할 수 있습니다.

이러한 두 옵션에 대해서는 아래에서 자세히 설명 합니다.

### <a name="using-a-custom-class"></a>사용자 지정 클래스 사용

이 섹션에서는이 문서의 시작 부분에서 만든 간단한 예제 앱을 확장 하 고 사용자 지정 클래스를 추가 하 여 windows 간에 복사 하 여 붙여 넣는 이미지에 대 한 정보를 추적 합니다.

프로젝트에 새 클래스를 추가 하 고 **ImageInfo.cs**를 호출 합니다. 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

다음 섹션에서는이 클래스를 자세히 살펴봅니다.

#### <a name="inheritance-and-interfaces"></a>상속 및 인터페이스

사용자 지정 데이터 클래스를 작성 하거나 대/ `INSPastebaordWriting` /문자에서 읽으려면 먼저 및 `INSPasteboardReading` 인터페이스를 준수 해야 합니다. 또한이 클래스는 `NSObject` `INSCoding` 에서 상속 하 고 인터페이스를 준수 해야 합니다.

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

또한 클래스는 `Register` 지시문을 사용 하 여 목표 C에 노출 되어야 하며를 사용 하 여 `Export`필요한 속성이 나 메서드를 노출 해야 합니다. 예를 들어:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

이 클래스에 포함 되는 데이터의 두 필드를 표시 합니다. 이미지 이름과 해당 형식 (jpg, png 등)이 포함 됩니다. 

자세한 내용은 [xamarin.ios 내부](~/mac/internals/how-it-works.md) `Register` 설명서의 [ C# 클래스/메서드를 목표로 제공-C](~/mac/internals/how-it-works.md) 섹션을 참조 하십시오. C# 클래스를 연결 하는 데 사용 되 `Export` 는 및 특성에 대해 설명 합니다. 목표-C 개체 및 UI 요소입니다.

#### <a name="constructors"></a>생성자

사용자 지정 데이터 클래스에 대 한 두 가지 생성자 (목적-C에 올바르게 노출 됨)는 대/소문자에서 읽을 수 있도록 필요 합니다.

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

먼저의 `init`기본 목표-C 메서드에서 _빈_ 생성자를 노출 합니다.

다음으로,의 `NSCoding` `initWithCoder`내보낸 이름으로 붙여넣을 때 대지의 새 개체 인스턴스를 만드는 데 사용 되는 호환 생성자를 노출 합니다.

이 생성자는를 `NSCoder` 사용 하 여에 게 `NSKeyedArchiver` 작성 될 때에 의해 생성 되는을 사용 하 고 키/값 쌍 데이터를 추출 하 여 데이터 클래스의 속성 필드에 저장 합니다.

#### <a name="writing-to-the-pasteboard"></a>대지의에 쓰기

`INSPasteboardWriting` 인터페이스를 준수 하 여 두 개의 메서드를 노출 하 고 필요에 따라 세 번째 메서드를 표시 하 여 클래스를 대지의에 쓸 수 있도록 해야 합니다.

먼저 사용자 지정 클래스를 쓸 수 있는 데이터 형식 표현에 대 한 정보를 지시 해야 합니다.

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

각 표현은 표시 되는 데이터의 형식을 고유 하 게 식별 하는 단순한 문자열 보다는 UTI (Uniform Type Identifier)를 통해 식별 됩니다. 자세한 내용은 Apple의 [균일 한 형식 식별자 개요](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) 를 참조 하세요. 설명서).

사용자 지정 형식에 대해 사용자 고유의 UTI을 만듭니다. "" (는 앱 식별자와 마찬가지로 역방향 표기법). 또한 클래스는 표준 문자열을 대지의 (`public.text`)에 쓸 수 있습니다. 

다음에는 실제로 대/면에 작성 되는 요청 된 형식으로 개체를 만들어야 합니다.

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

형식의 경우 간단한 형식의 `NSString` 개체를 반환 합니다. `public.text` 사용자 지정 `com.xamarin.image-info` 형식의 경우 `NSKeyedArchiver` 및 `NSCoder` 인터페이스를 사용 하 여 사용자 지정 데이터 클래스를 키/값 쌍 보관으로 인코딩합니다. 인코딩을 실제로 처리 하려면 다음 메서드를 구현 해야 합니다.

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

개별 키/값 쌍이 인코더에 기록 되 고 위에서 추가한 두 번째 생성자를 사용 하 여 디코딩됩니다.

필요에 따라 다음 메서드를 포함 하 여 데이터를 대지의에 쓸 때 옵션을 정의할 수 있습니다.

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

`WritingPromised` 현재 옵션은 사용할 수 있으며, 지정 된 형식이 약속 한 경우에만 사용할 수 있으며 실제로는 대/면에 기록 되지 않습니다. 자세한 내용은 위의 [약속 데이터](#Promised_Data) 섹션을 참조 하세요.

이러한 메서드를 사용 하는 경우 다음 코드를 사용 하 여 사용자 지정 클래스를 대지의에 쓸 수 있습니다.

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>대지의 읽기

`INSPasteboardReading` 인터페이스를 준수 하 여 사용자 지정 데이터 클래스를 대지의에서 읽을 수 있도록 세 가지 메서드를 노출 해야 합니다.

먼저 사용자 지정 클래스가 클립보드에서 읽을 수 있는 데이터 형식 표현에 대 한 정보를 지시 해야 합니다.

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

다시 말하지만, 이러한 항목은 단순 UTIs로 정의 되며 위의 **대지의에 쓰기** 에 정의 된 것과 동일한 형식입니다.

다음으로, 다음 메서드를 사용 하 여 각 UTI 유형을 _어떻게_ 읽을 지를 지시 해야 합니다.

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

형식에 대해 클래스에 추가 된 생성자를 `initWithCoder:` 호출 하 여에 대 한 클래스를 작성할 때를 `NSKeyedArchiver` 사용 하 여 만든 키/값 쌍을 디코드 하도록 대지의 지시를 합니다. `com.xamarin.image-info`

마지막으로, 다음 메서드를 추가 하 여 대지의 다른 UTI 데이터 표현을 읽어야 합니다.

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

이러한 모든 메서드를 적용 한 후에는 다음 코드를 사용 하 여 사용자 지정 데이터 클래스를 대지의에서 읽을 수 있습니다.

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>NSPasteboardItem 사용

사용자 지정 클래스 만들기를 보증 하지 않는 사용자 지정 항목을에 써야 하는 경우 또는 필요한 경우에만 공통 형식으로 데이터를 제공 하려는 경우가 있습니다. 이러한 경우에는를 `NSPasteboardItem`사용할 수 있습니다.

는 `NSPasteboardItem` 대/소문자에 작성 된 데이터에 대 한 세분화 된 제어를 제공 하며 임시 액세스용으로 설계 된 데이터에 대 한 세분화 된 제어를 제공 합니다 .이는 대/소문자에 쓰여진 후에 삭제 해야 합니다.

#### <a name="writing-data"></a>데이터 쓰기

사용자 지정 데이터 `NSPasteboardItem` 를에 쓰려면 사용자 지정 `NSPasteboardItemDataProvider`를 제공 해야 합니다. 프로젝트에 새 클래스를 추가 하 고 **ImageInfoDataProvider.cs**를 호출 합니다. 파일을 편집 하 여 다음과 같이 만듭니다.

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

사용자 지정 데이터 클래스를 사용 하는 것 처럼 `Register` 및 `Export` 지시문을 사용 하 여 목표에 노출 해야 합니다. C. 클래스는에서 `NSPasteboardItemDataProvider` 상속 해야 하 고 및 `ProvideDataForType` 메서드 `FinishedWithDataProvider` 를 구현 해야 합니다.

메서드를 사용 하 여 다음과 `NSPasteboardItem` 같이에 래핑되는 데이터를 제공 합니다. `ProvideDataForType`

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

이 경우 이미지에 대 한 두 가지 정보 (이름 및 ImageType)를 저장 하 고 단순 문자열 (`public.text`)에 기록 합니다.

데이터를 대/쓰기로 쓰기를 입력 하 고 다음 코드를 사용 합니다.

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>데이터 읽기

대지의 데이터를 다시 읽으려면 다음 코드를 사용 합니다.

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>요약

이 문서에서는 복사 및 붙여넣기 작업을 지원 하기 위해 Xamarin.ios 응용 프로그램의 대지의 사용에 대해 자세히 살펴봅니다. 먼저 표준 pasteboards 작업에 익숙해질 수 있는 간단한 예제를 소개 했습니다. 다음으로, 대지의 데이터를 읽고 쓰는 방법에 대해 자세히 살펴봅니다. 마지막으로, 사용자 지정 데이터 형식을 사용 하 여 앱 내에서 복잡 한 데이터 형식의 복사 및 붙여넣기를 지원 하는 방법을 살펴보았습니다.



## <a name="related-links"></a>관련 링크

- [MacCopyPaste (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccopypaste)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [대지의 프로그래밍 가이드](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
