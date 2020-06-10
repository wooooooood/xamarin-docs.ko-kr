---
제목: "CocosSharp in Xamarin.Forms " description: "CocosSharp를 사용 하 여 고급 시각화를 위해 응용 프로그램에 정확한 모양, 이미지 및 텍스트 렌더링을 추가할 수 있습니다. prod: assetid: E0F404D5-5C6B-4288-92EC-78996C674E4E:: xamarin: davidbritch author:: dabritch: 05/03/2016: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="using-cocossharp-in-xamarinforms"></a>CocosSharp 사용Xamarin.Forms

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)

_CocosSharp를 사용 하 여 고급 시각화를 위해 정확한 셰이프, 이미지 및 텍스트 렌더링을 응용 프로그램에 추가할 수 있습니다._

> [!VIDEO https://youtube.com/embed/eYCx63FeqVU]

**진화 2016: 코코스Xamarin.Forms**

## <a name="overview"></a>개요

CocosSharp는 그래픽을 표시 하 고, 터치식 입력을 읽고, 오디오를 재생 하 고, 콘텐츠를 관리 하기 위한 유연 하 고 강력한 기술입니다. 이 가이드에서는 응용 프로그램에 CocosSharp를 추가 하는 방법을 설명 합니다 Xamarin.Forms .

## <a name="what-is-cocossharp"></a>CocosSharp 이란?

[Cocossharp](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) 는 Xamarin 플랫폼에서 사용할 수 있는 오픈 소스 게임 엔진입니다.
CocosSharp는 다음과 같은 기능을 포함 하는 런타임 효율성 라이브러리입니다.

- 클래스를 사용 하 여 이미지 렌더링 `CCSprite`
- 클래스를 사용 하 여 셰이프 렌더링 `CCDrawNode`
- 클래스를 사용 하는 모든 프레임 논리 `CCNode.Schedule`
- 콘텐츠 관리 (. .png 파일 등의 리소스 로드 및 언로드)를 사용 하 여`CCTextureCache`
- 클래스를 사용 하는 애니메이션 `CCAction`

CocosSharp의 기본 초점은 플랫폼 간 2D 게임 만들기를 간소화 하는 것입니다. 그러나 Xamarin 양식 응용 프로그램에도 큰 도움이 될 수 있습니다. 게임은 일반적으로 시각적 개체를 효율적으로 렌더링 하 고 정확 하 게 제어 해야 하므로 CocosSharp를 사용 하 여 게임 이외의 응용 프로그램에 강력한 시각화 및 효과를 추가할 수 있습니다.

Xamarin.Forms는 플랫폼별 UI 시스템을 기반으로 빌드됩니다. 예를 들어 [ `Button` s](xref:Xamarin.Forms.Button) 는 iOS 및 Android에 다르게 표시 되며 운영 체제 버전에 따라 달라질 수 있습니다. 이와 대조적으로 CocosSharp에서는 플랫폼별 시각적 개체를 사용 하지 않으므로 모든 시각적 개체가 모든 플랫폼에서 동일 하 게 표시 됩니다. 물론 해상도와 가로 세로 비율이 장치 마다 다르며,이로 인해 CocosSharp의 시각적 개체가 렌더링 되는 방식에 영향을 줄 수 있습니다. 이러한 세부 정보는이 가이드의 뒷부분에서 설명 합니다.

자세한 내용은 [Cocossharp 섹션](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md)에서 찾을 수 있습니다.

## <a name="adding-the-cocossharp-nuget-packages"></a>CocosSharp NuGet 패키지 추가

CocosSharp를 사용 하기 전에 개발자는 프로젝트에 몇 가지를 추가 해야 Xamarin.Forms 합니다.
이 가이드에서는 Xamarin.Forms 프로젝트가 iOS, Android 및 .NET Standard library 프로젝트를 사용 한다고 가정 합니다.
모든 코드는 .NET Standard library 프로젝트에 작성 됩니다. 그러나 라이브러리를 iOS 및 Android 프로젝트에 추가 해야 합니다.

CocosSharp NuGet 패키지에는 CocosSharp 개체를 만드는 데 필요한 모든 개체가 포함 되어 있습니다.
CocosSharp. Forms NuGet 패키지는 `CocosSharpView` 에서 CocosSharp를 호스트 하는 데 사용 되는 클래스를 포함 합니다 Xamarin.Forms .
**Cocossharp** 를 추가 합니다. Forms NuGet 및 **cocossharp** 도 자동으로 추가 됩니다.
이렇게 하려면 .NET Standard library 프로젝트에서 **패키지** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **패키지 추가**...를 선택 합니다. 검색 용어 **Cocossharp**를 입력 하 고 ** Xamarin.Forms Cocossharp **를 선택한 다음 **패키지 추가**를 클릭 합니다.

![](cocossharp-images/image1.png "Add Packages Dialog")

**Cocossharp** 및 **Cocossharp. Forms** NuGet 패키지는 프로젝트에 추가 됩니다.

![](cocossharp-images/image2.png "Packages Folder")

플랫폼별 프로젝트 (예: iOS 및 Android)에 대해 위의 단계를 반복 합니다.

## <a name="walkthrough-adding-cocossharp-to-a-xamarinforms-app"></a>연습: 앱에 CocosSharp 추가 Xamarin.Forms

앱에 간단한 CocosSharp 뷰를 추가 하려면 다음 단계를 수행 합니다 Xamarin.Forms .

1. [Xamarin Forms 페이지 만들기](#1-creating-a-xamarin-forms-page)
1. [CocosSharpView 추가](#2-adding-a-cocossharpview)
1. [GameScene 만들기](#3-creating-the-gamescene)
1. [원 추가](#4-adding-a-circle)
1. [CocosSharp와 상호 작용](#5-interacting-with-cocossharp)

앱에 CocosSharp 뷰를 성공적으로 추가한 후 cocossharp Xamarin.Forms [설명서](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/index.md) 를 방문 하 여 cocossharp로 콘텐츠를 만드는 방법에 대해 자세히 알아보세요.

### <a name="1-creating-a-xamarin-forms-page"></a>1. Xamarin Forms 페이지 만들기

CocosSharp는 모든 컨테이너에서 호스팅될 수 있습니다 Xamarin.Forms . 이 페이지에 대 한이 샘플에서는 라는 페이지를 사용 `HomePage` 합니다. `HomePage`는 `Grid` Xamarin.Forms 와 CocosSharp이 동일한 페이지에서 동시에 렌더링 되는 방법을 보여 주기 위해 a로 분할 됩니다.

먼저 `Grid` 및 두 개의 인스턴스를 포함 하도록 페이지를 설정 합니다 `Button` .

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

IOS에서는 `HomePage` 다음 이미지와 같이 표시 됩니다.

![](cocossharp-images/image3.png "HomePage Screenshot")

### <a name="2-adding-a-cocossharpview"></a>2. CocosSharpView 추가

`CocosSharpView`클래스는 앱에 CocosSharp를 포함 하는 데 사용 됩니다 Xamarin.Forms . `CocosSharpView`는에서 상속 [ Xamarin.Forms 됩니다. 뷰](xref:Xamarin.Forms.View) 클래스는 레이아웃에 대 한 친숙 한 인터페이스를 제공 하며와 같은 레이아웃 컨테이너 내에서 사용할 수 있습니다 [ Xamarin.Forms . 표](xref:Xamarin.Forms.Grid). `CocosSharpView`메서드를 완료 하 여 프로젝트에 새를 추가 합니다 `CreateTopHalf` .

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

CocosSharp 초기화는 즉시 실행 되지 않으므로에서 만들기를 완료 한 경우에 대 한 이벤트를 등록 `CocosSharpView` 합니다. 메서드에서이 작업을 수행 합니다 `HandleViewCreated` .

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

`HandleViewCreated`이 메서드에는 두 가지 중요 한 세부 정보가 있습니다. 첫 번째는 `GameScene` 다음 섹션에서 생성 되는 클래스입니다. `GameScene`가 만들어지고 인스턴스 참조가 해결 될 때까지 앱이 컴파일되지 않습니다 `gameScene` .

두 번째 중요 한 세부 정보는 `DesignResolution` CocosSharp 개체에 대 한 게임의 표시 영역을 정의 하는 속성입니다. 속성은를 `DesignResolution` 만든 후에 조회 됩니다 `GameScene` .

### <a name="3-creating-the-gamescene"></a>3. GameScene 만들기

`GameScene`클래스는 CocosSharp에서 상속 `CCScene` 됩니다. `GameScene`는 CocosSharp만 처리 하는 첫 번째 지점입니다. 에 포함 된 코드는 `GameScene` 프로젝트 내에 있는지 여부에 관계 없이 모든 CocosSharp 앱에서 작동 Xamarin.Forms 합니다.

`CCScene`클래스는 모든 CocosSharp 렌더링의 시각적 루트입니다. 표시 되는 CocosSharp 개체는 내에 포함 되어야 합니다 `CCScene` . 좀 더 구체적으로 말해서, 시각적 개체를 `CCLayer` 인스턴스에 추가 하 고 해당 인스턴스를에 `CCLayer` 추가 해야 합니다 `CCScene` .

다음 그래프는 일반적인 CocosSharp 계층을 시각화 하는 데 도움이 될 수 있습니다.

![](cocossharp-images/image4.png "Typical CocosSharp Hierarchy")

한 번 `CCScene` 에 하나만 활성 상태일 수 있습니다. 대부분의 게임에서는 여러 `CCLayer` 인스턴스를 사용 하 여 콘텐츠를 정렬 하지만 응용 프로그램은 하나만 사용 합니다. 마찬가지로 대부분의 게임에서는 여러 시각적 개체를 사용 하지만 앱에는 하나만 있습니다. CocosSharp 시각적 계층 구조에 대 한 자세한 내용은 [BouncingGame 연습](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/bouncing-game.md)을 참조 하세요.

처음에는 `GameScene` 클래스가 거의 비어 있습니다 .에서 참조를 충족 하는 것만 만들면 됩니다 `HomePage` . 이라는 .NET Standard 라이브러리 프로젝트에 새 클래스를 추가 `GameScene` 합니다. 다음과 같이 클래스에서 상속 해야 합니다 `CCScene` .

```csharp
public class GameScene : CCScene
{
    public GameScene (CCGameView gameView) : base(gameView)
    {

    }
}
```

이제 `GameScene` 가 정의 되었으므로로 돌아가서 `HomePage` 필드를 추가할 수 있습니다.

```csharp
// Keep the GameScene at class scope
// so the button click events can access it:
GameScene gameScene;
```

이제 프로젝트를 컴파일하고 실행 하 여 CocosSharp 실행을 확인할 수 있습니다. 에 아무것도 추가 하지 `GameScene,` 않았으므로 페이지의 위쪽 절반이 검정색 이며 CocosSharp 장면의 기본 색입니다.

![](cocossharp-images/image5.png "Blank GameScene")

### <a name="4-adding-a-circle"></a>4. 원 추가

현재 앱에는 현재 실행 중인 CocosSharp 엔진 인스턴스가 있으며, 빈를 표시 `CCScene` 합니다. 다음에는 시각적 개체를 추가 합니다. 원. `CCDrawNode`이 클래스를 사용 하 여 [CCDrawNode를 사용 하 여 그리기 Geometry 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)에 설명 된 대로 다양 한 기 하 도형을 그릴 수 있습니다.

다음 코드와 같이 클래스에 원을 추가 `GameScene` 하 고 생성자에서 인스턴스화합니다.

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

이제 앱을 실행 하면 CocosSharp 표시 영역의 왼쪽에 원이 표시 됩니다.

![](cocossharp-images/image6.png "Circle in GameScene")

#### <a name="understanding-designresolution"></a>DesignResolution 이해

이제 visual CocosSharp 개체가 표시 되었으므로 속성을 조사할 수 있습니다 `DesignResolution` .

는 `DesignResolution` 개체를 배치 하 고 크기를 조정 하기 위한 CocosSharp 영역의 너비와 높이를 나타냅니다. 영역에 대 한 실제 해상도는 *픽셀* 단위로 측정 되는 반면는 `DesignResolution` 세계 *단위로*측정 됩니다. 다음 다이어그램은 iPhone 5에 표시 된 것과 같이 화면 해상도 640x1136 픽셀에 표시 되는 보기의 다양 한 부분에 대 한 해결 방법을 보여 줍니다.

![](cocossharp-images/image7.png "iPhone 5s Design Resolution")

위의 다이어그램에는 화면 외부의 픽셀 치수가 검은색 텍스트로 표시 됩니다. 단위는 다이어그램의 안쪽에 흰색 텍스트로 표시 됩니다. 위에 표시 된 몇 가지 중요 한 세부 정보는 다음과 같습니다.

- CocosSharp 표시의 원점은 왼쪽 아래에 있습니다. 오른쪽으로 이동 하면 X 값이 늘어나고 위로 이동 하면 Y 값이 늘어납니다. Y 값은 다른 일부 2D 레이아웃 엔진과 비교할 때 반전 됩니다. 여기서 (0, 0)은 캔버스의 왼쪽 위입니다.
- CocosSharp의 기본 동작은 뷰의 가로 세로 비율을 유지 하는 것입니다. 표의 첫 번째 행이 너비 보다 크기 때문에, CocosSharp는 점선 흰색 사각형에 표시 된 것 처럼 셀의 전체 너비를 채우지 않습니다. 이 동작은 [CocosSharp에서 여러 해결 방법 처리 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/resolutions.md)에 설명 된 대로 변경할 수 있습니다.
- 이 예제에서 CocosSharp는 장치의 크기 또는 가로 세로 비율에 관계 없이 100 단위의 표시 영역을 유지 합니다. 즉, 코드에서 X = 100이 CocosSharp 디스플레이의 오른쪽 끝에 있는 것으로 가정 하 여 모든 장치에서 레이아웃을 일관성 있게 유지할 수 있습니다.

#### <a name="ccdrawnode-details"></a>CCDrawNode 세부 정보

간단한 앱은 클래스를 사용 하 여 `CCDrawNode` 원을 그립니다. 이 클래스는 벡터 기반 기 하 도형 렌더링 (에서 누락 된 기능)을 제공 하므로 비즈니스 앱에 매우 유용할 수 있습니다 Xamarin.Forms . 원을 사용 하는 것 외에도 `CCDrawNode` 클래스를 사용 하 여 사각형, 스플라인, 선 및 사용자 지정 다각형을 그릴 수 있습니다. `CCDrawNode`는 이미지 파일 (예: .png)을 사용할 필요가 없기 때문에 쉽게 사용할 수 있습니다. CCDrawNode 대 한 자세한 내용은 [CCDrawNode를 사용 하는 그리기 Geometry 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/ccdrawnode.md)에서 찾을 수 있습니다.

### <a name="5-interacting-with-cocossharp"></a>5. CocosSharp와 상호 작용

와 같은 CocosSharp 시각적 요소 `CCDrawNode` 는 클래스에서 상속 `CCNode` 합니다. `CCNode`는 부모를 기준으로 개체를 배치 하는 데 사용할 수 있는 두 가지 속성인 및를 제공 `PositionX` `PositionY` 합니다. 코드는 현재이 두 가지 속성을 사용 하 여 다음 코드 조각과 같이 원의 중심을 배치 합니다.

```csharp
circle.PositionX = 20;
circle.PositionY = 50;
```

CocosSharp 개체는 대부분의 Xamarin.Forms 뷰가 아니라 부모 레이아웃 컨트롤의 동작에 따라 자동으로 배치 되는 명시적 위치 값으로 배치 된다는 점에 유의 해야 합니다.

사용자가 두 단추 중 하나를 클릭 하 여 원을 왼쪽 또는 오른쪽으로 이동할 수 있도록 하는 코드를 추가 합니다 (원이 CocosSharp 세계 단위 공간에서 그리기 때문에 픽셀 아님). 먼저 클래스에 두 개의 공용 메서드를 만듭니다 `GameScene` .

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

다음으로의 두 단추에 처리기를 추가 하 여 `HomePage` 클릭에 응답 합니다. 완료 되 면 `CreateBottomHalf` 메서드는 다음 코드를 포함 합니다.

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

이제 CocosSharp 원이 클릭에 대 한 응답으로 이동 합니다. 또한 왼쪽 또는 오른쪽으로 원을 이동 하 여 CocosSharp 캔버스의 경계를 명확 하 게 볼 수 있습니다.

![](cocossharp-images/image8.png "GameScene with Moving Circle")

## <a name="summary"></a>요약

이 가이드에서는 기존 프로젝트에 CocosSharp를 추가 하는 방법 Xamarin.Forms ,와 cocossharp 간의 상호 작용을 만드는 방법 Xamarin.Forms 및 cocossharp에서 레이아웃을 만들 때 다양 한 고려 사항에 대해 설명 합니다.

CocosSharp 게임 엔진은 많은 기능과 깊이를 제공 하므로이 가이드는 CocosSharp에서 수행할 수 있는 작업에 대 한 정보를 제공 합니다. CocosSharp에 대해 자세히 알고 싶은 개발자는 [cocossharp archive](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/)에서 많은 문서를 찾을 수 있습니다.

## <a name="related-links"></a>관련 링크

- [CocosSharpForms (샘플)](https://github.com/xamarin/xamarin-forms-samples/tree/master/CocosSharpForms)
