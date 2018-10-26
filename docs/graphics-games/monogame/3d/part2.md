---
title: MonoGame에서 사용 하 여 3D 그래픽 그리기
description: MonoGame 지원 꼭 짓 점 배열을 사용 하 여 지점 당 기준 3D 개체 렌더링 되는 방법을 정의 합니다. 사용자가 동적 기 하 도형 만들기 특수 효과 구현 하 고 고르기를 통해 렌더링의 효율성을 개선에 꼭 짓 점 배열 활용을 걸릴 수 있습니다.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: df36c149e98e8c0cbb16de4c2cf52def5713ec13
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50121440"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>MonoGame에서 사용 하 여 3D 그래픽 그리기

_MonoGame 지원 꼭 짓 점 배열을 사용 하 여 지점 당 기준 3D 개체 렌더링 되는 방법을 정의 합니다. 사용자가 동적 기 하 도형 만들기 특수 효과 구현 하 고 고르기를 통해 렌더링의 효율성을 개선에 꼭 짓 점 배열 활용을 걸릴 수 있습니다._

읽고 있는 사용자를 [렌더링 모델에 대 한 가이드](~/graphics-games/monogame/3d/part1.md) 익숙할 것 MonoGame에서 3D 모델을 렌더링 합니다. `Model` 클래스는 정적 데이터를 처리할 때 (예:.fbx) 파일에 정의 된 데이터로 작업할 때 및 3D 그래픽을 렌더링 하는 효과적인 방법입니다. 일부 게임에서는 3D 기 하 도형 정의 또는 런타임에 동적으로 조작 합니다. 이러한 경우 배열을 사용할 수 있습니다 *꼭 짓 점* 정의 하 고 기 하 도형에 렌더링 합니다. 꼭 짓 점을 geometry를 정의 하는 데 사용 하는 정렬 된 목록에 참가 하는 3D 공간에서 지점에 대 한 일반적인 용어입니다. 일반적으로 꼭 짓 점 일련의 삼각형을 정의 하는 방식으로 정렬 됩니다.

꼭 짓 점을 3D 개체를 만드는 데 사용 되는 방법을 시각화를 위해 다음 구체를 살펴보겠습니다.

![](part2-images/image1.png "꼭 짓 점을 3D 개체를 만드는 데 사용 되는 방법을 시각화를 위해이 구를 것이 좋습니다.")

위와 같이 구체 여러 삼각형의 명확 하 게 이루어집니다. 꼭 짓 점을 폼 삼각형에 연결 하는 방법을 보려면 구의 와이어 프레임을 볼 수 있습니다.

![](part2-images/image2.png "꼭 짓 점을 폼 삼각형에 연결 하는 방법을 보려면 구의 와이어 프레임을 보려면")

이 연습에서는 다음 항목을 설명 합니다.

- 프로젝트 만들기
- 꼭 짓 점 만들기
- 그리기 코드를 추가합니다.
- 질감을 사용 하 여 렌더링
- 질감 좌표를 수정합니다.
- 모델을 사용 하 여 꼭 짓 점 렌더링

완료 된 프로젝트는 꼭 짓 점 배열을 사용 하 여 그릴 결승된 floor를 포함 됩니다.

![](part2-images/image3.png "완료 된 프로젝트는 꼭 짓 점 배열을 사용 하 여 그릴 결승된 floor 포함 됩니다.")

## <a name="creating-a-project"></a>프로젝트 만들기

먼저 우리의 시작 점으로 사용할 프로젝트를 다운로드 했습니다. 모델 프로젝트를 사용 하 여 [여기서 찾을 수 있는](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)합니다.

다운로드 하 고 압축을 푼 후 열고 프로젝트를 실행 합니다. 화면에 그려지는 6 로봇 모델 표시 하고자 하는 것:

![](part2-images/image4.png "화면에 그려지는 6 로봇 모델")

이 프로젝트의 끝에서에서는 됩니다 수 결합 고유한 사용자 지정 버텍스 렌더링 로봇 `Model`이므로 로봇 렌더링 코드를 삭제 하는 것이 않겠습니다. 지웁니다 방금에서는 대신는 `Game1.Draw` 지금은 6 로봇의 그리기를 제거 하는 호출 합니다. 이 위해 엽니다는 **Game1.cs** 찾아서 파일을 `Draw` 메서드. 다음 코드를 포함 하도록 수정 합니다.

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

이 게임을 빈 블루 스크린이 표시 됩니다.

![](part2-images/image5.png "그러면 빈는 블루 스크린이 표시 게임")

## <a name="creating-the-vertices"></a>꼭 짓 점 만들기

