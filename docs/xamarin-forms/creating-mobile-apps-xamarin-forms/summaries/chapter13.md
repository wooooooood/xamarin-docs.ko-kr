---
title: 요약 13 장입니다. 비트맵
description: Xamarin.Forms를 사용 하 여 모바일 앱을 만듭니다. 요약 13 장입니다. 비트맵
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2018
ms.openlocfilehash: 737e242e14778f38405845541b2ca30d27c3cf5a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334619"
---
# <a name="summary-of-chapter-13-bitmaps"></a>요약 13 장입니다. 비트맵

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)

> [!NOTE] 
> 이 페이지에 대 한 참고 사항 Xamarin.Forms 책의 내용을에서 달라졌는지를 위치 하는 영역을 나타냅니다.

Xamarin.Forms [ `Image` ](xref:Xamarin.Forms.Image) 비트맵을 표시 하는 요소입니다. 모든 Xamarin.Forms 플랫폼 JPEG, PNG, GIF 및 BMP 파일 형식을 지원합니다.

Xamarin.Forms에서 비트맵 네 곳에서 제공합니다.

- URL에 지정 된 대로 웹을 통해
- 공유 라이브러리에 리소스로 포함
- 플랫폼 응용 프로그램 프로젝트에 리소스로 포함
- .NET에서 참조할 수 있는 어디서 `Stream` 개체를 포함 하 여 `MemoryStream`

공유 라이브러리에서 비트맵 리소스 플랫폼 독립적인은 플랫폼 프로젝트에서는 비트맵 리소스가 플랫폼 마다 다릅니다.

> [!NOTE] 
> 책의 텍스트는.NET Standard 라이브러리 바뀌었습니다는 이식 가능한 클래스 라이브러리에 대 한 참조를 만듭니다. 이 책에서 모든 샘플 코드는 .NET 표준 라이브러리를 사용하도록 변경되었습니다.

비트맵을 설정 하 여 지정 합니다 [ `Source` ](xref:Xamarin.Forms.Image.Source) 의 속성 `Image` 형식의 개체에 [ `ImageSource` ](xref:Xamarin.Forms.ImageSource), 세 개의 파생을 사용 하 여 추상 클래스:

- [`UriImageSource`](xref:Xamarin.Forms.UriImageSource) 비트맵을 기반으로 웹을 통해 액세스를 `Uri` 개체 집합 해당 [ `Uri` ](xref:Xamarin.Forms.UriImageSource.Uri) 속성
- [`FileImageSource`](xref:Xamarin.Forms.FileImageSource) 로 설정 하는 폴더 및 파일 경로에 따라 플랫폼 응용 프로그램 프로젝트에 저장 하는 비트맵에 액세스 하기 위한 해당 [ `File` ](xref:Xamarin.Forms.FileImageSource.File) 속성
- [`StreamImageSource`](xref:Xamarin.Forms.StreamImageSource) .NET을 사용 하 여 비트맵을 로드 하기 위한 `Stream` 를 반환 하 여 지정 된 개체를 `Stream` 에서 `Func` 로 해당 [ `Stream` ](xref:Xamarin.Forms.StreamImageSource.Stream) 속성

다음 정적 메서드를 사용할 수 있습니다 또는 (및 자주)는 `ImageSource` 클래스를 반환 하는 모든 `ImageSource` 개체:

- [`ImageSource.FromUri`](xref:Xamarin.Forms.ImageSource.FromUri(System.Uri)) 비트맵을 기반으로 웹을 통해 액세스를 `Uri` 개체
- [`ImageSource.FromResource`](xref:Xamarin.Forms.ImageSource.FromResource*) PCL; 응용 프로그램에 포함 리소스로 저장 된 비트맵에 액세스 하기 위한 [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Type)) 하거나 [ `ImageSource.FromResource` ](xref:Xamarin.Forms.ImageSource.FromResource(System.String,System.Reflection.Assembly)) 다른 원본 어셈블리에서 비트맵을 액세스 하려면
- [`ImageSource.FromFile`](xref:Xamarin.Forms.ImageSource.FromFile(System.String)) 플랫폼 응용 프로그램 프로젝트에서 비트맵에 액세스 하기 위한
- [`ImageSource.FromStream`](xref:Xamarin.Forms.ImageSource.FromStream(System.Func{System.IO.Stream})) 에 따라 비트맵을 로드 한 `Stream` 개체

