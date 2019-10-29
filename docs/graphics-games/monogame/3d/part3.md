---
title: MonoGame의 3D 좌표
description: 3d 좌표 시스템을 이해 하는 것은 3D 게임 개발의 중요 한 단계입니다. MonoGame는 3D 공간에서 개체의 위치 지정, 방향 지정 및 크기 조정에 사용할 수 있는 여러 클래스를 제공 합니다.
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: a38f4b81f684d6d416e6abe017bc463e3097c6b1
ms.sourcegitcommit: 93697a20e6fc7da547a8714ac109d7953b61d63f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/28/2019
ms.locfileid: "72980863"
---
# <a name="3d-coordinates-in-monogame"></a>MonoGame의 3D 좌표

_3d 좌표 시스템을 이해 하는 것은 3D 게임 개발의 중요 한 단계입니다. MonoGame는 3D 공간에서 개체의 위치 지정, 방향 지정 및 크기 조정에 사용할 수 있는 여러 클래스를 제공 합니다._

3D 게임을 개발 하려면 3D 좌표계에서 개체를 조작 하는 방법을 이해 해야 합니다. 이 연습에서는 시각적 개체 (특히 모델)를 조작 하는 방법을 다룹니다. 3D 카메라 클래스를 만들기 위한 모델을 제어 하는 개념을 기반으로 합니다.

제시 된 개념은 선형 대 수에서 시작 되지만 강력한 수학 배경이 없는 모든 사용자는 자신의 게임에서 이러한 개념을 적용할 수 있습니다.

다음 항목을 다룹니다.

- 프로젝트 만들기
- 로봇 엔터티 만들기
- 로봇 엔터티 이동
- 행렬 곱하기
- 카메라 엔터티 만들기
- 입력으로 카메라 이동

완료 되 면 원형으로 이동 하는 로봇 및 터치식 입력으로 제어할 수 있는 카메라를 포함 하는 프로젝트가 제공 됩니다.

![](part3-images/image1.gif "Once finished, the app will include a project with a robot moving in a circle and a camera which can be controlled by touch input")

## <a name="creating-a-project"></a>프로젝트 만들기

이 연습에서는 3D 공간에서 개체를 이동 하는 방법을 집중적으로 설명 합니다. [여기에서 찾을 수 있는](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelsandvertsmg/)모델 및 꼭 짓 점 배열의 렌더링을 위한 프로젝트를 시작 합니다. 다운로드가 완료 되 면 프로젝트를 압축 해제 하 고 열어 실행 하는지 확인 하 고 다음을 확인 해야 합니다.

![](part3-images/image2.png "Once downloaded, unzip and open the project to make sure it runs and this view should be displayed")

## <a name="creating-a-robot-entity"></a>로봇 엔터티 만들기

로봇을 이동 하기 전에 그리기 및 이동에 대 한 논리를 포함 하는 `Robot` 클래스를 만듭니다. 게임 개발자는 이러한 논리 및 데이터 캡슐화를 *엔터티로*지칭 합니다.

**MonoGame3D** 이식 가능한 클래스 라이브러리에 비어 있는 새 클래스 파일을 추가 합니다 (플랫폼별 ModelAndVerts. Android가 아님). 이름을 **로봇이** 로, **새로 만들기**를 클릭 합니다.

![](part3-images/image3.png "Name it Robot and click New")

`Robot` 클래스를 다음과 같이 수정 합니다.

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);

                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot` 코드는 기본적으로 `Model`를 그리기 위한 `Game1`의 코드와 동일 합니다. `Model` 로드 및 그리기에 대 한 검토는 [모델 사용에 대 한이 가이드](~/graphics-games/monogame/3d/part1.md)를 참조 하세요. 이제 `Game1`에서 `Model` 로드 및 렌더링 코드를 모두 제거 하 고 `Robot` 인스턴스로 바꿀 수 있습니다.

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }
}
```

이제 코드를 실행 하는 경우 주로 바닥에 그려진 로봇이 하나 뿐입니다.

![](part3-images/image4.png "If the code is run now, the app will display a scene with only one robot which is drawn mostly under the floor")

## <a name="moving-the-robot"></a>로봇 이동

이제 `Robot` 클래스가 있으므로 로봇이 이동 논리를 추가할 수 있습니다. 이 경우에는 게임 시간에 따라 원으로 원을 이동 하 게 됩니다. 문자는 일반적으로 입력 또는 인공 지능에 응답할 수 있지만 3D 위치 지정 및 회전을 탐색 하기 위한 환경을 제공 하기 때문에 실제로 게임을 구현 하는 경우에는 다소 실용적이 지 않습니다.

