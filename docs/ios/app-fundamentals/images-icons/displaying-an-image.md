---
title: Xamarin.ios에서 이미지 표시
description: 이 문서에서는 Xamarin.ios 앱에 이미지 자산을 포함 하 고 코드를 사용 C# 하거나 iOS 디자이너의 컨트롤에 해당 이미지를 할당 하 여 해당 이미지를 표시 하는 방법에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/24/2018
ms.openlocfilehash: cda45f01dae2dc17c2517a7f013acacde7906a4b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004491"
---
# <a name="displaying-an-image-in-xamarinios"></a>Xamarin.ios에서 이미지 표시

_이 문서에서는 Xamarin.ios 앱에 이미지 자산을 포함 하 고 코드를 사용 C# 하거나 iOS 디자이너의 컨트롤에 해당 이미지를 할당 하 여 해당 이미지를 표시 하는 방법에 대해 설명 합니다._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>Xamarin.ios 앱에서 이미지 추가 및 구성

Xamarin.ios 앱에서 사용할 이미지를 추가 하는 경우 개발자는 _자산 카탈로그_ 를 사용 하 여 앱에 필요한 모든 iOS 장치 및 해상도를 지원 합니다.

IOS 7에 추가 된 **자산 카탈로그 이미지 집합** 에는 앱에 대 한 다양 한 장치 및 크기 조정 요소를 지 원하는 데 필요한 이미지의 모든 버전 또는 표현이 포함 되어 있습니다. 이미지 자산 파일 이름을 사용 하는 대신, **이미지 집합** 에서는 Json 파일을 사용 하 여 장치 및/또는 해상도에 속하는 이미지를 지정 합니다. Ios에서 이미지를 관리 하 고 지 원하는 기본 방법입니다 (iOS 9 이상).

## <a name="adding-images-to-an-asset-catalog-image-set"></a>자산 카탈로그 이미지 집합에 이미지 추가

위에서 설명한 것 처럼 **자산 카탈로그 이미지 집합** 에는 앱에 대 한 다양 한 장치 및 크기 조정 요소를 지 원하는 데 필요한 이미지의 모든 버전 또는 표현이 포함 되어 있습니다. 이미지 자산 파일 이름을 사용 하는 대신, **이미지 집합** 에서는 Json 파일을 사용 하 여 장치 및/또는 해상도에 속하는 이미지를 지정 합니다.

새 이미지 집합을 만들고 여기에 이미지를 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **솔루션 탐색기**에서 `Assets.xcassets` 파일을 두 번 클릭 하 여 편집용으로 엽니다.

    ![](displaying-an-image-images/imageset01.png "The Assets.xcassets in the Solution Explorer")
2. **자산 목록을** 마우스 오른쪽 단추로 클릭 하 고 **새 이미지 집합**을 선택 합니다.

    ![](displaying-an-image-images/imageset02.png "Adding a New Image Set")
3. 새 이미지 집합을 선택 하면 편집기가 표시 됩니다.

    ![](displaying-an-image-images/imageset03.png "The Image Set editor")
4. 여기에서 필요한 여러 장치 및 해상도에 대 한 이미지를 끌어 놓습니다.
5. **자산 목록** 에서 새 이미지 집합의 **이름을** 두 번 클릭 하 여 편집 합니다.![](displaying-an-image-images/imageset04.png "새 이미지 집합의 이름 편집")

IOS 디자이너에서 **이미지 집합** 을 사용 하는 경우 속성 편집기의 드롭다운 목록에서 집합의 이름을 선택 하면 됩니다.

![](displaying-an-image-images/imageset06.png "Select an image set's name from the dropdown list")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 자산 카탈로그를 열고 왼쪽 위 모서리에서 **더하기** 단추를 클릭 합니다.

    ![](displaying-an-image-images/asset5.png "Click the Plus button")

2. **이미지 집합 추가** 를 선택 하면 새 이미지 집합에 대해 이미지 집합 편집기가 표시 됩니다. 여기에서 필요한 여러 장치 및 해상도에 대 한 이미지를 끌어 놓습니다.

    ![](displaying-an-image-images/asset7.png "The image set editor")

### <a name="renaming-an-image-set"></a>이미지 집합 이름 바꾸기

이미지 집합의 이름을 바꾸려면 다음을 수행 합니다.