동등한 옵션이 없습니다 클래스는 `Image.FromResource` 메서드. `UriImageSource` 클래스는 캐싱을 제어 하는 경우에 유용 합니다. `FileImageSource` 클래스는 XAML에서 유용 합니다. `StreamImageSource` 비동기 로딩을 유용 `Stream` 반면 개체 `ImageSource.FromStream` 동기화 됩니다.

## <a name="platform-independent-bitmaps"></a>플랫폼 독립적 비트맵

합니다 [ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) 사용 하 여 웹을 통해 비트맵을 로드 하는 프로젝트 `ImageSource.FromUri`합니다. `Image` 로 설정 된를 `Content` 의 속성을 `ContentPage`이므로 페이지의 크기로 제한 됩니다. 제한 된 비트맵의 크기에 관계 없이 `Image` 요소는 해당 컨테이너의 크기를 채우도록 확장 하 고 비트맵 내에서 최대 크기에 표시 되는 `Image` 비트맵의 가로 세로 비율을 유지 하면서 요소입니다. 영역의 합니다 `Image` 외에 비트맵으로 색이 지정 될 수 있습니다 [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor)합니다.

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) 예제와 유사 하지만 간단히 설정는 `Source` 속성을 URL입니다. 변환에서 처리 되는 [ `ImageSourceConverter` ](xref:Xamarin.Forms.ImageSourceConverter) 클래스입니다.

### <a name="fit-and-fill"></a>맞춤 및 채우기

설정 하 여 비트맵은 늘이는 방법을 제어할 수 있습니다.는 [ `Aspect` ](xref:Xamarin.Forms.Image.Aspect) 의 속성을 `Image` 의 다음 멤버 중 하나에 [ `Aspect` ](xref:Xamarin.Forms.Aspect) 열거형:

- [`AspectFit`](xref:Xamarin.Forms.Aspect.AspectFit): 가로 세로 비율 (기본값)를 준수 합니다.
- [`Fill`](xref:Xamarin.Forms.Aspect.Fill): 영역을 채우는, 가로 세로 비율을 고려 하지 않습니다
- [`AspectFill`](xref:Xamarin.Forms.Aspect.AspectFill): 영역을 채우는 있지만 비트맵의 부분을 자르기 하면 가로 세로 비율을 고려 합니다.

### <a name="embedded-resources"></a>포함된 리소스

PCL, 또는 PCL의 폴더에 비트맵 파일을 추가할 수 있습니다. 지정 된 **빌드 작업** 의 **EmbeddedResource**합니다. 합니다 [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) 샘플을 사용 하는 방법에 설명 `ImageSource.FromResource` 파일을 로드 합니다. 메서드에 전달 된 리소스 이름 뒤에 선택적 폴더 이름 뒤에 점, 및 파일 이름 뒤에 점, 어셈블리 이름으로 구성 됩니다.

프로그램 집합을 `VerticalOptions` 및 `HorizontalOptions` 의 속성을 `Image` 에 `LayoutOptions.Center`, 그러면는 `Image` 비제한 요소. `Image` 비트맵은 동일한 크기 및:

- IOS 및 Android에는 `Image` 비트맵의 픽셀 크기입니다. 비트맵 픽셀과 화면의 픽셀 간에 일대일 매핑이 있습니다.
- 유니버설 Windows 플랫폼에는 `Image` 장치 독립적 단위 비트맵의 픽셀 크기입니다. 대부분의 장치에서 각 비트맵 픽셀 여러 화면 픽셀을 차지합니다.

