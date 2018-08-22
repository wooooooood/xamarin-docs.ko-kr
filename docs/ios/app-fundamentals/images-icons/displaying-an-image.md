---
title: Xamarin.iOS에서 이미지를 표시합니다.
description: 이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너에서에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다.
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/24/2018
ms.openlocfilehash: 4b2bddeb6b04b5c5288f501fce0d6bb03e0b6584
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251188"
---
# <a name="displaying-an-image-in-xamarinios"></a>Xamarin.iOS에서 이미지를 표시합니다.

_이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너에서에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다._

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>추가 하 고 Xamarin.iOS 앱에 이미지를 구성 합니다.

개발자가 사용 하 여 Xamarin.iOS 앱에 사용할 이미지를 추가 하는 경우는 _자산 카탈로그_ 모든 iOS 장치 및 앱에 필요한 해상도 지원 하도록 합니다.

IOS 7에서에서 추가한 **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지원 하 고 앱에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 합니다. 이미지 자산 파일 이름에 의존 하지 않고 (참조 [해상도 독립적인 이미지 및 이미지 명명법](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **이미지 집합** Json 파일을 사용 하 여 이미지는 장치 및/또는 해결 속한 지정 . 이것이 (iOS 9 이상)에서 iOS에서 이미지를 지원 및 관리 하는 기본 방법입니다.

## <a name="adding-images-to-an-asset-catalog-image-set"></a>자산 카탈로그 이미지에 이미지 추가 설정

위에서 설명한 것 처럼를 **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지원 하 고 앱에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 합니다. 이미지 자산 파일 이름에 의존 하지 않고 **이미지 집합** Json 파일을 사용 하 여 이미지는 장치 및/또는 해결 속한 지정 합니다.

새 이미지 집합을 만들고 이미지를 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 탐색기**, 두 번 클릭 합니다 `Assets.xcassets` 파일을 편집 하는 것에 대 한 열:

    ![](displaying-an-image-images/imageset01.png "솔루션 탐색기에서 Assets.xcassets")
2. 마우스 오른쪽 단추로 클릭 합니다 **자산 목록** 선택한 **새 이미지 집합**:

    ![](displaying-an-image-images/imageset02.png "새 이미지 집합 추가")
3. 새 이미지 집합을 선택 하 고 편집기를 표시 됩니다.

    ![](displaying-an-image-images/imageset03.png "이미지 집합 편집기")
4. 여기에서 끌어 이미지의 각 다른 장치에 대 한 및 및 필요한 해결 방법입니다. 
5. 새 이미지 집합을 두 번 클릭 **이름을** 에 **자산 목록** 편집 하려면: ![](displaying-an-image-images/imageset04.png "새 이미지 집합의 이름 편집")

사용 하는 경우는 **집합 이미지** ios 디자이너에서 단순히 집합의 이름을 선택 속성 편집기에서 드롭다운 목록에서:

![](displaying-an-image-images/imageset06.png "이미지 집합의 이름을 드롭다운 목록에서 선택")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 자산 카탈로그를 엽니다는 **솔루션 탐색기**, 왼쪽된 위 모퉁이 클릭 합니다 **Plus** 단추:

    ![](displaying-an-image-images/asset5.png "더하기를 클릭 합니다. 단추")

2. 선택 **이미지 집합 추가** 집합 이미지 편집기 새 이미지 집합에 대 한 표시 됩니다. 여기에서 끌어 이미지의 각 다른 장치에 대 한 및 및 필요한 해결 방법입니다. 

    ![](displaying-an-image-images/asset7.png "이미지 편집기 설정")

### <a name="renaming-an-image-set"></a>이미지 집합 이름 바꾸기

이미지 집합을 바꾸려면 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭 합니다 **자산 카탈로그** 파일을 편집 하는 것에 대 한 열:

    ![](displaying-an-image-images/rename01.png "솔루션 탐색기에서 자산 카탈로그")
2. 선택 된 **집합 이미지** 이름을 바꾸려면:

    ![](displaying-an-image-images/rename02.png "이름을 바꾸려면 이미지 집합 선택")
3. 에 **속성 탐색기**, 아래로 스크롤하여 선택 **이름**(아래 합니다 **기타** 섹션):

    ![](displaying-an-image-images/rename03.png "기타 섹션에서 이름을 선택합니다")
4. 새 입력 **이름을** 에 대 한 합니다 **집합 이미지** 변경 내용을 저장 합니다.

-----

사용 하는 경우는 **집합 이미지** 코드에서 이름으로 참조를 호출 하 여 합니다 `FromBundle` 메서드를 `UIImage` 클래스. 예를 들면 다음과 같습니다.

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> 이미지 집합에 할당 된 이미지 올바르게 표시 되지 않으면, 올바른 파일 이름을 사용 하 여 사용 되 고 있는지 확인 합니다 `FromBundle` 메서드 (합니다 **집합 이미지** 부모가 아니라 **자산 카탈로그** 이름). PNG 이미지는 `.png` 확장을 생략할 수 있습니다. 다른 이미지 형식의 확장이 필요 (예: `PurpleMonkey.jpg`).

### <a name="using-vector-images-in-asset-catalogs"></a>자산 카탈로그에서 벡터 이미지 사용

IOS 8, 특별 한 기준으로 **벡터** 클래스에 추가 되었습니다 **이미지 집합** 개발자가 포함할 수 있도록를 **PDF** 대신 포함 하 여 카세트에 벡터 이미지 형식 다른 해상도로 개별 비트맵 파일입니다. 에 대 한 단일 벡터 파일 제공이 메서드를 사용 하는 `@1x` 해결 방법 (벡터 PDF 파일 형식) 및 `@2x` 및 `@3x` 버전 파일의 컴파일 시간에 생성 되며 응용 프로그램의 번들에 포함 된 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "자산 카탈로그 편집기에서 벡터 이미지")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "자산 카탈로그 편집기에서 벡터 이미지")

