---
title: Xamarin.Forms에서 CocosSharp 사용
description: CocosSharp는 사용 하 여 고급 시각화에 대 한 응용 프로그램에 정확한 도형, 이미지 및 텍스트 렌더링을 추가할 수 있습니다.
ms.prod: xamarin
ms.assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/03/2016
ms.openlocfilehash: 7b465391958a6e862bfed9fde8d9da1fdd52bee5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70759765"
---
# <a name="using-cocossharp-in-xamarinforms"></a>Xamarin.Forms에서 CocosSharp 사용

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](h https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp는 사용 하 여 고급 시각화에 대 한 응용 프로그램에 정확한 도형, 이미지 및 텍스트 렌더링을 추가할 수 있습니다._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**Evolve 2016: Xamarin.ios**

## <a name="overview"></a>개요

CocosSharp는 그래픽 표시, 터치 입력을 읽고, 오디오 및 관리 콘텐츠를 재생 하기 위한 유연 하 고 강력한 기술 됩니다. 이 가이드에서는 CocosSharp Xamarin.Forms 응용 프로그램을 추가 하는 방법을 설명 합니다. 다음 내용을 다룹니다.

- [CocosSharp 란?](#what)
- [CocosSharp Nuget 패키지 추가](#nuget)
- [연습: Xamarin. Forms 앱에 CocosSharp 추가](#add)

<a name="what" />

## <a name="what-is-cocossharp"></a>CocosSharp 란?

[CocosSharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) 는 Xamarin 플랫폼에서 사용할 수 있는 오픈 소스 게임 엔진입니다.
CocosSharp는 다음과 같은 기능을 포함 하는 효율적인 런타임 라이브러리:

- 클래스를 `CCSprite` 사용 하 여 이미지 렌더링
- 클래스를 `CCDrawNode` 사용 하 여 셰이프 렌더링
- 클래스를 사용 하는 `CCNode.Schedule` 모든 프레임 논리
- 콘텐츠 관리 (. .png 파일 등의 리소스 로드 및 언로드)를 사용 하 여`CCTextureCache`
- 클래스를 사용 `CCAction` 하는 애니메이션

CocosSharp의 주된 초점은 플랫폼 간 2D 게임;의 생성을 간소화 하는 그러나 Xamarin 양식 응용 프로그램에 크게 보강할 수도 있습니다. 게임에는 일반적으로 효율적인 렌더링 및 시각적 개체를 정확 하 게 제어할 필요, CocosSharp 게임 내 응용 프로그램에 강력한 시각화 및 효과 추가 하려면 사용할 수 있습니다.

Xamarin.Forms는 네이티브 플랫폼 특정 UI 시스템을 기반으로 합니다. 예를 들어 [ `Button`s](xref:Xamarin.Forms.Button) iOS 및 Android에서 다르게 표시 되 고 운영 체제 버전에도 다를 수 있습니다. 반면 CocosSharp 사용 하지 않으므로 모든 플랫폼별 시각적 개체를 모든 시각적 개체는 모든 플랫폼에서 동일 하 게 나타납니다. 물론 장치 간에 다를 확인 및 가로 세로 비율 및 CocosSharp 해당 시각적 개체를 렌더링 하는 방법에 영향을이 수 있습니다. 이 가이드의 뒷부분에서 이러한 세부 정보를 설명 합니다.

자세한 정보를 찾을 수 있습니다 합니다 [CocosSharp 섹션](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)합니다.

<a name="nuget" />

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp Nuget 패키지 추가

CocosSharp를 사용 하기 전에 개발자는 Xamarin.Forms 프로젝트에 대 한 몇 가지 추가 확인 해야 합니다.
이 가이드에서는 iOS, Android 및.NET Standard를 사용 하 여 Xamarin.Forms 프로젝트 가정 라이브러리 프로젝트.
코드의 모든.NET Standard 라이브러리 프로젝트에 기록 됩니다. 그러나 라이브러리는 iOS 및 Android 프로젝트에 추가 되어야 합니다.

CocosSharp Nuget 패키지는 모든 CocosSharp 개체를 만드는 데 필요한 개체를 포함 합니다.
CocosSharp.Forms nuget 패키지에 포함 된 `CocosSharpView` Xamarin.Forms에서 CocosSharp 호스팅하는 데 사용 하는 클래스입니다.
추가 된 **CocosSharp.Forms** NuGet 및 **CocosSharp** 도 자동으로 추가 될 예정입니다.
이렇게 하려면 마우스 오른쪽 단추로 클릭 합니다 **패키지** 선택한.NET Standard 라이브러리 프로젝트 폴더 **패키지 추가...** . 검색 용어를 입력 **CocosSharp.Forms**를 선택 **Xamarin.Forms 용 CocosSharp**, 클릭 **패키지 추가**합니다.

![](cocossharp-images/image1.png "추가 패키지 대화 상자")

둘 다 **CocosSharp** 하 고 **CocosSharp.Forms** NuGet 패키지를 프로젝트에 추가 됩니다.

![](cocossharp-images/image2.png "패키지 폴더")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대해 위의 단계를 반복 합니다.

<a name="add" />

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>연습: Xamarin. Forms 앱에 CocosSharp 추가

Xamarin.Forms 앱에 간단한 CocosSharp 뷰를 추가 하려면 다음이 단계를 수행 합니다.

1. [만들기는 Xamarin Forms 페이지](#1)
1. [CocosSharpView 추가](#2)
1. [GameScene 만들기](#3)
1. [원 추가](#4)
1. [CocosSharp 상호 작용](#5)

Xamarin.Forms 앱에 CocosSharp 뷰를 성공적으로 추가한 후 방문 합니다 [CocosSharp 설명서](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) CocosSharp로 콘텐츠를 만들기에 대해 자세히 알아보려면 합니다.

<a name="1" />

### <a name="1-creating-a-xamarin-forms-page"></a>1. 만들기는 Xamarin Forms 페이지

Xamarin.Forms는 모든 컨테이너에서 CocosSharp는 호스팅할 수 있습니다. 이 페이지에 대 한이 샘플에서는 라는 페이지 `HomePage`합니다. `HomePage` 절반으로 분할 됩니다는 `Grid` Xamarin.Forms 및 CocosSharp 수 렌더링 되는 방식을 동시에 동일한 페이지에 표시 합니다.

포함 하도록 페이지를 먼저 설정 된 `Grid` 두 개의 `Button` 인스턴스:

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

Ios의 경우는 `HomePage` 다음 이미지와 같이 표시 됩니다.

![](cocossharp-images/image3.png "홈 페이지 스크린샷")

<a name="2" />

### <a name="2-adding-a-cocossharpview"></a>2. CocosSharpView 추가

`CocosSharpView` 클래스 CocosSharp Xamarin.Forms 앱에 포함 하는 데 사용 됩니다. 이후 `CocosSharpView` 에서 상속 되는 [Xamarin.Forms.View](xref:Xamarin.Forms.View) 클래스 레이아웃에 대 한 친숙 한 인터페이스를 제공 하 고 같은 레이아웃 컨테이너 내에서 사용할 수 있습니다 [Xamarin.Forms.Grid](xref:Xamarin.Forms.Grid)합니다. 새 `CocosSharpView` 를 완료 하 여 프로젝트에는 `CreateTopHalf` 메서드:

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

CocosSharp 초기화가 즉시 없으므로 시기에 대 한 이벤트를 등록 합니다 `CocosSharpView` 생성을 완료 합니다. 이 작업을 수행 합니다 `HandleViewCreated` 메서드:

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

`HandleViewCreated` 메서드가에서 기대 하에서는 두 가지 중요 한 세부 정보입니다. 첫 번째는 `GameScene` 클래스를 다음 섹션에 생성 됩니다. 것에 앱이 될 때까지 컴파일되지 것입니다 유의 해야 합니다 `GameScene` 만들어집니다 및 `gameScene` 인스턴스 참조 해결 됨.

두 번째 중요 한 세부 정보는는 `DesignResolution` CocosSharp 개체에 대 한 게임의 표시 영역을 정의 하는 속성입니다. 합니다 `DesignResolution` 만든 후 속성에서 찾을 `GameScene`합니다.

<a name="3" />

### <a name="3-creating-the-gamescene"></a>3. GameScene 만들기

합니다 `GameScene` CocosSharp의에서 클래스 상속 `CCScene`합니다. `GameScene` CocosSharp를 통한 순수 하 게 처리할 경우 첫 번째 지점이입니다. 에 포함 된 코드 `GameScene` 여부 Xamarin.Forms 프로젝트 내에서 저장 하는지 여부를 CocosSharp 앱에서 작동 합니다.

`CCScene` 클래스는 visual 루트 모든 CocosSharp 렌더링입니다. 표시 되는 CocosSharp 개체에 포함 되어야 합니다는 `CCScene`합니다. 시각적 개체에 추가 해야 더 정확히 말하면 `CCLayer` 인스턴스 및 해당 `CCLayer` 인스턴스를 추가 해야 합니다는 `CCScene`합니다.

다음 그래프는 일반적인 CocosSharp 계층 구조를 시각화 하는 데 도움이 됩니다.

![](cocossharp-images/image4.png "일반적인 CocosSharp 계층")

하나의 `CCScene` 한 번에 활성화 될 수 있습니다. 대부분의 게임 사용 하 여 `CCLayer` 내용 정렬 하지만 응용 프로그램 인스턴스 하나만 사용 합니다. 마찬가지로 대부분의 게임 여러 시각적 개체를 사용 하지만 우리는 앱에만 해야 합니다. 시각적 계층에서 찾을 수 있습니다 CocosSharp에 대 한 논의 자세한 합니다 [BouncingGame 연습](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md)합니다.

처음에 `GameScene` 클래스는 거의 비어 있게 됩니다 – 방금 만들어에 대 한 참조를 충족 시키기 위해 `HomePage`합니다. 새 클래스 라는.NET Standard 라이브러리 프로젝트에 추가할 `GameScene`합니다. 상속 해야 합니다 `CCScene` 다음과 같이 클래스:

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

이제 `GameScene` 는 정의 돌아갈 수 있습니다 `HomePage` 필드를 추가 합니다.

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

이제 프로젝트를 컴파일 및 실행 CocosSharp을 실행 수 했습니다. 에 아무 것도 추가 하지 않은 것이 `GameScene,` 페이지의 위쪽 절반 검정-CocosSharp 장면의 기본 색은:

![](cocossharp-images/image5.png "빈 GameScene")

<a name="4" />

### <a name="4-adding-a-circle"></a>4. 원 추가

앱은 현재 비어 있는 표시 CocosSharp 엔진의 실행 중인 인스턴스에 `CCScene`입니다. 그런 다음 시각적 개체 추가: 원입니다. 합니다 `CCDrawNode` 에 설명 된 대로 다양 한 기 하 도형에 그릴 클래스를 사용할 수 있습니다 합니다 [CCDrawNode 가이드를 사용 하 여 기 하 도형 그리기](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)합니다.

에 원을 추가 우리의 `GameScene` 클래스 및 다음 코드와 같이 생성자에서 인스턴스화할:

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

이제 앱을 실행 하는 원 CocosSharp 표시 영역의 왼쪽에 보여 줍니다.

![](cocossharp-images/image6.png "GameScene 원")

#### <a name="understanding-designresolution"></a>DesignResolution 이해

CocosSharp 시각적 개체 표시 되는 이제 조사할 수 있습니다는 `DesignResolution` 속성입니다.

`DesignResolution` 배치 및 개체 크기 조정에 대 한 CocosSharp 영역의 높이 너비를 나타냅니다. 영역의 실제 해상도 측정 됩니다 *픽셀* 하는 동안 합니다 `DesignResolution` 환경에서 측정 됩니다 *단위*합니다. 다음 다이어그램은 다양 한 부분 640 x 1136 픽셀의 화면 해상도 사용 하 여 iPhone 5에 표시 된 뷰 확인을 보여줍니다.

![](cocossharp-images/image7.png "iPhone 5 초 디자인 확인")

위의 다이어그램 검은색 텍스트로 화면 외부의 픽셀 크기를 표시합니다. 단위는 흰색 텍스트의 다이어그램의 내부에 표시 됩니다. 위에 표시 된 몇 가지 중요 한 세부 정보는 다음과 같습니다.

- CocosSharp 디스플레이의 원점은 왼쪽 맨 아래에 있습니다. X 값 증가 오른쪽으로 이동 하 고 Y 값을 증가 위로 이동 됩니다. 일부 다른 2D 레이아웃 엔진에 비해으로 Y 값을 반전 됩니다 표시 위치 (0, 0) 캔버스의 왼쪽 위입니다.
- CocosSharp의 기본 동작은 해당 보기의 가로 세로 비율을 유지 하는 것입니다. 표의 첫 번째 행 높이 것 보다 더 광범위 한 이므로 CocosSharp 채워지지 해당 셀의 전체 너비 점선된 흰색 사각형으로 표시 된 것과 같이 합니다. 이 동작에 설명 된 대로 변경할 수는 [CocosSharp에서 여러 해상도 처리 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)합니다.
- 이 예제에서는 CocosSharp 표시 영역의 너비와 높이 크기에 관계 없이 100 단위 또는 해당 장치의 가로 세로 비율을 유지 합니다. 즉, 코드는는 CocosSharp의 오른쪽 끝 바인딩된 X = 100 나타냅니다 레이아웃을 표시, 보관 모든 장치에서 일관 된 가정할 수 있습니다.

#### <a name="ccdrawnode-details"></a>CCDrawNode 세부 정보

이 간단한 앱 사용을 `CCDrawNode` 원을 그리려면 클래스입니다. 이 클래스는 기 하 도형 벡터 기반 렌더링 – Xamarin.Forms에서 누락 된 기능을 제공 하므로 비즈니스 앱에 대 한 매우 유용할 수 있습니다. 원 외에도 `CCDrawNode` 를 그릴 사각형, 스플라인, 선 및 다각형을 사용자 지정 클래스를 사용할 수 있습니다. `CCDrawNode` 이미지 파일 (예:.png) 사용 하지 않아도 되므로 사용 하기 쉬운도 합니다. 자세한 설명은 CCDrawNode에서 찾을 수 있습니다 합니다 [CCDrawNode 가이드를 사용 하 여 기 하 도형 그리기](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)합니다.

<a name="5" />

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp 상호 작용

CocosSharp 시각적 요소 (같은 `CCDrawNode`)에서 상속 된 `CCNode` 클래스입니다. `CCNode` 부모를 기준으로 개체를 배치에 사용할 수 있는 두 개 속성도 제공 합니다. `PositionX` 고 `PositionY`입니다. 이 코드 조각에 나와 있는 것 처럼 코드는 원의 중심을 배치 하려면 이러한 두 속성을 사용 하는 현재:

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp 개체는 해당 부모 레이아웃 컨트롤의 동작에 따라 자동으로 배치 되는 대부분의 Xamarin.Forms 뷰 달리 명시적 위치 값을 배치 됩니다 하는 것이 반드시 합니다.

사용자가 10 (픽셀이 아닌, 원을 그립니다 CocosSharp world 단위 공간에 있으므로) 단위로 왼쪽 또는 오른쪽에 원의 이동 하려면 두 개의 단추 중 하나를 클릭 하도록 허용 하는 코드를 추가 하겠습니다. 처음에 두 개의 public 메서드가 만듭니다는 `GameScene` 클래스:

```csharp
public void MoveCircleLeft()
{
    circle.PositionX -= 10;
}

public void MoveCircleRight()
{
    circle.PositionX += 10;
}
```

그런 다음 처리기 두 개의 단추를 추가 `HomePage` 클릭에 응답 합니다. 완료 되 면이 `CreateBottomHalf` 메서드에 다음 코드를 포함 합니다.

```csharp
void CreateBottomHalf(Grid grid)
{
    // We'll use a StackLayout to organize our buttons
    var stackLayout = new StackLayout();

    // The first button will move the circle to the left when it is clicked:
    var moveLeftButton = new Button {
        Text = "Move Circle Left"
    };
    moveLeftButton.Clicked += (sender, e) => gameScene.MoveCircleLeft ();
    stackLayout.Children.Add (moveLeftButton);

    // The second button will move the circle to the right when clicked:
    var moveCircleRight = new Button {
        Text = "Move Circle Right"
    };
    moveCircleRight.Clicked += (sender, e) => gameScene.MoveCircleRight ();
    stackLayout.Children.Add (moveCircleRight);

    // The stack layout will be in the bottom half (row 1):
    grid.Children.Add (stackLayout, 0, 1);
}
```

이제 CocosSharp 원을 클릭에 대 한 응답으로 이동합니다. 또한 명확 하 게 CocosSharp 캔버스의 경계 원을 충분히 왼쪽 이나 오른쪽으로 이동 하 여 볼 수 있습니다.

![](cocossharp-images/image8.png "사용 하 여 원을 이동 GameScene")

## <a name="summary"></a>요약

이 가이드는 기존 Xamarin.Forms에서 CocosSharp 추가할 방법을 보여 줍니다. 프로젝트를 Xamarin.Forms에서 CocosSharp, 사이의 상호 작용을 만드는 방법 및 CocosSharp에서 레이아웃을 만들 때 다양 한 고려 사항에 설명 합니다.

CocosSharp 게임 엔진은이 가이드만 개략적으로 CocosSharp 수행할 수 있도록 많은 기능 및 깊이 제공 합니다. CocosSharp에 대해 자세히 알고 싶은 개발자는 [cocossharp archive](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/)에서 많은 문서를 찾을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CocosSharpForms (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