`Robot` 클래스 외부에서 필요한 유일한 정보는 현재 게임 시간입니다. `GameTime` 매개 변수를 사용 하는 `Update` 메서드를 추가 합니다. 이 `GameTime` 매개 변수는 로봇의 마지막 위치를 결정 하는 데 사용할 각도 변수를 증가 시키는 데 사용 됩니다.

먼저 `model` 필드 아래의 `Robot` 클래스에 각도 필드를 추가 합니다.

```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ...
```

 이제 `Update` 함수에서이 값을 증가 시킬 수 있습니다.

```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

`Game1.Update`에서 `Update` 메서드가 호출 되었는지 확인 해야 합니다.

```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
}
```

물론이 시점에서 각도 필드는 아무 작업도 수행 하지 않습니다 .이 필드를 사용 하는 코드를 작성 해야 합니다. 전용 방법으로 세계 `Matrix`를 계산할 수 있도록 `Draw` 메서드를 수정 합니다. 

```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);

            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
}
```

다음으로 `Robot` 클래스에 `GetWorldMatrix` 메서드를 구현 합니다.

```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;

    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
}
```

이 코드를 실행 하면 로봇이 원을 이동 합니다.

![](part3-images/image5.gif "Running this code results in the robot moving in a circle")

## <a name="matrix-multiplication"></a>행렬 곱하기

위의 코드는 `GetWorldMatrix` 메서드에서 `Matrix`를 만들어 로봇을 회전 시킵니다. `Matrix` 구조체에는 변환 (위치 설정), 회전 및 크기 조정 (크기 설정)에 사용할 수 있는 16 개의 float 값이 포함 되어 있습니다. `effect.World` 속성을 할당 하는 경우 기본 렌더링 시스템에는 어떤 드로잉 (즉, 꼭 짓 점에서 `Model` 또는 기 하 도형)의 위치, 크기 및 방향을 지정 하는 방법을 알려 줍니다. 

다행히 `Matrix` 구조체에는 일반적인 형식의 매트릭스 생성을 간소화 하는 여러 메서드가 포함 되어 있습니다. 위의 코드에 사용 되는 첫 번째은 `Matrix.CreateTranslation`입니다. 수치 용어 *변환은* 다른 위치 (예: 회전 또는 크기 조정) 없이 한 위치에서 다른 위치로 이동 하는 작업을 가리킵니다. 함수는 변환에 X, Y 및 Z 값을 사용 합니다. 위에서 아래로 장면을 볼 경우 `CreateTranslation` 메서드 (격리)는 다음을 수행 합니다.

![](part3-images/image6.png "The CreateTranslation method in isolation performs this action")

만드는 두 번째 행렬은 `CreateRotationZ` 매트릭스를 사용 하는 회전 행렬입니다. 이는 회전을 만드는 데 사용할 수 있는 세 가지 방법 중 하나입니다.

- `CreateRotationX`
- `CreateRoationY`
- `CreateRotationZ`

각 메서드는 지정 된 축을 중심으로 회전 하 여 회전 행렬을 만듭니다. 이 경우 Z 축에 대 한 회전은 "위쪽"을 가리킵니다. 다음은 축 기반 회전의 작동 방식을 시각화 하는 데 도움이 될 수 있습니다.

![](part3-images/image7.png "This can help visualize how axis-based rotation works")

또한 `Update` 메서드 호출로 인해 시간이 지남에 따라 증가 하는 각도 필드와 함께 `CreateRotationZ` 메서드를 사용 합니다. 그 결과 `CreateRotationZ` 방법을 통해 로봇이 시간 경과에 따라 원본 주위에 궤도를 설정 하 게 됩니다.

코드의 마지막 줄은 두 매트릭스를 하나로 결합 합니다.

```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

이를 행렬 곱셈 이라고 하며 일반적인 곱셈과 약간 다르게 작동 합니다. 곱하기 작업에서 숫자의 순서가 결과를 변경 하지 않는다는 *곱하기 상태의 교환 속성* 입니다. 즉, 3 \* 4는 4 \* 3과 같습니다. 행렬 곱셈은 교환 되지 않는다는 점에서 다릅니다. 즉, 위의 줄을 "translationMatrix 적용 하 여 모델을 이동한 다음, rotationMatrix를 적용 하 여 모든 항목을 회전"으로 읽을 수 있습니다. 위의 줄이 위치와 회전에 영향을 주는 방식을 다음과 같이 시각화할 수 있습니다.

