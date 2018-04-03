---
title: MonoGame PipelineTool를 사용 하 여
description: MonoGame 파이프라인 도구는 MonoGame 콘텐츠 프로젝트 만들기 및 관리 하는 데 사용 됩니다. 콘텐츠 프로젝트에 있는 파일 Monogame 파이프라인 도구에 의해 처리 되 고 CocosSharp 및 MonoGame 응용 프로그램에서 사용 하기 위해.xnb 파일로 출력 합니다.
ms.topic: article
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 37505b166488230be9d0e0690e415852506664f1
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2018
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구를 사용 하 여

_MonoGame 파이프라인 도구는 MonoGame 콘텐츠 프로젝트 만들기 및 관리 하는 데 사용 됩니다. 콘텐츠 프로젝트에 있는 파일 Monogame 파이프라인 도구에 의해 처리 되 고 CocosSharp 및 MonoGame 응용 프로그램에서 사용 하기 위해.xnb 파일로 출력 합니다._

MonoGame 파이프라인 도구에 대 한 콘텐츠 파일을 변환 하기 위한 사용 하기 쉬운 환경을 제공 **.xnb** CocosSharp 및 MonoGame 응용 프로그램에서 사용할 파일입니다. 참조 콘텐츠 파이프라인 및 이유에서 사용 되는 게임 개발에 대 한 내용은 [콘텐츠 파이프라인에 대 한이 소개](~/graphics-games/cocossharp/content-pipeline/introduction.md)

이 연습에서는 다음 설명 합니다.

 - MonoGame 파이프라인 도구 설치
 - CocosSharp 프로젝트 만들기
 - 콘텐츠 프로젝트 만들기
 - MonoGame 파이프라인 도구 파일을 처리
 - 파일을 사용 하 여 런타임 시

이 연습에서는 CocosSharp 프로젝트를 사용 하 여 보여 주기 위해 방법을 **.xnb** 파일 로드 하 고 응용 프로그램에서 사용할 수 있습니다. MonoGame의 사용자가 동일한 CocosSharp 및 MonoGame 사용이 연습을 참조할 수 있는 수도 **.xnb** 콘텐츠 파일입니다.

완성된 된 응용 프로그램에서 질감 표시 단일 스프라이트 표시 됩니다는 **.xnb** 파일과 sprite 글꼴을 표시 하는 단일 레이블은 **.xnb** 파일:

![](walkthrough-images/image1.png "완성된 된 응용 프로그램에 표시 한 질감.xnb 파일에서 단일 스프라이트 표시 됩니다.")


## <a name="monogame-pipeline-tool-discussion"></a>파이프라인 도구 MonoGame 토론

MonoGame 파이프라인 도구는 Windows, OS X, 및 Linux에서 사용할 수 있습니다. 이 연습에서는 windows에 도구를 실행 하지만 올 수 있습니다를 따라 Mac 및 Linux에도 합니다. Linux 또는 Max를 설정 하는 도구를 가져오는 방법에 대 한 정보를 참조 하십시오. [이 페이지](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/)합니다.

