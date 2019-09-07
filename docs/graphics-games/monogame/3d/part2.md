---
title: MonoGame에서 꼭 짓 점을 사용 하 여 3D 그래픽 그리기
description: MonoGame는 꼭 짓 점 배열의 사용을 지원 하 여 3D 개체가 특정 시점에 렌더링 되는 방식을 정의 합니다. 사용자는 꼭 짓 점 배열을 활용 하 여 동적 기 하 도형을 만들고, 특수 효과를 구현 하 고, 고르기을 통해 렌더링의 효율성을 향상 시킬 수 있습니다.
ms.prod: xamarin
ms.assetid: 932AF5C2-884D-46E1-9455-4C359FD7C092
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 1f2fce14f1839e3d9aff4c68dc0dffc0e8059e6c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766811"
---
# <a name="drawing-3d-graphics-with-vertices-in-monogame"></a>MonoGame에서 꼭 짓 점을 사용 하 여 3D 그래픽 그리기

_MonoGame는 꼭 짓 점 배열의 사용을 지원 하 여 3D 개체가 특정 시점에 렌더링 되는 방식을 정의 합니다. 사용자는 꼭 짓 점 배열을 활용 하 여 동적 기 하 도형을 만들고, 특수 효과를 구현 하 고, 고르기을 통해 렌더링의 효율성을 향상 시킬 수 있습니다._

[모델 렌더링에 대 한 지침](~/graphics-games/monogame/3d/part1.md) 을 읽은 사용자는 MonoGame에서 3d 모델 렌더링에 익숙할 것입니다. 클래스 `Model` 는 파일 (예: fbx)에 정의 된 데이터를 사용 하 고 정적 데이터를 처리할 때 3d 그래픽을 렌더링 하는 효과적인 방법입니다. 일부 게임에는 런타임에 동적으로 정의 하거나 조작 하는 3D 형상이 필요 합니다. 이러한 경우 *꼭 짓 점* 배열을 사용 하 여 기 하 도형을 정의 하 고 렌더링할 수 있습니다. 꼭 짓 점은 기 하 도형을 정의 하는 데 사용 되는 정렬 된 목록의 일부인 3D 공간에 대 한 일반적인 용어입니다. 일반적으로 꼭 짓 점은 일련의 삼각형을 정의 하는 방식으로 정렬 됩니다.

꼭 짓 점이 3D 개체를 만드는 데 사용 되는 방식을 시각화 하는 데 도움이 되도록 다음 구를 살펴보겠습니다.

![](part2-images/image1.png "꼭 짓 점을 사용 하 여 3D 개체를 만드는 방법을 시각화 하려면이 구를 고려 합니다.")

위와 같이 구는 여러 삼각형으로 명확 하 게 구성 됩니다. 구의 와이어 프레임을 확인 하 여 꼭지점이 폼 삼각형에 어떻게 연결 되는지 확인할 수 있습니다.

![](part2-images/image2.png "구의 와이어 프레임을 확인 하 여 꼭지점이 폼 삼각형에 연결 되는 방식을 확인 합니다.")

이 연습에서는 다음 항목을 다룹니다.

- 프로젝트 만들기
- 꼭 짓 점 만들기
- 그리기 코드 추가
- 질감으로 렌더링
- 질감 좌표 수정
- 모델을 사용 하 여 꼭 짓 점 렌더링

완성 된 프로젝트에는 꼭 짓 점 배열을 사용 하 여 바둑판 층이 포함 됩니다.

![](part2-images/image3.png "완성 된 프로젝트에는 꼭 짓 점 배열을 사용 하 여 바둑판 층이 포함 됩니다.")

## <a name="creating-a-project"></a>프로젝트 만들기

먼저 시작 지점 역할을 하는 프로젝트를 다운로드 합니다. [여기에서 찾을 수 있는](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/)모델 프로젝트를 사용 합니다.

다운로드 하 고 압축을 푼 후 프로젝트를 열고 실행 합니다. 화면에 표시 되는 여섯 개의 로봇 모델이 표시 될 것입니다.

![](part2-images/image4.png "화면에 그려진 6 개의 로봇 모델")

이 프로젝트의 끝에는 로봇 `Model`을 사용 하 여 사용자 지정 꼭 짓 점 렌더링을 결합 하 게 되므로 로봇 렌더링 코드를 삭제 하지 않습니다. 대신, 이제 `Game1.Draw` 호출을 지워 6 개 로봇의 그리기를 제거 합니다. 이렇게 하려면 **Game1.cs** 파일을 열고 메서드를 `Draw` 찾습니다. 다음 코드를 포함 하도록 수정 합니다.

