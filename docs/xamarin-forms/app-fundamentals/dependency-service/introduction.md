---
title: DependencyService 소개
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스에서 네이티브 플랫폼 기능에 액세스하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 3c8cc31c21f354b60001cefb919b51bf4d42da9f
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675018"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService 소개

## <a name="overview"></a>개요

[`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하면 애플리케이션이 공유 코드에서 플랫폼별 기능을 호출할 수 있습니다. 이 기능을 통해 Xamarin.Forms 애플리케이션에서 네이티브 애플리케이션의 모든 작업을 수행할 수 있습니다.

`DependencyService`는 서비스 로케이터입니다. 실제로 인터페이스가 정의되고 `DependencyService`가 다양한 플랫폼 프로젝트에서 해당 인터페이스의 올바른 구현을 찾습니다.

> [!NOTE]
> 기본적으로 [`DependencyService`](xref:Xamarin.Forms.DependencyService)는 매개 변수가 없는 생성자가 있는 플랫폼 구현만 확인합니다. 그러나 종속성 확인 메서드는 종속성 주입 컨테이너 또는 팩터리 메서드를 사용하여 플랫폼 구현을 확인하는 Xamarin.Forms에 주입할 수 있습니다. 이 방법은 매개 변수가 있는 생성자가 있는 가진 플랫폼 구현을 확인하는 데 사용할 수 있습니다. 자세한 내용은 [Xamarin.Forms에서 종속성 확인](~/xamarin-forms/internals/dependency-resolution.md)을 참조하세요.

## <a name="how-dependencyservice-works"></a>DependencyService의 작동 방식

Xamarin.Forms 애플리케이션에서 `DependencyService`를 사용하는 데 필요한 네 가지 구성 요소는 다음과 같습니다.

- **인터페이스** &ndash; 필요한 기능은 공유 코드의 인터페이스에서 정의됩니다.
- **플랫폼별 구현** &ndash; 인터페이스를 구현하는 각 클래스는 각 플랫폼 프로젝트에 추가해야 합니다.
- **등록** &ndash; 각 구현 클래스는 메타데이터 특성을 통해 `DependencyService`에 등록해야 합니다. `DependencyService`에서 등록을 통해 실행 클래스를 찾아 런타임에 인터페이스 대신 이를 제공할 수 있습니다.
- **DependencyService 호출** &ndash; 공유 코드에서 `DependencyService`를 명시적으로 호출하여 인터페이스의 구현을 요청해야 합니다.

솔루션에서 각 플랫폼 프로젝트에 대한 구현을 제공해야 합니다. 구현이 없는 플랫폼 프로젝트는 런타임에 실패하게 됩니다.

애플리케이션의 구조는 다음 다이어그램과 같습니다.

![](introduction-images/overview-diagram.png "DependencyService 애플리케이션 구조")

### <a name="interface"></a>인터페이스

설계하는 인터페이스는 플랫폼별 기능과 상호 작용하는 방법을 정의합니다. 구성 요소 또는 NuGet 패키지로 공유할 구성 요소를 개발하는 경우 주의하세요. API 디자인은 패키지를 만들거나 손상시킬 수 있습니다. 아래의 예제에서는 말하는 단어를 유연하게 지정할 수 있는 간단한 텍스트 말하기 인터페이스를 지정하지만 각 플랫폼에 맞게 구현을 사용자 지정할 수 있는 상태로 둡니다.

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>플랫폼별 구현

적절한 인터페이스가 설계되면 해당 인터페이스가 대상으로 하는 각 플랫폼에 맞게 프로젝트에 구현되어야 합니다. 예를 들어 다음 클래스는 iOS에서 `ITextToSpeech` 인터페이스를 구현합니다.

```csharp
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

### <a name="registration"></a>등록

인터페이스의 각 구현은 메타데이터 특성을 사용하여 `DependencyService`에 등록해야 합니다. iOS에 대한 구현을 등록하는 코드는 다음과 같습니다.

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

이를 모두 결합한 플랫폼별 구현은 다음과 같습니다.

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
    public class TextToSpeech_iOS : ITextToSpeech
    {
        public void Speak (string text)
        {
            var speechSynthesizer = new AVSpeechSynthesizer ();

            var speechUtterance = new AVSpeechUtterance (text) {
                Rate = AVSpeechUtterance.MaximumSpeechRate/4,
                Voice = AVSpeechSynthesisVoice.FromLanguage ("en-US"),
                Volume = 0.5f,
                PitchMultiplier = 1.0f
            };

            speechSynthesizer.SpeakUtterance (speechUtterance);
        }
    }
}
```

참고: 등록은 클래스 수준이 아니라 네임스페이스 수준에서 수행됩니다.

#### <a name="universal-windows-platform-net-native-compilation"></a>유니버설 Windows 플랫폼 .NET 네이티브 컴파일

.NET 네이티브 컴파일 옵션을 사용하는 UWP 프로젝트는 Xamarin.Forms를 초기화할 때 [약간 다른 구성](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception)을 따라야 합니다. 또한 .NET 네이티브 컴파일에는 종속성 서비스에 대해 약간 다른 등록이 필요합니다.

**App.xaml.cs** 파일에서 아래와 같이 `Register<T>` 메서드를 사용하여 UWP 프로젝트에 정의된 각 종속성 서비스를 수동으로 등록합니다.

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

참고: `Register<T>`를 사용하는 수동 등록은 .NET 네이티브 컴파일을 사용하는 릴리스 빌드에서만 적용됩니다. 이 줄을 생략하면 디버그 빌드는 계속 작동하지만 릴리스 빌드는 종속성 서비스를 로드하지 못합니다.

### <a name="call-to-dependencyservice"></a>DependencyService 호출

프로젝트가 각 플랫폼에 대한 공통 인터페이스 및 구현으로 설정되면 `DependencyService`를 사용하여 런타임에 올바른 구현을 가져올 수 있습니다.

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>`에서 `T` 인터페이스의 올바른 구현을 찾습니다.

### <a name="solution-structure"></a>솔루션 구조

위에서 설명한 코드 변경 내용이 강조 표시된 iOS 및 Android용 [UsingDependencyService 솔루션 샘플](https://developer.xamarin.com/samples/UsingDependencyService/)은 아래와 같습니다.

 [![iOS 및 Android 솔루션](introduction-images/solution-sml.png "DependencyService 솔루션 샘플 구조")](introduction-images/solution.png#lightbox "DependencyService 솔루션 샘플 구조")

> [!NOTE]
> 모든 플랫폼 프로젝트에서 구현을 **제공해야 합니다**. 등록된 인터페이스 구현이 없으면 `DependencyService`에서 런타임에 `Get<T>()` 메서드를 확인할 수 없습니다.

## <a name="related-links"></a>관련 링크

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
