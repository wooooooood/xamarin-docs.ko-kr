---
title: Xamarin.ios의 iOS 게임 Api
description: 이 문서에서는 Xamarin.ios 게임의 그래픽 및 오디오 기능을 개선 하는 데 사용할 수 있는 iOS 9에서 제공 하는 새로운 게임 향상 기능에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: fa78a596495b22ebb2c8b148aadb76261845ccdc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70281257"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>Xamarin.ios의 iOS 게임 Api

_이 문서에서는 Xamarin.ios 게임의 그래픽 및 오디오 기능을 개선 하는 데 사용할 수 있는 iOS 9에서 제공 하는 새로운 게임 향상 기능에 대해 설명 합니다._

Apple은 Xamarin.ios 앱에서 게임 그래픽과 오디오를 보다 쉽게 구현할 수 있도록 하는 iOS 9의 게임 Api에 대 한 몇 가지 기술적 향상을 만들었습니다.
여기에는 높은 수준의 프레임 워크를 통한 개발 용이성 및 향상 된 속도 및 그래픽 기능을 위한 iOS 장치의 GPU 활용 포함 됩니다.

[![](images/flocking01.png "Flocking 실행 되는 앱의 예")](images/flocking01.png#lightbox)

여기에는 GameplayKit, ReplayKit, Model i/o, MetalKit 및 메탈 Performance 셰이더가 포함 되며, 이러한 기능에는 금속, SceneKit 및 SpriteKit의 새롭고 향상 된 기능이 포함 됩니다.

이 문서에서는 iOS 9의 새로운 게임 향상 기능을 사용 하 여 Xamarin.ios 게임을 개선 하는 모든 방법을 소개 합니다.

## <a name="introducing-gameplaykit"></a>GameplayKit 소개

Apple의 새로운 GameplayKit 프레임 워크는 구현에 필요한 반복적인 공통 코드의 양을 줄여 iOS 장치용 게임을 쉽게 만들 수 있도록 하는 일련의 기술을 제공 합니다. GameplayKit는 게임 메커니즘을 개발 하기 위한 도구를 제공 합니다 .이 도구를 사용 하면 그래픽 엔진 (예: SceneKit 또는 SpriteKit)과 쉽게 결합 하 여 완료 된 게임을 신속 하 게 제공할 수 있습니다.

GameplayKit에는 다음과 같은 몇 가지 일반적인 게임 재생 알고리즘이 포함 되어 있습니다.

- AI가 자동으로 이동 하는 이동 및 목표를 정의할 수 있는 동작 기반 에이전트 시뮬레이션입니다.
- 턴 기반 게임 플레이를 위한 minmax 인공 지능입니다.
- 자원이 동작을 제공 하는 유사 추론을 사용 하는 데이터 기반 게임 논리에 대 한 규칙 시스템입니다.

또한 GameplayKit는 다음과 같은 기능을 제공 하는 모듈식 아키텍처를 사용 하 여 게임 개발에 대 한 빌딩 블록 방법을 사용 합니다.

- 게임 플레이에서 복잡 한 절차적 코드 기반 시스템을 처리 하기 위한 상태 시스템입니다.
- 디버깅 문제를 발생 시 키 지 않고 임의 게임 플레이 및 예측 불가능성을 제공 하는 도구입니다.
- 재사용 가능한 구조화 가능 엔터티 기반 아키텍처입니다.

GameplayKit에 대해 자세히 알아보려면 Apple의 [GameplayKit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) 및 [GameplayKit Framework 참조](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)를 참조 하세요.

## <a name="gameplaykit-examples"></a>GameplayKit 예제

게임 재생 키트를 사용 하 여 Xamarin.ios 앱에서 몇 가지 간단한 게임 재생 메커니즘을 간단히 살펴보겠습니다.

### <a name="pathfinding"></a>Pathfinding

Pathfinding는 게임 보드 주위에 있는 게임의 AI 요소에 대 한 기능입니다.
예를 들어 첫 사람-슈팅 세계 지형을 통해 미로 또는 3D 문자를 통해 해당 방법을 찾는 2D 적이 있습니다.

다음 맵을 고려 하십시오.

[![](images/gkpathfindpath.png "예제 pathfinding 맵")](images/gkpathfindpath.png#lightbox)

Pathfinding를 사용 C# 하 여이 코드는 맵을 통해 찾을 수 있습니다.

```csharp
var a = GKGraphNode2D.FromPoint (new Vector2 (0, 5));
var b = GKGraphNode2D.FromPoint (new Vector2 (3, 0));
var c = GKGraphNode2D.FromPoint (new Vector2 (2, 6));
var d = GKGraphNode2D.FromPoint (new Vector2 (4, 6));
var e = GKGraphNode2D.FromPoint (new Vector2 (6, 5));
var f = GKGraphNode2D.FromPoint (new Vector2 (6, 0));

a.AddConnections (new [] { b, c }, false);
b.AddConnections (new [] { e, f }, false);
c.AddConnections (new [] { d }, false);
d.AddConnections (new [] { e, f }, false);

var graph = GKGraph.FromNodes(new [] { a, b, c, d, e, f });

var a2e = graph.FindPath (a, e); // [ a, c, d, e ]
var a2f = graph.FindPath (a, f); // [ a, b, f ]

Console.WriteLine(String.Join ("->", (object[]) a2e));
Console.WriteLine(String.Join ("->", (object[]) a2f));
```

### <a name="classical-expert-system"></a>기존 전문가 시스템

다음 C# 코드 조각은 GameplayKit를 사용 하 여 기존 전문가 시스템을 구현 하는 방법을 보여 줍니다.

```csharp
string output = "";
bool reset = false;
int input = 15;

public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    /*
    If reset is true, clear the output and set reset to false
    */
    var clearRule = GKRule.FromPredicate ((rules) => reset, rules => {
        output = "";
        reset = false;
    });
    clearRule.Salience = 1;

    var fizzRule = GKRule.FromPredicate (mod (3), rules => {
        output += "fizz";
    });
    fizzRule.Salience = 2;

    var buzzRule = GKRule.FromPredicate (mod (5), rules => {
        output += "buzz";
    });
    buzzRule.Salience = 2;

    /*
    This *always* evaluates to true, but is higher Salience, so evaluates after lower-salience items
    (which is counter-intuitive). Print the output, and reset (thus triggering "ResetRule" next time)
    */
    var outputRule = GKRule.FromPredicate (rules => true, rules => {
        System.Console.WriteLine(output == "" ? input.ToString() : output);
        reset = true;
    });
    outputRule.Salience = 3;

    var rs = new GKRuleSystem ();
    rs.AddRules (new [] {
        clearRule,
        fizzRule,
        buzzRule,
        outputRule
    });

    for (input = 1; input < 16; input++) {
        rs.Evaluate ();
        rs.Reset ();
    }
}

protected Func<GKRuleSystem, bool> mod(int m)
{
    Func<GKRuleSystem,bool> partiallyApplied = (rs) => input % m == 0;
    return partiallyApplied;
}
```

지정 된 규칙 집합 (`GKRule`) 및 알려진 입력 집합을 기반으로 하는 전문가 시스템 (`GKRuleSystem`)은 위의 예제`fizzbuzz` 에 대 한 예측 가능한 출력을 만듭니다.

### <a name="flocking"></a>Flocking

Flocking AI 제어 게임 엔터티 그룹이 flock로 동작할 수 있습니다. 여기서 그룹은 비행의 flock와 같은 리드 엔터티의 움직임 및 작업에 응답 하 고, 물고기의 학교에 응답 합니다.

다음 C# 코드 조각은 그래픽 디스플레이에 대해 GameplayKit 및 SpriteKit를 사용 하 여 flocking 동작을 구현 합니다.

```csharp
using System;
using SpriteKit;
using CoreGraphics;
using UIKit;
using GameplayKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
using OpenTK;

namespace FieldBehaviorExplorer
{
    public static class FlockRandom
    {
        private static GKARC4RandomSource rand = new GKARC4RandomSource ();

        static FlockRandom ()
        {
            rand.DropValues (769);
        }

        public static float NextUniform ()
        {
            return rand.GetNextUniform ();
        }
    }

    public class FlockingScene : SKScene
    {
        List<Boid> boids = new List<Boid> ();
        GKComponentSystem componentSystem;
        GKAgent2D trackingAgent; //Tracks finger on screen
        double lastUpdateTime = Double.NaN;
        //Hold on to behavior so it doesn't get GC'ed
        static GKBehavior flockingBehavior;
        static GKGoal seekGoal;


        public FlockingScene (CGSize size) : base (size)
        {
            AddRandomBoids (20);

            var scale = 0.4f;
            //Flocking system
            componentSystem = new GKComponentSystem (typeof(GKAgent2D));
            var behavior = DefineFlockingBehavior (boids.Select (boid => boid.Agent).ToArray<GKAgent2D>(), scale);
            boids.ForEach (boid => {
                boid.Agent.Behavior = behavior;
                componentSystem.AddComponent(boid.Agent);
            });

            trackingAgent = new GKAgent2D ();
            trackingAgent.Position = new Vector2 ((float) size.Width / 2.0f, (float) size.Height / 2.0f);
            seekGoal = GKGoal.GetGoalToSeekAgent (trackingAgent);
        }

        public override void TouchesBegan (NSSet touches, UIEvent evt)
        {
            boids.ForEach(boid => boid.Agent.Behavior.SetWeight(1.0f, seekGoal));
        }

        public override void TouchesEnded (NSSet touches, UIEvent evt)
        {
            boids.ForEach (boid => boid.Agent.Behavior.SetWeight (0.0f, seekGoal));
        }

        public override void TouchesMoved (NSSet touches, UIEvent evt)
        {
            var touch = (UITouch) touches.First();
            var loc = touch.LocationInNode (this);
            trackingAgent.Position = new Vector2((float) loc.X, (float) loc.Y);
        }


        private void AddRandomBoids (int count)
        {
            var scale = 0.4f;
            for (var i = 0; i < count; i++) {
                var b = new Boid (UIColor.Red, this.Size, scale);
                boids.Add (b);
                this.AddChild (b);
            }
        }

        internal static GKBehavior DefineFlockingBehavior(GKAgent2D[] boidBrains, float scale)
        {
            if (flockingBehavior == null) {
                var flockingGoals = new GKGoal[3];
                flockingGoals [0] = GKGoal.GetGoalToSeparate (boidBrains, 100.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [1] = GKGoal.GetGoalToAlign (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);
                flockingGoals [2] = GKGoal.GetGoalToCohere (boidBrains, 40.0f * scale, (float)Math.PI * 8.0f);

                flockingBehavior = new GKBehavior ();
                flockingBehavior.SetWeight (25.0f, flockingGoals [0]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [1]);
                flockingBehavior.SetWeight (10.0f, flockingGoals [2]);
            }
            return flockingBehavior;
        }

        public override void Update (double currentTime)
        {
            base.Update (currentTime);
            if (Double.IsNaN(lastUpdateTime)) {
                lastUpdateTime = currentTime;
            }
            var delta = currentTime - lastUpdateTime;
            componentSystem.Update (delta);
        }
    }

    public class Boid : SKNode, IGKAgentDelegate
    {
        public GKAgent2D Agent { get { return brains; } }
        public SKShapeNode Sprite { get { return sprite; } }

        class BoidSprite : SKShapeNode
        {
            public BoidSprite (UIColor color, float scale)
            {
                var rot = CGAffineTransform.MakeRotation((float) (Math.PI / 2.0f));
                var path = new CGPath ();
                path.MoveToPoint (rot, new CGPoint (10.0, 0.0));
                path.AddLineToPoint (rot, new CGPoint (0.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 20.0));
                path.AddLineToPoint (rot, new CGPoint (20.0, 30.0));
                path.AddLineToPoint (rot, new CGPoint (10.0, 0.0));
                path.CloseSubpath ();

                this.SetScale (scale);
                this.Path = path;
                this.FillColor = color;
                this.StrokeColor = UIColor.White;

            }
        }

        private GKAgent2D brains;
        private BoidSprite sprite;
        private static int boidId = 0;

        public Boid (UIColor color, CGSize size, float scale)
        {
            brains = BoidBrains (size, scale);
            sprite = new BoidSprite (color, scale);
            sprite.Position = new CGPoint(brains.Position.X, brains.Position.Y);
            sprite.ZRotation = brains.Rotation;
            sprite.Name = boidId++.ToString ();

            brains.Delegate = this;

            this.AddChild (sprite);
        }

        private GKAgent2D BoidBrains(CGSize size, float scale)
        {
            var brains = new GKAgent2D ();
            var x = (float) (FlockRandom.NextUniform () * size.Width);
            var y = (float) (FlockRandom.NextUniform () * size.Height);
            brains.Position = new Vector2 (x, y);

            brains.Rotation = (float)(FlockRandom.NextUniform () * Math.PI * 2.0);
            brains.Radius = 30.0f * scale;
            brains.MaxSpeed = 0.5f;
            return brains;
        }

        [Export ("agentDidUpdate:")]
        public void AgentDidUpdate (GameplayKit.GKAgent agent)
        {
        }

        [Export ("agentWillUpdate:")]
        public void AgentWillUpdate (GameplayKit.GKAgent agent)
        {
            var brainsIn = (GKAgent2D) agent;
            sprite.Position = new CGPoint(brainsIn.Position.X, brainsIn.Position.Y);
            sprite.ZRotation = brainsIn.Rotation;
            Console.WriteLine ($"{sprite.Name} -> [{sprite.Position}], {sprite.ZRotation}");
        }
    }
}
```

다음으로 보기 컨트롤러에서이 장면을 구현 합니다.

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
        // Perform any additional setup after loading the view, typically from a nib.
        this.View = new SKView {
        ShowsFPS = true,
        ShowsNodeCount = true,
        ShowsDrawCount = true
    };
}

public override void ViewWillLayoutSubviews ()
{
    base.ViewWillLayoutSubviews ();

    var v = (SKView)View;
    if (v.Scene == null) {
        var scene = new FlockingScene (View.Bounds.Size);
        scene.ScaleMode = SKSceneScaleMode.AspectFill;
        v.PresentScene (scene);
    }
}
```

실행 하면 약간의 애니메이션이 적용 된 _"Boids"_ 는 손가락 탭 주위에 flock 됩니다.

[![](images/flocking01.png "작은 애니메이션 Boids는 손가락 탭 주위에 flock 됩니다.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>기타 Apple 예

Apple은 위에 나와 있는 샘플 외에도 및 Xamarin.ios로 C# 트랜스 코딩 될 수 있는 다음과 같은 샘플 앱을 제공 했습니다.

- [FourInARow: GameplayKit Minmax 전략가 (상대 AI) 사용](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: GameplayKit에서 에이전트 시스템 사용](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: SpriteKit 및 GameplayKit를 사용 하 여 플랫폼 간 게임 빌드](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

IOS 9에서 Apple은 몇 가지 변경 및 금속을 추가 하 여 GPU에 대 한 낮은 오버 헤드 액세스를 제공 합니다. 금속을 사용 하면 그래픽을 극대화 하 고 iOS 앱의 잠재력을 계산할 수 있습니다.

금속 프레임 워크에는 다음과 같은 새로운 기능이 포함 되어 있습니다.

- OS X에 대 한 새로운 개인 및 깊이 스텐실 질감입니다.
- 깊이 고정 및 별도의 front 및 back 스텐실 값을 사용 하 여 그림자 품질을 개선 했습니다.
- 금속 음영 언어 및 금속 표준 라이브러리 개선
- 계산 셰이더는 더 넓은 범위의 픽셀 형식을 지원 합니다.

### <a name="the-metalkit-framework"></a>MetalKit 프레임 워크

MetalKit 프레임 워크는 iOS 앱에서 금속을 사용 하는 데 필요한 작업 양을 줄이는 유틸리티 클래스 및 기능 집합을 제공 합니다. MetalKit는 세 가지 주요 영역을 지원 합니다.

1. PNG, JPEG, KTX 및 PVR와 같은 일반적인 형식을 포함 하 여 다양 한 소스에서 로드 하는 비동기 질감입니다.
2. 금속 특정 모델 처리를 위한 모델 i/o 기반 자산에 쉽게 액세스할 수 있습니다. 이러한 기능은 모델 i/o 메시와 메탈 버퍼 간에 효율적인 데이터 전송을 제공 하도록 최적화 되었습니다.
3. IOS 앱 내에서 그래픽 렌더링을 표시 하는 데 필요한 코드의 양을 크게 줄여 주는 미리 정의 된 금속 보기 및 보기 관리

MetalKit에 대 한 자세한 내용은 Apple의 [MetalKit Framework 참조](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [금속 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) 및 [금속 음영 언어 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)를 참조 하세요.

### <a name="metal-performance-shaders-framework"></a>메탈 성능 셰이더 프레임 워크

금속 성능 셰이더 프레임 워크는 금속 기반 iOS 앱에서 사용 하기 위한 최적화 된 그래픽 및 계산 기반 셰이더 집합을 제공 합니다. 금속 성능 셰이더 프레임 워크의 각 셰이더는 지원 되는 완전 한 iOS Gpu에서 고성능을 제공 하도록 구체적으로 조정 되었습니다.

금속 성능 셰이더 클래스를 사용 하 여 개별 코드 베이스를 대상으로 지정 하 고 유지 관리할 필요 없이 각 특정 iOS GPU에서 가장 높은 성능을 달성할 수 있습니다. 금속 성능 셰이더는 질감 및 버퍼와 같은 금속 리소스와 함께 사용할 수 있습니다.

금속 성능 셰이더 프레임 워크는 다음과 같은 일반적인 셰이더 집합을 제공 합니다.

- **가우시안 흐림 효과** (`MPSImageGaussianBlur`)
- **Sobel Edge 검색** (`MPSImageSobel`)
- **이미지 히스토그램** (`MPSImageHistogram`)

자세한 내용은 Apple의 [금속 음영 언어 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)를 참조 하세요.

## <a name="introducing-model-io"></a>모델 i/o 소개

Apple의 모델 i/o 프레임 워크는 3D 자산 (예: 모델 및 관련 리소스)에 대 한 깊은 이해를 제공 합니다. 모델 i/o는 GameplayKit, 금속 및 SceneKit와 함께 사용할 수 있는 물리적 기반 재질, 모델 및 조명으로 iOS 게임을 제공 합니다.

모델 i/o를 사용 하면 다음과 같은 유형의 작업을 지원할 수 있습니다.

- 다양 한 인기 소프트웨어 및 게임 엔진 형식에서 조명, 재질, 메시 데이터, 카메라 설정 및 기타 장면 기반 정보를 가져옵니다.
- Procedurally 텍스처 하늘 domes 또는 굽기 조명 만들기와 같은 장면 기반 정보를 메시로 처리 하거나 생성 합니다.
- MetalKit, SceneKit 및가 중 키트와 함께 작동 하 여 렌더링을 위해 게임 자산을 GPU 버퍼로 효율적으로 로드 합니다.
- 자주 사용 하는 다양 한 소프트웨어 및 게임 엔진 형식으로 장면 기반 정보를 내보냅니다.

모델 i/o에 대해 자세히 알아보려면 Apple의 [모델 I/o 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421) 를 참조 하세요.

## <a name="introducing-replaykit"></a>ReplayKit 소개

Apple의 새 ReplayKit 프레임 워크를 사용 하면 게임 플레이 기록을 iOS 게임에 쉽게 추가 하 고 사용자가 앱 내에서이 비디오를 빠르고 쉽게 편집 하 고 공유할 수 있습니다.

자세한 내용은 Apple의 [replaykit 및 Game Center 비디오](https://developer.apple.com/videos/wwdc/2015/?id=605) 및 해당 [demobots를 참조 하세요. SpriteKit 및 GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 앱을 사용 하 여 플랫폼 간 게임을 빌드하는 중입니다.

## <a name="scenekit"></a>SceneKit

장면 키트는 3D 그래픽 작업을 간소화 하는 3D 장면 그래프 API입니다. OS X 10.8에 처음 도입 되었으며 이제 iOS 8로 제공 되었습니다. 장면 키트를 사용 하면 몰입 형 3D 시각화 및 일반 3D 게임을 만들 때 OpenGL의 전문 지식이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 기반으로 하는 장면 키트는 OpenGL 및 OpenGL ES의 복잡성을 추상화 하 여 응용 프로그램에 3D 콘텐츠를 매우 쉽게 추가할 수 있도록 합니다. 그러나 OpenGL 전문가 인 경우 장면 키트는 OpenGL에 직접 연결할 수 있는 뛰어난 지원을 제공 합니다. 또한 물리와 같은 3D 그래픽을 보완 하 고 핵심 애니메이션, 핵심 이미지 및 스프라이트 키트와 같은 다른 여러 Apple 프레임 워크와 매우 잘 통합 하는 다양 한 기능이 포함 되어 있습니다.

자세한 내용은 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서를 참조 하세요.

### <a name="scenekit-changes"></a>SceneKit 변경

Apple은 iOS 9 용 SceneKit에 다음과 같은 새 기능을 추가 했습니다.

- 이제 Xcode는 Xcode 내에서 직접 장면을 직접 편집 하 여 게임 및 대화형 3D 앱을 신속 하 게 빌드할 수 있는 장면 편집기를 제공 합니다.
- `SCNView` 및`SCNSceneRenderer` 클래스를 사용 하 여 지원 되는 iOS 장치에서 금속 렌더링을 사용 하도록 설정할 수 있습니다.
- `SCNAudioPlayer` 및`SCNNode` 클래스를 사용 하 여 플레이어 위치를 iOS 앱에 자동으로 추적 하는 공간 오디오 효과를 추가할 수 있습니다.

자세한 내용은 [SceneKit 설명서](~/ios/platform/introduction-to-ios8.md#scenekit) 및 Apple의 [SceneKit Framework 참조](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) 및 [Fox를 참조 하세요. Xcode 장면 편집기](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154) 샘플 프로젝트를 사용 하 여 SceneKit 게임을 빌드합니다.

## <a name="spritekit"></a>SpriteKit

Apple의 2D 게임 프레임 워크인 Sprite Kit에는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새로운 기능이 있습니다. 여기에는 장면 키트와의 통합, 셰이더 지원, 조명, 그림자, 제약 조건, 일반적인 맵 생성 및 물리학 향상이 포함 됩니다. 특히 새로운 물리학 기능을 통해 게임에 현실적인 효과를 매우 쉽게 추가할 수 있습니다.

자세한 내용은 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서를 참조 하세요.

### <a name="spritekit-changes"></a>SpriteKit 변경

Apple은 iOS 9 용 SpriteKit에 다음과 같은 새 기능을 추가 했습니다.

- `SKAudioNode` 클래스를 사용 하 여 플레이어의 위치를 자동으로 추적 하는 공간 오디오 효과입니다.
- 이제 Xcode는 2D 게임 및 앱을 쉽게 만들 수 있도록 장면 편집기와 작업 편집기를 제공 합니다.
- 새 카메라 노드 (`SKCameraNode`) 개체를 사용 하 여 손쉬운 스크롤 게임 지원.
- 금속을 지 원하는 iOS 장치에서 SpriteKit는 사용자 지정 OpenGL ES 셰이더를 이미 사용 하 고 있는 경우에도 렌더링에 자동으로 사용 됩니다.

자세한 내용은 [SpriteKit 설명서](~/ios/platform/introduction-to-ios8.md#spritekit) Apple의 [SpriteKit Framework 참조](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) 및 해당 [demobots를 참조 하세요. SpriteKit 및 GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 앱을 사용 하 여 플랫폼 간 게임을 빌드하는 중입니다.

## <a name="summary"></a>요약

이 문서에서는 iOS 9에서 Xamarin.ios 앱에 대해 제공 하는 새로운 게임 기능에 대해 살펴보았습니다.
GameplayKit 및 모델 i/o를 소개 했습니다. 금속의 주요 개선 사항 SceneKit 및 SpriteKit의 새로운 기능을 제공 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
