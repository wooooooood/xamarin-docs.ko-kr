---
title: Xamarin.ios의 OpenTK 소개
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 OpenTK를 사용 하는 방법을 소개 합니다. 게임 창을 만들고 유지 관리 하 고, 간단한 개체를 렌더링 하 고, 개체를 사용자에 게 표시 하는 과정을 다룹니다.
ms.prod: xamarin
ms.assetid: BDE05645-7273-49D3-809B-8642347678D2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 953b36eb48823cc23c5e7b3e831beca7b655a057
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645059"
---
# <a name="introduction-to-opentk-in-xamarinmac"></a>Xamarin.ios의 OpenTK 소개

OpenTK (개방형 도구 키트)는 OpenGL, OpenCL 및 OpenAL C# 작업을 용이 하 게 하는 고급 하위 수준 라이브러리입니다. OpenTK는 3D 그래픽, 오디오 또는 계산 기능을 필요로 하는 게임, 공학용 응용 프로그램 또는 기타 프로젝트에 사용할 수 있습니다. 이 문서에서는 Xamarin.ios 앱에서 OpenTK 사용에 대 한 간략 한 소개를 제공 합니다.

[![](opentk-images/intro01.png "예제 앱 실행")](opentk-images/intro01.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램의 OpenTK에 대 한 기본 사항을 다룹니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

[Xamarin.ios 내부](~/mac/internals/how-it-works.md) 문서의 [클래스/메서드를 대상으로 노출 C# -C](~/mac/internals/how-it-works.md) 섹션을 살펴볼 수 있습니다. C# 클래스를 연결 하는 데 사용 되는 `Register` 및 `Export` 명령을 설명 합니다. 목표-C 개체 및 UI 요소입니다.

<a name="About_OpenTK" />

## <a name="about-opentk"></a>OpenTK 정보

위에서 설명한 것 처럼 OpenTK (Open Toolkit)는 OpenGL, OpenCL 및 OpenAL 작업 C# 을 용이 하 게 하는 고급 하위 수준 라이브러리입니다. Xamarin.ios 앱에서 OpenTK를 사용 하면 다음과 같은 기능을 제공 합니다.

- **신속한 개발** -OpenTK은 코딩 워크플로를 개선 하 고 오류를 보다 쉽고 빠르게 파악 하기 위한 강력한 데이터 형식 및 인라인 설명서를 제공 합니다.
- **간편한 통합** -OpenTK은 .net 응용 프로그램과 쉽게 통합 되도록 설계 되었습니다.
- **관대 한 라이선스** -OPENTK는 MIT/X11 라이선스에 따라 배포 되며 완전히 무료입니다.
- **형식이 안전한 다양 한 바인딩** -OpenTK는 자동 확장 로드, 오류 검사 및 인라인 설명서를 포함 하는 최신 버전의 OpenGL, OPENGL | ES, openal 및 OpenCL를 지원 합니다.
- **유연한 GUI 옵션** -OpenTK는 게임 및 xamarin.ios 전용으로 특별히 설계 된 고성능 게임 창을 제공 합니다.
- **완전히 관리 되는 CLS 규격 코드** -OpenTK는 관리 되지 않는 라이브러리가 없는 32 비트 및 64 비트 버전의 macos를 지원 합니다.
- **3D 수학 도구 키트** OpenTK는 `Vector`3d 수학 `Quaternion` 도구 `Bezier` 키트를 통해, 및 구조체를 `Matrix`제공 합니다.

OpenTK는 3D 그래픽, 오디오 또는 계산 기능을 필요로 하는 게임, 공학용 응용 프로그램 또는 기타 프로젝트에 사용할 수 있습니다.

자세한 내용은 Toolkit 웹 사이트 [열기](http://www.opentk.com) 를 참조 하세요.

<a name="OpenTK_Quickstart" />

## <a name="opentk-quickstart"></a>OpenTK 빠른 시작

Xamarin.ios 앱에서 OpenTK를 사용 하는 방법에 대 한 간략 한 소개로, 게임 보기를 여는 간단한 응용 프로그램을 만들고, 해당 보기에서 간단한 삼각형을 렌더링 하 고, Mac 앱의 주 창에 게임 보기를 attachs 하 여 사용자에 게 삼각형을 표시할 수 있습니다.

<a name="Starting_a_New_Project" />

### <a name="starting-a-new-project"></a>새 프로젝트를 시작 하는 중

Mac용 Visual Studio를 시작 하 고 새 Xamarin.ios 솔루션을 만듭니다. **Mac**앱 일반cocoa > 앱을 선택 합니다. >  > 

[![](opentk-images/sample01.png "새 Cocoa 앱 추가")](opentk-images/sample01.png#lightbox)

`MacOpenTK` **프로젝트 이름**으로를 입력 합니다.

[![](opentk-images/sample02.png "프로젝트 이름 설정")](opentk-images/sample02.png#lightbox)

**만들기** 단추를 클릭 하 여 새 프로젝트를 빌드합니다.

<a name="Including_OpenTK" />

### <a name="including-opentk"></a>OpenTK 포함

Xamarin.ios 응용 프로그램에서 Open TK를 사용 하려면 먼저 OpenTK 어셈블리에 대 한 참조를 포함 해야 합니다. **솔루션 탐색기**에서 **참조** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **참조 편집**...을 선택 합니다.

확인 `OpenTK` 을 수행 하 고 **확인** 단추를 클릭 합니다.

[![](opentk-images/sample03.png "프로젝트 참조 편집")](opentk-images/sample03.png#lightbox)

<a name="Using_OpenTK" />

### <a name="using-opentk"></a>OpenTK 사용

새 프로젝트를 만든 상태에서 `MainWindow.cs` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집을 위해 엽니다. 클래스가 다음과 `MainWindow` 같이 표시 되도록 합니다.

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

아래에 자세히 설명 되어 있는 코드를 살펴보겠습니다.

<a name="Required_APIs" />

### <a name="required-apis"></a>필수 Api

Xamarin.ios 클래스에서 OpenTK를 사용 하려면 몇 가지 참조가 필요 합니다. 정의의 시작 부분에는 다음 `using` 문이 포함 되었습니다.

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
이 최소 집합은 OpenTK를 사용 하는 모든 클래스에 필요 합니다.

<a name="Adding_the_Game_View" />

### <a name="adding-the-game-view"></a>게임 보기 추가

다음으로 OpenTK와의 모든 상호 작용을 포함 하 고 결과를 표시 하는 게임 보기를 만들어야 합니다. 다음 코드를 사용 했습니다.

```csharp
public MonoMacGameView Game { get; set; }
...

// Create new Game View and replace the window content with it
Game = new MonoMacGameView(ContentView.Frame);
ContentView = Game;
```

여기서는 주 Mac 창과 동일한 크기를 게임 보기에 적용 하 고 창의 콘텐츠 뷰를 새 `MonoMacGameView`으로 바꿉니다. 기존 창 내용을 대체 했기 때문에 주 창의 크기를 조정할 때 제공 된 보기의 크기가 자동으로 조정 됩니다.

<a name="Responding_to_Events" />

### <a name="responding-to-events"></a>이벤트에 응답

각 게임 보기가 응답 해야 하는 몇 가지 기본 이벤트가 있습니다. 이 섹션에서는 필요한 주요 이벤트를 다룹니다.

<a name="The_Load_Event" />

### <a name="the-load-event"></a>Load 이벤트

이벤트 `Load` 는 이미지, 질감이 나 음악과 같은 디스크에서 리소스를 로드 하는 장소입니다. 간단한 테스트 앱의 경우에는 `Load` 이벤트를 사용 하지 않지만 참조용으로 포함 되어 있습니다.

```csharp
Game.Load += (sender, e) =>
{
    // TODO: Initialize settings, load textures and sounds here
};
```

<a name="The_Resize_Event" />

### <a name="the-resize-event"></a>크기 조정 이벤트

게임 `Resize` 보기의 크기를 조정할 때마다 이벤트를 호출 해야 합니다. 샘플 앱의 경우 다음 코드를 사용 하 여 게임 보기 (Mac 주 창에의 한 자동 크기 조정)와 동일한 크기로 GL 뷰포트를 만듭니다.

```csharp
Game.Resize += (sender, e) =>
{
    // Adjust the GL view to be the same size as the window
    GL.Viewport(0, 0, Game.Size.Width, Game.Size.Height);
};
```

<a name="The_UpdateFrame_Event" />

### <a name="the-updateframe-event"></a>UpdateFrame 이벤트

`UpdateFrame` 이벤트는 사용자 입력을 처리 하 고, 개체 위치를 업데이트 하 고, 물리 또는 AI 계산을 실행 하는 데 사용 됩니다. 간단한 테스트 앱의 경우에는 `UpdateFrame` 이벤트를 사용 하지 않지만 참조용으로 포함 되어 있습니다. 

```csharp
Game.UpdateFrame += (sender, e) =>
{
    // TODO: Add any game logic or physics
};
```

> [!IMPORTANT]
> OpenTK의 xamarin.ios 구현에는 `Input API`가 포함 되지 않으므로 키보드 및 마우스 지원을 추가 하려면 Apple에서 제공 하는 api를 사용 해야 합니다. 필요에 따라 `MonoMacGameView` 의 사용자 지정 인스턴스를 만들고 및 `KeyUp` 메서드를 `KeyDown` 재정의할 수 있습니다.

<a name="The_RenderFrame_Event" />

### <a name="the-renderframe-event"></a>RenderFrame 이벤트

이벤트 `RenderFrame` 에는 그래픽을 렌더링 (그리기) 하는 데 사용 되는 코드가 포함 되어 있습니다. 예제 앱의 경우 간단한 삼각형으로 게임 보기를 채웁니다. 

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

일반적으로 렌더링 코드는를 `GL.Clear` 호출 하 여 새 요소를 그리기 전에 기존 요소를 제거 합니다.

> [!IMPORTANT]
> OpenTK의 xamarin.ios 버전의 경우 렌더링 코드의 끝에서 `MonoMacGameView` 인스턴스의 `SwapBuffers` 메서드를 호출 **하지 마십시오** . 이렇게 하면 렌더링 된 뷰를 표시 하는 대신 게임 보기가 섬광에 빠르게 표시 됩니다.

<a name="Running_the_Game_View" />

### <a name="running-the-game-view"></a>게임 보기 실행

모든 필수 이벤트를 정의 하 고 게임 보기를 앱의 기본 Mac 창에 연결 하면 게임 보기를 실행 하 고 그래픽을 표시 하기 위해 읽혀집니다. 다음 코드를 사용합니다.

```csharp
// Run the game at 60 updates per second
Game.Run(60.0);
```

게임 보기를 업데이트 하는 데 필요한 프레임 속도를 전달 합니다. 예를 들어, 초당 프레임을 선택 `60` 했습니다 (일반 TV와 동일한 새로 고침 속도).

앱을 실행 하 고 출력을 확인 하겠습니다.

[![](opentk-images/intro01.png "앱 출력 샘플")](opentk-images/intro01.png#lightbox)

창의 크기를 조정 하는 경우 게임 보기만 존재 하며 삼각형의 크기를 조정 하 고 실시간으로 업데이트 합니다.

<a name="Where_to_Next" />

### <a name="where-to-next"></a>다음 위치

Xamarin.ios 응용 프로그램에서 OpenTk 작업을 수행 하는 기본 사항을 사용 하 여 다음에 사용해 볼 수 있는 몇 가지 제안 사항을 확인할 수 있습니다.

- `Load` 및`RenderFrame` 이벤트에서 게임 보기의 삼각형 색과 배경색을 변경해 보세요.
- `UpdateFrame` 사용자가 `KeyDown` 및 `RenderFrame` 이벤트에서 키를 누르거나 고유한 사용자 지정 `MonoMacGameView` 클래스를 만들고 및 메서드를 `KeyUp` 재정의 하는 경우 삼각형의 색을 변경 합니다.
- `UpdateFrame` 이벤트에서 인식 되는 키를 사용 하 여 삼각형이 화면에서 이동 하도록 합니다. 힌트: `Matrix4.CreateTranslation` 메서드를 사용 하 여 변환 행렬을 만들고 `GL.LoadMatrix` 메서드를 호출 하 여 `RenderFrame` 이벤트에서 로드 합니다.
- 루프를 `for` 사용 하 여 `RenderFrame` 이벤트에서 여러 삼각형을 렌더링 합니다.
- 카메라를 회전 하 여 3D 공간에서 삼각형의 다른 보기를 제공 합니다. 힌트: `Matrix4.CreateTranslation` 메서드를 사용 하 여 변환 행렬을 만들고 `GL.LoadMatrix` 메서드를 호출 하 여 로드 합니다. 카메라 `Vector2`조작을 위해 `Vector3` ,및`Matrix4`클래스 를 사용할 수도 있습니다. `Vector4`

더 많은 예제는 [OpenTK 샘플 GitHub](https://github.com/opentk/opentk/tree/master/Source/Examples) 리포지토리를 참조 하세요. OpenTK 사용의 공식 예제 목록이 포함 되어 있습니다. OpenTK의 Xamarin.ios 버전과 함께를 사용 하기 위해 이러한 예제를 조정 해야 합니다.

OpenTK 구현에 대 한 보다 복잡 한 Xamarin.ios 예제는 [MonoMacGameView](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow) 샘플을 참조 하세요.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 OpenTK를 사용 하는 방법을 간략히 살펴보겠습니다. 게임 창을 만드는 방법, 게임 창을 Mac 창에 연결 하는 방법 및 게임 창에서 간단한 셰이프를 렌더링 하는 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacOpenTK (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macopentk)
- [MonoMacGameView (샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/monomacgamewindow)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Windows 작업](~/mac/user-interface/window.md)
- [Open Toolkit](http://www.opentk.com)
- [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows 소개](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
