---
title: iOS Xamarin.iOS의 게임 Api
description: 이 문서에서는 Xamarin.iOS 게임의 그래픽 및 오디오 기능을 개선 하기 위해 사용할 수 있는 iOS 9 제공한 새 게임 향상 된 기능을 설명 합니다.
ms.prod: xamarin
ms.assetid: 958D38FD-9240-482E-9A42-D6671ED8F2B0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: 263c325816867e9eee32c92edf97f703b39bda7c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786862"
---
# <a name="ios-gaming-apis-in-xamarinios"></a>iOS Xamarin.iOS의 게임 Api

_이 문서에서는 Xamarin.iOS 게임의 그래픽 및 오디오 기능을 개선 하기 위해 사용할 수 있는 iOS 9 제공한 새 게임 향상 된 기능을 설명 합니다._

Apple는 iOS 9에에서 Api 게임을 보다 쉽게 Xamarin.iOS 앱에서 게임 그래픽 및 오디오를 구현할 수 있도록 몇 가지 기술적 개선을 했습니다.
여기에 두 간편한 개발을 통해 높은 수준의 프레임 워크 및 향상 된 속도 그래픽 기능에 대 한 iOS 장치 GPU의 기능을 통해 포함 됩니다.

[![](images/flocking01.png "떼지 실행 되는 앱의 예")](images/flocking01.png#lightbox)

여기에 GameplayKit "," ReplayKit "," 모델 I/O "," MetalKit "및" Metal 성능 셰이더 금속, SceneKit 및 SpriteKit 더욱 향상 기능입니다.

이 문서의 모든 iOS 9의 게임 향상 된 기능을 사용 하 여 Xamarin.iOS 게임을 향상 시키는 방법을 소개 합니다.

## <a name="introducing-gameplaykit"></a>GameplayKit 소개

Apple의 새로운 GameplayKit 프레임 워크는 구현에 필요한 반복적인, 일반적인 코드의 양을 줄여 iOS 장치에 대 한 게임을 작성을 용이 하 게 하는 일련의 기술을 제공 합니다. GameplayKit를 신속 하 게 (예: SceneKit 또는 SpriteKit)는 그래픽 엔진와 쉽게 조합 될 다음 게임 메커니즘을 개발 하기 위한 도구 제공 완료 게임을 제공 합니다.

일반적인 여러 GameplayKit 포함, 게임와 같은 알고리즘을 재생:

- 동작을 기반으로 동작과 AI는 자동으로 진행 하는 목표를 정의할 수 있도록 하는 에이전트 시뮬레이션 합니다.
- 턴 기반 게임에 대 한 minmax 인공 인텔리전스
- 새 동작을 제공 하도록 유사 항목 원리와 데이터 기반 게임 논리는 규칙 시스템입니다.

또한 GameplayKit 걸립니다 빌딩 블록 방법을 게임 개발에는 다음과 같은 기능을 제공 하는 모듈식 아키텍처를 사용 하 여:

- 복잡 한, 절차적 코드를 처리 하기 위한 상태 시스템 기반 게임 플레이 시스템.
- 도구를 제공 하는 데 임의 디버깅 문제를 일으키지 않고 게임와 예측 불가능성을 제공 합니다.
- 다시 사용할 수 있는, 즉 엔터티 기반 아키텍처입니다.

GameplayKit에 대 한 자세한 내용은 Apple의를 참조 하십시오 [Gameplaykit 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/GameplayKit_Guide/index.html#//apple_ref/doc/uid/TP40015172) 및 [GameplayKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/GameplayKit/Reference/GameplayKit_Framework/index.html#//apple_ref/doc/uid/TP40015199)합니다.

## <a name="gameplaykit-examples"></a>GameplayKit 예제

게임 키트를 사용 하 여 Xamarin.iOS 앱에서 몇 가지 간단한 게임 메커니즘을 구현 빠른 살펴보겠습니다.

### <a name="pathfinding"></a>경로 찾기

경로 찾기 게임 AI 요소에 대 한 그 반대의 게임 보드를 찾을 수 있다는 점입니다.
예를 들어 2D 적 미로 통해 또는 첫 번째-사용자-해결사 세계 지형을 통해 3D 문자를 찾기.

다음과 같은 맵을 고려해 야 합니다.

[![](images/gkpathfindpath.png "예제 경로 찾기 맵")](images/gkpathfindpath.png#lightbox)

경로 찾기를 사용 하 여이 C# 코드 맵을 통해는 방법을 찾을 수 있습니다.:

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

C# 코드의 다음 코드 조각은 고전 전문가 시스템을 구현 하 GameplayKit를 사용할 수 있는 방법을 보여 줍니다.

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

지정 된 집합이 규칙에 따라 (`GKRule`) 및 입력을 전문가 시스템 알려진된 집합 (`GKRuleSystem`) 예측 가능한 출력을 만듭니다 (`fizzbuzz` 위의 예에 대 한).

### <a name="flocking"></a>떼지

떼지 AI의 그룹 제어 게임 엔터티 그룹 이동 및는 새 떼의 경우 처리 중에서 또는 수영 어 학교와 같은 잠재 고객 엔터티의 동작에 응답 하는 위치는 떼도 작동 하도록 허용 합니다.

C# 코드의 다음 코드 조각은 GameplayKit 및 SpriteKit를 사용 하 여 그래픽 표시를 위해 떼지 동작을 구현 합니다.

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

그런 다음이 장면 뷰 컨트롤러에서 구현:

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

[![](images/flocking01.png "애니메이션 효과 준된 약간 Boids 손가락 탭 주위 flock 됩니다.")](images/flocking01.png#lightbox)

### <a name="other-apple-examples"></a>다른 Apple 예제

위의 샘플 이외에 Apple Xamarin.iOS 및 C#를 코드 변환 될 수 있는 다음 샘플 앱을 제공 했습니다.

- [FourInARow: GameplayKit Minmax 전략가 사용 하 여 상대가 AI에 대 한](https://developer.apple.com/library/prerelease/ios/samplecode/FourInARow/Introduction/Intro.html#//apple_ref/doc/uid/TP40016142)
- [AgentsCatalog: GameplayKit의 에이전트 시스템 사용](https://developer.apple.com/library/prerelease/ios/samplecode/AgentsCatalog/Introduction/Intro.html#//apple_ref/doc/uid/TP40016141)
- [DemoBots: SpriteKit GameplayKit와 플랫폼 간 게임 작성](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179)

## <a name="metal"></a>Metal

IOS 9에에서 Apple에 수행한 몇 가지 변경 내용과 추가 금속 GPU에 오버 헤드가 적은 액세스 권한을 제공 합니다. 복구를 사용 하 여 그래픽 및 iOS 앱의 컴퓨팅 잠재력을 최대화할 수 있습니다.

금속 프레임 워크에는 다음과 같은 새로운 기능이 포함 됩니다.

- 새 개인 및 깊이 스텐실 텍스처 OS X에 대 한 합니다.
- 향상 된 그림자 품질 깊이 고정할 및 별도 전면 및 후면 스텐실 값입니다.
- 금속 음영 언어 및 표준 라이브러리 금속 향상
- 계산 셰이더 픽셀 형식 보다 넓은 범위의 지원합니다.

### <a name="the-metalkit-framework"></a>MetalKit 프레임 워크

MetalKit 프레임 워크는 iOS 앱에서 복구를 사용 하는 데 필요한 작업의 양을 줄일 수 있는 기능 및 유틸리티 클래스 집합을 제공 합니다. MetalKit에는 세 가지 주요 영역에 대 한 지원을 제공합니다.

1. 비동기 질감은 여러 가지 일반적인 형식 PNG, JPEG, KTX PVR 등을 포함 하 여 소스에서에서 로드 합니다.
2. 쉽게 액세스할 수 있는 모델 I/O 금속 특정 모델 처리를 위해 자산을 기반 으로합니다. 이러한 기능은 모델 I/O 메시와 금속 버퍼 간의 효율적인 데이터 전송을 제공 하기 위해 항상 최적화 되었습니다.
3. 미리 정의 된 금속 뷰와 그래픽 렌더링 iOS 앱 내에서 표시 하는 데 필요한 코드의 양을 상당히 줄일 뷰 관리 합니다.

MetalKit에 대 한 자세한 내용은 Apple의를 참조 하십시오 [MetalKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/MetalKit/Reference/MTKFrameworkReference/index.html#//apple_ref/doc/uid/TP40015356), [금속 프로그래밍 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221), [금속 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalFrameworkReference/index.html#//apple_ref/doc/uid/TP40014161) 및 [금속 언어 가이드 음영](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)합니다.

### <a name="metal-performance-shaders-framework"></a>금속 성능 셰이더 프레임 워크

성능 셰이더 금속 프레임 워크 그래픽 최적화 된 집합을 제공 및 계산 기반 셰이더 프로그램 금속에서 사용 하기 위해 iOS 앱을 기반으로 합니다. 프레임 워크 금속에 높은 성능을 제공 하도록 구체적으로 조정 되었습니다 금속 성능 셰이더에 각 셰이더 Gpu iOS를 지원 합니다.

금속 성능 셰이더 클래스를 사용 하 여 대상으로 하 여 개별 코드 베이스를 유지 관리할 필요 없이 각 특정 iOS GPU에서 가능한 최고의 성능을 얻을 수 있습니다. 예: 질감 버퍼 금속 리소스와 금속 성능 셰이더를 사용할 수 있습니다.

성능 셰이더 금속 프레임 워크와 같은 일반적인 셰이더 집합을 제공합니다.

- **가우스 흐림 효과** (`MPSImageGaussianBlur`)
- **Sobel 가장자리 검출** (`MPSImageSobel`)
- **히스토그램 이미지** (`MPSImageHistogram`)

자세한 내용은 Apple의를 참조 하십시오 [금속 음영 언어 가이드](https://developer.apple.com/library/prerelease/ios/documentation/Metal/Reference/MetalShadingLanguageGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014364)합니다.

## <a name="introducing-model-io"></a>모델 I/O 소개

Apple의 모델 I/O 프레임 워크 (예: 모델 및 해당 관련된 리소스) 3D 자산을 깊이 이해를 제공합니다. 모델 I/O 물리적 기반 자료, 모델 및 GameplayKit, 금속 SceneKit와 사용할 수 있는 조명와 함께 iOS 게임을 제공 합니다.

모델 I/O로 다음과 같은 유형의 작업을 지원할 수 있습니다.

- 데이터, 카메라 설정 및 다양 한 인기 있는 소프트웨어 및 게임 엔진 형식에서에서 다른 장면 기반 정보 메시를 조명 자료 가져와서 합니다.
- 프로세스 또는 sky 돔 또는 메시에 조명 빵 할인 질감 절차에 따라 만들기와 같은 장면 기반 정보를 생성 합니다.
- MetalKit와 SceneKit GLKit 효율적으로 렌더링 하기 위한 GPU 버퍼에 게임 자산을 로드 하는 작동 합니다.
- 다양 한 인기 있는 소프트웨어 및 게임 엔진 형식을를 장면 기반 정보를 내보냅니다.

모델 I/O에 대 한 자세한 내용은 Apple의를 참조 하십시오 [모델 I/O 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/ModelIO/Reference/ModelIO_Framework/index.html#//apple_ref/doc/uid/TP40015421)

## <a name="introducing-replaykit"></a>ReplayKit 소개

Apple의 새로운 ReplayKit 프레임 워크를 사용 하면 쉽게 iOS 게임에 게임의 기록을 추가 하 고 사용자가 신속 하 고 쉽게 편집 및 응용 프로그램 내에서이 비디오를 공유 하도록 허용 수 있습니다.

자세한 내용은 Apple의를 참조 하십시오 [ReplayKit 및 게임 센터 비디오 소셜 이동](https://developer.apple.com/videos/wwdc/2015/?id=605) 및 해당 [DemoBots: SpriteKit 및 GameplayKit 크로스 플랫폼 게임 작성](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 응용 프로그램입니다.

## <a name="scenekit"></a>SceneKit

장면 키트 3D 장면 graph API 작업 3D 그래픽을 간소화 하는입니다. OS X 10.8에서 처음 도입 하 고 iOS 8 하기 위해 이제 왔습니다. 장면 키트 몰입 3D 시각화 및 아마추어 3D 게임을 만들기는 OpenGL 관련 지식이 필요 하지 않습니다. 일반적인 장면 그래프 개념에 대해 빌드, 장면 키트에서 벗어난 OpenGL 및 OpenGL ES 매우 간편 하 게 추가 3D 콘텐츠를 응용 프로그램의 복잡성을 합니다. 그러나 OpenGL 전문가 인 경우 장면 키트는도 OpenGL을 사용 하 여 직접 전후해 서 연결에 대 한 뛰어난 지원 합니다. 또한 물리학 같은 3D 그래픽을 보완 하는 다양 한 기능을 포함 하 고 몇 가지 다른 Apple 프레임 워크 Sprite 키트 코어 애니메이션, Core 이미지 등 매우 잘 통합 합니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit](~/ios/platform/gaming/scenekit.md) 설명서입니다.

### <a name="scenekit-changes"></a>SceneKit 변경 내용

Apple는 iOS 9에 대 한 SceneKit에 다음과 같은 새로운 기능을 추가 했습니다.

- Xcode는 이제 Xcode 내에서 백그라운드에서 직접 편집 하 여 게임 및 3D 대화형 응용 프로그램을 신속 하 게 만들 수 있는 장면 편집기를 제공 합니다.
- `SCNView` 및 `SCNSceneRenderer` 금속 렌더링 (지원 되는 iOS 장치)에서 사용할 수 있도록 클래스를 사용할 수 있습니다.
- `SCNAudioPlayer` 및 `SCNNode` 자동으로 iOS 앱에 플레이어 위치를 추적 하는 공간 오디오 효과 추가 하는 클래스를 사용할 수 있습니다.

자세한 내용은 참조 하십시오 우리의 [SceneKit 설명서](~/ios/platform/introduction-to-ios8.md#scenekit) 과 Apple [SceneKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/SceneKit/Reference/SceneKit_Framework/index.html#//apple_ref/doc/uid/TP40012283) 및 [Fox: SceneKit 게임 Xcode 장면 편집기와작성](https://developer.apple.com/library/prerelease/ios/samplecode/Fox/Introduction/Intro.html#//apple_ref/doc/uid/TP40016154)샘플 프로젝트입니다.

## <a name="spritekit"></a>SpriteKit

Sprite 키트, Apple에서 2D 게임 프레임 워크는 iOS 8 및 OS X Yosemite의 몇 가지 흥미로운 새로운 기능에 있습니다. 여기에 장면 키트, 셰이더 지원, 조명, 그림자, 제약 조건, 기본 맵 생성 및 물리학 향상 된 기능 통합을 포함 합니다. 특히, 새로운 물리 기능 매우 쉽게 게임에 실제 효과를 추가 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit](~/ios/platform/gaming/spritekit.md) 설명서입니다.

### <a name="spritekit-changes"></a>SpriteKit 변경 내용

Apple는 iOS 9에 대 한 SpriteKit에 다음과 같은 새로운 기능을 추가 했습니다.

- 플레이어의 위치를 자동으로 추적 하는 공간 오디오 효과 `SKAudioNode` 클래스입니다.
- Xcode는 이제 장면 편집기와 쉽게 2D 게임 및 앱 만들기에 대 한 작업 편집기 특징입니다.
- 새 카메라 노드를 사용 하 여 게임 지원을 쉽게 스크롤할 (`SKCameraNode`) 개체입니다.
- 복구를 지 원하는 iOS 장치에서 SpriteKit 자동으로 사용 합니다 렌더링을 사용자 지정 OpenGL ES 셰이더를 이미 사용 하는 경우에 합니다.

자세한 내용은 참조 하십시오 우리의 [SpriteKit 설명서](~/ios/platform/introduction-to-ios8.md#spritekit) Apple의 [SpriteKit 프레임 워크 참조](https://developer.apple.com/library/prerelease/ios/documentation/SpriteKit/Reference/SpriteKitFramework_Ref/index.html#//apple_ref/doc/uid/TP40013041) 및 해당 [DemoBots: SpriteKit 사용 크로스 플랫폼 게임 작성 및 GameplayKit](https://developer.apple.com/library/prerelease/ios/samplecode/DemoBots/Introduction/Intro.html#//apple_ref/doc/uid/TP40015179) 샘플 응용 프로그램입니다.

## <a name="summary"></a>요약

이 문서의 새 검사가 수행 게임 기능 Xamarin.iOS 앱에 대해 제공 하는 iOS 9입니다.
GameplayKit 및 모델 I/O를 도입 금속;의 주요 향상 된 기능 및 SceneKit 및 SpriteKit의 새로운 기능을 추가 합니다.



## <a name="related-links"></a>관련 링크

- [iOS 9 샘플](https://developer.xamarin.com/samples/ios/iOS9/)
- [개발자를 위한 iOS 9](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
