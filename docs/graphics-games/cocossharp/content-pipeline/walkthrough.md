---
title: MonoGame 파이프라인 도구를 사용 하 여
description: MonoGame 파이프라인 도구 콘텐츠 MonoGame 프로젝트 만들기 및 관리 됩니다. 콘텐츠 프로젝트에 있는 파일 MonoGame 파이프라인 도구를 통해 처리 되 고 CocosSharp 및 MonoGame 응용 프로그램에서 사용 하기 위해.xnb 파일로 출력 합니다.
ms.prod: xamarin
ms.assetid: CACFBF5F-BBD4-4D46-8DDA-1F46466725FD
author: charlespetzold
ms.author: chape
ms.date: 03/27/2017
ms.openlocfilehash: 347cb7e9d417f97cb6e8d78e67b1c76a378cd188
ms.sourcegitcommit: 7ffbecf4a44c204a3fce2a7fb6a3f815ac6ffa21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "34783301"
---
# <a name="using-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구를 사용 하 여

_MonoGame 파이프라인 도구 콘텐츠 MonoGame 프로젝트 만들기 및 관리 됩니다. 콘텐츠 프로젝트에 있는 파일 MonoGame 파이프라인 도구를 통해 처리 되 고 CocosSharp 및 MonoGame 응용 프로그램에서 사용 하기 위해.xnb 파일로 출력 합니다._

MonoGame 파이프라인 도구를 콘텐츠 파일을 변환 하는 데 사용 하기 쉬운 환경을 제공 **.xnb** CocosSharp 및 MonoGame 응용 프로그램에서 사용할 파일입니다. 콘텐츠 파이프라인 및 게임 개발에 유용한 통계가 이유에 대 한 정보를 참조 하세요. [콘텐츠 파이프라인에 대 한이 소개](~/graphics-games/cocossharp/content-pipeline/introduction.md)

이 연습에서는 다음을 다룹니다.

 - MonoGame 파이프라인 도구를 설치합니다.
 - CocosSharp 프로젝트 만들기
 - 콘텐츠 프로젝트 만들기
 - MonoGame 파이프라인 도구 파일을 처리
 - 파일을 사용 하 여 런타임 시

이 연습에서는 보여 주기 위해 CocosSharp 프로젝트를 사용 하는 방법 **.xnb** 파일 로드 및 응용 프로그램에서 사용할 수 있습니다. MonoGame 사용자 CocosSharp와 MonoGame 둘 다 사용 하는 동일한이 연습을 참조할 수 수도 **.xnb** 파일 콘텐츠입니다.

완성된 된 응용 프로그램에서 질감을 표시 하는 단일 스프라이트 표시 됩니다는 **.xnb** 파일과에서 스프라이트 글꼴을 표시 하는 단일 레이블은 **.xnb** 파일:

![](walkthrough-images/image1.png "완성된 된 앱.xnb 파일에서 질감을 표시 하는 단일 스프라이트 표시 됩니다.")


## <a name="monogame-pipeline-tool-discussion"></a>MonoGame 파이프라인 도구 설명

