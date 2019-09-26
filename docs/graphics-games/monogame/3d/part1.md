---
title: 모델 클래스 사용
description: 모델 클래스는 3D 그래픽을 렌더링 하는 일반적인 방법과 비교할 때 복잡 한 3D 개체 렌더링을 크게 간소화 합니다. 모델 개체는 콘텐츠 파일에서 생성 되므로 사용자 지정 코드 없이 콘텐츠를 쉽게 통합할 수 있습니다.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: c5702780b6a0f0732d846a2cd4226aec5e49fc21
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70766828"
---
# <a name="using-the-model-class"></a>모델 클래스 사용

_모델 클래스는 3D 그래픽을 렌더링 하는 일반적인 방법과 비교할 때 복잡 한 3D 개체 렌더링을 크게 간소화 합니다. 모델 개체는 콘텐츠 파일에서 생성 되므로 사용자 지정 코드 없이 콘텐츠를 쉽게 통합할 수 있습니다._

MonoGame API에는 콘텐츠 `Model` 파일에서 로드 된 데이터를 저장 하 고 렌더링을 수행 하는 데 사용할 수 있는 클래스가 포함 되어 있습니다. 모델 파일은 매우 간단할 수도 있고 (예: 색이 칠해진 삼각형) 질감 및 조명을 포함 하 여 복잡 한 렌더링 정보를 포함할 수도 있습니다.

