---
title: 모델 클래스를 사용 하 여
description: 모델 클래스는 3D 그래픽을 렌더링 하는 기존의 방법에 비해 복잡 한 3D 개체 렌더링 크게 단순화 합니다. 모델 개체는 사용자 지정 코드 없이 콘텐츠를 간단 하 게 통합할 수 있는 콘텐츠 파일에서 생성 됩니다.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: e108bc6098d2c754428bf35e7312ebdc89459501
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783469"
---
# <a name="using-the-model-class"></a>모델 클래스를 사용 하 여

_모델 클래스는 3D 그래픽을 렌더링 하는 기존의 방법에 비해 복잡 한 3D 개체 렌더링 크게 단순화 합니다. 모델 개체는 사용자 지정 코드 없이 콘텐츠를 간단 하 게 통합할 수 있는 콘텐츠 파일에서 생성 됩니다._

MonoGame API에 포함 되어는 `Model` 렌더링을 수행 하 고 콘텐츠 파일에서 데이터를 저장 하는 데 사용할 수 있는 클래스가 로드 합니다. 모델 파일 (예: 실선 색이 지정 된 삼각형) 매우 간단 하거나 질감 및 조명를 포함 하 여 복잡 한 렌더링에 대 한 정보를 포함 될 수 있습니다.

