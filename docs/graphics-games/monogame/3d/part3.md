---
title: "3D MonoGame 좌표"
description: "3D 게임 개발에 중요 한 단계는 3D 좌표계를 이해 합니다. MonoGame 다양 한 위치, 태도 및 3D 공간에서 개체 크기 조정에 대 한 클래스를 제공 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: A4130995-48FD-4E2E-9C2B-ADCEFF35BE3A
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: 861d47d001c10c14a0294536c6122cafb33a93ac
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="3d-coordinates-in-monogame"></a>3D MonoGame 좌표

_3D 게임 개발에 중요 한 단계는 3D 좌표계를 이해 합니다. MonoGame 다양 한 위치, 태도 및 3D 공간에서 개체 크기 조정에 대 한 클래스를 제공 합니다._

3D 게임 개발 3D 좌표계에서 개체를 조작 하는 방법 이해를 해야 합니다. 이 연습에서는 시각적 개체 (특히 모델)을 조작 하는 방법을 설명 합니다. 3D 카메라 클래스를 만들려면 모델을 제어 개념에 빌드합니다.

제시 된 개념 선형 대 수에서 시작 하지만 강력한 수학 배경 없는 모든 사용자가 자신의 게임에서 이러한 개념을 적용할 수 있도록 실용적인 방법이 이동 합니다.

다음 항목 다루는 것 했습니다.

 - 프로젝트 만들기
 - 로봇 엔터티 만들기
 - 로봇 엔터티 이동
 - 매트릭스 곱
 - 카메라 엔터티 만들기
 - 카메라를 입력으로 이동

완료 되 면 원과 터치식 입력 하 여 제어할 수 있는 카메라에서 이동 하는 로봇으로 프로젝트를 봅시다.

![](part3-images/image1.gif "완료 되 면 응용 프로그램 원과 터치식 입력 하 여 제어할 수 있는 카메라에서 이동 하는 로봇으로 프로젝트를 포함 됩니다.")


# <a name="creating-a-project"></a>프로젝트 만들기

이 연습에서는 3D 공간에서 개체를 이동에 중점을 둡니다. 렌더링 모델 및 꼭 짓 점 배열에 대 한 프로젝트 먼저 [있는 여기](https://developer.xamarin.com/samples/mobile/ModelsAndVertsMG/)합니다. 를 다운로드 한 후 압축을 풀 및 실행 되 고 다음를 확인할 수 있도록 프로젝트를 엽니다.

![](part3-images/image2.png "를 다운로드 한 후 압축 해제 하 고 실행 되 고이 보기를 표시할 수 있도록 프로젝트를 엽니다")


# <a name="creating-a-robot-entity"></a>로봇 엔터티 만들기

이 로봇 주위를 이동 하기 시작 하기 전에 만듭니다는 `Robot` 그리기 및 이동에 대 한 논리를 포함 하는 클래스입니다. 이 캡슐화 논리로 데이터의 게임 개발자 참조는 *엔터티*합니다.

새 빈 클래스 파일을 추가 **MonoGame3D** 이식 가능한 클래스 라이브러리 (플랫폼 특정 ModelAndVerts.Android 제외). 이름 지정 it * * 로봇 * *를 클릭 하 고 **새로**:

![](part3-images/image3.png "로봇 이름을 지정 하 고 새로 만들기를 클릭 합니다.")

수정 된 `Robot` 클래스를 다음과 같이 합니다.


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;
using Microsoft.Xna.Framework.Content;

namespace MonoGame3D
{
    public class Robot
    {
        Model model;

        public void Initialize(ContentManager contentManager)
        {
            model = contentManager.Load<Model> ("robot");

        }

        // For now we'll take these values in, eventually we'll
        // take a Camera object
        public void Draw(Vector3 cameraPosition, float aspectRatio)
        {
            foreach (var mesh in model.Meshes)
            {
                foreach (BasicEffect effect in mesh.Effects)
                {
                    effect.EnableDefaultLighting ();
                    effect.PreferPerPixelLighting = true;

                    effect.World = Matrix.Identity; 
                    var cameraLookAtVector = Vector3.Zero;
                    var cameraUpVector = Vector3.UnitZ;

                    effect.View = Matrix.CreateLookAt (
                        cameraPosition, cameraLookAtVector, cameraUpVector);
                        
                    float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                    float nearClipPlane = 1;
                    float farClipPlane = 200;

                    effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                        fieldOfView, aspectRatio, nearClipPlane, farClipPlane);


                }

                // Now that we've assigned our properties on the effects we can
                // draw the entire mesh
                mesh.Draw ();
            }
        }
    }
}
```

`Robot` 코드에서 기본적으로 동일한 코드는 `Game1` 그리기에 대 한는 `Model`합니다. 에 검토에 대 한 `Model` 참조를 로드 하 고 그리기 [모델 작업에이 가이드](~/graphics-games/monogame/3d/part1.md)합니다. 모든 지금 제거할 수 있습니다는 `Model` 로드 하 고 코드에서 렌더링 `Game1`, 바꿉니다는 `Robot` 인스턴스:


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        Vector3 cameraPosition = new Vector3(15, 10, 10);

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            float aspectRatio = 
                graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
            robot.Draw (cameraPosition, aspectRatio);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
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
    }                                          
}
```

