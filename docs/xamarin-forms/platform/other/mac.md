---
title: ''
description: 이 문서에서는 Xamarin.Forms macOS Sierra 및 macOS El Capitan에서 실행할 수 있는 앱을 생성 하는 Mac 프로젝트를 프로젝트에 추가 하는 방법을 설명 합니다.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
ms.custom: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 11897d2d3b8b7ba0a62956f1dbe4d8b873352e7a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139557"
---
# <a name="mac-platform-setup"></a>Mac 플랫폼 설정

![미리 보기](~/media/shared/preview.png)

시작 하기 전에 기존 프로젝트를 만들거나 기존 프로젝트를 사용 Xamarin.Forms 합니다. Mac용 Visual Studio를 사용 하 여 Mac 앱만 추가할 수 있습니다.

> [!VIDEO https://youtube.com/embed/mvQ7jzaNseM]

**비디오에 macOS 프로젝트 추가 Xamarin.Forms**

## <a name="adding-a-mac-app"></a>Mac 앱 추가

다음 지침에 따라 macOS Sierra 및 macOS El Capitan에서 실행 되는 Mac 앱을 추가 합니다.

1. Mac용 Visual Studio에서 기존 솔루션을 마우스 오른쪽 단추로 클릭 Xamarin.Forms 하 고 **추가 > 새 프로젝트 추가** ...를 선택 합니다.

2. **새 프로젝트** 창에서 **Mac > 앱 > cocoa 앱** 을 선택 하 고 **다음**을 누릅니다.

3. **앱 이름** 을 입력 하 고 필요에 따라 도크 항목의 다른 이름을 선택한 후 **다음**을 누릅니다.

4. 구성을 검토 하 고 **만들기**를 누릅니다. 이러한 단계는 아래에 나와 있습니다.

    ![Cocoa 앱을 추가 하는 방법을 보여 주는 애니메이션 지침](mac-images/add-macos-proj.gif)

5. Mac 프로젝트에서 패키지를 마우스 오른쪽 단추로 클릭 하 **> 패키지 추가** ...를 클릭 하 여 NuGet을 추가 합니다. [Xamarin.Forms](https://www.nuget.org/packages/Xamarin.Forms/) 또한 동일한 버전의 NuGet 패키지를 사용 하도록 다른 프로젝트를 업데이트 해야 합니다 Xamarin.Forms .

6. Mac 프로젝트에서 **참조** 를 마우스 오른쪽 단추로 클릭 하 고 Xamarin.Forms 프로젝트 (공유 프로젝트 또는 .NET Standard 라이브러리 프로젝트)에 대 한 참조를 추가 합니다.

    ![공유 코드 프로젝트에 대 한 참조 추가 Xamarin.Forms](mac-images/references-sml.png)

7. **Main.cs** 를 업데이트 하 여를 초기화 합니다 `AppDelegate` .

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

8. 를 `AppDelegate` 초기화 하 Xamarin.Forms 고, 창을 만들며, Xamarin.Forms 응용 프로그램을 로드 (적절 한 설정에 기억 `Title` ) 할 업데이트입니다. _초기화 해야 하는 다른 종속성이 있는 경우 여기에서 작업을 수행 합니다._

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

9. Xcode에서 편집할 **주. storyboard** 를 두 번 클릭 합니다. **창을** 선택 하 고 **초기 컨트롤러** 확인란의 _선택을 취소_ 합니다 .이는 위의 코드에서 창을 만들기 때문입니다.

    [![Xcode에서 초기 컨트롤러 확인란의 선택을 취소 합니다.](mac-images/xcode-init-controller-sml.png)](mac-images/xcode-init-controller.png#lightbox)

    스토리 보드에서 메뉴 시스템을 편집 하 여 원치 않는 항목을 제거할 수 있습니다.

10. 마지막으로 로컬 리소스를 추가 합니다 (예: 이미지 파일)을 입력 합니다.

11. 이제 macOS에서 Mac 프로젝트가 코드를 실행 해야 합니다 Xamarin.Forms .

## <a name="next-steps"></a>다음 단계

### <a name="styling"></a>스타일 지정

에 대 한 최근 변경 내용으로 이제 원하는 수 `OnPlatform` 의 플랫폼을 대상으로 지정할 수 있습니다. 여기에는 macOS가 포함 됩니다.

```xml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White"/>
        <On Platform="macOS" Value="White"/>
        <On Platform="Android" Value="Black"/>
    </OnPlatform>
</Button.TextColor>
```

참고:와 같은 플랫폼을 사용할 수도 있습니다 `<On Platform="iOS, macOS" ...>` .

### <a name="window-size-and-position"></a>창 크기 및 위치

에서 창의 초기 크기와 위치를 조정할 수 있습니다 `AppDelegate` .

```csharp
var rect = new CoreGraphics.CGRect(200, 1000, 1024, 768);  // x, y, width, height
```

## <a name="known-issues"></a>알려진 문제

이는 미리 보기 이므로 프로덕션이 준비 되지 않은 것으로 간주 됩니다. 다음은 프로젝트에 macOS를 추가할 때 발생할 수 있는 몇 가지 사항입니다.

### <a name="not-all-nugets-are-ready-for-macos"></a>모든 Nuget가 macOS에 대해 준비 된 것은 아닙니다.

사용 하는 라이브러리 중 일부는 아직 macOS를 지원 하지 않는 것을 알 수 있습니다. 이 경우 프로젝트의 유지 관리자에 게 요청을 보내 해당 요청을 추가 해야 합니다. 지원이 필요할 때까지 대안을 찾을 수 있습니다.

### <a name="missing-xamarinforms-features"></a>누락 된 Xamarin.Forms 기능

모든 Xamarin.Forms 기능이이 미리 보기에서 완료 되는 것은 아닙니다. 자세한 내용은 GitHub 리포지토리에서 [플랫폼 지원 macOS 상태](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support-macOS-Status) 를 참조 하세요 [Xamarin.Forms](https://github.com/xamarin/Xamarin.Forms) .

## <a name="related-links"></a>관련 링크

- [Xamarin.Mac](~/mac/index.yml)
