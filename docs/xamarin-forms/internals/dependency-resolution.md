---
title: Xamarin.Forms에서 종속성 확인
description: 이 문서에서는 응용 프로그램의 종속성 주입 컨테이너에 생성 및 사용자 지정 렌더러, 효과 및 DependencyService 구현을의 수명을 제어할 수 있도록 Xamarin.Forms에 종속성 확인 메서드를 삽입 하는 방법에 설명 합니다.
ms.prod: xamarin
ms.assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/23/2018
ms.openlocfilehash: 2379c8ddc4bea6dd97bc4febd055dd8dfef39beb
ms.sourcegitcommit: 46bb04016d3c35d91ff434b38474e0cb8197961b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39270490"
---
# <a name="dependency-resolution-in-xamarinforms"></a>Xamarin.Forms에서 종속성 확인

_이 문서에서는 응용 프로그램의 종속성 주입 컨테이너에 생성 및 사용자 지정 렌더러, 효과 및 DependencyService 구현을의 수명을 제어할 수 있도록 Xamarin.Forms에 종속성 확인 메서드를 삽입 하는 방법에 설명 합니다. 코드 예제에서 수행 되는 [종속성 확인](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/) 샘플입니다._

모델-뷰-ViewModel (MVVM) 패턴을 사용 하는 Xamarin.Forms 응용 프로그램의 컨텍스트에서 서비스를 등록 하 고 모델 보기에 주입 하 한 및 보기 모델을 해결 하는 등록에 대 한 종속성 주입 컨테이너를 사용할 수 있습니다. 뷰 모델을 만드는 동안 컨테이너는 필요한 모든 종속성을 삽입 합니다. 해당 종속성을 만들지 않은 경우 컨테이너를 만들고 먼저 종속성을 확인 합니다. 종속성 주입을 모델 보기에 종속성 주입의 예제를 포함 하는 방법에 대 한 자세한 내용은 참조 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)합니다.

만들기에 대 한 제어 플랫폼 프로젝트에서 형식의 수명 Xamarin.Forms를 사용 하 여 일반적으로 수행 되 고는 `Activator.CreateInstance` 사용자 지정 렌더러의 경우 효과의 인스턴스를 만드는 방법 및 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 구현입니다. 안타깝게도이 개발자를 만들고 이러한 형식과에 종속성을 주입할 수 있는 기간을 제어할을 제한 됩니다. 그러나이 동작을 제어 하는 방법을 형식 만들어집니다 – Xamarin.Forms에서 또는 응용 프로그램의 종속성 주입 컨테이너에 의해 Xamarin.Forms에는 종속성 확인 메서드를 삽입 하 여 변경할 수 있습니다.

> [!NOTE]
> Xamarin.Forms에 종속성 확인 메서드를 삽입 하는 요구 사항이 있습니다. Xamarin.Forms를 만들고 종속성 해결 방법을 삽입 되지 않습니다 하는 경우 플랫폼 프로젝트에서 형식의 수명 관리 계속 됩니다.

## <a name="injecting-a-dependency-resolution-method"></a>종속성 확인 메서드를 삽입합니다.

합니다 [ `DependencyResolver` ](xref:Xamarin.Forms.Internals.DependencyResolver) 클래스 중 하나를 사용 하 여 Xamarin.Forms 종속성 확인 메서드를 삽입 하는 기능 제공 합니다 [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) 메서드. 그런 다음 Xamarin.Forms에서 특정 형식의 인스턴스를 필요한 경우, 종속성 확인 메서드가 인스턴스를 제공 하는 데 지정 됩니다. 종속성 확인 메서드를 반환 하는 경우 `null` 요청 된 형식에 대 한 Xamarin.Forms 대체 형식을 만들려고 하를 사용 하 여 자체 인스턴스를 `Activator.CreateInstance` 메서드.

다음 예제에서는 사용 하 여 종속성 확인 메서드를 설정 하는 방법을 보여 줍니다 합니다 [ `ResolveUsing` ](Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) 메서드:

```csharp
using Autofac;
using Xamarin.Forms.Internals;
...

public partial class App : Application
{
    // IContainer and ContainerBuilder are provided by Autofac
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();

    public App()
    {
        ...
        DependencyResolver.ResolveUsing(type => container.IsRegistered(type) ? container.Resolve(type) : null);
        ...
    }
    ...
}
```

이 예제에서는 종속성 해결 방법은 Autofac 종속성 주입 컨테이너를 사용 하 여 컨테이너를 사용 하 여 등록 된 모든 형식을 확인 하는 람다 식으로 설정 됩니다. 그렇지 않으면 `null` 형식을 확인 하려고 하는 Xamarin.Forms 결과 반환 될 됩니다.

> [!NOTE]
> 종속성 주입 컨테이너에서 사용 하는 API 컨테이너와 관련이 있습니다. 이 문서의 코드 예제를 제공 하는 종속성 주입 컨테이너를으로 Autofac을 사용 합니다 `IContainer` 및 `ContainerBuilder` 형식입니다. 다른 종속성 주입 컨테이너 동일 하 게을 사용할 수 있지만 여기에 제시 된 보다 다양 한 Api를 사용 합니다.

응용 프로그램 시작 중 종속성 확인 방법을 설정 하는 요구 사항이 있는지 참고 합니다. 언제 든 지 설정할 수 있습니다. 유일한 제약 조건 Xamarin.Forms 응용 프로그램에서 종속성 주입 컨테이너에 저장 된 형식 사용 하려고 하는 시간을 기준으로 종속성 해결 방법을 파악 하는 것입니다. 따라서 응용 프로그램을 시작 하는 동안 해야 하는 종속성 주입 컨테이너에 서비스가 있으면 종속성 해결 방법을 응용 프로그램의 수명 주기 초기에 설정 해야 합니다. 마찬가지로, 종속성 주입 컨테이너 생성 및 특정의 수명을 관리 하는 경우 [ `Effect` ](xref:Xamarin.Forms.Effect), Xamarin.Forms 뷰 만들기를 시도 하기 전에 종속성 확인 메서드에 대 한 파악 해야 합니다는 `Effect`합니다.

> [!WARNING]
> 등록 하 고 종속성 주입 컨테이너를 사용 하 여 형식을 확인 응용 프로그램에서 각 페이지 탐색에 대 한 종속성을 재구성 되는 경우에 특히 컨테이너의 각 형식 만들기에 대 한 리플렉션 사용 비용은 성능을 있습니다. 여러 경우에 전체 종속성 생성 비용이 크게 증가할 수 있습니다.

## <a name="registering-types"></a>형식 등록

종속성 확인 메서드를 통해 해결할 수 전에 형식은 종속성 주입 컨테이너를 사용 하 여 등록 되어야 합니다. 다음 코드 예제는 샘플 응용 프로그램에서 노출 하는 등록 메서드 표시는 `App` Autofac 컨테이너 클래스:

```csharp
using Autofac;
using Autofac.Core;
...

public partial class App : Application
{
    static IContainer container;
    static readonly ContainerBuilder builder = new ContainerBuilder();
    ...

    public static void RegisterType<T>() where T : class
    {
        builder.RegisterType<T>();
    }

    public static void RegisterType<TInterface, T>() where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>().As<TInterface>();
    }

    public static void RegisterTypeWithParameters<T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where T : class
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        });
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

응용 프로그램에 컨테이너에서 형식을 확인 하기 위해 종속성 해결 방법을 사용 하는 경우 형식 등록 플랫폼 프로젝트에서 일반적으로 수행 됩니다. 이 통해 플랫폼 프로젝트 유형을 사용자 지정 렌더러의 경우 효과 등록 하 고 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 구현.

플랫폼 프로젝트에서 형식 등록을 수행 합니다 `IContainer` 호출 하 여 수행 되는 개체를 빌드해야 합니다 `BuildContainer` 메서드. 이 메서드는 Autofac의 호출 `Build` 메서드를 `ContainerBuilder` 적용 된 등록을 포함 하는 새 종속성 주입 컨테이너를 빌드하는 경우.

이어지는 섹션에는 `Logger` 클래스를 구현 하는 `ILogger` 인터페이스 클래스 생성자에 주입 됩니다. `Logger` 사용 하 여 클래스 구현 간단한 로깅 기능을 `Debug.WriteLine` 메서드를 사용자 지정 렌더러의 경우 효과에 서비스를 삽입할 수 있습니다 하는 방법을 보여 주기 위해 사용 됩니다 및 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 구현 합니다.

### <a name="registering-custom-renderers"></a>사용자 지정 렌더러를 등록합니다.

샘플 응용 프로그램에 XAML 소스는 다음 예와에서 같이 웹 비디오를 재생 하는 페이지에 포함 됩니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

합니다 `VideoPlayer` 보기에서 각 플랫폼에서 구현 되는 `VideoPlayerRenderer` 비디오 재생 기능을 제공 하는 클래스입니다. 이러한 사용자 지정 렌더러 클래스에 대 한 자세한 내용은 참조 하세요. [비디오 플레이어 구현](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)합니다.

IOS 및 유니버설 Windows 플랫폼 (UWP)에 `VideoPlayerRenderer` 클래스에 필요한 다음 생성자는 `ILogger` 인수:

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

세 플랫폼 모두에서 종속성 주입 컨테이너를 사용 하 여 유형을 등록 하 여 수행 됩니다 합니다 `RegisterTypes` 메서드를 사용 하 여 응용 프로그램을 로드 하는 플랫폼 전에 호출 되는 `LoadApplication(new App())` 메서드. 다음 예제는 `RegisterTypes` iOS 플랫폼에서 메서드:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

이 예제에서는 합니다 `Logger` 구체적인 형식이 해당 인터페이스 형식에 대 한 매핑을 통해 등록 된 및 `VideoPlayerRenderer` 유형이 인터페이스 매핑을 하지 않고 직접 등록 합니다. 사용자가 포함 된 페이지를 탐색 하는 경우는 `VideoPlayer` 보기를 해결 하려면 종속성 확인 메서드를 호출할 합니다 `VideoPlayerRenderer` 또한 해결 하 고 삽입 하는 종속성 주입 컨테이너에서 형식은 `Logger` 입력할 `VideoPlayerRenderer` 생성자입니다.

합니다 `VideoPlayerRenderer` Android 플랫폼에는 생성자가 필요 하므로 조금 더 복잡 한는 `Context` 외에 인수는 `ILogger` 인수:

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

다음 예제는 `RegisterTypes` Android 플랫폼에서 메서드:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

이 예제는 `App.RegisterTypeWithParameters` 메서드 레지스터는 `VideoPlayerRenderer` 종속성 주입 컨테이너를 사용 하 여 합니다. 등록 메서드를 사용 하면를 `MainActivity` 인스턴스도 삽입 되는 `Context` 인수를 지정 하 고 있는 `Logger` 형식으로 삽입 되는 `ILogger` 인수.

### <a name="registering-effects"></a>효과 등록합니다.

끌기 효과 추적 하는 터치를 사용 하는 페이지를 포함 하는 샘플 응용 프로그램 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 페이지 인스턴스. 합니다 [ `Effect` ](xref:Xamarin.Forms.Effect) 에 추가 되는 `BoxView` 다음 코드를 사용 하 여:

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect` 클래스는를 [ `RoutingEffect` ](xref:Xamarin.Forms.RoutingEffect) 하 여 각 플랫폼에서 구현 되는 `TouchEffect` 클래스에 `PlatformEffect`합니다. 플랫폼 `TouchEffect` 끌기를 위한 기능을 제공 하는 클래스는 `BoxView` 페이지입니다. 이러한 효과 클래스에 대 한 자세한 내용은 참조 하세요. [효과의 이벤트를 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)합니다.

세 플랫폼 모두에 `TouchEffect` 클래스에 필요한 다음 생성자는 `ILogger` 인수:

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