이 연습에서는 [로봇의 3D 모델](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 다음에 다룹니다.

- 새 게임 프로젝트 시작
- 모델 및 해당 질감에 대 한 XNBs 만들기
- 게임 프로젝트에는 XNBs를 포함합니다.
- 3D 모델 그리기
- 여러 모델을 그리기

완료 되 면이 프로젝트는 다음과 같이 표시 됩니다.

![6 개의 로봇을 보여 주는 완성 된 샘플](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>빈 게임 프로젝트 만들기

처음 호출 MonoGame3D 게임 프로젝트를 설정 해야 합니다. 새 MonoGame 프로젝트를 만드는 방법에 대 한 정보를 참조 하십시오. [크로스 플랫폼 Monogame 프로젝트를 만드는 방법에이 연습에서는](~/graphics-games/monogame/introduction/part1.md)합니다.

넘어가기 전에 म 프로젝트가 열리고 올바르게 배포를 확인 해야 합니다. 한 번 배포 빈 파란색 화면을 참조 해야 했습니다.

![빈 파란색 게임 화면](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>게임 프로젝트에는 XNBs를 포함합니다.

.Xnb 파일 형식이 작성 된 콘텐츠에 대 한 표준 확장명입니다 (하 여 작성 된 콘텐츠는 [MonoGame 파이프라인 도구](http://www.monogame.net/documentation/?page=Pipeline)). 모든 기본 제공된 콘텐츠 소스 파일 (즉,이 모델에서.fbx 파일) 및 대상 파일 (.xnb 파일)에 있습니다. .Fbx 형식은 같은 응용 프로그램에서 만들 수 있는 일반적인 3D 모델 형식 [마 야](http://www.autodesk.com/products/maya/overview) 및 [블렌더](http://www.blender.org/)합니다. 

`Model` 3D 기 하 도형 데이터를 포함 하는 디스크에서.xnb 파일을 로드 하 여 클래스를 생성할 수 있습니다.   이.xnb 파일 콘텐츠 프로젝트를 통해 생성 됩니다. Monogame 템플릿에 우리의 콘텐츠 폴더에 확장명.mgcp) (포함 하는 콘텐츠 프로젝트를 자동으로 포함 됩니다. MonoGame 파이프라인 도구에 대 한 자세한 내용은 참조 하십시오.는 [콘텐츠 파이프라인 가이드](~/graphics-games/cocossharp/content-pipeline/index.md)합니다.

이 가이드 MonoGame 파이프라인을 사용 하 여 건너뛸 수에 대 한 도구를 사용 합니다는 합니다. 여기에 포함 된 XNB 파일입니다. 고 합니다. XNB 파일 플랫폼 마다 달라, 사용 하는 어떤 플랫폼에 대 한 올바른 XNB 파일 집합을 사용 해야 합니다.

압축을 푸는 것 우리는 [Content.zip 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 게임에서.xnb 포함 된 파일을 사용할 수 있도록 합니다. Android 프로젝트에서 작업 하는 경우 마우스 오른쪽 단추로 클릭는 **자산** 폴더에는 **WalkingGame.Android** 프로젝트. IOS 프로젝트에서 작업 하는 경우 마우스 오른쪽 단추로 클릭는 **WalkingGame.iOS** 프로젝트. 선택 **추가-> 파일을 추가 중...**  에서 작업 하는 플랫폼에 대 한 폴더에.xnb 파일을 모두 선택 하 고 있습니다.

두 파일에는 이제이 프로젝트의 일부 여야 합니다.

![솔루션 탐색기 콘텐츠 폴더가 xnb 파일](part1-images/xnbsinxs.png)

Mac 용 visual Studio의 빌드 작업을 새로 추가 된 XNBs 설정 되지 않을 수 있습니다. Ios의 경우 파일 및 선택의 각 단추로 **빌드 작업 BundleResource->** 합니다. Android의 경우 마우스 오른쪽 단추로 클릭 파일 및 선택의 각 **빌드 작업 AndroidAsset->** 합니다.

## <a name="rendering-a-3d-model"></a>3D 모델 렌더링

화면에 나타나는 모델을 참조 하는 데 필요한 마지막 단계는 로드 하 고 그리기 코드를 추가 하는 것입니다. 특히, 우리 수행한 다음:

- 정의 `Model` 인스턴스 우리의 `Game1` 클래스
- 로드는 `Model` 인스턴스 `Game1.LoadContent`
- 그리기는 `Model` 인스턴스 `Game1.Draw`

대체는 `Game1.cs` 코드 파일 (에 있는 **WalkingGame** PCL)를 다음:

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

실행 하는 경우이 코드를 볼 수 있겠지만, 모델 화면:

![화면에 표시 되는 모델](part1-images/image8.png "이 코드를 실행 하는 경우 모델 화면 표시 됩니다")

### <a name="model-class"></a>모델 클래스

`Model` 클래스는 (예: 파일.fbx) 콘텐츠 파일에서 3D 렌더링을 수행 하기 위한 핵심 클래스입니다. 3D geometry 질감 참조를 포함 하 여 렌더링을 위해 필요한 정보를 포함 하 고 `BasicEffect` 위치, 조명, 및 카메라 값을 제어 하는 인스턴스만 있습니다.

`Model` 자체 클래스에이 가이드의 뒷부분에 나오는 보여 주 겠지만 단일 모델 인스턴스를 여러 위치에서 렌더링 될 수 있으므로 위치 지정에 대 한 변수를 직접가 없습니다.

각 `Model` 는 하나 이상의 구성 `ModelMesh` 를 통해 노출 하는 인스턴스는 `Meshes` 속성입니다. 고려할 수 있지만 `Model` 단일에 게임으로 개체 (예: 로봇 또는 자동차) 각 `ModelMesh` 서로 다른에 그릴 수 `BasicEffect` 값입니다. 예를 들어 개별 메시 파트 로봇 또는 자동차, 바퀴 다리 나타낼 수 있습니다 및 할당 될 수 있습니다는 `BasicEffect` 바퀴 스핀 또는 다리 값을 이동 합니다. 

### <a name="basiceffect-class"></a>BasicEffect 클래스

`BasicEffect` 클래스 렌더링 옵션을 제어 하기 위한 속성을 제공 합니다. 처음 수정 하기에 `BasicEffect` 호출 하는 것은 `EnableDefaultLighting` 메서드. 이렇게 하면 특히 편리 있는지를 확인 하는 것에 대 한 기본 조명 이름에서 알 수 있듯이 한 `Model` 게임 예상 대로 표시 됩니다. 에서는 주석으로 처리 하는 경우는 `EnableDefaultLighting` 해당 개의 질감 같지만 없는 음영 또는 반사 광선을 렌더링 하는 모델을 보면 호출:

```csharp
//effect.EnableDefaultLighting ();
```

![해당 개의 질감 같지만 없는 음영 또는 반사 광선 렌더링 모델](part1-images/image9.png "해당 개의 질감 같지만 없는 음영 또는 반사 광선 렌더링 모델")

`World` 위치, 회전 및는 모델의 배율이 조정 속성을 사용할 수 있습니다. 사용 하 여 위의 코드는 `Matrix.Identity` 됨을 의미 하는 값은 `Model` 게임 내에서 지정 된 대로 정확 하 게.fbx 파일에서 렌더링 됩니다. 에서는 행렬 및 3D 좌표에서 더 자세하게에서 담당할 수 합니다 [3 부](~/graphics-games/monogame/3d/part3.md), 예를 들어의 위치를 변경할 수 있습니다는 `Model` 변경 하 여는 `World` 속성을 다음과 같이 합니다.

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

이 코드 3 전체 단위에 의해 이동 된 개체에 발생 합니다.

![이 코드 3 전체 단위에 의해 이동 된 개체에서 발생](part1-images/image10.png "3 전체 단위에 의해 이동 된 개체에이 코드에서는 발생")

에 할당 된 마지막 두 속성은 `BasicEffect` 는 `View` 및 `Projection`합니다. 3D 카메라를 담당할 수 합니다 म [3 부](~/graphics-games/monogame/3d/part3.md), 예를 들어, 로컬 변경 하 여 카메라의 위치를 수정할 수 있습니다 하지만 `cameraPosition` 변수:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

볼 수 있습니다 카메라 추가 역방향으로 이동 되었습니다 그 결과 `Model` 관점으로 인해 더 작은 표시:

![카메라 관점으로 인해 더 작은 표시 되는 모델에 더 이상 역방향으로 이동 되었습니다](part1-images/image11.png "카메라 관점으로 인해 더 작은 표시 되는 모델에 더 이상 역방향으로 이동")

## <a name="rendering-multiple-models"></a>여러 모델을 렌더링

단일 위에서 언급 한 대로 `Model` 여러 번을 그릴 수 있습니다. 더 쉽게 확인 하는 것에 이동는 `Model` 원하는 받아들이는 자체 메서드로 그리기 코드 `Model` 위치가 매개 변수입니다. 완료 한 번 우리의 `Draw` 및 `DrawModel` 메서드 같이 표시 됩니다.


```csharp
protected override void Draw(GameTime gameTime)
{
    GraphicsDevice.Clear(Color.CornflowerBlue);
    DrawModel (new Vector3 (-4, 0, 0));
    DrawModel (new Vector3 ( 0, 0, 0));
    DrawModel (new Vector3 ( 4, 0, 0));
    DrawModel (new Vector3 (-4, 0, 3));
    DrawModel (new Vector3 ( 0, 0, 3));
    DrawModel (new Vector3 ( 4, 0, 3));
    base.Draw(gameTime);
}
void DrawModel(Vector3 modelPosition)
{
    foreach (var mesh in model.Meshes)
    {
        foreach (BasicEffect effect in mesh.Effects)
        {
            effect.EnableDefaultLighting ();
            effect.PreferPerPixelLighting = true;
            effect.World = Matrix.CreateTranslation (modelPosition);
            var cameraPosition = new Vector3 (0, 10, 0);
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
        }
        // Now that we've assigned our properties on the effects we can
        // draw the entire mesh
        mesh.Draw ();
    }
}
```

이 인해 로봇 6 번 그리고 모델:

![이 인해 모델 6 번 그리고 로봇](part1-images/image1.png "로봇 6 번 그리고 모델에에서이 인해")

## <a name="summary"></a>요약

이 연습에서는 도입 MonoGame의 `Model` 클래스입니다. .Fbx 파일로 변환 하는.xnb 다루고 있는 바이 턴에 로드할 수는 `Model` 클래스입니다. 또한 표시 방법을 수정은 `BasicEffect` 인스턴스에 영향을 줄 수 `Model` 그리기 합니다.

## <a name="related-links"></a>관련 링크

- [MonoGame 모델 참조](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [완료 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
