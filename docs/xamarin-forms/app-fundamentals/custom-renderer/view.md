---
title: 보기 구현
description: 이 문서에서는 디바이스의 카메라에서 미리 보기 비디오 스트림을 표시하는 데 사용되는 Xamarin.Forms 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 22392603e337205dcdd4909dc61b6c22ca2f00b9
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057972"
---
# <a name="implementing-a-view"></a>보기 구현

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)

_Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤은 화면에 레이아웃과 컨트롤을 배치하는 데 사용되는 보기 클래스에서 파생되어야 합니다. 이 문서에서는 디바이스 카메라에서 미리 보기 비디오 스트림을 표시하는 데 사용되는 Xamarin.Forms 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 설명합니다._

모든 Xamarin.Forms 보기에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대해 함께 제공되는 렌더러가 있습니다. iOS의 Xamarin.Forms 애플리케이션에서 [`View`](xref:Xamarin.Forms.View)를 렌더링하면 `ViewRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `UIView` 컨트롤이 인스턴스화됩니다. Android 플랫폼에서 `ViewRenderer` 클래스는 네이티브 `View` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `ViewRenderer` 클래스는 네이티브 `FrameworkElement` 컨트롤을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`View`](xref:Xamarin.Forms.View) 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](view-images/view-classes.png "보기 클래스와 네이티브 클래스 구현 간의 관계")

렌더링 프로세스는 각 플랫폼에서 [`View`](xref:Xamarin.Forms.View)에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 사용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 사용자 지정 컨트롤을 [만듭니다](#Creating_the_Custom_Control).
1. Xamarin.Forms에서 사용자 지정 컨트롤을 [사용합니다](#Consuming_the_Custom_Control).
1. 각 플랫폼의 컨트롤에 대한 사용자 지정 렌더러를 [만듭니다](#Creating_the_Custom_Renderer_on_each_Platform).

디바이스의 카메라에서 미리 보기 비디오 스트림을 표시하는 `CameraPreview` 렌더러를 구현하기 위해 각 항목을 차례로 살펴보겠습니다. 비디오 스트림을 탭하면 중지 및 시작됩니다.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>사용자 지정 컨트롤 만들기

다음 코드 예제와 같이 [`View`](xref:Xamarin.Forms.View) 클래스를 서브클래스하여 사용자 지정 컨트롤을 만들 수 있습니다.

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

`CameraPreview` 사용자 지정 컨트롤은 PCL(이식 가능한 클래스 라이브러리) 프로젝트에서 생성되고 컨트롤에 대한 API를 정의합니다. 사용자 지정 컨트롤은 디바이스의 전면 또는 후면 카메라의 비디오 스트림을 표시해야 하는지 여부를 제어하는 데 사용되는 `Camera` 속성을 노출합니다. 컨트롤이 생성될 때 `Camera` 속성에 대해 값이 지정되지 않은 경우에는 기본적으로 후면 카메라가 지정됩니다.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`CameraPreview` 사용자 지정 컨트롤은 해당 위치의 네임스페이스를 선언하고 사용자 지정 컨트롤 요소의 네임스페이스 접두사를 사용하여 PCL 프로젝트의 XAML에서 참조할 수 있습니다. 다음 코드 예제에서는 `CameraPreview` 사용자 지정 컨트롤을 XAML 페이지에서 사용하는 방법을 보여줍니다.

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

`local` 네임스페이스 접두사는 원하는 이름으로 지정할 수 있습니다. 그러나 `clr-namespace` 및 `assembly` 값은 사용자 지정 컨트롤의 세부 정보와 일치해야 합니다. 네임스페이스가 선언되면 사용자 지정 컨트롤을 참조하는 데 접두사가 사용됩니다.

다음 코드 예제에서는 C# 페이지에서 `CameraPreview` 사용자 지정 컨트롤을 사용하는 방법을 보여줍니다.

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

`CameraPreview` 사용자 지정 컨트롤의 인스턴스는 디바이스의 카메라에서 미리 보기 비디오 스트림을 표시하는 데 사용됩니다. 필요에 따라 `Camera` 속성에 대한 값을 지정하는 것 외에 컨트롤의 사용자 지정은 사용자 지정 렌더러에서 수행됩니다.

이제 각 애플리케이션 프로젝트에 사용자 지정 렌더러를 추가하여 플랫폼별 카메라 미리 보기 컨트롤을 만들 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 사용자 지정 컨트롤을 렌더링하는 `ViewRenderer<T1,T2>` 클래스의 서브클래스를 만듭니다. 첫 번째 형식 인수는 렌더러가 사용할(이 경우 `CameraPreview`) 사용자 지정 컨트롤이어야 합니다. 두 번째 형식 인수는 사용자 지정 컨트롤을 구현할 네이티브 컨트롤이어야 합니다.
1. 사용자 지정 컨트롤을 렌더링하는 `OnElementChanged` 메서드를 정의하고 이를 사용자 지정하기 위한 논리를 작성합니다. 이 메서드는 해당 Xamarin.Forms 컨트롤이 생성될 때 호출됩니다.
1. 사용자 지정 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 사용자 지정 컨트롤을 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 랜더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소의 경우 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공하는 것은 선택 사항입니다. 사용자 지정 렌더러가 등록되지 않은 경우 컨트롤의 기본 클래스에 대한 기본 렌더러가 사용됩니다. 그러나 [보기](xref:Xamarin.Forms.View) 요소를 렌더링하는 경우에는 각 플랫폼 프로젝트에 사용자 지정 렌더러가 필요합니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](view-images/solution-structure.png "CameraPreview 사용자 지정 렌더러 프로젝트 책임")

`CameraPreview` 사용자 지정 컨트롤은 각 플랫폼의 `ViewRenderer` 클래스에서 모두 파생되는 플랫폼별 렌더러 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `CameraPreview` 사용자 지정 컨트롤이 플랫폼별 컨트롤로 렌더링됩니다.

![](view-images/screenshots.png "각 플랫폼의 CameraPreview")

`ViewRenderer` 클래스는 해당 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 사용자 지정 컨트롤이 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 렌더러가 연결*된* Xamarin.Forms 요소와 렌더러가 연결*되는* Xamarin.Forms 요소를 각각 나타냅니다. 샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `CameraPreview` 인스턴스에 대한 참조를 포함합니다.

각 플랫폼별 렌더러 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 컨트롤 인스턴스화 및 사용자 지정을 수행하는 위치입니다. `SetNativeControl` 메서드는 네이티브 컨트롤을 인스턴스화하는 데 사용되어야 하며, 이 메서드는 또한 `Control` 속성에 컨트롤 참조를 할당합니다. 또한 랜더링되는 Xamarin.Forms 컨트롤에 대한 참조는 `Element` 속성을 통해 얻을 수 있습니다.

그러나 일부 경우에는 `OnElementChanged` 메서드가 여러 번 호출될 수 있습니다. 따라서 메모리 누수를 방지하려면 새로운 네이티브 컨트롤을 인스턴스화할 때 주의를 기울여야 합니다. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우 이 컨트롤만 구성해야 합니다. 마찬가지로, 렌더러가 변경된 항목에 연결된 경우 구독 대상 이벤트 처리기의 구독이 취소되어야 합니다. 이러한 방식을 채택하면 메모리 누수가 없는 성능이 뛰어난 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

각 사용자 지정 렌더러 클래스는 랜더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 속성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름과 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 플랫폼별 사용자 지정 렌더러 클래스의 구현을 설명합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>iOS에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

`Control` 속성이 `null`인 경우 `SetNativeControl` 메서드가 호출되어 새 `UICameraPreview` 컨트롤을 인스턴스화하고 `Control` 속성에 대한 참조를 할당합니다. `UICameraPreview` 컨트롤은 카메라의 미리 보기 스트림을 제공하는 데 `AVCapture` API를 사용하는 플랫폼별 사용자 지정 컨트롤입니다. `OnCameraPreviewTapped` 메서드에 의해 처리되는 `Tapped` 이벤트를 노출하여 탭할 때 비디오 미리 보기를 중지 및 시작합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `Tapped` 이벤트가 구독되며, 렌더러가 변경 내용에 연결된 요소에서만 구독이 취소됩니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android에서 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

`Control` 속성이 `null`인 경우 `SetNativeControl` 메서드가 호출되어 새 `CameraPreview` 컨트롤을 인스턴스화하고 `Control` 속성에 대한 참조를 할당합니다. `CameraPreview` 컨트롤은 카메라의 미리 보기 스트림을 제공하는 데 `Camera` API를 사용하는 플랫폼별 사용자 지정 컨트롤입니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `CameraPreview` 컨트롤이 구성됩니다. 이 구성에는 특정 하드웨어 카메라에 액세스하기 위한 새 네이티브 `Camera`개체 만들기 및 `Click` 이벤트를 처리하기 위한 이벤트 처리기 등록이 포함됩니다. 따라서 이 처리기는 탭하면 비디오 미리 보기를 멈추고 시작합니다. `Click` 이벤트는 렌더러가 변경 내용에 연결된 Xamarin.Forms 요소에서만 구독이 취소됩니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에서 사용자 지정 렌더러 만들기

다음 코드 예제는 UWP용 사용자 지정 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

`Control` 속성이 `null`인 경우 새 `CaptureElement`가 인스턴스화되고 카메라에서 미리 보기 스트림을 제공하는 데 `MediaCapture` API를 사용하는 `SetupCamera` 메서드가 호출됩니다. 그런 다음, `SetNativeControl` 메서드가 호출되어 `CaptureElement` 인스턴스에 대한 참조를 `Control` 속성에 할당합니다. `CaptureElement` 컨트롤은 `OnCameraPreviewTapped` 메서드에 의해 처리되는 `Tapped` 이벤트를 노출하여 탭할 때 비디오 미리 보기를 중지 및 시작합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결되어 있는 경우 `Tapped` 이벤트가 구독되며, 렌더러가 변경 내용에 연결된 요소에서만 구독이 취소됩니다.

> [!NOTE]
> UWP 애플리케이션에서 카메라에 대한 액세스를 제공하는 개체를 중지 및 삭제하는 것이 중요합니다. 이렇게 하지 않으면 디바이스의 카메라에 액세스하려고 하는 다른 애플리케이션을 방해할 수 있습니다. 자세한 내용은 [카메라 미리 보기 표시](/windows/uwp/audio-video-camera/simple-camera-preview-access/)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 디바이스 카메라에서 미리 보기 비디오 스트림을 표시하는 데 사용되는 Xamarin.Forms 사용자 지정 컨트롤에 대한 사용자 지정 렌더러를 만드는 방법을 설명했습니다. Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤은 화면에 레이아웃과 컨트롤을 배치하는 데 사용되는 [`View`](xref:Xamarin.Forms.View) 클래스에서 파생되어야 합니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererView(샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
