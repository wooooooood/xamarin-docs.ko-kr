---
title: Xamarin.forms에서 CocosSharp를 사용 하 여
description: CocosSharp는 사용 하 여 고급 시각화에 대 한 응용 프로그램에 정확한 도형, 이미지 및 텍스트 렌더링을 추가할 수 있습니다.
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/03/2016
ms.openlocfilehash: 7ce541134e6db9a26699f96ab3114ced2ad22244
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.forms에서 CocosSharp를 사용 하 여

_CocosSharp는 사용 하 여 고급 시각화에 대 한 응용 프로그램에 정확한 도형, 이미지 및 텍스트 렌더링을 추가할 수 있습니다._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**2016 발전: Cocos # xamarin.forms**

## <a name="overview"></a>개요

CocosSharp는 그래픽 표시, 터치식 입력을 읽는, 오디오 및 관리 콘텐츠를 재생 하는 유연 하 고 강력한 기술입니다. 이 가이드에서는 CocosSharp Xamarin.Forms 응용 프로그램에 추가 하는 방법에 설명 합니다. 다음 내용을 설명 합니다.

* [CocosSharp 란?](#what)
* [CocosSharp Nuget 패키지를 추가합니다.](#nuget)
* [연습: CocosSharp Xamarin.Forms 앱 추가](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp 란?

[CocosSharp](~/graphics-games/cocossharp/index.md) 는 Xamarin 플랫폼에서 사용할 수 있는 오픈 소스 게임 엔진입니다.
CocosSharp는 다음과 같은 기능을 포함 하는 효율적인 런타임 라이브러리:

* 사용 하 여 이미지 렌더링은 [CCSprite 클래스](https://developer.xamarin.com/api/type/CocosSharp.CCSprite/)
* 사용 하 여 도형을 렌더링은 [CCDrawNode 클래스](https://developer.xamarin.com/api/type/CocosSharp.CCDrawNode/)
* 사용 하 여 모든 프레임 논리는 [CCNode.Schedule 메서드](https://developer.xamarin.com/api/member/CocosSharp.CCNode.Schedule/p/System.Action%7BSystem.Single%7D/)
* 콘텐츠 관리 (로드 및 언로드.png 파일 등의 리소스)를 사용 하 여 [CCTextureCache 클래스](https://developer.xamarin.com/api/type/CocosSharp.CCTextureCache/)
* 사용 하 여 애니메이션은 [CCAction 클래스](https://developer.xamarin.com/api/type/CocosSharp.CCAction/)

CocosSharp의 주된 초점은 플랫폼 2D 게임; 만들기 과정을 간소화 하는 것 그러나 Xamarin 양식 응용 프로그램에 유용한 추가 수도 있습니다. 게임은 일반적으로 효율적인 렌더링 및 시각적 개체 정밀 하 게 제어 해야, 있으므로 CocosSharp 강력한 시각화 및 영향에 비 게임 응용 프로그램에 추가 하려면 사용할 수 있습니다.

Xamarin.Forms는 네이티브 플랫폼 특정 UI 시스템을 기반으로 합니다. 예를 들어 [ `Button`s](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) iOS 및 Android에서 다르게 표시 되 고 운영 체제 버전에도 다를 수 있습니다. 반면, CocosSharp 모든 시각적 개체는 모든 플랫폼에서 동일 하 게 나타납니다 하므로 플랫폼별 시각적 개체를 사용 하지 않습니다. 물론, 해상도 및 가로 세로 비율 장치 간에 다이 영향을 미칩니다 CocosSharp 해당 시각적 개체를 렌더링 하는 방법입니다. 이 가이드의 뒷부분에 나오는 이러한 세부 정보를 설명 합니다.

더 자세한 정보를 찾을 수 있습니다는 [CocosSharp 섹션](~/graphics-games/cocossharp/index.md)합니다.

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp Nuget 패키지를 추가합니다.

CocosSharp를 사용 하기 전에 개발자 Xamarin.Forms 프로젝트에 대 한 몇 가지 추가 확인 해야 합니다.
이 가이드에서는 iOS, Android, 및 표준.NET을 사용 하 여 Xamarin.Forms 프로젝트 가정 라이브러리 프로젝트.
.NET 표준 라이브러리 프로젝트; 기록 되어 모든 코드 그러나 iOS 및 Android 프로젝트에 라이브러리를 추가 합니다.

CocosSharp Nuget 패키지는 모든 CocosSharp 개체를 만드는 데 필요한 개체를 포함 합니다.
CocosSharp.Forms nuget 패키지에 포함 된 `CocosSharpView` xamarin.forms에서 CocosSharp 호스트 하는 데 사용 되는 클래스입니다.
추가 **CocosSharp.Forms** NuGet 및 **CocosSharp** 도 자동으로 추가 되어야 합니다.
이렇게 하려면 마우스 오른쪽 단추로 클릭는 <span class="UIItem">패키지</span> 고 표준.NET 라이브러리 프로젝트에서 폴더 <span class="UIItem">패키지 추가 중... </span>. 검색어를 입력 <span class="UIItem">CocosSharp.Forms</span>선택, <span class="UIItem">CocosSharp Xamarin.Forms에 대해</span>, 클릭 <span class="UIItem">패키지 추가</span>합니다.

![](cocossharp-images/image1.png "추가 패키지 대화 상자")

둘 다 **CocosSharp** 및 **CocosSharp.Forms** NuGet 패키지를 프로젝트에 추가 됩니다.

![](cocossharp-images/image2.png "패키지 폴더")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대 한 위의 단계를 반복 합니다.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>연습: CocosSharp Xamarin.Forms 앱 추가

Xamarin.Forms 응용 프로그램에는 간단한 CocosSharp 뷰를 추가 하려면 다음이 단계를 수행 합니다.

1. [페이지를 형성 하는 Xamarin 만들기](#1)
1. [CocosSharpView 추가](#2)
1. [GameScene 만들기](#3)
1. [원 추가](#4)
1. [CocosSharp와 상호 작용](#5)

Xamarin.Forms 응용 프로그램에 CocosSharp 뷰를 성공적으로 추가한 후 방문는 [CocosSharp 설명서](~/graphics-games/cocossharp/index.md) CocosSharp를 사용 하 여 콘텐츠를 만들기에 대 한 자세한 내용을 보려면 합니다.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. 페이지를 형성 하는 Xamarin 만들기

CocosSharp는 Xamarin.Forms는 모든 컨테이너에서 호스팅할 수 있습니다. 이 페이지에 대 한이 샘플에서는 라고 하는 페이지가 `HomePage`합니다. `HomePage` 반으로 분할 한 `Grid` 어떻게 Xamarin.Forms 및 CocosSharp 렌더링할 수 있습니다 동시에 같은 페이지에 표시 합니다.

먼저, 포함 하도록 페이지를 설정는 `Grid` 와 두 개의 `Button` 인스턴스:


```csharp
public class HomePage : ContentPage
{
public HomePage ()
    {
        // This is the top-level grid, which will split our page in half
        var grid = new Grid ();
        this.Content = grid;
        grid.RowDefinitions = new RowDefinitionCollection {
            // Each half will be the same size:
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
            new RowDefinition{ Height = new GridLength(1, GridUnitType.Star)},
        };
        CreateTopHalf (grid);
        CreateBottomHalf (grid);
    }
    void CreateTopHalf(Grid grid)
    {
        // We'll be adding our CocosSharpView here:
    }
    void CreateBottomHalf(Grid grid)
    {
        // We'll use a StackLayout to organize our buttons
        var stackLayout = new StackLayout();
        // The first button will move the circle to the left when it is clicked:
        var moveLeftButton = new Button {
            Text = "Move Circle Left"
        };
        stackLayout.Children.Add (moveLeftButton);

        // The second button will move the circle to the right when clicked:
        var moveCircleRight = new Button {
            Text = "Move Circle Right"
        };
        stackLayout.Children.Add (moveCircleRight);
        // The stack layout will be in the bottom half (row 1):

        grid.Children.Add (stackLayout, 0, 1);
    }
}
```

IOS의 경우에 `HomePage` 다음 그림에 나와 있는 것 처럼 나타납니다.

![](cocossharp-images/image3.png "홈 페이지의 스크린 샷")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. CocosSharpView 추가

`CocosSharpView` 클래스 CocosSharp Xamarin.Forms 응용 프로그램에 포함 하는 데 사용 됩니다. 이후 `CocosSharpView` 에서 상속 되는 [Xamarin.Forms.View](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 클래스 레이아웃에 대 한 친숙 한 인터페이스를 제공 및와 같은 레이아웃 컨테이너 내에서 사용할 수 있습니다 [Xamarin.Forms.Grid](https://developer.xamarin.com/api/type/Xamarin.Forms.Grid/)합니다. 새로 추가 `CocosSharpView` 를 완료 하 여 프로젝트에는 `CreateTopHalf` 메서드:


```csharp
void CreateTopHalf(Grid grid)
{
    // This hosts our game view.
    var gameView = new CocosSharpView () {
        // Notice it has the same properties as other XamarinForms Views
        HorizontalOptions = LayoutOptions.FillAndExpand,
        VerticalOptions = LayoutOptions.FillAndExpand,
        // This gets called after CocosSharp starts up:
        ViewCreated = HandleViewCreated
    };
    // We'll add it to the top half (row 0)
    grid.Children.Add (gameView, 0, 0);
}
```

있으므로 시기에 대 한 이벤트를 등록, CocosSharp 초기화 즉시 적용 되지 않습니다는 `CocosSharpView` 생성을 마쳤습니다. 이 작업에서 수행 된 `HandleViewCreated` 메서드:


```csharp
void HandleViewCreated (object sender, EventArgs e)
{
    var gameView = sender as CCGameView;
    if (gameView != null)
    {
        // This sets the game "world" resolution to 100x100:
        gameView.DesignResolution = new CCSizeI (100, 100);
        // GameScene is the root of the CocosSharp rendering hierarchy:
        gameScene = new GameScene (gameView);
        // Starts CocosSharp:
        gameView.RunWithScene (gameScene);
    }
}
```

`HandleViewCreated` 메서드는 두 가지 중요 한 세부 정보에서 우리가 보고 될 것입니다. 첫 번째는는 `GameScene` 클래스는 다음 섹션에 생성 됩니다. 응용 프로그램까지 컴파일되지 것입니다에 유의 해야는 `GameScene` 만들어집니다 및 `gameScene` 인스턴스 참조는 확인 됩니다.

두 번째 중요 한 세부 정보는는 `DesignResolution` CocosSharp 개체에 대 한 게임의 표시 영역을 정의 하는 속성입니다. `DesignResolution` 만든 후 속성에 검토 `GameScene`합니다.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene 만들기

`GameScene` CocosSharp의에서 클래스 상속 `CCScene`합니다. `GameScene` CocosSharp 사용 하 여 처리할 경우 첫 번째 지점이입니다. 에 포함 된 코드 `GameScene` 에서는 함수에 모든 CocosSharp 앱 여부 또는 not Xamarin.Forms 프로젝트 내에서 보관 됩니다.

`CCScene` 클래스는 모든 CocosSharp 렌더링의 시각적 루트입니다. 모든 표시 CocosSharp 개체 안에 포함 되어야 합니다는 `CCScene`합니다. 시각적 개체에 추가 해야 보다 구체적으로, `CCLayer` 인스턴스 및 해당 `CCLayer` 인스턴스를 추가 해야 합니다는 `CCScene`합니다.

다음 그래프는 일반적인 CocosSharp 계층 구조를 시각화 하는 데 도움이 됩니다.

![](cocossharp-images/image4.png "일반적인 CocosSharp 계층 구조")

하나의 `CCScene` 한 번에 활성화 될 수 있습니다. 대부분의 게임 여러 개 사용 `CCLayer` 인스턴스 정렬 콘텐츠 있지만 응용 프로그램을 하나만 사용 합니다. 마찬가지로, 대부분의 게임 여러 시각적 개체를 사용 했지만 앱에서 하나가 합니다. 시각적 계층에 있습니다 CocosSharp에 대 한 토론에 대 한 세부는 [BouncingGame 연습](~/graphics-games/cocossharp/bouncing-game.md)합니다.

처음에 `GameScene` 클래스 거의 비어 있게 됩니다에 대 한 참조를 충족 시키기 위해 방금 만들 것 – `HomePage`합니다. 이라는.NET 표준 라이브러리 프로젝트에 새 클래스를 추가 `GameScene`합니다. 상속 해야는 `CCScene` 클래스를 다음과 같이 합니다.


```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

이제 `GameScene` 은 정의 돌아갈 수 있습니다 `HomePage` 필드 추가 및:


```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

프로젝트 진행을 컴파일하고 실행 CocosSharp 알아 보려고 실행 이제 우리 합니다. 에 아무 것도 추가 하지 우리의 `GameScene,` 가격 페이지의 위쪽 절반 검정-CocosSharp 장면의 기본 색 이므로:

![](cocossharp-images/image5.png "빈 GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. 원 추가

응용 프로그램에는 현재 실행 중인 인스턴스는 빈 표시 CocosSharp 엔진의에 `CCScene`합니다. 다음으로 시각적 개체 추가 하겠습니다: 원입니다. `CCDrawNode` 클래스 수 그리는 데 사용할 기 하 도형의 다양 한에 설명 된 대로 [CCDrawNode 가이드에 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md)합니다.

에 원을 추가 우리의 `GameScene` 클래스 하 고 다음 코드에 나와 있는 것 처럼 생성자에서 인스턴스화하여:


```csharp
public class GameScene : CCScene
{
    CCDrawNode circle;
    public GameScene (CCGameView gameView) : base(gameView)
    {
        var layer = new CCLayer ();
        this.AddLayer (layer);
        circle = new CCDrawNode ();
        layer.AddChild (circle);
        circle.DrawCircle (
            // The center to use when drawing the circle,
            // relative to the CCDrawNode:
            new CCPoint (0, 0),
            radius:15,
            color:CCColor4B.White);
        circle.PositionX = 20;
        circle.PositionY = 50;
    }
}
```

응용 프로그램을 지금 실행 CocosSharp 표시 영역 왼쪽에 원을 보여 줍니다.

![](cocossharp-images/image6.png "GameScene 원")


#### <a name="understanding-designresolution"></a>DesignResolution 이해

표시 된 visual CocosSharp 개체가 했으므로 자세히 조사할 수는 `DesignResolution` 속성입니다.

`DesignResolution` 를 배치 하 고 개체를 크기 조정 CocosSharp 영역의 높이 너비를 나타냅니다. 영역의 실제 해상도 측정 됩니다 *픽셀* 동안는 `DesignResolution` 세계에서 측정 *단위*합니다. 다음 다이어그램은 다양 한 부분의 640 x 1136 픽셀 해상도로 iPhone 5에 표시 된 대로 보기의 해상도 보여 줍니다.

![](cocossharp-images/image7.png "iPhone 5 초 디자인 확인")

위의 다이어그램은 검정 텍스트로 화면의 바깥쪽에 픽셀 크기를 표시합니다. 단위는 흰색 텍스트에서 다이어그램의 안쪽에 표시 됩니다. 다음은 위에 표시 된 몇 가지 중요 한 세부 정보입니다.

* CocosSharp 디스플레이의 원점은 왼쪽 아래에 있습니다. X 값을 증가 오른쪽으로 이동 하 고 Y 값을 증가 위로 이동 합니다. 표시는 Y 값은 다르게 반전 다른 2D 레이아웃 엔진에 비해 위치 (0, 0)는 캔버스의 왼쪽 위 합니다.
* CocosSharp의 기본 동작은 해당 보기의 가로 세로 비율 유지 됩니다. 표에서 첫 번째 행 보다 더 넓은 이므로 CocosSharp 채우지 않고 해당 셀의 전체 너비 점선된 흰색 사각형에 의해 표시 된 것 처럼 합니다. 에 설명 된 대로이 동작은 변경할 수 있습니다는 [CocosSharp에 여러 해상도 처리 안내](~/graphics-games/cocossharp/resolutions.md)합니다.
* 이 예제에서는 CocosSharp 표시 영역 너비와 높이 크기에 관계 없이 100 건을 하거나, 장치로의 가로 세로 비율 유지 됩니다. 즉, 코드에는 CocosSharp X = 100 나타냅니다 오른쪽 끝 바인딩된 표시, keeping 레이아웃 모든 장치에서 일관 된 가정할 수 있습니다.


#### <a name="ccdrawnode-details"></a>CCDrawNode 세부 정보

이 간단한 앱을 사용 하 여는 `CCDrawNode` 원을 그리려면 클래스입니다. 이 클래스는 기 하 도형 벡터 기반 렌더링 – Xamarin.Forms에서 누락 된 기능 제공 하므로 비즈니스 앱에 대 한 매우 유용할 수 있습니다. 원형, 이외에 `CCDrawNode` 클래스 사각형, 스플라인, 선 및 다각형을 사용자 지정을 그리는 데 사용할 수 있습니다. `CCDrawNode` 없으므로 사용 하기 쉬운 (예:.png) 이미지 파일의 사용 필요 하지 않습니다. CCDrawNode의 자세한 내용은에서 확인할 수 있습니다는 [CCDrawNode 가이드에 기 하 도형 그리기](~/graphics-games/cocossharp/ccdrawnode.md)합니다.

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp와 상호 작용

CocosSharp 시각적 요소 (예: `CCDrawNode`)에서 상속 하는 `CCNode` 클래스. `CCNode` 부모에 상대적인 개체 위치를 사용할 수 있는 두 개 속성도 제공: `PositionX` 및 `PositionY`합니다. 코드 현재 사용 하 여 이러한 두 속성은 원의 중심 위치를 지정할이 코드 조각에 나와 있는 것 처럼:


```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

해당 부모 레이아웃 컨트롤의 동작에 따라 자동으로 배치 하는 대부분 Xamarin.Forms 보기와 달리 명시적 위치 값에 의해 CocosSharp 개체를 배치를 확인 하는 것이 유용 합니다.

사용자가 10 (하지 픽셀, 원 그립니다 CocosSharp 세계 단위 공간에 있으므로) 단위로 원을 왼쪽 또는 오른쪽으로 이동 하려면 두 단추 중 하나를 클릭 하도록 허용 하는 코드를 추가 합니다. 두 개의 공용 메서드가 만들겠습니다 먼저는 `GameScene` 클래스:


```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

다음으로, 처리기에 두 개의 단추를 추가 하겠습니다 `HomePage` 클릭에 응답 하도록 합니다. 완료 되 면 우리의 `CreateBottomHalf` 메서드에 다음 코드를 포함 합니다.


```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

이제 CocosSharp 원을 클릭에 대 한 응답으로 이동합니다. 또한 명확 하 게 보면 CocosSharp 캔버스의 경계 원을 충분히 멀리 왼쪽 이나 오른쪽으로 이동 하 여:

![](cocossharp-images/image8.png "원 이동 있는 GameScene")

## <a name="summary"></a>요약

이 가이드에서는 기존 xamarin.forms CocosSharp를 추가 하는 방법을 보여 줍니다. 프로젝트를 Xamarin.Forms와 CocosSharp, 간의 상호 작용을 만드는 방법 및 CocosSharp에서 레이아웃을 만들 때 다양 한 고려 사항에 설명 합니다.

이 가이드만 개략적으로 CocosSharp 수행할 수 있도록 CocosSharp 게임 엔진 많은 기능 및 깊이 제공 합니다. CocosSharp에 대 한 추가 정보에 관심이 있는 개발자에 많은 문서를 찾을 수는 [CocosSharp 섹션](~/graphics-games/cocossharp/index.md)합니다.



## <a name="related-links"></a>관련 링크

- [CocosSharp Api](https://developer.xamarin.com/api/root/CocosSharp/)
- [CocosSharpForms (샘플)](https://developer.xamarin.com/samples/xamarin-forms/CocosSharpForms/)
