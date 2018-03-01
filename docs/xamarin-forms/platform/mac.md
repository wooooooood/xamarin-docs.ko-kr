---
title: "Mac 플랫폼 설정"
description: "Xamarin.Forms에는 Mac 플랫폼에 대 한 미리 보기 지원"
ms.topic: article
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: bda207796d1019f8188176acce055d782cb9e32d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="mac-platform-setup"></a>Mac 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

시작 하기 전에 만듭니다 (또는 기존 사용) Xamarin.Forms 프로젝트.
Mac 앱 Mac.에 대 한 Visual Studio를 사용 하 여 추가할 수 있습니다.

## <a name="adding-a-mac-app"></a>Mac 앱 추가

시에라 및 Mac OS X El Capitan macOS에서 실행 되는 Mac 응용 프로그램을 추가 하려면 다음이 지침을 따릅니다.

1. Mac 용 Visual Studio에서 기존 Xamarin.Forms 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...**

2. 에 **새 프로젝트** 창 선택 **Mac > 앱 > Cocoa 앱** 한 키를 누릅니다 **다음**합니다.

3. 종류는 **응용 프로그램 이름** (및 필요에 따라 도킹 항목에 대 한 다른 이름을 선택) 키를 누릅니다 **다음**합니다.

4. 구성 및 키를 눌러 검토 **만들기**합니다. 이러한 단계는 아래에 나와 있습니다.

  ![Cocoa 응용 프로그램을 추가 하는 방법을 보여 주는 애니메이션된 지침](mac-images/add-macos-proj.gif)

5. Mac 프로젝트에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가 중...**  추가 하는 [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet 합니다. 또한이 버전을 다른 프로젝트를 업데이트 해야 합니다.

6. Mac 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조** Xamarin.Forms 프로젝트 (공유 프로젝트 또는 PCL)에 대 한 참조를 추가 합니다.

  ![Xamarin.Forms 공유 코드 프로젝트에 대 한 참조를 추가 합니다.](mac-images/references-sml.png)

7. 업데이트 **Main.cs** 초기화 하는 `AppDelegate`:

    ```csharp
    static class MainClass
    {
        static void Main(string[] args)
        {
            NSApplication.Init();
            NSApplication.SharedApplication.Delegate = new AppDelegate(); // add this line
            NSApplication.Main(args);
        }
    }
    ```

8. 업데이트 `AppDelegate` Xamarin.Forms를 초기화 하는 창을 만들고 Xamarin.Forms 응용 프로그램을 로드 (를 적절 한 설정 `Title`). _초기화 하는 다른 종속성을 가져서는 않으며도 합니다._

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.MacOS;
    // also add a using for the Xamarin.Forms project, if the namespace is different to this file
    ...
    [Register("AppDelegate")]
    public class AppDelegate : FormsApplicationDelegate
    {
        NSWindow window;
        public AppDelegate()
        {
            var style = NSWindowStyle.Closable | NSWindowStyle.Resizable | NSWindowStyle.Titled;

            var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);
            window = new NSWindow(rect, style, NSBackingStore.Buffered, false);
            window.Title = "Xamarin.Forms on Mac!"; // choose your own Title here
            window.TitleVisibility = NSWindowTitleVisibility.Hidden;
        }

        public override NSWindow MainWindow
        {
            get { return window; }
        }

        public override void DidFinishLaunching(NSNotification notification)
        {
            Forms.Init();
            LoadApplication(new App());
            base.DidFinishLaunching(notification);
        }
    }
    ```

9. 두 번 클릭 **Main.storyboard** Xcode에서 편집할 수 있습니다. 선택 된 **창** 및 _의 선택을 취소_ 는 **초기 컨트롤러는** 확인란 (이 위의 코드 창을 만들기 때문에):

  [ ![Xcode에서 초기 컨트롤러는 확인란을 선택 취소](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png)

  원치 않는 항목을 제거 하려면 스토리 보드의 메뉴 시스템을 편집할 수 있습니다.

10. 마지막으로, 로컬 리소스를 않음 (예: 추가 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

11. 이제 Mac 프로젝트 macOS에서 Xamarin.Forms 코드를 실행 합니다!

## <a name="next-steps"></a>다음 단계

### <a name="styling"></a>스타일 지정

최근 변경 내용으로 `OnPlatform` 플랫폼을 개수에 관계 없이 대상 이제 지정할 수 있습니다. MacOS 포함 됩니다.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

다음과 같은 플랫폼에서 배로 또한 수 참고: `<On Platform="iOS, macOS" ...>`합니다.

### <a name="window-size-and-position"></a>창 크기와 위치

초기 크기 및 창 위치를 조정할 수 있습니다는 `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>알려진 문제

이것은 하지를 프로덕션 준비가 될 때 이므로 미리 보기입니다. 다음은 몇 가지 macOS 프로젝트에 추가 하면 발생할 수 있습니다.

### <a name="not-all-nugets-are-ready-for-macos"></a>일부 NuGets macOS에 대 한 준비가 되었습니다.

패키지는 macOS 프로젝트에서 작동 하도록 "xamarinmac20" 대상으로 해야 합니다. 사용 하면 라이브러리의 일부를 지원 하지 않는 macOS 알 수 있습니다.

이 경우 추가 프로젝트의 유지 관리자에 게 요청 해야 합니다. 지원 있을 때까지 대안에 대 한 확인 해야 합니다.

### <a name="missing-xamarinforms-features"></a>누락 된 Xamarin.Forms 기능

일부 Xamarin.Forms 기능은이 미리 보기; 완료 다음은 아직 구현 되지 않은 기능 중 일부 목록입니다.

* 바닥글
* 이미지 – 양상
* ListView – ScrollTo, UnevenRows 지원, 새로 고치거 나, SeparatorColor, SeparatorVisibility
* MasterDetailPage – BackgroundColor
* 탐색-InsertPageBefore
* OpenGLRenderer
* 선택 – 바인딩 가능한/Observable 구현
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView – UnevenRows
* ViewCell – IsEnabled, ForceUpdateSize
* 웹 보기 – 대부분 WebNavigationEvents


## <a name="related-links"></a>관련 링크

- [Xamarin.Mac](~/mac/index.yml)
