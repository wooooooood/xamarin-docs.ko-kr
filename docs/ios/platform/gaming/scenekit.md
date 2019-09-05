---
title: Xamarin.ios의 SceneKit
description: 이 문서에서는 OpenGL의 복잡성을 추상화 하 여 3D 그래픽 작업을 간소화 하는 3D 장면 그래프 API 인 SceneKit에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 19049ED5-B68E-4A0E-9D57-B7FAE3BB8987
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/14/2017
ms.openlocfilehash: d6e6ff02fef3d2919e9716dc8a456aabd9533820
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292795"
---
# <a name="scenekit-in-xamarinios"></a>Xamarin.ios의 SceneKit

SceneKit는 3D 그래픽 작업을 간소화 하는 3D 장면 그래프 API입니다. OS X 10.8에 처음 도입 되었으며 이제 iOS 8로 제공 되었습니다. SceneKit는 몰입 형 3D 시각화 및 일반 3D 게임을 만드는 데 OpenGL의 전문 지식이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 기반으로 하는 SceneKit는 OpenGL 및 OpenGL ES의 복잡성을 추상화 하 여 응용 프로그램에 3D 콘텐츠를 매우 쉽게 추가할 수 있도록 합니다. 그러나 OpenGL 전문가 인 경우 SceneKit는 OpenGL에 직접 연결할 수 있는 기능을 제공 합니다. 또한 물리와 같은 3D 그래픽을 보완 하 고 핵심 애니메이션, 핵심 이미지 및 스프라이트 키트와 같은 다른 여러 Apple 프레임 워크와 매우 잘 통합 하는 다양 한 기능이 포함 되어 있습니다.

SceneKit는 작업 하기가 매우 쉽습니다. 렌더링을 처리 하는 선언적 API입니다. 장면을 설정 하 고, 여기에 속성을 추가 하 고, SceneKit 장면 렌더링을 처리 하기만 하면 됩니다.

SceneKit로 작업 하려면 클래스를 `SCNScene` 사용 하 여 장면 그래프를 만듭니다. 장면에는의 `SCNNode`인스턴스로 표시 되는 노드의 계층이 포함 되어 3d 공간에서 위치를 정의 합니다. 각 노드에는 다음 그림에 나와 있는 것 처럼 모양, 조명, 재질 등의 모양에 영향을 주는 속성이 있습니다.

![](scenekit-images/image7.png "SceneKit 계층 구조")

## <a name="create-a-scene"></a>장면 만들기

화면이 화면에 표시 되도록 하려면 해당 장면을 보기의 장면 속성에 `SCNView` 할당 하 여에 추가 합니다. 또한 장면을 변경 하는 경우에서 `SCNView` 자체 업데이트를 수행 하 여 변경 내용을 표시 합니다.

```csharp
scene = SCNScene.Create ();
sceneView = new SCNView (View.Frame);
sceneView.Scene = scene;
```

3d 모델링 도구를 통해 내보낸 파일이 나 기 하 도형 기본 형식에서 프로그래밍 방식으로 장면을 채울 수 있습니다. 예를 들어 다음은 구를 만들고 장면에 추가 하는 방법입니다.

```csharp
sphere = SCNSphere.Create (10.0f);
sphereNode = SCNNode.FromGeometry (sphere);
sphereNode.Position = new SCNVector3 (0, 0, 0);
scene.RootNode.AddChildNode (sphereNode);
```

## <a name="adding-light"></a>광원 추가

이 시점에서 장면에는 광원이 없으므로 구에는 아무것도 표시 되지 않습니다. 노드에 `SCNLight` 인스턴스를 연결 하면 SceneKit에서 조명이 생성 됩니다. 다양 한 형태의 방향성 조명에서 주변 조명까지 다양 한 형식의 조명이 있습니다. 예를 들어 다음 코드는 구의 측면에 전방향 라이트를 만듭니다.

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

전방향 조명을 통해 확산 반사를 생성 하 여 손전등를 비추는 것과 비슷한 방식으로 조명 합니다. 주변 광원을 만드는 것은 유사 하지만 모든 방향에서 동일 하 게 나타납니다. 분위기 조명 처럼 생각:)

```csharp
// ambient light
ambientLight = SCNLight.Create ();
ambientLightNode = SCNNode.Create ();
ambientLight.LightType = SCNLightType.Ambient;
ambientLight.Color = UIColor.Purple;
ambientLightNode.Light = ambientLight;
scene.RootNode.AddChildNode (ambientLightNode);
```

