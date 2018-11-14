---
title: Mac 플랫폼 설정
description: 이 문서에서는 macOS Sierra 및 El Capitan macOS에서 실행할 수 있는 앱을 생성 하는 Xamarin.Forms 프로젝트 Mac 프로젝트를 추가 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: EEC549E0-F182-4F9C-B2BA-B31D19569AA5
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2017
ms.openlocfilehash: 1aa21a416f4abca0440e96e25aebe5f834a717ce
ms.sourcegitcommit: f3f28722198e172d81c16bdeab0cb0a581a08dd0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51598875"
---
# <a name="mac-platform-setup"></a>Mac 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

시작 하기 전에 만들고 (또는 기존 사용할) Xamarin.Forms 프로젝트입니다.
Mac 용 Visual Studio를 사용 하 여 Mac 앱만 추가할 수 있습니다.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**추가 macOS 프로젝트 Xamarin.Forms에서 [Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-mac-app"></a>Mac 앱을 추가합니다.

MacOS Sierra 및 El Capitan macOS에서 실행 되는 Mac 앱을 추가 하려면 이러한 지침을 따릅니다.

1. Mac 용 Visual Studio에서 기존 Xamarin.Forms 솔루션을 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 프로젝트 추가...**

2. 에 **새 프로젝트** 창 **Mac > 앱 > Cocoa 앱** 누릅니다 **다음**합니다.

3. 형식은 **앱 이름** (및 필요에 따라 Dock 항목에 다른 이름을 선택) 키를 누릅니다 **다음**합니다.

4. 구성 및 키를 눌러 검토 **만들기**합니다. 이러한 단계는 아래에 나와 있습니다.

  ![Cocoa 앱을 추가 하는 방법을 보여 주는 애니메이션된 지침](mac-images/add-macos-proj.gif)

5. Mac 프로젝트에서 마우스 오른쪽 단추로 클릭 **패키지 > 패키지 추가...**  추가할 합니다 [Xamarin.Forms/2.3.5.235-pre2](https://www.nuget.org/packages/Xamarin.Forms/2.3.5.235-pre2) NuGet 합니다. 또한이 버전을 다른 프로젝트를 업데이트 해야 합니다.

6. Mac 프로젝트에서 마우스 오른쪽 단추로 클릭 **참조가** Xamarin.Forms 프로젝트 (프로젝트 공유 또는.NET Standard 라이브러리 프로젝트)에 대 한 참조를 추가 합니다.

  ![Xamarin.Forms 공유 코드 프로젝트에 대 한 참조를 추가 합니다.](mac-images/references-sml.png)

7. 업데이트 **Main.cs** 초기화를 `AppDelegate`:

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

8. 업데이트 `AppDelegate` Xamarin.Forms를 초기화 하는 창을 만들고 Xamarin.Forms 응용 프로그램을 로드 (적절 한 설정으로 `Title`). _를 초기화 해야 하는 다른 종속성이 있는 경우 여기에서 수행할도 합니다._

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

9. 두 번 클릭 **Main.storyboard** Xcode에서 편집 합니다. 선택 합니다 **창을** 및 _의 선택을 취소_ 는 **초기 컨트롤러는** (이 위의 코드 창을 만들기 때문에) 확인란을 선택:

  [![Xcode에서 초기 컨트롤러는 확인란의 선택을 취소합니다](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

  원치 않는 항목을 제거 하는 스토리 보드의 메뉴 시스템을 편집할 수 있습니다.

10. 마지막으로 모든 로컬 리소스 (예: 추가 이미지 파일)는 필요한 기존 플랫폼 프로젝트에서.

11. 이제 Mac 프로젝트 macos Xamarin.Forms 코드를 실행 해야 합니다!

## <a name="next-steps"></a>다음 단계

### <a name="styling"></a>스타일 지정

최근 변경 내용으로 `OnPlatform` 플랫폼을 개수에 관계 없이 이제 대상 지정할 수 있습니다. MacOS 포함합니다.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

다음과 같은 플랫폼에서 배로 수도 있습니다 참고: `<On Platform="iOS, macOS" ...>`합니다.

### <a name="window-size-and-position"></a>창 크기와 위치

초기 크기와 창의 위치를 조정할 수 있습니다는 `AppDelegate`:

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>알려진 문제

이것이 미리 하므로 해야 프로덕션을 준비 하는 것은 아닙니다. 다음은 몇 가지 macOS 프로젝트를 추가할 때 발생할 수 있습니다.

### <a name="not-all-nugets-are-ready-for-macos"></a>일부 Nuget macOS에 대 한 준비가 되었습니다.

패키지는 macOS 프로젝트에서 작동 하도록 "xamarinmac20" 대상으로 해야 합니다. 사용 하면 라이브러리의 일부 아직 지원 되지 않으므로 macOS 알 수 있습니다.

이 경우 추가 프로젝트 유지 관리자에 게 요청 해야 합니다. 지원, 있어야 대안을 찾아보세요 해야 합니다.

### <a name="missing-xamarinforms-features"></a>누락 된 Xamarin.Forms 기능

모든 Xamarin.Forms 기능은이 미리 보기에에서 완료 목록이 아직 구현 되지 않은 기능 중 일부는 다음과 같습니다.

* 바닥글
* 이미지 – 측면
* 새로 고침, SeparatorColor, SeparatorVisibility를 ListView – ScrollTo, UnevenRows 지원
* MasterDetailPage-BackgroundColor
* 탐색-InsertPageBefore
* OpenGLRenderer
* 선택 – Bindable/Observable 구현
* TabbedPage – BarBackgroundColor, BarTextColor
* TableView – UnevenRows
* ViewCell – IsEnabled ForceUpdateSize
* WebView – 대부분의 WebNavigationEvents


## <a name="related-links"></a>관련 링크

- [Xamarin.Mac](~/mac/index.yml)
