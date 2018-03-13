---
title: "13 장의 요약입니다. Bitmaps"
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5D153857-B6B7-4A14-8FB9-067DE198C2C7
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 74e5e47a481d02fe11be4b770b818d2c88b517f7
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/12/2018
---
# <a name="summary-of-chapter-13-bitmaps"></a>13 장의 요약입니다. Bitmaps

Xamarin.Forms는 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) 요소 비트맵이 표시 됩니다. 모든 Xamarin.Forms 플랫폼에는 JPEG, PNG, GIF, 및 BMP 파일 형식을 지원합니다.

Xamarin.Forms에는 비트맵에서 네 가지 위치를 가져옵니다.

- URL에 지정 된 대로 웹을 통해
- 일반적인 이식 가능한 클래스 라이브러리에 리소스로 포함
- 플랫폼 응용 프로그램 프로젝트에 리소스로 포함
- .NET에서 참조할 수 있는 모든 위치에서 `Stream` 개체를 포함 하 여 `MemoryStream`

비트맵의 PCL 리소스가 플랫폼 독립적인 플랫폼 프로젝트의 비트맵 리소스는 플랫폼 특정입니다.

비트맵을 설정 하 여 지정는 [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Source/) 속성 `Image` 형식의 개체를 [ `ImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSource/), 추상 클래스에 세 가지 파생 항목:

- [`UriImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.UriImageSource/) 비트맵을 기반으로 웹을 통해 액세스 하기 위한는 `Uri` 설정 개체의 [ `Uri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.UriImageSource.Uri/) 속성
- [`FileImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/) 로 설정 하는 폴더 및 파일 경로에 따라 플랫폼 응용 프로그램 프로젝트에 저장 된 비트맵에 액세스 하기 위해 해당 [ `File` ](https://developer.xamarin.com/api/property/Xamarin.Forms.FileImageSource.File/) 속성
- [`StreamImageSource`](https://developer.xamarin.com/api/type/Xamarin.Forms.StreamImageSource/) .NET을 사용 하 여 비트맵을 로드 하기 위한 `Stream` 반환 하 여 지정 된 개체는 `Stream` 에서 `Func` 로 설정 해당 [ `Stream` ](https://developer.xamarin.com/api/property/Xamarin.Forms.StreamImageSource.Stream/) 속성

또는 (및 일반적으로)의 다음 정적 메서드를 사용할 수 있습니다는 `ImageSource` 반환 하는 모든 클래스 `ImageSource` 개체:

- [`ImageSource.FromUri`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromUri/p/System.Uri/) 비트맵을 기반으로 웹을 통해 액세스 하기 위한는 `Uri` 개체
- [`ImageSource.FromResource`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/) PCL, 응용 프로그램에 포함 리소스로 저장 된 비트맵에 액세스 하기 위해 또는 [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Type/) 또는 [ `ImageSource.FromResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromResource/p/System.String/System.Reflection.Assembly/) 비트맵 다른 소스 어셈블리에 액세스 하려면
- [`ImageSource.FromFile`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromFile/p/System.String/) 플랫폼 응용 프로그램 프로젝트에서 비트맵에 액세스 하기 위한
- [`ImageSource.FromStream`](https://developer.xamarin.com/api/member/Xamarin.Forms.ImageSource.FromStream/p/System.Func%7BSystem.IO.Stream%7D/) 기반으로 하는 비트맵을 로드 하는 `Stream` 개체

클래스의 동등한 옵션이 없습니다는 `Image.FromResource` 메서드. `UriImageSource` 클래스는 캐싱을 제어 하는 경우에 유용 합니다. `FileImageSource` 클래스는 XAML에서 유용 합니다. `StreamImageSource` 비동기 로딩 유용 `Stream` 반면 개체 `ImageSource.FromStream` 는 동기 메서드 됩니다.

## <a name="platform-independent-bitmaps"></a>플랫폼 독립적 비트맵

[ **WebBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapCode) 프로젝트 비트맵을 사용 하 여 웹을 통해 로드 `ImageSource.FromUri`합니다. `Image` 요소가로 설정 되는 `Content` 속성은 `ContentPage`이므로 페이지의 크기 제한 됩니다. 비트맵의 크기는 제한에 관계 없이 `Image` 해당 컨테이너의 크기를 위해 확장 된 요소 및 비트맵 내에서 최대 크기에 표시 됩니다는 `Image` 비트맵의 가로 세로 비율을 유지 하면서 요소입니다. 영역에는 `Image` 비트맵 수 색을 지정 외에 [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/)합니다.

[ **WebBitmapXaml** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/WebBitmapXaml) 샘플 유사 하지만 설정는 `Source` 속성을 URL입니다. 변환에서 처리 되는 [ `ImageSourceConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageSourceConverter/) 클래스입니다.

### <a name="fit-and-fill"></a>적합성 및 채우기

비트맵을 설정 하 여 늘어나는 방법을 제어할 수 있습니다는 [ `Aspect` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.Aspect/) 의 속성은 `Image` 의 다음 멤버 중 하나에 [ `Aspect` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect/) 열거형:

- [`AspectFit`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.AspectFit/): 문자의 가로 세로 비율 (기본값)
- [`Fill`](https://developer.xamarin.com/api/field/Xamarin.Forms.Aspect.Fill/): 영역을 채우는, 가로 세로 비율을 고려 하지 않습니다
- [`AspectFill`](https://developer.xamarin.com/api/type/Xamarin.Forms.Aspect.AspectFill/): 영역을 채우는 하지만 비트맵의 부분을 자르기 작업 만으로 가로 세로 비율을 고려 합니다.

### <a name="embedded-resources"></a>포함된 리소스

비트맵 파일 또는 PCL에 폴더는 PCL에 추가할 수 있습니다. 지정 된 **빌드 작업** 의 **포함 리소스**합니다. [ **ResourceBitmapCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ResourceBitmapCode) 샘플에서는 사용 하는 방법을 보여 줍니다. `ImageSource.FromResource` 파일을 로드 합니다. 메서드에 전달 된 리소스 이름은 어셈블리 이름, 선택적 폴더 이름, 점 및 뒤에 파일 이름으로 점, 밑줄 구성 됩니다.

프로그램 설정의 `VerticalOptions` 및 `HorizontalOptions` 의 속성은 `Image` 를 `LayoutOptions.Center`, 수 있게 해줍니다는 `Image` 제한 되지 않은 요소입니다. `Image` 비트맵은 동일한 크기 및:

- IOS 및 Android에서의 `Image` 비트맵의 픽셀 크기입니다. 비트맵 픽셀, 픽셀 화면 간의 일대일 매핑이 있습니다.
- Windows 런타임 플랫폼의 `Image` 장치 독립적 단위에 비트맵의 픽셀 크기입니다. 대부분의 장치에서 각 비트맵 픽셀 여러 화면 픽셀을 차지합니다.

[ **StackedBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/StackedBitmap) 넣습니다 예제는 `Image` 세로에서 `StackLayout` XAML에서 합니다. 명명 된 태그 확장 [ `ImageResourceExtension` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter13/StackedBitmap/StackedBitmap/StackedBitmap/ImageResourceExtension.cs) XAML에서 포함 된 리소스를 참조 하는 데 도움이 됩니다. 이 클래스는 자신이 어셈블리에서 리소스 로드 위치, 라이브러리에 배치 될 수 없습니다.

### <a name="more-on-sizing"></a>크기 조정에 대 한 자세한

모든 플랫폼 간에 일관 되 게 크기 비트맵 하는 것이 좋습니다.
작업할 수 있도록 **StackedBitmap**를 설정할 수 있습니다는 `WidthRequest` 에 `Image` 세로 요소 `StackLayout` 크기를 플랫폼에 있지만 간에 일관 되 게 하려면이 방법을 사용 하 여 크기를 줄일만 수입니다.

설정할 수도 있습니다는 `HeightRequest` 이미지 크기는 플랫폼에서 일관 된 있지만 비트맵의 제약 조건이 지정 된 너비는이 방법의 다양 한 기능을 제한 합니다. 세로에 있는 이미지에 대 한 `StackLayout`, `HeightRequest` 피해 야 합니다.

장치 독립적 단위에서 전화 너비 보다 넓은 비트맵으로 시작 하 고 설정 하는 가장 좋은 방법입니다 `WidthRequest` 장치 독립적 단위로 원하는 너비입니다. 이 확인할는 [ **DeviceIndBitmapSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DeviceIndBitmapSize) 샘플.

[ **MadTeaParty** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/MadTeaParty) Lewis Carroll의 7 장 표시 *Wonderland Alice의 탐험* John Tenniel 하 여 원래 그림:

[![Mad 찻잔 파티의 삼중 스크린 샷](images/ch13fg16-small.png "Mad Hatters 찻잔 파티 책 텍스트")](images/ch13fg16-large.png#lightbox "Mad Hatters 찻잔 파티 책 텍스트")

### <a name="browsing-and-waiting"></a>찾아보기 및 대기

[ **ImageBrowser** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageBrowser) 샘플을 사용 하면 Xamarin 웹 사이트에 저장 된 기존 이미지를 검색할 수 있습니다. .NET을 사용 하 여 `WebRequest` 비트맵의 목록 사용 하 여 JSON 파일을 다운로드 하는 클래스입니다.

프로그램에서 사용 하는 [ `ActivityIndicator` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ActivityIndicator/) 작업 진행 나타냅니다. 각 비트맵 로드 읽기 전용 [ `IsLoading` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Image.IsLoading/) 속성 `Image` 은 `true`합니다. `IsLoading` 속성 이므로 바인딩 가능한 속성에 의해 지원 되는 `PropertyChanged` 해당 속성이 변경 되 면 이벤트가 발생 합니다. 프로그램이이 이벤트 처리기를 연결 하 고의 현재 설정을 사용 하 여 `IsLoaded` 설정 하는 [ `IsRunning` ](https://api/property/Xamarin.Forms.ActivityIndicator.IsRunning/) 속성은 `ActivityIndicator`합니다.

## <a name="streaming-bitmaps"></a>스트리밍 비트맵

`ImageSource.FromStream` 메서드 만듭니다는 `ImageSource` .NET 기반 `Stream`합니다. 메서드를 전달 해야 합니다는 `Func` 개체를 반환 하는 `Stream` 개체입니다.

### <a name="accessing-the-streams"></a>스트림 액세스

[ **BitmapStreams** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/BitmapStreams) 샘플에 사용 하는 방법을 보여 줍니다는 `ImaageSource.FromStream` 메서드 포함 리소스로 저장 비트맵을 로드 하 고 웹을 통해 비트맵을 로드 합니다.

### <a name="generating-bitmaps-at-run-time"></a>런타임에 비트맵을 생성합니다.

모든 Xamarin.Forms 플랫폼이 지원 쉽게 코드에서 생성 하 고 다음에 저장 하는 압축 되지 않은 BMP 파일 형식으로는 `MemoryStream`합니다. 이 기술을 사용 하면 알고리즘 방식으로 런타임에 비트맵 만들기에서 구현 되는 [ `BmpMaker` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BmpMaker.cs) 클래스에 **Xamrin.FormsBook.Toolkit** 라이브러리입니다.

"않습니다 스스로 하기" [ **DiyGradientBitmap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/DiyGradientBitmap) 샘플의 사용법을 보여줍니다 `BmpMaker` 그라데이션 이미지가 있는 비트맵을 만들 수 있습니다.

## <a name="platform-specific-bitmaps"></a>플랫폼별 비트맵

모든 Xamarin.Forms 플랫폼 플랫폼 응용 프로그램 어셈블리에서 비트맵을 저장할 수 있습니다. 이러한 플랫폼 비트맵은 형식의 Xamarin.Forms 응용 프로그램으로 가져오면 [ `FileImageSource` ](https://developer.xamarin.com/api/type/Xamarin.Forms.FileImageSource/)합니다. 위해 사용합니다.

- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Icon/) 속성 [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/)
- [ `Icon` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) 속성 [`ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)
- [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) 속성 `Button`

플랫폼 어셈블리에 아이콘 및 시작 화면에 대 한 비트맵 이미 포함 되어 있습니다.

- IOS 프로젝트에서에 **리소스** 폴더
- Android 프로젝트의 하위 폴더에는 **리소스** 폴더
- Windows 프로젝트에서에 **자산** 폴더 (하지만 Windows 플랫폼은 해당 폴더를 비트맵을 제한 하지 않음)

[ **PlatformBitmaps** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/PlatformBitmaps) 샘플 코드를 사용 하 여 플랫폼 응용 프로그램 프로젝트의 아이콘을 표시 합니다.

### <a name="bitmap-resolutions"></a>비트맵 해상도

모든 플랫폼에서 다른 장치 해상도 대해 비트맵 이미지의 여러 버전을 저장할 허용 합니다. 런타임 시 적절 한 버전의 화면 장치 해상도에 따라 로드 됩니다.

Ios에서 이러한 비트맵 파일 이름에 접미사 구분 됩니다.

- 160 DPI 장치 (장치 독립적 단위에 1 픽셀)에 대 한 접미사 없음
- '@2x' 320 DPI 장치 (2는 DIU 픽셀)에 대 한 접미사
- '@3x' 480 DPI 장치 (DIU는 픽셀 3)에 대 한 접미사

비트맵에 1 인치의 사각형으로 표시 될 3 가지 버전으로 존재 합니다.

- 정사각형 160 픽셀로 MyImage.jpg
- MyImage@2x.jpg 정사각형 320 픽셀
- MyImage@3x.jpg 정사각형 480 픽셀

이 비트맵 MyImage.jpg으로 프로그램을 참조 하지만 적절 한 버전의 화면 해상도에 따라 런타임에 검색 됩니다. 제약 받지 않을, 비트맵은 항상 160 장치 독립적 단위에서 렌더링 됩니다.

Android의 경우 비트맵의 다양 한 하위 폴더에 저장 됩니다는 **리소스** 폴더:

- 그릴-ldpi (낮은 DPI) 120 DPI 장치용 (에 DIU 0.75 픽셀 단위)
- 그릴-mdpi (중간) 160 DPI 장치 (1 픽셀의 DIU에)에 대 한
- 그릴-hdpi (높음) 240 DPI 장치 (1.5는 DIU 픽셀)에 대 한
- 그릴-xhdpi (extra 높음) 320 DPI 장치용 (2는 DIU 픽셀)
- 그릴-xxhdpi (extra extra 높음) 480 DPI 장치 (DIU는 픽셀 3)에 대 한
- 그릴-xxxhdpi (세 개의 추가 최대값) 640 DPI 장치 (4는 DIU 픽셀)에 대 한

1 평방 인치에서 렌더링할 수를 비트맵에 대 한 비트맵의 다양 한 버전은 이름은 같지만 서로 다른 크기 있으며 이러한 폴더에 저장:

- 그릴-ldpi/MyImage.jpg 정사각형 120 픽셀
- 그릴-mdpi/MyImage.jpg 정사각형 160 픽셀
- 그릴-hdpi/MyImage.jpg 정사각형 240 픽셀
- 그릴-xhdpi/MyImage.jpg 정사각형 320 픽셀
- 그릴-xxhdpi/MyImage.jpg 정사각형 480 픽셀
- 그릴-xxxhdpi/MyImage.jpg 정사각형 640 픽셀

비트맵은 항상 160 장치 독립적 단위에 렌더링 합니다. (표준 Xamarin.Forms 솔루션 템플릿을 포함 hdpi, xhdpi, 및 xxhdpi 폴더입니다.)

Windows 런타임 프로젝트는 비트맵 이름 지정으로 구성 된 장치 독립적 단위 (픽셀)는 배율 인수입니다. 백분율, 예를 들어 체계를 지원 합니다.

- MyImage.scale 200.jpg 정사각형 320 픽셀

일부 백분율만 올바른지 확인 하십시오입니다. 이 책에 대 한 샘플 프로그램 이미지를 포함 **배율 200** 접미사 하지만 포함 하는 현재 Xamarin.Forms 솔루션 템플릿을 **배율 100**, **배율 125**, **배율 150**, 및 **배율 400**합니다. 

플랫폼 프로젝트에 비트맵 추가 하는 경우는 **빌드 작업** 이어야 합니다.

- iOS: **BundleResource**
- Android: **AndroidResource**
- Windows 런타임: **콘텐츠**

[ **ImageTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ImageTap) 샘플에서는 구성 된 두 단추와 비슷한 개체를 만듭니다 `Image` 요소는 `TapGestureRecognizer` 설치 합니다. 개체 1 인치의 사각형 된다는 것입니다. `Source` 속성 `Image` 를 사용 하 여 설정 되어 `OnPlatform` 및 `On` 플랫폼에서 잠재적으로 서로 다른 파일 이름을 참조 하는 개체입니다. 비트맵 이미지 어떤 크기 비트맵 검색 되 고 렌더링을 확인할 수 있도록 허용 된 픽셀 크기를 나타내는 숫자를 포함 합니다.

### <a name="toolbars-and-their-icons"></a>도구 모음과 아이콘

Xamarin.Forms 도구 모음을 추가 하 여 생성 된 플랫폼별 비트맵의 주요 용도 중 하나는 [ `ToolbarItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/) 개체는 [ `ToolbarItems` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.ToolbarItems/) 에정의된컬렉션`Page`. `ToobarItem` 파생 [ `MenuItem` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) 하는 일부 속성을 상속 합니다.

가장 중요 한 `ToolbarItem` 속성입니다.

- [`Text`](https://developer.xamarin.com/api/property/Xamarin.Forms.MenuItem.Text/) 플랫폼에 따라 표시 될 수 있는 텍스트 및 `Order`
- [`Icon`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Icon/) 형식의 `FileImageSource` 플랫폼에 따라 표시 될 수 있는 이미지에 대 한 및 `Order`
- [`Order`](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Order/) 형식의 [ `ToolbarItemOrder` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItemOrder/)는 세 멤버가 포함 된 열거형 [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Default/), [ `Primary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Primary/), 및 [ `Secondary` ](https://developer.xamarin.com/api/field/Xamarin.Forms.ToolbarItemOrder.Secondary/).

수가 `Primary` 항목 3 또는 4 제한 되어야 합니다. 포함 해야는 `Text` 모든 항목에 대 한 설정입니다. 대부분의 플랫폼에만 대 한는 `Primary` 항목 해야는 `Icon` Windows 8.1 필요 하지만 `Icon` 모든 항목에 대 한 합니다. 아이콘에는 사각형 32 장치 독립적 단위 여야 합니다. `FileImageSource` 형식은 플랫폼 관련 되는지 나타냅니다.

`ToolbarItem` 발생 한 [ `Clicked` ](https://developer.xamarin.com/api/event/Xamarin.Forms.MenuItem.Clicked/) 와 매우 비슷한 누를 때 이벤트는 `Button`합니다. `ToolbarItem` 또한 지원 [ `Command` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.Command/) 및 [ `CommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ToolbarItem.CommandParameter/) MVVM 관련 하 여 자주 사용 되는 속성입니다. (참조 [18 장, MVVM](chapter18.md)).

IOS 및 Android도 록 요구할 도구 모음을 표시 하는 페이지는 [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) 또는 탐색 페이지는 `NavigationPage`합니다. [ **ToolbarDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ToolbarDemo) 집합 프로그램는 `MainPage` 속성의 해당 `App` 클래스를 [ `NavigationPage` 생성자](https://developer.xamarin.com/api/constructor/Xamarin.Forms.NavigationPage.NavigationPage/p/Xamarin.Forms.Page/) 와 `ContentPage` 인수는 도구 모음의 생성 및 이벤트 처리기를 보여 줍니다.

### <a name="button-images"></a>단추 이미지

설정 하려면 플랫폼별 비트맵을 사용할 수도 있습니다는 [ `Image` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Image/) 속성 `Button` 에 나타난 것 처럼 32 장치 독립적 단위 정사각형의 비트맵에는 [ **ButtonImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13/ButtonImage) 샘플.



## <a name="related-links"></a>관련 링크

- [13 장 전체 텍스트 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch13-Apr2016.pdf)
- [13 장 샘플](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter13)
- [이미지 작업](~/xamarin-forms/user-interface/images.md)
