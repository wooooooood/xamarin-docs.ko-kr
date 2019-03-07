---
title: Xamarin.Forms의 이미지
description: Xamarin.Forms 사용 하 여 플랫폼 이미지를 공유할 수 있습니다 하 고, 각 플랫폼에 대해 구체적으로 로드 될 수 있습니다 또는 표시를 위해 다운로드할 수 있습니다.
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1a08803930eaaa3c2c5c5f8b8aa9561a9a7b8d88
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557245"
---
# <a name="images-in-xamarinforms"></a>Xamarin.Forms의 이미지

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)

_Xamarin.Forms 사용 하 여 플랫폼 이미지를 공유할 수 있습니다 하 고, 각 플랫폼에 대해 구체적으로 로드 될 수 있습니다 또는 표시를 위해 다운로드할 수 있습니다._

이미지는 응용 프로그램 탐색, 유용성 및 브랜딩의 중요 한 부분입니다. Xamarin.Forms 응용 프로그램 수는 각 플랫폼에서 다양 한 이미지를 표시할 수도 있지만 모든 플랫폼 이미지를 공유 해야 합니다.

플랫폼 특정 이미지도 아이콘 및 시작 화면; 필요 이러한 플랫폼별으로 구성 해야 합니다.

## <a name="displaying-images"></a>이미지 표시

Xamarin.Forms를 사용 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 페이지에 이미지를 표시 하려면 보기. 두 가지 중요 한 속성을 가집니다.

- [`Source`](xref:Xamarin.Forms.Image.Source) -An [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) 인스턴스, 파일, Uri 또는 리소스에 표시할 이미지를 설정 합니다.
- [`Aspect`](xref:Xamarin.Forms.Image.Aspect) -(것인지 stretch, 자르기 또는 레터 박스) 내에 표시 되는 범위 내에 있는 이미지의 크기를 조정 하는 방법입니다.

[`ImageSource`](xref:Xamarin.Forms.ImageSource) 이미지 원본의 각 유형에 대 한 정적 메서드를 사용 하 여 인스턴스를 가져올 수 있습니다.