MonoGame 파이프라인 도구는 Windows, OS X 및 Linux에서 사용할 수 있습니다. 이 연습에서는 Windows에서 도구를 실행 됩니다 있지만 올 수 있습니다를 따라 Mac 및 Linux도 합니다. 최대 또는 Linux에서 설정 도구를 가져오는 방법에 대 한 정보를 참조 하세요 [이 페이지](http://www.monogame.net/2015/01/09/monogame-pipeline-tool-available-for-macos-and-linux/)합니다.

MonoGame 파이프라인 도구는 iOS 응용 프로그램 에서도에서 실행할 때 Windows를 사용 하 여 하므로 개발자에 대 한 콘텐츠를 만들지 못했습니다 [Xamarin Mac Agent](~/ios/get-started/installation/windows/connecting-to-mac/index.md) Windows에서 개발을 계속할 수 있게 됩니다.


## <a name="installing-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구를 설치합니다.

MonoGame MonoGame 콘텐츠 파이프라인을 포함 하는 설치 하 여 시작 됩니다. MonoGame 콘텐츠 파이프라인에는 mac 용 별도 다운로드 모든 MonoGame 설치 관리자에서 찾을 수 있습니다 합니다 [MonoGame 다운로드 페이지](http://www.monogame.net/downloads/)합니다. 다운로드에서는 MonoGame for Visual Studio가 있지만 개발자 설치 되 면도 사용할 수 MonoGame Visual Studio에서 Mac 용:

![](walkthrough-images/image2.png "Visual Studio 용 이지만 설치 되 면 개발자 사용 MonoGame Visual Studio에서 Mac에 대 한 너무 MonoGame 다운로드")

다운로드 되 면에서는 설치 관리자를 통해 실행 하 고 기본값 옵션을 그대로 적용 합니다. 설치 관리자에는 다음이 완료 되 면 MonoGame 파이프라인 도구를 설치 하 고 시작 메뉴 검색에서 찾을 수 있습니다.

![](walkthrough-images/image3.png "설치 관리자가 완료 되 면 MonoGame 파이프라인 도구를 설치 후 시작 메뉴 검색에서 찾을 수 있습니다.")

MonoGame 파이프라인 도구를 시작 합니다.

![](walkthrough-images/image4.png "MonoGame 파이프라인 도구를 시작 합니다.")

MonoGame 파이프라인 도구를 실행 중이면 콘텐츠 및 게임 프로젝트를 확인 합니다. 시작할 수 있습니다.


## <a name="creating-an-empty-cocossharp-project"></a>빈 CocosSharp 프로젝트 만들기

다음 단계 CocosSharp 프로젝트를 만드는 것입니다. 만드는 것입니다 CocosSharp 프로젝트 먼저 우리 CocosSharp 프로젝트에서 만든 폴더 구조의 콘텐츠 프로젝트를 저장할 수 있도록 반드시 합니다. CocosSharp 프로젝트의 구조를 이해 하려면 잠시 살펴를 [BouncingGame](~/graphics-games/cocossharp/bouncing-game.md),이 가이드에서 사용 됩니다. 그러나 기존 CocosSharp 프로젝트 콘텐츠를 추가 하 고 싶은 경우 자유롭게 BouncingGame 대신 해당 프로젝트를 사용 합니다.

프로젝트를 만든 후 작성을 올바르게 설정 했으므로 모든 확인을 위해 실행 됩니다.

![](walkthrough-images/image5.png "프로젝트를 만든 후 빌드 되었는지 확인 하 고 모든 항목이 올바르게 설정 되었는지 실행")


## <a name="creating-a-content-project"></a>콘텐츠 프로젝트 만들기

이제 게임 프로젝트로, 스토리를 만들었으므로 MonoGame 파이프라인 프로젝트를 만들 수 있습니다. MonoGame 파이프라인 도구에서이 작업을 수행 하려면 **파일 > 새로 만들기...**  프로젝트의 Content 폴더로 이동 합니다. Android에 대 한 폴더 위치는 **[프로젝트 root]\BouncingGame.Android\Assets\Content\** 합니다. IOS에 대 한 폴더 위치는 **[프로젝트 root]\BouncingGame.iOS\Content\** 합니다.

변경 합니다 **파일 이름** 에 **ContentProject** 클릭 합니다 **저장** 단추:

![](walkthrough-images/image8.png "ContentProject에 파일 이름을 변경 하 고 저장 단추를 클릭 합니다.")

MonoGame 파이프라인 도구는 프로젝트에 대 한 정보를 보여 프로젝트가 만들어지면 때 루트 **ContentProject** 항목을 선택 합니다.

![](walkthrough-images/image9.png "MonoGame 파이프라인 도구 루트 ContentProject 항목이 선택 될 때 프로젝트에 대 한 정보를 표시 됩니다는 프로젝트를 만든 후")

콘텐츠 프로젝트에 대 한 가장 중요 한 옵션 중 일부에 대해 살펴보겠습니다.


### <a name="output-folder"></a>출력 폴더

이 폴더는 폴더 (프로젝트에 상대적 콘텐츠 자체)는 출력 **.xnb** 파일이 저장 됩니다. 간단히를 같은 폴더를 입력을 보유할 출력 파일 사용 하겠습니다. 이번 변경 즉 합니다 **출력 폴더** 되도록 **.\**  :

![](walkthrough-images/image10.png "")


### <a name="platform"></a>플랫폼

이 콘텐츠에 대 한 대상 플랫폼을 정의합니다. 이 **Windows** 기본적으로 따라서 하고자는 대상 플랫폼 변경 **Android** (또는 iOS는 iOS 프로젝트와 함께 수행 하는 경우).

![](walkthrough-images/image11.png "기본적으로 Windows 임을 알 수 있습니다.이 Android 또는 iOS는 iOS 프로젝트와 함께 수행 하는 경우 대상 플랫폼을 변경 하십시오")


## <a name="processing-files-in-the-monogame-pipeline-tool"></a>MonoGame 파이프라인 도구 파일을 처리

다음으로 추가할 예정 콘텐츠를 우리의 **ContentProject**합니다. 이 프로젝트에 추가 되므로 파일 프로젝트의 루트에 있지만 대규모 프로젝트 폴더에 있는 해당 콘텐츠를 일반적으로 구성 됩니다.

두 개의 파일 프로젝트를 추가 합니다.

 - A **.png** 스프라이트를 그리는 데 사용할 파일입니다. 이 파일 수 [여기에서 다운로드](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)합니다.
 - A **.spritefont** 화면에 텍스트 그리기에 사용 되는 파일입니다. 콘텐츠 파이프라인 도구를 다운로드 하려면 파일이 새.spritefont 파일 만들기를 지원 합니다.


### <a name="adding-a-png-file"></a>.Png 파일 추가

추가할를 **.png** 파일을 프로젝트에에서는에서는 먼저 복사 있는 파이프라인 프로젝트와 동일한 디렉터리에는 **.mgcb** 확장 합니다.

![](walkthrough-images/image12.png ".Png 파일을 프로젝트에 추가")

그런 다음 파이프라인 프로젝트에 파일을 추가 합니다. MonoGame 파이프라인 도구에서이 작업을 수행 하려면 **편집 > 항목 추가...** 를 선택 합니다 **ball.png** 파일을 클릭 **오픈**합니다. 파일 콘텐츠 프로젝트의 일부가 될 이제 및 옵션을 선택 하면 해당 속성이 표시 됩니다.

![](walkthrough-images/image13.png "파일 콘텐츠는 프로젝트의 일부가 될 이제을 선택 하면 해당 속성이 표시 됩니다.")

변경 하지 않고는 CocosSharp에서.xnb 파일을 로드 하는 데 필요한 모든 값은 기본값 유지 되는 것이 우리가 합니다. 파일을 선택 하 여 작성할 수 있습니다는 **빌드 > 빌드** 메뉴 옵션을 빌드를 시작 하 고 빌드에 대 한 출력을 표시 합니다. 빌드를 확인 하 여 제대로 작동 확인할 수 있습니다 합니다 **콘텐츠** 새 폴더 **ball.xnb** 파일:

![](walkthrough-images/image14.png "새 ball.xnb 파일의 콘텐츠 폴더를 확인 하 여 빌드가 올바르게 작동 확인")


### <a name="adding-a-spritefont-file"></a>.Spritefont 파일 추가

.Spritefont 파일 MonoGame 파이프라인 도구를 통해 만들 수 있습니다. CocosSharp 필요한 글꼴에는 **글꼴** 폴더 및 CocosSharp 템플릿을 자동으로 글꼴 폴더를 자동으로 만듭니다. MonoGame 파이프라인 도구를이 폴더를 선택 하 여 추가할 수 **편집 > 추가 > 기존 폴더...** . 로 이동 합니다 **콘텐츠** 폴더를 선택 합니다 **글꼴** 폴더를 클릭 **확인**:

![](walkthrough-images/browsetofonts.png "콘텐츠 폴더를 찾습니다. 글꼴 폴더를 선택 하 고 확인을 클릭 합니다.")

새.sprintefont 파일을 추가할 글꼴 폴더를 마우스 오른쪽 단추로 클릭 하 고 선택 **추가 > 새 항목...** 를 선택 합니다 **SpriteFont 설명** 옵션의 이름을 입력 합니다 **arial 36**, 클릭 **확인**합니다. CocosSharp 필요 – 되어야 합니다 [FontType] 형식의-글꼴 파일의 매우 구체적인 이름 지정 [FontSize]. 글꼴을이 명명 형식은 일치 하지 않는 경우이 로드 되지 않습니다 CocosSharp에서 런타임 시.

![](walkthrough-images/image15.png "글꼴을이 로드 되지 않습니다 CocosSharp에서 런타임 시이 명명 형식은 일치 하지 않는 경우")

.Spritefont 파일은 mac 용 Visual Studio를 비롯 한 임의의 텍스트 편집기에서 편집할 수 있는 XML 파일 실제로 가장 일반적인 변수.spritefont 파일에서 편집 합니다 `FontName` 고 `Size` 속성:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>

    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->
    <Size>12</Size> 
```

에서는 임의의 텍스트 편집기에서 파일을 엽니다. 으로 우리의 **arial 36.spritefont** 이름을 제안 하 고 그대로 유지 하겠습니다 합니다 `FontName` 으로 `Arial` 변경 하지만 `Size` 값을 `36`:


```xml
    <!-- Modify this string to change the font that will be imported. -->
    <FontName>Arial</FontName>   
  
    <!-- Size is a float value, measured in points. 
    Modify this value to change the size of the font. -->4/10/2016 12:57:28 PM 
    <Size>36</Size>
```
 
## <a name="using-files-at-runtime"></a>파일을 사용 하 여 런타임 시

.Xnb 파일은 이제 기본 제공 되며 프로젝트에 사용할 준비가 되었습니다. 추가할 예정 파일은 Visual Studio를 Mac에 대 한 다음 코드를 추가 했습니다 우리의 `GameScene.cs` 이러한 파일을 로드 하 고 표시할 파일입니다.


### <a name="adding-xnb-files-to-visual-studio-for-mac"></a>Mac 용 Visual Studio.xnb 파일 추가

먼저 파일을 프로젝트에 추가 합니다. 확장에서는 Mac 용 Visual Studio에서를 **BouncingGame.Android** 프로젝트를 확장 합니다 **자산** 폴더를 마우스 오른쪽 단추로 클릭 합니다 **콘텐츠** 폴더를 마우스 선택 **추가 > 파일 추가...** 먼저 선택 합니다 **ball.xnb** 앞서 작성 하 고 클릭 **열려**합니다. 그런 다음 위의 단계를 반복 하지만 추가 합니다 **arial 36.xnb** 파일입니다. 선택 합니다 **현재 해당 하위 디렉터리에 파일을 유지** Mac 용 Visual Studio 파일을 추가 하는 방법을 요청 하는 경우 옵션입니다. 완료 된 후 두 파일에는 프로젝트의 일부 여야 합니다.

![](walkthrough-images/image20.png "완료 된 후 두 파일은 프로젝트의 일부 여야 합니다.")


### <a name="adding-gamescenecs"></a>추가 **GameScene.cs**

라는 클래스를 만들겠습니다 `GameScene,` 스프라이트와 텍스트 개체가 포함 됩니다. 이렇게 하려면 마우스 오른쪽 단추로 클릭 합니다 **BouncingGame** (없습니다 BouncingGame.Android) 프로젝트를 마우스 **추가 > 새 파일...** . 선택 합니다 **일반** 범주를 선택 합니다 **빈 클래스** 옵션을 선택한 다음 이름을 입력 **GameScene**합니다.

를 만든 후 수정 된 `GameScene.cs` 다음 코드를 포함 하는 파일:


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

에서는 설명 하지 않겠습니다 위의 코드 CCSprite 등 CCLabelTtf CocosSharp 시각적 개체를 사용 하 여 작업에 대해서는 이후 합니다 [BouncingGame 가이드](~/graphics-games/cocossharp/bouncing-game.md)합니다.

이 새로 만든 로드 하는 코드를 추가 해야 `GameScene`합니다. 엽니다에서는이 작업을 수행 하는 `GameAppDelegate.cs` 파일 (에 있는 **BouncingGame** PCL) 수정 하 고는 `ApplicationDidFinishLaunching` 와 비슷하게 메서드:


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

를 실행 하는 경우 게임이 같이 표시 됩니다.

![](walkthrough-images/image1.png "게임 모양은 실행 하는 경우")


## <a name="summary"></a>요약

이 연습에서는.sprintefont 새로 만든 파일에서 새.xnb 파일을 만드는 방법 뿐만 아니라 입력된.png 파일에서.xnb 파일을 만들려면 MonoGame 파이프라인 도구를 사용 하는 방법을 보여 줍니다. CocosSharp.xnb 파일을 사용 하는 프로젝트를 구조화 하는 방법과 런타임 시 이러한 파일을 로드 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame 다운로드](http://www.monogame.net/downloads/)
- [MonoGame 파이프라인 설명서](http://www.monogame.net/documentation/?page=Pipeline)
- [Android (샘플)에 대 한 BouncingGame 프로젝트 시작](https://developer.xamarin.com/samples/BouncingGameCompleteAndroid/)
- [ball.png 그래픽 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/ball.png?raw=true)