조명이 제자리에 있으면 이제 구가 장면에 표시 됩니다.

![](scenekit-images/image8.png "조명이 켜질 때 장면에 표시 됩니다.")

## <a name="adding-a-camera"></a>카메라 추가

장면에 카메라 (SCNCamera)를 추가 하면 보기 지점이 변경 됩니다. 카메라를 추가 하는 패턴은 비슷합니다. 카메라를 만들어 노드에 연결 하 고 해당 노드를 장면에 추가 합니다.

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

위의 코드에서 볼 수 있듯이 생성자를 사용 하거나 Create factory 메서드에서 SceneKit 개체를 만들 수 있습니다. 이전에는 이니셜라이저 C# 구문을 사용 하는 것을 허용 하지만 사용할 수 있는 것은 대부분 기본 설정의 문제입니다.

카메라를 배치한 상태에서 전체 구가 사용자에 게 표시 됩니다.

![](scenekit-images/image9.png "사용자에 게 전체 구가 표시 됩니다.")

장면에 조명을 더 추가할 수도 있습니다. 다음은 몇 가지 전방향 광원으로 표시 되는 모양입니다.

![](scenekit-images/image10.png "더 많은 전방향 표시등을 포함 하는 구")

또한를 설정 `sceneView.AllowsCameraControl = true`하면 사용자가 터치 제스처를 사용 하 여 뷰 지점을 변경할 수 있습니다.

### <a name="materials"></a>자재

자료는 SCNMaterial 클래스를 사용 하 여 생성 됩니다. 예를 들어 구의 표면에 이미지를 추가 하려면 재질의 *확산* 콘텐츠로 이미지를 설정 합니다.

```csharp
material = SCNMaterial.Create ();
material.Diffuse.Contents = UIImage.FromFile ("monkey.png");
sphere.Materials = new SCNMaterial[] { material };
```

그러면 아래와 같이 이미지가 노드로 표시 됩니다.

![](scenekit-images/image11.png "구에 이미지를 계층화 합니다.")

재질은 다른 종류의 조명에도 응답 하도록 설정 될 수 있습니다. 예를 들어 개체를 광택이 나 반사 내용을 표시 하도록 반사를 설정 하 여 아래와 같이 화면에 밝은 지점을 만들 수 있습니다.

![](scenekit-images/image12.png "반사 반사를 사용 하 여 빛나는 개체입니다. 그러면 화면에 밝은 지점이 생성 됩니다.")

재질은 매우 유연 하므로 매우 작은 코드를 사용 하 여 많은 것을 달성할 수 있습니다. 예를 들어 이미지를 확산 콘텐츠로 설정 하는 대신 반사 콘텐츠로 설정 합니다.

```csharp
material.Reflective.Contents = UIImage.FromFile ("monkey.png");
```

이제 원숭이은 보기의 지점에 관계 없이 구에 시각적으로 표시 됩니다.

### <a name="animation"></a>애니메이션

SceneKit는 애니메이션과 함께 작동 하도록 설계 되었습니다. 암시적 또는 명시적 애니메이션을 모두 만들 수 있으며, 핵심 애니메이션 계층 트리에서 장면을 렌더링할 수도 있습니다. 암시적 애니메이션을 만들 때 SceneKit는 자체 전환 클래스인를 `SCNTransaction`제공 합니다.

다음은 구를 회전 하는 예제입니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
sphereNode.Rotation = new SCNVector4 (0, 1, 0, (float)Math.PI * 4);
SCNTransaction.Commit ();
```

그러나 회전 보다 훨씬 더 많은 애니메이션 효과를 적용할 수 있습니다. SceneKit의 많은 속성은 애니메이션 효과입니다. 예를 들어 다음 코드는 재질 `Shininess` 에 반사 반사를 높이는 애니메이션 효과를 적용 합니다.

```csharp
SCNTransaction.Begin ();
SCNTransaction.AnimationDuration = 2.0;
material.Shininess = 0.1f;
SCNTransaction.Commit ();
```

SceneKit은 매우 간단 하 게 사용할 수 있습니다. 제약 조건, 물리, 선언적 작업, 3D 텍스트, 필드 지원 깊이, 스프라이트 키트 통합 및 핵심 이미지 통합을 비롯 한 다양 한 추가 기능을 제공 합니다.
