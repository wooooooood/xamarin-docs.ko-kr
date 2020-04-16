---
title: Xamarin.iOS 앱 단위 테스트
description: 이 문서에서는 Xamarin.iOS 애플리케이션을 단위 테스트하는 방법의 개요를 제공합니다. 테스트를 작성하고 테스트를 실행하는 단위 테스트 프로젝트를 만드는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: BD959779-3239-79B6-5289-3A9ECDFBD973
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 08ddf282c8839a6283b90c0736c0b4259bd01469
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73028453"
---
# <a name="unit-testing-xamarinios-apps"></a>Xamarin.iOS 앱 단위 테스트

이 문서에서는 Xamarin.iOS 프로젝트의 단위 테스트를 만드는 방법을 설명합니다.
Xamarin.iOS를 사용한 단위 테스트는 Touch.Unit 프레임워크를 사용하여 수행됩니다. 여기에는 단위 테스트를 작성하기 위한 친숙한 API 집합을 제공하는 [Touch.Unit](https://github.com/xamarin/Touch.Unit)이라는 NUnit의 수정된 버전 외에도 iOS 테스트 실행기가 모두 포함되어 있습니다.

## <a name="setting-up-a-test-project-in-visual-studio-for-mac"></a>Mac용 Visual Studio에서 테스트 프로젝트 설정

프로젝트에 대한 단위 테스트 프레임워크를 설정하려면 **iOS 단위 테스트 프로젝트** 유형의 프로젝트를 솔루션에 추가하기만 하면 됩니다. 이렇게 하려면 솔루션을 마우스 오른쪽 단추로 클릭하고 **추가 > 새 프로젝트 추가**를 차례로 선택합니다. 목록에서 **iOS > 테스트 > 통합 API > iOS 단위 테스트 프로젝트**(C# 또는 F# 중 하나를 선택할 수 있음)를 차례로 선택합니다.

![](touch.unit-images/00.png "Choose either C# or F#")

위에서는 기본 실행기 프로그램을 포함하고 새 MonoTouch.NUnitLite 어셈블리를 참조하는 기본 프로젝트를 만듭니다. 이 프로젝트는 다음과 같습니다.

![](touch.unit-images/01.png "The project in the Solution Explorer")

`AppDelegate.cs` 클래스에는 테스트 실행기가 포함되어 있으며 다음과 같습니다.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TouchRunner runner;

    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        // create a new window instance based on the screen size
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        runner = new TouchRunner (window);

        // register every tests included in the main application/assembly
        runner.Add (System.Reflection.Assembly.GetExecutingAssembly ());

        window.RootViewController = new UINavigationController (runner.GetViewController ());

        // make the window visible
        window.MakeKeyAndVisible ();

        return true;
    }
}
```

> [!NOTE]
> Windows의 Visual Studio 2019 또는 Visual Studio 2017에서는 iOS 단위 테스트 프로젝트 형식을 사용할 수 없습니다.

## <a name="writing-some-tests"></a>일부 테스트 작성

기본 셸이 제대로 준비되었으므로 첫 번째 테스트 집합을 작성해야 합니다.

테스트는 `[TestFixture]` 특성이 적용된 클래스를 만들어 작성됩니다. 각 TestFixture 클래스 내에서 테스트 실행기가 호출할 모든 메서드에 `[Test]` 특성을 적용해야 합니다. 실제 테스트 픽스쳐는 테스트 프로젝트의 모든 파일에 있을 수 있습니다.

빨리 시작하려면 **추가/새 파일 추가**를 선택하고 **UnitTests** Xamarin.iOS 그룹을 선택합니다. 그러면 통과한 테스트, 실패한 테스트 및 무시한 테스트가 하나씩 포함된 기본 구조 파일이 추가되며 다음과 같습니다.

```csharp
using System;
using NUnit.Framework;

namespace Fixtures {

    [TestFixture]
    public class Tests {

        [Test]
        public void Pass ()
        {
                Assert.True (true);
        }

        [Test]
        public void Fail ()
        {
                Assert.False (true);
        }

        [Test]
        [Ignore ("another time")]
        public void Ignore ()
        {
                Assert.True (false);
        }
    }
}
```

## <a name="running-your-tests"></a>테스트 실행

솔루션 내에서 이 프로젝트를 실행하려면, 마우스 오른쪽 단추로 클릭하고 **항목 디버그** 또는 **항목 실행**을 선택합니다.

테스트 실행기를 사용하면 등록된 테스트를 확인하고 실행할 수 있는 테스트를 개별적으로 선택할 수 있습니다.

[![](touch.unit-images/02-sml.png "The list of registered tests")](touch.unit-images/02.png#lightbox) 
[![](touch.unit-images/03-sml.png "An individual text")](touch.unit-images/03.png#lightbox) 

[![](touch.unit-images/04-sml.png "The run results")](touch.unit-images/04.png#lightbox)

중첩된 뷰에서 테스트 픽스쳐를 선택하여 개별 테스트 픽스쳐를 실행하거나, "모든 항목 실행"으로 모든 테스트를 실행할 수 있습니다. 기본 테스트를 실행하는 경우 통과한 테스트, 실패한 테스트 및 무시한 테스트를 하나씩 포함하도록 되어 있습니다. 보고서는 다음과 같이 표시되며, 실패한 테스트를 직접 드릴다운하여 실패에 대한 자세한 정보를 확인할 수 있습니다.

[![](touch.unit-images/05-sml.png "샘플 보고서")](touch.unit-images/05.png#lightbox) [![](touch.unit-images/06-sml.png "샘플 보고서")](touch.unit-images/06.png#lightbox) [![](touch.unit-images/07-sml.png "샘플 보고서")](touch.unit-images/07.png#lightbox)

또한 IDE에서 애플리케이션 출력 창을 통해 실행 중인 테스트와 현재 상태를 확인할 수도 있습니다.

## <a name="writing-new-tests"></a>새 테스트 작성

NUnitLite는 [Touch.Unit](https://github.com/xamarin/Touch.Unit) 프로젝트라고 하는 NUnit의 수정된 버전입니다. [NUnit](https://nunit.com/)의 아이디어를 기반으로 하고 해당 기능의 일부를 제공하는 간단한 .NET 테스트 프레임워크입니다.
최소한의 리소스를 사용하며, 포함 및 모바일 개발에 사용되는 플랫폼과 같은 리소스 제한 플랫폼에서 실행됩니다. NUnitLite API는 Xamarin.iOS에서 사용할 수 있습니다. 단위 테스트 템플릿에서 제공하는 기본 구조를 사용하면 주 진입점은 [Assert 클래스](xref:NUnit.Framework.Assert) 메서드가 됩니다.

Assert 클래스 메서드 외에도 단위 테스트 기능은 NUnitLite의 일부인 다음 네임스페이스에서 분할됩니다.

- [NUnit.Framework](xref:NUnit.Framework)
- [NUnit.Constraints](xref:NUnit.Framework.Constraints)
- [NUniteLite.Runner](xref:NUnitLite.Runner)