-----

예를 들어, 개발자를 포함 하는 경우는 `MonkeyIcon.pdf` 150px x 150px 컴파일할 때 자산 최종 앱 번들에 포함 되어 다음과 같은 비트맵의 해상도로 자산 카탈로그의 벡터 파일:

- `MonkeyIcon@1x.png` -150px x 150px 확인 합니다.
- `MonkeyIcon@2x.png` -300px x 300px 확인 합니다.
- `MonkeyIcon@3x.png` -450px x 450px 확인 합니다.

다음은 고려해 야 자산 카탈로그에서 PDF 벡터 이미지를 사용 하는 경우:

- PDF는 컴파일 시간 및 최종 응용 프로그램에서 제공 하는 비트맵 비트맵 래스터화된 있을 전체 벡터 지원 되지 않습니다.
- 자산 카탈로그에 설정한 후에 이미지의 크기를 조정할 수 없습니다. 개발자 (또는 코드에서 자동 레이아웃 및 Size 클래스를 사용 하 여) 이미지 크기를 조정 하는 경우 다른 비트맵 마찬가지로 이미지 왜곡 됩니다.
- 자산 카탈로그는 호환 ios 7 이상, 경우에 앱 iOS 6을 지 원하는 데 필요한 또는 이하인 자산 카탈로그를 사용할 수 없습니다.

## <a name="working-with-template-images"></a>템플릿 이미지를 사용 하 여 작업

IOS 앱의 디자인에 따라 경우도 개발자 아이콘 또는 이미지 (예: 사용자 기본 설정에 따라) 색 구성표 변경에 맞게 사용자 인터페이스 내에서 사용자 지정 해야 할 경우.

이 효과 쉽게 달성할 수 있도록 전환 합니다 _렌더링 모드_ 이미지 자산의 **템플릿 이미지**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "템플릿 이미지에는 렌더링 모드 설정")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "렌더링 모드를 사용 하는 템플릿 설정")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS 디자이너에서에서 UI 컨트롤에 이미지 자산 할당을 설정 합니다 **Tint** 이미지 색을 지정 하려면:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "이미지 색을 지정 하려면 색조를 설정 합니다.")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "이미지 색을 지정 하려면 색조를 설정 합니다.")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

필요에 따라 이미지 자산 및 Tint 코드에서 직접 설정할 수 있습니다.

```csharp
MyIcon.Image = UIImage.FromBundle ("MessageIcon");
MyIcon.TintColor = UIColor.Red;
```

