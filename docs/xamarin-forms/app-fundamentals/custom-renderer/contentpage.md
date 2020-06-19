---
title: ContentPage 사용자 지정
description: ContentPage는 단일 보기를 표시하고 화면 대부분을 차지하는 시각적 요소입니다. 이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: ca9a541c3d152d1b84ed682881c395f2199b9eaf
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84574381"
---
# <a name="customizing-a-contentpage"></a>ContentPage 사용자 지정

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)

_ContentPage는 단일 보기를 표시하고 화면 대부분을 차지하는 시각적 요소입니다. 이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대한 사용자 지정 렌더러를 만드는 방법을 보여줍니다._

모든 Xamarin.Forms 컨트롤에는 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 함께 제공되는 렌더러가 있습니다. Xamarin.Forms 애플리케이션에서 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 렌더링하는 경우 iOS에서 `PageRenderer` 클래스가 인스턴스화되며, 차례로 네이티브 `UIViewController` 컨트롤이 인스턴스화됩니다. Android 플랫폼에서 `PageRenderer` 클래스는 `ViewGroup` 컨트롤을 인스턴스화합니다. UWP(유니버설 Windows 플랫폼)에서 `PageRenderer` 클래스는 `FrameworkElement` 컨트롤을 인스턴스화합니다. Xamarin.Forms 컨트롤에 매핑되는 렌더러 및 네이티브 컨트롤 클래스에 대한 자세한 내용은 [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)을 참조하세요.

다음 다이어그램은 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 및 이를 구현하는 해당 네이티브 컨트롤 간의 관계를 보여줍니다.

![](contentpage-images/contentpage-classes.png "Relationship Between ContentPage Class and Implementing Native Controls")

