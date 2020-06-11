---
제목: "설명:"에서 종속성 확인 " Xamarin.Forms 이 문서는 Xamarin.Forms 응용 프로그램의 종속성 주입 컨테이너가 사용자 지정 렌더러, 효과 및 DependencyService 구현의 생성 및 수명을 제어 하도록 종속성 확인 메서드를에 삽입 하는 방법을 설명 합니다.
assetid: 491B87DC-14CB-4ADC-AC6C-40A7627B2524: xamarin-forms author: davidbritch: dabritch:: 07/27/2018-loc: [ Xamarin.Forms ,]입니다. Xamarin.Essentials
---

# <a name="dependency-resolution-in-xamarinforms"></a>Xamarin.Forms의 종속성 확인

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)

_이 문서에서는 Xamarin.Forms 응용 프로그램의 종속성 주입 컨테이너가 사용자 지정 렌더러, 효과 및 DependencyService 구현의 생성 및 수명을 제어 하도록 종속성 확인 방법을에 삽입 하는 방법을 설명 합니다. 이 문서의 코드 예제는 컨테이너 샘플을 [사용 하 여 종속성 확인](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo) 에서 가져옵니다._

Xamarin.FormsMVVM (모델-뷰-ViewModel) 패턴을 사용 하는 응용 프로그램의 컨텍스트에서 종속성 주입 컨테이너를 사용 하 여 뷰 모델을 등록 하 고 해결 하며, 서비스를 등록 하 고이를 뷰 모델에 삽입할 수 있습니다. 뷰 모델을 만드는 동안 컨테이너는 필요한 모든 종속성을 삽입 합니다. 이러한 종속성을 만들지 않은 경우 컨테이너는 종속성을 먼저 만들고 확인 합니다. 뷰 모델에 종속성을 삽입 하는 예제를 포함 하 여 종속성 주입에 대 한 자세한 내용은 [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)을 참조 하세요.

플랫폼 프로젝트에서 형식의 생성 및 수명에 대 한 제어는 일반적으로 Xamarin.Forms 메서드를 사용 하 여 `Activator.CreateInstance` 사용자 지정 렌더러, 효과 및 구현의 인스턴스를 만드는에 의해 수행 됩니다 [`DependencyService`](xref:Xamarin.Forms.DependencyService) . 아쉽게도 이러한 형식의 생성 및 수명 및 종속성을 삽입 하는 기능에 대 한 개발자 제어를 제한 합니다. Xamarin.Forms응용 프로그램의 종속성 주입 컨테이너 또는에서 형식을 만드는 방법을 제어 하는 종속성 확인 메서드를에 삽입 하 여이 동작을 변경할 수 있습니다 Xamarin.Forms . 그러나 종속성 확인 메서드를에 삽입할 필요는 없습니다 Xamarin.Forms . Xamarin.Forms종속성 확인 방법이 삽입 되지 않은 경우는 플랫폼 프로젝트에서 형식의 수명을 계속 만들고 관리 합니다.

> [!NOTE]
> 이 문서에서는 종속성 확인 메서드를에 삽입 하 Xamarin.Forms 여 종속성 주입 컨테이너를 사용 하는 등록 된 형식을 확인 하는 데 중점을 둔 반면, 팩터리 메서드를 사용 하 여 등록 된 형식을 확인 하는 종속성 확인 메서드를 삽입할 수 있습니다. 자세한 내용은 [팩터리 메서드를 사용 하 여 종속성 확인](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-factoriesdemo) 샘플을 참조 하세요.

## <a name="injecting-a-dependency-resolution-method"></a>종속성 확인 방법 삽입

[`DependencyResolver`](xref:Xamarin.Forms.Internals.DependencyResolver)클래스는 Xamarin.Forms 메서드를 사용 하 여 종속성 확인 메서드를에 삽입 하는 기능을 제공 합니다 [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) . 그런 다음 Xamarin.Forms 특정 형식의 인스턴스가 필요할 때 종속성 확인 방법에 인스턴스를 제공할 수 있는 기회가 제공 됩니다. 종속성 확인 메서드가 요청 된 형식에 대해를 반환 하는 경우는 `null` Xamarin.Forms 메서드를 사용 하 여 형식 인스턴스를 만들려고 시도 하는 것으로 대체 합니다 `Activator.CreateInstance` .

다음 예제에서는 메서드를 사용 하 여 종속성 확인 메서드를 설정 하는 방법을 보여 줍니다 [`ResolveUsing`](xref:Xamarin.Forms.Internals.DependencyResolver.ResolveUsing*) .

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

이 예제에서 종속성 확인 메서드는 Autofac 종속성 주입 컨테이너를 사용 하 여 컨테이너에 등록 된 형식을 확인 하는 람다 식으로 설정 됩니다. 그렇지 않으면가 반환 되 고,이로 `null` 인해 Xamarin.Forms 형식을 확인 하려고 시도 합니다.