```csharp
protected override void Draw(GameTime gameTime)
{
  GraphicsDevice.Clear(Color.CornflowerBlue);
  base.Draw(gameTime);
}
```

이로 인해 게임이 빈 파란색 화면으로 표시 됩니다.

![](part2-images/image5.png "그러면 게임이 빈 파란색 화면으로 표시 됩니다.")

## <a name="creating-the-vertices"></a>꼭 짓 점 만들기

기 하 도형을 정의 하는 꼭 짓 점 배열을 만듭니다. 이 연습에서는 3D 평면 (비행기가 아닌 3D 공간에서 사각형)을 만듭니다. 평면에는 네 개의 면과 네 개의 모퉁이가 있지만 각각 3 개의 꼭지점이 필요한 두 개의 삼각형으로 구성 됩니다. 따라서 총 6 개의 점이 정의 됩니다.

지금까지 꼭 짓 점에 대해 일반적으로 이야기 했지만 MonoGame는 꼭 짓 점에 사용할 수 있는 몇 가지 표준 구조체를 제공 합니다.

- `Microsoft.Xna.Framework.Graphics.VertexPositionColor`
- `Microsoft.Xna.Framework.Graphics.VertexPositionColorTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionNormalTexture`
- `Microsoft.Xna.Framework.Graphics.VertexPositionTexture`

각 형식의 이름은 포함 하는 구성 요소를 나타냅니다. 예를 들어 `VertexPositionColor` 에는 위치 및 색에 대 한 값이 포함 됩니다. 각 구성 요소를 살펴보겠습니다.

- Position – 모든 꼭 짓 점 형식 `Position` 에는 구성 요소가 포함 됩니다. 값 `Position` 은 꼭지점이 3d 공간 (X, Y, Z)에서 배치 되는 위치를 정의 합니다.
- 색 – 꼭 짓 점은 선택적으로 `Color` 사용자 지정 색조를 수행 하는 값을 지정할 수 있습니다.
- Normal – 법선은 개체의 표면이 향하는 방법을 정의 합니다. 표면이 가리키는 방향이 받는 빛의 양에 영향을 주므로 조명을 사용 하 여 개체를 렌더링 하는 경우 법선이 필요 합니다. 일반적으로 법선은 길이가 1 인 3D 벡터 인 *단위 벡터로* 지정 됩니다.
- 질감 – 질감은 질감 좌표를 나타냅니다. 즉, 특정 꼭 짓 점에 표시 되어야 하는 질감의 부분입니다. 질감이 있는 3D 개체를 렌더링 하는 경우 질감 값이 필요 합니다. 질감 좌표는 정규화 된 좌표 이며 값은 0에서 1 사이입니다. 질감 좌표는이 가이드의 뒷부분에서 자세히 설명 합니다.

비행기가 바닥의 역할을 하 고, 렌더링을 수행 하는 동안 질감을 적용 하려고 하므로, `VertexPositionTexture` 형식을 사용 하 여 꼭 짓 점을 정의 합니다.

먼저 Game1 클래스에 멤버를 추가 합니다.

```csharp
VertexPositionTexture[] floorVerts;
```

그런 다음에서 `Game1.Initialize`꼭 짓 점을 정의 합니다. 이 문서의 앞부분에서 참조 하는 제공 된 템플릿에는 `Game1.Initialize` 메서드가 포함 되어 있지 않으므로 전체 메서드를에 `Game1`추가 해야 합니다.

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

꼭 짓 점이 표시 되는 모양을 시각화 하려면 다음 다이어그램을 참조 하세요.

![](part2-images/image6.png "꼭 짓 점이 표시 되는 모양을 시각화 하려면 다음 다이어그램을 참조 하세요.")

렌더링 코드 구현을 마칠 때까지 꼭 짓 점을 시각화 하려면 다이어그램을 사용 해야 합니다.

## <a name="adding-drawing-code"></a>그리기 코드 추가

기 하 도형에 대해 정의 된 위치를 만들었으므로 렌더링 코드를 작성할 수 있습니다.