렌더링 프로세스는 각 플랫폼에서 [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 대한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현하는 데 활용할 수 있습니다. 이 작업을 수행하는 프로세스는 다음과 같습니다.

1. Xamarin.Forms 페이지를 [만듭니다](#creating-the-xamarinforms-page).
1. Xamarin.Forms에서 페이지를 [사용합니다](#consuming-the-xamarinforms-page).
1. 각 플랫폼의 페이지에 대한 사용자 지정 렌더러를 [만듭니다](#creating-the-page-renderer-on-each-platform).

라이브 카메라 피드 및 사진을 캡처하는 기능을 제공하는 `CameraPage`를 구현하기 위해 각 항목을 차례로 살펴보겠습니다.

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms 페이지 만들기

다음 XAML 코드 예제에 나온 대로 변경되지 않은 [`ContentPage`](xref:Xamarin.Forms.ContentPage)를 공유 Xamarin.Forms 프로젝트에 추가할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

마찬가지로, [`ContentPage`](xref:Xamarin.Forms.ContentPage)에 대한 코드 숨김 파일을 다음 코드 예제에 표시된 것처럼 변경하지 않은 상태로 유지해야 합니다.

```csharp
public partial class CameraPage : ContentPage
{
    public CameraPage ()
    {
        // A custom renderer is used to display the camera UI
        InitializeComponent ();
    }
}
```

다음 코드 예제에서는 C#에서 페이지를 어떻게 만들 수 있는지를 보여줍니다.

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

`CameraPage`의 인스턴스는 각 플랫폼에서 라이브 카메라 피드를 표시하는 데 사용됩니다. 컨트롤의 사용자 지정은 사용자 지정 렌더러에서 수행되므로 `CameraPage` 클래스에서 추가 구현할 필요가 없습니다.

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms 페이지 사용

빈 `CameraPage`가 Xamarin.Forms 애플리케이션에서 표시되어야 합니다. 이는 다음 코드 예제에 표시된 대로 `MainPage` 인스턴스의 단추를 탭하여 `OnTakePhotoButtonClicked` 메서드를 실행할 때 발생합니다.

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

이 코드는 간단히 `CameraPage`로 이동하여 사용자 지정 렌더러가 각 플랫폼에서 페이지의 모양을 사용자 지정합니다.

## <a name="creating-the-page-renderer-on-each-platform"></a>각 플랫폼에서 페이지 렌더러 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. `PageRenderer` 클래스의 서브클래스를 만듭니다.
1. 네이티브 페이지를 렌더링하는 `OnElementChanged` 메서드를 재정의하고 페이지를 사용자 지정하는 논리를 작성합니다. `OnElementChanged` 메서드는 해당 Xamarin.Forms 컨트롤이 생성될 때 호출됩니다.
1. 페이지 렌더러 클래스에 `ExportRenderer` 특성을 추가하여 Xamarin.Forms 페이지를 렌더링하는 데 사용하도록 지정합니다. 이 특성은 사용자 지정 렌더러를 Xamarin.Forms에 등록하는 데 사용됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 페이지 렌더러를 제공하는 것은 선택 사항입니다. 페이지 렌더러가 등록되지 않은 경우 페이지에 대한 기본 렌더러가 사용됩니다.

다음 다이어그램은 샘플 애플리케이션에서 각 프로젝트의 책임과 이들 간의 관계를 보여줍니다.

![](contentpage-images/solution-structure.png "CameraPage Custom Renderer Project Responsibilities")

`CameraPage` 인스턴스는 각 플랫폼의 `PageRenderer` 클래스에서 모두 파생되는 플랫폼별 `CameraPageRenderer` 클래스에 의해 렌더링됩니다. 그러면 다음 스크린샷과 같이 각 `CameraPage` 인스턴스가 라이브 카메라 피드로 렌더링됩니다.

![](contentpage-images/screenshots.png "CameraPage on each Platform")

`PageRenderer` 클래스는 해당 네이티브 컨트롤을 렌더링하기 위해 Xamarin.Forms 페이지가 생성될 때 호출되는 `OnElementChanged` 메서드를 노출합니다. 이 메서드는 `OldElement` 및 `NewElement` 속성이 포함된 `ElementChangedEventArgs` 매개 변수를 가져옵니다. 이러한 속성은 렌더러가 연결된 Xamarin.Forms 요소와 렌더러가 연결되는 Xamarin.Forms 요소를 각각 나타냅니다. 샘플 애플리케이션에서 `OldElement` 속성은 `null`이고, `NewElement` 속성은 `CameraPage` 인스턴스에 대한 참조를 포함합니다.

`CameraPageRenderer` 클래스에서 `OnElementChanged` 메서드의 재정의된 버전은 네이티브 페이지 사용자 지정을 수행하는 위치입니다. 또한 렌더링되는 Xamarin.Forms 페이지 인스턴스에 대한 참조는 `Element` 속성을 통해 얻을 수 있습니다.

각 사용자 지정 렌더러 클래스는 렌더러를 Xamarin.Forms에 등록하는 `ExportRenderer` 특성으로 데코레이트됩니다. 이 특성은 렌더링되는 Xamarin.Forms 페이지의 형식 이름과 사용자 지정 렌더러의 형식 이름이라는 두 가지 매개 변수를 사용합니다. 특성의 `assembly` 접두사는 특성이 전체 어셈블리에 적용되도록 지정합니다.

다음 섹션에서는 각 플랫폼에 대한 `CameraPageRenderer` 사용자 지정 렌더러의 구현을 설명합니다.

### <a name="creating-the-page-renderer-on-ios"></a>iOS에서 페이지 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼용 페이지 렌더러를 보여줍니다.

```csharp
[assembly:ExportRenderer (typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPageRenderer : PageRenderer
    {
        ...

        protected override void OnElementChanged (VisualElementChangedEventArgs e)
        {
            base.OnElementChanged (e);

            if (e.OldElement != null || Element == null) {
                return;
            }

            try {
                SetupUserInterface ();
                SetupEventHandlers ();
                SetupLiveCameraStream ();
                AuthorizeCameraUse ();
            } catch (Exception ex) {
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 iOS `UIViewController` 컨트롤을 인스턴스화합니다. 라이브 카메라 스트림은 렌더러가 기존 Xamarin.Forms 요소에 아직 연결되어 있지 않은 경우 및 사용자 지정 렌더러에 의해 렌더링된 페이지 인스턴스가 존재하는 경우에만 렌더링됩니다.

페이지는 카메라의 라이브 스트림 및 사진을 캡처하는 기능을 제공하는 데 `AVCapture` API를 사용하는 일련의 메서드에 의해 사용자 지정됩니다.

### <a name="creating-the-page-renderer-on-android"></a>Android에서 페이지 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼용 페이지 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPageRenderer : PageRenderer, TextureView.ISurfaceTextureListener
    {
        ...
        public CameraPageRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetupUserInterface();
                SetupEventHandlers();
                AddView(view);
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 보기의 그룹인 Android `ViewGroup` 컨트롤을 인스턴스화합니다. 라이브 카메라 스트림은 렌더러가 기존 Xamarin.Forms 요소에 아직 연결되어 있지 않은 경우 및 사용자 지정 렌더러에 의해 렌더링된 페이지 인스턴스가 존재하는 경우에만 렌더링됩니다.

그런 다음, 페이지는 `AddView` 메서드가 호출되어 `ViewGroup`에 라이브 카메라 스트림 UI를 추가하기 전에, 카메라의 라이브 스트림 및 사진을 캡처하는 기능을 제공하는 데 `Camera` API를 사용하는 일련의 메서드를 호출하여 사용자 지정됩니다. Android에서는 보기에서 측정값 및 레이아웃 작업을 수행하는 `OnLayout` 메서드도 재정의해야 합니다. 자세한 내용은 [ContentPage 렌더러 샘플](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)을 참조하세요.

### <a name="creating-the-page-renderer-on-uwp"></a>UWP에서 페이지 렌더러 만들기

다음 코드 예제는 UWP용 페이지 렌더러를 보여줍니다.

```csharp
[assembly: ExportRenderer(typeof(CameraPage), typeof(CameraPageRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPageRenderer : PageRenderer
    {
        ...
        protected override void OnElementChanged(ElementChangedEventArgs<Xamarin.Forms.Page> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                ...
                SetupUserInterface();
                SetupBasedOnStateAsync();

                this.Children.Add(page);
            }
            ...
        }

        protected override Size ArrangeOverride(Size finalSize)
        {
            page.Arrange(new Windows.Foundation.Rect(0, 0, finalSize.Width, finalSize.Height));
            return finalSize;
        }
        ...
    }
}

```

기본 클래스의 `OnElementChanged` 메서드에 대한 호출은 페이지가 렌더링되는 `FrameworkElement` 컨트롤을 인스턴스화합니다. 라이브 카메라 스트림은 렌더러가 기존 Xamarin.Forms 요소에 아직 연결되어 있지 않은 경우 및 사용자 지정 렌더러에 의해 렌더링된 페이지 인스턴스가 존재하는 경우에만 렌더링됩니다. 그런 다음, 페이지는 사용자 지정된 페이지가 표시를 위해 `Children` 컬렉션에 추가되기 전에, 카메라의 라이브 스트림 및 사진을 캡처하는 기능을 제공하는 데 `MediaCapture` API를 사용하는 일련의 메서드를 호출하여 사용자 지정됩니다.

UWP에서 `PageRenderer`로부터 파생되는 사용자 지정 렌더러를 구현하는 경우 기본 렌더러는 수행할 작업을 알 수 없으므로 `ArrangeOverride` 메서드 또한 페이지 컨트롤을 정렬하도록 구현되어야 합니다. 그렇지 않은 경우 빈 페이지가 발생합니다. 따라서 이 예제에서 `ArrangeOverride` 메서드는 `Page` 인스턴스에서 `Arrange` 메서드를 호출합니다.

> [!NOTE]
> UWP 애플리케이션에서 카메라에 대한 액세스를 제공하는 개체를 중지 및 삭제하는 것이 중요합니다. 이렇게 하지 않으면 디바이스의 카메라에 액세스하려고 하는 다른 애플리케이션을 방해할 수 있습니다. 자세한 내용은 [카메라 미리 보기 표시](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access)를 참조하세요.

## <a name="summary"></a>요약

이 문서에서는 개발자가 자체적인 플랫폼별 사용자 지정을 통해 기본 네이티브 렌더링을 재정의할 수 있도록 [`ContentPage`](xref:Xamarin.Forms.ContentPage) 페이지에 대한 사용자 지정 렌더러를 만드는 방법을 설명했습니다. `ContentPage`는 단일 보기를 표시하고 화면 대부분을 차지하는 시각적 요소입니다.

## <a name="related-links"></a>관련 링크

- [CustomRendererContentPage(샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-contentpage)