> [!NOTE]
> 종속성 주입 컨테이너에서 사용 하는 API는 컨테이너에만 적용 됩니다. 이 문서의 코드 예제에서는 및 형식을 제공 하는 종속성 주입 컨테이너로 Autofac를 사용 합니다 `IContainer` `ContainerBuilder` . 대체 종속성 주입 컨테이너는 동일 하 게 사용할 수 있지만 여기에 표시 된 것과 다른 Api를 사용 합니다.

응용 프로그램 시작 중에 종속성 확인 방법을 설정 하기 위한 요구 사항은 없습니다. 언제 든 지 설정할 수 있습니다. 유일한 제약 조건은 Xamarin.Forms 응용 프로그램이 종속성 주입 컨테이너에 저장 된 형식을 사용 하려고 시도 하는 시간을 기준으로 종속성 확인 방법에 대해 알아야 하는 것입니다. 따라서 응용 프로그램을 시작 하는 동안 응용 프로그램에서 필요로 하는 종속성 주입 컨테이너에 서비스가 있으면 응용 프로그램의 수명 주기 초기에 종속성 확인 방법을 설정 해야 합니다. 마찬가지로 종속성 주입 컨테이너가 특정의 생성 및 수명을 관리 하는 경우이를 [`Effect`](xref:Xamarin.Forms.Effect) Xamarin.Forms 사용 하는 뷰를 만들기 전에 종속성 확인 방법에 대해 알고 있어야 `Effect` 합니다.

> [!WARNING]
> 종속성 주입 컨테이너를 사용 하 여 형식을 등록 하 고 확인 하는 경우, 특히 응용 프로그램의 각 페이지 탐색에 대해 종속성을 다시 생성 하는 경우 컨테이너의 리플렉션 사용으로 인해 성능 비용이 발생 합니다. 종속성이 많거나 깊은 경우에는 생성 비용이 많이 증가할 수 있습니다.

## <a name="registering-types"></a>형식 등록

종속성 확인 메서드를 통해 형식을 확인 하려면 종속성 주입 컨테이너에 형식을 등록 해야 합니다. 다음 코드 예제에서는 `App` Autofac 컨테이너에 대해 샘플 응용 프로그램이 클래스에서 노출 하는 등록 메서드를 보여 줍니다.

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

    public static void RegisterTypeWithParameters<TInterface, T>(Type param1Type, object param1Value, Type param2Type, string param2Name) where TInterface : class where T : class, TInterface
    {
        builder.RegisterType<T>()
               .WithParameters(new List<Parameter>()
        {
            new TypedParameter(param1Type, param1Value),
            new ResolvedParameter(
                (pi, ctx) => pi.ParameterType == param2Type && pi.Name == param2Name,
                (pi, ctx) => ctx.Resolve(param2Type))
        }).As<TInterface>();
    }

    public static void BuildContainer()
    {
        container = builder.Build();
    }
    ...
}
```

응용 프로그램에서 종속성 확인 메서드를 사용 하 여 컨테이너에서 형식을 확인 하는 경우 일반적으로 플랫폼 프로젝트에서 형식 등록이 수행 됩니다. 이렇게 하면 플랫폼 프로젝트가 사용자 지정 렌더러, 효과 및 구현에 대 한 형식을 등록할 수 있습니다 [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

플랫폼 프로젝트에서 형식 등록 후에는 `IContainer` 메서드를 호출 하 여 수행 되는 개체를 빌드해야 합니다 `BuildContainer` . 이 메서드는 인스턴스에 대해 Autofac의 `Build` 메서드 `ContainerBuilder` 를 호출 합니다 .이 메서드는 생성 된 등록을 포함 하는 새 종속성 주입 컨테이너를 작성 합니다.

다음 섹션에서는 `Logger` 인터페이스를 구현 하는 클래스가 `ILogger` 클래스 생성자에 삽입 됩니다. `Logger`클래스는 메서드를 사용 하 여 간단한 로깅 기능 `Debug.WriteLine` 을 구현 하며, 사용자 지정 렌더러, 효과 및 구현에 서비스를 삽입 하는 방법을 보여 주는 데 사용 됩니다 [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

### <a name="registering-custom-renderers"></a>사용자 지정 렌더러 등록

샘플 응용 프로그램에는 다음 예제에 XAML 소스가 표시 된 웹 비디오를 재생 하는 페이지가 포함 되어 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             ...>
    <video:VideoPlayer Source="https://archive.org/download/BigBuckBunny_328/BigBuckBunny_512kb.mp4" />
</ContentPage>
```