세 플랫폼 모두에서 종속성 주입 컨테이너를 사용 하 여 유형을 등록 하 여 수행 됩니다 합니다 `RegisterTypes` 메서드를 사용 하 여 응용 프로그램을 로드 하는 플랫폼 전에 호출 되는 `LoadApplication(new App())` 메서드. 다음 예제는 `RegisterTypes` Android 플랫폼에서 메서드:

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

이 예제에서는 합니다 `Logger` 구체적인 형식이 해당 인터페이스 형식에 대 한 매핑을 통해 등록 된 및 `TouchEffect` 유형이 인터페이스 매핑을 하지 않고 직접 등록 합니다. 사용자가 포함 된 페이지를 탐색 하는 경우는 [ `BoxView` ](xref:Xamarin.Forms.BoxView) 된 인스턴스를 `TouchEffect` 에 연결 된 종속성 확인 메서드를 호출할 플랫폼을 해결 하려면 `TouchEffect` 종속성 유형을 또한 해결 하 고 삽입 주입 컨테이너에는 `Logger` 입력할는 `TouchEffect` 생성자입니다.

### <a name="registering-dependencyservice-implementations"></a>DependencyService 구현을 등록합니다.

샘플 응용 프로그램에 사용 하는 페이지 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService) 사진을 장치의 그림 라이브러리에서 선택할 수 있도록 각 플랫폼에서 구현 합니다. `IPhotoPicker` 인터페이스에서 구현 되는 기능을 정의 `DependencyService` 구현 하 고 다음 예제에 표시 됩니다.

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

각 플랫폼 프로젝트에는 `PhotoPicker` 구현 클래스는 `IPhotoPicker` 플랫폼 Api를 사용 하 여 인터페이스입니다. 이러한 종속성 서비스에 대 한 자세한 내용은 참조 하세요. [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)합니다.

세 플랫폼 모두에 `PhotoPicker` 클래스에 필요한 다음 생성자는 `ILogger` 인수:

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

세 플랫폼 모두에서 종속성 주입 컨테이너를 사용 하 여 유형을 등록 하 여 수행 됩니다 합니다 `RegisterTypes` 메서드를 사용 하 여 응용 프로그램을 로드 하는 플랫폼 전에 호출 되는 `LoadApplication(new App())` 메서드. 다음 예제는 `RegisterTypes` UWP 메서드:

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

이 예제는 `Logger` 구체적인 형식이 해당 인터페이스 형식에 대 한 매핑을 통해 등록 된 및 `PhotoPicker` 형식 인터페이스 매핑을 통해도 등록 합니다. 사용자의 사진 선택 페이지를 탐색 하 고 사진을 선택 하기로 합니다 `OnSelectPhotoButtonClicked` 처리기가 실행 됩니다.

```csharp
async void OnSelectPhotoButtonClicked(object sender, EventArgs e)
{
    ...
    var photoPickerService = DependencyService.Resolve<IPhotoPicker>();
    var stream = await photoPickerService.GetImageStreamAsync();
    if (stream != null)
    {
        image.Source = ImageSource.FromStream(() => stream);
    }
    ...
}
```

경우는 [ `DependencyService.Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드가 호출 되 면 해결 하려면 종속성 확인 메서드를 호출할 합니다 `PhotoPicker` 또한 해결 하 고 삽입 하는 종속성 주입 컨테이너에서 형식을 `Logger` 형식 에 `PhotoPicker` 생성자입니다.

> [!NOTE]
> 합니다 [ `Resolve<T>` ](xref:Xamarin.Forms.DependencyService.Resolve*) 메서드를 통해 응용 프로그램의 종속성 주입 컨테이너에서 형식을 확인할 때 사용 해야 합니다 [ `DependencyService` ](xref:Xamarin.Forms.DependencyService)합니다.

## <a name="related-links"></a>관련 링크

- [종속성 확인 (샘플)](https://developer.xamarin.com/samples/xamarin-forms/Advanced/DependencyResolution/)
- [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [비디오 플레이어 구현](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [효과의 이벤트를 호출합니다.](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [사진 그림 라이브러리에서 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
