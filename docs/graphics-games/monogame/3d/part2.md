---
title: 3D 그래픽 꼭지점을 MonoGame에 그리기
description: MonoGame 3D 개체 당 포인트 단위로 렌더링 되는 방법을 정의 하 꼭 짓 점의 배열을 사용할 수 있도록 지원 합니다. 꼭 짓 점 배열 동적 기 하 도형 만들기 특수 효과 구현 하 고 컬링을 통해 렌더링의 효율성을 개선에 활용을 취할 수 있습니다.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 4736bedd413663af098bbad522cc56f432e36ea0
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/18/2018
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>3D 그래픽 꼭지점을 MonoGame에 그리기

_MonoGame 3D 개체 당 포인트 단위로 렌더링 되는 방법을 정의 하 꼭 짓 점의 배열을 사용할 수 있도록 지원 합니다. 꼭 짓 점 배열 동적 기 하 도형 만들기 특수 효과 구현 하 고 컬링을 통해 렌더링의 효율성을 개선에 활용을 취할 수 있습니다._

사용자가을 통해 읽기는 [가이드에서 모델을 렌더링에](~/graphics-games/monogame/3d/part1.md) 익숙할 것 MonoGame에서 3D 모델을 렌더링 합니다. `Model` 클래스는 데이터 (예:.fbx) 파일에 정의 된 작업 및 정적 데이터를 처리할 때 3D 그래픽을 렌더링 하는 효과적인 방법입니다. 일부 게임 정의 하거나 런타임에 동적으로 조작 필요로 합니다. 이러한 경우 배열을 사용할 수 *꼭지점* 정의 하 고 기 하 도형을 렌더링할 합니다. 꼭 짓 점은 기 하 도형을 정의 하는 데 정렬 된 목록에 포함 되어 있는 3D 공간에서 지점에 대 한 일반 용어입니다. 일반적으로 꼭지점 일련의 삼각형을 정의 하는 방식에 정렬 됩니다.

꼭 짓 점을 3D 개체를 만드는 데 사용 되는 방식을 시각화 하려면 다음 구를 살펴보겠습니다.

![](part2-images/image1.png "하려면 시각화 꼭지점 3D 개체를 만드는 데 사용 되는 방법을 고려 하십시오이 구")

위와 같이 구의 여러 삼각형 명확 하 게 구성 됩니다. 양식 삼각형에 꼭 짓 점을 연결 하는 방법을 보려면 구의 솔루션의 토대를 볼 수 있습니다.

![](part2-images/image2.png "양식 삼각형에 꼭 짓 점을 연결 하는 방법을 보려면 구의 와이어 프레임을 보려면")

이 연습에서는 다음 항목을 설명 합니다.

- 프로젝트 만들기
- 꼭 짓 점을 만들기
- 그리기 코드를 추가합니다.
- 질감을 사용 하 여 렌더링
- 질감 좌표를 수정합니다.
- 모델과 함께 꼭지점을 렌더링

완료 된 프로젝트에는 꼭 짓 점 배열을 사용 하 여 그려진 바둑판된 층을 포함 됩니다.

![](part2-images/image3.png "완료 된 프로젝트에는 꼭 짓 점 배열을 사용 하 여 그려진 바둑판된 층 포함 됩니다.")

## <a name="creating-a-project"></a>프로젝트 만들기

첫째, 우리의 시작 지점으로 사용할 프로젝트를 다운로드 합니다 했습니다. 모델 프로젝트에서는 [있는 여기](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)합니다.

다운로드 하 고 압축을 푼를 열고 프로젝트를 실행 합니다. 화면에 그릴 되 고 6 개의 로봇 모델 참조를 것으로 예상 합니다.

![](part2-images/image4.png "화면에 그릴 되 고 6 개의 로봇 모델")

이 프로젝트의 끝에서 우리 합니다 수 고유한 사용자 지정 꼭지점 렌더링 함께 사용 하면 로봇 `Model`이므로 로봇 렌더링 코드를 삭제 하지 않겠습니다. 대신 삭제 조금 합니다는 `Game1.Draw` 지금은 드로잉 6 로봇의 제거를 호출 합니다. 이 작업을 수행 하려면 엽니다는 **Game1.cs** 파일를 찾습니다는 `Draw` 메서드. 다음 코드를 포함 하도록 수정 합니다.

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

