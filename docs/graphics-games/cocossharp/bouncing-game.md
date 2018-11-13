---
title: BouncingGame 세부 정보
description: 이 문서는 BouncingGame, CocosSharp를 사용 하 여 빌드한 간단한 바운스 ball 게임 만들기에 대 한 연습을 제공 합니다.
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
author: conceptdev
ms.author: crdun
ms.date: 03/29/2018
ms.openlocfilehash: 9fd6c9108695f58bd69a1c4aa307ca2e4be6dede
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528743"
---
# <a name="bouncinggame-details"></a>BouncingGame 세부 정보

> [!TIP]
> BouncingGame에 대 한 코드에서 찾을 수 있습니다 합니다 [Xamarin 샘플](https://developer.xamarin.com/samples/mobile/BouncingGame/)합니다. 이 [V2]를 가장 잘 따라가려면 다운로드 하 여 가이드를 참조 하는 대로 코드를 탐색 합니다.

이 연습에서는 CocosSharp를 사용 하 여 간단한 바운스 ball 게임을 구현 하는 방법을 보여 줍니다. 

 - 압축을 푸는 게임 콘텐츠
 - 일반적인 CocosSharp 시각적 요소
 - 시각적 요소를 추가합니다. `GameLayer`
 - 프레임의 모든 논리를 구현합니다.

완료 된 게임은 다음과 같습니다.

![BouncingGame, 완료](bouncing-game-images/image1.png "BouncingGame, 완료")

## <a name="unzipping-game-content"></a>압축을 푸는 게임 콘텐츠

이라는 용어를 자주 사용 하는 게임 개발자 *콘텐츠* visual 아티스트, 게임 디자이너 또는 오디오 디자이너에서 일반적으로 생성 되는 비 코드 파일을 참조 합니다. 일반적인 콘텐츠 유형의 시각적 개체를 표시 하거나 소리를 재생 하거나 AI (인공 지능) 동작을 제어 하는 데 사용 하는 파일을 포함 합니다. 게임 개발 팀의 관점에서 보면 콘텐츠는 일반적으로 프로그래머가 아닌 사용자도 하 여 만들어집니다. 게임이 두 가지 유형의 콘텐츠를 포함 합니다.

 - png 파일 공 및 패 들을 표시 하는 방법을 정의입니다.
 - 점수 표시 (아래 점수 표시를 추가할 때 좀 더 자세히 설명)에서 사용 된 글꼴을 정의 하는 단일 xnb 파일

여기에이 콘텐츠를 찾을 수 있습니다 [zip 콘텐츠](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)합니다. 이러한 파일을 다운로드 하 고이 연습의 뒷부분에서 액세스 하는 것을 위치에 압축을 해제 해야 합니다.

## <a name="common-cocossharp-visual-elements"></a>일반적인 CocosSharp 시각적 요소

CocosSharp 다양 한 시각적 개체를 표시 하는 데 사용 하는 클래스를 제공 합니다. 일부 요소는 조직에 사용 되는 언어도 바로 표시 합니다. 다음 게임에서 사용 해야 합니다.

 - `CCNode` – CocosSharp에서 모든 시각적 개체에 대 한 기본 클래스입니다. `CCNode` 클래스를 포함는 `AddChild` 부모/자식 계층을 만드는 데 사용할 수 있는 함수 라고도 하는 *시각적 트리* 또는 *장면 그래프*합니다. 아래에 언급 된 모든 클래스에서 상속 `CCNode`합니다.
 - `CCScene` – 모든 CocosSharp 게임에 대 한 시각적 트리의 루트입니다. 모든 시각적 요소와 시각적 트리의 일부 여야 합니다.는 `CCScene` 루트로 또는 표시 되지 않습니다.
 - `CCLayer` –와 같은 시각적 개체에 대해 컨테이너 개체 `CCSprite`합니다. 이름에서 알 수 있듯이 `CCLayer` 클래스 서로 기반으로 하는 방법을 시각적 요소 계층을 제어 하는 데 사용 됩니다.
 - `CCSprite` – 이미지 또는 이미지의 일부를 표시합니다. `CCSprite` 인스턴스 크기를 조정할 놓을 수 있습니다 하 고 다양 한 시각 효과 제공 합니다.
 - `CCLabel` – 화면에 문자열로 표시합니다. 사용 된 글꼴 `CCLabel` xnb 파일에 정의 됩니다. Xnbs를 아래에서 자세히 설명 하겠습니다.

다른 형식을 사용 하는 방법을 이해 하는 단일 살펴보겠습니다 `CCSprite`합니다. 시각적 요소에 추가 되어야 합니다는 `CCLayer`, 시각적 트리에 있어야 하 고는 `CCScene` 루트입니다. 따라서 단일 부모-자식 관계가 `CCSprite` 것 `CCScene`  >  `CCLayer`  >  `CCSprite`합니다.

## <a name="adding-visual-elements-to-gamelayer"></a>GameLayer에 시각적 요소 추가

### <a name="adding-the-paddle-sprite"></a>패 들 스프라이트를 추가합니다.

검정 및 단일 추가 게임의 배경 설정 처음 `CCSprite` 화면에 렌더링 합니다. 수정 된 `GameLayer` 추가할 클래스는 `CCSprite` 같이:

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

위의 코드 single을 만듭니다 `CCSprite` 의 자식으로 추가 된 `GameLayer`합니다. `CCSprite` 생성자를 문자열로 사용할 이미지 파일을 정의할 수 있습니다. 코드 CocosSharp 라는 파일을 찾도록 지시 `paddle` (확장명을 생략 하면이 가이드의 뒷부분에서 설명할 것입니다). CocosSharp는 모든 파일 이름을 찾습니다 `paddle` 콘텐츠 루트 폴더에 (되 **콘텐츠**)에 추가 된 모든 폴더와는 `gameView.ContentManager.SearchPaths` (이전 섹션에서 설명).

파일 루트에 직접 추가한 **콘텐츠** iOS 및 Android에 대 한 폴더입니다. 이렇게 하려면 마우스 오른쪽 단추 메뉴나 컨트롤-클릭 합니다 **콘텐츠** 폴더에서 iOS 프로젝트를 마우스 **추가** > **파일 추가...** 선택한 위치에서는 압축을 푼 콘텐츠 앞으로 이동 **paddle.png**합니다. 폴더에 파일을 추가 하는 방법에 대 한 요청 되 면 선택 해야 합니다 **복사** 옵션:

![폴더 대화 상자에 파일 추가](bouncing-game-images/image2.png "폴더 대화 상자는 추가 파일")

그런 다음 Android 프로젝트에 파일을 추가 합니다. 마우스 오른쪽 단추 메뉴나 컨트롤-클릭 Content 폴더 (보기로 제공 되는 **자산** Android 프로젝트의 폴더) 선택한 **추가** > **파일 추가...** . 이 이번에는 iOS 프로젝트의 이동할 **콘텐츠** 폴더입니다. 파일을 추가 하는 방법에 대 한 요청을 받는 경우 선택 합니다 **링크를 추가할** 옵션:

![폴더 대화 상자에 파일 추가](bouncing-game-images/addalink.png "폴더 대화 상자에 파일 추가")

아래 두 프로젝트에 추가할 파일 했습니다.이 이유 설명 하겠습니다. 각 프로젝트의 **콘텐츠** 폴더에 이제 포함 된 **paddle.png** 파일:

![콘텐츠 폴더의 내용을](bouncing-game-images/image3.png "콘텐츠 폴더의 콘텐츠")

살펴보겠습니다 게임 실행 하는 경우는 `CCSprite` 그리고:

![화면에 그릴 패 들](bouncing-game-images/image4.png "화면에 그릴 패 들,")

### <a name="file-details"></a>파일 세부 정보

단일 파일을 프로젝트에 추가 했습니다 지금 및 프로세스는 비교적 간단 했습니다. 추가 하기만 합니다 **paddle.png** 파일을 합니다 **콘텐츠** 폴더 코드에서 참조 하 고 합니다. CocosSharp에서 파일로 작업할 때 몇 가지 고려 사항을 확인 하려면 잠시를 살펴보겠습니다.

#### <a name="capitalization"></a>대문자 표시

파일 이름 및에서는 사용 되는 문자열이 코드 파일에 액세스 하는 소문자 모두 확인할 수 있습니다. 일부 플랫폼 (예: Windows 데스크톱 및 iOS 시뮬레이터)은 대/소문자를 구분 합니다 (예: Android 및 iOS 장치) 다른 플랫폼은 대/소문자 구분 때문입니다. 파일 및 코드를 플랫폼으로 남아 있도록이 자습서의 나머지 부분에 대 한 모든 소문자 파일 사용은 최대한 합니다.

#### <a name="extensions"></a>확장

스프라이트를 만들기 위한 생성자 패 들 파일을 참조할 때 ".png" 확장명을 포함 되지 않습니다. 파일의 확장명은 플랫폼 간에 다를 수 동일한 자산 형식에 대해 파일 확장명으로 CocosSharp 프로젝트에서 작업 하는 경우에 일반적으로 생략 됩니다. 예를 들어, 오디오 파일은 플랫폼에 따라.aiff. mp3,.wma 파일 형식의 수 있습니다. 확장 삭제 파일 확장명에 관계 없이 모든 플랫폼에서 작동 하는 동일한 코드를 허용 합니다.

#### <a name="content-in-platform-specific-projects"></a>플랫폼별 프로젝트에 콘텐츠

PCL에 있을 수 있습니다 하는 코드 파일의 대부분에서는 달리 콘텐츠 파일을 각 플랫폼별 프로젝트에 추가 되어야 합니다. CocosSharp에 두 가지 이유로이 필요합니다.

1. 각 플랫폼에는 다양 한 **빌드 작업**합니다. IOS 프로젝트에 추가 하는 콘텐츠 사용 해야 합니다 **BundleResource** 빌드 작업입니다. Android 프로젝트에 추가 하는 콘텐츠 사용 해야 합니다 **AndroidAsset** 빌드 작업입니다.
2. 일부 플랫폼에 플랫폼 특정 파일 형식 필요합니다. 예를 들어 글꼴.xnb 파일이이 연습의 뒷부분에서 살펴보겠지만 iOS와 Android 간에 서로 다릅니다.

파일 형식 (예:.png) 플랫폼 간에 다르지 않습니다, 경우 하나 이상의 플랫폼 동일한 위치에서 파일을 연결할 수 있는 동일한 파일을 각 플랫폼에 사용할 수 있습니다.

## <a name="orientation"></a>방향

다른 앱 처럼 CocosSharp 앱 세로 또는 가로 방향에서 실행할 수 있습니다. 게임을 세로 전용 모드에서 실행 하도록 조정 됩니다. 첫째, 세로 가로 세로 비율을 처리 하도록 게임에서 확인 코드를 변경 합니다. 이 위해 수정 합니다 `width` 및 `height` 값을 `LoadGame` 에서 메서드를 `ViewController` iOS 클래스 및 `MainActivity` Android 클래스:

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

IOS 프로젝트의 방향을 수정 하려면 선택는 **Info.plist** 파일을 **BouncingGame.iOS** 프로젝트를 마우스 변경 **iPhone/iPod 배포 정보** 및 합니다 **iPad 배포 정보** 세로 방향을 포함 하도록 합니다.

![방향 옵션](bouncing-game-images/image5.png "방향 옵션")

### <a name="android-orientation"></a>Android 방향

Android 프로젝트의 방향을 수정 하려면 BouncingGame.Android 프로젝트에서 MainActivity.cs 파일을 엽니다. 다음과 같이 세로 방향만 지정 하므로 활동 특성 정의 수정 합니다.

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>기본 좌표계

인스턴스화하는 코드를 `CCSprite`를 설정 합니다 `PositionX` 및 `PositionY` 100 사이의 값입니다. 기본적으로 즉는 `CCSprite` center를 화면의 왼쪽 아래에서에서 오른쪽으로 100 픽셀에 배치 됩니다. 좌표계를 연결 하 여 수정할 수는 `CCCamera` 에 `CCLayer`합니다. 사용 하 여 노력 하지 않습니다 `CCCamera` 이 프로젝트에 대 한 자세한 내용은 `CCCamera` 에서 찾을 수 있습니다 합니다 [CocosSharp API 문서](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/)합니다.

하드웨어에서 픽셀 달리 "게임" 픽셀 위에서 언급 한 100 픽셀입니다. 이 같은 게임 (iPhone 및 iPad)와 같은 다른 해상도의 장치에서 실행 하면 개체에 배치 되 고 실제 화면을 기준으로 올바르게 크기를 의미 합니다. 특히, 게임의 표시 영역은 항상 1024 픽셀 높이 및 768 픽셀, 해상도 이라고 지정 했기 때문에 이전에 `LoadGame` 메서드.

## <a name="adding-the-ball-sprite"></a>공 스프라이트를 추가합니다.

우리가 사용 하는 기본 사항을 했으므로 `CCSprite`, 두 번째 추가 `CCSprite` – 공입니다. 에서는 따르게 됩니다는 매우 유사한 패 들을 생성 하는 단계 `CCSprite`합니다. 

먼저 추가 합니다 **ball.png** iOS 프로젝트의 압축 푼된 폴더에서 이미지 **콘텐츠** 폴더입니다. 선택 하 여 **복사본** 파일을 **콘텐츠** 디렉터리입니다. 에 대 한 링크를 추가 하려면 위와 동일한 단계를 수행 합니다 **ball.png** Android 프로젝트의 파일입니다.

다음 만듭니다는 `CCSprite` 라는 멤버를 추가 하 여이 공 `ballSprite` 에 `GameScene` 에 대 한 인스턴스화 코드 뿐만 아니라 클래스는 `ballSprite`합니다. 완료 되 면를 `GameLayer` 클래스는 다음과 같이 표시 됩니다.

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

## <a name="adding-the-score-label"></a>점수 매기기 레이블 추가

게임에 추가할 것 마지막 visual 요소는 한 `CCLabel` 횟수를 표시 하려면 사용자가 성공적으로 반송 됨 공입니다. `CCLabel` 생성자에 지정 된 글꼴을 사용 하 여 화면에 문자열을 표시 합니다.

다음 코드를 추가 `CCLabel` 인스턴스의 `GameLayer`합니다. 완료 된 후 코드는 다음과 같아야 합니다.

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

ScoreLabel를 사용 하는 `AnchorPoint` 의 `CCPoint.AnchorUpperLeft`, 즉를 `PositionX` 및 `PositionY` 값의 왼쪽 위 위치를 정의 합니다. 통해이 위치는 `scoreLabel` 레이블의 크기를 고려해 야 할 필요 없이 화면의 왼쪽 위를 기준으로 합니다.

## <a name="implementing-every-frame-logic"></a>프레임의 모든 논리를 구현합니다.

지금 게임 정적 장면을 표시합니다. 높은 빈도로 개체의 위치를 업데이트 하는 코드를 추가 하 여 장면의 개체의 움직임을 제어 하는 논리를 추가할 예정입니다. 60 시간 / 초-라고도 60까지 코드가 실행 됩니다는 예에서 *프레임* 초당 (하지 않는 한 하드웨어가이 자주 업데이트를 처리할 수 없습니다). 구체적으로 추가할 예정 공을 속하며 공을 패 들 발생할 때마다 플레이어의 점수를 업데이트 하 고 입력에 따라 이동 하는 데 패 들에 대해 반송 하는 논리입니다.

합니다 `Schedule` 에서 제공 하는 메서드는 `CCNode` 클래스를 통해 게임 프레임의 모든 논리를 추가할 수 있습니다. 뒤에 코드를 추가한 `// New code` GameLayer 생성자:

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

이제 만들기를 `RunGameLogic` 에서 메서드는 `GameLayer` 모든 프레임의 모든 논리를 저장할 클래스:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Float 매개 변수는 초 단위로 시간 프레임을 정의 합니다. 사용이 값은 공의 움직임을 구현 하는 경우.

### <a name="making-the-ball-fall"></a>대체 공 만들기

CocosSharp에서 기본 제공 된 Box2D 기능을 사용 하 여 또는 수동으로 구현 하거나 중력 코드로 대체 하 여 공을 만들 수 있습니다. 물리학 시뮬레이션 된 Box2D 엔진은 CocosSharp 게임 수 있습니다. 매우 강력 하 고 효율적 이지만 설치 코드를 작성 해야 합니다. 간단한 물리학 시뮬레이션 이므로 여기이 구현 됩니다 수동으로.

무게를 구현 하려면 저장소 현재 X 공 Y 속도를 해야 합니다. 두 멤버를 추가 합니다 `GameLayer` 클래스:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

떨어지는 논리를 구현할 수 있습니다 다음 `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>터치식 입력을 사용 하 여 패 들을 이동

공 속도가 늦는 이제 추가한 수평 이동 패 들을 사용 하 여는 `CCEventListenerTouchAllAtOnce` 개체입니다. 이 개체는 다양 한 입력 관련 이벤트를 제공합니다. 이 예에서는 패 들의 위치를 조정할 수 있도록 모든 터치 지점을 이동 하는 경우 알림을 받을 하려고 합니다. `GameLayer` 이미 인스턴스화하는 `CCEventListenerTouchAllAtOnce`이므로 할당 해야는 `OnTouchesMoved` 대리자. 이렇게 하려면 AddedToScene 메서드를 다음과 같이 수정 합니다.

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

다음으로, 구현 하겠습니다 `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>공 충돌 구현

게임 실행 하는 경우 이제 것을 보게 공을 패 들 실패로 합니다. 구현 하겠습니다 *충돌* (겹치는 게임 개체에 반응 하는 논리) 프레임 마다 코드에서입니다. 프레임 마다를 배치 하는 이동 개체가 변경, 충돌 확인 되므로 일반적으로 프레임 마다 수행 합니다. 또한 추가할 예정 속도 X 축의 공의 도전을 게임에 추가할 패 들에 도달 합니다. 하지만 공이 화면 가장자리를 지나서 이동 해야 하는 경우. 에서는이 작업을 수행 합니다 `RunGameLogic` 속도를 적용 한 후 코드를 `ballSprite` 후 `// New Code`:

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

이제 게임을 플레이할 했으므로 점수 매기기 논리를 추가 하는 마지막 단계가입니다. 먼저 점수 멤버 라는 GameLayer 클래스에 추가 `score`:

```csharp
int score;
```

에서는이 변수 플레이어의 점수를 추적 하 고 사용 하 여 표시 하는 `scoreLabel`합니다. 이렇게 하려면에서 if 문 안에 다음 코드를 추가 `RunGameLogic` 볼 및 패 들이 겹칠 때:

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

이제 수 게임을 실행 하 고 게임 공을 패 들에서 바운스으로 증가 하는 점수를 표시 합니다.

![증분 점수를 사용 하 여 완료 된 게임](bouncing-game-images/image1.png "증분 점수를 사용 하 여 완료 된 게임을")

## <a name="summary"></a>요약

이 연습에서는 그래픽, 물리, 및 CocosSharp를 사용 하 여 입력을 사용 하 여 플랫폼 간 게임을 만들기 표시 합니다. CocosSharp 게임 개발 시작 하기는 첫 번째 단계는 것입니다. CocosSharp, 개체는 올바르게 렌더링 하도록 시각적 트리를 생성 하는 방법 및 프레임 마다 게임 논리를 구현 하는 방법의 가장 일반적인 클래스 중 일부에 대해 살펴보았습니다.

이 연습에서는 CocosSharp 게임 엔진에서 제공 하는 기능 중 일부만을 설명 합니다. 정보 및 기타 CocosSharp 항목에 대 한 연습을 참조 하세요 [CocosSharp 가이드의 나머지 부분](~/graphics-games/cocossharp/index.md)합니다.

## <a name="related-links"></a>관련 링크

- [완료 된 게임 (샘플)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [게임 콘텐츠 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