1. **솔루션 탐색기**에서 **자산 카탈로그** 파일을 두 번 클릭 하 여 편집용으로 엽니다.

    ![](displaying-an-image-images/rename01.png "The Asset Catalog in the Solution Explorer")
2. 이름을 바꿀 **이미지 집합** 선택:

    ![](displaying-an-image-images/rename02.png "Select the Image Set to rename")
3. **속성 탐색기**에서 아래쪽으로 스크롤하고 **기타** 섹션 아래에서 **이름**을 선택 합니다.

    ![](displaying-an-image-images/rename03.png "Select Name under the Misc section")
4. **이미지 집합** 의 새 **이름을** 입력 하 고 변경 내용을 저장 합니다.

-----

코드에서 **이미지 집합** 을 사용 하는 경우 `UIImage` 클래스의 `FromBundle` 메서드를 호출 하 여 이름별로 참조 합니다. 예를 들면 다음과 같습니다.

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> 이미지 집합에 할당 된 이미지가 제대로 표시 되지 않는 경우 올바른 파일 이름이 `FromBundle` 메서드 ( **이미지 집합** 및 부모 **자산 카탈로그** 이름이 아님)와 함께 사용 되 고 있는지 확인 합니다. PNG 이미지의 경우 `.png` 확장을 생략할 수 있습니다. 다른 이미지 형식의 경우 확장이 필요 합니다 (예: `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>자산 카탈로그에서 벡터 이미지 사용

IOS 8부터 개발자가 다른 해상도의 개별 비트맵 파일을 포함 하는 대신 카세트에 **PDF** 형식의 벡터 이미지를 포함할 수 있도록 하는 **이미지 집합** 에 특수 **vector** 클래스가 추가 되었습니다. 이 메서드를 사용 하 여 `@1x` 해상도 (벡터 PDF 파일로 서식 지정)에 대 한 단일 벡터 파일을 제공 하면 파일의 `@2x` 및 `@3x` 버전이 컴파일 시간에 생성 되 고 응용 프로그램의 번들에 포함 됩니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/imageset05.png "Vector Images in the Asset Catalogs editor")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/asset8.png "Vector Images in the Asset Catalogs editor")

-----

예를 들어 개발자가 150px x 150px의 해상도를 사용 하 여 자산 카탈로그의 벡터로 `MonkeyIcon.pdf` 파일을 포함 하는 경우 컴파일된 후 다음 비트맵 자산이 최종 앱 번들에 포함 됩니다.

- `MonkeyIcon@1x.png`-150px x 150px resolution.
- `MonkeyIcon@2x.png`-300px x 300px 해상도입니다.
- `MonkeyIcon@3x.png`-450px x 450px resolution.

자산 카탈로그에서 PDF 벡터 이미지를 사용 하는 경우 다음 사항을 고려해 야 합니다.

- PDF는 컴파일 시간에 비트맵으로 래스터화됩니다. 최종 응용 프로그램에서 제공 되는 비트맵으로 래스터화됩니다.
- 이미지 크기는 자산 카탈로그에 설정 된 후에는 조정할 수 없습니다. 개발자가 이미지 크기를 조정 하려고 하면 (코드에서 또는 자동 레이아웃 및 크기 클래스를 사용 하 여) 이미지는 다른 비트맵과 마찬가지로 왜곡 됩니다.
- 자산 카탈로그는 iOS 7 이상 에서만 호환 되며, 앱이 iOS 6이 하를 지원 해야 하는 경우 자산 카탈로그를 사용할 수 없습니다.

## <a name="working-with-template-images"></a>템플릿 이미지 작업

IOS 앱의 디자인에 따라 사용자 기본 설정에 따라 색 구성표의 변경과 일치 하도록 개발자가 사용자 인터페이스 내에서 아이콘이 나 이미지를 사용자 지정 해야 하는 경우가 있을 수 있습니다.

이러한 효과를 쉽게 얻으려면 이미지 자산의 _렌더링 모드_ 를 **템플릿 이미지로**전환 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage01.png "The Render Mode set to Template Image")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage01vs.png "The Render Mode set to Template")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS 디자이너에서 이미지 자산을 UI 컨트롤에 할당 한 다음 **색조** 를 설정 하 여 이미지를 색상화 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](displaying-an-image-images/templateimage03.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](displaying-an-image-images/templateimage03vs.png "Set the Tint to colorize the image")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

필요에 따라 이미지 자산과 색조를 코드에서 직접 설정할 수 있습니다.

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

코드에서 템플릿 이미지를 완전히 사용 하려면 다음을 수행 합니다.

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

`UIImage`의 `RenderMode` 속성은 읽기 전용 이므로 `ImageWithRenderingMode` 메서드를 사용 하 여 원하는 렌더링 모드 설정을 사용 하 여 이미지의 새 인스턴스를 만듭니다.

`UIImageRenderingMode` 열거형을 통해 `UIImage.RenderMode`에 대 한 세 가지 설정이 있을 수 있습니다.

- `AlwaysOriginal`-이미지를 변경 하지 않고 원래 원본 이미지 파일로 렌더링 합니다.
- `AlwaysTemplate`-지정 된 `Tint` 색으로 픽셀의 색을 지정 하 여 이미지가 템플릿 이미지로 렌더링 되도록 합니다.
- `Automatic`-이미지를 템플릿 또는 원본으로 사용 되는 환경을 기반으로 하는 원본으로 렌더링 합니다. 예를 들어 `UIToolBar`에서 이미지를 사용 하는 경우 `UINavigationBar`, `UITabBar` 또는 `UISegmentControl` 템플릿으로 처리 됩니다.

## <a name="adding-new-assets-collections"></a>새 자산 컬렉션 추가

자산 카탈로그에서 이미지 작업을 수행 하는 경우 모든 앱 이미지를 `Assets.xcassets` 컬렉션에 추가 하는 대신 새 컬렉션이 필요한 경우가 있을 수 있습니다. 예를 들어 주문형 리소스를 디자인할 때입니다.

프로젝트에 새 자산 카탈로그를 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. **솔루션 탐색기** 에서 **프로젝트 이름을** 마우스 오른쪽 단추로 클릭 하 고 **추가** > **새 파일** ...을 선택 합니다.
2. **IOS** > **Asset Catalog**를 선택 하 고, 컬렉션의 **이름을** 입력 하 고, **새로 만들기** 단추를 클릭 합니다.

    ![](displaying-an-image-images/asset01.png "Creating a new Asset Catalog")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. 솔루션 탐색기에서 **자산 카탈로그** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가 > 새 자산 카탈로그**를 선택 합니다.
2. 이름을 지정 하 고 **추가**를 클릭 합니다.

    ![](displaying-an-image-images/asset1.png "Creating a new Asset Catalog")

-----

여기에서 컬렉션은 프로젝트에 자동으로 포함 되는 기본 `Assets.xcassets` 컬렉션과 동일한 방법으로 작업할 수 있습니다.

## <a name="using-images-with-controls"></a>컨트롤과 함께 이미지 사용

IOS는 이미지를 사용 하 여 앱을 지 원하는 것 외에도 탭 모음, 도구 모음, 탐색 모음, 테이블 및 단추와 같은 앱 컨트롤 형식이 포함 된 이미지를 사용 합니다. 컨트롤에 이미지를 표시 하는 간단한 방법은 `UIImage` 인스턴스를 컨트롤의 `Image` 속성에 할당 하는 것입니다.

### <a name="frombundle"></a>FromBundle

`FromBundle` 메서드 호출은 다양 한 해상도를 위한 캐싱 지원 및 이미지 파일 자동 처리와 같이 여러 이미지 로드 및 관리 기능을 기본 제공 하는 동기 (차단) 호출입니다.

다음 예제에서는 `UITabBar`에서 `UITabBarItem` 이미지를 설정 하는 방법을 보여 줍니다.

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

이 `MyImage`은 위의 자산 카탈로그에 추가 된 이미지 자산의 이름 이라고 가정 합니다. 자산 카탈로그 이미지를 작업할 때 **PNG** 형식의 이미지에 대 한 `FromBundle` 메서드에 설정 된 이미지의 이름만 지정 하면 됩니다.

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

다른 이미지 형식의 경우 이름으로 확장을 포함 합니다. 예를 들면,

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

아이콘 및 이미지에 대 한 자세한 내용은 [사용자 지정 아이콘 및 이미지 만들기 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)에 대 한 Apple 설명서를 참조 하세요.

## <a name="displaying-an-image-in-a-storyboards"></a>스토리 보드에 이미지 표시

자산 카탈로그를 사용 하 여 Xamarin.ios 프로젝트에 이미지를 추가한 후에는 iOS 디자이너의 `UIImageView`을 사용 하 여 스토리 보드에 쉽게 표시할 수 있습니다. 예를 들어 다음 이미지 자산이 추가 된 경우:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](displaying-an-image-images/display01.png "A sample Image Asset has been added")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. **솔루션 탐색기** 에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 편집할 수 있도록 엽니다.
2. **도구 상자**에서 **이미지 뷰** 를 선택 합니다.

     ![](displaying-an-image-images/display02.png "Select an Image View from the Toolbox")
3. 이미지 뷰를 디자인 화면으로 끌어 놓고 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03.png "A new Image View on the Design Surface")
4. **속성 탐색기** 의 **위젯** 섹션에서 표시할 **이미지** 자산을 선택 합니다.

    ![](displaying-an-image-images/display04.png "Select the desired Image asset to be displayed")
5. **보기** 섹션에서 **모드** 를 사용 하 여 **이미지 뷰의** 크기를 조정할 때 이미지 크기를 조정 하는 방법을 제어 합니다.
6. **이미지 뷰가** 선택 된 상태에서이를 다시 클릭 하 여 **제약 조건을**추가 합니다.

    ![](displaying-an-image-images/display05.png "Adding Constraints")
7. **이미지 보기** 의 각 가장자리에 대 한 "T" 모양 핸들을 화면의 해당 측면으로 끌어 이미지를 면에 "고정" 합니다. 이러한 방식으로 화면 크기를 조정 하면 **이미지 뷰가** 축소 되 고 증가 합니다.
8. 스토리 보드에 대 한 변경 내용을 저장 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](displaying-an-image-images/display01vs.png "A sample Image Asset has been added")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. **솔루션 탐색기** 에서 `Main.storyboard` 파일을 두 번 클릭 하 여 iOS 디자이너에서 편집할 수 있도록 엽니다.
2. **도구 상자**에서 **이미지 뷰** 를 선택 합니다.

     ![](displaying-an-image-images/display02vs.png "Select an Image View from the Toolbox")
3. 이미지 뷰를 디자인 화면으로 끌어 놓고 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03vs.png "A new Image View on the Design Surface")
4. **속성 탐색기** 의 **위젯** 섹션에서 표시할 **이미지** 자산을 선택 합니다.

    ![](displaying-an-image-images/display04vs.png "Select the desired Image asset to be displayed")
5. **보기** 섹션에서 **모드** 를 사용 하 여 **이미지 뷰의** 크기를 조정할 때 이미지 크기를 조정 하는 방법을 제어 합니다.
6. **이미지 뷰가** 선택 된 상태에서이를 다시 클릭 하 여 **제약 조건을**추가 합니다.

    ![](displaying-an-image-images/display05vs.png "Adding Constraints")
7. **이미지 보기** 의 각 가장자리에 대 한 "T" 모양 핸들을 화면의 해당 측면으로 끌어 이미지를 면에 "고정" 합니다. 이러한 방식으로 화면 크기를 조정 하면 **이미지 뷰가** 축소 되 고 증가 합니다.
8. 스토리 보드에 대 한 변경 내용을 저장 합니다.

-----

## <a name="displaying-an-image-in-code"></a>코드에서 이미지 표시

스토리 보드에서 이미지를 표시 하는 것과 마찬가지로, 자산 카탈로그를 사용 하 여 Xamarin.ios 프로젝트에 이미지를 추가한 후에는 코드를 사용 하 여 C# 쉽게 표시할 수 있습니다.

다음 예제를 참조하세요.

```csharp
// Create an image view that will fill the
// parent image view and set the image from
// an Image Asset

var imageView = new UIImageView (View.Frame);
imageView.Image = UIImage.FromBundle ("Kemah");

// Add the Image View to the parent view
View.AddSubview (imageView);
```

이 코드는 새 `UIImageView`를 만들어 초기 크기와 위치를 제공 합니다. 그런 다음 프로젝트에 추가 된 이미지 자산의 이미지를 로드 하 고이를 표시 하기 위해 부모 `UIView`에 `UIImageView`을 추가 합니다.

## <a name="related-links"></a>관련 링크

- [이미지 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [이미지 크기 및 해상도 (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
