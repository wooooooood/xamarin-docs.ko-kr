---
title: "이미지"
description: "Xamarin.forms는 플랫폼에서 이미지를 공유할 수, 각 플랫폼에 대해 구체적으로 로드 될 수 또는 표시를 위해 다운로드할 수 있습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: C025AB53-05CC-49BA-9815-75D6DF9E40B7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 440ee997b075b5c89504dcf20171fa3c8713e1ce
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="images"></a>이미지

_Xamarin.forms는 플랫폼에서 이미지를 공유할 수, 각 플랫폼에 대해 구체적으로 로드 될 수 또는 표시를 위해 다운로드할 수 있습니다._

이미지는 응용 프로그램 탐색, 효용성 및 브랜딩의 중요 한 부분입니다. Xamarin.Forms 응용 프로그램을 모든 플랫폼 이미지를 공유 하지만 각 플랫폼에도 잠재적으로 서로 다른 이미지를 표시 해야 합니다.

플랫폼별 이미지 아이콘 및 시작 화면;에 필요한 수도 있습니다. 이러한 플랫폼 기준으로 구성 해야 합니다.

이 문서에서는 다음 내용을 다룹니다.

- [ **로컬 이미지** ](#Local_Images) -iOS는 이미지의 높은 DPI 버전 레 티 나 또는 Android와 같은 네이티브 해상도 확인을 포함 하 여 응용 프로그램을 함께 제공 된 이미지를 표시 합니다.
- [ **포함 이미지** ](#Embedded_Images) -어셈블리 리소스로 포함 된 이미지를 표시 합니다.
- [ **이미지를 다운로드 한** ](#Downloading_Images) -다운로드 하 고 이미지를 표시 합니다.
- [ **아이콘 및 여 시작 화면** ](#Icons_and_splashscreens) -플랫폼별 아이콘 및 시작 시 이미지입니다.

## <a name="displaying-images"></a>이미지 표시

Xamarin.Forms는는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 페이지에 이미지를 표시 하려면 보기. 여기에 두 가지 중요 한 속성이 있습니다.

- [`Source`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) -An [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) 인스턴스, 파일, Uri 또는 리소스를 표시 하려면 이미지를 설정 합니다.
- [`Aspect`](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) -방법 (것인지 스트레치, 자르기 또는 letterbox) 내에서 표시 되는 범위 내에서 이미지 크기를 조정 합니다.

[`ImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/) 이미지 원본의 각 유형에 대 한 정적 메서드를 사용 하 여 인스턴스를 가져올 수 있습니다.

- [`FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) -파일 이름 또는 파일 경로 각 플랫폼에서 확인 될 수 있는 필요 합니다.
- [`FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) -예: Uri 개체가 필요 합니다.  `new Uri("http://server.com/image.jpg")` .
- [`FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) -응용 프로그램이 나 PCL에 포함 된 이미지 파일에 리소스 식별자 필요는 **빌드 동작: 포함 리소스**합니다.
- [`FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) -이미지 데이터를 제공 하는 스트림이 필요 합니다.

[ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) 속성 이미지는 표시 영역에 맞게 크기가 조정 방법을 결정 합니다.

- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/) -를 완전히 정확 하 게 표시 영역을 채우도록 이미지를 맞춥니다. 이 인해 왜곡 되지 이미지 될 수 있습니다.
- [`AspectFill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFill/) -을 측면을 유지 하면서 표시 영역을 채우도록 이미지를 자릅니다 (ie. 왜곡).
- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/) -Letterboxes (필요한 경우) 이미지 위쪽/아래쪽 또는 여부에 따라 양쪽에 추가 공백이 있는 가로 또는 세로 이미지는에 전체 이미지 표시 영역 맞도록, 합니다.

이미지를 로드할 수 있습니다는 [로컬 파일](#Local_Images_in_Xaml), [포함 리소스](#embedded_images), 또는 [다운로드](#Downloading_Images)합니다.

<a name="Local_Images" />

## <a name="local-images"></a>로컬 이미지

이미지 파일 각 응용 프로그램 프로젝트에 추가 하 고 Xamarin.Forms 공유 코드에서 참조할 수 있습니다. 모든 앱에 대해 단일 이미지를 사용 하 *모든 플랫폼에 같은 파일 이름을 사용 해야*, 올바른 Android 리소스 이름을 지정 해야 (즉. 소문자, 숫자, 밑줄, 및 기간에 허용 됩니다).

- **iOS** -으로 관리 하 고 이미지를 사용 하는 iOS 9 지원 기본 **자산 카탈로그 이미지 집합**는 모든 다양 한 장치를 지원 하 고에 대 한 요소를 확장 하는 데 필요한 이미지의 버전을 포함 해야 하는 프로그램 응용 프로그램입니다. 자세한 내용은 참조 [는 자산 카탈로그 이미지를 설정 하는 추가 이미지](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.
- **Android** -에 이미지를 배치는 **리소스/그릴** ड ि र ॅ **빌드 작업: AndroidResource**합니다. 이미지의 높은 및 낮은 DPI 버전을 제공할 수 있습니다 (적절 하 게 이름이 **리소스** 와 같은 하위 디렉터리 **그릴 ldpi**, **그릴 hdpi**, 및 **그릴 xhdpi**).
- **Windows Phone** -응용 프로그램의 루트 디렉터리에 이미지를 배치 **빌드 작업: 콘텐츠**합니다.
- **유니버설 Windows 플랫폼 (UWP)** -응용 프로그램의 루트 디렉터리에 이미지를 배치 **빌드 작업: 콘텐츠**합니다.

> [!IMPORTANT]
> IOS 9 이전 이미지 일반적으로 배치 된는 **리소스** 폴더 **빌드 작업: BundleResource**합니다. 그러나이 방법은 iOS 앱의 이미지 작업에 사용 되지 Apple에서. 자세한 내용은 참조 [이미지 크기와 파일 이름을](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

파일 이름 지정 및 배치에 대 한 이러한 규칙을 따르는 다음 XAML을 로드 하 고 모든 플랫폼에서 이미지를 표시할 수 있습니다.

```xaml
<Image Source="waterfront.jpg" />
```

해당 하는 C# 코드는 다음과 같습니다.

```csharp
var image = new Image { Source = "waterfront.jpg" };
```

다음 스크린샷에서 각 플랫폼에서 로컬 이미지를 표시 하는 결과 보여 줍니다.

[![로컬 ImageSource](images-images/local-sml.png "샘플 로컬 이미지를 표시 하는 응용 프로그램")](images-images/local.png#lightbox "샘플 로컬 이미지를 표시 하는 응용 프로그램")

유연성을 높이 `Device.RuntimePlatform` 이 코드 예제와 같이 다른 이미지 파일 또는 일부 또는 모든 플랫폼에 대 한 경로 선택 하 속성을 사용할 수 있습니다.

```csharp
image.Source = Device.RuntimePlatform == Device.Android ? ImageSource.FromFile("waterfront.jpg") : ImageSource.FromFile("Images/waterfront.jpg");
```

> [!IMPORTANT]
> 모든 플랫폼에 대해 같은 이미지 파일 이름을 사용 하는 이름을 모든 플랫폼에서 유효 해야 합니다. Android drawables는 명명 제한 – 소문자, 숫자, 밑줄 및 기간 – 허용 하는 플랫폼 간 호환성에 대 한이 뒤에 야 다른 플랫폼에서 너무 합니다. 파일 이름 예 **waterfront.png** 잘못 된 파일의 예제만 규칙을 다음과 "물 front.png", "WaterFront.png" 포함 "물-front.png" 및 "wåterfront.png"입니다.

<a name="Native_Resolutions" />

### <a name="native-resolutions-retina-and-high-dpi"></a>기본 해상도 (레 티 나 및 높은 DPI)

IOS 및 Android 플랫폼 둘 다에 다른 이미지 해상도, 운영 체제에서 장치의 기능에 따라 런타임에 적절 한 이미지를 선택 하는 위치에 대 한 지원이 포함 됩니다. Xamarin.Forms는 파일은 올바르게 라는 이며 프로젝트에 있는 경우 자동으로 대체 해상도 지원 하므로 로컬 이미지를 로드 하기 위한 네이티브 플랫폼의 Api를 사용 합니다.

적절 한 자산 카탈로그 이미지 세트에 필요한 각 해상도 대 한 이미지를 끌기를 iOS 9 이후 이미지를 관리 하는 것이 좋습니다. 자세한 내용은 참조 [는 자산 카탈로그 이미지를 설정 하는 추가 이미지](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

IOS 9 이전 이미지의 레 티 나 버전에 놓일 수 있습니다는 **리소스** 폴더-2와 3 해상도와 제한 시간이 한  **@2x**  또는  **@3x** 않음 (예: 파일 확장명 앞 파일 이름에 접미사 **myimage@2x.png**). 그러나이 방법은 iOS 앱의 이미지 작업에 사용 되지 Apple에서. 자세한 내용은 참조 [이미지 크기와 파일 이름을](~/ios/app-fundamentals/images-icons/displaying-an-image.md)합니다.

Android 대체 고해상도 이미지를 배치할 [특별 하 게 명명 된 디렉터리](http://developer.android.com/guide/practices/screens_support.html) 다음 스크린샷에 표시 된 대로 Android 프로젝트:

[![Android 다중 해상도 이미지 위치](images-images/xs-highdpisolution-sml.png "Android 다중 해상도 이미지 위치")](images-images/xs-highdpisolution.png#lightbox "Android 다중 해상도 이미지 위치")

### <a name="additional-controls-that-display-images"></a>이미지를 표시 하는 추가 컨트롤

일부 컨트롤은와 같은 이미지를 표시 하는 속성을 포함 합니다.

- [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) -모든 페이지에서 파생 된 형식 `Page` 가 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) 및 [ `BackgroundImage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.BackgroundImage/) 속성 로컬 파일 참조를 지정할 수 있습니다. 예를 들어 특정 상황에서는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 표시 되는 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), 플랫폼에서 지원 되는 경우 아이콘이 표시 됩니다.

  > [!IMPORTANT]
  > IOS의 경우에 [ `Page.Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Icon/) 자산 카탈로그 이미지 집합의 이미지에서 속성을 채울 수 없습니다. 에 대 한 아이콘 이미지를 로드 하는 대신,는 `Page.Icon` 속성에서는 **리소스** iOS 프로젝트의 폴더에에서 있습니다.

- [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) -는 [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) 로컬 파일 참조로 설정할 수 있는 속성입니다.
- [`ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/) -는 [ `ImageSource` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ImageCell.ImageSource/) 이미지에 설정할 수 있는 속성은 로컬 파일, 포함된 된 리소스 또는 URI에서 검색 합니다.

<a name="embedded_images" />

## <a name="embedded-images"></a>포함 이미지

포함 된 이미지는도 응용 프로그램 (예: 로컬 이미지)와 함께 제공 하지만 각 응용 프로그램의 파일 구조 이미지에에서는 이미지의 복사본을 대신 파일은 리소스로 어셈블리에 포함 됩니다. 이미지를 배포 하는이 메서드는 이미지는 코드와 함께 제공 되는 대로 구성 요소를 만드는 데 특히 적합 합니다.

프로젝트에 이미지를 포함 하려면 마우스 오른쪽 단추를 새 항목을 추가 하 고 추가 하려는 이미지/s를 선택 합니다. 기본적으로 이미지 적용 됩니다 **빌드 작업: None**;로 설정 해야이 **빌드 작업: 포함 리소스**합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](images-images/vs-buildaction.png "빌드 동작 설정: 포함 리소스")

**빌드 작업** 확인 및 변경할 수는 **속성** 파일에 대 한 창.

이 예의 경우 리소스 ID는 **WorkingWithImages.beach.jpg**합니다.
IDE 연결 하 여이 기본값에서 생성 된 **기본 Namespace** 파일 이름으로이 프로젝트에 대 한 각 값 사이 마침표 (.)를 사용 하 여 합니다.
<!-- https://msdn.microsoft.com/library/ms950960.aspx -->

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](images-images/xs-buildaction.png "빌드 동작 설정: 포함 리소스")

**빌드 작업** 도 확인 및 변경할 수는 **속성** 패드 파일에 대 한 합니다.
이 패드 표시는 **리소스 ID** 코드에서 리소스를 참조 하는 데 사용 되는 합니다. 아래 스크린샷에서 **리소스 ID** 은 **WorkingWithImages.beach.jpg**합니다.
IDE 연결 하 여이 기본값에서 생성 된 **기본 Namespace** 파일 이름으로이 프로젝트에 대 한 각 값 사이 마침표 (.)를 사용 하 여 합니다.
이 ID를 편집할 수는 **속성** 패드 하지만 이러한 예제에 대 한 값 **WorkingWithImages.beach.jpg** 사용 됩니다.

![](images-images/xs-embeddedproperties.png "포함 리소스 속성 채움")

-----

폴더 이름은 폴더에 프로젝트 내에 포함 된 이미지를 배치 하는 경우 리소스 ID에 마침표 (.)도 구분 됩니다. 이동 된 **beach.jpg** 라고 하는 폴더에 이미지 **MyImages** 의 리소스 ID를 초래 **WorkingWithImages.MyImages.beach.jpg**

포함된 된 이미지를 로드 하는 코드 그대로 전달는 **리소스 ID** 에 [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) 아래와 같이 메서드:

```csharp
var embeddedImage = new Image { Source = ImageSource.FromResource("WorkingWithImages.beach.jpg") };
```

현재 리소스 식별자에 대 한 암시적 변환이 있습니다. 를 대신 사용 해야 [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) 또는 `new ResourceImageSource()` 포함 된 이미지를 로드 합니다.

다음 스크린샷에서 각 플랫폼에서 포함된 된 이미지를 표시 하는 결과 보여 줍니다.

[![ResourceImageSource](images-images/resource-sml.png "샘플 포함 된 이미지를 표시 하는 응용 프로그램")](images-images/resource.png#lightbox "샘플 포함 된 이미지를 표시 하는 응용 프로그램")

<a name="Embedded_Images_in_Xaml" />

### <a name="using-xaml"></a>XAML을 사용 하 여

기본 제공 형식 변환기가 있으므로 `string` 를 `ResourceImageSource`, 이러한 유형의 이미지는 xaml을 고유 하 게 로드할 수 없습니다. 대신를 사용 하 여 이미지를 로드 하는 간단한 사용자 지정 XAML 태그 확장을 작성할 수 있습니다는 **리소스 ID** XAML에 지정 된:

```csharp
[ContentProperty ("Source")]
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
   var imageSource = ImageSource.FromResource(Source);

   return imageSource;
 }
}
```

이 확장을 사용 하려면 사용자 지정 추가 `xmlns` 는 XAML에는 프로젝트에 대 한 올바른 네임 스페이스와 어셈블리 값을 사용 하 합니다. 이미지 원본을 설정할 수 있습니다이 구문을 사용 하 여: `{local:ImageResource WorkingWithImages.beach.jpg}`합니다. 전체 XAML 예제는 다음과 같습니다.

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

### <a name="troubleshooting-embedded-images"></a>포함된 이미지를 문제 해결

<a name="Debugging_Embedded_Images" />

#### <a name="debugging-code"></a>코드 디버깅

특정 이미지 리소스를 로드 되 고 있지 않습니다 이유를 이해 하기 어려운 경우도 이기 때문에 올바르게 구성 하는 리소스를 확인할 수 있도록 응용 프로그램에 일시적으로 다음 디버그 코드를 추가할 수 있습니다. 지정된 된 어셈블리에 포함 된 알려진된 모든 리소스를 출력 합니다는 <span class="UIItem">콘솔</span> 문제를 로드 하는 리소스를 디버그할 수 있도록 합니다.

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

#### <a name="images-embedded-in-other-projects-dont-appear"></a>다른 프로젝트에 포함 된 이미지에 표시 되지 않습니다.

`Image.FromResource` 호출 하는 코드와 동일한 어셈블리의 이미지에 대해 모양만 `FromResource`합니다. 변경 하 여 특정 리소스를 포함 하는 어셈블리를 확인할 수 있습니다 위에 디버그 코드를 사용 하는 `typeof()` 문을 `Type` 각 어셈블리에 대 한 것으로 알려져 있습니다.

<a name="Downloading_Images" />

## <a name="downloading-images"></a>이미지 다운로드

다음 XAML에 표시 된 대로 이미지를 표시 하려면 자동으로 다운로드 수 있습니다.

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

해당 하는 C# 코드는 다음과 같습니다.

```csharp
var webImage = new Image { Source = ImageSource.FromUri(new Uri("https://xamarin.com/content/images/pages/forms/example-app.png")) };
```

[ `ImageSource.FromUri` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) 메서드를 사용 하려면 한 `Uri` 개체를 반환 새 [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) 에서 읽는 `Uri`합니다.

또한는 암시적 변환이 URI 문자열에 대 한 다음 예제에서는 작동 하므로:

```csharp
webImage.Source = "https://xamarin.com/content/images/pages/forms/example-app.png";
```

다음 스크린샷에서 각 플랫폼에서 원격 이미지를 표시 하는 결과 보여 줍니다.

[![ImageSource 다운로드](images-images/download-sml.png "샘플 다운로드 한 이미지를 표시 하는 응용 프로그램")](images-images/download.png#lightbox "샘플 응용 프로그램을 다운로드 한 이미지를 표시 합니다.")

<a name="Image_Caching" />

### <a name="downloaded-image-caching"></a>다운로드 한 이미지 캐싱

A [ `UriImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) 는 또한 다음과 같은 속성을 통해 구성 하는 다운로드 한 이미지의 캐싱을 지원 합니다.

- [`CachingEnabled`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CachingEnabled/) -캐싱 사용 여부 (`true` 기본적으로).
- [`CacheValidity`](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.CacheValidity/) -A `TimeSpan` 하는 정의 기간 이미지 로컬에 저장 됩니다.

캐싱은 기본적으로 사용 되 고 24 시간 동안 로컬로 이미지를 저장 합니다. 특정 이미지에 대 한 캐싱을 사용 하지 않으려면 다음과 같이 이미지 소스를 인스턴스화하십시오.

```csharp
image.Source = new UriImageSource { CachingEnabled = false, Uri="http://server.com/image" };
```

특정 캐시 기간 (예: 5 일)을 설정 하려면 다음과 같이 이미지 소스를 인스턴스화하십시오.

```csharp
webImage.Source = new UriImageSource
{
    Uri = new Uri("https://xamarin.com/content/images/pages/forms/example-app.png"),
    CachingEnabled = true,
    CacheValidity = new TimeSpan(5,0,0,0)
};
```

기본 제공 캐싱에서는 각 셀에 이미지, 하거나 수 있는 설정 (바인딩) 이미지 목록이 스크롤 같은 시나리오를 지원 하 고 기본 제공 캐시 셀은 스크롤하여 다시 볼 때 이미지를 다시 로드를 해결할 수 있도록 하기가 매우 쉽습니다.

<a name="Icons_and_splashscreens" />

## <a name="icons-and-splashscreens"></a>아이콘 및 여 시작 화면

관련이 없는 동안는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 보기, 응용 프로그램 아이콘 및 여 시작 화면 Xamarin.Forms 프로젝트에 이미지는 중요 한 사용 됩니다.

아이콘 및 Xamarin.Forms 앱에 대 한 여 시작 화면 설정 각 응용 프로그램 프로젝트에서 수행 됩니다. 즉, iOS, Android 및 UWP에 대 한 이미지 크기의 올바르게 생성 합니다. 이러한 이미지 이름이 고 각 플랫폼의 요구 사항에 따라 배치 해야 합니다.

## <a name="icons"></a>아이콘

참조는 [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md), [Google의도 해](http://developer.android.com/design/style/iconography.html), 및 [타일과 아이콘 자산에 대 한 지침이](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/) 이러한 응용 프로그램 리소스를 만드는 방법에 대 한 자세한 내용은 합니다.

## <a name="splashscreens"></a>여 시작 화면

IOS 및 UWP 응용 프로그램 (시작 화면 또는 기본 이미지 라고도 함) splashscreen이 필요 합니다.

에 대 한 설명서를 참조 [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 및 [시작 화면](/windows/uwp/launch-resume/splash-screens/) Windows 개발자 센터에 있습니다.

## <a name="summary"></a>요약

Xamarin.Forms는 다양 한 이미지를 지정 하는 플랫폼 특정 이미지 또는 플랫폼에서 사용할 동일한 이미지에 대 한 허용 되는 플랫폼 간 응용 프로그램에 포함할 다른 방법으로 제공 합니다. 다운로드 한 이미지도 자동으로 캐시 되며 코딩 하는 일반적인 시나리오를 자동화 합니다.

응용 프로그램 아이콘 및 시작 화면 이미지 등 설치 되며 플랫폼 관련 응용 프로그램에 사용 되는 동일한 지침에 따라 구조의 Xamarin.Forms 아닌 응용 프로그램-구성.


## <a name="related-links"></a>관련 링크

- [WorkingWithImages (샘플)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/)
- [iOS 이미지 작업](~/ios/app-fundamentals/images-icons/index.md)
- [Android의 해](http://developer.android.com/design/style/iconography.html)
- [타일과 아이콘 자산에 대 한 지침](/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets/)