합니다 [ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) put 샘플을 `Image` 세로에서 `StackLayout` XAML에서. 명명 된 태그 확장 [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML에 포함 된 리소스를 참조 하는 데 도움이 됩니다. 이 클래스는에서 어셈블리에서 리소스를 로드만 있는 라이브러리에 배치 될 수 없습니다.

### <a name="more-on-sizing"></a>크기 조정에 대 한 자세한 내용

크기 비트맵 모든 플랫폼 간에 일관 되 게 하는 것이 좋습니다.
시험해 **StackedBitmap**를 설정할 수 있습니다를 `WidthRequest` 에 `Image` 세로에서 요소 `StackLayout` 크기 수 있지만 플랫폼 간에 일관 되도록이 기술을 사용 하 여 크기를 줄일만 수입니다.

설정할 수도 있습니다는 `HeightRequest` 이미지 수 있도록 플랫폼에서 일관 된 크기 있지만 비트맵의 너비를 제한 된이 방법의 다양성을 제한 합니다. 세로 이미지 `StackLayout`, `HeightRequest` 하지 않아야 합니다.

장치 독립적 단위에서 phone 너비 보다 더 광범위 한 비트맵을 시작 하 여 설정 하는 가장 좋은 방법은 `WidthRequest` 장치 독립적 단위에서 원하는 너비입니다. 에 설명 되어이 [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) 샘플입니다.

합니다 [ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Lewis Carroll의 7 장 표시 *Wonderland Alice의 탐험* John Tenniel 하 여 원래 그림을 사용 하 여:

[![Mad tea 파티의 삼중 스크린 샷](images/ch13fg16-small.png "Mad Hatters Tea 타사 책 텍스트")](images/ch13fg16-large.png#lightbox "Mad Hatters Tea 타사 책 텍스트")

### <a name="browsing-and-waiting"></a>검색 및 대기

합니다 [ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) 샘플 Xamarin 웹 사이트에 저장 하는 스톡 이미지를 탐색할 수 있습니다. .NET을 사용 하 여 [ `WebRequest` ](xref:System.Net.WebRequest) 비트맵의 목록 사용 하 여 JSON 파일을 다운로드 하는 클래스입니다.

> [!NOTE]
> Xamarin.Forms 프로그램을 사용할지 [ `HttpClient` ](xref:System.Net.Http.HttpClient) 대신 [ `WebRequest` ](xref:System.Net.WebRequest) 인터넷을 통해 파일에 액세스 합니다. 

프로그램을 사용 하는 [ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator) 진행을 나타냅니다. 각 비트맵 로드의 읽기 전용 [ `IsLoading` ](xref:Xamarin.Forms.Image.IsLoading) 속성을 `Image` 는 `true`합니다. 합니다 `IsLoading` 속성이 있으므로 바인딩 가능한 속성으로 지원 되는 `PropertyChanged` 해당 속성이 변경 될 때 이벤트가 발생 합니다. 프로그램이이 이벤트 처리기를 연결 하 고 현재 설정을 사용 `IsLoaded` 설정 하는 [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) 의 속성을 `ActivityIndicator`합니다.

## <a name="streaming-bitmaps"></a>스트리밍 비트맵

합니다 `ImageSource.FromStream` 메서드를 만듭니다를 `ImageSource` .NET 기반 `Stream`입니다. 메서드에 전달 해야 합니다는 `Func` 반환 하는 개체를 `Stream` 개체입니다.

### <a name="accessing-the-streams"></a>스트림 액세스

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) 샘플을 사용 하는 방법에 설명 합니다 `ImaageSource.FromStream` 포함 리소스로 저장 비트맵을 로드 하 고 웹에서 비트맵을 로드 하는 메서드.

### <a name="generating-bitmaps-at-run-time"></a>런타임에 비트맵 생성

모든 Xamarin.Forms 플랫폼 지원 코드에서 생성 하 고 다음 저장 하는 작업을 쉽게는 압축 되지 않은 BMP 파일 형식으로는 `MemoryStream`합니다. 이 방법을 사용 하면 알고리즘 방식으로 런타임에 비트맵 만들기에서 구현 되는 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 클래스를 **Xamrin.FormsBook.Toolkit** 라이브러리입니다.

"수행 직접" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) 샘플의 사용법을 보여 줍니다. `BmpMaker` 그라데이션 이미지를 사용 하 여 비트맵을 만들 수 있습니다.

## <a name="platform-specific-bitmaps"></a>플랫폼별 비트맵

모든 Xamarin.Forms 플랫폼 플랫폼 응용 프로그램 어셈블리에서 비트맵을 저장할 수 있습니다. 이러한 플랫폼 비트맵 형식의 Xamarin.Forms 응용 프로그램에 의해 가져오면 [ `FileImageSource` ](xref:Xamarin.Forms.FileImageSource)합니다. 에 대 한 사용:

- 합니다 [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) 속성 [`MenuItem`](xref:Xamarin.Forms.MenuItem)
- 합니다 [ `Icon` ](xref:Xamarin.Forms.MenuItem.Icon) 속성 [`ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)
- 합니다 [ `Image` ](xref:Xamarin.Forms.Button) 속성 `Button`

플랫폼 어셈블리에는 이미 아이콘 및 시작 화면에 대 한 비트맵에

- IOS 프로젝트에서에 **리소스** 폴더
- 하위 폴더에 Android 프로젝트에는 **리소스** 폴더
- Windows 프로젝트에서에 **자산** 폴더 (Windows 플랫폼은 해당 폴더에 비트맵을 제한 하지 않습니다) 있음

합니다 [ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) 샘플 코드를 사용 하 여 플랫폼 응용 프로그램 프로젝트에서 아이콘을 표시 합니다.

### <a name="bitmap-resolutions"></a>비트맵 해상도

모든 플랫폼에 다른 장치 해상도 대해 비트맵 이미지의 여러 버전을 저장할 수 있습니다. 런타임 시 적절 한 버전 화면 장치 해상도에 따라 로드 됩니다.

Ios의 경우 이러한 비트맵 파일 이름에 접미사 구분 됩니다.

- 160 DPI 장치 (장치 독립적 단위 1 픽셀)에 대 한 접미사 없음
- '@2x' 320 DPI 장치 (2는 DIU 픽셀)에 대 한 접미사
- '@3x' 480 DPI 장치 (3는 DIU 픽셀)에 대 한 접미사

1 인치의 사각형으로 표시 하기 위한 비트맵은 세 가지 버전에 존재 합니다.

- 160 픽셀 사각형에서 MyImage.jpg
- MyImage@2x.jpg 320 픽셀 사각형
- MyImage@3x.jpg 480 픽셀 사각형

이 비트맵 MyImage.jpg으로 프로그램 참조 하지만 적절 한 버전의 화면 해상도에 따라 런타임에 검색 됩니다. 제한 되지 않은, 비트맵은 항상 160 장치 독립적 단위에서 렌더링 됩니다.

Android 비트맵의 다양 한 하위 폴더에 저장 합니다 **리소스** 폴더:

- drawable-ldpi (낮은 DPI) 120 DPI 장치 (0.75를 DIU 픽셀)에 대 한
- drawable-mdpi (중간) (160DPI 장치용 (DIU는 1 픽셀)
- drawable-hdpi (높음) 240 DPI 장치 (1.5는 DIU 픽셀)에 대 한
- drawable-xhdpi (매우 높음) 320 DPI 장치 (2는 DIU 픽셀)에 대 한
- drawable-xxhdpi (추가 매우 높은) 480 DPI 장치 (3는 DIU 픽셀)에 대 한
- drawable-xxxhdpi (세 개의 추가 최대값) 640 DPI 장치 (4는 DIU 픽셀)에 대 한

정사각형 1 인치에서 렌더링 되는 데는 비트맵에 대 한 비트맵의 다양 한 버전에는 이름은 같지만 다른 크기로 있고 이러한 폴더에 저장:

- drawable-ldpi/MyImage.jpg 120 픽셀 사각형
- drawable-mdpi/MyImage.jpg 160 픽셀 사각형
- drawable-hdpi/MyImage.jpg 240 픽셀 사각형
- drawable-xhdpi/MyImage.jpg 320 픽셀 사각형
- drawable-xxhdpi/MyImage.jpg 480 픽셀 사각형
- drawable-xxxhdpi/MyImage.jpg 640 픽셀 사각형

비트맵은 항상 160 장치 독립적 단위에 렌더링 합니다. (표준 Xamarin.Forms 솔루션 템플릿을 포함 hdpi, xhdpi, 및 xxhdpi 폴더.)

UWP 프로젝트에 구성 된 장치 독립적 단위 당 픽셀 수 크기 조정 비율을 백분율로 예를 들어 비트맵 이름 지정 체계를 지원 합니다.

- 320 픽셀 사각형 MyImage.scale 200.jpg

일부 백분율에만 유효 합니다. 이 책에 대 한 샘플 프로그램의 이미지만 포함 **확장 200** 접미사 하지만 포함 하는 현재 Xamarin.Forms 솔루션 템플릿을 **확장-100**를 **확장 125**, **배율을 150**, 및 **확장-400**합니다.

플랫폼 프로젝트에 비트맵을 추가 하는 경우는 **빌드 작업** 이어야 합니다.

- iOS: **BundleResource**
- Android: **AndroidResource**
- UWP: **콘텐츠**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) 샘플으로 구성 된 두 개의 단추와 유사한 개체를 만듭니다 `Image` 사용 하 여 요소를 `TapGestureRecognizer` 설치 합니다. 개체 1 인치의 사각형 되도록 작성 되었습니다. 합니다 `Source` 속성을 `Image` 사용 하도록 설정 됩니다 `OnPlatform` 및 `On` 플랫폼에서 잠재적으로 서로 다른 파일 이름을 참조 하는 개체입니다. 비트맵 이미지는 크기 비트맵을 검색 하 고 렌더링을 볼 수 있도록 해당 픽셀 크기를 나타내는 숫자를 포함 합니다.

### <a name="toolbars-and-their-icons"></a>도구 모음 및 해당 아이콘

플랫폼별 비트맵의 주요 용도 중 하나는 추가 하 여 생성 된 Xamarin.Forms 도구 모음 [ `ToolbarItem` ](xref:Xamarin.Forms.ToolbarItem) 개체를 [ `ToolbarItems` ](xref:Xamarin.Forms.Page.ToolbarItems) 정의한컬렉션`Page`. `ToobarItem` 파생 [ `MenuItem` ](xref:Xamarin.Forms.MenuItem) 하는 일부 속성을 상속 합니다.

가장 중요 한 `ToolbarItem` 속성입니다.

- [`Text`](xref:Xamarin.Forms.MenuItem.Text) 플랫폼에 따라 표시 될 수 있는 텍스트 및 `Order`
- [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) 형식의 `FileImageSource` 플랫폼에 따라 표시 될 수 있는 이미지 및 `Order`
- [`Order`](xref:Xamarin.Forms.ToolbarItem.Order) 형식의 [ `ToolbarItemOrder` ](xref:Xamarin.Forms.ToolbarItemOrder)는 세 멤버가 포함 된 열거형 [ `Default` ](xref:Xamarin.Forms.ToolbarItemOrder.Default)하십시오 [ `Primary` ](xref:Xamarin.Forms.ToolbarItemOrder.Primary), 및 [ `Secondary` ](xref:Xamarin.Forms.ToolbarItemOrder.Secondary).

수가 `Primary` 항목 3 또는 4로 제한 되어야 합니다. 포함 해야는 `Text` 모든 항목에 대 한 설정입니다. 만 대부분의 플랫폼에 대 한는 `Primary` 항목의 필요를 `Icon` Windows 8.1 필요 하지만 `Icon` 모든 항목에 대 한 합니다. 아이콘에는 32 장치 독립적 단위 정사각형 이어야 합니다. `FileImageSource` 형식은 플랫폼별 지를 나타냅니다.

합니다 `ToolbarItem` 발생을 [ `Clicked` ](xref:Xamarin.Forms.MenuItem.Clicked) 변하는데, 비슷한 이벤트를 `Button`입니다. `ToolbarItem` 또한 지원 [ `Command` ](xref:Xamarin.Forms.MenuItem.Command) 하 고 [ `CommandParameter` ](xref:Xamarin.Forms.MenuItem.CommandParameter) MVVM과 관련 하 여 자주 사용 되는 속성입니다. (참조 [18 장, MVVM](chapter18.md)).

IOS 및 Android 도구 모음을 표시 하는 페이지가 되도록 요구할를 [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) 또는 탐색 페이지는 `NavigationPage`합니다. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) 집합을 프로그래밍 합니다 `MainPage` 속성 해당 `App` 클래스를 [ `NavigationPage` 생성자](xref:Xamarin.Forms.NavigationPage.%23ctor(Xamarin.Forms.Page)) 사용 하 여를 `ContentPage` 인수를 도구 모음 생성 및 이벤트 처리기를 보여 줍니다.

### <a name="button-images"></a>단추 이미지

설정에 플랫폼 전용 비트맵을 사용할 수도 있습니다는 [ `Image` ](xref:Xamarin.Forms.Button.Image) 속성을 `Button` 나타난 것 처럼 32 장치 독립적 단위 정사각형 비트맵에는 [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) 샘플입니다.

> [!NOTE]
> 단추의 이미지를 사용 하는 향상 되었습니다. 참조 [단추를 사용 하 여 비트맵을 사용 하 여](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)입니다.

## <a name="related-links"></a>관련 링크

- [13 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [13 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [이미지 작업](~/xamarin-forms/user-interface/images.md)
- [단추를 사용 하 여 비트맵을 사용 하 여](~/xamarin-forms/user-interface/button.md#using-bitmaps-with-buttons)