![](part3-images/image8.png "A visualization pf the way that the above line affects the position and rotation")

행렬 곱셈의 순서가 결과에 영향을 줄 수 있는 방법을 이해 하려면 행렬 곱셈이 반전 된 다음을 고려 하십시오.

```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

위의 코드는 먼저 모델을 현재 위치에서 회전 한 다음 변환 합니다.

![](part3-images/image9.png "The code above would first rotate the model in-place, then translate it")

역 곱셈을 사용 하 여 코드를 실행 하는 경우 회전이 먼저 적용 된 후 모델의 방향에만 영향을 주며 모델의 위치는 동일 하 게 유지 된다는 것을 알 수 있습니다. 즉, 모델이 원위치에서 회전 합니다.

![](part3-images/image10.gif "The model rotates in place")

## <a name="creating-the-camera-entity"></a>카메라 엔터티 만들기

`Camera` 엔터티는 입력 기반 이동을 수행 하 고 `BasicEffect` 클래스에서 속성을 할당 하기 위한 속성을 제공 하는 데 필요한 모든 논리를 포함 합니다.

먼저 정적 카메라 (입력 기반 이동 없음)를 구현 하 고 기존 프로젝트에 통합 합니다. **MonoGame3D** 이식 가능한 클래스 라이브러리 (`Robot.cs`가 있는 동일한 프로젝트)에 새 클래스를 추가 하 고 이름을 **Camera**로 바꿉니다. 파일의 내용을 다음 코드로 바꿉니다.

```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;

                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

위의 코드는 `BasicEffect`에서 행렬을 할당 하는 `Game1` 및 `Robot`의 코드와 매우 비슷합니다. 

이제 기존 프로젝트에 새 `Camera` 클래스를 통합할 수 있습니다. 먼저 `Robot` 클래스를 수정 하 여 여러 중복 코드를 제거 하는 `Draw` 메서드에서 `Camera` 인스턴스를 사용 합니다. `Robot.Draw` 메서드를 다음으로 바꿉니다.

```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
}
```

다음으로 `Game1.cs` 파일을 수정 합니다.

```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;

            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
}
```

이전 버전의 `Game1`에 대 한 수정 (`// New camera code`으로 식별 됨)은 다음과 같습니다.

- `Game1`의 `Camera` 필드
- `Game1.Initialize`의 `Camera` 인스턴스화
- `Game1.Update`에서 `Camera.Update` 호출
- 이제 `Robot.Draw` `Camera` 매개 변수를 사용 합니다.
- 이제 `Game1.Draw` `Camera.ViewMatrix` 및 `Camera.ProjectionMatrix`를 사용 합니다.

## <a name="moving-the-camera-with-input"></a>입력으로 카메라 이동

지금까지 `Camera` 엔터티를 추가 했지만 런타임 동작을 변경 하는 작업을 수행 하지 않았습니다. 사용자가 다음 작업을 수행할 수 있도록 하는 동작을 추가 합니다.

- 화면 왼쪽을 터치 하 여 카메라를 왼쪽으로 전환 합니다.
- 화면 오른쪽을 터치 하 여 카메라를 오른쪽으로 전환 합니다.
- 화면 가운데를 터치 하 여 카메라를 앞으로 이동 합니다.

### <a name="making-lookat-relative"></a>상대적으로 살펴보겠습니다

먼저 `Camera` 클래스를 업데이트 하 여 `Camera` 향하는 방향을 설정 하는 데 사용 되는 `angle` 필드를 포함 합니다. 현재 `Camera`에는 `Vector3.Zero`에 할당 된 로컬 `lookAtVector`를 통해 향하는 방향이 결정 됩니다. 즉, `Camera`는 항상 출처를 확인 합니다. 카메라를 움직이면 카메라를 향하는 각도도 변경 됩니다.

![](part3-images/image11.gif "If the Camera moves, then the angle that the camera is facing will also change")

입력을 사용 하 여 `Camera`를 회전 하는 논리를 구현할 때까지 해당 위치에 관계 없이 `Camera` 동일한 방향으로 연결 하려고 합니다. 첫 번째 변경은 절대 위치를 확인 하는 대신 현재 위치를 기준으로 `lookAtVector` 변수를 조정 하는 것입니다.

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

