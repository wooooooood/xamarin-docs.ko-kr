---
title: Xamarin.iOS 사용 하 여 이미지를 표시합니다.
description: 이 문서에서는 Xamarin.iOS에서 이미지를 표시 하는 방법을 설명 합니다. IOS 디자이너를 통해 또는 프로그래밍 방식으로 앱에 추가 이미지는 내용을 다룹니다.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/13/2018
ms.openlocfilehash: 9777b4abf6e7f370178bcff2cb40666612888a9f
ms.sourcegitcommit: cb80df345795989528e9df78eea8a5b45d45f308
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39038380"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin.iOS 사용 하 여 이미지를 표시합니다.

앱에 이미지를 추가 하려면 두 단계가 필요 합니다: 먼저 프로젝트에 이미지 추가 컨트롤 및 화면에 표시 하 여 코드를 추가 합니다. 참조를 [이미지를 사용 하 여 작업](~/ios/app-fundamentals/images-icons/index.md) 검사 Xamarin.iOS에서 처리 하는 이미지의 자세한 문서.

## <a name="adding-images-to-your-app"></a>앱에 이미지 추가

Mac 솔루션에 대 한 Visual Studio에서 모든 폴더에 이미지를 추가할 수 있습니다 및 경우에는 **빌드 작업** 로 설정 된 **콘텐츠** 파일을 앱에 포함 됩니다 하 고 표시할 수 있습니다.

Mac 용 visual Studio는 또한 라는 특수 디렉터리 지원 **리소스** 이미지 파일을 포함할 수도 있습니다. 파일 리소스 폴더에 있어야 합니다 **빌드 작업** 로 설정 **BundleResource**합니다.

이 스크린샷은 합니다 **빌드 작업** 파일에 나타나는 옵션을 마우스 오른쪽 단추로 클릭:

 [![](image-images/image30a.png "빌드 작업 메뉴")](image-images/image30a.png#lightbox)

Mac 용 visual Studio에서는 일반적으로 올바른 선택 **빌드 작업** 자동으로 있지만 프로젝트에서 파일을 이동 하는 경우에 특히 이러한 설정을 알고 있어야 합니다.

### <a name="adding-an-image-file"></a>이미지 파일 추가

이미지 파일을 프로젝트에 추가 하려면 먼저 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **파일 추가...**

 [![](image-images/image31a.png "파일 추가... 메뉴")](image-images/image31a.png#lightbox)

이미지 (또는 이미지)을 선택 하려면 표준 파일 대화 상자에 포함 합니다. 이미지에 대해 기본 빌드 동작 **BundleResource** – 특별 한 이유가 없다면이 값을 재정의 하지 않습니다.

 [![](image-images/image32a.png "파일 대화 상자 추가")](image-images/image32a.png#lightbox)

프로젝트를 로드 하 고 코드에 표시 된 사용 가능한 이미지 추가 됩니다. 이 스크린샷은 iOS 응용 프로그램 프로젝트에 추가 하는 이미지를 보여 줍니다.

 [![](image-images/image33a.png "프로젝트의 이미지")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>리소스 디렉터리 란?

파일에 배치 합니다 **리소스** 디렉터리에서 일반 파일 – 내용의 다르게 처리 됩니다 합니다 **리소스** 폴더 응용 프로그램의 루트에 복사 되 고 여기에서에서 참조할 수 있습니다 사용자 코드입니다. 이 유용할 수 있습니다 여러 가지 이유로 합니다.

-  기본 시작 이미지 및 응용 프로그램 아이콘 같은 응용 프로그램의 속성에 구성 된 이미지를 저장 합니다.
-  코드에서 별도로 다른 파일과 이미지를 저장 하므로 하기 쉽도록 관리 (하위 디렉터리는 리소스 디렉터리 콘텐츠를 복사할 때 유지 됩니다).


합니다 **리소스** 디렉터리는 이러한 이미지를 필요로 하는 쓰기 공유 코드 라이브러리를 보다 쉽게 소비 응용 프로그램의 루트에 복사할 코드 가정할 수 있으므로 라이브러리 프로젝트에서 특히 유용 이미지, 사운드, 비디오, XML 또는 기타 파일입니다.

합니다 **리소스** 디렉터리 해야 하므로 이름이 지정 되어야 하 고 모든 파일에 빌드 작업으로 설정 해야 합니다. **BundleResource**합니다.

## <a name="displaying-the-image"></a>이미지를 표시합니다.

IOS 디자이너를 사용 하 여는 **이미지 보기** 이미지 또는 애니메이션이 적용 된 일련의 이미지를 표시 합니다. 합니다 **이미지 보기** 도구 상자에서 아이콘 아래 표시 됩니다.

 [![](image-images/image35a.png "도구 상자에서 ImageView")](image-images/image35.png#lightbox)

끌어서 합니다 **이미지 보기** 에서 합니다 **도구 상자** 뷰 컨트롤러를 합니다. 그런 다음 **이미지 보기 > 이미지** 드롭 다운 목록에는 프로젝트의 모든 사용 가능한 이미지 파일의 목록을 제공 됩니다. 이미지 보기에 추가 하려면 다음 중 하나를 선택 합니다.

 [![](image-images/image36a.png "도구 상자에서 ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>이미지를 프로그래밍 방식으로 표시

때문에 **SF Monkey.jpg** 의 루트에는 **리소스** directory 응용 프로그램 번들의 루트에서 런타임에 사용할 수 있습니다. 이 이미지를 이미지 뷰 컨트롤에 표시 하려면 다음 코드를 사용 합니다.

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

이미지를 배치 했습니다 것 **SF Pics/리소스/Monkey.jpg**, 코드는 다음을 **Pics** 경로의 폴더:

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

리소스 파일 참조를 포함할 필요가 없습니다 합니다 **리소스** 폴더입니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
