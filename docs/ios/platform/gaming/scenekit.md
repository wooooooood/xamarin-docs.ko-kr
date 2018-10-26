---
title: Xamarin.iOS에서 SceneKit
description: 이 문서에서는 OpenGL의 복잡성을 추상화 함으로써 3D 그래픽 작업을 간소화 하는 3D 장면 그래프 API, SceneKit을 설명 합니다.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 0944978b34c8e164acd6e829db177bf4fd72dea9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109863"
---
# <a name="scenekit-in-xamarinios"></a>Xamarin.iOS에서 SceneKit

SceneKit 3D 장면 그래프 API 3D 그래픽 작업을 간소화 하는 경우 OS X 10.8에 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. SceneKit를 사용 하 여 몰입 감이 뛰어난 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL에 대 한 전문이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 토대로 SceneKit 추상화 OpenGL 및 OpenGL ES를 매우 간편 하 게 추가 3D 콘텐츠 응용 프로그램의 복잡성입니다. 그러나 OpenGL 전문가 라면 SceneKit는도 OpenGL을 사용 하 여 직접 시도 하는 것에 대 한 훌륭한 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 Apple 등 다른 프레임 워크, Core Animation, Core 이미지 및 Sprite Kit를 사용 하 여 매끄럽게 통합 되 합니다.

SceneKit 사용 하기가 매우 쉽습니다. 렌더링을 처리 하는 선언적 API입니다. 단순히를 설정 하 장면 장면의 렌더링, 그리고 SceneKit 처리 속성을 추가 합니다.

사용 하 여 장면 그래프 만든 SceneKit을 사용 하 여 `SCNScene` 클래스입니다. 인스턴스를 나타내는 노드 계층을 포함 하는 장면 `SCNNode`를 3D 공간에서 위치를 정의 합니다. 다음 그림에 나온 것 처럼 각 노드에 기 하 도형, 조명 등 해당 모양에 영향을 주는 자료는 속성이 있습니다.

![](scenekit-images/image7.png "SceneKit 계층") 

## <a name="create-a-scene"></a>장면 만들기

화면에 표시 하는 장면 있도록에 추가 `SCNView` 뷰의 장면 속성에 할당 하 여 합니다. 또한 장면 변경을 수행한 경우 `SCNView` 변경 내용을 표시 하려면 자동으로 업데이트 됩니다.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

장면 3d 모델링 도구를 통해 내보낸 파일에서 또는 프로그래밍 방식으로 기하학적 기본 형식에서 채울 수 있습니다. 예를 들어, 이것이 구 만들고 장면에 추가 하는 방법입니다.

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>광원 추가

이 시점에서 구체 장면에서 없습니다 light 있기 때문에 항목으로 표시 되지 않습니다. 연결 `SCNLight` 인스턴스 노드를 SceneKit에서 광원을 만듭니다. 주변 조명 까지의 다양 한 형식의 방향성 조명은 광원의 몇 가지 종류가 있습니다. 예를 들어 다음 코드는 구체 측면의 전방향 light를 만듭니다.

```csharp
// omnidirectional light
var light = SCNLight.Create ();
var lightNode = SCNNode.Create ();
light.LightType = SCNLightType.Omni;
light.Color = UIColor.Blue;
lightNode.Light = light;
lightNode.Position = new SCNVector3 (-40, 40, 60);
scene.RootNode.AddChildNode (lightNode);
```

전방향 조명을 비추는 손전등 같은 일종의 결과도 조명에 확산 반사를 생성 합니다. 주변 광원을 만드는 경우와 마찬가지로 하지만 방향이 없는 모든 방향에서 균등 하 게 장점을 발휘 하는 대로 생각 무드:) 조명

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

원위치에서 조명을 사용 하 여 구의 장면에 표시 됩니다.

![](scenekit-images/image8.png "구체 켜지 면 장면에 표시 됩니다.")
 
## <a name="adding-a-camera"></a>카메라를 추가합니다.

관점을 변경 (SCNCamera) 카메라 장면에 추가 합니다. 카메라를 추가 하는 패턴이 비슷합니다. 카메라를 만들고 노드에 연결 장면 노드를 추가 합니다.

```csharp
// camera
camera = new SCNCamera {
    XFov = 80,
    YFov = 80
};
cameraNode = new SCNNode {
    Camera = camera,
    Position = new SCNVector3 (0, 0, 40)
};
scene.RootNode.AddChildNode (cameraNode);
```

Create 팩터리 메서드 또는 생성자를 사용 하 여 개체를 만들 수 있습니다 SceneKit 위의 코드에서 볼 수 있습니다. 전자를 사용 하 여 사용 하면 C# 이니셜라이저 구문을 있지만 사용할지는 주로 기본 설정 합니다.

위치에서 카메라를 사용 하 여 전체 구를 사용자에 게 표시 됩니다.

![](scenekit-images/image9.png "전체 구를 사용자에 게 표시 됩니다.")
 
장면에 추가 광원을 추가할 수 있습니다. 모양을 몇 가지 자세한 전방향 표시등이 다음과 같습니다.

![](scenekit-images/image10.png "몇 가지 자세한 전방향 표시등이 구")
 
또한 설정 하 여 `sceneView.AllowsCameraControl = true`, 사용자가 터치 제스처를 사용 하 여 관점을 변경할 수 있습니다.

### <a name="materials"></a>자료

자료는 SCNMaterial 클래스를 사용 하 여 생성 됩니다. 구의 화면에 이미지를 추가 하는 예에 대 한 자료의 이미지를 설정 *확산* 내용입니다.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

이 계층 아래와 같이 노드로 이미지:

![](scenekit-images/image11.png "계층화 된 sphere 이미지")
 
다른 유형의 너무 조명에 응답 하는 자료를 설정할 수 있습니다. 예를 들어 개체 shiny 수 있습니다 및 반사 내용을 설정한 반사광을 아래와 같이 화면에서 밝은 위치에 결과 표시 하려면:

![](scenekit-images/image12.png "화면에 흰 점이 발생 하는 반사 리플렉션을 사용 하 여 shiny 수행 하는 개체")
 
자료는 매우 유연 하므로 매우 적은 양의 코드로 많이 얻을 수 있습니다. 예를 들어, 확산 내용에 이미지를 설정 하는 대신 해당 내용으로 설정 반사 대신 합니다.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

이제는 monkey 구체를 관점의 독립적인 내에서 시각적으로 배치에 나타납니다.

### <a name="animation"></a>애니메이션

SceneKit은 애니메이션에서 잘 작동 하도록 설계 되었습니다. 암시적 또는 명시적 애니메이션을 만들 수 있습니다 하 고 핵심 애니메이션 계층 트리에서 장면 렌더링도 있습니다. SceneKit 자체 전환 클래스를 제공 암시적 애니메이션을 만들 때 `SCNTransaction`합니다.

구체를 회전 하는 예제는 다음과 같습니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

그러나 회전 보다 훨씬 더 애니메이트할 수 있습니다. SceneKit의 많은 속성은 애니메이션 효과 줄. 예를 들어, 다음 코드는 애니메이션 효과 줍니다 재질의 `Shininess` 반사광을 늘려야 합니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit 매우 간단 하 게 사용 됩니다. 다양 한 제약 조건, 물리, 선언적 작업, 3D 텍스트, 필드 지원, Sprite Kit 통합 및 몇 가지 이름을 Core 이미지 통합의 깊이 비롯 한 추가 기능을 제공 합니다.