이 기 하 도형 정의에 꼭 짓 점의 배열을 만듭니다. 이 연습에서는 만들게 됩니다를 3D 평면 (3D 공간에서 사각형, 비행기 없습니다). 이 평면 네 면 및 네 모서리에 있지만 두 개의 삼각형, 세 개의 꼭 짓 점 필요한 각각의 구성 됩니다. 따라서 우리가 정의 하 게 됩니다 총 6 개의 점입니다.

지금까지 이야기 했습니다 일반적인 의미의 꼭 짓 점 하는 방법에 대 한 하지만 MonoGame 꼭 짓 점에 사용할 수 있는 몇 가지 표준 구조체를 제공 합니다.

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

각 형식의 이름을 포함 하는 구성 요소를 나타냅니다. 예를 들어 `VertexPositionColor` 위치와 색에 대 한 값을 포함 합니다. 각 구성 요소에 살펴보겠습니다.

- 위치 – 모든 꼭 짓 점 형식 포함을 `Position` 구성 요소입니다. `Position` 값 (X, Y 및 Z) 3D 공간에서 꼭 짓 점 위치를 정의 합니다.
- 색 – 꼭 짓 점 선택적으로 지정할 수는 `Color` 색조 사용자 지정을 수행 하는 값입니다.
- 보통-Normals 개체 표면에 가입과 관련 한 방향을 정의 합니다. Normals는 화면을 조명의 정도 영향 받은 연결 되는 조명 방향 이후를 사용 하 여 개체를 렌더링 하는 경우 필요 합니다. Normals로 일반적으로 지정 되는 *단위 벡터* – 3D 벡터의 길이는 1입니다.
- 질감 – 질감 질감 좌표 – 즉,: 질감의 부분을 지정 된 꼭 짓 점 앞에 나타납니다 가리킵니다. 질감 값은 질감을 사용 하 여 3D 개체를 렌더링 하는 경우에 필요 합니다. 질감 좌표는 정규화 된 좌표 값이 0과 1 사이의 포함 됩니다. 질감 좌표가이 가이드의 뒷부분에서 자세히 설명 합니다.

이 평면은 바닥으로 사용 되 고 사용 하므로 렌더링을 수행 하는 경우 질감을 적용 해야 할 수는 `VertexPositionTexture` 이 꼭 짓 점 정의 하는 형식입니다.

먼저 Game1 클래스 멤버를 추가 됩니다.

```csharp
VertexPositionTexture[] floorVerts; 
```

다음으로의 꼭 짓 점 정의 `Game1.Initialize`합니다. 이 문서의 앞부분에서 참조 하는 제공된 된 템플릿을 포함 하지 않는 표시는 `Game1.Initialize` 전체 메서드를 추가 해야 하므로 `Game1`:

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

이 꼭 짓 점 모양을 시각화를 위해 다음 다이어그램을 고려 합니다.

![](part2-images/image6.png "꼭 짓 점의 모양을 시각화를 위해이 다이어그램을 것이 좋습니다.")

렌더링 코드를 구현 합니다. 완료 될 때까지 꼭 짓 점을 시각화 하려면 다이어그램을 사용 해야 합니다.

## <a name="adding-drawing-code"></a>그리기 코드를 추가합니다.

정의 하는 기 하 도형에 대 한 위치를 만들었으므로 이제 해당 렌더링 코드를 작성할 수 있습니다.

먼저 정의 해야 합니다는 `BasicEffect` 위치와 같은 렌더링 및 조명 매개 변수를 저장할 인스턴스. 이 작업을 수행 하려면 추가 `BasicEffect` 멤버를 `Game1` where 아래 클래스는 `floorVerts` 필드가 정의:


```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

다음으로 수정 된 `Initialize` 효과 정의 하는 방법.

```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;
    // new code:
    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

이제 그리기를 수행 하는 코드를 추가할 수 있습니다.

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
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

호출 해야 `DrawGround` 에서 우리의 `Game1.Draw`:

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

앱을 실행 하는 경우 다음에 표시 됩니다.

![](part2-images/image7.png "앱을 실행 하는 경우이 표시 됩니다.")

위의 코드에서 세부 정보 중 일부에 대해 살펴보겠습니다.

### <a name="view-and-projection-properties"></a>뷰 및 프로젝션 속성

합니다 `View` 고 `Projection` 장면 보고 하는 방법을 제어 하는 속성입니다. 수정할 것이 코드 나중에 모델 렌더링 코드를 다시 추가 했습니다. 특히 `View` 제어 위치와 카메라의 방향 및 `Projection` 컨트롤을 *시야* (사용할 수 있는 카메라 확대/축소) 합니다.

### <a name="techniques-and-passes"></a>기술 및 전달

한 번 할당 했습니다. 속성을 수행할 수 있습니다이 효과 대해 실제 렌더링 합니다. 