코드에서 완전히 템플릿 이미지를 사용 하려면 다음을 수행 합니다.

```csharp
if (MyIcon.Image != null) {
    var mutableImage = MyIcon.Image.ImageWithRenderingMode (UIImageRenderingMode.AlwaysTemplate);
    MyIcon.Image = mutableImage;
    MyIcon.TintColor = UIColor.Red;
}
```

때문에 합니다 `RenderMode` 의 속성을 `UIImage` 읽기 전용을 사용 하 여는 `ImageWithRenderingMode` 원하는 렌더링 모드 설정을 사용 하 여 이미지의 새 인스턴스를 만드는 방법.

세 가지에 대 한 설정 가능한 `UIImage.RenderMode` 를 통해를 `UIImageRenderingMode` 열거형:

* `AlwaysOriginal` -이미지를 변경 하지 않고 원래 소스 이미지 파일로 렌더링 되도록 합니다.
* `AlwaysTemplate` -지정 된 픽셀 색을 지정 하 여 템플릿 이미지를 렌더링할 이미지 강제로 `Tint` 색입니다.
* `Automatic` -하나에 사용 되는 환경에 따라 템플릿 또는 원래 대로 이미지를 렌더링 합니다. 예를 들어, 이미지에서 사용 되는 경우는 `UIToolBar`, `UINavigationBar`를 `UITabBar` 또는 `UISegmentControl` 템플릿으로 간주 됩니다.

## <a name="adding-new-assets-collections"></a>새 자산 컬렉션 추가

자산 카탈로그의 이미지를 사용 하 여 작업할 때 경우가 있을 때 새 컬렉션은 모든 앱의 이미지를 추가 하는 대신 필요한 수의 `Assets.xcassets` 컬렉션입니다. 예를 들어, 주문형 리소스를 디자인할 때.

프로젝트에 새 자산 카탈로그 추가:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마우스 오른쪽 단추로 클릭 합니다 **프로젝트 이름** 에 **솔루션 탐색기** 선택한 **추가** > **새 파일...**
2. 선택 **iOS** > **자산 카탈로그**, 입력을 **이름** 컬렉션을 클릭 합니다 **새** 단추:

    ![](displaying-an-image-images/asset01.png "새 자산 카탈로그를 만들려면")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 **자산 카탈로그** 선택한 폴더 **추가 > 새 자산 카탈로그**합니다.
2. 이름을 지정 하 고 클릭 **추가**:

    ![](displaying-an-image-images/asset1.png "새 자산 카탈로그를 만들려면")

-----

여기에서 컬렉션 작업할 수와 동일한 방식으로 기본 `Assets.xcassets` 프로젝트에 자동으로 포함 하는 컬렉션입니다.

## <a name="using-images-with-controls"></a>컨트롤을 사용 하 여 이미지를 사용 하 여

이미지를 사용 하 여 앱을 지원 하기 위해, 하는 것 외에도 iOS 탭 표시줄, 도구 모음, 탐색 모음, 테이블 및 단추와 같은 앱 컨트롤 형식을 사용 하 여 이미지도 사용 합니다. 컨트롤에 표시할 이미지를 확인 하는 간단한 방법을 할당 하는 것을 `UIImage` 컨트롤의 인스턴스 `Image` 속성입니다.

### <a name="frombundle"></a>FromBundle

`FromBundle` 메서드 호출 다양 한 캐싱 지원 및 다양 한 해상도 대 한 이미지 파일의 자동 처리와 같은 기본 제공 하는 이미지 로드 하 고 관리 기능에는 동기 (블로킹) 호출을 의미 합니다.  

