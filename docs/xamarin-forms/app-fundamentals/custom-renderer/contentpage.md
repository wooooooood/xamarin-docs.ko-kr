---
title: ContentPage 사용자 지정
description: ContentPage를는 단일 보기를 표시 하 고 화면 대부분을 차지 하는 시각적 요소입니다. 이 문서에는 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다.
ms.prod: xamarin
ms.assetid: A4E61D93-73D9-4668-8D1C-DB6FC2491822
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 2369b249681b926476cf3938c51c99745eba9098
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995744"
---
# <a name="customizing-a-contentpage"></a>ContentPage 사용자 지정

_ContentPage를는 단일 보기를 표시 하 고 화면 대부분을 차지 하는 시각적 요소입니다. 이 문서에는 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 ContentPage 페이지에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 줍니다._

모든 Xamarin.Forms 컨트롤에 네이티브 컨트롤의 인스턴스를 만드는 각 플랫폼에 대 한는 함께 제공 되는 렌더러. 경우는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) iOS에서 Xamarin.Forms 응용 프로그램에서 렌더링 되는 `PageRenderer` 클래스가 인스턴스화되면 네이티브를 다시 인스턴스화하는 `UIViewController` 컨트롤입니다. Android 플랫폼에는 `PageRenderer` 클래스를 인스턴스화하는 `ViewGroup` 컨트롤입니다. Windows 플랫폼 (UWP (유니버설)을는 `PageRenderer` 클래스를 인스턴스화하는 `FrameworkElement` 제어 합니다. 렌더러 및 Xamarin.Forms 컨트롤에 매핑되는 네이티브 컨트롤 클래스에 대 한 자세한 내용은 참조 하세요. [렌더러 기본 클래스 및 네이티브 컨트롤](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md)합니다.

다음 다이어그램 간의 관계는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 및 구현 하는 해당 네이티브 컨트롤:

![](contentpage-images/contentpage-classes.png "ContentPage 클래스와 네이티브 컨트롤을 구현 간의 관계")

렌더링 프로세스를 수행할 수 활용에 대 한 사용자 지정 렌더러를 만들어 플랫폼별 사용자 지정을 구현 하는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 각 플랫폼에서 합니다. 이 수행 하는 프로세스는 다음과 같습니다.

1. [만들](#Creating_the_Xamarin.Forms_Page) Xamarin.Forms 페이지입니다.
1. [사용할](#Consuming_the_Xamarin.Forms_Page) Xamarin.Forms의 페이지입니다.
1. [만들](#Creating_the_Page_Renderer_on_each_Platform) 각 플랫폼의 경우 페이지에 대 한 사용자 지정 렌더러.

각 항목 이제 살펴봅니다를 구현 하는 `CameraPage` 피드 라이브 카메라와 사진 캡처 수를 제공 하는 합니다.

<a name="Creating_the_Xamarin.Forms_Page" />

## <a name="creating-the-xamarinforms-page"></a>Xamarin.Forms 페이지 만들기

변경 되지 않은 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 다음 XAML 코드 예제 에서처럼 공유 Xamarin.Forms 프로젝트에 추가할 수 있습니다.

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CustomRenderer.CameraPage">
    <ContentPage.Content>
    </ContentPage.Content>
</ContentPage>
```

마찬가지로,에 대 한 코드 숨김 파일을 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 다음 코드 예제에 표시 된 것으로 변경 되지 않은 경우에 유지 되어야 합니다.

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

다음 코드 예제에서는 C#에서 페이지를 만들 수 있습니다 하는 방법을 보여 줍니다.

```csharp
public class CameraPageCS : ContentPage
{
    public CameraPageCS ()
    {
    }
}
```

인스턴스는 `CameraPage` 라이브 카메라는 각 플랫폼에서 피드를 표시 하는 데 사용할 합니다. 컨트롤의 사용자 지정을 수행할 사용자 지정 렌더러에 추가 구현이 필요 하므로 `CameraPage` 클래스입니다.

<a name="Consuming_the_Xamarin.Forms_Page" />

## <a name="consuming-the-xamarinforms-page"></a>Xamarin.Forms 페이지를 사용합니다.

빈 `CameraPage` Xamarin.Forms 응용 프로그램에서 표시 되어야 합니다. 이 경우의 단추를 `MainPage` 인스턴스 탭을 차례로 실행 하는 `OnTakePhotoButtonClicked` 메서드를 다음 코드 예제에 표시 된 대로:

```csharp
async void OnTakePhotoButtonClicked (object sender, EventArgs e)
{
    await Navigation.PushAsync (new CameraPage ());
}
```

이 코드를 간단히 탐색을 `CameraPage`에 사용자 지정 렌더러는 각 플랫폼에서 페이지의 모양을 사용자 지정 됩니다.

<a name="Creating_the_Page_Renderer_on_each_Platform" />

## <a name="creating-the-page-renderer-on-each-platform"></a>각 플랫폼에서 페이지 렌더러를 만들고

사용자 지정 렌더러 클래스를 만드는 프로세스는 다음과 같습니다.

1. 서브 클래스를 만든를 `PageRenderer` 클래스입니다.
1. 재정의 `OnElementChanged` 네이티브 페이지 및 쓰기는 논리를 페이지 사용자 지정 렌더링 하는 메서드. `OnElementChanged` 메서드 해당 Xamarin.Forms 컨트롤을 만들 때 호출 됩니다.
1. 추가 `ExportRenderer` 특성을 사용 Xamarin.Forms 페이지를 렌더링 하는 않도록 지정 하려면 페이지 렌더러 클래스입니다. 이 특성은 Xamarin.Forms를 사용 하 여 사용자 지정 렌더러를 등록 하는 데 사용 됩니다.

> [!NOTE]
> 각 플랫폼 프로젝트에서 페이지 렌더러를 제공 하는 선택 사항입니다. 페이지 렌더러를 등록 하지 않은 경우 페이지에 대 한 기본 렌더러 사용 됩니다.

다음 다이어그램은 이들 간의 관계와 함께 샘플 응용 프로그램에서 각 프로젝트의 책임을 보여 줍니다.

![](contentpage-images/solution-structure.png "CameraPage 사용자 지정 렌더러 프로젝트 책임")

`CameraPage` 플랫폼별 인스턴스 렌더링 되 `CameraPageRenderer` 클래스에서 파생 되는 `PageRenderer` 해당 플랫폼에 대 한 클래스입니다. 이 인해 각 `CameraPage` 라이브 카메라 피드를 사용 하 여 렌더링 되 고 다음 스크린샷과에서 같이 인스턴스:

![](contentpage-images/screenshots.png "각 플랫폼에서 CameraPage")

합니다 `PageRenderer` 노출 클래스는 `OnElementChanged` 해당 네이티브 컨트롤을 렌더링 하는 Xamarin.Forms 페이지를 만들 때 호출 되는 메서드. 이 메서드는 `ElementChangedEventArgs` 포함 된 매개 변수 `OldElement` 및 `NewElement` 속성입니다. 이러한 속성은 Xamarin.Forms 요소를 나타냅니다는 렌더러 *되었습니다* 에 연결 및 Xamarin.Forms 요소는 렌더러 *는* 을 각각 연결 합니다. 샘플 응용 프로그램에서을 `OldElement` 속성이 `null` 와 `NewElement` 속성에 대 한 참조가 포함 됩니다는 `CameraPage` 인스턴스.

재정의 된 버전의 `OnElementChanged` 의 메서드를 `CameraPageRenderer` 클래스는 기본 페이지 사용자 지정을 수행 하는 위치입니다. 렌더링 되는 Xamarin.Forms 페이지 인스턴스에 대 한 참조를 통해 가져올 수는 `Element` 속성입니다.

으로 데코 레이트 된 각 사용자 지정 렌더러 클래스는 `ExportRenderer` Xamarin.Forms를 사용 하 여 렌더러를 등록 하는 특성입니다. 특성은 두 개의 매개 변수 – Xamarin.Forms 페이지를 렌더링할 형식 이름 및 사용자 지정 렌더러의 유형 이름을 사용 합니다. `assembly` 접두사 특성에 특성을 전체 어셈블리에 적용 되도록 지정 합니다.

다음 섹션에서는 구현에 설명 된 `CameraPageRenderer` 각 플랫폼에 대 한 사용자 지정 렌더러.

### <a name="creating-the-page-renderer-on-ios"></a>IOS에서 페이지 렌더러를 만들

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
                System.Diagnostics.Debug.WriteLine (@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데 iOS `UIViewController` 제어 합니다. 카메라 라이브 스트림은 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않습니다 및 사용자 지정 렌더러에 의해 렌더링 되는 페이지에 인스턴스가 제공한에 렌더링 됩니다.

일련의 메서드를 사용 하는 페이지를 사용자 지정 후의 `AVCapture` 라이브 스트림을 카메라에서 사진을 캡처하는 기능을 제공 하기 위한 Api입니다.

### <a name="creating-the-page-renderer-on-android"></a>Android에서 페이지 렌더러를 만들

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
                System.Diagnostics.Debug.WriteLine(@"            ERROR: ", ex.Message);
            }
        }
        ...
    }
}
```

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데 Android `ViewGroup` 컨트롤 보기의 그룹입니다. 카메라 라이브 스트림은 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않습니다 및 사용자 지정 렌더러에 의해 렌더링 되는 페이지에 인스턴스가 제공한에 렌더링 됩니다.

일련의를 사용 하는 메서드를 호출 하 여는 페이지를 사용자 지정한 다음를 `Camera` 전에 사진을 캡처하는 기능과 카메라에서 라이브 스트림을 제공 하기 위해 API를 `AddView` 메서드를 호출 하는 라이브 카메라 추가 UI를 스트림는 `ViewGroup`.

### <a name="creating-the-page-renderer-on-uwp"></a>UWP의 페이지 렌더러를 만들

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

기본 클래스에 대 한 호출 `OnElementChanged` 메서드를 만드는 데는 `FrameworkElement` 컨트롤, 페이지를 렌더링 합니다. 카메라 라이브 스트림은 렌더러에서 기존 Xamarin.Forms 요소에 이미 연결 되지 않습니다 및 사용자 지정 렌더러에 의해 렌더링 되는 페이지에 인스턴스가 제공한에 렌더링 됩니다. 일련의를 사용 하는 메서드를 호출 하 여는 페이지를 사용자 지정한 다음 합니다 `MediaCapture` 사용자 지정된 된 페이지에 추가 되기 전에 사진을 캡처하는 기능과 카메라에서 라이브 스트림을 제공 하기 위해 API를 `Children` 표시에 대 한 컬렉션입니다.

파생 되는 사용자 지정 렌더러를 구현 하는 경우 `PageRenderer` UWP에서는 `ArrangeOverride` 메서드 또한 때문에 구현 해야 페이지 컨트롤을 정렬 하려면 기본 렌더러도 수행할 작업을 알 수 없습니다. 그렇지 않은 경우 빈 페이지가 발생합니다. 따라서이 예제의 `ArrangeOverride` 메서드 호출을 `Arrange` 메서드를 `Page` 인스턴스.

> [!NOTE]
> 중지 하 고 UWP 응용 프로그램에서 카메라에 대 한 액세스를 제공 하는 개체를 삭제 하는 것이 반드시 합니다. 이렇게 하지 않으면 장치의 카메라에 액세스 하려고 하는 다른 응용 프로그램을 방해할 수 있습니다. 자세한 내용은 [카메라 미리 보기를 표시](https://msdn.microsoft.com/windows/uwp/audio-video-camera/simple-camera-preview-access)합니다.

## <a name="summary"></a>요약

이 문서에 대 한 사용자 지정 렌더러를 만드는 방법을 보여 주었습니다 합니다 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) 페이지에서 개발자가 자신의 플랫폼 특정 사용자 지정을 사용 하 여 기본 네이티브 렌더링을 재정의할 수 있도록 합니다. `ContentPage` 는 단일 보기를 표시 하 고 화면 대부분을 차지 하는 시각적 요소입니다.


## <a name="related-links"></a>관련 링크

- [CustomRendererContentPage (샘플)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/contentpage/)