먼저 위치 및 조명과 같은 렌더링을 위한 `BasicEffect` 매개 변수를 보관할 인스턴스를 정의 해야 합니다. 이렇게 하려면 `floorVerts` 필드가 정의 된 아래 `BasicEffect` `Game1` 클래스에 멤버를 추가 합니다.

```csharp
...
VertexPositionTexture[] floorVerts;
// new code:
BasicEffect effect;
```

그런 다음, `Initialize` 메서드를 수정 하 여 효과를 정의 합니다.

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

다음에서를 호출 `DrawGround` 해야 합니다. `Game1.Draw`

```csharp
protected override void Draw (GameTime gameTime)
{
    GraphicsDevice.Clear (Color.CornflowerBlue);

    DrawGround ();

    base.Draw (gameTime);
}
```

앱이 실행 될 때 다음을 표시 합니다.

![](part2-images/image7.png "앱이 실행 될 때이를 표시 합니다.")

위의 코드에 있는 세부 정보 중 일부를 살펴보겠습니다.

### <a name="view-and-projection-properties"></a>보기 및 프로젝션 속성

`View` 및`Projection` 속성은 장면을 보는 방법을 제어 합니다. 모델 렌더링 코드를 다시 추가할 때 나중에이 코드를 수정할 예정입니다. 특히는 카메라의 위치와 방향을 `Projection` 제어하고보기의필드를제어합니다.이필드는카메라를확대/축소하는데사용할수있습니다.`View`

### <a name="techniques-and-passes"></a>기술 및 통과

효과에 대 한 속성을 할당 하면 실제 렌더링을 수행할 수 있습니다.

이 연습에서는 속성을 `CurrentTechnique` 변경 하지 않지만 고급 게임에는 다른 방식으로 그리기를 수행할 수 있는 단일 효과 (예: 색 값이 적용 되는 방법)가 있을 수 있습니다. 이러한 각 렌더링 모드는 렌더링 전에 할당 될 수 있는 기술로 표시 될 수 있습니다. 또한 각 기법을 제대로 렌더링 하려면 여러 단계가 필요할 수 있습니다. 광선 표면 또는 이유 같이 복잡 한 시각적 개체를 렌더링 하는 경우에는 여러 패스가 필요할 수 있습니다.

주의 해야 할 중요 한 점은 루프에서 `foreach` 기본 `BasicEffect`의 복잡성에 관계 없이 C# 동일한 코드를 사용 하 여 모든 효과를 렌더링 하도록 하는 것입니다.

### <a name="drawuserprimitives"></a>DrawUserPrimitives

`DrawUserPrimitives`는 꼭 짓 점이 렌더링 되는 위치입니다. 첫 번째 매개 변수는 메서드를 메서드로 구성 하는 방법을 알려 줍니다. 각 삼각형이 세 개의 정렬 된 꼭 짓 점으로 정의 되도록 구성 했으므로 `PrimitiveType.TriangleList` 값을 사용 합니다.

두 번째 매개 변수는 앞에서 정의한 꼭 짓 점의 배열입니다.

세 번째 매개 변수는 그릴 첫 번째 인덱스를 지정 합니다. 전체 꼭 짓 점 배열을 렌더링 하려고 하기 때문에 값 0을 전달 합니다.

마지막으로 렌더링할 삼각형 수를 지정 합니다. 꼭 짓 점 배열에 두 개의 삼각형이 있으므로 값 2를 전달 합니다.

## <a name="rendering-with-a-texture"></a>질감으로 렌더링

이 시점에서 앱은 흰색 평면 (관점에서)을 렌더링 합니다. 다음으로는 평면을 렌더링할 때 사용할 텍스처를 프로젝트에 추가 합니다.

작업을 단순하게 유지 하기 위해 MonoGame 파이프라인 도구를 사용 하는 대신 .png를 프로젝트에 직접 추가할 수 있습니다. 이렇게 하려면 [이 .png 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true) 을 컴퓨터에 다운로드 합니다. 다운로드 되 면 Solution pad에서 **Content** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 파일 추가** ...를 선택 합니다. Android에서 작업 하는 경우이 폴더는 Android 관련 프로젝트의 **자산** 폴더 아래에 있습니다. IOS에서이 폴더는 iOS 프로젝트의 루트에 있습니다. **체크 무늬 .png** 가 저장 된 위치로 이동 하 여이 파일을 선택 합니다. 디렉터리에 파일을 복사 하려면 선택 합니다.

