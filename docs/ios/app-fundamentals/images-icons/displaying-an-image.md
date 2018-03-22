---
title: "이미지 표시"
description: "이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너의에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 60288B12-49E3-4E87-8690-D04A5EC7A664
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 39da41b7fb5118a16f2b2953f8fcb0a5b72aa819
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/22/2018
---
# <a name="displaying-an-image"></a>이미지 표시

_이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너의에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다._

이 문서에서 자세히 설명하는 항목은 다음과 같습니다.

- [Xamarin.iOS 앱에서 이미지를 구성 및 추가](#adding-assets) -이미지 자산에 설명 하 고 구성 하 고 Xamarin.iOS 프로젝트 내부 관리 되는 방식을 포함 될 수 있습니다.
- [자산 카탈로그에 이미지 추가](#asset-catalogs) -자산 카탈로그를 사용 하 여 이미지를 관리 합니다.
    - [벡터 이미지를 사용 하 여 자산 카탈로그의](#Using-Vector-Images-in-Asset-Catalogs) -단일 벡터에 모든 이미지 크기를 제공 합니다.
- [템플릿 이미지 작업](#Working-with-Template-Images) -템플릿 이미지를 이미지 자산을 설정 하 여 개발자 수 쉽게 색을 지정 된 이미지를 설정 하 여 응용 프로그램의 UI 테마 변경 내용과 일치 하도록 `Tint` 속성입니다.
- [이미지를 사용 하 여 컨트롤과](#controls) -와 같은 UI 컨트롤을 사용 하 여 Xamarin.iOS 프로젝트에 포함 된 이미지 자산을 사용 하 여 적용 됩니다. `UIButton` 및 `UIImageView` 및 C# 사용 하 여 이미지와 함께 작업 하는 방법의 `UIImage` 개체의 [FromBundle](#frombundle) 메서드.
- [이미지는 스토리 보드에 표시](#Displaying-an-Image-in-a-Storyboards) -스토리 보드를 사용 하 여 이미지를 표시 하는의 예제를 제공 합니다.
- [코드에서 이미지를 표시](#Displaying-an-Image-in-Code) -C# 코드를 사용 하 여 이미지를 표시 하는의 예제를 제공 합니다.

<a name="adding-assets" />

## <a name="adding-and-organizing-images-in-a-xamarinios-app"></a>추가 및 Xamarin.iOS 앱에서 이미지를 구성 합니다.

개발자 ´ ֲ Xamarin.iOS 앱에서 사용할 이미지를 추가 하는 경우는 _자산 카탈로그_ 모든 iOS 장치 및 앱에 필요한 해상도 지원 하도록 합니다.

IOS 7에 추가 된 **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지 원하는 응용 프로그램에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 합니다. 이미지 자산 파일 이름에 의존 하지 않고 (참조 [해상도 독립적인 이미지와 이미지 명명법](~/ios/app-fundamentals/images-icons/displaying-an-image.md)), **이미지 집합** Json 파일을 사용 하 여 어떤 장치 및/또는 해결 하는 이미지로 속하는 지정 . 이 관리 및 ios (iOS 9 이상)에서 이미지를 지원 하는 것이 좋습니다.

<a name="asset-catalogs" />

## <a name="adding-images-to-an-asset-catalog-image-set"></a>자산 카탈로그 이미지에 추가 이미지 설정

위에서 설명 했 듯이 **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지 원하는 응용 프로그램에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 합니다. 이미지 자산 파일 이름에 의존 하지 않고 **이미지 집합** Json 파일을 사용 하 여 어떤 장치 및/또는 해결 하는 이미지로 속하는 지정 합니다.

새 이미지 집합을 만들고 이미지를 추가 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 에 **솔루션 탐색기**, 두 번 클릭는 `Assets.xcassets` 편집을 위해 열 파일입니다.

    ![](displaying-an-image-images/imageset01.png "솔루션 탐색기에서 Assets.xcassets")
2. 마우스 오른쪽 단추로 클릭는 **자산 목록** 선택 **새 이미지 집합**:

    ![](displaying-an-image-images/imageset02.png "새 이미지 집합 추가")
3. 새 이미지 집합을 선택 하 고 편집기를 표시 됩니다.

    ![](displaying-an-image-images/imageset03.png "이미지 집합 편집기")
4. 여기에서 끌어 이미지의 다양 한 장치 각각에 대해 및 및 해상도 필요 합니다. (참고: 이러한 해결 방법에 지정 된 해상도에 일치 하는 [이미지 크기와 파일 이름을](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 문서입니다.)
5. 새 이미지 집합을 두 번 클릭 **이름** 에 **자산 목록** 편집할: ![ ] (displaying-an-image-images/imageset04.png "새 이미지 집합의 이름을 편집 합니다.")

사용 하는 경우는 **이미지 설정** iOS 디자이너에에서 단순히 집합의 이름을 목록에서 선택 드롭다운 속성 편집기에서:

![](displaying-an-image-images/imageset06.png "드롭다운 목록에서 이미지 집합의 이름을 선택합니다")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Asset Catalog에서 열고는 **솔루션 탐색기**, 왼쪽된 위 모서리에서 클릭 하 고는 **플러스** 단추:

    ![](displaying-an-image-images/asset5.png "더하기 클릭 단추")
2. 선택 **이미지 집합 추가** 새 이미지 집합에 대 한 이미지 설정 편집기를 표시 됩니다. 여기에서 끌어 이미지의 다양 한 장치 각각에 대해 및 및 해상도 필요 합니다. (참고: 이러한 해결 방법에 지정 된 해상도에 일치 하는 [이미지 크기와 파일 이름을](~/ios/app-fundamentals/images-icons/displaying-an-image.md) 문서):

    ![](displaying-an-image-images/asset7.png "이미지 편집기 설정")

### <a name="renaming-an-image-set"></a>이미지 집합 이름 바꾸기

설정 하는 이미지의 이름을 바꾸려면를 다음을 수행 합니다.

1. 에 **솔루션 탐색기**, 두 번 클릭는 **자산 카탈로그** 편집을 위해 열 파일입니다.

    ![](displaying-an-image-images/rename01.png "솔루션 탐색기에서 자산 카탈로그의")
2. 선택 된 **이미지 설정** 이름을 바꾸려면:

    ![](displaying-an-image-images/rename02.png "이름 바꾸기로 이미지 설정 선택")
3. 에 **속성 탐색기**, 아래쪽으로 스크롤하여 선택 **이름**(아래에서 **기타** 섹션):

    ![](displaying-an-image-images/rename03.png "기타 섹션 아래에서 이름을 선택합니다")
4. 새 입력 **이름** 에 대 한는 **이미지 설정** 변경 내용을 저장 합니다.

-----

사용 하는 경우는 **이미지 설정** 코드에서 이름으로 참조를 호출 하 여는 `FromBundle` 의 메서드는 `UIImage` 클래스입니다. 예를 들면 다음과 같습니다.

```csharp
MonkeyImage.Image = UIImage.FromBundle ("PurpleMonkey");
```

> [!IMPORTANT]
> 설정 하는 이미지에 할당 된 이미지 올바르게 표시 되지 않으면, 올바른 파일 이름으로 사용 되 고 있는지 확인는 `FromBundle` 메서드 (에서 **이미지 설정** 부모가 아니라 **자산 카탈로그** 이름). PNG 이미지는 `.png` 확장을 생략할 수 있습니다. 다른 이미지 형식에 대 한 확장은 필요한 않음 (예: `PurpleMonkey.jpg`).

<a name="Using-Vector-Images-in-Asset-Catalogs" />

### <a name="using-vector-images-in-asset-catalogs"></a>자산 카탈로그의 벡터 이미지를 사용합니다.

IOS 8, 특별 한 당시 **벡터** 클래스에 추가 된 **이미지 집합** 개발자 포함 하도록 허용 하는 **PDF** 벡터 이미지를 포함 하 여 대신 카세트 포맷 다른 해상도로 개별 비트맵 파일입니다. 에 대 한 단일 벡터 파일 제공이 메서드를 사용 하는 `@1x` 해상도 (벡터 PDF 파일 형식) 및 `@2x` 및 `@3x` 파일의 버전 컴파일 타임에 생성 되며 응용 프로그램의 번들에 포함 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/imageset05.png "자산 카탈로그 편집기에서 벡터 이미지")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/asset8.png "자산 카탈로그 편집기에서 벡터 이미지")

-----

예를 들어, 개발자가 포함 된 경우는 `MonkeyIcon.pdf` 150px x 150px, 자산 컴파일할 때 최종 응용 프로그램 번들에 포함 되어 다음과 같은 비트맵의 해결 방법을 통해 벡터 자산 카탈로그의 파일:

- `MonkeyIcon@1x.png` -150px x 150px 확인 합니다.
- `MonkeyIcon@2x.png` -300px x 300px 확인 합니다.
- `MonkeyIcon@3x.png` -450px x 450px 확인 합니다.

다음은 고려해 야 자산 카탈로그의 PDF 벡터 이미지를 사용 하는 경우:

- PDF는 컴파일 시간 및 최종 응용 프로그램에서 제공 하는 비트맵에 비트맵 래스터화된 있을 전체 벡터 지원 되지 않습니다.
- 자산 카탈로그에 설정 되 면 이미지의 크기를 조정할 수 없습니다. 개발자 (또는 코드에서 자동 레이아웃 및 크기 클래스를 사용 하 여) 이미지 크기 조정 하려고 할 경우 다른 비트맵 마찬가지로 이미지가 왜곡 될 됩니다.
- 자산 카탈로그는만 호환 ios 7 이상 또는 경우에 응용 프로그램 iOS 6을 지 원하는 데 필요한 이하인 자산 카탈로그를 사용할 수 없습니다.

<a name="Working-with-Template-Images" />

## <a name="working-with-template-images"></a>템플릿 이미지 작업

IOS 앱의 디자인에 따라, 경우도 아이콘 또는 변경 (예: 사용자 기본 설정에 따라) 색 구성표에 일치 하는 사용자 인터페이스 내에서 이미지를 사용자 지정 하는 개발자 필요로 하는 경우.

이 작업을 쉽게 수행 하려면 전환는 _렌더링 모드_ 이미지 자산의 **템플릿 이미지**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage01.png "템플릿 이미지에 렌더링 모드 설정")](displaying-an-image-images/templateimage01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage01vs.png "서식 파일을 렌더링할 모드 설정")](displaying-an-image-images/templateimage01vs.png#lightbox)

-----

IOS 디자이너에서에서 이미지 자산 UI 컨트롤을 할당 한 다음 설정의 **Tint** 이미지 색을 지정 하려면:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](displaying-an-image-images/templateimage03.png "Tint 색상을 지정 하는 이미지 설정")](displaying-an-image-images/templateimage03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](displaying-an-image-images/templateimage03vs.png "Tint 색상을 지정 하는 이미지 설정")](displaying-an-image-images/templateimage03vs.png#lightbox)

-----

필요에 따라 코드에서 직접 이미지 자산 및 Tint을 설정할 수 있습니다.

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

때문에 `RenderMode` 속성은 `UIImage` 읽기 전용으로 사용 되는 `ImageWithRenderingMode` 원하는 렌더링 모드 설정을 사용 하 여 이미지의 새 인스턴스를 만드는 메서드를 합니다.

세 가지에 대 한 가능한 설정 `UIImage.RenderMode` 통해는 `UIImageRenderingMode` enum:

* `AlwaysOriginal` -이미지를 변경 하지 않고 원래 원본 이미지 파일도로 렌더링 되도록 합니다.
* `AlwaysTemplate` -지정 된 픽셀의 색을 지정 하 여 템플릿 이미지 형식으로 렌더링 될 이미지를 강제로 `Tint` 색입니다.
* `Automatic` -하나에 사용 되는 환경에 따라 서식 파일이 나 원래 대로 이미지를 렌더링 합니다. 예를 들어, 이미지에 사용 되는 경우는 `UIToolBar`, `UINavigationBar`, `UITabBar` 또는 `UISegmentControl` 서식 파일로 처리 됩니다.

<a name="Adding-new-Assets-Collections" />

## <a name="adding-new-assets-collections"></a>새 자산 컬렉션 추가

자산 카탈로그에는 이미지와 함께 작업 하는 경우 모든 응용 프로그램의 이미지를 추가 하는 대신 필요한 수 있는 새 컬렉션을 시점 경우가 있을 수 있습니다는 `Assets.xcassets` 컬렉션입니다. 예를 들어 요청 시 리소스를 디자인 하는 경우.

에 추가 하려면 새 자산 카탈로그의 프로젝트:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 마우스 오른쪽 단추로 클릭는 **프로젝트 이름** 에 **솔루션 탐색기** 선택 **추가** > **새 파일...**
2. 선택 **iOS** > **자산 카탈로그**를 입력 한 **이름** 를 수집 하 고 클릭에 대 한는 **새로** 단추:

    ![](displaying-an-image-images/asset01.png "새 자산 카탈로그 만들기")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 **자산 카탈로그** 폴더를 찾아 선택 **추가 > 새 자산 카탈로그**합니다.
2. 이름을 지정 하 고 클릭 **추가**:

    ![](displaying-an-image-images/asset1.png "새 자산 카탈로그 만들기")

-----


여기에서 컬렉션 작동할 수와 같은 방식으로 기본 `Assets.xcassets` 프로젝트에 자동으로 포함 하는 컬렉션입니다.

<a name="controls" />

## <a name="using-images-with-controls"></a>이미지를 사용 하 여 컨트롤과

이미지를 사용 하 여 응용 프로그램을 지 원하는 iOS 막대 탭, 도구 모음, 탐색 모음, 테이블 및 단추와 같은 응용 프로그램 컨트롤 형식과 함께 이미지도 사용 합니다. 컨트롤에 표시할 이미지를 확인 하는 간단한 방법을 할당 하는 한 `UIImage` 를 컨트롤의 인스턴스 `Image` 속성입니다.

<a name="frombundle" />

### <a name="frombundle"></a>FromBundle

`FromBundle` 메서드 호출에 다양 한 이미지 로드 하 고 관리 기능 지원 및 다양 한 해상도 대 한 이미지 파일의 자동 처리 캐싱과 같은 기본 제공 하는 동기 (블로킹) 호출 됩니다.  

이미지를 설정 하는 방법을 보여 주는 다음 예제는 `UITabBarItem` 에 `UITabBar`:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

있다고 가정할 경우 `MyImage` 위의 자산 카탈로그에 추가 된 이미지 자산의 이름입니다. 이미지 설정의 이름을 지정 자산 카탈로그 이미지를 사용할 때는 `FromBundle` 방법을 **PNG** 이미지 형식:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage");
```

다른 이미지 형식에 대 한 이름으로 확장명을 포함 합니다. 예를 들어:

```csharp
TabBarItem.Image = UIImage.FromBundle ("MyImage.jpg");
```

아이콘 및 이미지에 대 한 자세한 내용은 Apple 설명서를 참조에 [사용자 지정 아이콘 및 이미지 생성 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)합니다.

<a name="Displaying-an-Image-in-a-Storyboards" />

## <a name="displaying-an-image-in-a-storyboards"></a>이미지는 스토리 보드에 표시

자산 카탈로그를 사용 하 여 Xamarin.iOS 프로젝트에 이미지를 추가한 후 쉽게 수 사용 하 여 스토리 보드에 표시 되는 `UIImageView` 디자이너는 ios입니다. 예를 들어 다음 이미지 자산에 추가 되었습니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

![](displaying-an-image-images/display01.png "샘플 이미지 자산 추가 되었습니다.")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. 두 번 클릭은 `Main.storyboard` 파일에 **솔루션 탐색기** iOS 디자이너에서에서 편집 하기 위해 열려는 합니다.
2. 선택 된 **이미지 보기** 에서 **도구 상자**:

     ![](displaying-an-image-images/display02.png "도구 상자에서 이미지 뷰를 선택 합니다.")
3. 이미지 뷰를 끌어 디자인 화면 위치 고 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03.png "디자인 화면에서 새 이미지 보기")
4. 에 **위젯** 의 섹션은 **속성 탐색기** 원하는 **이미지** 자산을 표시:

    ![](displaying-an-image-images/display04.png "표시할 원하는 이미지 자산을 선택 합니다.")
5. 에 **보기** 섹션에서 사용 하 여는 **모드** 이미지 되는 방식을 제어할 수 크기를 조정할 때는 **이미지 보기** 크기가 조정 됩니다.
6. 와 **이미지 보기** 선택한을 추가 하려면 다시 클릭 **제약 조건을**:

    ![](displaying-an-image-images/display05.png "제약 조건 추가")
7. 각 가장자리에 "T" 모양 핸들을 끌어는 **이미지 보기** 해당 "고정" 측면에서 이미지를 화면의 측면에 있습니다. 이러한 방식으로 **이미지 보기** 화면 크기 조정에 따라 확장 해 서 축소 됩니다.
8. 스토리 보드의 변경 내용을 저장 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![](displaying-an-image-images/display01vs.png "샘플 이미지 자산 추가 되었습니다.")

스토리 보드에 표시 하려면 다음을 수행 합니다.

1. 두 번 클릭은 `Main.storyboard` 파일에 **솔루션 탐색기** iOS 디자이너에서에서 편집 하기 위해 열려는 합니다.
2. 선택 된 **이미지 보기** 에서 **도구 상자**:

     ![](displaying-an-image-images/display02vs.png "도구 상자에서 이미지 뷰를 선택 합니다.")
3. 이미지 뷰를 끌어 디자인 화면 위치 고 필요에 따라 크기를 조정 합니다.

    ![](displaying-an-image-images/display03vs.png "디자인 화면에서 새 이미지 보기")
4. 에 **위젯** 의 섹션은 **속성 탐색기** 원하는 **이미지** 자산을 표시:

    ![](displaying-an-image-images/display04vs.png "표시할 원하는 이미지 자산을 선택 합니다.")
5. 에 **보기** 섹션에서 사용 하 여는 **모드** 이미지 되는 방식을 제어할 수 크기를 조정할 때는 **이미지 보기** 크기가 조정 됩니다.
6. 와 **이미지 보기** 선택한을 추가 하려면 다시 클릭 **제약 조건을**:

    ![](displaying-an-image-images/display05vs.png "제약 조건 추가")
7. 각 가장자리에 "T" 모양 핸들을 끌어는 **이미지 보기** 해당 "고정" 측면에서 이미지를 화면의 측면에 있습니다. 이러한 방식으로 **이미지 보기** 화면 크기 조정에 따라 확장 해 서 축소 됩니다.
8. 스토리 보드의 변경 내용을 저장 합니다.

-----


<a name="Displaying-an-Image-in-Code" />

## <a name="displaying-an-image-in-code"></a>코드에서 이미지를 표시합니다.

스토리 보드의 이미지를 표시 하는 것 처럼 이미지 자산 카탈로그를 사용 하 여 Xamarin.iOS 프로젝트에 추가 되 면 표시 될 수 쉽게 C# 코드를 사용 하 여 합니다.

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

이 코드에서는 새 `UIImageView` 초기 크기와 위치를 제공 합니다. 프로젝트에 추가 하는 이미지 자산에서 이미지를 로드 하 고 추가 `UIImageView` 부모 `UIView` 표시 합니다.

## <a name="related-links"></a>관련 링크

- [이미지 (샘플) 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
