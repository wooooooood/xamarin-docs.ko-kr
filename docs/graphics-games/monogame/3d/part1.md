---
title: 모델 클래스를 사용 하 여
description: 모델 클래스는 3D 그래픽을 렌더링 하는 기존의 방법에 비해 복잡 한 3D 개체 렌더링 크게 간소화 합니다. 모델 개체는 사용자 지정 코드 없이 콘텐츠의 쉽게 통합할 수 있도록 콘텐츠 파일에서 생성 됩니다.
ms.prod: xamarin
ms.assetid: AD0A7971-51B1-4E38-B412-7907CE43CDDF
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: d9c73dfcee6321ecb314ca229db407c6d0438977
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342594"
---
# <a name="using-the-model-class"></a>모델 클래스를 사용 하 여

_모델 클래스는 3D 그래픽을 렌더링 하는 기존의 방법에 비해 복잡 한 3D 개체 렌더링 크게 간소화 합니다. 모델 개체는 사용자 지정 코드 없이 콘텐츠의 쉽게 통합할 수 있도록 콘텐츠 파일에서 생성 됩니다._

MonoGame API를 포함 한 `Model` 렌더링을 수행 하는 콘텐츠 파일에서 데이터를 저장 하는 클래스가 로드 합니다. 모델 파일 (예: 실선 색이 지정 된 삼각형) 매우 간단할 수 있습니다 또는 질감 및 조명를 포함 하 여 복잡 한 렌더링에 대 한 정보를 포함 될 수 있습니다.

