---
title: Xamarin.Mac에 OpenTK 소개
description: 이 문서에서는 OpenTK Xamarin.Mac 응용 프로그램에서 작업에 대해 소개 합니다. 설명 생성 및 게임 창 유지 관리, 단순 개체 렌더링 및 사용자에 게 개체를 표시 합니다.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 448b8bdba8ccedbb732a73a265d0ce76bb589190
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792608"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Xamarin.Mac에 OpenTK 소개

OpenTK (The Open Toolkit)은 고급, 하위 수준 C# 라이브러리 OpenAL, OpenGL 및 OpenCL와 작업을 쉽게 합니다. OpenTK 게임, 과학 응용 프로그램 또는 다른 3D 그래픽을 필요로 하는 프로젝트, 오디오 또는 계산 기능에 사용할 수 있습니다. 이 문서에서는 OpenTK Xamarin.Mac 응용 프로그램에서 사용 하 여에 대 한 간략 한 소개를 제공 합니다.

[![](opentk-images/intro01.png "실행 하는 예제 응용 프로그램")](opentk-images/intro01.png#lightbox)

이 문서는 기본적인 OpenTK Xamarin.Mac 응용 프로그램에서 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.

참조 하려는 경우는 [노출 C# 클래스 / Objective-c 하는 메서드를](~/mac/internals/how-it-works.md) 의 섹션은 [Xamarin.Mac 내부](~/mac/internals/how-it-works.md) 설명도 문서는 `Register` 및 `Export` 명령 와이어 접속 C# 클래스 Objective-c 개체 및 UI 요소에 하는 데 사용 합니다.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK에 대 한

위에서 설명 했 듯이 OpenTK (The Open Toolkit)은 고급, 낮은 수준의 C# 라이브러리 OpenAL, OpenGL 및 OpenCL와 작업을 쉽게. OpenTK Xamarin.Mac 응용 프로그램에서 사용 하 여 다음과 같은 기능을 제공 합니다.

- **신속 하 게 개발** -OpenTK 코딩 워크플로 개선 하 고 더욱 더 빨리 오류를 catch 하는 인라인 설명서 및 강력한 데이터 형식을 제공 합니다.
- **간단 하 게 통합할** -OpenTK.NET 응용 프로그램에 쉽게 통합 하도록 설계 되었습니다.
- **한 허가 라이선스** -OpenTK MIT/X11 라이선스에 따라 배포와 완전히 무료로 제공 합니다.
- **서식 있는, 형식이 안전한 바인딩** -OpenTK OpenGL OpenGL의 최신 버전을 지원 | ES, OpenAL 및 OpenCL를 자동 확장 로드 하 여 오류 검사 및 인라인 설명서입니다.
- **유연한 GUI 옵션** -게임 및 Xamarin.Mac 위해 특별히 설계 된 고성능 게임 창, OpenTK 네이티브를 제공 합니다.
- **완전히 관리 되는 CLS 규격 코드** -OpenTK 없는 관리 되지 않는 라이브러리와 macOS의 32 비트 및 64 비트 버전을 지원 합니다.
- **3D 수학 Toolkit** OpenTK 제공 `Vector`, `Matrix`, `Quaternion` 및 `Bezier` 의 3D 수학 도구 키트를 통해 구조체입니다.

OpenTK 게임, 과학 응용 프로그램 또는 다른 3D 그래픽을 필요로 하는 프로젝트, 오디오 또는 계산 기능에 사용할 수 있습니다.

자세한 내용은 참조 하십시오 [The Open Toolkit](http://www.opentk.com) 웹 사이트입니다.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK 빠른 시작

OpenTK Xamarin.Mac 응용 프로그램에서 사용 하 여 간략 한 소개를으로 하겠습니다 게임 보기를 열고, 사용자에 게 삼각형을 표시 하려면 Mac 응용 프로그램의 주 창에 해당 보기와 attachs 게임 보기의 간단한 삼각형을 렌더링 하는 간단한 응용 프로그램을 만듭니다.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>새 프로젝트를 시작합니다.

Mac 용 Visual Studio를 시작 하 고 새 Xamarin.Mac 솔루션을 만듭니다. 선택 **Mac** > **앱** > **일반** > **Cocoa 앱**:

[![](opentk-images/sample01.png "새 Cocoa 앱 추가")](opentk-images/sample01.png#lightbox)

입력 `MacOpenTK` 에 대 한는 **프로젝트 이름**:

[![](opentk-images/sample02.png "프로젝트 이름을 설정합니다.")](opentk-images/sample02.png#lightbox)

클릭는 **만들기** 단추를 새 프로젝트를 빌드합니다.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK를 포함 하 여

열린 TK Xamarin.Mac 응용 프로그램에서을 사용 하려면 먼저 OpenTK 어셈블리에 대 한 참조를 포함 해야 합니다. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭는 **참조** 폴더를 선택 **참조 편집...** .

확인란을 선택 하 여 `OpenTK` 클릭는 **확인** 단추:

[![](opentk-images/sample03.png "프로젝트 참조를 편집합니다.")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK를 사용 하 여

만들 새 프로젝트를 두 번 클릭는 `MainWindow.cs` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다. 확인 된 `MainWindow` 다음과 같은 클래스 보기:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacOpenTK
{
    public partial class MainWindow : NSWindow
    {
        #region Computed Properties
        public MonoMacGameView Game { get; set; }
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create new Game View and replace the window content with it
            Game = new MonoMacGameView(ContentView.Frame);
            ContentView = Game;

            // Wire-up any required Game events
            Game.Load += (sender, e) =>
            {
                // TODO: Initialize settings, load textures and sounds here
            };

            Game.Resize += (sender, e) =>
            {
                // Adjust the GL view to be the same size as the window
                GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
            };

            Game.UpdateFrame += (sender, e) =>
            {
                // TODO: Add any game logic or physics
            };

            Game.RenderFrame += (sender, e) =>
            {
                // Setup buffer
                GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
                GL.MatrixMode(MatrixMode.Projection);

                // Draw a simple triangle
                GL.LoadIdentity();
                GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
                GL.Begin(BeginMode.Triangles);
                GL.Color3(Color.MidnightBlue);
                GL.Vertex2(-1.0f, 1.0f);
                GL.Color3(Color.SpringGreen);
                GL.Vertex2(0.0f, -1.0f);
                GL.Color3(Color.Ivory);
                GL.Vertex2(1.0f, 1.0f);
                GL.End();

            };

            // Run the game at 60 updates per second
            Game.Run(60.0);
        }
        #endregion
    }
}
```

이 코드 아래에 자세히 살펴보겠습니다.

<a name="Required_APIs" />

### <a name="required-apis"></a>필수 Api

여러 개의 참조가 OpenTK Xamarin.Mac 클래스에서 사용 해야 합니다. 다음 정의의 시작 부분에 나와 `using` 문:

```csharp
using System;
using System.Drawing;
using OpenTK;
using OpenTK.Graphics;
using OpenTK.Graphics.OpenGL;
using OpenTK.Platform.MacOS;
using Foundation;
using CoreGraphics;
```
이 최소 집합 OpenTK를 사용 하 여 모든 클래스에 대 한 요구 됩니다.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>게임 뷰 추가

다음 게임 보기를 만들어 우리의 OpenTK와의 상호 작용을 모두 포함 하 고 결과 표시 해야 합니다. 다음 코드를 사용 했습니다.

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

여기 만들었고 게임 보기 우리의 주 Mac 창 크기가 같은 새 창의 콘텐츠 뷰를 대체 `MonoMacGameView`합니다. 기존 창 콘텐츠를 교체 하기 때문에 지정한 시각 자동으로 크기가 조정 됩니다 주 창 크기를 조정할 때.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>이벤트에 응답

각 게임 보기에 응답 해야 하는 몇 가지 기본 이벤트 있습니다. 이 섹션에서는 필요한 주요 이벤트를 설명 합니다.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>로드 이벤트

`Load` 이벤트가 디스크 이미지, 텍스처, 음악 등에서 리소스를 로드 하는 위치입니다. 이 간단 하 고 테스트 응용 프로그램에 대 한 사용 하지는 `Load` 이벤트, 참조에 대 한 포함 한 하지만:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>크기 조정 이벤트

`Resize` 게임 보기 크기를 조정할 때마다 이벤트를 호출 해야 합니다. 이 샘플 응용 프로그램에 대 한 진행 중인 GL 뷰포트 우리의 게임 보기 (은 Mac 주 창 크기를 조정 하는 자동)와 같은 크기를 다음 코드로:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame 이벤트

`UpdateFrame` 이벤트 개체 위치, 실행된 물리학 또는 AI 계산 업데이트, 사용자 입력을 처리 하는 데 사용 됩니다. 이 간단 하 고 테스트 응용 프로그램에 대 한 사용 하지는 `UpdateFrame` 이벤트, 참조에 대 한 포함 한 하지만: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK Xamarin.Mac 구현의 포함 되지 않습니다는 `Input API`이므로 마우스 및 키보드를 추가 하는 Api 지원 제공 했으며 Apple를 사용 해야 합니다. 필요에 따라의 사용자 지정 인스턴스를 만들 수 있습니다는 `MonoMacGameView` 재정의 `KeyDown` 및 `KeyUp` 메서드.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame 이벤트

`RenderFrame` 이벤트 (그리기) 렌더링 하는 데 사용 되는 코드가 포함 되어 해당 그래픽 합니다. 이 예제에서는 앱에 대 한 간단한 삼각형 인 게임 보기 채우는 했습니다. 

```csharp
Game.RenderFrame += (sender, e) =>
{
    // Setup buffer
    GL.Clear(ClearBufferMask.ColorBufferBit | ClearBufferMask.DepthBufferBit);
    GL.MatrixMode(MatrixMode.Projection);

    // Draw a simple triangle
    GL.LoadIdentity();
    GL.Ortho(-1.0, 1.0, -1.0, 1.0, 0.0, 4.0);
    GL.Begin(BeginMode.Triangles);
    GL.Color3(Color.MidnightBlue);
    GL.Vertex2(-1.0f, 1.0f);
    GL.Color3(Color.SpringGreen);
    GL.Vertex2(0.0f, -1.0f);
    GL.Color3(Color.Ivory);
    GL.Vertex2(1.0f, 1.0f);
    GL.End();

};
```

렌더링 코드를 호출 하 여 되 고의 기능은 일반적으로 `GL.Clear` 모든 기존 요소를 제거 하기 전에 새 요소를 그린 합니다.

> [!IMPORTANT]
> OpenTK Xamarin.Mac 버전용 **하지 않으면** 호출는 `SwapBuffers` 메서드 프로그램 `MonoMacGameView` 렌더링 코드의 끝에 있는 인스턴스에 합니다. 이렇게 하면 게임 보기에서는 렌더링 된 보기를 표시 하는 대신에 신속 하 게 strobe 합니다.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>게임 보기를 실행합니다.

모든 필요한 이벤트를 사용 하 여 정의할 및 게임 보기는 응용 프로그램의 Main Mac 창에 연결 된, 게임 보기를 실행 하 고 우리의 그래픽 표시 읽는 합니다. 다음 코드를 사용합니다.

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

전달 원하는 프레임 속도에서 우리가 원하는에 업데이트할 게임 보기, 예를 들어 선택한 `60` 당 프레임 수 (일반 TV로 동일한 새로 고침 빈도)의 두 번째입니다.

앱을 실행 하 고 출력 하겠습니다.

[![](opentk-images/intro01.png "응용 프로그램 출력의 예제")](opentk-images/intro01.png#lightbox)

이 창의 크기를 조정 했습니다 들이 있고 삼각형 크기가 조정 되며 역시 실시간 업데이트 게임 보기 수도 있습니다.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>다음 위치

OpenTk 수행 Xamarin.mac 응용 프로그램에서 작업의 기본 개념, 몇 가지 제안 사항에 다음을 사용해 다음과 같습니다.

- 삼각형의 색과에서 게임 보기의 배경색을 변경 하세요는 `Load` 및 `RenderFrame` 이벤트입니다.
- 사용자의 키를 누를 때 색을 변경 삼각형 확인는 `UpdateFrame` 및 `RenderFrame` 이벤트 또는 사용자 고유의 사용자 지정 확인 `MonoMacGameView` 클래스를 재정의 `KeyUp` 및 `KeyDown` 메서드.
- 인식 하는 키를 사용 하 여 화면으로 이동할 삼각형 확인는 `UpdateFrame` 이벤트입니다. 참고: 사용는 `Matrix4.CreateTranslation` 메서드 호출 및 변환 행렬을 만드는 `GL.LoadMatrix` 에서 로드 하는 메서드는 `RenderFrame` 이벤트입니다.
- 사용 하 여 한 `for` 루프에 여러 삼각형을 렌더링 하는 `RenderFrame` 이벤트입니다.
- 3D 공간에서 삼각형의 다른 보기에 카메라를 회전 합니다. 참고: 사용는 `Matrix4.CreateTranslation` 메서드 호출 및 변환 행렬을 만드는 `GL.LoadMatrix` 메서드를 로드 하는 것입니다. 사용할 수도 있습니다는 `Vector2`, `Vector3`, `Vector4` 및 `Matrix4` 카메라 조작 위한 클래스입니다.

더 많은 예제를 참조 하십시오는 [OpenTK 샘플 GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) 리 포 합니다. 공식 목록이 OpenTK를 사용 하는 예제를 포함 합니다. OpenTK Xamarin.Mac 버전에 사용 하기 위해 이러한 예제를 조정 해야 합니다.

OpenTK 구현 보다 복잡 한 Xamarin.Mac 예제를 참조 하십시오 우리의 [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) 샘플.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서 개요 OpenTK Xamarin.Mac 응용 프로그램에서 작업을 수행 했습니다. 게임 창을 만드는 방법을 살펴보았습니다 게임 창 Mac 창에 연결 하는 방법 및 게임 창에 간단한 도형을 렌더링 하는 방법입니다.

## <a name="related-links"></a>관련 링크

- [MacOpenTK (샘플)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (샘플)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [창 작업](~/mac/user-interface/window.md)
- [열기 도구 키트](http://www.opentk.com)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