- [`FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) -파일 이름 또는 파일 경로 각 플랫폼에서 확인할 수 있는 필요 합니다.
- [`FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) -예를 들어 Uri 개체를 필요합니다.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) -리소스 식별자를 사용 하 여 응용 프로그램 또는.NET Standard 라이브러리 프로젝트에 포함 된 이미지 파일을 필요는 **빌드 작업: EmbeddedResource**합니다.
- [`FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) -이미지 데이터를 제공 하는 스트림이 필요 합니다.

합니다 [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) 속성 표시 영역에 맞게 이미지를 조정 하는 방법을 결정 합니다.

- [`Fill`](xref:Xamarin.Forms.Aspect.Fill) -이미지를 완전히 정확 하 게 표시 영역을 채우도록 확장 됩니다. 이 이미지 왜곡 되지 될 수 있습니다.
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill) -을 측면을 유지 하면서 표시 영역을 채우도록 이미지를 자릅니다 (ie. 왜곡).
- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit) -레터 박스 이미지 (필요한 경우) 여부에 따라 양쪽 위쪽/아래쪽에 추가 공백이 있는 가로 또는 세로 이미지는 전체 이미지에 표시 영역에 적합 한 수 있도록 합니다.

이미지를 로드할 수는 [로컬 파일](#Local_Images), [포함 리소스](#embedded-images), 또는 [다운로드](#Downloading_Images)합니다. 또한 글꼴 아이콘 표시할 수 있습니다 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 글꼴 아이콘 데이터를 지정 하 여 보기를 `FontImageSource` 개체입니다. 자세한 내용은 [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) 에 [글꼴](~/xamarin-forms/user-interface/text/fonts.md) 가이드입니다.

## <a name="local-images"></a>로컬 이미지

이미지 파일 각 응용 프로그램 프로젝트에 추가 하 고 Xamarin.Forms 공유 코드에서 참조할 수 있습니다. 이미지를 배포 하는이 메서드는 다양 한 플랫폼을 약간 다른 디자인에 다른 해상도 사용 하는 경우 이미지와 같은 플랫폼 관련을 경우 필요 합니다.

모든 앱에서 단일 이미지를 사용 하 *모든 플랫폼에서 같은 파일 이름을 사용 해야*, 올바른 Android 리소스 이름을 지정 해야 (ie. 소문자, 숫자, 밑줄 및 마침표 수)입니다.

- **iOS** -기본 방식으로 관리 하 고 iOS 9를 사용 하는 것 이므로 이미지를 지원할 **자산 카탈로그 이미지 집합**, 다양 한 장치를 지원 하 고에 대 한 요소를 확장 하는 데 필요한 이미지의 버전 모두를 포함 해야 하는 응용 프로그램입니다. 자세한 내용은 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.
- **Android** -이미지를 배치 합니다 **리소스/drawable** 디렉터리 **빌드 작업: AndroidResource**. 이미지의 높은 DPI 및 낮은 버전을 제공할 수 있습니다 (적절 하 게 이름이 **리소스** 같은 하위 디렉터리 **drawable ldpi**에 **drawable hdpi**, 및 **drawable xhdpi**).
- **유니버설 Windows 플랫폼 (UWP)** -이미지를 사용 하 여 응용 프로그램의 루트 디렉터리에 배치 **빌드 작업: 콘텐츠**합니다.

> [!IMPORTANT]
> IOS 9 이전 이미지 일반적으로 배치 된 합니다 **리소스** 폴더 **빌드 작업: BundleResource**. 그러나이 메서드는 iOS 앱에서 이미지를 사용 하는 Apple에서 되지 합니다. 자세한 내용은 [이미지 크기 및 파일 이름](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

파일 이름 지정 및 배치에 대 한 이러한 규칙을 준수 로드 하 고 모든 플랫폼에서 이미지를 표시 하려면 다음 XAML을 허용 합니다.

```xaml
<Image Source="waterfront.jpg" />
```

에 해당 하는 C# 코드는 다음과 같습니다.

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

다음 스크린샷에서 각 플랫폼에서 로컬 이미지를 표시 하는 결과 보여 줍니다.

[![로컬 ImageSource](images-images/local-sml.png "로컬 이미지를 표시 하는 응용 프로그램 예제")](images-images/local.png#lightbox "로컬 이미지를 표시 하는 응용 프로그램 샘플")

보다 유연 하 게 여 `Device.RuntimePlatform` 속성 사용할 수 있습니다 다른 이미지 파일 또는 일부 또는 모든 플랫폼에 대 한 경로 선택 하려면이 코드 예제 에서처럼:

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> 모든 플랫폼에서 동일한 이미지 파일 이름을 사용 하 여 이름을 모든 플랫폼에서 유효 해야 합니다. Android 드로어 블 명명 제한 – 소문자, 숫자, 밑줄 및 마침표 수 – 있고 플랫폼 간 호환성을 위해이 야 합니다. 다른 모든 플랫폼에서 너무 합니다. 파일 이름 예 **waterfront.png** 규칙은 다음과 같이 있지만 잘못 된 파일 이름의 예입니다 "WaterFront.png", "water front.png" 포함 "water-front.png" 및 "wåterfront.png"입니다.

### <a name="native-resolutions-retina-and-high-dpi"></a>기본 해상도 (Retina 및 높은 DPI)

iOS, Android 및 UWP 다른 이미지 해상도, 운영 체제에서 장치의 기능에 따라 런타임에 적절 한 이미지를 선택 하는 위치에 대 한 지원이 포함 됩니다. Xamarin.Forms 기본 플랫폼의 Api를 사용 하 여 파일이 올바르게 명명 되 고 프로젝트에 있는 경우 자동으로 대체 해상도 지원 하므로 로컬 이미지를 로드 합니다.

IOS 9를 적절 한 자산 카탈로그 이미지 집합에 필요한 각 해상도 대 한 이미지를 끌기 이므로 이미지를 관리 하는 기본 방법입니다. 자세한 내용은 [자산 카탈로그 이미지 집합에 이미지 추가](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

IOS 9 이전 이미지의 레 티 나 버전에 놓일 수 있습니다는 **리소스** 폴더 2와 3 번 사용 하 여 확인을 **@2x** 하거나 **@3x**파일 이름 (예: 파일 확장명 앞에 접미사 **myimage@2x.png**). 그러나이 메서드는 iOS 앱에서 이미지를 사용 하는 Apple에서 되지 합니다. 자세한 내용은 [이미지 크기 및 파일 이름](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

Android 대체 고해상도 이미지를 배치할 [라는 특수 디렉터리](http://developer.android.com/guide/practices/screens_support.html) 다음 스크린샷에 표시 된 것 처럼 Android 프로젝트에서:

[![Android 다중 해상도 이미지 위치](images-images/xs-highdpisolution-sml.png "Android 다중 해상도 이미지 위치")](images-images/xs-highdpisolution.png#lightbox "Android 다중 해상도 이미지 위치")

UWP 이미지 파일 이름을 [붙어야 수 `.scale-xxx` 파일 확장명 앞](https://docs.microsoft.com/windows/uwp/app-resources/images-tailored-for-scale-theme-contrast)여기서 `xxx` 예를 들어 자산에 적용 하는 크기 조정 비율 **myimage.scale 200.png**합니다. 이미지를 코드에 배율 한정자 없이 XAML 예를 들어 참조할 수 있습니다 **myimage.png**합니다. 플랫폼에서는 디스플레이 현재 DPI를 기준으로 가장 가까운 적절 한 자산 눈금자를 선택 합니다.

### <a name="additional-controls-that-display-images"></a>이미지를 표시 하는 추가 컨트롤

일부 컨트롤에 같은 이미지를 표시 하는 속성이 있습니다.

- [`Page`](xref:Xamarin.Forms.Page) -형식에서 파생 되는 페이지 any `Page` 가 [ `Icon` ](xref:Xamarin.Forms.Page.Icon) 및 [ `BackgroundImage` ](xref:Xamarin.Forms.Page.BackgroundImage) 속성을 로컬 파일 참조를 할당할 수 있습니다. 때와 같이 특정 상황을 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 표시 되는 [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), 플랫폼에서 지원 되는 경우 아이콘이 표시 됩니다.

  > [!IMPORTANT]
  > Ios의 경우는 [ `Page.Icon` ](xref:Xamarin.Forms.Page.Icon) 이미지 자산 카탈로그 이미지 집합에서 속성을 채울 수 없습니다. 대신에 대 한 아이콘 이미지를 로드 합니다 `Page.Icon` 속성을 **리소스** iOS 프로젝트의 폴더.

- [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem) -는 [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) 로컬 파일 참조로 설정할 수 있는 속성입니다.
- [`ImageCell`](xref:Xamarin.Forms.ImageCell) -는 [ `ImageSource` ](xref:Xamarin.Forms.ImageCell.ImageSource) 속성 이미지를 설정할 수 있는 로컬 파일, 포함된 된 리소스 또는 URI에서 검색 합니다.

## <a name="embedded-images"></a>포함 이미지

포함된 이미지는도 응용 프로그램 (예: 로컬 이미지)와 함께 제공 하지만 파일 리소스 어셈블리에 포함 된 각 응용 프로그램의 파일 구조 이미지에서에서 이미지의 복사본을 대신 합니다. 이미지를 배포 하는이 메서드는 각 플랫폼에서 동일한 이미지를 사용할 때 것이 좋습니다 하며 이미지 코드를 함께 제공 되는 대로 구성 요소를 만드는 데 특히 적합입니다.

이미지를 프로젝트에 포함 하려면 마우스 오른쪽 단추를 새 항목을 추가 하 고 추가 하려는 이미지/s를 선택 합니다. 기본적으로 이미지 해야 **빌드 작업: None**;로 설정 해야이 **빌드 작업: EmbeddedResource**를 확인합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](images-images/vs-buildaction.png "빌드 작업을 설정 합니다. EmbeddedResource")

**빌드 작업** 보기 및 변경 합니다 **속성** 파일 창.

리소스 ID는이 예제의 **WorkingWithImages.beach.jpg**합니다.
IDE에 연결 하 여이 기본값을 생성 합니다 **기본 Namespace** 파일 이름으로이 프로젝트에 대 한 각 값 사이 마침표 (.)를 사용 하 여 합니다.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](images-images/xs-buildaction.png "빌드 작업을 설정 합니다. EmbeddedResource")

**빌드 작업** 도 확인 및 변경할 수는 **속성** 파일에 대 한 채움 합니다.
이 패드 표시 합니다 **리소스 ID** 코드에서 리소스를 참조 하는 데 사용 되는 합니다. 아래 스크린샷에 **리소스 ID** 됩니다 **WorkingWithImages.beach.jpg**합니다.
IDE에 연결 하 여이 기본값을 생성 합니다 **기본 Namespace** 파일 이름으로이 프로젝트에 대 한 각 값 사이 마침표 (.)를 사용 하 여 합니다.
이 ID를 편집할 수 있습니다 합니다 **속성** 패드 하지만 이러한 예제에 대 한 값 **WorkingWithImages.beach.jpg** 사용 됩니다.

![](images-images/xs-embeddedproperties.png "EmbeddedResource Properties Pad")

-----

프로젝트 내에서 폴더에 포함 된 이미지를 배치 하는 경우 폴더 이름은 또한 마침표로 구분 하 여 (.)의 리소스 id입니다. 이동 합니다 **beach.jpg** 라는 폴더에 이미지 **MyImages** 의 리소스 ID를 초래 **WorkingWithImages.MyImages.beach.jpg**

포함 된 이미지를 로드 하는 코드는 단순히 전달 된 **리소스 ID** 에 [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) 아래와 같이 메서드:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg", typeof(EmbeddedImages).GetTypeInfo().Assembly) };
```

> [!NOTE]
> 를 지원 하기 위해 유니버설 Windows 플랫폼에서 릴리스 모드에서 포함 된 이미지를 표시 해야 하는 오버 로드를 사용 하 여 `ImageSource.FromResource` 이미지에 대 한 검색 하는 소스 어셈블리를 지정 하는 합니다.

현재 리소스 식별자에 대 한 암시적 변환이 있습니다. 대신 사용 해야 합니다 [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource*) 또는 `new ResourceImageSource()` 포함 된 이미지를 로드 합니다.

다음 스크린샷은 각 플랫폼에 포함 된 이미지를 표시 하는 결과 보여 줍니다.

[![ResourceImageSource](images-images/resource-sml.png "포함 된 이미지를 표시 하는 응용 프로그램 예제")](images-images/resource.png#lightbox "포함 된 이미지를 표시 하는 응용 프로그램 샘플")

### <a name="using-xaml"></a>XAML을 사용 하 여

기본 제공 형식 변환기가 있으므로 `string` 에 `ResourceImageSource`, XAML에서 이러한 유형의 이미지를 고유 하 게 로드할 수 없습니다. 대신 단순 사용자 지정 XAML 태그 확장을 쓸 수 있습니다 사용 하 여 이미지를 로드를 **리소스 ID** XAML에 지정 합니다.

```csharp
[ContentProperty (nameof(Source))]
public class ImageResourceExtension : IMarkupExtension
{
 public string Source { get; set; }

 public object ProvideValue (IServiceProvider serviceProvider)
 {
   if (Source == null)
   {
     return null;
   }

   // Do your translation lookup here, using whatever method you require
   var imageSource = ImageSource.FromResource(Source, typeof(ImageResourceExtension).GetTypeInfo().Assembly);

   return imageSource;
 }
}
```

> [!NOTE]
> 를 지원 하기 위해 유니버설 Windows 플랫폼에서 릴리스 모드에서 포함 된 이미지를 표시 해야 하는 오버 로드를 사용 하 여 `ImageSource.FromResource` 이미지에 대 한 검색 하는 소스 어셈블리를 지정 하는 합니다.

이 확장을 사용 하려면 사용자 지정 추가 `xmlns` 는 XAML을 프로젝트에 대 한 올바른 네임 스페이스와 어셈블리 값을 사용 하 합니다. 이미지 원본 설정할 수 있습니다이 구문을 사용 하 여: `{local:ImageResource WorkingWithImages.beach.jpg}`합니다. 전체 XAML 예제는 다음과 같습니다.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage
   xmlns="http://xamarin.com/schemas/2014/forms"
   xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
   xmlns:local="clr-namespace:WorkingWithImages;assembly=WorkingWithImages"
   x:Class="WorkingWithImages.EmbeddedImagesXaml">
 <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
   <!-- use a custom Markup Extension -->
   <Image Source="{local:ImageResource WorkingWithImages.beach.jpg}" />
 </StackLayout>
</ContentPage>
```

### <a name="troubleshooting-embedded-images"></a>포함된 이미지 문제 해결

#### <a name="debugging-code"></a>코드 디버깅

특정 이미지 리소스 로드 되지 이유를 이해 하기 어려운 경우가 있기 때문에 리소스를 올바르게 구성 되었는지 확인 하는 데 응용 프로그램에 일시적으로 다음 디버그 코드를 추가할 수 있습니다. 지정된 된 어셈블리에 포함 된 알려진된 모든 리소스를 출력 합니다 <span class="UIItem">콘솔</span> 리소스 로드 문제를 디버깅 하는 데 있습니다.

```csharp
using System.Reflection;
// ...
// NOTE: use for debugging, not in released app code!
var assembly = typeof(EmbeddedImages).GetTypeInfo().Assembly;
foreach (var res in assembly.GetManifestResourceNames())
{
    System.Diagnostics.Debug.WriteLine("found resource: " + res);
}
```

#### <a name="images-embedded-in-other-projects"></a>다른 프로젝트에 포함 된 이미지

기본적으로 `ImageSource.FromResource` 메서드 호출을 하는 코드와 동일한 어셈블리에서 이미지 검색은 `ImageSource.FromResource` 메서드. 위의 디버그 코드를 사용 하는 어셈블리를 변경 하 여 특정 리소스를 포함할 확인할 수 있습니다는 `typeof()` 문을 `Type` 각 어셈블리에 있는 것으로 알려져 있습니다.

그러나 인수로 포함된 이미지를 검색 하는 소스 어셈블리를 지정할 수 있습니다는 `ImageSource.FromResource` 메서드:

```csharp
var imageSource = ImageSource.FromResource("filename.png", typeof(MyClass).GetTypeInfo().Assembly);
```

## <a name="downloading-images"></a>이미지를 다운로드합니다.

다음 XAML과 같이 이미지 표시에 대 한 자동으로 다운로드 수 있습니다.

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
       x:Class="WorkingWithImages.DownloadImagesXaml">
  <StackLayout VerticalOptions="Center" HorizontalOptions="Center">
    <Label Text="Image UriSource Xaml" />
    <Image Source="https://xamarin.com/content/images/pages/forms/example-app.png" />
    <Label Text="example-app.png gets downloaded from xamarin.com" />
  </StackLayout>
</ContentPage>
```

에 해당 하는 C# 코드는 다음과 같습니다.

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

합니다 [ `ImageSource.FromUri` ](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) 메서드를 사용 하려면를 `Uri` 개체를 반환 된 새 [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) 에서 읽어오는 `Uri`합니다.

또한는 암시적 변환이 URI 문자열의 경우 다음 예제에서는 에서도 작동 하므로:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

다음 스크린샷에서 각 플랫폼에서 원격 이미지를 표시 하는 결과 보여 줍니다.

[![다운로드 ImageSource](images-images/download-sml.png "샘플을 다운로드 한 이미지를 표시 하는 응용 프로그램")](images-images/download.png#lightbox "샘플 응용 프로그램을 다운로드 한 이미지를 표시 합니다.")

### <a name="downloaded-image-caching"></a>다운로드 한 이미지 캐싱

A [ `UriImageSource` ](xref:Xamarin.Forms.UriImageSource) 는 또한 다음 속성을 통해 구성 하는 다운로드 한 이미지의 캐싱을 지원 합니다.

- [`CachingEnabled`](xref:Xamarin.Forms.UriImageSource.CachingEnabled) -캐싱 사용 여부 (`true` 기본적으로).
- [`CacheValidity`](xref:Xamarin.Forms.UriImageSource.CacheValidity) - `TimeSpan` 는 이미지를 로컬로 저장할 수는 기간을 정의 합니다.

캐싱 기본적으로 활성화 되 고 24 시간 동안 로컬 이미지를 저장 합니다. 특정 이미지에 대 한 캐싱을 사용 하지 않으려면 이미지 소스를 다음과 같이 인스턴스화하십시오.

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

특정 캐시 기간 (예: 5 일)을 설정 하려면 이미지 소스를 다음과 같이 인스턴스화하십시오.

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

기본 제공 캐싱 매우 쉽게 하 고 셀 보기로 다시 스크롤될 때 이미지를 다시 로드 하는 주의 하는 기본 제공 캐시, 각 셀에 이미지를 하거나 수 있는 설정 (바인딩) 이미지 목록을 스크롤 하는 등의 시나리오를 지원 합니다.

## <a name="icons-and-splash-screens"></a>아이콘 및 시작 화면

관련이 없는 동안 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 보기, 응용 프로그램 아이콘 및 시작 화면 이미지 Xamarin.Forms 프로젝트에서 사용 하면 중요 한도입니다.

설정 아이콘 및 시작 화면 Xamarin.Forms 앱에 대 한 각 응용 프로그램 프로젝트에서 수행 됩니다. 즉, iOS, Android 및 UWP에 대 한 이미지 크기 올바르게 생성 합니다. 이러한 이미지 이름이 있고 각 플랫폼의 요구 사항에 따라 배치 합니다.

## <a name="icons"></a>아이콘

참조를 [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md)를 [Google의도 해](http://developer.android.com/design/style/iconography.html), 및 [타일 및 아이콘 자산에 대 한 지침](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) 이러한 응용 프로그램 리소스를 만드는 대 한 자세한 내용은 합니다.

또한 글꼴 아이콘 표시할 수 있습니다 합니다 [ `Image` ](xref:Xamarin.Forms.Image) 글꼴 아이콘 데이터를 지정 하 여 보기를 `FontImageSource` 개체입니다. 자세한 내용은 [글꼴 아이콘 표시](~/xamarin-forms/user-interface/text/fonts.md#display-font-icons) 에 [글꼴](~/xamarin-forms/user-interface/text/fonts.md) 가이드입니다.

## <a name="splash-screens"></a>시작 화면

IOS 및 UWP 응용 프로그램 (시작 화면 또는 기본 이미지 라고도 함)를 시작 화면이 필요 합니다.

에 대 한 설명서를 참조 하세요 [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 하 고 [시작 화면](/windows/uwp/launch-resume/splash-screens/) Windows 개발자 센터에서.

## <a name="summary"></a>요약

Xamarin.Forms는 플랫폼 간 응용 프로그램에서 동일한 플랫폼에서 사용할 이미지 또는 플랫폼 특정 이미지를 지정할 수 있도록 이미지를 포함 하는 다양 한 방법의 수를 제공 합니다. 다운로드 한 이미지도 자동으로 캐시는 일반적인 코딩 시나리오를 자동화 합니다.

응용 프로그램 아이콘 및 시작 화면 이미지 설정 되며, 플랫폼별 앱에 사용 되는 동일한 지침에 따라 비 Xamarin.Forms 응용 프로그램-구성 합니다.

## <a name="related-links"></a>관련 링크

- [WorkingWithImages (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md)
- [Android의 해](http://developer.android.com/design/style/iconography.html)
- [타일 및 아이콘 자산에 대 한 지침](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
