---
title: Xamarin.ios를 사용 하 여 이미지 표시
description: 이 문서에서는 Xamarin.ios에 이미지를 표시 하는 방법을 설명 합니다. 프로그래밍 방식으로 또는 iOS 디자이너를 통해 앱에 이미지를 추가 하는 과정을 다룹니다.
ms.prod: xamarin
ms.assetid: 67CA8DB6-769D-42BB-A137-3AF933789FE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/13/2018
ms.openlocfilehash: 7f1e61f9364a6a59f2bbacd1c773fe49cc338db9
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528829"
---
# <a name="displaying-images-with-xamarinios"></a>Xamarin.ios를 사용 하 여 이미지 표시

앱에 이미지를 추가 하려면 먼저 두 단계가 필요 합니다. 먼저 프로젝트에 이미지를 추가 합니다. 그런 다음 컨트롤 및 코드를 추가 하 여 화면에 표시 합니다. Xamarin.ios에서 이미지를 처리 하는 방법에 대 한 자세한 내용은 [이미지 작업](~/ios/app-fundamentals/images-icons/index.md) 문서를 참조 하세요.

## <a name="adding-images-to-your-app"></a>앱에 이미지 추가

Mac용 Visual Studio 솔루션의 모든 폴더에 이미지를 추가할 수 있으며, **빌드 작업** 을 **내용** 으로 설정 하면 파일이 앱에 포함 되 고 표시 될 수 있습니다.

또한 Mac용 Visual Studio는 이미지 파일을 포함할 수 있는 **리소스** 라는 특수 디렉터리도 지원 합니다. Resources 폴더의 파일은 **빌드 작업** 을 **BundleResource**로 설정 해야 합니다.

이 스크린샷에서는 파일을 마우스 오른쪽 단추로 클릭할 때 표시 되는 **빌드 작업** 옵션을 보여 줍니다.

 [![](image-images/image30a.png "빌드 작업 메뉴")](image-images/image30a.png#lightbox)

Mac용 Visual Studio은 일반적으로 올바른 **빌드 작업** 을 자동으로 선택 하지만 특히 프로젝트에서 파일을 이동 하는 경우 이러한 설정을 알고 있어야 합니다.

### <a name="adding-an-image-file"></a>이미지 파일 추가

프로젝트에 이미지 파일을 추가 하려면 먼저 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **파일 추가** ...를 선택 합니다.

 [![](image-images/image31a.png "파일 추가 ... 메뉴가")](image-images/image31a.png#lightbox)

표준 파일 대화 상자에 포함할 이미지 (또는 이미지)를 선택 합니다. 이미지에 대 한 기본 빌드 작업은 **BundleResource** 입니다. 특별 한 이유가 없으면이 값을 재정의 하지 마십시오.

 [![](image-images/image32a.png "파일 추가 대화 상자")](image-images/image32a.png#lightbox)

이미지가 프로젝트에 추가 되 고 로드 되어 코드에 표시 될 수 있습니다. 이 스크린샷은 iOS 응용 프로그램 프로젝트에 추가 된 이미지를 보여 줍니다.

 [![](image-images/image33a.png "프로젝트의 이미지")](image-images/image33a.png#lightbox)

### <a name="what-is-the-resources-directory"></a>Resources 디렉터리 란?

**Resources** 디렉터리에 배치 된 파일은 일반 파일과 다르게 처리 됩니다. **리소스** 폴더의 콘텐츠는 응용 프로그램의 루트에 복사 되 고 코드에서 참조 될 수 있습니다. 이는 다음과 같은 다양 한 이유로 유용할 수 있습니다.

- 기본 시작 이미지 및 응용 프로그램 아이콘과 같이 응용 프로그램의 속성에 구성 된 이미지를 저장 합니다.
- 다른 이미지 및 파일을 코드와 별도로 저장 하므로 관리 하기가 더 쉽습니다. 하위 디렉터리는 리소스 디렉터리 콘텐츠가 복사 될 때 유지 됩니다.


**리소스** 디렉터리는 라이브러리 프로젝트에서 특히 유용 합니다. 코드는 해당 이미지가 소비 응용 프로그램의 루트에 복사 되는 것으로 가정할 수 있으므로 이미지, 사운드, 비디오, XML 또는를 필요로 하는 공유 코드 라이브러리를 더 쉽게 작성할 수 있기 때문입니다. 기타 파일.

**리소스** 디렉터리는로 명명 되어야 하며 모든 파일의 빌드 작업은 **BundleResource**로 설정 해야 합니다.

## <a name="displaying-the-image"></a>이미지 표시

IOS 디자이너에서 이미지 **뷰** 를 사용 하 여 이미지 또는 애니메이션 된 일련의 이미지를 표시 합니다. 도구 상자의 **이미지 뷰** 아이콘은 다음과 같습니다.

 [![](image-images/image35a.png "도구 상자의 ImageView")](image-images/image35.png#lightbox)

**도구 상자** 의 **이미지 뷰** 를 뷰 컨트롤러로 끌어 옵니다. 그런 다음 **이미지 보기 > 이미지** 아래에 있는 드롭다운 목록에는 프로젝트에서 사용 가능한 모든 이미지 파일의 목록이 제공 됩니다. 이러한 중 하나를 선택 하 여 이미지 보기에 추가 합니다.

 [![](image-images/image36a.png "도구 상자의 ImageView")](image-images/image36.png#lightbox)

### <a name="displaying-the-image-programmatically"></a>프로그래밍 방식으로 이미지 표시

**SF 원숭이 jpg** 는 **Resources** 디렉터리의 루트에 있으므로 응용 프로그램 번들의 루트에서 런타임에 사용할 수 있습니다. 이미지 뷰 컨트롤에이 이미지를 표시 하려면 다음 코드를 사용 합니다.

```csharp
imageview1.Image = UIImage.FromBundle("SF Monkey.png");
```

**/Resources/Pics/SF 원숭이**에 이미지를 배치한 경우 코드의 경로에는 **Pics** 폴더가 포함 됩니다.

```csharp
imageview1.Image = UIImage.FromBundle("Pics/SF Monkey.png");
```

리소스 파일 참조에는 **리소스** 폴더를 포함할 필요가 없습니다.

## <a name="related-links"></a>관련 링크

- [컨트롤 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