`VideoPlayer`뷰는 `VideoPlayerRenderer` 비디오 재생 기능을 제공 하는 클래스에 의해 각 플랫폼에서 구현 됩니다. 이러한 사용자 지정 렌더러 클래스에 대 한 자세한 내용은 [비디오 플레이어 구현](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)을 참조 하세요.

IOS 및 유니버설 Windows 플랫폼 (UWP)에서 클래스에는 `VideoPlayerRenderer` 인수가 필요한 다음과 같은 생성자가 있습니다 `ILogger` .

```csharp
public VideoPlayerRenderer(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

모든 플랫폼에서 종속성 주입 컨테이너를 사용 하 여 형식 등록은 메서드를 통해 수행 됩니다 `RegisterTypes` .이 메서드는 플랫폼을 사용 하 여 응용 프로그램을 로드 하기 전에 호출 됩니다 `LoadApplication(new App())` . 다음 예제에서는 `RegisterTypes` iOS 플랫폼의 메서드를 보여 줍니다.

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<FormsVideoLibrary.iOS.VideoPlayerRenderer>();
    App.BuildContainer();
}
```

이 예제에서는 `Logger` 구체적 형식이 인터페이스 형식에 대 한 매핑을 통해 등록 되 고 `VideoPlayerRenderer` 형식이 인터페이스 매핑을 사용 하지 않고 직접 등록 됩니다. 사용자가 뷰를 포함 하는 페이지로 이동 하면 종속성 `VideoPlayer` 확인 메서드가 호출 되어 해당 형식을 확인 하 `VideoPlayerRenderer` 고 생성자에 삽입 하는 종속성 주입 컨테이너에서 형식을 확인 합니다 `Logger` `VideoPlayerRenderer` .

`VideoPlayerRenderer`Android 플랫폼의 생성자는 `Context` 인수 외에도 인수가 필요 하므로 약간 더 복잡 합니다 `ILogger` .