에서는 변경 하지 않습니다는 `CurrentTechnique` 이 연습에서는 고급 게임에는 속성 (예: 색 값을 적용 되는 방식을) 가지에서 드로잉을 수행할 수 있는 단일 효과 가질 수 있습니다. 각 렌더링 모드 렌더링 하기 전에 할당 되어야 하는 기법으로 나타낼 수 있습니다. 또한 각 기술의 제대로 렌더링에 대 한 패스가 여러 개 필요할 수 있습니다. 효과 빛나는 화면 또는 털와 같은 복잡 한 시각적 개체를 렌더링 하는 경우 여러 전달 해야 합니다.

염두에 두어야 합니다 `foreach` 루프를 사용 하면 동일한 C# 기본의 복잡성에 관계 없이 모든 효과 렌더링 하는 코드 `BasicEffect`합니다.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives` 꼭 짓 점을 렌더링 되는 위치입니다. 첫 번째 매개 변수는 꼭 짓 점 이끄는 것 어떻게 메서드를 알려 줍니다. 사용 하도록 각 삼각형 세 개의 정렬 된 꼭 짓 점에서 정의한 있도록 구조적 있는 것을 `PrimitiveType.TriangleList` 값입니다.

두 번째 매개 변수는 앞에서 정의한는 꼭 짓 점 배열입니다.

세 번째 매개 변수는 그리려는 첫 번째 인덱스를 지정 합니다. 렌더링할 전체 꼭 짓 점 배열, 것 이므로 0을 전달 합니다.

마지막으로, 얼마나 많은 삼각형 렌더링을 지정 합니다. 꼭 짓 점 배열 두 삼각형 들어 있으므로 2 값을 전달 합니다.

## <a name="rendering-with-a-texture"></a>질감을 사용 하 여 렌더링

이 시점에서 앱을 흰색 평면 (큐브 뷰)를 렌더링합니다. 다음 질감 우리의 평면을 렌더링할 때 사용 되는 프로젝트에 추가 하겠습니다. 

간단 하 게 유지 하는.png MonoGame 파이프라인 도구를 사용 하지 않고 프로젝트에 직접 추가 합니다. 이 위해 다운로드할 [이.png 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) 컴퓨터에 있습니다. 다운로드 한 후 마우스 오른쪽 단추로 클릭 합니다 **콘텐츠** 선택한 Solution pad에서 폴더 **추가 > 파일 추가...**  . Android에서 작동 하는 경우 다음이 폴더에 배치 됩니다 합니다 **자산** Android 특정 프로젝트의 폴더입니다. Ios의 경우 다음이 폴더에에서 있을 경우 iOS 프로젝트의 루트입니다. 위치로 이동 합니다. 여기서 **checkerboard.png** 저장 되 고이 파일을 선택 합니다. 디렉터리에 파일을 복사 하려면 선택 합니다.

다음으로 만드는 코드를 추가한 우리의 `Texture2D` 인스턴스. 먼저 추가 합니다 `Texture2D` 의 멤버로 `Game1` 아래에서 `BasicEffect` 인스턴스:

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

수정 `Game1.LoadContent` 다음과 같습니다.


```csharp
protected override void LoadContent()
{
    // Notice that loading a model is very similar
    // to loading any other XNB (like a Texture2D).
    // The only difference is the generic type.
    model = Content.Load<Model> ("robot");

    // We aren't using the content pipeline, so we need
    // to access the stream directly:
    using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
    {
        checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
    }
}
```

다음으로 수정 된 `DrawGround` 메서드. 수정만 수행 하면 할당 하는 것 `effect.TextureEnabled` 하 `true` 및 설정 하는 `effect.Texture` 에 `checkerboardTexture`:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraPosition = new Vector3 (0, 40, 20);
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);

    float aspectRatio = 
        graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
    float nearClipPlane = 1;
    float farClipPlane = 200;

    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

    // new code:
    effect.TextureEnabled = true;
    effect.Texture = checkerboardTexture;

    foreach (var pass in effect.CurrentTechnique.Passes)
    {
        pass.Apply ();

        graphics.GraphicsDevice.DrawUserPrimitives (
                    PrimitiveType.TriangleList,
            floorVerts,
            0,
            2);
    }
}
```

마지막으로 수정 해야 합니다 `Game1.Initialize` 질감을 할당할 수도 방법 좌표는 꼭 짓 점:


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    // New code:
    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, 1);
    floorVerts [2].TextureCoordinate = new Vector2 (1, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (1, 1);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
} 
```

코드를 실행 하는 경우이 평면 체크 무늬 패턴을 표시 하는 이제 볼 수 있습니다.

![](part2-images/image8.png "평면을 체크 무늬 패턴 표시")

## <a name="modifying-texture-coordinates"></a>좌표를 질감 수정

MonoGame 사용 좌표 0 및 질감의 너비 또는 높이 사이 있지 않고 0과 1 사이의 수 있는 질감 좌표를 정규화 합니다. 다음 다이어그램은 정규화 된 좌표를 시각화 하는 데 도움이 됩니다.