다음으로, `Texture2D` 인스턴스를 만드는 코드를 추가 합니다. 먼저 `Game1` `Texture2D` 인스턴스`BasicEffect` 아래에를 멤버로 추가 합니다.

```csharp
...
BasicEffect effect;
// new code:
Texture2D checkerboardTexture;
```

다음과 `Game1.LoadContent` 같이 수정 합니다.

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

그런 다음 `DrawGround` 메서드를 수정 합니다. 를 `effect.TextureEnabled` 에 할당 `true` 하 고를 `effect.Texture` 로 `checkerboardTexture`설정 하는 것만 필요 합니다.

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

마지막으로, 꼭지점에 질감 좌표 `Game1.Initialize` 를 할당 하도록 메서드를 수정 해야 합니다.

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

코드를 실행 하는 경우 이제 평면에서 바둑판 패턴을 표시 하는 것을 볼 수 있습니다.

![](part2-images/image8.png "이제 평면에서 바둑판 패턴을 표시 합니다.")

## <a name="modifying-texture-coordinates"></a>질감 좌표 수정

MonoGame는 0과 1 사이의 좌표를 갖는 정규화 된 질감 좌표를 사용 합니다 .이는 0과 질감의 너비 또는 높이입니다. 다음 다이어그램은 정규화 된 좌표를 시각화 하는 데 도움이 됩니다.

![](part2-images/image9.png "이 다이어그램은 정규화 된 좌표를 시각화 하는 데 도움이 됩니다.")

정규화 된 질감 좌표를 사용 하면 코드를 다시 작성 하거나 모델을 다시 만들 필요 없이 텍스처 크기를 조정할 수 있습니다 (예: fbx 파일). 이는 정규화 된 좌표가 특정 픽셀이 아닌 비율을 나타내므로 가능 합니다. 예를 들어, (1, 1)은 항상 질감 크기와 관계 없이 오른쪽 아래 모퉁이를 나타냅니다.

반복 횟수에 대 한 단일 변수를 사용 하도록 텍스처 좌표 할당을 변경할 수 있습니다.

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

이로 인해 질감이 20 번 반복 됩니다.

![](part2-images/image10.png "이로 인해 질감이 20 번 반복 됩니다.")

## <a name="rendering-vertices-with-models"></a>모델을 사용 하 여 꼭 짓 점 렌더링

이제 평면이 제대로 렌더링 되었으므로 모델을 다시 추가 하 여 모든 항목을 함께 볼 수 있습니다. 먼저 모델 코드 `Game1.Draw` 를 수정 된 위치를 사용 하 여 메서드에 다시 추가 합니다.

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

또한 `Vector3` 에서`Game1` 카메라의 위치를 나타내는를 만듭니다. `checkerboardTexture` 선언 아래에 필드를 추가 합니다.

```csharp
...
Texture2D checkerboardTexture;
// new code:
Vector3 cameraPosition = new Vector3(0, 10, 10);
```

다음으로 `cameraPosition` `DrawModel` 메서드에서 지역 변수를 제거 합니다.

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

마찬가지로 `cameraPosition` `DrawGround` 메서드에서 지역 변수를 제거 합니다.

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

이제 코드를 실행 하는 경우 모델과 땅을 동시에 볼 수 있습니다.

![](part2-images/image11.png "모델과 그라운드 모두 동시에 표시 됩니다.")

카메라 위치를 수정 하는 경우 (예: X 값을 늘려서 카메라를 왼쪽으로 이동 하는 경우) 값이 그라운드 모델과 모델에 모두 영향을 주는 것을 볼 수 있습니다.

```csharp
Vector3 cameraPosition = new Vector3(15, 10, 10);
```

이 코드는 다음과 같은 결과를 생성 합니다.

![](part2-images/image3.png "이 코드는이 뷰를 생성 합니다.")

## <a name="summary"></a>요약

이 연습에서는 꼭 짓 점 배열을 사용 하 여 사용자 지정 렌더링을 수행 하는 방법을 살펴보았습니다. 이 경우 꼭 짓 점 기반 렌더링과 질감 및 `BasicEffect`를 결합 하 여 바둑판 층을 만들었으며 여기에 제공 된 코드는 3d 렌더링의 기반으로 사용 됩니다. 또한 동일한 장면의 모델과 함께 버텍스 기반 렌더링을 혼합할 수 있음을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [바둑판 파일 (샘플)](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/checkerboard.png?raw=true)
- [완료 된 프로젝트 (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/)