이렇게 하면 다루는 게임은 빈 파란색 화면 표시:

![](part2-images/image5.png "이 빈 파란색 화면을 표시 하는 게임 결과")

## <a name="creating-the-vertices"></a>꼭 짓 점을 만들기

이 기 하 도형을 정의 하는 꼭지점 배열을 만듭니다. 이 연습에서는 만들게 됩니다를 3D 평면 (3D 공간에서 사각형, 비행기 없습니다). 이 평면 네 면 및 네 모서리에으로 세 개의 꼭 짓 필요 하며 각 하는 두 개의 삼각형 중 구성 됩니다. 따라서 여기서 정의 합니다 총 6 개의 점입니다.

지금까지 논의해 왔습니다 일반적인 의미에서 정점에 대 한 하지만 MonoGame 꼭 짓 점에 대해 사용할 수 있는 몇 가지 표준 구조체를 제공 합니다.

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

각 종류의 이름이 포함 된 구성 요소를 나타냅니다. 예를 들어 `VertexPositionColor` 위치 및 색에 대 한 값을 포함 합니다. 각 구성 요소에 살펴보겠습니다.

- 위치 – 모든 꼭 짓 점 형식에는 한 `Position` 구성 요소입니다. `Position` 값 (X, Y 및 Z) 3D 공간에서의 꼭 짓 점 위치를 정의 합니다.
- 색 – 꼭지점 선택적으로 지정할 수는 `Color` 색조 사용자 지정을 수행 하는 값입니다.
- 보통-법선 개체의 표면을 마주보는 어떤 방식으로 정의 합니다. 정규 분포는 표면 얼마나 밝은 영향 받은 향하도록 조명 방향 이후을 가진 개체를 렌더링 하는 경우 필요 합니다. 정규 분포는 일반적으로로 지정 되는 *단위 벡터* – 1의 길이가 3 차원 벡터입니다.
- 질감 – 질감 질감 좌표 – 즉,: 질감의 부분을 지정 된 꼭 짓 점에 배치를 참조 합니다. 질감 값은 3D 질감으로 개체를 렌더링 하는 경우에 필요 합니다. 질감 좌표는 정규화 된 좌표 값은 0과 1 사이 해당 됩니다. 질감 좌표가이 가이드의 뒷부분에 자세히 설명 합니다.

우리의 평면 층, 역할을 수행 하며이 렌더링을 수행 하는 경우 질감을 적용 하고자는 `VertexPositionTexture` 우리의 꼭지점을 정의 하는 형식입니다.

첫째, 구성원 Game1 클래스를 추가 합니다.

```csharp
VertexPositionTexture[] floorVerts; 
```

다음으로 우리의 정점에 정의 `Game1.Initialize`합니다. 이 문서의 앞부분에서 참조 되는 제공된 템플릿을 포함 하지 않는 통지는 `Game1.Initialize` 전체 메서드를 추가 해야 하므로 `Game1`:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];
    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);
    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // We’ll be assigning texture values later
    base.Initialize ();
}
```

우리의 꼭지점 모양을 시각화 하려면 다음 다이어그램을 고려 합니다.

![](part2-images/image6.png "꼭 짓 점의 모양을 시각화 하려면이 다이어그램을 검토")

렌더링 코드 구현 완료 될 때까지 꼭 짓 점을 시각화 하려면 다이어그램을 사용 해야 합니다.

## <a name="adding-drawing-code"></a>그리기 코드를 추가합니다.

정의 된 우리의 y 위치를 만들었으므로 이제 해당 렌더링 코드를 작성할 수 있습니다.

먼저 정의 해야 합니다는 `BasicEffect` 위치와 같은 렌더링 및 조명 매개 변수를 포함할 인스턴스. 이 작업을 수행 하려면 추가 `BasicEffect` 멤버는 `Game1` where 아래 클래스는 `floorVerts` 필드가 정의:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

다음으로 수정 된 `Initialize` 효과 정의 하는 메서드:

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

이제 그리기를 수행 하는 코드를 추가할 수 있습니다.

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
            // We’ll be rendering two trinalges
            PrimitiveType.TriangleList,
            // The array of verts that we want to render
            floorVerts,
            // The offset, which is 0 since we want to start 
            // at the beginning of the floorVerts array
            0,
            // The number of triangles to draw
            2);
    }
}
```