MonoGame 파이프라인 도구는 iOS 응용 프로그램 에서도 실행할 때 windows에서 사용 하는 등 개발자에 대 한 콘텐츠를 만들 수 [Xamarin Mac 에이전트](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Windows에서 개발을 계속할 수 있게 됩니다.


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구 설치

우리는 MonoGame MonoGame 콘텐츠 파이프라인을 포함 하는 설치 하 여 시작 됩니다. MonoGame 콘텐츠 파이프라인은 Mac.에 대 한 별도 다운로드 모든 MonoGame 설치 관리자에서 확인할 수 있습니다는 [MonoGame 다운로드 페이지](http://www.monogame.net/downloads/)합니다. 다운로드에서는 Visual Studio에 대 한 하지만 개발자 설치 되 면 MonoGame도 사용할 수 MonoGame Visual Studio에서 Mac 용:

![](walkthrough-images/image2.png "Visual Studio 이지만 설치 되 면 개발자 사용 MonoGame Visual Studio에서 Mac 용 너무 MonoGame 다운로드")

를 다운로드 한 설치 관리자를 통해 실행 알아보고 기본값 옵션을 그대로 적용 하겠습니다. 설치 관리자가 완료 되 면 MonoGame 파이프라인 도구를 설치 및 시작 메뉴 검색에서 확인할 수 있습니다.

![](walkthrough-images/image3.png "MonoGame 파이프라인 도구를 설치 및 시작 메뉴 검색에서 확인할 수 있습니다 설치 관리자가 완료 되 면")

MonoGame 파이프라인 도구를 시작 합니다.

![](walkthrough-images/image4.png "MonoGame 파이프라인 도구 시작")

MonoGame 파이프라인 도구를 실행 하 고 콘텐츠 및 게임 프로젝트를 시작할 수 있습니다.


## <a name="creating-an-empty-cocossharp-project"></a>빈 CocosSharp 프로젝트 만들기

다음 단계는 CocosSharp 프로젝트를 만드는 것입니다. 만들 없음을 CocosSharp 프로젝트 먼저 म CocosSharp 프로젝트에서 만든 폴더 구조에서이 콘텐츠 프로젝트를 저장할 수 있도록 유용 합니다. CocosSharp 프로젝트의 구조를 이해 하려면 살펴보세요는 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md),이 가이드에 사용 됩니다. 그러나 기존 CocosSharp 프로젝트를 콘텐츠를 추가 하려는 경우 자유롭게 BouncingGame 대신 해당 프로젝트를 사용 합니다.

프로젝트를 만든 후 작성를 제대로 설정 모든 항목이 있는지 확인 하 여 실행 합니다.

![](walkthrough-images/image5.png "프로젝트를 만든 후 빌드 되었는지 확인 하 고 모든 항목이 올바르게 설정 되었다고 실행")


## <a name="creating-a-content-project"></a>콘텐츠 프로젝트 만들기

게임 프로젝트를 만들었으므로 이제 해당 MonoGame 파이프라인 프로젝트를 만들 수 있습니다. 이렇게 하려면, MonoGame 파이프라인 도구 선택에서 **파일 > 새로 만들기...**  프로젝트의 콘텐츠 폴더를 탐색 합니다. Android, 해당 폴더에는 **[root]\BouncingGame.Android\Assets\Content 프로젝트\**합니다. IOS의 경우 해당 폴더에는 **[root]\BouncingGame.iOS\Content 프로젝트\**합니다.

변경 된 **파일 이름** 를 **ContentProject** 클릭는 **저장** 단추:

![](walkthrough-images/image8.png "저장 단추를 클릭 하 고 파일 이름을 ContentProject으로 변경")

프로젝트를 만든 후 MonoGame 파이프라인 도구는 프로젝트에 대 한 정보를 보여 때 루트 **ContentProject** 항목을 선택 합니다.

![](walkthrough-images/image9.png "프로젝트를 만든 후 루트 ContentProject 항목이 선택 될 때 MonoGame 파이프라인 도구는 프로젝트에 대 한 정보를 표시 됩니다.")

콘텐츠 프로젝트에 대 한 가장 중요 한 옵션 중 일부에 대해 살펴보겠습니다.


### <a name="output-folder"></a>출력 폴더

이 폴더는 폴더 (프로젝트에 상대적으로 콘텐츠 자체) 여기서 출력 **.xnb** 파일이 저장 됩니다. 단순화 하기 위해 보유 우리의 입력 및 출력 파일을 같은 폴더를 사용 합니다. 이번 변경 즉는 **출력 폴더** 되도록 **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>플랫폼

콘텐츠에 대 한 대상 플랫폼을 정의합니다. **Windows** 기본적으로 하므로 하고자 즉 대상 플랫폼으로 변경 **Android** (또는 iOS 프로젝트와 함께 수행 하는 경우 iOS).

![](walkthrough-images/image11.png "기본적으로이 Windows 이름은, 따라서 iOS 프로젝트와 함께 다음 Android 또는 iOS은 대상 플랫폼으로 변경")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구 파일을 처리

콘텐츠를 추가 되므로 다음으로 우리의 **ContentProject**합니다. 이 프로젝트에 대 한 추가 되므로 파일, 프로젝트의 루트에 있지만 대규모 프로젝트의 폴더에 콘텐츠를 일반적으로 정리 됩니다.

이 프로젝트에 두 개의 파일 추가 하겠습니다.

 - A **.png** 스프라이트를 그리는 데 사용 되는 파일입니다. 이 파일 수 [여기에서 다운로드](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)합니다.
 - A **.spritefont** 화면에 텍스트를 그리는 데 사용 되는 파일입니다. 콘텐츠 파이프라인 도구 이므로 다운로드할 파일이 없습니다. 새.spritefont 파일을 만들 수 있도록 지원 합니다.


### <a name="adding-a-png-file"></a>.Png 파일 추가

추가 하는 **.png** 파일을 프로젝트에 먼저 복사 합니다 것에 파이프라인 프로젝트와 동일한 디렉터리에는 **.mgcb** 확장 합니다.

![](walkthrough-images/image12.png "파일은.png 파일이 프로젝트에 추가")

다음으로, 파일 파이프라인 프로젝트에 추가 합니다. MonoGame 파이프라인 도구에서이 작업을 수행 하려면 선택 **편집 > 항목 추가...** , 선택는 **ball.png** 파일을 클릭 하 여 **열려**합니다. 파일 콘텐츠 프로젝트의 일부가 될 이제 및 옵션을 선택 하면 해당 속성이 표시 됩니다.

![](walkthrough-images/image13.png "파일을 지금 콘텐츠 프로젝트에 포함 되며, 옵션을 선택 하면 해당 속성이 표시 됩니다.")

त ु म च 변경이 CocosSharp에.xnb 파일을 로드 하는 데 필요 하지 않으면 대로 기본값으로 ळ 남게 되 고 있습니다. 선택 하 여 파일을 빌드할 수 있습니다는 **빌드 > 빌드** 메뉴 옵션을 빌드를 시작 하 고 빌드에 대 한 출력을 표시 합니다. 빌드를 확인 하 여 올바르게 작동 했는지 확인할 수 있는 **콘텐츠** 폴더에 대 한 새 **ball.xnb** 파일:

![](walkthrough-images/image14.png "빌드의 새 ball.xnb 파일에 대 한 콘텐츠 폴더를 확인 하 여 올바르게 작동 했는지 확인")


### <a name="adding-a-spritefont-file"></a>.Spritefont 파일 추가

MonoGame 파이프라인 도구를 통해.spritefont 파일을 만들 수 있습니다. CocosSharp 글꼴에 포함 되도록 필요는 **글꼴** 폴더 및 CocosSharp 템플릿을 자동으로 글꼴이 폴더를 자동으로 만듭니다. 이 폴더를 선택 하 여 MonoGame 파이프라인 도구를 추가할 수 있습니다 **편집 > 추가 > 기존 폴더 중...** . 찾아는 **콘텐츠** 폴더를 선택은 **글꼴** 폴더 **확인**:

![](walkthrough-images/browsetofonts.png "콘텐츠 폴더를 찾습니다. Fonts 폴더를 선택 하 고 확인을 클릭 합니다.")

새.sprintefont 파일을 추가 하려면 Fonts 폴더를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 항목...** 을 선택는 **SpriteFont 설명** 옵션 이름을 입력 합니다. **arial 36**를 클릭 하 고 **확인**합니다. CocosSharp 필요 – 해야만 해당 [FontType] 형식의-글꼴 파일의 특정 이름 지정 [FontSize]. 글꼴 명명이 형식이 일치 하지 않는 경우이 로드 되지 않습니다 CocosSharp 하 여 런타임 시.

![](walkthrough-images/image15.png "글꼴 하지 로드 CocosSharp 하 여 런타임 시이 명명 형식이 일치 하지 않는 경우")

.Spritefont 파일은 실제로 Mac. for Visual Studio를 비롯 한 임의의 텍스트 편집기에서 편집할 수 있는 한 XML 파일 .Spritefont 파일에서 편집 하는 가장 일반적인 변수는는 `FontName` 및 `Size` 속성:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

임의의 텍스트 편집기에서 파일을 엽니다. 으로 우리의 **arial 36.spritefont** 이름을 알 수 있듯이 두겠습니다는 `FontName` 으로 `Arial` 변경 하지만 `Size` 값을 `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>파일을 사용 하 여 런타임 시

.Xnb 파일은 이제 만들어지고 프로젝트에 사용할 준비가 되었습니다. 추가할 예정이 니 파일 Visual Studio를 Mac에 대 한 다음 코드를 추가 하겠습니다 우리의 `GameScene.cs` 파일을이 파일을 로드 하 고 표시 합니다.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Mac 용 Visual Studio에.xnb 파일 추가

먼저 파일의 프로젝트에 추가 합니다. Mac 용 Visual Studio에서 확장 됩니다는 **BouncingGame.Android** 프로젝트를 확장 하 고는 **자산** 폴더를 마우스 오른쪽 단추로 클릭은 **콘텐츠** 폴더를 찾아 선택 **추가 > 파일을 추가 중...** 첫째, 선택는 **ball.xnb** 앞에서 만든 하 고 클릭 **열려**합니다. 그런 다음 위의 단계를 반복 하지만 추가 **arial 36.xnb** 파일입니다. 선택은 **파일의 현재 하위 디렉터리에 보관** Mac 용 Visual Studio 파일을 추가 하는 방법을 요청 하는 경우 옵션입니다. 두 파일에이 프로젝트의 일부가 되어야 합니다. 한 번만 완료:

![](walkthrough-images/image20.png "한 번만 완료 두 파일은 프로젝트의 일부가 되어야 합니다.")


### <a name="adding-gamescenecs"></a>추가 **GameScene.cs**

라는 클래스를 만들겠습니다 `GameScene,` sprite 및 텍스트 개체를 포함 하 합니다. 이렇게 하려면 마우스 오른쪽 단추로 클릭는 **BouncingGame** (하지 BouncingGame.Android) 프로젝트를 마우스 선택 **추가 > 새 파일...** . 선택 된 **일반** 범주를 선택는 **빈 클래스** 옵션을 선택한 다음 이름을 입력 **GameScene**합니다.

를 만든 후 수정 합니다는 `GameScene.cs` 파일에 다음 코드를 포함 합니다.


```csharp
using System;
using CocosSharp;

namespace BouncingGame
{
    public class GameScene : CCScene
    {
        // All visual elements must be added to a CCLayer:
        CCLayer mainLayer;

        // The CCSprite is used to display the "ball" texture
        CCSprite sprite;
        // The CCLabelTtf is used to display the Arial36 sprite font
        CCLabelTtf label;

        public GameScene(CCWindow mainWindow) : base(mainWindow)
        {
            // Instantiate the CCLayer first:
            mainLayer = new CCLayer ();
            AddChild (mainLayer);

            // Now we can create the Sprite using the ball.xnb file:
            sprite = new CCSprite ("ball");
            sprite.PositionX = 200;
            sprite.PositionY = 200;
            mainLayer.AddChild (sprite);

            // The font name (arial) and size (36) need to match 
            // the .spritefont definition and file name.  
            label = new CCLabelTtf ("Using font 36", "arial", 36);
            label.PositionX = 200;
            label.PositionY = 300;
            mainLayer.AddChild (label);
        }
    }
} 
```

여기서 설명 하지 않겠습니다 위의 코드에 대해서는 CCSprite 등 CCLabelTtf CocosSharp 시각적 개체를 사용 하므로 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md)합니다.

또한 우리의 새로 만든 로드 하는 코드를 추가 해야 한다고 `GameScene`합니다. 엽니다이 작업을 수행 하는 `GameAppDelegate.cs` 파일 (에 있는 **BouncingGame** PCL) 수정 하 고는 `ApplicationDidFinishLaunching` 와 비슷하게 메서드:


```csharp
public override void ApplicationDidFinishLaunching (CCApplication application, CCWindow mainWindow)
{
    application.PreferMultiSampling = false;
    application.ContentRootDirectory = "Content";

    // New code:
    GameScene gameScene = new GameScene (mainWindow);
    mainWindow.RunWithScene (gameScene);
} 
```

를 실행 하는 경우에 게임 같이 표시 됩니다.

![](walkthrough-images/image1.png "게임 버리면를 실행 하는 경우")


## <a name="summary"></a>요약

이 연습에서는 파일을 만드는.xnb 입력된.png 파일에서 MonoGame 파이프라인 도구를 사용 하는 방법 뿐만 아니라.sprintefont 새로 만든 파일에서 새.xnb 파일을 만드는 방법을 배웠습니다. 에서도 CocosSharp.xnb 파일을 사용 하는 프로젝트를 구성 하는 방법 및 런타임에 이러한 파일을 로드 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련된 링크

- [MonoGame 다운로드](http://www.monogame.net/downloads/)
- [MonoGame 파이프라인 설명서](http://www.monogame.net/documentation/?page=Pipeline)
- [Android (샘플)에 대 한 BouncingGame 프로젝트 시작](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png 그래픽 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
