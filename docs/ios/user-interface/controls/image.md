---
title: "이미지 표시"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 716189fbf1518e9100a78cc5ae64e9e63a24c949
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="displaying-images"></a>이미지 표시

앱에 이미지를 추가 하려면 두 단계가 필요 합니다: 먼저 프로젝트;에 이미지를 추가 컨트롤 및 화면에 표시 하려면 코드를 추가 합니다. 참조는 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) Xamarin.iOS에서 처리 하는 이미지의 범위 보다 자세한 문서.

## <a name="adding-images-to-your-app"></a>앱에 이미지 추가

Mac 솔루션에 대 한 Visual Studio에서 임의의 폴더에 이미지를 추가할 수 있습니다 및 경우에는 **빌드 작업** 로 설정 된 **콘텐츠** 다음 파일이 응용 프로그램과 함께 포함 되 고 표시 될 수 있습니다.

Mac 용 visual Studio는 이미지 파일에도 포함할 수 있는 리소스 라는 특수 디렉터리도 지원 합니다. 파일 리소스 폴더에는 **빌드 작업** 로 설정 **BundleResource**합니다.

이 스크린샷에서 **빌드 작업** 때 파일이 표시 되는 옵션은 마우스 오른쪽 단추로 클릭 합니다.

 [![](image-images/image30a.png "빌드 작업 메뉴")](image-images/image30a.png#lightbox)

Mac 용 visual Studio는 일반적으로 올바른 선택 **빌드 작업** 자동으로 하지만 특히 하면 파일을 이동할 프로젝트의 경우 이러한 설정을 알고 있어야 합니다.

### <a name="adding-an-image-file"></a>이미지 파일 추가

이미지 파일을 프로젝트에 추가 하려면 먼저 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **파일 추가 중...**

 [![](image-images/image31a.png "파일 추가... 메뉴")](image-images/image31a.png#lightbox)

이미지 (또는 이미지)를 선택 합니다. 표준 파일 대화 상자에 포함 하려면. 기본 빌드 작업 이미지 됩니다 **BundleResource** – 특정 이유가 없으면이 값을 무시 하지 않습니다.

 [![](image-images/image32a.png "파일 대화 상자를 추가 합니다.")](image-images/image32a.png#lightbox)

이미지 추가 됩니다. 프로젝트에 로드 되 고 코드에 표시를 사용할 수 있습니다. 이 스크린샷에서 iOS 응용 프로그램 프로젝트에 추가 하는 이미지를 보여 줍니다.

 [![](image-images/image33a.png "프로젝트에 이미지")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>리소스 디렉터리는 무엇입니까?

리소스 디렉터리에 배치 파일에서 일반 파일에서 다르게 처리 됩니다 – Resources 폴더의 내용을 응용 프로그램의 루트에 복사 되 고 코드에는 여기에서 참조 될 수 있습니다. 이 기능은 유용 여러 가지 이유로:

-  기본 시작 시 이미지 및 응용 프로그램 아이콘 등의 응용 프로그램의 속성에 구성 된 이미지를 저장 합니다.
-  다른 이미지와 별도로 코드에서 파일에 저장 되므로 더 쉽게 관리할 수 (하위 디렉터리는 리소스가 디렉터리 내용을 복사할 때 유지 됨).


Resources 디렉터리 특히 유용 라이브러리 프로젝트에서 코드 작성 이미지, 사운드, 비디오, XML 또는 기타 해야 하는 공유 코드 라이브러리를 더욱 쉽게 소비 응용 프로그램의 루트에 해당 이미지를 복사 되지 것입니다 것으로 간주할 수 있으므로 파일입니다.



Resources 디렉터리 이름을 하므로,로 모든 파일에 빌드 작업으로 설정 해야 합니다. **BundleResource**

## <a name="displaying-the-image"></a>이미지를 표시합니다.

디자이너를 사용 하 여 이미지를 표시 하려면 이미지 보기는 컨테이너로 사용 해야 하며 단일 이미지 또는 이미지의 애니메이션 표시할 수 있습니다. **이미지 보기** 도구 상자의 아이콘은 다음과 같습니다.

 [![](image-images/image35a.png "도구 상자에 ImageView")](image-images/image35.png#lightbox)

끌어서는 **이미지 보기** 에서 **Toobox** 보기 컨트롤러에 있습니다. 다음 * * 이미지 보기 > 이미지 * * 드롭 다운 목록에는 프로젝트의 모든 사용 가능한 이미지 파일의 목록을 제공 합니다. 이러한 이미지 보기에 추가할 항목 중 하나를 선택 합니다.

 [![](image-images/image36a.png "도구 상자에 ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>이미지를 프로그래밍 방식으로 표시

Blocks.jpg Resources 디렉터리의 루트에 있기 때문에 응용 프로그램 번들의 루트에서 런타임에 사용할 수 있습니다. 이 이미지는 ImageView에 표시할 컨트롤 다음 코드를 사용 합니다.

```csharp
imageview1.Image = UIImage.FromBundle ("SF Monkey.png");
```

에서는 이미지를 배치 하는 경우 `/Resources/Pics/blocks.jpg` 코드 경로에 Pics 폴더를 포함 합니다.

```csharp
imageview1.Image = UIImage.FromBundle ("Pics/SF Monkey.png");
```

리소스 파일 참조를 포함할 필요가 없습니다는 `Resources` 폴더입니다.


## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://developer.xamarin.com/samples/Controls/)
