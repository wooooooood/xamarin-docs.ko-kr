---
title: ContentPage 사용자 지정
description: ContentPage는 단일 보기를 표시 하는 화면의 대부분을 차지 하는 시각적 요소입니다. 이 문서에는 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0f4de4594e8abb8d0ee03690e5829193c51a3736
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-contentpage"></a>ContentPage 사용자 지정

_ContentPage는 단일 보기를 표시 하는 화면의 대부분을 차지 하는 시각적 요소입니다. 이 문서에는 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

Xamarin.Forms는 모든 컨트롤에 네이티브 컨트롤의 인스턴스를 생성 하는 각 플랫폼에 대 한 함께 제공 되는 렌더러 있습니다. 경우는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) ios에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `PageRenderer` 네이티브 다시 인스턴스화하는 클래스를 인스턴스화할 `UIViewController` 제어 합니다. Android 플랫폼의 `PageRenderer` 클래스를 인스턴스화하는 `ViewGroup` 제어 합니다. 에 플랫폼 UWP (유니버설 Windows)는 `PageRenderer` 클래스를 인스턴스화하는 `FrameworkElement` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 [렌더러 기본 클래스와 기본 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램에서는 간의 관계는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 및 구현 하는 해당 네이티브 컨트롤:

![](contentpage-images/contentpage-classes.png "ContentPage 클래스와 기본 컨트롤을 구현 간의 관계")

렌더링 프로세스 활용 하기 위해 취할 수에 대 한 사용자 지정 렌더러를 만들어 플랫폼 관련 사용자 지정을 구현는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 각 플랫폼에 있습니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms 페이지.
1. [사용할](#Consuming_the_Xamarin.Forms_Page) Xamarin.Forms에서 페이지입니다.
1. [만들](#Creating_the_Page_Renderer_on_each_Platform) 각 플랫폼에 대 한 페이지에 대 한 사용자 지정 렌더러 합니다.

각 항목 이제 살펴봅니다 차례로 구현 하는 `CameraPage` 피드 라이브 카메라 및 사진을 캡처하는 기능을 제공 하는 합니다.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms 페이지 만들기

변경 되지 않은 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 다음 XAML 코드 예제에 나와 있는 것 처럼 공유 Xamarin.Forms 프로젝트에 추가할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

마찬가지로, 코드 숨김 파일에 대 한는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 도 유지할지 변경 되지 않은 경우, 다음 코드 예제에 나와 있는 것 처럼:

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

다음 코드 예제에서는 C#에서 페이지를 만들고 하는 방법을 보여 줍니다.

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

인스턴스는 `CameraPage` 각 플랫폼에 피드 라이브 카메라 표시할 사용 됩니다. 컨트롤의 사용자 지정을 수행할 사용자 지정 렌더러의 하므로 추가 구현이 필요는 `CameraPage` 클래스입니다.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms 페이지를 사용합니다.

빈 `CameraPage` Xamarin.Forms 응용 프로그램에서 표시 되어야 합니다. 경우가이에 있는 단추는 `MainPage` 차례로 실행 되는 인스턴스 탭의 `OnTakePhotoButtonClicked` 메서드를 다음 코드 예제에 나와 있는 것 처럼:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

이 코드를 단순히 탐색는 `CameraPage`에 사용자 지정 렌더러는 각 플랫폼에서 페이지의 모양을 사용자 지정 합니다.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>페이지 렌더러는 각 플랫폼에서 만들기

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든는 `PageRenderer` 클래스입니다.
1. 재정의 `OnElementChanged` 페이지를 사용자 지정 하는 기본 페이지 및 쓰기 논리를 렌더링 하는 메서드. `OnElementChanged` 메서드 해당 Xamarin.Forms 컨트롤을 만들 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용 Xamarin.Forms 페이지를 렌더링 하는 않도록 지정 하려면 페이지 렌더러 클래스입니다. 이 특성은 Xamarin.Forms를 사용한 사용자 지정 렌더러를 등록 하려면 사용 합니다.

> [!NOTE]
> 각 플랫폼 프로젝트에는 페이지 렌더러 수 있도록 선택적입니다. 페이지 렌더러 등록 되지 않은 페이지에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 둘 사이의 관계와 함께 샘플 응용 프로그램의 각 프로젝트의 책임을 보여줍니다.

![](contentpage-images/solution-structure.png "CameraPage 사용자 지정 렌더러 프로젝트 책임")

`CameraPage` 인스턴스 플랫폼별에 의해 렌더링 된 `CameraPageRenderer` 모든에서 파생 되는 클래스는 `PageRenderer` 해당 플랫폼에 대 한 클래스입니다. 이 인해 각 `CameraPage` 다음 스크린샷에 표시 된 것 처럼 라이브 카메라 피드를 사용 하 여 렌더링 되 고 인스턴스:

![](contentpage-images/screenshots.png "각 플랫폼에서 CameraPage")

`PageRenderer` 클래스가 노출은 `OnElementChanged` 메서드를 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 페이지를 만들 때 호출 됩니다. 이 메서드는 `ElementChangedEventArgs` 포함 하는 매개 변수 `OldElement` 및 `NewElement` 속성입니다. Xamarin.Forms 요소를 나타내야 하는 이러한 속성 하는 렌더러 *되었습니다* 에, 연결 된 및 Xamarin.Forms 요소는 렌더러 *은* 에, 각각 연결 된 합니다. 샘플 응용 프로그램에서의 `OldElement` 속성이 `null` 및 `NewElement` 속성에 대 한 참조에 포함 됩니다는 `CameraPage` 인스턴스.

재정의 된 버전의 `OnElementChanged` 에서 메서드는 `CameraPageRenderer` 클래스는 기본 페이지 사용자 지정을 수행 하기에 적합 합니다. 통해 렌더링 되는 Xamarin.Forms 페이지 인스턴스에 대 한 참조를 가져올 수는 `Element` 속성입니다.

각 사용자 지정 렌더러 클래스도 데코레이팅되 어는 `ExportRenderer` xamarin.forms 렌더러를 등록 하는 특성입니다. 속성에는 두 개의 매개 변수-렌더링 되 고 Xamarin.Forms 페이지의 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 특성에 대 한 접두사 특성이 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 설명의 구현에서 `CameraPageRenderer` 각 플랫폼에 대 한 사용자 지정 렌더러 합니다.

### <a name="creating-the-page-renderer-on-ios"></a>IOS에서 페이지 렌더러 만들기

다음 코드 예제에서는 iOS 플랫폼에 대 한 페이지 렌더러를 보여 줍니다.

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
                System.Diagnostics.Debug.WriteLine (@"          ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데 iOS `UIViewController` 제어 합니다. 라이브 카메라 스트림 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않으면 사용자 지정 렌더러를 통해 렌더링 되 고 있는 페이지 인스턴스가 존재 하는 권한과만 렌더링 됩니다.

페이지를 일련의 메서드를 사용 하는 다음 사용자 지정는 `AVCapture` 사진을 캡처하는 기능과 카메라에서 라이브 스트림을 제공 하기 위한 Api입니다.

### <a name="creating-the-page-renderer-on-android"></a>Android에서 페이지 렌더러 만들기

다음 코드 예제에서는 Android 플랫폼에 대 한 페이지 렌더러를 보여 줍니다.

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
                System.Diagnostics.Debug.WriteLine(@"           ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 Android `ViewGroup` 컨트롤 뷰의 그룹입니다. 라이브 카메라 스트림 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않으면 사용자 지정 렌더러를 통해 렌더링 되 고 있는 페이지 인스턴스가 존재 하는 권한과만 렌더링 됩니다.

일련의를 사용 하는 메서드를 호출 하 여 페이지를 사용자 지정한 다음는 `Camera` 전에 사진, 캡처할 수 있는 기능과 카메라에서 라이브 스트림을 제공 하는 API는 `AddView` 메서드를 호출 하는 라이브 카메라 추가 UI를 스트리밍하는 `ViewGroup`합니다.

### <a name="creating-the-page-renderer-on-uwp"></a>페이지 렌더러 UWP에서 만들기

다음 코드 예제에서는 UWP에 대 한 페이지 렌더러를 보여 줍니다.

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 `FrameworkElement` 컨트롤, 페이지를 렌더링 합니다. 라이브 카메라 스트림 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않으면 사용자 지정 렌더러를 통해 렌더링 되 고 있는 페이지 인스턴스가 존재 하는 권한과만 렌더링 됩니다. 일련의를 사용 하는 메서드를 호출 하 여 페이지를 사용자 지정한 다음는 `MediaCapture` 를 사용자 지정된 된 페이지에 추가 되기 전에 사진을 캡처하는 기능과 카메라에서 라이브 스트림을 제공 하는 API는 `Children` 디스플레이 대 한 컬렉션입니다.

파생 되는 사용자 지정 렌더러를 구현 하는 경우 `PageRenderer` UWP에 `ArrangeOverride` 기본 렌더러를 수행할지 무엇 인지 인식 하지 때문에 메서드 페이지 컨트롤을 정렬 하려면 구현도 해야 합니다. 그렇지 않은 경우 빈 페이지가 발생합니다. 따라서이 예제에에서는 `ArrangeOverride` 메서드 호출의 `Arrange` 메서드를는 `Page` 인스턴스.

> [!NOTE]
> 중지 하 고 UWP 응용 프로그램에서 카메라에 대 한 액세스를 제공 하는 개체를 삭제 하는 것이 유용 합니다. 이렇게 하지 않으면 장치의 카메라를 액세스 하려고 하는 다른 응용 프로그램을 방해할 수 있습니다. 자세한 내용은 참조 [카메라 미리 보기 표시](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access)합니다.

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 제시 합니다.는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) 페이지에서 개발자가 자신의 플랫폼 관련 사용자 지정과 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. A `ContentPage` 단일 보기를 표시 하는 화면의 대부분을 차지 하는 시각적 요소입니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererContentPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