호출 해야 `DrawGround` 에 우리의 `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

앱에서 다음을 실행 하면 표시 됩니다.

![](part2-images/image7.png "응용 프로그램 실행 될 때이 표시 됩니다.")

위의 코드에서 세부 정보 중 일부에 대해 살펴보겠습니다.

### <a name="view-and-projection-properties"></a>보기 및 프로젝션 속성

`View` 및 `Projection` 장면에서는 보는 방법을 제어 하는 속성입니다. म 수정이 코드 나중에 다시 모델 렌더링 코드를 추가 했습니다. 특히, `View` 위치 및 카메라의 방향을 제어 하 고 `Projection` 컨트롤은 *시야* (사용할 수 있는 카메라를 확대/축소) 합니다.

### <a name="techniques-and-passes"></a>기술 및 전달

한 번 할당 되어 속성을 수행할 수 있습니다이 효과 대해 실제 렌더링 합니다. 

변경 하지 않으므로 우리는 `CurrentTechnique` 이 연습에서는 있지만 보다 고급 게임 속성에는 다양 한 방법 (예: 색상 값 적용 되는 방식을)에서 그리기를 수행할 수 있도록 단일 효과 가질 수 있습니다. 각 렌더링 모드 렌더링 하기 전에 지정할 수 있는 기법으로 나타낼 수 있습니다. 또한 각 기법 제대로 렌더링에 대 한 패스가 여러 개 필요할 수 있습니다. 효과 발광 화면 또는 털 같은 복잡 한 시각적 개체를 렌더링 하는 경우 한 패스가 여러 개를 할 수 있습니다.

염두에 두어야 하는 `foreach` 루프 내부의 복잡성에 관계 없이 모든 효과 렌더링 동일한 C# 코드를 사용 하면 `BasicEffect`합니다.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` 꼭 짓 점을 렌더링 됩니다. 첫 번째 매개 변수 구성 방법 및 우리는 우리의 꼭지점 메서드를 알려 줍니다. 사용 하도록 각 삼각형 세 개의 정렬 된 꼭 짓 정의한 있도록 구조화 있는 우리는 `PrimitiveType.TriangleList` 값입니다.

두 번째 매개 변수는 앞에서 정의한는 꼭 짓 점 배열입니다.

세 번째 매개 변수를 그릴 첫 번째 인덱스를 지정 합니다. 렌더링 될 전체 꼭 짓 점 배열 할 것 이므로 값 0 전달 합니다.

마지막으로 렌더링할 수 삼각형 지정 합니다. 꼭 짓 점 배열에는 두 개의 삼각형 포함 하므로 값이 2 전달 합니다.

## <a name="rendering-with-a-texture"></a>질감을 사용 하 여 렌더링

이 시점에서 앱을 흰색 축 (관점)를 렌더링합니다. 그런 다음 질감 우리의 평면을 렌더링할 때 사용할이 프로젝트에 추가 합니다. 

간단 하 게 유지 하 고.png MonoGame 파이프라인 도구를 사용 하지 않고이 프로젝트에 직접 추가 합니다. 이 작업을 수행 하려면 다운로드 [이.png 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) 컴퓨터에 있습니다. 를 다운로드 한 후 마우스 오른쪽 단추로 클릭는 **콘텐츠** 솔루션 패드 고에서 폴더 **추가 > 파일 추가 중...**  . Android를 사용 하는 경우 다음이 폴더는 아래에 있을 수는 **자산** 관련 Android 프로젝트의 폴더에에서 있습니다. Ios, 그런 다음이 폴더에에서 있을 경우 iOS 프로젝트의 루트입니다. 위치로 이동 하 여 여기서 **checkerboard.png** 저장 되 고이 파일을 선택 합니다. 디렉터리에 파일을 복사 하려면 선택 합니다.