코드 실행 하는 경우 대부분 바닥 아래에 표시 되는 하나의 로봇으로 장면을 해야는 이제 합니다.

![](part3-images/image4.png "이제는 코드를 실행 하면 표시 장면이 바닥 대부분 아래에 표시 되는 하나의 로봇으로")


# <a name="moving-the-robot"></a>로봇 이동

이제는 `Robot` 클래스 로봇을 이동 논리를 추가할 수 있습니다. 이 경우 게임 시간에 따라 원에 이동 하는 로봇을 확인 합니다. 문자 수 일반적으로 입력에 응답 또는 인공 지능 하지만 3D 위치 및 회전을 탐색 하기 위한 환경을 제공 하므로 실제 게임에 대 한 다소 비현실적 구현입니다.

외부에서 사용 해야 하는 유일한 정보는 `Robot` 클래스는 현재 게임 시간입니다. 추가 `Update` 걸립니다 메서드는 `GameTime` 매개 변수입니다. 이 `GameTime` 매개 변수에서는 로봇에 대 한 최종 위치를 결정 하는 각도 변수를 증가 시키는 데 사용 됩니다.

첫째, 각도 필드를 추가 하겠습니다는 `Robot` 아래 클래스는 `model` 필드:


```csharp
public class Robot
{
    public Model model;

    // new code:
    float angle;
    ... 
```

 이 값을 증가 수 이제는 `Update` 함수:


```csharp
public void Update(GameTime gameTime)
{
    // TotalSeconds is a double so we need to cast to float
    angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
}
```

다음 사항을 확인 해야는 `Update` 에서 메서드는 `Game1.Update`:


```csharp
protected override void Update(GameTime gameTime)
{
    robot.Update (gameTime);
    base.Update(gameTime);
} 
```

물론,이 시점에서 각도 필드는 아무 작업도 수행-를 사용 하는 코드를 작성 해야 합니다. 수정 합니다는 `Draw` 메서드 전 세계 계산 수 있도록 `Matrix` 전용 방법에서: 


```csharp
public void Draw(Vector3 cameraPosition, float aspectRatio)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            // We’ll be doing our calculations here...
            effect.World = GetWorldMatrix();

            var cameraLookAtVector = Vector3.Zero;
            var cameraUpVector = Vector3.UnitZ;

            effect.View = Matrix.CreateLookAt (
                cameraPosition, cameraLookAtVector, cameraUpVector);
                
            float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
            float nearClipPlane = 1;
            float farClipPlane = 200;

            effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
        }

        mesh.Draw ();
    }
} 
```

다음으로 구현 된 `GetWorldMatrix` 에서 메서드는 `Robot` 클래스:


```csharp
Matrix GetWorldMatrix()
{
    const float circleRadius = 8;
    const float heightOffGround = 3;
    
    // this matrix moves the model "out" from the origin
    Matrix translationMatrix = Matrix.CreateTranslation (
        circleRadius, 0, heightOffGround);

    // this matrix rotates everything around the origin
    Matrix rotationMatrix = Matrix.CreateRotationZ (angle);

    // We combine the two to have the model move in a circle:
    Matrix combined = translationMatrix * rotationMatrix;

    return combined;
} 
```