이로 인해 세계를 똑바로 볼 `Camera` 있습니다. 초기 `position` 값이 `(0, 20, 10)`으로 수정 되었으므로 `Camera` X 축의 가운데에 있습니다. 게임을 실행 하면 다음이 표시 됩니다.

![](part3-images/image12.png "Running the game displays this view")

### <a name="creating-an-angle-variable"></a>각도 변수 만들기

`lookAtVector` 변수는 카메라에서 보는 각도를 제어 합니다. 현재 음수 Y 축을 표시 하 고 약간 하향 (`-.5f` Z 값에서) 하는 것으로 고정 되어 있습니다. `lookAtVector` 속성을 조정 하는 데 사용 되는 `angle` 변수를 만듭니다. 

이 연습의 이전 섹션에서는 행렬을 사용 하 여 개체를 그리는 방법을 회전할 수 있었습니다. 또한 `Vector3.Transform` 메서드를 사용 하 여 `lookAtVector`와 같은 벡터를 회전 하는 데 행렬을 사용할 수 있습니다. 

`angle` 필드를 추가 하 고 다음과 같이 `ViewMatrix` 속성을 수정 합니다.

```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    }
    ...
```

### <a name="reading-input"></a>입력 읽기

이제 `Camera` 엔터티를 위치 및 각도 변수를 통해 완전 하 게 제어할 수 있습니다. 입력에 따라 변경 해야 합니다.

먼저 `TouchPanel` 상태를 가져와 사용자가 화면에서 접촉 하는 위치를 찾습니다. `TouchPanel` 클래스를 사용 하는 방법에 대 한 자세한 내용은 [TOUCHPANEL API 참조](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel)를 참조 하세요.

사용자가 왼쪽 3에서 접촉 하는 경우 `angle` 값을 조정 하 여 `Camera` 왼쪽으로 회전 하 고, 사용자가 오른쪽 세 번째에서 접촉 하는 경우 다른 방법을 회전 합니다. 사용자가 화면의 가운데 1/3에서 접촉 하는 경우 `Camera`를 앞으로 이동 합니다.

먼저 `Camera.cs`에서 `TouchPanel` 및 `TouchCollection` 클래스를 한정 하는 using 문을 추가 합니다.

```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

다음으로 `Update` 메서드를 수정 하 여 터치 패널을 읽고 `angle` 및 `position` 변수를 적절 하 게 조정 합니다.

```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
}
```

이제 `Camera`이 터치식 입력에 응답 합니다.

![](part3-images/image1.gif "Now the Camera will respond to touch input")

Update 메서드는 접촉 컬렉션을 반환 하는 `TouchPanel.GetState`를 호출 하 여 시작 합니다. `TouchPanel.GetState`는 여러 개의 터치 지점만 반환할 수 있지만 간단 하 게 하기 위해 첫 번째 지점만 걱정 합니다.

사용자가 화면을 터치 하는 경우 코드는 첫 번째 터치가 화면의 왼쪽, 가운데 또는 오른쪽에 있는지 확인 합니다. 왼쪽 및 오른쪽 1/3은 `TotalSeconds` 값에 따라 `angle` 변수를 늘리거나 줄여서 카메라를 회전 시킵니다. 따라서 게임은 프레임 요금에 관계 없이 동일 하 게 작동 합니다.

사용자가 화면의 세 번째 중심에 닿을 경우 카메라가 앞으로 진행 됩니다. 이는 먼저 음의 Y 축을 가리키는 것으로 정의 된 다음 `Matrix.CreateRotationZ` 및 `angle` 값을 사용 하 여 만든 행렬에 의해 회전 되는 전방 벡터를 가져와 수행 합니다. 마지막으로 `forwardVector` `unitsPerSecond` 계수를 사용 하 여 `position`에 적용 됩니다.

## <a name="summary"></a>요약

이 연습에서는 `Matrices` 및 `BasicEffect.World` 속성을 사용 하 여 3D 공간에서 `Models`를 이동 하 고 회전 하는 방법을 설명 합니다. 이러한 형태의 움직임은 3D 게임에서 개체를 이동 하기 위한 기초를 제공 합니다. 이 연습에서는 위치와 각도에서 전 세계를 볼 수 있도록 `Camera` 엔터티를 구현 하는 방법에 대해서도 설명 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame API 링크](http://www.monogame.net/documentation/?page=api)
- [완료 된 프로젝트 (샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/monogame3dcamera)
