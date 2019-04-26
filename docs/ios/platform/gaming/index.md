---
title: iOS 게임 Api Xamarin.iOS에서
description: 이 문서에서는 Xamarin.iOS 게임 그래픽 및 오디오 기능을 개선 하기 위해 사용할 수 있는 iOS 9 제공한 새 게임 향상 된 기능을 설명 합니다.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: d8a531e495a19be7437d4a600e758028594248ab
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953326"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>iOS 게임 Api Xamarin.iOS에서

_이 문서에서는 Xamarin.iOS 게임 그래픽 및 오디오 기능을 개선 하기 위해 사용할 수 있는 iOS 9 제공한 새 게임 향상 된 기능을 설명 합니다._

Apple는 iOS 9에서에서 게임 Api 쉽게 Xamarin.iOS 앱에서 오디오 및 게임 그래픽을 구현 하는 몇 가지 기술적 향상을 했습니다.
여기에 고급 프레임 워크 및 향상 된 속도 및 그래픽 기능에 대 한 iOS 장치의 GPU의 강력한 기능 활용을 통해 개발의 용이성 모두 포함 됩니다.

[![](images/flocking01.png "떼지 실행 되는 앱의 예")](images/flocking01.png#lightbox)

SpriteKit 체제 미 설치 컴퓨터, SceneKit을 더욱 향상 기능과 함께 GameplayKit, ReplayKit, 모델 I/O, MetalKit 및 체제 미 설치 컴퓨터 성능 셰이더 포함 됩니다.

이 문서에서는 모든 iOS 9의 새로운 게임 향상 된 기능을 사용 하 여 Xamarin.iOS 게임을 개선 하는 방법을 소개 합니다.

## <a name="introducing-gameplaykit"></a>GameplayKit 소개

Apple 새 GameplayKit 프레임 워크에는 쉽게 구현 하는 데 필요한 반복 되는 일반적인 코드의 양을 줄여 iOS 장치용 게임을 만들 수 있도록 하는 기술 집합을 제공 합니다. GameplayKit 신속 하 게 하려면 다음 결합 될 수 있는 쉽게 (예: SceneKit 또는 SpriteKit) 그래픽 엔진을 사용 하 여 게임 메커니즘을 개발 하기 위한 도구 제공 완료 된 게임을 제공 합니다.

여러 일반적인 GameplayKit 포함, 게임 플레이 알고리즘 같은:

- 동작 기반, 이동 및 목표에는 AI는 함에 따라 자동으로 정의할 수 있도록 에이전트 시뮬레이션 합니다.
- 턴 기반 게임에 대 한 minmax 인공 지능 합니다.
- Emergent 동작을 제공 하도록 유사 항목 추론을 사용 하 여 데이터 기반 게임 논리에 대 한 규칙 시스템입니다.

또한 GameplayKit은 문서 블록 방식 게임 개발에는 다음과 같은 기능을 제공 하는 모듈식 아키텍처를 사용 하 여:

- 복잡 한 절차적 코드를 처리 하는 것에 대 한 상태 시스템 기반 게임에는 시스템.
- 도구를 제공 하는 것에 대 한 디버그 관련 문제가 발생 하지 않고 게임 플레이 예측 불가능성 임의입니다.
- 구성 요소화 된 엔터티를 재사용 가능한 아키텍처를 기반합니다.

GameplayKit에 대 한 자세한 내용은 Apple의를 참조 하세요 [Gameplaykit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) 하 고 [GameplayKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)합니다.

## <a name="gameplaykit-examples"></a>GameplayKit 예제

게임 플레이 키트를 사용 하 여 Xamarin.iOS 앱에 몇 가지 간단한 게임 메커니즘을 구현 빠른 살펴보겠습니다.

### <a name="pathfinding"></a>경로 찾기

경로 찾기 게임의 AI 요소에 대 한 해당 반대로 게임 보드를 찾을 수 있다는 점입니다.
예를 들어, 2D 적 미로 통해 서 또는 첫 번째-사용자-슈팅 world 지형 통해 3D 문자 찾기.

다음 맵에 것이 좋습니다.

[![](images/gkpathfindpath.png "예제 경로 찾기 지도")](images/gkpathfindpath.png#lightbox)

경로 찾기 사용 C# 코드 맵을 통해 방법을 찾을 수 있습니다.

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

### <a name="classical-expert-system"></a>고전 전문가 시스템

다음 코드 조각은 C# 코드 고전 전문가 시스템을 구현 하려면 GameplayKit를 사용할 수 있는 방법을 보여 줍니다.

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

지정된 된 규칙 집합에 따라 (`GKRule`) 및 입력 전문가 시스템의 알려진된 집합 (`GKRuleSystem`) 예측 가능한 출력 만들어집니다 (`fizzbuzz` 위의 예제에 대 한).

### <a name="flocking"></a>떼지

떼지 AI의 그룹 제어 게임 엔터티 그룹 이동 및 잠재 고객 엔터티를 새 떼의 진행에서 또는 유영 fish의 학교와 같은 작업에 응답 하는 위치를 떼도 작동 하도록 허용 합니다.

다음 코드 조각은 C# 코드 그래픽 표시에 대 한 GameplayKit 및 SpriteKit를 사용 하 여 떼지 동작을 구현 합니다.

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

그런 다음 뷰 컨트롤러에서이 장면을 구현 합니다.

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

를 실행할 때 약간 애니메이션 _"Boids"_ 우리의 손가락 탭 주위 flock 됩니다.

[![](images/flocking01.png "약간 애니메이션된 Boids 손가락 탭 주위 flock 됩니다.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>다른 Apple 예제

위의 샘플 외에도 Apple를 제공한 다음 샘플 앱으로 트랜스 코딩할 수 있는 C# Xamarin.iOS 및:

- [FourInARow: GameplayKit Minmax 전략가 사용 하 여 상대가 AI에 대 한](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: 에이전트 시스템을 사용 하 여 GameplayKit에서](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: SpriteKit GameplayKit와 플랫폼 간 게임 빌드](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

Ios 9에서 Apple에에 대 한 여러 변경 및 추가 체제 미 설치 컴퓨터 GPU에 오버 헤드가 낮은 액세스 권한을 제공 합니다. Metal을 사용 하는 그래픽 및 iOS 앱의 컴퓨팅 잠재력을 최대화할 수 있습니다.

다음과 같은 새로운 기능을 포함 하는 금속 프레임 워크:

- 새 개인 및 깊이 스텐실 텍스처 OS X에 대 한 합니다.
- 깊이 고정 및 별도 앞면과 뒷면 스텐실 값을 사용 하 여 향상 된 섀도 품질입니다.
- Metal 음영 언어 및 금속 표준 라이브러리 향상
- 계산 셰이더는 광범위 한 픽셀 형식 지원합니다.

### <a name="the-metalkit-framework"></a>MetalKit 프레임 워크

MetalKit 프레임 워크에는 유틸리티 클래스 및 iOS 앱에서 체제 미 설치 컴퓨터를 사용 하는 데 필요한 작업의 양을 줄일 수 있는 기능 집합을 제공 합니다. MetalKit에는 세 가지 주요 영역에 대 한 지원을 제공합니다.

1. 비동기 질감 다양 한 일반적인 형식은 PNG, JPEG, KTX PVR 등을 비롯 한 원본에서에서 로드 합니다.
2. 모델의 간편한 액세스에는 금속 특정 모델 처리에 대 한 자산 기반합니다. 이러한 기능은 항상 모델 I/O 메시과 Metal 버퍼 간의 효율적인 데이터 전송을 위해 최적화 되었습니다.
3. 미리 정의 된 Metal 뷰 및 iOS 앱 내에서 그래픽 렌더링을 표시 하는 데 필요한 코드의 양을 크게 줄일 뷰 관리 합니다.

MetalKit에 대 한 자세한 내용은 Apple의를 참조 하세요 [MetalKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [체제 미 설치 컴퓨터 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)하십시오 [금속 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) 및 [체제 미 설치 컴퓨터 음영 언어 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)합니다.

### <a name="metal-performance-shaders-framework"></a>Metal 성능 셰이더 프레임 워크

Metal 성능 셰이더 프레임 워크는 고도로 최적화 된 그래픽 집합을 제공 하 고 계산 기반 셰이더 프로그램 체제 미 설치 컴퓨터에서 사용 하기 위해 iOS 앱을 기반으로 합니다. Framework 체제 미 설치 컴퓨터에서 고성능을 제공 하도록 특별히 조정 되었습니다 체제 미 설치 컴퓨터 성능 셰이더에서 각 셰이더 Gpu iOS를 지원 합니다.

Metal 성능 셰이더 클래스를 사용 하 여 대상 및 개별 코드 베이스를 유지 관리 하지 않고도 각 특정 iOS GPU에서 가능한 가장 높은 성능을 얻을 수 있습니다. Metal 성능 셰이더는 질감 및 버퍼와 같은 모든 Metal 리소스를 사용 하 여 사용할 수 있습니다.

Metal 성능 셰이더 프레임 워크와 같은 일반적인 셰이더는 집합을 제공합니다.

- **가우스 흐리게** (`MPSImageGaussianBlur`)
- **Sobel 가장자리 검출** (`MPSImageSobel`)
- **히스토그램 이미지** (`MPSImageHistogram`)

자세한 내용은 Apple의를 참조 하세요 [Metal 음영 언어 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)합니다.

## <a name="introducing-model-io"></a>모델 I/O를 소개합니다.

Apple의 모델 I/O framework 3D 자산 (예: 모델 및 해당 관련된 리소스)에 대 한 깊은 이해를 제공합니다. 모델 I/O 물리적 기반 자료, 모델 및 조명 GameplayKit, 금속 및 SceneKit을 사용 하 여 사용할 수 있는 사용 하 여 iOS 게임을 제공 합니다.

모델 I/O를 사용 하 여 다음과 같은 유형의 작업을 지원할 수 있습니다.

- 조명, 자료를 가져오려면, 데이터, 카메라 설정 및 기타 다양 한 인기 있는 소프트웨어 및 게임 엔진 형식 장면 기반 정보 메시입니다.
- 프로세스 또는 하늘 돔 또는 메시를 조명 적용 질감 절차적 만들기와 같은 장면 기반 정보를 생성 합니다.
- 와 MetalKit SceneKit GLKit 렌더링에 대 한 GPU 버퍼 게임 자산 로드 효율적으로 작동 합니다.
- 다양 한 인기 있는 소프트웨어 및 게임 엔진 형식 장면 기반 정보를 내보냅니다.

모델 I/O에 대 한 자세한 내용은 Apple의를 참조 하십시오 [모델 I/O 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>ReplayKit 소개

새 Apple ReplayKit 프레임 워크를 사용 하면 iOS 게임의 게임 기록을 추가할 쉽고 빠르고 쉽게 편집 및 앱 내에서이 비디오를 공유할 수 있도록 수 있습니다.

자세한 내용은 Apple의를 참조 하세요 [ReplayKit Game Center 비디오를 사용 하 여 소셜 이동](https://developer.apple.com/videos/wwdc/2015/?id=605) 및 해당 [DemoBots: SpriteKit GameplayKit와 크로스 플랫폼 게임 제작](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 앱입니다.

## <a name="scenekit"></a>SceneKit

장면 키트는 3D 장면 그래프 3D 그래픽 작업을 간소화 하는 API입니다. OS X 10.8에 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. Scene Kit를 사용 하 여 몰입 감이 뛰어난 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL에 대 한 전문이 필요 하지 않습니다. 일반적인 장면 그래프 개념을 토대로 Scene Kit 추상화 OpenGL 및 OpenGL ES를 매우 간편 하 게 추가 3D 콘텐츠 응용 프로그램의 복잡성입니다. 그러나 OpenGL 전문가 라면 Scene Kit는도 OpenGL을 사용 하 여 직접 시도 하는 것에 대 한 훌륭한 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 Apple 등 다른 프레임 워크, Core Animation, Core 이미지 및 Sprite Kit를 사용 하 여 매끄럽게 통합 되 합니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서.

### <a name="scenekit-changes"></a>SceneKit 변경

Apple는 iOS 9 용 SceneKit에 다음과 같은 새로운 기능을 추가:

- 이제 Xcode을 빠르게 Xcode 내에서 백그라운드에서 직접 편집 하 여 게임 및 대화형 3D 응용 프로그램을 빌드할 수 있도록 장면 편집기를 제공 합니다.
- 합니다 `SCNView` 고 `SCNSceneRenderer` Metal 렌더링 (지원 되는 iOS 장치)에서 사용할 수 있도록 클래스를 사용할 수 있습니다.
- 합니다 `SCNAudioPlayer` 고 `SCNNode` 클래스를 사용 하 여 자동으로 iOS 앱에는 플레이어 위치를 추적 하는 공간 오디오 효과 추가할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit 설명서](~/ios/platform/introduction-to-ios8.md#scenekit) 과 Apple [SceneKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) 고 [Fox: SceneKit 게임 Xcode 장면 편집기를 사용 하 여 제작](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154) 샘플 프로젝트입니다.

## <a name="spritekit"></a>SpriteKit

Sprite Kit를 Apple에서 2D 게임 프레임 워크에는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새 기능에 있습니다. 여기에 Scene Kit, 셰이더 지원, 조명, 그림자, 제약 조건, 법선 맵 생성 및 물리학 향상 된 기능을 사용 하 여 통합이 포함 됩니다. 특히 물리 기능과 매우 쉽게 게임에 실제 효과를 추가 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서.

### <a name="spritekit-changes"></a>SpriteKit 변경

Apple는 iOS 9 용 SpriteKit에 다음과 같은 새로운 기능을 추가:

- 자동으로 사용 하 여 플레이어의 위치를 추적 하는 공간 오디오 효과 `SKAudioNode` 클래스입니다.
- Xcode는 이제 장면 편집기를 쉽게 2D 게임 및 앱 만들기에 대 한 작업 편집기 기능입니다.
- 쉽게 새 카메라 노드를 사용 하 여 게임 지원 스크롤 (`SKCameraNode`) 개체입니다.
- 체제 미 설치 컴퓨터를 지 원하는 iOS 장치에서 SpriteKit를 자동으로 사용 렌더링을 위해 사용자 지정 OpenGL ES 셰이더를 이미 사용한 경우에 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit 설명서](~/ios/platform/introduction-to-ios8.md#spritekit) Apple [SpriteKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) 및 해당 [DemoBots: SpriteKit GameplayKit와 크로스 플랫폼 게임 제작](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 앱입니다.

## <a name="summary"></a>요약

이 문서에서는 이러한 새 부분이 Xamarin.iOS 앱에 대 한 게임 기능은 iOS 9 제공 합니다.
GameplayKit 모델 I/O를 도입 금속;의 주요 향상 된 기능 및 SceneKit 및 SpriteKit의 새로운 기능을 추가 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