```csharp
public VideoPlayerRenderer(Context context, ILogger logger) : base(context)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

다음 예제에서는 `RegisterTypes` Android 플랫폼의 메서드를 보여 줍니다.

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<FormsVideoLibrary.Droid.VideoPlayerRenderer>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

이 예제에서 메서드는 `App.RegisterTypeWithParameters` `VideoPlayerRenderer` 종속성 주입 컨테이너를 사용 하 여를 등록 합니다. 등록 메서드를 사용 하면 `MainActivity` 인스턴스가 인수로 삽입 되 `Context` 고 `Logger` 형식이 인수로 삽입 됩니다 `ILogger` .

### <a name="registering-effects"></a>효과 등록

샘플 응용 프로그램에는 페이지 주위에서 인스턴스를 끌기 위한 터치 추적 효과를 사용 하는 페이지가 포함 되어 있습니다 [`BoxView`](xref:Xamarin.Forms.BoxView) . 는 [`Effect`](xref:Xamarin.Forms.Effect) `BoxView` 다음 코드를 사용 하 여에 추가 됩니다.

```csharp
var boxView = new BoxView { ... };
var touchEffect = new TouchEffect();
boxView.Effects.Add(touchEffect);
```

`TouchEffect`클래스는 클래스에 [`RoutingEffect`](xref:Xamarin.Forms.RoutingEffect) 의해 각 플랫폼에 구현 되는입니다 `TouchEffect` `PlatformEffect` . Platform `TouchEffect` 클래스는 페이지 주위를 끌기 위한 기능을 제공 합니다 `BoxView` . 이러한 효과 클래스에 대 한 자세한 내용은 [효과에서 이벤트 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)을 참조 하세요.

모든 플랫폼에서 클래스에는 `TouchEffect` 인수가 필요한 다음과 같은 생성자가 있습니다 `ILogger` .

```csharp
public TouchEffect(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

모든 플랫폼에서 종속성 주입 컨테이너를 사용 하 여 형식 등록은 메서드를 통해 수행 됩니다 `RegisterTypes` .이 메서드는 플랫폼을 사용 하 여 응용 프로그램을 로드 하기 전에 호출 됩니다 `LoadApplication(new App())` . 다음 예제에서는 `RegisterTypes` Android 플랫폼의 메서드를 보여 줍니다.

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterType<TouchTracking.Droid.TouchEffect>();
    App.BuildContainer();
}
```

이 예제에서는 `Logger` 구체적 형식이 인터페이스 형식에 대 한 매핑을 통해 등록 되 고 `TouchEffect` 형식이 인터페이스 매핑을 사용 하지 않고 직접 등록 됩니다. 사용자가가 연결 된 인스턴스를 포함 하는 페이지로 이동 하면 종속성 [`BoxView`](xref:Xamarin.Forms.BoxView) `TouchEffect` 확인 메서드가 호출 되어 해당 형식을 `TouchEffect` 확인 하 고 생성자에 삽입 하는 종속성 주입 컨테이너에서 플랫폼 형식을 확인 합니다 `Logger` `TouchEffect` .

### <a name="registering-dependencyservice-implementations"></a>DependencyService 구현 등록

샘플 응용 프로그램에는 [`DependencyService`](xref:Xamarin.Forms.DependencyService) 각 플랫폼에서 구현을 사용 하 여 사용자가 장치의 사진 라이브러리에서 사진을 선택할 수 있도록 하는 페이지가 포함 되어 있습니다. `IPhotoPicker`인터페이스는 구현에 의해 구현 되는 기능을 정의 하 `DependencyService` 고 다음 예제에 나와 있습니다.

```csharp
public interface IPhotoPicker
{
    Task<Stream> GetImageStreamAsync();
}
```

각 플랫폼 프로젝트에서 클래스는 `PhotoPicker` `IPhotoPicker` 플랫폼 api를 사용 하 여 인터페이스를 구현 합니다. 이러한 종속성 서비스에 대 한 자세한 내용은 [그림 라이브러리에서 사진 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)을 참조 하세요.

IOS 및 UWP의 클래스에는 `PhotoPicker` 인수가 필요한 다음과 같은 생성자가 있습니다 `ILogger` .

```csharp
public PhotoPicker(ILogger logger)
{
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

모든 플랫폼에서 종속성 주입 컨테이너를 사용 하 여 형식 등록은 메서드를 통해 수행 됩니다 `RegisterTypes` .이 메서드는 플랫폼을 사용 하 여 응용 프로그램을 로드 하기 전에 호출 됩니다 `LoadApplication(new App())` . 다음 예제에서는 `RegisterTypes` UWP의 메서드를 보여 줍니다.

```csharp
void RegisterTypes()
{
    DIContainerDemo.App.RegisterType<ILogger, Logger>();
    DIContainerDemo.App.RegisterType<IPhotoPicker, Services.UWP.PhotoPicker>();
    DIContainerDemo.App.BuildContainer();
}
```

이 예제에서는 `Logger` 구체적 형식이 인터페이스 형식에 대 한 매핑을 통해 등록 되며, `PhotoPicker` 인터페이스 매핑을 통해 형식이 등록 됩니다.

`PhotoPicker`Android 플랫폼의 생성자는 `Context` 인수 외에도 인수가 필요 하므로 약간 더 복잡 합니다 `ILogger` .

```csharp
public PhotoPicker(Context context, ILogger logger)
{
    _context = context ?? throw new ArgumentNullException(nameof(context));
    _logger = logger ?? throw new ArgumentNullException(nameof(logger));
}
```

다음 예제에서는 `RegisterTypes` Android 플랫폼의 메서드를 보여 줍니다.

```csharp
void RegisterTypes()
{
    App.RegisterType<ILogger, Logger>();
    App.RegisterTypeWithParameters<IPhotoPicker, Services.Droid.PhotoPicker>(typeof(Android.Content.Context), this, typeof(ILogger), "logger");
    App.BuildContainer();
}
```

이 예제에서 메서드는 `App.RegisterTypeWithParameters` `PhotoPicker` 종속성 주입 컨테이너를 사용 하 여를 등록 합니다. 등록 메서드를 사용 하면 `MainActivity` 인스턴스가 인수로 삽입 되 `Context` 고 `Logger` 형식이 인수로 삽입 됩니다 `ILogger` .

사용자가 사진 선택 페이지로 이동 하 고 사진을 선택 하도록 선택 하면 `OnSelectPhotoButtonClicked` 처리기가 실행 됩니다.

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

[`DependencyService.Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)메서드가 호출 되 면 종속성 확인 메서드가 호출 되어 해당 `PhotoPicker` 형식을 확인 하 고 생성자에 삽입 하는 종속성 주입 컨테이너에서 형식을 확인 합니다 `Logger` `PhotoPicker` .

> [!NOTE]
> [`Resolve<T>`](xref:Xamarin.Forms.DependencyService.Resolve*)메서드는를 통해 응용 프로그램의 종속성 주입 컨테이너에서 형식을 확인할 때 사용 해야 합니다 [`DependencyService`](xref:Xamarin.Forms.DependencyService) .

## <a name="related-links"></a>관련 링크

- [컨테이너를 사용 하 여 종속성 확인 (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/advanced-dependencyresolution-dicontainerdemo)
- [종속성 주입](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md)
- [비디오 플레이어 구현](~/xamarin-forms/app-fundamentals/custom-renderer/video-player/index.md)
- [효과에서 이벤트 호출](~/xamarin-forms/app-fundamentals/effects/touch-tracking.md)
- [그림 라이브러리에서 사진 선택](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
