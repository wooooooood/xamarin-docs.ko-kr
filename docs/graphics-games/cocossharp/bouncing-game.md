---
title: BouncingGame 세부 정보
description: 이 연습에서는 CocosSharp를 사용 하 여 간단한 바운스되 볼 게임을 구현 하는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: d12d6fb8ecfcba5e5093b2af4790a51ef8cf8e47
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/09/2018
---
# <a name="bouncinggame-details"></a>BouncingGame 세부 정보

> [!TIP]
> BouncingGame에 대 한 코드를 찾을 수는 [Xamarin 샘플](https://developer.xamarin.com/samples/mobile/BouncingGame/)합니다. 이 가이드를 따라가려면 가장 다운로드 하 여이 가이드를 참조 하는 대로 코드를 탐색 합니다.

이 연습에서는 CocosSharp를 사용 하 여 간단한 바운스되 볼 게임을 구현 하는 방법을 보여 줍니다. 

 - 압축을 푸는 게임 콘텐츠
 - 일반적인 CocosSharp 시각적 요소
 - 시각적 요소를 추가합니다. `GameLayer`
 - 모든 프레임 논리 구현

완성 된 게임은 다음과 같이 표시 됩니다.

![완료 BouncingGame](bouncing-game-images/image1.png "완료 BouncingGame")

## <a name="unzipping-game-content"></a>압축을 푸는 게임 콘텐츠

이라는 용어를 자주 사용 하는 게임 개발자 *콘텐츠* 일반적으로 시각적 아티스트, 게임 디자이너 또는 오디오 디자이너에 의해 생성 되는 비 코드 파일을 참조 합니다. 일반적인 콘텐츠 유형의 시각적 개체를 표시 하거나, 소리를 재생 하거나 인공 인텔리전스 (AI) 동작을 제어 하는 데 사용 하는 파일을 포함 합니다. 게임 개발 팀의 관점에서 콘텐츠는 프로그래머가 아닌 일반적으로 만들어집니다. 게임 두 가지 유형의 콘텐츠를 포함 합니다.

 - png 파일 볼 및 패 표시 되는 방식을 정의할 수 있습니다.
 - (아래의 점수 디스플레이 추가할 때 더 자세히 설명) 점수 표시에서 사용 되는 글꼴을 정의 하는 단일 xnb 파일

여기에이 콘텐츠를 찾을 수 [zip 콘텐츠](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)합니다. 이러한 파일을 다운로드 하 고이 연습의 뒷부분에서 액세스 하는 위치에 압축을 푼 필요 합니다.

## <a name="common-cocossharp-visual-elements"></a>일반적인 CocosSharp 시각적 요소

CocosSharp 다양 한 시각적 개체를 표시 하는 데 사용 되는 클래스를 제공 합니다. 일부 요소는 다른 조직에 대해 사용 되지만 직접 표시입니다. 게임에 다음을 사용 합니다.

 - `CCNode` – CocosSharp 모든 시각적 개체에 대 한 기본 클래스입니다. `CCNode` 클래스를 포함 한 `AddChild` 부모/자식 계층을 구성할 사용할 수 있는 함수 라고도 하는 *시각적 트리* 또는 *장면 그래프*합니다. 아래에 언급 된 모든 클래스에서 상속 `CCNode`합니다.
 - `CCScene` – 모든 CocosSharp 게임에 대 한 시각적 트리의 루트입니다. 모든 시각적 요소를 시각적 트리의 일부 여야 합니다. 한 `CCScene` 루트, 또는 표시 되지 않습니다.
 - `CCLayer` – Visual에 대 한 컨테이너와 같은 개체 `CCSprite`합니다. 이름에서 알 수 있듯이 `CCLayer` 클래스는 서로 기반으로 하는 방법을 시각적 요소 계층을 제어 하는 데 사용 합니다.
 - `CCSprite` – 이미지 또는 이미지의 부분을 표시 합니다. `CCSprite` 인스턴스 크기를 조정 배치 될 수 있습니다 및 다양 한 시각 효과 제공 합니다.
 - `CCLabel` – 화면에는 문자열을 표시합니다. 사용 되는 글꼴 `CCLabel` xnb 파일에 정의 되어 있습니다. 아래에서 자세히 xnbs에 설명 합니다.

다양 한 종류를 사용 하는 방식을 이해 하는 단일 것으로 간주 하겠습니다 `CCSprite`합니다. 시각적 요소에 추가 되어야 합니다는 `CCLayer`, 시각적 트리에 있어야 하 고는 `CCScene` 루트에 있습니다. 따라서 단일에 대 한 부모/자식 관계 `CCSprite` 것 `CCScene`  >  `CCLayer`  >  `CCSprite`합니다.

## <a name="adding-visual-elements-to-gamelayer"></a>GameLayer에 시각적 요소 추가

### <a name="adding-the-paddle-sprite"></a>패 sprite 추가

차단 하 고 단일 추가 게임의 배경이 바로잡을 처음 `CCSprite` 화면에 렌더링 합니다. 수정 된 `GameLayer` 추가할 클래스는 `CCSprite` 다음과 같습니다:

```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of the drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

위의 코드에서는 단일 `CCSprite` 의 자식으로 추가 하 고는 `GameLayer`합니다. `CCSprite` 생성자를 사용 하면 문자열으로 사용할 이미지 파일을 정의할 수 있습니다. 코드 CocosSharp 라는 파일을 찾도록 지시 `paddle` (확장명을 생략 하면이 가이드의 뒷부분에 나오는 설명할 것입니다). CocosSharp는 모든 파일 이름을 찾습니다 `paddle` 루트 콘텐츠 폴더에서 (변수인 **콘텐츠**)에 추가 된 모든 폴더 뿐 아니라는 `gameView.ContentManager.SearchPaths` (이전 섹션에서 설명 함).

파일의 루트에 직접 추가 **콘텐츠** iOS 및 Android에 대 한 폴더입니다. 이렇게 하려면 마우스 오른쪽 단추로 클릭 하거나 컨트롤에서 클릭에는 **콘텐츠** 고 iOS 프로젝트에서 폴더 **추가** > **파일 추가 중...** म 푼 콘텐츠 앞으로 이동 하 고 선택 **paddle.png**합니다. 폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 선택 해야는 **복사** 옵션:

![폴더 대화 상자에 파일 추가](bouncing-game-images/image2.png "폴더 대화 상자에는 추가 파일")

다음으로, 파일 Android 프로젝트에 추가 합니다. 단추로 클릭 하거나 컨트롤을 클릭 하 여 콘텐츠 폴더에 (에 **자산** Android 프로젝트의 폴더) 선택을 선택 하 고 **추가** > **파일 추가 중...** . 이 이번에 iOS 프로젝트의 이동 **콘텐츠** 폴더입니다. 파일을 추가 하는 방법에 대 한 요청을 받는 경우 선택 된 **링크 추가** 옵션:

![폴더 대화 상자에 파일 추가](bouncing-game-images/addalink.png "폴더 대화 상자에 파일 추가")

아래 두 프로젝트에 추가할 파일 시도가 이유에 대해 설명 합니다. 각 프로젝트의 **콘텐츠** 이제 폴더에 포함 된 **paddle.png** 파일:

![콘텐츠 폴더의 내용을](bouncing-game-images/image3.png "콘텐츠 폴더의 내용을")

보면 게임을 실행 하는 경우는 `CCSprite` 그려집니다.

![화면에 그릴 패 들](bouncing-game-images/image4.png "화면에 그릴 패 들")

### <a name="file-details"></a>파일 세부 정보

단일 파일을 프로젝트에 추가 했습니다 지금까지 되었으며 프로세스 매우 간단 합니다. 단순히 추가 **paddle.png** 파일을 여 **콘텐츠** 폴더와 코드에서 참조 합니다. 파일로 CocosSharp에서 작업할 때 몇 가지 고려 사항 살펴볼 간단히 살펴보겠습니다.

#### <a name="capitalization"></a>대문자 표시

파일 이름 및 코드에서 해당 파일에 액세스를 사용 하는 문자열은 소문자 모두 있는지 확인 합니다. 일부 플랫폼 (예: Windows 데스크톱 및 iOS 시뮬레이터) 대/소문자, 다른 플랫폼 (예: Android 및 iOS 장치)은 대/소문자 구분 하는 동안 때문입니다. 이 자습서의 나머지 부분에 대 한 모든 소문자 파일 플랫폼으로 유지 하는 파일 및 코드를 사용 합니다 최대한.

#### <a name="extensions"></a>확장

패 파일 참조 시 스프라이트의 만들기에 대 한 생성자는 ".png" 확장을 포함 되지 않습니다. 플랫폼 간에 다를 수 파일 확장명과 동일한 자산 유형으로 CocosSharp 프로젝트에서 작업 하는 경우에 일반적으로 파일의 확장명을 무시 됩니다. 예를 들어, 오디오 파일의 플랫폼에 따라.aiff,.mp3, 또는.wma 파일 형식의 수 있습니다. 확장 삭제 동일한 코드를를 파일 확장명에 관계 없이 모든 플랫폼에서 사용할 수 있습니다.

#### <a name="content-in-platform-specific-projects"></a>플랫폼별 프로젝트에 콘텐츠

대부분의 코드 파일에 있을 수 있습니다는 달리는 PCL, 콘텐츠 파일을 각 플랫폼별 프로젝트에 추가 해야 합니다. CocosSharp 두 가지 이유로이 구성이 필요 합니다.

1. 각 플랫폼에 다른 **빌드 작업**합니다. IOS 프로젝트에 추가 된 내용은 사용할지는 **BundleResource** 빌드 작업입니다. Android 프로젝트에 추가 된 내용은 사용할지는 **AndroidAsset** 빌드 작업입니다.
2. 일부 플랫폼에는 플랫폼 특정 파일 형식 필요합니다. 예를 들어 글꼴.xnb 파일을 볼 수 있겠지만,이 연습의 뒷부분에서 iOS 및 Android에서 간에 서로 다릅니다.

파일 형식 (예:.png) 플랫폼 간에 다르지 않습니다, 각 플랫폼 하나 이상의 플랫폼 동일한 위치에서 파일을 연결할 수 있는, 동일한 파일을 사용할 수 있습니다.

## <a name="orientation"></a>방향

다른 모든 앱과 마찬가지로 CocosSharp 앱 세로 또는 가로 방향에서 실행할 수 있습니다. 세로 전용 모드에서 실행 되도록 게임을 조정 합니다. 첫째, 세로 가로 세로 비율을 처리 하는 게임의 확인 코드를 변경 합니다. 이 작업을 수행 하려면 수정 하는 `width` 및 `height` 값에 `LoadGame` 에서 메서드는 `ViewController` iOS에서 클래스 및 `MainActivity` Android에서 클래스:

```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    // ...
```

그런 다음 세로 모드에서 실행 되도록 각 플랫폼별 프로젝트를 수정 해야 합니다.

### <a name="ios-orientation"></a>iOS 방향

수정 하려면 iOS 프로젝트의 방향을 선택 합니다는 **Info.plist** 파일에 **BouncingGame.iOS** 프로젝트를 마우스 변경 **iPhone/iPod 배포 정보** 및는 **iPad 배포 정보** 세로 방향을 포함 하도록 합니다.

![방향 옵션](bouncing-game-images/image5.png "방향 옵션")

### <a name="android-orientation"></a>Android 방향

Android 프로젝트의 방향을 수정 하려면 BouncingGame.Android 프로젝트에서 MainActivity.cs 파일을 엽니다. 작업 특성 정의 수정 하 여 세로 방향 에서만 다음과 같이 지정 합니다.

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>기본 좌표계

인스턴스화하는 코드는 `CCSprite`, 설정 하는 `PositionX` 및 `PositionY` 100 사이의 값입니다. 기본적으로이 의미 하는 `CCSprite` 센터 고 화면의 왼쪽 아래에서에서 오른쪽으로 100 픽셀에 배치 됩니다. 연결 하 여 좌표계를 수정할 수는 `CCCamera` 에 `CCLayer`합니다. 와 노력지 않습니다 `CCCamera` 에서이 프로젝트에 대 한 자세한 내용은 `CCCamera` 에서 찾을 수는 [CocosSharp API 문서](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/)합니다.

하드웨어에서 픽셀 달리 "게임" 픽셀 위에서 언급 한 100 픽셀입니다. 이 다른 해결 (iPhone 및 iPad) 등의 장치에서 같은 게임을 실행 개체에 배치 되 고 실제 화면을 기준으로 올바르게 크기의 발생 않을 것임을 의미 합니다. 특히, 게임의 표시 영역은 항상 1024 픽셀 높이 및 너비가 768 픽셀, 이것이 해상도 지정 했습니다에서 이전에 `LoadGame` 메서드.

## <a name="adding-the-ball-sprite"></a>볼 sprite 추가

작업의 기본 사항이 된 했으므로 `CCSprite`, 두 번째 추가 `CCSprite` – 공 합니다. म 합니다 될 다음 단계는 매우 유사한 만들었습니다 패 들 `CCSprite`합니다. 

첫째, 추가 **ball.png** iOS 프로젝트의 압축 푼된 폴더에서 이미지 **콘텐츠** 폴더입니다. 선택 **복사** 파일을 여 **콘텐츠** 디렉터리입니다. 에 대 한 링크를 추가 하려면 위와 동일한 단계에 따라는 **ball.png** Android 프로젝트에서 파일입니다.

다음을 만듭니다는 `CCSprite` 구성원을 추가 하 여이 ball에 대 한 호출 `ballSprite` 에 `GameScene` 으로 클래스에 대 한 인스턴스화 코드는 `ballSprite`합니다. 완료 되 면는 `GameLayer` 클래스는 다음과 같습니다.

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```

## <a name="adding-the-score-label"></a>점수 레이블 추가

게임에 추가 하 고 마지막 시각적 요소는 `CCLabel` 횟수를 표시 하려면 사용자는 볼 반송 성공적으로 했습니다. `CCLabel` 생성자에 지정 된 글꼴을 사용 하 여 화면에 문자열을 표시 합니다.

다음 코드를 추가 `CCLabel` 인스턴스 `GameLayer`합니다. 일단 완성 되는 코드는 다음과 같이 표시 됩니다.

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    // ...
```

scoreLabel를 사용 하는 프로그램 `AnchorPoint` 의 `CCPoint.AnchorUpperLeft`, 즉는 `PositionX` 및 `PositionY` 값의 왼쪽 위 위치를 정의 합니다. 통해이 위치는 `scoreLabel` 레이블의 크기를 고려해 야 할 필요 없이 화면의 왼쪽 위를 기준으로 합니다.

## <a name="implementing-every-frame-logic"></a>모든 프레임 논리 구현

지금까지 게임 정적 장면을 표시합니다. 자주 발생 하는 개체의 위치를 업데이트 하는 코드를 추가 하 여 장면에서 개체의 움직임을 제어 하는 논리를 추가할 예정입니다. 이 경우에 코드 60 시간 / 초-라고도 60까지 실행 됩니다 *프레임* 초당 (하드웨어가 자주 업데이트를 처리할 수 없습니다) 없는 경우. 특히, 추가할 예정이 니 대체 하 고, 입력에 따라 패 이동 하 고 볼 패 들을 이룹니다 때마다 플레이어의 점수를 업데이트 하려면 패 들에 대해 bounce 볼 하는 논리입니다.

`Schedule` 에서 제공 하는 메서드는 `CCNode` 클래스를 통해 게임에 모든 프레임 논리를 추가할 수 있습니다. 뒤에 코드 추가 `// New code` GameLayer 생성자에:

```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

이제 만들는 `RunGameLogic` 에서 메서드는 `GameLayer` 모든 모든 프레임 논리를 저장할 클래스:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

초에서 시간 프레임은 float 매개 변수를 정의 합니다. 볼의 움직임을 구현 하는 경우이 값을 사용해 보겠습니다.

### <a name="making-the-ball-fall"></a>대체 볼 만들기

수동으로 구현 하거나 중력 코드 또는 CocosSharp에 기본 제공 Box2D 기능을 사용 하 여 대체 볼을 하도록 수 있습니다. 물리 시뮬레이션 Box2D 엔진 CocosSharp 게임 ´ ù. 매우 강력 하 고 효율적 이지만 설치 코드를 작성 해야 합니다. 물리 시뮬레이션은 매우 간단 하므로 여기는 구현 될 것 수동으로 합니다.

무게를 구현 하려면 저장소 현재 X 및 Y 속도 클로즈업 해야 합니다. 두 명의 멤버를 추가 하겠습니다는 `GameLayer` 클래스:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

다음에 하강 논리를 구현할 수 있습니다 `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>터치식 입력 패 이동

공 떨어지지만 했으므로 추가 하겠습니다 수평 이동 패 들을 사용 하 여는 `CCEventListenerTouchAllAtOnce` 개체입니다. 이 개체는 다양 한 입력 관련 이벤트를 제공 합니다. 이 경우 패 들의 위치가 사용자가 조정할 수 있도록 모든 터치 포인트를 이동 하는 경우 알림을 받을 하려고 합니다. `GameLayer` 이미 인스턴스화하는 `CCEventListenerTouchAllAtOnce`하므로 단순히 할당할 필요가는 `OnTouchesMoved` 위임 합니다. 이렇게 하려면 AddedToScene 메서드를 다음과 같이 수정 합니다.

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

다음으로 구현 `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>볼 충돌 구현

게임을 실행 하는 경우 이제 합니다 알 수는 패 실패로 볼 합니다. 구현 *충돌* (반응 하도록 논리를 위의 게임 개체) 모든 프레임 코드에서입니다. 모든 프레임을 배치 하는 변경 개체 이동, 충돌 확인 되므로 일반적으로 모든 프레임을 수행 합니다. 또한 추가할 예정이 니 속도 X 축에 볼 적중 하는 게임에 몇 가지 과제를 추가할 패 하지만이 의미는 클로즈업 화면의 가장자리를 지나서 이동 하지 못하도록 하려면 필요 합니다. 수행 됩니다는 `RunGameLogic` 개발 속도를 적용 한 후 코드는 `ballSprite` 후 `// New Code`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```

## <a name="adding-scoring"></a>점수 매기기 추가

이제 게임을 재생할 수, 했으므로 점수 매기기 논리를 추가 하는 마지막 단계가입니다. 첫째, 점수 멤버 이라는 GameLayer 클래스에 추가 하겠습니다 `score`:

```csharp
int score;
```

에서는이 변수 플레이어의 점수를 추적 하 고 사용 하 여 표시할 수는 `scoreLabel`합니다. 이렇게 하려면에서 if 문 내에 다음 코드를 추가 `RunGameLogic` 볼 및 패 겹칠 때:

```csharp
// ...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
// ...
```

이제 게임을 실행 하 고 게임 패 들 오프 반송 볼 때마다 증가 하는 점수를 표시 참조 수 우리:

![완료 된 게임 증분 점수가](bouncing-game-images/image1.png "증분 점수가 완료 게임")

## <a name="summary"></a>요약

이 연습에서는 물리학 그래픽과 CocosSharp를 사용 하 여 입력을 사용 하 여 플랫폼 간 게임을 만들기를 표시 합니다. CocosSharp 게임 개발 시작에서 첫 번째 단계입니다. 일부의 CocosSharp, 개체가 제대로 렌더링 하므로 시각적 트리를 생성 하는 방법 및 모든 프레임 게임 논리를 구현 하는 방법의 가장 일반적인 클래스에 찾았습니다.

이 연습에서는 CocosSharp 게임 엔진에서 제공 하는 기능 중 작은 부분만 다룹니다. 정보 및 다른 CocosSharp 주제에 대 한 연습을 참조 하세요. [CocosSharp 가이드의 나머지 부분](~/graphics-games/cocossharp/index.md)합니다.

## <a name="related-links"></a>관련된 링크

- [완료 된 게임 (샘플)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [게임 콘텐츠 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