원 안에 이동 로봇이이 코드를 실행 한 결과를 반환 합니다.

![](part3-images/image5.gif "원 안에 이동 로봇에이 코드의 결과 실행 합니다.")


# <a name="matrix-multiplication"></a>매트릭스 곱

위의 코드를 만들어서 로봇 회전 하는 `Matrix` 에 `GetWorldMatrix` 메서드. `Matrix` (집합 위치)를 변환, 회전 및 크기를 사용할 수 있는 16 부동 소수점 값을 포함 하는 구조체 (크기 설정). 할당 했습니다는 `effect.World` 속성에서는 한다는 시스템 위치, 크기 및 무엇이 든 방향을 지정 하는 방법을 렌더링 하는 기본 소모할 수 하는 문제가 발생 했습니다 (한 `Model` 꼭지점에서 기 하 도형 또는). 

다행히는 `Matrix` 구조체 다양 한 일반적인 유형의 행렬 만들기 과정을 간소화 하는 메서드를 포함 합니다. 위의 코드에서 사용 되는 첫 번째는 `Matrix.CreateTranslation`합니다. 수학 용어인 *번역* 참조 하는 지점 (또는 경우에 모델)으로 인해 되는 작업 (예: 회전 하거나 크기 조정) 다른 수정 하지 않고 다른 위치로 이동 합니다. 번역에 대 한 X, Y 및 Z 값을 사용 하는 함수입니다. 우리의 장면 위에서 아래로에서 보고 하는 경우 우리의 `CreateTranslation` 메서드 (격리) 다음 작업을 수행 합니다.

![](part3-images/image6.png "이 작업을 수행 하는 격리 된 상태에서 CreateTranslation 메서드")

작성 하는 두 번째 행렬은 회전 매트릭스를 사용 하는 `CreateRotationZ` 행렬입니다. 이 회전을 만드는 데 사용할 수 있는 세 가지 방법 중 하나입니다.

 - `CreateRotationX`
 - `CreateRoationY`
 - `CreateRotationZ`

각 메서드는 지정 된 축에 대 한 회전 하 여 회전 행렬을 만듭니다. 경우에 "위쪽" 가리키는 Z 축에 대 한 회전 했습니다. 다음 회전 축 기반를 시각화할 수 작동 합니다.

![](part3-images/image7.png "회전 축 기반를 시각화할 수 있습니다이 작동")

또한 사용 하 여는 `CreateRotationZ` 로 인해 시간이 지남에 따라 증가 각도 필드와 메서드 우리의 `Update` 메서드를 호출 합니다. 결과 `CreateRotationZ` 메서드를 사용 하면 시간이 지남에 따라 원점 궤도를 우리의 로봇 합니다.

두 행렬을 결합 하는 코드의 마지막 줄:


```csharp
Matrix combined = translationMatrix * rotationMatrix;
```

이 일반 곱하기와 약간 다를 작동 하는 매트릭스 곱 라고 합니다. *곱하기의 가환 적 속성이* 상태 곱하기 연산에서 숫자의 순서는 결과 변경 되지 않습니다. 즉, 3 * 4는 4 * 3에 해당 합니다. 행렬 곱 달리 하은 누적 되지 않습니다. 즉, "적용 하 여 모델 translationMatrix 한 다음 모든 항목은 rotationMatrix 적용 하 여 회전" 위의 줄으로 읽을 수 있습니다. 위의 줄 영향을 받는 있다는 위치 및 회전 다음과 같이 하는 방법을 시각적으로 나타낼 수 있습니다.

![](part3-images/image8.png "시각화 pf는 위의 코드에 영향을 줍니다 위치 및 회전 하는 방법")

행렬 곱하기의 순서가 결과 미치는 영향을 이해 하려면 매트릭스 곱 반전 된 다음을 고려 합니다.


```csharp
Matrix combined = rotationMatrix * translationMatrix;
```

먼저 모델에 내부에서 회전 한 다음 위의 코드는 번역:

![](part3-images/image9.png "위의 코드는 먼저 모델에 내부에서 회전 다음 변환")