다음으로 만드는 코드를 추가 하겠습니다 우리의 `Texture2D` 인스턴스. 먼저 추가 하는 `Texture2D` 의 구성원으로 `Game1` 아래에서 `BasicEffect` 인스턴스:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

수정 `Game1.LoadContent` 다음과 같습니다.


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

다음으로 수정 된 `DrawGround` 메서드. 지정 하는 데 필요한 유일한 수정 `effect.TextureEnabled` 를 `true` 설정할 수는 `effect.Texture` 를 `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

마지막으로 수정 해야는 `Game1.Initialize` 우리의 꼭지점도 질감을 할당 하는 메서드를 조정 합니다.


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

코드를 실행 하는 경우 우리의 평면 체크 무늬 패턴 이제 표시 되는지 확인할 수 있습니다.

![](part2-images/image8.png "평면 체크 무늬 패턴 표시")

## <a name="modifying-texture-coordinates"></a>조정 텍스처를 수정 합니다.

MonoGame 사용 하 여 질감 좌표 0 및 질감의 너비 또는 높이 사이 있지 않고 0과 1 사이의 좌표를 정규화 합니다. 다음 다이어그램은 정규화 된 좌표를 시각화 하는 데 도움이 됩니다.

![](part2-images/image9.png "이 다이어그램 정규화 된 좌표를 시각화할 수 있습니다.")

정규화 된 질감 좌표 코드를 다시 작성 하거나 모델 (예:.fbx 파일)를 다시 만들지 않고도 질감 크기 조정을 허용 합니다. 정규화 된 좌표 특정 픽셀 보다는 비율을 나타내기 때문에 이것이 가능 합니다. 예를 들어 (1, 1)는 항상 질감 크기에 관계 없이 오른쪽 아래 모서리를 나타냅니다.

반복 횟수에 대 한 단일 변수를 사용 하는 질감 좌표 할당을 변경할 수 있습니다.


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

이 인해 20 회 반복 질감:

![](part2-images/image10.png "이 인해 20 회 반복 질감")


## <a name="rendering-vertices-with-models"></a>모델과 함께 꼭지점을 렌더링

우리의 평면 제대로 렌더링 하 고, 했으므로 모든 항목을 함께 보려면 모델 다시 추가할 수 있습니다. 첫째, 모델 코드를 다시 추가 하겠습니다 우리의 `Game1.Draw` 메서드 (수정 된 위치):

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

또한 만듭니다는 `Vector3` 에 `Game1` 우리의 카메라의 위치를 나타내는 합니다. 아래 필드를 추가 하겠습니다 우리의 `checkerboardTexture` 선언 합니다.

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

다음으로 로컬 제거 `cameraPosition` 에서 변수는 `DrawModel` 메서드:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

마찬가지로 로컬 제거 `cameraPosition` 에서 변수는 `DrawGround` 메서드:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

이제 코드 실행 하는 경우 동시에 모델과 명확 하 게 볼 수 있습니다.

![](part2-images/image11.png "모델과 명확 하 게 모두 동시에 표시 됩니다.")

카메라 위치를 수정 했습니다 (등 해당 X 값을 늘려는 예에서 카메라를 왼쪽으로 이동) 값은 명확 하 게 및 모델 둘 다 영향을 있는지 확인할 수 있습니다.

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

이 코드는 다음에 발생합니다.

![](part2-images/image3.png "이 보기에이 코드에서는 발생")

## <a name="summary"></a>요약

이 연습에서는 사용자 지정 렌더링을 수행 하려면 꼭 짓 점 배열을 사용 하는 방법을 배웠습니다. 이 경우 바둑판된 층 우리의 꼭 짓 점 기반 렌더링 질감으로 결합 하 여 만든 및 `BasicEffect`, 코드는 여기에 제시 된 하는 데 사용 모든 3D 렌더링에 대 한 기반으로 하지만 합니다. 했죠 동일한 장면의 모델을 기반으로 하는 꼭 짓 점 렌더링을 함께 사용할 수 있습니다.

## <a name="related-links"></a>관련 링크

- [바둑판 파일 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [완료 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