이 연습에서는 [로봇의 3d 모델](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 을 사용 하 고 다음을 다룹니다.

- 새 게임 프로젝트 시작
- 모델 및 해당 질감에 대 한 XNBs 만들기
- 게임 프로젝트에 XNBs 포함
- 3D 모델 그리기
- 여러 모델 그리기

완료 되 면 프로젝트는 다음과 같이 표시 됩니다.

![6 개 로봇을 보여 주는 완성 된 샘플](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>빈 게임 프로젝트 만들기

먼저 MonoGame3D 이라는 게임 프로젝트를 설정 해야 합니다. 새 MonoGame 프로젝트를 만드는 방법에 대 한 자세한 내용은 [플랫폼 간 MonoGame 프로젝트를 만드는 방법에](~/graphics-games/monogame/introduction/part1.md)대 한 연습을 참조 하세요.

이동 하기 전에 프로젝트가 제대로 열리고 배포 되는지 확인 해야 합니다. 배포 되 면 빈 파란색 화면이 표시 됩니다.

![빈 파란색 게임 화면](part1-images/image2.png)

## <a name="including-the-xnbs-in-the-game-project"></a>게임 프로젝트에 XNBs 포함

.Xnb 파일 형식은 빌드된 콘텐츠 ( [MonoGame 파이프라인 도구로](http://www.monogame.net/documentation/?page=Pipeline)만든 콘텐츠)의 표준 확장입니다. 빌드된 모든 콘텐츠에는 원본 파일 (모델의 경우 fbx 파일) 및 대상 파일 (.xnb 파일)이 있습니다. Fbx 형식은 [Maya](http://www.autodesk.com/products/maya/overview) 및 [Blender](http://www.blender.org/)와 같은 응용 프로그램에서 만들 수 있는 일반적인 3d 모델 형식입니다. 

3d `Model` 기 하 도형 데이터가 포함 된 디스크에서 .xnb 파일을 로드 하 여 클래스를 생성할 수 있습니다.   이. xnb 파일은 콘텐츠 프로젝트를 통해 생성 됩니다. Monogame 템플릿은 콘텐츠 폴더에 콘텐츠 프로젝트 (mgcp 확장명 포함)를 자동으로 포함 합니다. MonoGame 파이프라인 도구에 대 한 자세한 내용은 [콘텐츠 파이프라인 가이드](https://github.com/xamarin/docs-archive/blob/master/Docs/CocosSharp/content-pipeline/introduction.md)를 참조 하세요.

이 가이드에서는 MonoGame 파이프라인 도구를 사용 하 여 건너뛰고을 사용 합니다. 여기에는 XNB 파일이 포함 되어 있습니다. 에 유의 하십시오. XNB 파일은 플랫폼별로 다르므로 작업 중인 플랫폼에 따라 올바른 XNB 파일 집합을 사용 해야 합니다.

게임에서 포함 된 .xnb 파일을 사용할 수 있도록 [콘텐츠 .zip 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 의 압축을 풉니다. Android 프로젝트에서 작업 하는 경우 **WalkingGame** 프로젝트에서 **자산** 폴더를 마우스 오른쪽 단추로 클릭 합니다. IOS 프로젝트에서 작업 하는 경우 **WalkingGame** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다. **추가-> 파일 추가** ...를 선택 하 고 작업 중인 플랫폼에 대 한 폴더에서 .xnb 파일을 모두 선택 합니다.

이제 두 파일이 프로젝트의 일부 여야 합니다.

![Xnb 파일이 있는 솔루션 탐색기 콘텐츠 폴더](part1-images/xnbsinxs.png)

Mac용 Visual Studio 새로 추가 된 XNBs에 대 한 빌드 작업을 자동으로 설정할 수 없습니다. IOS의 경우 각 파일을 마우스 오른쪽 단추로 클릭 하 고 **빌드 작업-> BundleResource**를 선택 합니다. Android의 경우 각 파일을 마우스 오른쪽 단추로 클릭 하 고 **빌드 작업-> AndroidAsset**를 선택 합니다.

## <a name="rendering-a-3d-model"></a>3D 모델 렌더링

화면에 모델을 표시 하는 데 필요한 마지막 단계는 로드 및 그리기 코드를 추가 하는 것입니다. 구체적으로 다음을 수행 합니다.

- 클래스에서 인스턴스 `Model` `Game1` 정의
- `Model` 인스턴스 로드`Game1.LoadContent`
- `Model` 인스턴스 그리기`Game1.Draw`

**WalkingGame PCL** 에 있는 코드파일을다음으로`Game1.cs` 바꿉니다.

```csharp
public class Game1 : Game
{
    GraphicsDeviceManager graphics;

    // This is the model instance that we'll load
    // our XNB into:
    Model model;

    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        graphics.IsFullScreen = true;

        Content.RootDirectory = "Content";
    }
    protected override void LoadContent()
    {
        // Notice that loading a model is very similar
        // to loading any other XNB (like a Texture2D).
        // The only difference is the generic type.
        model = Content.Load<Model> ("robot");
    }

    protected override void Update(GameTime gameTime)
    {
        base.Update(gameTime);
    }

    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);

        // A model is composed of "Meshes" which are
        // parts of the model which can be positioned
        // independently, which can use different textures,
        // and which can have different rendering states
        // such as lighting applied.
        foreach (var mesh in model.Meshes)
        {
            // "Effect" refers to a shader. Each mesh may
            // have multiple shaders applied to it for more
            // advanced visuals. 
            foreach (BasicEffect effect in mesh.Effects)
            {
                // We could set up custom lights, but this
                // is the quickest way to get somethign on screen:
                effect.EnableDefaultLighting ();
                // This makes lighting look more realistic on
                // round surfaces, but at a slight performance cost:
                effect.PreferPerPixelLighting = true;

                // The world matrix can be used to position, rotate
                // or resize (scale) the model. Identity means that
                // the model is unrotated, drawn at the origin, and
                // its size is unchanged from the loaded content file.
                effect.World = Matrix.Identity;

                // Move the camera 8 units away from the origin:
                var cameraPosition = new Vector3 (0, 8, 0);
                // Tell the camera to look at the origin:
                var cameraLookAtVector = Vector3.Zero;
                // Tell the camera that positive Z is up
                var cameraUpVector = Vector3.UnitZ;

                effect.View = Matrix.CreateLookAt (
                    cameraPosition, cameraLookAtVector, cameraUpVector);

                // We want the aspect ratio of our display to match
                // the entire screen's aspect ratio:
                float aspectRatio = 
                    graphics.PreferredBackBufferWidth / (float)graphics.PreferredBackBufferHeight;
                // Field of view measures how wide of a view our camera has.
                // Increasing this value means it has a wider view, making everything
                // on screen smaller. This is conceptually the same as "zooming out".
                // It also 
                float fieldOfView = Microsoft.Xna.Framework.MathHelper.PiOver4;
                // Anything closer than this will not be drawn (will be clipped)
                float nearClipPlane = 1;
                // Anything further than this will not be drawn (will be clipped)
                float farClipPlane = 200;

                effect.Projection = Matrix.CreatePerspectiveFieldOfView(
                    fieldOfView, aspectRatio, nearClipPlane, farClipPlane);

            }

            // Now that we've assigned our properties on the effects we can
            // draw the entire mesh
            mesh.Draw ();
        }
        base.Draw(gameTime);
    }
}
```

이 코드를 실행 하는 경우 모델이 화면에 표시 됩니다.

![화면에 표시 되는 모델](part1-images/image8.png "이 코드가 실행 되 면 모델이 화면에 표시") 됩니다.

### <a name="model-class"></a>모델 클래스

`Model` 클래스는 콘텐츠 파일 (예: fbx 파일)에서 3d 렌더링을 수행 하기 위한 핵심 클래스입니다. 여기에는 3d 기 하 도형, 질감 참조 및 `BasicEffect` 위치, 조명 및 카메라 값을 제어 하는 인스턴스를 포함 하 여 렌더링에 필요한 모든 정보가 포함 됩니다.

이 `Model` 가이드의 뒷부분에서 설명 하는 것 처럼 단일 모델 인스턴스는 여러 위치에서 렌더링 될 수 있으므로 클래스 자체는 위치를 지정 하기 위한 변수를 직접 포함 하지 않습니다.

각 `Model` 는 `Meshes` 속성을 통해 노출 되 `ModelMesh` 는 하나 이상의 인스턴스로 구성 됩니다. 를 `Model` 단일 게임 개체 (예: 로봇 또는 자동차)로 간주할 수도 있지만 각각 `ModelMesh` 다른 `BasicEffect` 값을 사용 하 여 그릴 수 있습니다. 예를 들어 개별 메시 부분은 자동차의 로봇이 나 휠 다리를 나타낼 수 있으며,이 `BasicEffect` 값을 할당 하 여 휠 회전 또는 다리 이동을 만들 수 있습니다. 

### <a name="basiceffect-class"></a>BasicEffect 클래스

클래스 `BasicEffect` 는 렌더링 옵션을 제어 하기 위한 속성을 제공 합니다. 에서 `BasicEffect` 처음으로 수정 하는 작업은 `EnableDefaultLighting` 메서드를 호출 하는 것입니다. 이름에서 알 수 있듯이이 경우 기본 조명이 사용 됩니다 .이는가 `Model` 예상 대로 게임에 표시 되는 것을 확인 하는 데 매우 유용 합니다. 호출을 주석으로 처리 `EnableDefaultLighting` 하는 경우 해당 텍스처를 사용 하 여 렌더링 된 모델은 표시 되지만 음영 또는 반사 광선은 표시 되지 않습니다.

```csharp
//effect.EnableDefaultLighting ();
```

![모델은 해당 질감 으로만 렌더링 되지만 음영 또는 반사 광선은 포함 하지 않습니다] . (part1-images/image9.png "모델은 해당 질감 으로만 렌더링 되지만 음영 또는 반사 광선은 포함 하지 않습니다") .

속성 `World` 은 모델의 위치, 회전 및 배율을 조정 하는 데 사용할 수 있습니다. 위의 코드에서는 `Matrix.Identity` 값을 사용 합니다. 즉,가 `Model` fbx 파일에 지정 된 것과 정확히 일치 하는 게임을 렌더링 합니다. [3 부](~/graphics-games/monogame/3d/part3.md)에서는 행렬 및 3d 좌표에 대해 자세히 설명 하지만 예를 들어 다음과 같이 속성을 `Model` `World` 변경 하 여의 위치를 변경할 수 있습니다.

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

이 코드를 통해 개체는 세 개의 세계 단위로 이동 됩니다.

![이 코드를 통해 개체가 3 개의 세계 단위로 이동 합니다] . (part1-images/image10.png "이 코드를 통해 개체가 3 개의 세계 단위로 이동 합니다") .

에 `BasicEffect` 할당된`Projection`마지막 두 속성은 및입니다.`View` [3 부에서](~/graphics-games/monogame/3d/part3.md)3d 카메라를 다룰 예정 이지만 예를 들어 지역 `cameraPosition` 변수를 변경 하 여 카메라의 위치를 수정할 수 있습니다.

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

카메라가 더 뒤로 이동 하 여 큐브 뷰 때문에 더 작게 표시 `Model` 되는 것을 볼 수 있습니다.

![카메라가 뒤로 이동 하 여 큐브 뷰 때문에 모델이 더 작게] 표시 됩니다. (part1-images/image11.png "카메라가 뒤로 이동 하 여 큐브 뷰 때문에 모델이 더 작게") 표시 됩니다.

## <a name="rendering-multiple-models"></a>여러 모델 렌더링

위에서 설명한 것 처럼 단일 `Model` 을 여러 번 그릴 수 있습니다. 이 작업을 더 쉽게 수행 하기 위해 원하는 `Model` `Model` 위치를 매개 변수로 사용 하는 자체 메서드로 그리기 코드를 이동 하 게 됩니다. 완료 `Draw` 되 면 및 `DrawModel` 메서드는 다음과 같습니다.

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

이로 인해 로봇 모델이 6 번 그려집니다.

![이로 인해 로봇 모델이 6 번 그려집니다] . (part1-images/image1.png "이로 인해 로봇 모델이 6 번 그려집니다") .

## <a name="summary"></a>요약

이 연습에서는 MonoGame의 `Model` 클래스를 소개 했습니다. Fbx 파일을 .xnb로 변환 하는 방법에 대해 설명 합니다 .이를 `Model` 클래스에 로드할 수 있습니다. 또한 인스턴스에 대 한 `BasicEffect` 수정 내용이 그리기에 영향을 줄 `Model` 수 있는 방법도 보여 줍니다.

## <a name="related-links"></a>관련 링크

- [MonoGame 모델 참조](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [완료 된 프로젝트 (샘플)](https://docs.microsoft.com/samples/xamarin/mobile-samples/modelrenderingmg/)