이 연습에서는 [로봇의 3D 모델](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 다음에 다룹니다.

- 새 게임 프로젝트를 시작합니다.
- 모델 및 해당 질감 XNBs 만들기
- 게임 프로젝트에는 XNBs를 포함합니다.
- 3D 모델에 그리기
- 여러 모델 그리기

완료 되 면 프로젝트는 다음과 같이 표시 됩니다.

![6 로봇을 보여 주는 완성 된 샘플](part1-images/image1.png)

## <a name="creating-an-empty-game-project"></a>빈 게임 프로젝트 만들기

먼저 MonoGame3D 호출 게임 프로젝트를 설정 해야 합니다. 새 MonoGame 프로젝트 만들기에 대 한 내용은 참조 하세요 [플랫폼 간 Monogame 프로젝트 만들기에 대 한이 연습](~/graphics-games/monogame/introduction/part1.md)합니다.

넘어가기 전에 프로젝트 열리고 올바르게 배포 확인 해야 합니다. 한 번 배포는 빈 블루 스크린이 표시 되어야 합니다.

![빈 파란색 게임 화면](part1-images/image2.png)


## <a name="including-the-xnbs-in-the-game-project"></a>게임 프로젝트에는 XNBs를 포함합니다.

.Xnb 파일 형식으로 작성된 된 콘텐츠에 대 한 표준 확장명입니다 (하 여 만든 콘텐츠를 [MonoGame 파이프라인 도구](http://www.monogame.net/documentation/?page=Pipeline)). 모든 기본 제공된 콘텐츠는 소스 파일 (.fbx 파일이 모델의 경우) 및 대상 파일 (.xnb 파일)에 있습니다. .Fbx 형식은 같은 응용 프로그램에서 만들 수 있는 일반적인 3D 모델 형식을 [Maya](http://www.autodesk.com/products/maya/overview) 하 고 [Blender](http://www.blender.org/)합니다. 

`Model` 3D 기 하 도형 데이터가 포함 된 디스크에서.xnb 파일을 로드 하 여 클래스를 생성할 수 있습니다.   이.xnb 파일 콘텐츠는 프로젝트를 통해 생성 됩니다. Monogame 템플릿 우리의 콘텐츠 폴더 (사용 하 여 확장.mgcp) 콘텐츠 프로젝트를 자동으로 포함 합니다. MonoGame 파이프라인 도구에 대 한 자세한 내용은 참조는 [콘텐츠 파이프라인 가이드](~/graphics-games/cocossharp/content-pipeline/index.md)합니다.

MonoGame 파이프라인을 사용 하 여 생략 하겠습니다이 가이드에 대 한 도구를 사용 합니다. 여기에 포함 된 XNB 파일입니다. 유의 합니다. XNB 파일 플랫폼 마다 다를 사용 하는 어떤 플랫폼에 대 한 올바른 XNB 파일 집합을 사용 해야 합니다.

압축을 풉니다 됩니다 것를 [Content.zip 파일](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true) 게임에서.xnb 포함 된 파일을 사용할 수 있습니다 있도록 합니다. Android 프로젝트를 진행 하는 경우 마우스 오른쪽 단추로 클릭 합니다 **자산** 폴더에는 **WalkingGame.Android** 프로젝트입니다. IOS 프로젝트를 진행 하는 경우 마우스 오른쪽 단추로 클릭 합니다 **WalkingGame.iOS** 프로젝트입니다. 선택 **추가-> 파일 추가...**  작업 중인 플랫폼에 대 한 폴더에 두.xnb 파일을 선택 합니다.

두 파일에는 이제 프로젝트의 일부 여야 합니다.

![솔루션 탐색기 xnb 파일을 사용 하 여 콘텐츠 폴더](part1-images/xnbsinxs.png)

Mac 용 visual Studio의 빌드 작업을 새로 추가 된 XNBs 자동으로 설정 하지 않을 수 있습니다. IOS에 대 한 마우스 오른쪽 단추로 클릭 선택한 파일의 각 **빌드 작업 BundleResource을->** 합니다. Android에 대 한 마우스 오른쪽 단추로 클릭 선택한 파일의 각 **빌드 작업 AndroidAsset->** 합니다.

## <a name="rendering-a-3d-model"></a>3D 모델을 렌더링합니다.

화면에 표시 되는 모델을 참조 하는 데 필요한 마지막 단계를 로드 하 고 그리기 코드를 추가 하는 것입니다. 특히, 우리가 수행한 다음:

- 정의 된 `Model` 의 인스턴스는 `Game1` 클래스
- 로드 된 `Model` 인스턴스 `Game1.LoadContent`
- 그리기는 `Model` 인스턴스 `Game1.Draw`

대체는 `Game1.cs` 코드 파일 (위치한 합니다 **WalkingGame** PCL) 다음을 사용 하 여:

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

이 코드 실행 하는 경우에서는 모델을 화면의 표시 됩니다.

![화면에 표시 되는 모델](part1-images/image8.png "이 코드를 실행 하는 경우 모델을 표시할 화면")

### <a name="model-class"></a>모델 클래스

`Model` 클래스는 콘텐츠 파일 (예:.fbx 파일)에서 3D 렌더링을 수행 하기 위한 핵심 클래스입니다. 3D 기 하 도형, 질감에 대 한 참조를 포함 하 여 렌더링 하는 데 필요한 정보를 모두 포함 하 고 `BasicEffect` 위치 지정, 조명 및 카메라 값을 제어 하는 인스턴스.

`Model` 클래스 자체에이 가이드의 뒷부분에서 살펴보겠습니다 단일 모델 인스턴스를 여러 위치에서 렌더링 될 수 있으므로 위치 지정에 대 한 변수를 직접가 없습니다.

각 `Model` 하나 이상의 이루어집니다 `ModelMesh` 를 통해 노출 되는 인스턴스는 `Meshes` 속성. 것으로 간주 될 수 있습니다 하지만 `Model` 단일 게임으로 개체 (예: 로봇 또는 자동차)을 각 `ModelMesh` 다른 그릴 수 `BasicEffect` 값입니다. 예를 들어, 개별 메시 파트 로봇 또는 자동차, 바퀴 다리를 나타낼 수 있습니다 하 고 할당 될 수 있습니다는 `BasicEffect` 바퀴 스핀 또는 다리를 확인 하는 값을 이동 합니다. 

### <a name="basiceffect-class"></a>BasicEffect 클래스

`BasicEffect` 클래스 렌더링 옵션을 제어 하기 위한 속성을 제공 합니다. 우리에 게 처음 수정 합니다 `BasicEffect` 호출 하는 것은 `EnableDefaultLighting` 메서드. 이렇게 하면 매우 간편 하 게 확인 하는 기본 조명 이름에서 알 수 있듯이 `Model` 게임에서 예상 대로 표시 됩니다. 에서는 주석으로 처리 된 `EnableDefaultLighting` 호출에 없는 음영 또는 반사 광선 했지만 해당 개의 질감을 사용 하 여 렌더링 모델을 살펴보겠습니다.

```csharp
//effect.EnableDefaultLighting ();
```

![모델 없음 음영 또는 반사 광선 했지만 해당 개의 질감을 사용 하 여 렌더링](part1-images/image9.png "없습니다 음영 또는 반사 광선 했지만 해당 개의 질감을 사용 하 여 렌더링 모델")

`World` 속성 위치, 회전 및 모델의 배율 조정에 사용할 수 있습니다. 사용 하 여 위의 코드는 `Matrix.Identity` 값, 즉는 `Model` 게임 내 지정 된 대로 정확 하 게.fbx 파일에서 렌더링 됩니다. 저희 다루고 있습니다 행렬 및 3D 좌표에서 더 자세하게에서 [3 부](~/graphics-games/monogame/3d/part3.md), 예를 들어 위치를 변경할 수 있습니다 합니다 `Model` 변경 하 여는 `World` 속성을 다음과 같이:

```csharp
// Z is up, so changing Z to 3 moves the object up 3 units:
var modelPosition = new Vector3 (0, 0, 3);
effect.World = Matrix.CreateTranslation (modelPosition); 
```

이 코드는 3 world 단위에서 위로 이동 되는 개체에서 발생 합니다.

![이 코드 3 world 단위에서 위로 이동 되는 개체의 결과](part1-images/image10.png "이 코드 3 world 단위에서 위로 이동 되는 개체의 결과")

에 할당 된 마지막 두 속성을 `BasicEffect` 됩니다 `View` 및 `Projection`합니다. 저희 다루고 있습니다 3D 카메라 [3 부](~/graphics-games/monogame/3d/part3.md), 예를 들어 로컬 변경 하 여 카메라 위치를 수정할 수 있습니다 하지만 `cameraPosition` 변수:

```csharp
// The 8 has been changed to a 30 to move the Camera further back
var cameraPosition = new Vector3 (0, 30, 0);
```

보면 카메라 추가 뒤로 이동 되었습니다 그 결과 `Model` 관점으로 인해 작은 표시:

![카메라 관점으로 인해 작은 표시 되는 모델의 결과 추가 뒤로 이동 되었습니다](part1-images/image11.png "카메라 관점으로 인해 작은 표시 되는 모델의 결과 추가 뒤로 이동 되었습니다")

## <a name="rendering-multiple-models"></a>여러 모델을 렌더링

단일 위에서 언급 한 대로 `Model` 여러 번 그릴 수 있습니다. 간편 하 게 하는 것에 이동 합니다 `Model` 그리기 코드를 원하는 사용 하는 고유한 메서드로 `Model` 매개 변수로 위치. 완료 되 면 당사의 `Draw` 및 `DrawModel` 메서드 같이 표시 됩니다.


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

이 연습에서는 MonoGame의 도입 `Model` 클래스입니다. .xnb.fbx 파일로 변환 설명 하는 다시 로드할 수는 `Model` 클래스입니다. 또한 표시 하는 방법을 수정을 `BasicEffect` 인스턴스에 영향을 줄 수 `Model` 그리기.

## <a name="related-links"></a>관련 링크

- [MonoGame 모델 참조](http://www.monogame.net/documentation/?page=T_Microsoft_Xna_Framework_Graphics_Model)
- [Content.zip](https://github.com/xamarin/mobile-samples/blob/master/ModelRenderingMG/Resources/Content.zip?raw=true)
- [완성 된 프로젝트 (샘플)](https://developer.xamarin.com/samples/mobile/ModelRenderingMG/)