![](part2-images/image9.png "이 다이어그램 정규화 된 좌표를 시각화 하는 데 도움이 됩니다.")

정규화 된 질감 좌표를 질감 코드를 다시 작성 하거나 모델 (예:.fbx 파일)를 다시 만들지 않고도 조정 합니다. 정규화 된 좌표에 특정 픽셀 보다는 비율을 나타내기 때문에 이것이 가능 합니다. 예를 들어, (1, 1)은 항상 질감 크기에 관계 없이 오른쪽 아래 모서리를 나타냅니다.

질감 좌표 할당 하는 단일 변수를 사용 하 여 반복 횟수에 대 한 변경할 수 있습니다.


```csharp
protected override void Initialize ()
{
    floorVerts = new VertexPositionTexture[6];

    floorVerts [0].Position = new Vector3 (-20, -20, 0);
    floorVerts [1].Position = new Vector3 (-20,  20, 0);
    floorVerts [2].Position = new Vector3 ( 20, -20, 0);

    floorVerts [3].Position = floorVerts[1].Position;
    floorVerts [4].Position = new Vector3 ( 20,  20, 0);
    floorVerts [5].Position = floorVerts[2].Position;

    int repetitions = 20;

    floorVerts [0].TextureCoordinate = new Vector2 (0, 0);
    floorVerts [1].TextureCoordinate = new Vector2 (0, repetitions);
    floorVerts [2].TextureCoordinate = new Vector2 (repetitions, 0);

    floorVerts [3].TextureCoordinate = floorVerts[1].TextureCoordinate;
    floorVerts [4].TextureCoordinate = new Vector2 (repetitions, repetitions);
    floorVerts [5].TextureCoordinate = floorVerts[2].TextureCoordinate;

    effect = new BasicEffect (graphics.GraphicsDevice);

    base.Initialize ();
}
```

이 인해 질감 20 번 반복 합니다.

![](part2-images/image10.png "이 인해 20 번 반복 하는 질감")


## <a name="rendering-vertices-with-models"></a>모델을 사용 하 여 꼭 짓 점 렌더링

이제는 평면 올바르게 렌더링 하는 모든 것을 볼 함께 모델 다시 추가할 수 있습니다. 모델 코드를 다시 추가 먼저 우리의 `Game1.Draw` 메서드 (수정 된 위치).

```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);

    DrawGround ();

    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));

    DrawModel (new Vector3 (-4, 4, 3));
    DrawModel (new Vector3 ( 0, 4, 3));
    DrawModel (new Vector3 ( 4, 4, 3));

    base.Draw(gameTime);
} 
```

또한 만들겠습니다를 `Vector3` 에서 `Game1` 이 카메라의 위치를 나타내는입니다. 아래 필드를 추가 했습니다 우리의 `checkerboardTexture` 선언 합니다.

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10); 
```

다음 단계에서는 로컬을 제거 `cameraPosition` 변수는 `DrawModel` 메서드:

```csharp
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = Matrix.CreateTranslation (modelPosition);

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector); 
            ...
```

마찬가지로 로컬 제거 `cameraPosition` 변수는 `DrawGround` 메서드:

```csharp
void DrawGround()
{
    // The assignment of effect.View and effect.Projection
    // are nearly identical to the code in the Model drawing code.
    var cameraLookAtVector = Vector3.Zero;
    var cameraUpVector = Vector3.UnitZ;

    effect.View = Matrix.CreateLookAt (
        cameraPosition, cameraLookAtVector, cameraUpVector);
    ... 
```

이제 코드를 실행 하는 경우 보면 모델과 처음부터 모두 동시에:

![](part2-images/image11.png "모델 및 처음부터 모두 동시에 표시 됩니다.")

카메라 위치를 수정 했습니다 (같은 해당 X 값을 늘려는 여기서 카메라를 왼쪽으로 이동)는 값에 영향을 줍니다 기초 및 모델을 모두 볼 수 있습니다.

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

이 코드는 다음 발생합니다.

![](part2-images/image3.png "이 코드는이 뷰에서 결과")

## <a name="summary"></a>요약

이 연습에서는 사용자 지정 렌더링을 수행 하려면 꼭 짓 점 배열을 사용 하는 방법을 보여 주었습니다. 이 예제의 경우 질감을 사용 하 여이 꼭 짓 점 기반 렌더링을 결합 하 여 체크 무늬 floor를 만들었습니다 및 `BasicEffect`, 하지만 코드를 여기에 제시 된는 모든 3D 렌더링에 대 한 기준으로 합니다. 또한 동일한 장면에서 모델을 사용 하 여 꼭 짓 점 기반 렌더링을 혼합할 수는 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [체크 무늬 file (샘플)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [완성 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)