반전 된 곱하기 코드를 실행 하는 경우 모델의 방향에만 영향을 줍니다 회전 먼저 적용, 이후 및 모델의 위치는 동일 하 게 유지 알 수 합니다. 즉, 모델 위치에 회전.

![](part3-images/image10.gif "원위치에서 모델 회전")


# <a name="creating-the-camera-entity"></a>카메라 엔터티 만들기

`Camera` 모든 속성에 할당 하기 위한 속성을 제공 하 고 입력 기반 이동을 수행 하는 데 필요한 논리 엔터티에 포함 됩니다는 `BasicEffect` 클래스입니다.

먼저 정적 카메라 (입력 기반 이동 없이) 구현 알아보고 우리의 기존 프로젝트에 통합 하겠습니다. 에 새 클래스 추가 **MonoGame3D** 이식 가능한 클래스 라이브러리 (으로 프로젝트를 동일한 `Robot.cs`) 하 고 이름을 **카메라**합니다. 파일의 내용을 다음 코드로 바꿉니다.


```csharp
using System;
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Camera
    {
        // We need this to calculate the aspectRatio
        // in the ProjectionMatrix property.
        GraphicsDevice graphicsDevice;

        Vector3 position = new Vector3(15, 10, 10);

        public Matrix ViewMatrix
        {
            get
            {
                var lookAtVector = Vector3.Zero;
                var upVector = Vector3.UnitZ;

                return Matrix.CreateLookAt (
                    position, lookAtVector, upVector);
            }
        }

        public Matrix ProjectionMatrix
        {
            get
            {
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                float nearClipPlane = 1;
                float farClipPlane = 200;
                float aspectRatio = graphicsDevice.Viewport.Width / (float)graphicsDevice.Viewport.Height;
                
                return Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);
            }
        }

        public Camera(GraphicsDevice graphicsDevice)
        {
            this.graphicsDevice = graphicsDevice;
        }

        public void Update(GameTime gameTime)
        {
            // We'll be doing some input-based movement here
        }
    }
}
```

위의 코드는 코드에서 매우 비슷합니다 `Game1` 및 `Robot` 매트릭스에서 할당는 `BasicEffect`합니다. 

에서는 새 통합할 수 이제 `Camera` 기존 프로젝트에 클래스입니다. 먼저 수정 합니다는 `Robot` 되려면 클래스는 `Camera` 인스턴스 해당` Draw `많은 중복 코드를 제거 하는 메서드. 대체는 `Robot.Draw` 메서드를 다음:


```csharp
public void Draw(Camera camera)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;

            effect.World = GetWorldMatrix();
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;
        }

        mesh.Draw ();
    }
} 
```

다음으로 수정 된 `Game1.cs` 파일:


```csharp
using Microsoft.Xna.Framework;
using Microsoft.Xna.Framework.Graphics;

namespace MonoGame3D
{
    public class Game1 : Game
    {
        GraphicsDeviceManager graphics;

        VertexPositionNormalTexture[] floorVerts;

        BasicEffect effect;

        Texture2D checkerboardTexture;

        // New camera code
        Camera camera;

        Robot robot;

        public Game1()
        {
            graphics = new GraphicsDeviceManager(this);
            graphics.IsFullScreen = true;
                        
            Content.RootDirectory = "Content";
        }

        protected override void Initialize ()
        {
            floorVerts = new VertexPositionNormalTexture[6];

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

            robot = new Robot ();
            robot.Initialize (Content);

            // New camera code
            camera = new Camera (graphics.GraphicsDevice);

            base.Initialize ();
        }

        protected override void LoadContent()
        {
            using (var stream = TitleContainer.OpenStream ("Content/checkerboard.png"))
            {
                checkerboardTexture = Texture2D.FromStream (this.GraphicsDevice, stream);
            }
        }

        protected override void Update(GameTime gameTime)
        {
            robot.Update (gameTime);
            // New camera code
            camera.Update (gameTime);
            base.Update(gameTime);
        }

        protected override void Draw(GameTime gameTime)
        {
            GraphicsDevice.Clear(Color.CornflowerBlue);

            DrawGround ();

            // New camera code
            robot.Draw (camera);

            base.Draw(gameTime);
        }

        void DrawGround()
        {
            // New camera code
            effect.View = camera.ViewMatrix;
            effect.Projection = camera.ProjectionMatrix;

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
    }
} 
```

