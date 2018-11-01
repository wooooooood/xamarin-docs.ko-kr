---
title: DependencyService 소개
description: 이 문서에서는 네이티브 플랫폼 기능에 액세스 하도록 Xamarin.Forms DependencyService 클래스의 작동 방식을 설명 합니다.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/15/2018
ms.openlocfilehash: 3c8cc31c21f354b60001cefb919b51bf4d42da9f
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675018"
---
# <a name="introduction-to-dependencyservice"></a>DependencyService 소개

## <a name="overview"></a>개요

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 앱을을 공유 코드에서 플랫폼별 기능을 호출할 수 있습니다. 이 기능은 Xamarin.Forms 앱을 네이티브 앱을 수행할 수 있는 모든 작업을 수행할 수 있습니다.

`DependencyService` 서비스 로케이터가입니다. 실제로 인터페이스를 정의 하 고 `DependencyService` 올바른 구현의 다양 한 플랫폼 프로젝트에서 해당 인터페이스를 찾습니다.

> [!NOTE]
> 기본적으로 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 매개 변수가 없는 생성자는 플랫폼 구현만 확인 됩니다. 그러나 종속성 해결 방법은 종속성 주입 컨테이너 또는 팩터리 메서드를 사용 하 여 플랫폼 구현을 확인 하는 Xamarin.Forms에 삽입할 수 있습니다. 이 방법은 매개 변수가 있는 생성자는 플랫폼 구현을 확인에 사용할 수 있습니다. 자세한 내용은 [Xamarin.Forms에서 종속성 확인](~/xamarin-forms/internals/dependency-resolution.md)합니다.

## <a name="how-dependencyservice-works"></a>DependencyService의 작동 원리

Xamarin.Forms 앱을 사용 하 여 네 가지 구성 요소 필요 `DependencyService`:

- **인터페이스** &ndash; 필요한 기능을 공유 코드에서 인터페이스에서 정의 됩니다.
- **플랫폼 당 구현을** &ndash; 클래스 인터페이스를 구현 하는 각 플랫폼 프로젝트에 추가 해야 합니다.
- **등록** &ndash; 구현 하는 각 클래스에 등록 해야 합니다 `DependencyService` 메타 데이터 특성을 통해. 등록 하면 `DependencyService` 를 구현 하는 클래스를 찾고 런타임에 인터페이스를 대신 제공 합니다.
- **DependencyService를 호출** &ndash; 명시적으로 호출 해야 하는 코드를 공유 `DependencyService` 인터페이스의 구현에 대 한 요청입니다.

솔루션에서 각 플랫폼 프로젝트에 대 한 구현을 제공 해야 하는 참고 합니다. 플랫폼 프로젝트를 구현 하지 않고 런타임에 실패 합니다.

응용 프로그램의 구조는 다음 다이어그램에서 설명 됩니다.

![](introduction-images/overview-diagram.png "DependencyService 응용 프로그램 구조")

### <a name="interface"></a>인터페이스

플랫폼별 기능 상호 작용 디자인 인터페이스를 정의 합니다. 구성 요소 또는 NuGet 패키지 공유 구성 요소를 개발 하는 경우 주의 해야 합니다. API 디자인 망칠 수도 패키지 있습니다. 아래 예제에서는 각 플랫폼에 대 한 사용자 지정 구현 되지만 읽을 단어를 지정 하는 유연성을 허용 하는 텍스트 말하기에 대 한 간단한 인터페이스를 지정 합니다.

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>플랫폼 당 구현

적절 한 인터페이스를 설계 되 면 대상으로 하는 각 플랫폼에 대 한 프로젝트에서 해당 인터페이스를 구현 해야 합니다. 예를 들어 다음 클래스가 구현 하는 `ITextToSpeech` iOS에서 인터페이스:

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

각 인터페이스 구현을 사용 하 여 등록 해야 `DependencyService` 메타 데이터 특성을 사용 하 여 합니다. 다음 코드에는 iOS에 대 한 구현을 등록합니다.

```csharp
[assembly: Dependency (typeof (TextToSpeech_iOS))]
namespace UsingDependencyService.iOS
{
  ...
}
```

전체 과정에 플랫폼 특정 구현을 다음과 같습니다.

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

참고: 등록 클래스 수준이 아닌 네임 스페이스 수준에서 수행 되는 합니다.

#### <a name="universal-windows-platform-net-native-compilation"></a>유니버설 Windows 플랫폼.NET 네이티브 컴파일

.NET 네이티브 컴파일 옵션을 사용 하는 UWP 프로젝트를 수행 해야 합니다는 [약간 다른 구성](~/xamarin-forms/platform/windows/installation/index.md#target-invocation-exception) Xamarin.Forms를 초기화할 때. .NET 네이티브 컴파일 종속성 서비스에 대 한 약간 다른 등록을 해야합니다.

에 **App.xaml.cs** 파일을 수동으로 사용 하 여 UWP 프로젝트에 정의 된 각 종속성 서비스를 등록 합니다 `Register<T>` 메서드를 아래와 같이:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

참고: 수동 등록을 사용 하 여 `Register<T>` 릴리스에서만 적용.NET 네이티브 컴파일을 사용 하 여 작성 됩니다. 이 줄을 생략 하면, 디버그 빌드에서 계속 작동 하지만 릴리스 빌드 종속성 서비스를 로드 하지 못합니다.

### <a name="call-to-dependencyservice"></a>DependencyService를 호출 합니다.

사용 하 여 프로젝트에 공통 인터페이스 및 각 플랫폼에 대 한 구현을 사용 하 여 설정 되 면 `DependencyService` 런타임 시 오른쪽 구현을 가져올 수 있습니다.

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` 인터페이스의 올바른 구현을 찾을 수 `T`입니다.

### <a name="solution-structure"></a>솔루션 구조

합니다 [UsingDependencyService 솔루션 샘플](https://developer.xamarin.com/samples/UsingDependencyService/) iOS 및 Android에 대 한 아래 이면 위에서 설명한 코드 변경 내용으로 강조 표시 합니다.

 [![iOS 및 Android 솔루션](introduction-images/solution-sml.png "DependencyService 샘플 솔루션 구조")](introduction-images/solution.png#lightbox "DependencyService 샘플 솔루션 구조")

> [!NOTE]
> 있습니다 **해야** 모든 플랫폼 프로젝트에서 구현을 제공 합니다. 인터페이스 구현이 등록 되 면 해당 `DependencyService` 확인할 수 없습니다는 `Get<T>()` 런타임에 메서드.

## <a name="related-links"></a>관련 링크

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