다음 예제에서는 이미지를 설정 하는 방법을 보여 줍니다는 `UITabBarItem` 에 `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

가정 `MyImage` 위의 자산 카탈로그를 추가할 이미지 자산의 이름입니다. 자산 카탈로그 이미지를 사용할 때의 이미지 집합의 이름을 지정 합니다 `FromBundle` 에 대 한 메서드 **PNG** 이미지 형식:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

다른 이미지 형식을 위한 이름 확장명을 포함 합니다. 예를 들어:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

아이콘 및 이미지에 대 한 자세한 내용은 Apple 설명서를 참조 [사용자 지정 아이콘 및 이미지 만들기 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)합니다.

## <a name="displaying-an-image-in-a-storyboards"></a>스토리 보드에 이미지를 표시합니다.

이미지에는 자산 카탈로그를 사용 하 여 Xamarin.iOS 프로젝트에 추가 된 후 쉽게 수 사용 하 여 스토리 보드에 표시 되는 `UIImageView` iOS 디자이너에서에서 합니다. 예를 들어, 다음 이미지 자산에 추가 되었습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "샘플 이미지 자산 추가 되었습니다.")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. 두 번 클릭 합니다 `Main.storyboard` 파일 합니다 **솔루션 탐색기** 하 여 iOS 디자이너에서에서 편집을 위해 엽니다.
2. 선택는 **뷰 이미지** 에서 합니다 **도구 상자**:

     ![](displaying-an-image-images/display02.png "도구 상자에서 이미지 뷰를 선택 합니다.")
3. 이미지 뷰를 끌어 디자인 화면 위치 및 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03.png "디자인 화면에서 새 이미지 보기")
4. 에 **위젯** 섹션을 **속성 탐색기** 원하는 **이미지** 표시할 자산:

    ![](displaying-an-image-images/display04.png "표시할 원하는 이미지 자산 선택")
5. 에 **보기** 섹션을 사용 하 여는 **모드** 이미지 되는 방식을 제어 하 크기를 조정할 때를 **이미지 보기** 크기가 조정 됩니다.
6. 사용 하 여 합니다 **이미지 보기** 클릭 하 여 추가 다시 선택 하면 **제약 조건**:

    ![](displaying-an-image-images/display05.png "제약 조건 추가")
7. 각 가장자리에 "T" 모양 핸들을 끌어 합니다 **이미지 보기** 측면에 이미지를 "고정" 하는 화면의 해당 쪽에 있습니다. 이러한 방식으로 **이미지 보기** 축소 되며 화면 크기가 증가 합니다.
8. 스토리 보드에 변경 내용을 저장 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "샘플 이미지 자산 추가 되었습니다.")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. 두 번 클릭 합니다 `Main.storyboard` 파일 합니다 **솔루션 탐색기** 하 여 iOS 디자이너에서에서 편집을 위해 엽니다.
2. 선택는 **뷰 이미지** 에서 합니다 **도구 상자**:

     ![](displaying-an-image-images/display02vs.png "도구 상자에서 이미지 뷰를 선택 합니다.")
3. 이미지 뷰를 끌어 디자인 화면 위치 및 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03vs.png "디자인 화면에서 새 이미지 보기")
4. 에 **위젯** 섹션을 **속성 탐색기** 원하는 **이미지** 표시할 자산:

    ![](displaying-an-image-images/display04vs.png "표시할 원하는 이미지 자산 선택")
5. 에 **보기** 섹션을 사용 하 여는 **모드** 이미지 되는 방식을 제어 하 크기를 조정할 때를 **이미지 보기** 크기가 조정 됩니다.
6. 사용 하 여 합니다 **이미지 보기** 클릭 하 여 추가 다시 선택 하면 **제약 조건**:

    ![](displaying-an-image-images/display05vs.png "제약 조건 추가")
7. 각 가장자리에 "T" 모양 핸들을 끌어 합니다 **이미지 보기** 측면에 이미지를 "고정" 하는 화면의 해당 쪽에 있습니다. 이러한 방식으로 **이미지 보기** 축소 되며 화면 크기가 증가 합니다.
8. 스토리 보드에 변경 내용을 저장 합니다.

-----

## <a name="displaying-an-image-in-code"></a>코드에서 이미지를 표시합니다.

스토리 보드에 이미지 표시와 마찬가지로 이미지는 자산 카탈로그를 사용 하 여 Xamarin.iOS 프로젝트에 추가 되 면 표시 될 수 쉽게 C# 코드를 사용 하 여 합니다.

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

이 코드에서는 새 `UIImageView` 초기 크기와 위치를 제공 합니다. 프로젝트에 추가 이미지 자산에서 이미지를 로드 하 고 추가 합니다 `UIImageView` 부모 `UIView` 표시 합니다.

## <a name="related-links"></a>관련 링크

- [이미지 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [이미지 크기 및 해상도 (Apple)](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/image-size-and-resolution/)
