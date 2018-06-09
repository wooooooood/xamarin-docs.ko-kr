---
title: 뷰를 구현합니다.
description: 이 문서에서는 장치의 카메라에서 미리 보기 비디오 스트림을 표시 하는 데 사용 되는 Xamarin.Forms 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: eb0bc199bfd31ac631ca9d131796b606960d76aa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240488"
---
# <a name="implementing-a-view"></a>뷰를 구현합니다.

_Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤 레이아웃와 화면에 컨트롤을 배치 하는 데 사용 되는 뷰 클래스에서 파생 되어야 합니다. 이 문서에는 장치의 카메라에서 미리 보기 비디오 스트림을 표시 하는 데 사용 되는 Xamarin.Forms 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 보기에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) ios에서는 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `ViewRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `UIView` 제어 합니다. Android 플랫폼의 `ViewRenderer` 클래스 인스턴스화합니다 네이티브 `View` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `ViewRenderer` 클래스 인스턴스화합니다 네이티브 `FrameworkElement` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 및 구현 하는 해당 네이티브 컨트롤:

![](view-images/view-classes.png "뷰 클래스와 해당 기본 클래스 구현 간의 관계")

에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현 하는 렌더링 프로세스를 사용할 수 있습니다는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Custom_Control) Xamarin.Forms 사용자 지정 컨트롤입니다.
1. [사용할](#Consuming_the_Custom_Control) Xamarin.Forms에서 사용자 지정 컨트롤입니다.
1. [만들](#Creating_the_Custom_Renderer_on_each_Platform) 각 플랫폼에서 컨트롤에 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 `CameraPreview` 장치의 카메라에서 비디오 스트림을 미리 보기를 표시 하는 렌더러 합니다. 비디오 스트림 탭 중지 하 고 시작 됩니다.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>사용자 지정 컨트롤 만들기

사용자 지정 컨트롤을 서브클래싱하 여 만들 수 있습니다는 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 다음 코드 예제와 같이 클래스:

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

`CameraPreview` 사용자 지정 컨트롤 이식 가능한 클래스 라이브러리 PCL 프로젝트에 만들어지고 컨트롤에 대 한 API를 정의 합니다. 사용자 지정 컨트롤을 노출 한 `Camera` 비디오 스트림 전면 또는 후면 카메라를 장치에 표시할지 여부를 제어 하기 위한 사용 되는 속성입니다. 에 대 한 값을 지정 하지 않으면는 `Camera` 기본적으로 후면 카메라를 지정 하는 컨트롤을 만들 경우 속성을 합니다.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>사용자 지정 컨트롤 사용

`CameraPreview` 사용자 지정 컨트롤 수 XAML에서 참조할 수는 PCL 프로젝트에 해당 위치에 대 한 네임 스페이스를 선언 하 고 사용자 지정 컨트롤 요소에 네임 스페이스 접두사를 사용 하 여 합니다. 다음 코드 예제는 방법을 `CameraPreview` XAML 페이지에서 사용자 지정 컨트롤을 사용할 수 있습니다.

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

`local` 원하는 네임 스페이스 접두사를 이름을 지정할 수 있습니다. 그러나는 `clr-namespace` 및 `assembly` 값에는 사용자 지정 컨트롤의 세부 정보과 일치 해야 합니다. 네임 스페이스 선언 되 면 사용자 지정 컨트롤을 참조 하는 접두사가 사용 됩니다.

다음 코드 예제는 방법을 `CameraPreview` C# 페이지 사용자 지정 컨트롤을 사용할 수 있습니다.

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

인스턴스는 `CameraPreview` 사용자 지정 컨트롤에서 장치의 카메라에서 비디오 스트림을 미리 보기를 표시 하는 데 사용 됩니다. 필요에 따라 값을 지정 하는 것 외의 `Camera` 속성, 컨트롤의 사용자 지정에 사용자 지정 렌더러에서 수행 됩니다.

사용자 지정 렌더러는 이제 플랫폼별 카메라 미리 보기 컨트롤을 만드는 각 응용 프로그램 프로젝트에 추가할 수 있습니다.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>각 플랫폼에서 사용자 지정 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `ViewRenderer<T1,T2>` 사용자 지정 컨트롤을 렌더링 하는 클래스입니다. 첫 번째 형식 인수를 사용자 지정 컨트롤 렌더러,이 경우 해야 `CameraPreview`합니다. 두 번째 형식 인수는 사용자 지정 컨트롤을 구현 하는 기본 컨트롤 이어야 합니다.
1. 재정의 `OnElementChanged` 사용자 지정 하는 사용자 지정 권한 및 쓰기 논리를 렌더링 하는 메서드입니다. 이 메서드는 해당 하는 Xamarin.Forms 컨트롤이 만들어질 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용자 지정 렌더러 클래스 Xamarin.Forms 사용자 지정 컨트롤을 렌더링 하 사용 수를 지정할 수 있습니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 대부분의 Xamarin.Forms 요소에 대 한 각 플랫폼 프로젝트에서 사용자 지정 렌더러를 제공 하기 선택 사항입니다. 사용자 지정 렌더러 등록 되지 않은 경우 컨트롤의 기본 클래스에 대 한 기본 렌더러 사용 됩니다. 그러나 사용자 지정 렌더러 필요한 각 플랫폼 프로젝트에 렌더링 하는 경우는 [보기](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 요소입니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](view-images/solution-structure.png "CameraPreview 사용자 지정 렌더러 프로젝트 책임")

`CameraPreview` 모든에서 파생 되는 플랫폼 특정 렌더러 클래스에서 사용자 지정 컨트롤이 렌더링 되는 `ViewRenderer` 각 플랫폼에 대 한 클래스입니다. 이 인해 각 `CameraPreview` 다음 스크린샷에서 같이 플랫폼 관련 컨트롤을 렌더링 하는 사용자 지정 컨트롤:

![](view-images/screenshots.png "각 플랫폼에서 CameraPreview")

`ViewRenderer` 클래스가 노출은 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 사용자 지정 컨트롤을 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 하는 매개 변수 `OldElement` 및 `NewElement` 속성입니다. Xamarin.Forms 요소를 나타내야 하는 이러한 속성 하는 렌더러 *되었습니다* 에, 연결 된 및 Xamarin.Forms 요소는 렌더러 *은* 에, 각각 연결 된 합니다. 샘플 응용 프로그램에서의 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조에 포함 됩니다는 `CameraPreview` 인스턴스.

재정의 된 버전의 `OnElementChanged` 각 플랫폼별 렌더러 클래스에서 메서드는 네이티브 컨트롤 인스턴스화 및 사용자 지정을 수행할 수 있는 곳입니다. `SetNativeControl` 네이티브 컨트롤을 인스턴스화하 메서드를 사용 해야 하 고이 메서드는 컨트롤 참조를 할당 된 `Control` 속성입니다. 또한 통해 렌더링 되는 Xamarin.Forms 컨트롤에 대 한 참조를 가져올 수 있습니다는 `Element` 속성입니다.

일부 경우에는 `OnElementChanged` 메서드가 여러 번 호출할 수 있습니다. 따라서 메모리 누수를 방지 하려면 주의 해야 새 네이티브 컨트롤을 인스턴스화할 때. 사용자 지정 렌더러에서 새 네이티브 컨트롤을 인스턴스화할 때 사용하는 방법은 다음 코드 예제에 표시되어 있습니다.

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

`Control` 속성이 `null`일 경우 새 네이티브 컨트롤은 한 번만 인스턴스화되어야 합니다. 사용자 지정 렌더러가 새 Xamarin.Forms 요소에 연결된 경우 이 컨트롤만 구성해야 합니다. 마찬가지로, 구독 했던 모든 이벤트 처리기만 취소 해야 렌더러에 연결 된 요소 변경 될 때에서 구독 합니다. 이 방법을 채택 메모리 누수에서 발생 하지 않는 한 고성능 사용자 지정 렌더러를 만드는 데 도움이 됩니다.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 사용자 지정 컨트롤의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 각 사용자 지정 렌더러 플랫폼 특정 클래스의 구현에 설명 합니다.

### <a name="creating-the-custom-renderer-on-ios"></a>Ios 사용자 지정 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

있는 경우는 `Control` 속성은 `null`, `SetNativeControl` 메서드는 새 `UICameraPreview` 컨트롤에 대 한 참조 항목을 할당할 수는 `Control` 속성입니다. `UICameraPreview` 컨트롤을 사용 하는 플랫폼 특정 사용자 지정 컨트롤은 `AVCapture` 는 카메라의 미리 보기 스트림을 제공 하는 Api입니다. 노출 한 `Tapped` 에서 처리 하는 이벤트는 `OnCameraPreviewTapped` 메서드를 중지 하는 탭 비디오 미리 보기를 시작 합니다. `Tapped` 이벤트를 구독 하는 사용자 지정 렌더러를 새 Xamarin.Forms 요소에 연결 하 고 렌더러 요소 변경 내용에 추가 되 면에에서 구독 취소 했습니다.

### <a name="creating-the-custom-renderer-on-android"></a>Android 사용자 지정 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

에 `Control` 속성은 `null`, `SetNativeControl` 메서드는 새 `CameraPreview` 제어에 대 한 참조 항목을 할당 하 고는 `Control` 속성입니다. `CameraPreview` 컨트롤을 사용 하는 플랫폼 특정 사용자 지정 컨트롤은 `Camera` 는 카메라의 미리 보기 스트림을 제공 하는 API입니다. `CameraPreview` 컨트롤은 다음 구성, 사용자 지정 렌더러 새 Xamarin.Forms 요소에 연결 되어 있는 경우. 이 구성은 새 네이티브를 만들어야 `Camera` 특정 하드웨어 카메라에 액세스 하려면 개체를 처리 하는 이벤트 처리기를 등록 하는 중는 `Click` 이벤트입니다. 차례로이 처리기 중지 하 고는 탭 비디오 미리 보기를 시작 합니다. `Click` Xamarin.Forms 요소 렌더러는 변경 내용에 연결 된 경우 이벤트를 구독 취소 합니다.

### <a name="creating-the-custom-renderer-on-uwp"></a>UWP에 사용자 지정 렌더러 만들기

다음 코드 예제에서는 UWP에 대 한 사용자 지정 렌더러를 보여 줍니다.

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

에 `Control` 속성은 `null`, 새 `CaptureElement` 인스턴스화될 및 `SetupCamera` 메서드가 호출 되 면 사용 하는 `MediaCapture` 카메라에서 미리 보기 스트림을 제공 하는 API입니다. `SetNativeControl` 메서드를 호출에 대 한 참조를 할당 하는 `CaptureElement` 인스턴스는 `Control` 속성입니다. `CaptureElement` 노출 제어는 `Tapped` 에서 처리 하는 이벤트는 `OnCameraPreviewTapped` 메서드를 중지 하는 탭 비디오 미리 보기를 시작 합니다. `Tapped` 이벤트를 구독 하는 사용자 지정 렌더러를 새 Xamarin.Forms 요소에 연결 하 고 렌더러 요소 변경 내용에 추가 되 면에에서 구독 취소 했습니다.

> [!NOTE]
> 중지 하 고 UWP 응용 프로그램에서 카메라에 대 한 액세스를 제공 하는 개체를 삭제 하는 것이 유용 합니다. 이렇게 하지 않으면 장치의 카메라를 액세스 하려고 하는 다른 응용 프로그램을 방해할 수 있습니다. 자세한 내용은 참조 [카메라 미리 보기 표시](/windows/uwp/audio-video-camera/simple-camera-preview-access/)합니다.

## <a name="summary"></a>요약

이 문서 장치의 카메라에서 미리 보기 비디오 스트림을 표시 하는 데 사용 되는 Xamarin.Forms 사용자 지정 컨트롤에 대 한 사용자 지정 렌더러를 만드는 방법을 제시 합니다. Xamarin.Forms 사용자 지정 사용자 인터페이스 컨트롤에서 파생 된 [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) 레이아웃와 화면에 컨트롤을 배치 하는 데 사용 되는 클래스입니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererView (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/view/)
