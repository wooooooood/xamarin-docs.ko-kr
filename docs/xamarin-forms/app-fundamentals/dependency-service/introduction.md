---
title: DependencyService 소개
description: DependencyService 네이티브 플랫폼 기능에 액세스 하려면 작동 방식을 이해합니다
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/06/2017
ms.openlocfilehash: 01953d55a104a70b0451c9b796c732254afb081e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-dependencyservice"></a>DependencyService 소개

## <a name="overview"></a>개요

[`DependencyService`](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) 앱을 공유 코드에서 플랫폼별 기능을 호출할 수 있습니다. 이 기능을 통해 Xamarin.Forms 앱이 네이티브 응용 프로그램 작업을 수행할 수 있는 모든 항목을 할 수 있습니다.

`DependencyService` 종속성 확인자가입니다. 실제로 인터페이스를 정의 하 고 `DependencyService` 다양 한 플랫폼 프로젝트에서 해당 인터페이스의 올바른 구현을 찾습니다.

## <a name="how-dependencyservice-works"></a>DependencyService의 작동 원리

Xamarin.Forms 앱 사용 하는 세 가지 구성 요소 필요 `DependencyService`:

- **인터페이스** &ndash; 필요한 기능 공유 코드에서 인터페이스에 의해 정의 됩니다.
- **구현 당 플랫폼** &ndash; 인터페이스를 구현 하는 클래스는 각 플랫폼 프로젝트에 추가 해야 합니다.
- **등록** &ndash; 구현 하는 각 클래스에 등록 해야 `DependencyService` 메타 데이터 특성을 통해. 등록 하면 `DependencyService` 를 구현 하는 클래스를 찾아 런타임에 인터페이스 대신 제공 합니다.
- **DependencyService에 대 한 호출** &ndash; 명시적으로 호출 해야 하는 코드를 공유 `DependencyService` 인터페이스의 구현에 게 요청 합니다.

솔루션의 각 플랫폼 프로젝트에 대 한 구현을 제공 해야 하는 참고 합니다. 구현 없이 플랫폼 프로젝트에서 런타임에 실패 합니다.

응용 프로그램의 구조는 다음 그림에 설명 되어 있습니다.

![](introduction-images/overview-diagram.png "DependencyService 응용 프로그램 구조")

### <a name="interface"></a>인터페이스

디자인 인터페이스 플랫폼별 기능 상호 작용 하는 방법을 정의 합니다. 구성 요소 또는 Nuget 패키지를 공유 하도록 구성 요소를 개발 하는 경우 주의 해야 합니다. API 디자인 확인 하거나 패키지를 중단할 수 있습니다. 다음 예제에서는 말할 텍스트를 단어를 지정 하는 유연성에 대 한 읽을 수 있지만 각 플랫폼에 대해 사용자 지정할 수 있도록 구현에 대 한 간단한 인터페이스를 지정 합니다.

```csharp
public interface ITextToSpeech {
    void Speak ( string text ); //note that interface members are public by default
}
```

### <a name="implementation-per-platform"></a>플랫폼 마다 구현

적절 한 인터페이스를 디자인 한 후 대상으로 하는 각 플랫폼에 대 한 프로젝트에서 해당 인터페이스를 구현 합니다. 예를 들어 다음 클래스에서는 구현 된 `ITextToSpeech` Windows Phone 인터페이스:

```csharp
namespace TextToSpeech.WinPhone
{
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

모든 구현에 대 한 순서 대로 기본 (매개 변수가 없는) 생성자가 있어야 `DependencyService` , 인스턴스화할 수 있습니다. 인터페이스에 의해 매개 변수가 없는 생성자를 정의할 수 없습니다.

### <a name="registration"></a>등록

에 등록 해야 하는 인터페이스의 각 구현은 `DependencyService` 메타 데이터 특성을 가진 합니다. 다음 코드는 Windows Phone 대 한 구현을 등록합니다.

```csharp
using TextToSpeech.WinPhone;

[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  ...
```

플랫폼별 구현을 모두 조합, 다음과 같이 보입니다.

```csharp
[assembly: Xamarin.Forms.Dependency (typeof (TextToSpeechImplementation))]
namespace TextToSpeech.WinPhone {
  public class TextToSpeechImplementation : ITextToSpeech
  {
      public TextToSpeechImplementation() {}

      public async void Speak(string text)
      {
          SpeechSynthesizer synth = new SpeechSynthesizer();
          await synth.SpeakTextAsync(text);
      }
  }
}
```

참고: 등록 클래스 수준이 아닌 네임 스페이스 수준에서 수행 합니다.

#### <a name="universal-windows-platform-net-native-compilation"></a>유니버설 Windows 플랫폼.NET 네이티브 컴파일

.NET 네이티브 컴파일 옵션을 사용 하는 UWP 프로젝트 따라야는 [약간 다른 구성](~/xamarin-forms/platform/windows/installation/universal.md#target-invocation-exception) Xamarin.Forms를 초기화할 때. .NET 네이티브 컴파일 종속성 서비스에 대 한 약간 다른 등록을 해야합니다.

에 **App.xaml.cs** 파일을 사용 하 여 UWP 프로젝트에 정의 된 각 종속성 서비스를 수동으로 등록는 `Register<T>` 메서드를 다음과 같이 합니다.

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// register the dependencies in the same
Xamarin.Forms.DependencyService.Register<TextToSpeechImplementation>();
```

참고: 수동 등록을 사용 하 여 `Register<T>` 릴리스에서 유효.NET 네이티브 컴파일을 사용 하 여 작성 됩니다. 이 줄을 생략 하면, 디버그 빌드는 계속 작동 하지만 릴리스 빌드 종속성 서비스를 로드 되지 것입니다.

### <a name="call-to-dependencyservice"></a>DependencyService에 대 한 호출

사용 하 여 프로젝트에는 공용 인터페이스와 각 플랫폼에 대 한 구현을 설정 되 고 나면 `DependencyService` 런타임에 오른쪽 구현을 가져올 수 있습니다.

```csharp
DependencyService.Get<ITextToSpeech>().Speak("Hello from Xamarin Forms");
```

`DependencyService.Get<T>` 인터페이스의 올바른 구현을 찾을 수 `T`합니다.

### <a name="solution-structure"></a>솔루션 구조

[샘플 UsingDependencyService 솔루션](https://developer.xamarin.com/samples/UsingDependencyService/) iOS 및 Android에 대 한 아래 표시 된, 위에 나와 있는 코드 변경 내용으로 강조 표시 됩니다.

 [![iOS 및 Android 솔루션](introduction-images/solution-sml.png "DependencyService 샘플 솔루션 구조")](introduction-images/solution.png#lightbox "DependencyService 샘플 솔루션 구조")

> [!NOTE]
> 하면 **해야** 모든 플랫폼 프로젝트에 대 한 구현을 제공 합니다. 인터페이스 구현이 등록 되어 있으면 하면 `DependencyService` 확인할 수 없습니다는 `Get<T>()` 런타임에 메서드.


## <a name="related-links"></a>관련 링크

- [DependencyServiceSample](https://developer.xamarin.com/samples/xamarin-forms/UsingDependencyService/)
- [Xamarin.Forms 샘플](https://developer.xamarin.com/samples/xamarin-forms/all/)
