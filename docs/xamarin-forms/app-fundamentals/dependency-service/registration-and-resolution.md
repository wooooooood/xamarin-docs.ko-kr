---
title: Xamarin.Forms DependencyService 등록 및 확인
description: 이 문서에서는 Xamarin.Forms DependencyService 클래스를 사용하여 네이티브 플랫폼 기능을 호출하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 5d019604-4f6f-4932-9b26-1fce3b4d88f8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
ms.openlocfilehash: 6e666c16c9b1afc3478f524cae2f84d6704319c2
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199227"
---
# <a name="xamarinforms-dependencyservice-registration-and-resolution"></a>Xamarin.Forms DependencyService 등록 및 확인

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)

Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService)를 사용하여 네이티브 플랫폼 기능을 호출할 때 플랫폼 구현을 호출하려면 `DependencyService`에 등록한 후 공유 코드에서 확인해야 합니다.

## <a name="register-platform-implementations"></a>플랫폼 구현 등록

[`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록해야 Xamarin.Forms가 런타임에 해당 구현을 찾을 수 있습니다.

[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute) 또는 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 사용하여 등록을 수행할 수 있습니다.

> [!IMPORTANT]
> .NET 네이티브 컴파일을 사용하는 UWP 프로젝트의 릴리스 빌드는 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 사용하여 플랫폼 구현을 등록해야 합니다.

### <a name="registration-by-attribute"></a>특성으로 등록

[`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록할 수 있습니다. 지정된 형식이 인터페이스의 구체적인 구현을 제공하는 것을 나타내는 특성입니다.

다음 예제에서는 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 `IDeviceOrientationService` 인터페이스의 iOS 구현을 등록하는 방법을 보여 줍니다.

```csharp
using Xamarin.Forms;

[assembly: Dependency(typeof(DeviceOrientationService))]
namespace DependencyServiceDemos.iOS
{
    public class DeviceOrientationService : IDeviceOrientationService
    {
        public DeviceOrientation GetOrientation()
        {
            ...
        }
    }
}
```

이 예제에서 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 `DeviceOrientationService`를 등록합니다. 결과적으로 구체적인 형식이 구현하는 인터페이스에 해당 형식이 등록됩니다.

마찬가지로 다른 플랫폼의 `IDeviceOrientationService` 인터페이스 구현은 [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용하여 등록해야 합니다.

> [!NOTE]
> [`DependencyAttribute`](xref:Xamarin.Forms.DependencyAttribute)를 사용한 등록은 네임스페이스 수준에서 수행됩니다.

### <a name="registration-by-method"></a>메서드로 등록

[`DependencyService.Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 사용하여 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록할 수 있습니다.

다음 예제에서는 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 사용하여 `IDeviceOrientationService` 인터페이스의 iOS 구현을 등록하는 방법을 보여 줍니다.

```csharp
[Register("AppDelegate")]
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());
        DependencyService.Register<IDeviceOrientationService, DeviceOrientationService>();
        return base.FinishedLaunching(app, options);
    }
}
```

이 예제에서 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드는 구체적인 형식 `DeviceOrientationService`를 `IDeviceOrientationService` 인터페이스에 등록합니다. 또는 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드의 오버로드를 사용하여 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 플랫폼 구현을 등록할 수 있습니다.

```csharp
DependencyService.Register<DeviceOrientationService>();
```

이 예제에서 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드는 [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 `DeviceOrientationService`를 등록합니다. 결과적으로 구체적인 형식이 구현하는 인터페이스에 해당 형식이 등록됩니다.

마찬가지로 다른 플랫폼의 `IDeviceOrientationService` 인터페이스 구현은 [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 사용하여 등록해야 합니다.

> [!IMPORTANT]
> [`Register`](xref:Xamarin.Forms.DependencyService.Register*) 메서드를 통한 등록을 플랫폼 프로젝트에서 수행한 후 플랫폼 구현이 제공하는 기능을 공유 코드에서 호출해야 합니다.

## <a name="resolve-the-platform-implementations"></a>플랫폼 구현 확인

플랫폼 구현은 확인한 후 호출해야 합니다. 이 작업은 일반적으로 공유 코드에서 [`DependencyService.Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드를 사용하여 수행합니다. 그러나 [`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드를 사용하여 수행할 수도 있습니다.

기본적으로 [`DependencyService`](xref:Xamarin.Forms.DependencyService)는 매개 변수가 없는 생성자가 있는 플랫폼 구현만 확인합니다. 그러나 종속성 확인 메서드는 종속성 주입 컨테이너 또는 팩터리 메서드를 사용하여 플랫폼 구현을 확인하는 Xamarin.Forms에 주입할 수 있습니다. 이 방법은 매개 변수가 있는 생성자가 있는 가진 플랫폼 구현을 확인하는 데 사용할 수 있습니다. 자세한 내용은 [Xamarin.Forms에서 종속성 확인](~/xamarin-forms/internals/dependency-resolution.md)을 참조하세요.

> [!IMPORTANT]
> [`DependencyService`](xref:Xamarin.Forms.DependencyService)에 등록되지 않은 플랫폼 구현을 호출하면 `NullReferenceException`이 throw됩니다.

### <a name="resolve-using-the-getlttgt-method"></a>Get&lt;T&gt; 메서드를 사용하여 확인

[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드는 런타임에 인터페이스 `T`의 플랫폼 구현을 검색하고 이 구현의 인스턴스를 싱글톤으로 만듭니다. 이 인스턴스는 애플리케이션의 수명 동안 유지되며 동일한 플랫폼 구현을 확인하는 후속 호출은 동일한 인스턴스를 검색합니다.

다음 코드는 [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드를 호출하여 `IDeviceOrientationService` 인터페이스를 확인한 후 해당 `GetOrientation` 메서드를 호출하는 예제를 보여 줍니다.

```csharp
IDeviceOrientationService service = DependencyService.Get<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

또는 이 코드를 다음 한 줄로 요약할 수 있습니다.

```csharp
DeviceOrientation orientation = DependencyService.Get<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드는 기본적으로 인터페이스 `T`의 플랫폼 구현 인스턴스를 싱글톤으로 만듭니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [확인된 개체의 수명 관리](#manage-the-lifetime-of-resolved-objects)를 참조하세요.

### <a name="resolve-using-the-resolvelttgt-method"></a>Resolve&lt;T&gt; 메서드를 사용하여 확인

[`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드는 [`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver) 클래스를 통해 Xamarin.Forms에 삽입된 종속성 확인 메서드를 사용하여 런타임에 인터페이스 `T`의 플랫폼 구현을 검색합니다. 종속성 확인 메서드가 Xamarin.Forms에 삽입되지 않은 경우 `Resolve<T>` 메서드는 플랫폼 구현을 검색하기 위해 [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드 호출로 대체됩니다. 종속성 확인 메서드를 Xamarin.Forms에 삽입하는 방법에 대한 자세한 내용은 [Xamarin.Forms의 종속성 확인](~/xamarin-forms/internals/dependency-resolution.md)을 참조하세요.

다음 코드는 [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드를 호출하여 `IDeviceOrientationService` 인터페이스를 확인한 후 해당 `GetOrientation` 메서드를 호출하는 예제를 보여 줍니다.

```csharp
IDeviceOrientationService service = DependencyService.Resolve<IDeviceOrientationService>();
DeviceOrientation orientation = service.GetOrientation();
```

또는 이 코드를 다음 한 줄로 요약할 수 있습니다.

```csharp
DeviceOrientation orientation = DependencyService.Resolve<IDeviceOrientationService>().GetOrientation();
```

> [!NOTE]
> [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드가 [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 메서드 호출로 대체되면 이 메서드는 기본적으로 인터페이스 `T`의 플랫폼 구현 인스턴스를 싱글톤으로 만듭니다. 그러나 이 동작을 변경할 수 있습니다. 자세한 내용은 [확인된 개체의 수명 관리](#manage-the-lifetime-of-resolved-objects)를 참조하세요.

## <a name="manage-the-lifetime-of-resolved-objects"></a>확인된 개체의 수명 관리

[`DependencyService`](xref:Xamarin.Forms.DependencyService) 클래스의 기본 동작은 플랫폼 구현을 싱글톤으로 확인하는 것입니다. 따라서 플랫폼 구현은 애플리케이션의 수명 동안 유지됩니다.

이 동작은 [`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 및 [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드에서 [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) 선택적 인수를 사용하여 지정합니다. [`DependencyFetchTarget`](xref:Xamarin.Forms.DependencyFetchTarget) 열거형은 다음 두 개의 멤버를 정의합니다.

- `GlobalInstance` - 플랫폼 구현을 싱글톤으로 반환합니다.
- `NewInstance` - 플랫폼 구현의 새 인스턴스를 반환합니다. 그런 다음, 애플리케이션이 플랫폼 구현 인스턴스의 수명을 관리해야 합니다.

[`Get<T>`](xref:Xamarin.Forms.DependencyService.Get*) 및 [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드는 둘 다 선택적 인수를 [`DependencyFetchTarget.GlobalInstance`](xref:Xamarin.Forms.DependencyFetchTarget)로 설정하므로 플랫폼 구현은 항상 싱글톤으로 확인됩니다. 이 동작은 변경될 수 있으므로 [`DependencyFetchTarget.NewInstance`](xref:Xamarin.Forms.DependencyFetchTarget)를 `Get<T>` 및 `Resolve<T>` 메서드에 대한 인수로 지정하면 플랫폼 구현의 새 인스턴스가 생성됩니다.

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
```

이 예제에서 [`DependencyService`](xref:Xamarin.Forms.DependencyService)는 `ITextToSpeechService` 인터페이스용 플랫폼 구현의 새 인스턴스를 만듭니다. `ITextToSpeechService`를 확인하는 모든 후속 호출도 새 인스턴스를 만듭니다.

항상 플랫폼 구현의 새 인스턴스를 만들면 결과적으로 애플리케이션이 인스턴스의 수명을 관리합니다. 즉, 플랫폼 구현에 정의된 이벤트를 구독하는 경우 플랫폼 구현이 더 이상 필요하지 않을 때 이벤트에서 구독을 취소해야 합니다. 또한 `IDisposable`을 구현하고 `Dispose` 메서드에서 해당 리소스를 정리해야 할 수 있습니다. 애플리케이션 예제는 `TextToSpeechService` 플랫폼 구현에서 이 시나리오를 보여 줍니다.

애플리케이션은 `IDisposable`을 구현하는 플랫폼 구현의 사용을 완료할 때 개체의 `Dispose` 구현을 호출해야 합니다. 이 작업을 수행하는 한 가지 방법은 `using` 문을 사용하는 것입니다.

```csharp
ITextToSpeechService service = DependencyService.Get<ITextToSpeechService>(DependencyFetchTarget.NewInstance);
using (service as IDisposable)
{
    await service.SpeakAsync("Hello world");
}
```

이 예제에서 `SpeakAsync` 메서드가 호출된 후 `using` 문은 플랫폼 구현 개체를 자동으로 삭제합니다. 이렇게 하면 필요한 정리를 수행하는 개체의 `Dispose` 메서드가 호출됩니다.

개체의 `Dispose` 메서드를 호출하는 방법에 대한 자세한 내용은 [IDisposable을 구현하는 개체 사용](/dotnet/standard/garbage-collection/using-objects)을 참조하세요.

## <a name="related-links"></a>관련 링크

- [DependencyService 데모(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/dependencyservice/)
- [Xamarin.Forms의 종속성 확인](~/xamarin-forms/internals/dependency-resolution.md)