수정 내용을 `Game1` 이전 버전에서 (로 식별 되는 `// New camera code` )은:

 - `Camera` 필드에 `Game1`
 - `Camera` 에 대 한 인스턴스화 `Game1.Initialize`
 - `Camera.Update` 호출 `Game1.Update`
 - `Robot.Draw` 이제는 `Camera` 매개 변수
 - `Game1.Draw` 이제 사용 `Camera.ViewMatrix` 및 `Camera.ProjectionMatrix`


# <a name="moving-the-camera-with-input"></a>카메라를 입력으로 이동

추가한 지금까지 `Camera` 엔터티 런타임 동작에 대해 어떠한 작업도 수행 하지 않은 있지만 합니다. 사용자는 동작 추가 하려면:

 - 터치 화면이 왼쪽 카메라의 왼쪽
 - 터치 화면이 오른쪽 카메라의 오른쪽
 - 카메라를 앞으로 이동 하려면 화면 중앙 터치


## <a name="making-lookat-relative"></a>상대 lookAt 만들기

먼저 업데이트 하는 중는 `Camera` 포함 하도록 클래스는 `angle` 필드 하는 데 사용 됩니다 하는 방향을 설정는 `Camera` 됩니다. 현재 우리의 `Camera` 로컬을 통해 연결 되는 방향을 결정 `lookAtVector`에 할당 됨 `Vector3.Zero`합니다. 즉, 우리의 `Camera` 항상 원점에서 찾습니다. 카메라 이동 하는 경우 카메라 향하도록 되는 각도도 변경 됩니다.

![](part3-images/image11.gif "카메라 이동 하는 경우 다음 카메라 향하도록 되는 각도도 변경 합니다.")

원하는 `Camera` – 해당 위치에 관계 없이 같은 방향 이상 이해가를 회전 하기 위한 논리를 구현 될 때까지` Camera `입력을 사용 하 여 합니다. 첫 번째 변경 사항을 조정할 수 있습니다는 `lookAtVector` 절대 위치에 찾고 대신이 현재 위치를 기반으로 할 변수:


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    // Let's start at X = 0 so we're looking at things head-on
    Vector3 position = new Vector3(0, 20, 10);

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```

이 인해는 `Camera` 세계 일직선으로 보고 합니다. 초기 `position` 값이 수정 된 `(0, 20, 10)` 하므로 `Camera` 가운데 X 축에 표시 됩니다. 게임 디스플레이 실행 합니다.

![](part3-images/image12.png "이 보기를 표시 하는 게임을 실행 합니다.")


## <a name="creating-an-angle-variable"></a>각도 변수 만들기

`lookAtVector` 변수를 보는 우리의 카메라 각도 제어 합니다. 음수 Y 축을 보려는 고정 및 약간 아래로 기울어진 현재 (에서 `-.5f` Z 값). 만들겠습니다는 `angle` 조정에 사용 되는 변수는 `lookAtVector` 속성입니다. 

이 연습의 이전 섹션에서 개체를 그리는 방법을 회전 행렬을 사용할 수 있도록 하는 것이 했죠. 또한 행렬 같은 벡터를 회전 하려면 사용할 수는 `lookAtVector` 를 사용 하는 `Vector3.Transform` 메서드. 

추가 `angle` 필드 및 수정 된 `ViewMatrix` 속성을 다음과 같이:


```csharp
public class Camera
{
    GraphicsDevice graphicsDevice;

    Vector3 position = new Vector3(0, 20, 10);

    float angle;

