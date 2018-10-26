---
title: Xamarin.mac에서 OpenTK 소개
description: 이 문서에서는 Xamarin.Mac 응용 프로그램에서 OpenTK를 사용 하 여 작업을 소개 합니다. 만들고 유지 관리는 게임 창, 간단한 개체를 렌더링 합니다. 사용자에 게 개체를 표시를 설명 합니다.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 835b8cd0f2e689c4d7d4cace1d846543863b7393
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107718"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Xamarin.mac에서 OpenTK 소개

OpenTK (The Open Toolkit) 고급, 하위 수준 C# 라이브러리를 보다 쉽게 OpenAL, OpenGL 및 OpenCL와 작업 하는 경우 OpenTK 게임, 과학 응용 프로그램 또는 다른 3D 그래픽을 필요로 하는 프로젝트, 오디오 또는 계산 기능을 사용할 수 있습니다. 이 문서에서는 Xamarin.Mac 앱에서 OpenTK 사용에 대해 간략하게 소개 합니다.

[![](opentk-images/intro01.png "앱의 예 실행")](opentk-images/intro01.png#lightbox)

이 문서에서는 Xamarin.Mac 응용 프로그램에서 OpenTK의 기본 사항을 설명 합니다. 것이 가장 좋습니다를 통해 작업 하는 합니다 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서 합니다 [Xcode 및 Interface Builder 소개](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 하 고 [출 선 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션으로 주요 개념 및이 문서를 사용 하는 기술을 설명 합니다.

확인 하려는 경우는 [노출 C# 클래스 / objective-c 메서드](~/mac/internals/how-it-works.md) 섹션을 [Xamarin.Mac 내부 요소](~/mac/internals/how-it-works.md) 설명도 문서를 `Register` 및 `Export` 명령 하 여 통신-C# 클래스 Objective-c 개체 및 UI 요소에 사용 합니다.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK에 대 한

위에서 설명한 대로 OpenTK (The Open Toolkit)는 고급, 하위 수준 C# 라이브러리 OpenAL, OpenGL 및 OpenCL와 작업을 더 쉽게 하는. OpenTK Xamarin.Mac 앱에서 사용 하 여 다음과 같은 기능을 제공 합니다.

- **신속한 개발** -OpenTK 코딩 워크플로 개선 하 고 쉽고 빠르게 오류를 catch 하는 강력한 데이터 형식 및 인라인 설명서를 제공 합니다.
- **간편한 통합** -OpenTK.NET 응용 프로그램을 쉽게 통합 하도록 설계 되었습니다.
- **라이선스** -OpenTK MIT/X11 라이선스에서 배포 되 고 완전히 무료로 제공 됩니다.
- **풍부한, 형식 안전 바인딩을** -OpenTK OpenGL, OpenGL의 최신 버전 지원 | ES, OpenAL 및 OpenCL 자동 확장 로드를 사용 하 여 오류 검사 및 인라인 설명서.
- **유연한 GUI 옵션** -게임 및 Xamarin.Mac 위해 특별히 설계 된 고성능 게임 창, OpenTK 네이티브를 제공 합니다.
- **완전히 관리 되 고 CLS 규격 코드** -OpenTK 없습니다 관리 되지 않는 라이브러리를 사용 하 여 macOS의 32 비트 및 64 비트 버전을 지원 합니다.
- **3D 수학 도구 키트** OpenTK 제공 `Vector`, `Matrix`, `Quaternion` 고 `Bezier` 해당 3D 수학 도구 키트를 통해 구조체입니다.

OpenTK 게임, 과학 응용 프로그램 또는 다른 3D 그래픽을 필요로 하는 프로젝트, 오디오 또는 계산 기능을 사용할 수 있습니다.

자세한 내용은 참조 하십시오 [열려이 도구 키트](http://www.opentk.com) 웹 사이트입니다.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK 빠른 시작

OpenTK Xamarin.Mac 앱에서 사용 하 여 빠른 소개의 경우와 게임 뷰를 엽니다. Mac 앱의 주 창에 사용자에 게 삼각형을 표시 하는 뷰와 attachs 게임 보기에서 간단한 삼각형을 렌더링 하는 간단한 응용 프로그램을 만들 하려고 합니다.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>새 프로젝트를 시작합니다.

Mac 용 Visual Studio를 시작 하 고 새 Xamarin.Mac 솔루션을 만듭니다. 선택 **Mac** > **앱** > **일반** > **Cocoa 앱**:

[![](opentk-images/sample01.png "새 Cocoa 앱 추가")](opentk-images/sample01.png#lightbox)

입력 `MacOpenTK` 에 대 한 합니다 **프로젝트 이름**:

[![](opentk-images/sample02.png "프로젝트 이름을 설정합니다.")](opentk-images/sample02.png#lightbox)

클릭 합니다 **만들기** 단추를 새 프로젝트를 빌드합니다.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK 포함

Xamarin.Mac 응용 프로그램에서 열려 TK를 사용할 수 있습니다, 전에 OpenTK 어셈블리에 대 한 참조를 포함 해야 합니다. 에 **솔루션 탐색기**를 마우스 오른쪽 단추로 클릭 합니다 **참조** 폴더를 선택 **참조 편집...** .

확인란을 선택 하 여 `OpenTK` 을 클릭 합니다 **확인** 단추:

[![](opentk-images/sample03.png "프로젝트 참조를 편집합니다.")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK를 사용 하 여

새 프로젝트를 만든 후 두 번 클릭 합니다 `MainWindow.cs` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다. 확인 된 `MainWindow` 다음과 같은 클래스 모양:

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

여러 참조 OpenTK Xamarin.Mac 클래스에서 사용 해야 합니다. 정의의 시작 부분에 다음 포함 된 `using` 문:

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
이 최소한 OpenTK를 사용 하 여 모든 클래스에 대 한 요구 됩니다.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>게임 뷰 추가

다음 보기를 만들어 게임 합니다 모든 OpenTK 사용 하 여와 상호 작용을 포함 하 고 결과 표시 해야 합니다. 다음 코드를 사용 했습니다.

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

여기 만들었습니다 게임 뷰는 기본 Mac 창 크기가 같은 새 창의 콘텐츠 뷰를 대체 하 고 `MonoMacGameView`입니다. 기존 창 콘텐츠를 교체 하기 때문에 주 Windows 크기를 조정할 때 자동으로 지정한 뷰에 조정 됩니다.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>이벤트에 응답

각 게임 보기에 응답 해야 하는 몇 가지 기본 이벤트 있습니다. 이 섹션에서는 필요한 주요 이벤트를 설명 합니다.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Load 이벤트

`Load` 이벤트는 디스크 이미지, 질감, 음악 등에서 리소스를 로드 하는 위치입니다. 이 간단한 테스트 앱을 사용 하지는 `Load` 이벤트 참조에 포함할 수 있지만:

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>크기 조정 이벤트

`Resize` 게임 뷰 크기를 조정할 때마다 이벤트를 호출 해야 합니다. 이 샘플 앱에 대 한 진행 중인 GL 뷰포트 동일한 크기 (자동 Mac 주 창에서 크기를 조정할 수)는 게임 뷰에 다음 코드를 사용 하 여:

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame 이벤트

`UpdateFrame` 이벤트는 사용자 입력을 처리 하 여 개체 위치, 실행된 물리학 또는 AI 계산 업데이트를 사용 합니다. 이 간단한 테스트 앱을 사용 하지는 `UpdateFrame` 이벤트 참조에 포함할 수 있지만: 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> Xamarin.Mac 구현의 OpenTK 포함 되지 않습니다는 `Input API`이므로 사용할 Apple 마우스 및 키보드를 추가 하는 Api를 지원 해야 합니다. 사용자 지정 인스턴스를 만들 수는 필요에 따라 합니다 `MonoMacGameView` 재정의 `KeyDown` 및 `KeyUp` 메서드.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame 이벤트

`RenderFrame` 이벤트 (그리기)를 렌더링 하는 데 사용 되는 코드가 포함 되어 그래픽입니다. 이 예제에서는 앱에 대 한 간단한 삼각형을 사용 하 여 게임 뷰를 채우는 것: 

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

일반적으로 렌더링 코드를 호출 하 여 되 고 `GL.Clear` 기존 요소를 제거 하기 전에 새 요소를 그립니다.

> [!IMPORTANT]
> OpenTK Xamarin.Mac 버전용 **하지 않습니다** 호출을 `SwapBuffers` 메서드의 `MonoMacGameView` 렌더링 코드의 끝에 있는 인스턴스에 합니다. 이렇게 하면 게임 뷰의 strobe 렌더링 된 보기를 표시 하는 대신에 신속 하 게 합니다.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>게임 보기를 실행합니다.

모든 필수 이벤트를 사용 하 여 정의 및 게임 보기 앱의 주 Mac 창에 연결 된 내용은에서는 게임 보기를 실행 하는 그래픽을 표시 합니다. 다음 코드를 사용합니다.

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

전달 원하는 프레임 속도 업데이트 하려면 게임 보기, 예를 들어 선택한 `60` 프레임 / 초 (일반 TV와 같은 새로 고침 빈도).

앱을 실행 하 고 출력이 표시 해 보겠습니다.

[![](opentk-images/intro01.png "앱 출력의 샘플")](opentk-images/intro01.png#lightbox)

이 창의 크기를 조정 했습니다 있고 삼각형 크기가 조정 되며 실시간 업데이트 게임 보기 수도 있습니다.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>다음 위치?

OpenTk 수행 Xamarin.mac 응용 프로그램에서 사용 하는 기본 기능을 사용 하 여 다음을 사용해의 몇 가지 제안 사항 다음과 같습니다.

- 삼각형의 색 및에서 게임 뷰의 배경 색상을 변경 합니다 `Load` 및 `RenderFrame` 이벤트입니다.
- 사용자는 키를 누를 때 색을 변경 하는 삼각형을 확인 합니다 `UpdateFrame` 및 `RenderFrame` 이벤트 또는 사용자 고유의 사용자 지정 `MonoMacGameView` 클래스를 재정의 합니다 `KeyUp` 및 `KeyDown` 메서드.
- 인식 키를 사용 하 여 화면 간에 이동 삼각형을 확인 합니다 `UpdateFrame` 이벤트입니다. 힌트: 사용 하 여는 `Matrix4.CreateTranslation` 메서드 호출을 변환 행렬을 만들려면를 `GL.LoadMatrix` 에 로드 하는 방법은 `RenderFrame` 이벤트.
- 사용 하 여는 `for` 루프에서 여러 개의 삼각형을 렌더링 하는 `RenderFrame` 이벤트입니다.
- 3D 공간에서 삼각형의 다른 보기를 제공 하려면 카메라를 회전 합니다. 힌트: 사용 합니다 `Matrix4.CreateTranslation` 메서드 호출을 변환 행렬을 만들려면는 `GL.LoadMatrix` 로드 하는 방법. 사용할 수도 있습니다는 `Vector2`, `Vector3`를 `Vector4` 및 `Matrix4` 카메라 조작 위한 클래스입니다.

더 많은 예제를 참조 하십시오 합니다 [OpenTK 샘플 GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) 리포지토리. 공식 목록이 OpenTK를 사용 하는 예제를 포함 합니다. OpenTK Xamarin.Mac 버전 사용에 대 한 이러한 예제를 조정 해야 합니다.

OpenTK 구현의 복잡 Xamarin.Mac 예제를 참조 하세요. 당사의 [MonoMacGameView](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/) 샘플입니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.Mac 응용 프로그램에서 OpenTK 작업 둘러보기를 걸렸습니다. 게임 창을 만드는 방법에 살펴보았습니다 게임 창 Mac 창에 연결 하는 방법 및 게임 창에서 간단한 도형을 렌더링 하는 방법입니다.

## <a name="related-links"></a>관련 링크

- [MacOpenTK (샘플)](https://developer.xamarin.com/samples/mac/MacOpenTK/)
- [MonoMacGameView (샘플)](https://developer.xamarin.com/samples/mac/MonoMacGameWindow/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [열린 도구 키트](http://www.opentk.com)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
