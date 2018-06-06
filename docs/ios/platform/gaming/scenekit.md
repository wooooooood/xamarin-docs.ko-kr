---
title: Xamarin.iOS에 SceneKit
description: 이 문서에서는 SceneKit, OpenGL의 복잡성을 도외시 여 3D 그래픽 작업을 간소화 하는 3D 장면 graph API에 설명 합니다.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: fb72e194e14f903061e1bd2dc6d04ef88ab429d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786787"
---
# <a name="scenekit-in-xamarinios"></a>Xamarin.iOS에 SceneKit

SceneKit은 3D 장면 그래프 3D 그래픽 작업을 간소화 하는 API입니다. OS X 10.8에서 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. SceneKit와 몰입 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL 관련 지식이 필요 하지 않습니다. 을 일반적인 장면 그래프 개념을 기반으로 구축 SceneKit 추상화 OpenGL 및 OpenGL ES 매우 간편 하 게 추가 3D 콘텐츠를 응용 프로그램의 복잡성을 합니다. 그러나 OpenGL 전문가 인 경우 SceneKit는도 OpenGL을 사용 하 여 직접 전후해 서 연결에 대 한 뛰어난 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 다른 Apple 프레임 워크 Sprite 키트 코어 애니메이션, Core 이미지 등 매우 잘 통합 합니다.

SceneKit 작업할 매우 쉽습니다. 이 렌더링은 담당 하는 선언적 API입니다. 장면을 설정 하기만 하면, 장면의 렌더링 및 SceneKit 핸들 속성을 추가 합니다.

사용 하 여 장면 그래프 만들면 SceneKit 작업할는 `SCNScene` 클래스입니다. 인스턴스로 표현 노드 계층 구조를 포함 하는 장면 `SCNNode`, 3D 공간에서 위치를 정의 합니다. 다음 그림에 표시 된 것 처럼 각 노드에 기 하 도형, 조명 등의 모양에 영향을 주는 자료는 속성이 있습니다.

![](scenekit-images/image7.png "SceneKit 계층 구조") 

## <a name="create-a-scene"></a>장면 만들기

화면에 표시 되는 장면 있도록에 추가 `SCNView` 보기의 장면 속성에 할당 하 여 합니다. 또한를 장면에 변경 내용을 확인 하는 경우, `SCNView` 는 변경 사항을 표시 하려면 자동으로 업데이트 됩니다.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

장면 3d 모델링 도구를 통해 내보낸 파일 또는 기하학적 기본 형식에서 프로그래밍 방식으로 채울 수 있습니다. 예를 들어 다음은 구 생성 및 장면에 추가 하는 방법입니다.

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>Light 추가

이 시점에서 구의 장면의 어렵습니다 있기 때문에 아무 것도 표시 되지 않습니다. 연결 `SCNLight` 인스턴스 노드에 SceneKit에 표시등을 만듭니다. 여러 가지 방법으로 다양 한 형태의 직접 조명에서 앰비언트 조명 까지의 조명 합니다. 예를 들어 다음 코드는 전방향 조명을 구의 측면에 만듭니다.

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

전방향 조명 확산 반사 손전등 비추는 같은 일종의 짝수 조명에 결과 생성 합니다. 앰비언트 light 만들기 비슷합니다 하지만 방향이 없는 발휘 동일 하 게 모든 방향으로 합니다. 라고 생각 무드가:) 조명

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

원위치에서 표시등이 구의 장면에 표시 됩니다.

![](scenekit-images/image8.png "구의 켜지 면 장면에 표시 됩니다.")
 
## <a name="adding-a-camera"></a>카메라 추가

관점을 변경 (SCNCamera) 카메라를 장면에 추가 합니다. 카메라를 추가 하는 패턴은 유사 합니다. 카메라를 만들고 노드에 연결 장면에 노드를 추가 합니다.

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

Create 팩터리 메서드 또는 생성자를 사용 하 여 개체를 만들 수 있습니다 SceneKit 위의 코드에서 볼 수 있습니다. 전자를 사용 하면 C# 이니셜라이저 구문을 사용 하 여 기본 설정 하기에 사용할 있지만.

카메라 전체 구를 사용자에 게 표시 됩니다.

![](scenekit-images/image9.png "전체 구를 사용자에 게 표시 됩니다.")
 
추가 lights도 장면에 추가할 수 있습니다. 모양 몇 가지 더 많은 전방향 표시등으로 다음과 같습니다.

![](scenekit-images/image10.png "몇 가지 더 많은 전방향 표시등이 구")
 
또한 설정 하 여 `sceneView.AllowsCameraControl = true`, 터치 제스처와 관점을 변경할 수 있습니다.

### <a name="materials"></a>자료

자료는 SCNMaterial 클래스를 사용 하 여 만들어집니다. 구의 화면에 이미지를 추가 하는 등의 자료를 이미지 설정 *확산* 내용입니다.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

이 계층 아래와 같이 노드에 끌어 이미지:

![](scenekit-images/image11.png "구에 이미지를 레이어로 표시")
 
다른 유형의 너무 조명에 응답 하는 자료를 설정할 수 있습니다. 예를 들어 개체와 할 수 있고 빛나는 반사 내용이 반사 반사, 아래와 같이 화면에 흰 점이 그 결과 표시 하려면 설정:

![](scenekit-images/image12.png "화면에 흰 점이 발생 하는 반사 리플렉션을 사용 하 여 반짝이 수행 하는 개체")
 
자료는 유연 하 게 매우 적은 양의 코드로 훨씬 달성할 수 있게 합니다. 예를 들어 설정 하는 대신 확산 내용에 대 한 이미지 설정 반사 내용에 대신.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

이제는 원숭이 구, 독립적으로 관점에서 시각적으로 있으면 표시 됩니다.

### <a name="animation"></a>애니메이션

SceneKit은 애니메이션와 잘 작동 하도록 설계 되었습니다. 암시적 또는 명시적 애니메이션을 만들 수 및 코어 애니메이션 계층 트리에서 장면을도 렌더링할 수 있습니다. 만들 때 암시적 애니메이션, SceneKit 제공 transition 클래스 자체 `SCNTransaction`합니다.

구의 회전 하는 예제는 다음과 같습니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

그러나 회전 보다 훨씬 더 애니메이션 수 있습니다. SceneKit의 많은 속성은 애니메이션을 적용 합니다. 예를 들어 다음 코드 애니메이션 효과 적용 재질의 `Shininess` 반사 리플렉션 증가 합니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit은 사용 하도록 매우 간단 합니다. 다양 한 제약 조건, 물리학, 선언적 동작, 3D 텍스트, 필드 지원, Sprite 키트 통합 및 Core 이미지 통합이 군의 깊이 비롯 한 추가 기능을 제공 합니다.