    public Matrix ViewMatrix
    {
        get
        {
            var lookAtVector = new Vector3 (0, -1, -.5f);
            // We'll create a rotation matrix using our angle
            var rotationMatrix = Matrix.CreateRotationZ (angle);
            // Then we'll modify the vector using this matrix:
            lookAtVector = Vector3.Transform (lookAtVector, rotationMatrix);
            lookAtVector += position;

            var upVector = Vector3.UnitZ;

            return  Matrix.CreateLookAt (
                position, lookAtVector, upVector);
        }
    } 
    ...
```


## <a name="reading-input"></a>입력을 읽기

우리의 `Camera` 엔터티 이제 완전히 제어할 수의 위치와 각도 변수를 통해 – त ु म 입력에 따라 변경 합니다.

첫째, 살펴보겠습니다는 `TouchPanel` 상태는 사용자가 여 화면을 터치 하는 위치를 찾을 수 있습니다. 사용 하 여 대 한 자세한 내용은 `TouchPanel` 클래스를 참조 하십시오. [TouchPanel API 참조](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Input_Touch_TouchPanel)합니다.

사용자가 왼쪽에 연결 되어 있으면를 조정 합니다는 `angle` 값 이므로 `Camera` 왼쪽으로 회전 하 고, 다른 방법으로 순환할 수 사용자 오른쪽 세 번째에 연결 하는 경우. 사용자는 화면의 가운데 세 번째 연결 되어 있으면 살펴보고는 `Camera` 앞으로 합니다.

먼저, 사용 하는 추가 정규화 하는 문에 `TouchPanel` 및 `TouchCollection` 에 있는 클래스 `Camera.cs`:


```csharp
using Microsoft.Xna.Framework.Input.Touch; 
```

다음으로 수정 된 `Update` 터치 패널 읽고 조정 메서드는 `angle` 및 `position` 변수 적절 하 게:


```csharp
public void Update(GameTime gameTime)
{
    TouchCollection touchCollection = TouchPanel.GetState();

    bool isTouchingScreen = touchCollection.Count > 0;
    if (isTouchingScreen)
    {
        var xPosition = touchCollection [0].Position.X;

        float xRatio = xPosition / (float)graphicsDevice.Viewport.Width;

        if (xRatio < 1 / 3.0f)
        {
            angle += (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
        else if (xRatio < 2 / 3.0f)
        {
            var forwardVector = new Vector3 (0, -1, 0);

            var rotationMatrix = Matrix.CreateRotationZ (angle);
            forwardVector = Vector3.Transform (forwardVector, rotationMatrix);

            const float unitsPerSecond = 3;

            this.position += forwardVector * unitsPerSecond *
                (float)gameTime.ElapsedGameTime.TotalSeconds ;
        }
        else
        {
            angle -= (float)gameTime.ElapsedGameTime.TotalSeconds;
        }
    }
} 
```

이제는 `Camera` 터치식 입력에 응답 합니다.

![](part3-images/image1.gif "카메라의 터치식 입력에 응답 하는 이제")

Update 메서드를 호출 하 여 시작 `TouchPanel.GetState`, 작업의 컬렉션을 반환 하는 합니다. 하지만 `TouchPanel.GetState` 여러 터치 포인트를 반환할 수 편의 위해 첫 번째 걱정만 합니다.

사용자가 화면을 터치 하는 코드는 첫 번째 터치 왼쪽, 가운데 또는 오른쪽 화면 세 번째에 포함 되는지 여부를 확인 합니다. 왼쪽 및 오른쪽 3가 증가 하거나 감소 하 여 카메라를 회전는 `angle` 에 따라 변수는 `TotalSeconds` (있도록 게임 프레임 속도 관계 없이 동일 하 게 작동) 값입니다.

사용자 중심을 세 번째 연결 되어 경우 화면의 다음 카메라 앞으로 이동 합니다. 이 위해서는 먼저 처음 음수 Y 축을 가리키게로 정의 다음 사용 하 여 만든 행렬으로 회전 정방향 벡터 `Matrix.CreateRotationZ` 및 `angle` 값입니다. 마지막으로 `forwardVector` 적용할 `position` 를 사용 하 여는 `unitsPerSecond` 계수입니다.


# <a name="summary"></a>요약

이 연습에서는 이동 및 회전 하는 방법을 `Models` 3d에서를 사용 하 여 공간 `Matrices` 및 `BasicEffect.World` 속성입니다. 이러한 형태의 이동 3D 게임에서 개체를 이동 하기 위한 기본 사항을 제공 합니다. 또한이 연습에서는 구현 하는 방법에 설명 된 `Camera` 엔터티는 전 세계 모든 위치와 각도에서 보기 위한 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame API 링크](http://www.monogame.net/documentation/?page=api)
- [완료 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/monodroid/MonoGame3DCamera/)
