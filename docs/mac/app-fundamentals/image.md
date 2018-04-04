---
title: 이미지
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘 이미지와 작업을 수행 합니다. 만들기 및 응용 프로그램의 아이콘을 만드는 데 필요한 및 C# 코드와 Xcode의 인터페이스 작성기에서 이미지를 사용 하 여 이미지를 유지 관리를 설명 합니다.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: dc33dc78c09c0b5b7cb7533afdd2f95b8ebd9c4e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="images"></a>이미지

_이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘 이미지와 작업을 수행 합니다. 만들기 및 응용 프로그램의 아이콘을 만드는 데 필요한 및 C# 코드와 Xcode의 인터페이스 작성기에서 이미지를 사용 하 여 이미지를 유지 관리를 설명 합니다._


## <a name="overview"></a>개요

해야 하는 이미지에 대 한 액세스를 사용할 때 C# 및.NET Xamarin.Mac 응용 프로그램에서 및 아이콘을 도구 작업 하는 개발자에 *Objective-c* 및 *Xcode* 않습니다.

해당 이미지 자산 macOS (이전의 Mac OS X) 응용 프로그램 내에서 사용 되는 여러 가지가입니다. 단순히 표시 안 함 아이콘을 제공 하기 위해 도구 모음 또는 소스 목록 항목과 같은 UI 컨트롤에 할당 하는 이미지, 응용 프로그램의 UI의 일부로 Xamarin.Mac 쉽게 macOS 응용 프로그램에 다음과 같은 방법으로 멋진 아트 워크를 추가 하려면 : 

- **UI 요소** -이미지 보기에서 응용 프로그램의 일부로 또는 배경으로 이미지를 표시할 수 있습니다 (`NSImageView`).
- **단추** -이미지 단추에 표시할 수 있습니다 (`NSButton`).
- **셀 이미지** -기반으로 하는 테이블 컨트롤의 일부로 (`NSTableView` 또는 `NSOutlineView`), 이미지 셀에 이미지를 사용할 수 있습니다 (`NSImageCell`).
- **도구 모음 항목** -도구 모음에 이미지를 추가할 수 있습니다 (`NSToolbar`) 이미지 도구 모음 항목으로 (`NSToolbarItem`).
- **원본 목록 아이콘** -소스 목록의 일부로 (한 특별 한 형식의 `NSOutlineView`).
- **응용 프로그램 아이콘** -으로 일련의 이미지를 함께 그룹화 할 수는 `.icns` 설정 하 고 응용 프로그램의 아이콘으로 사용 합니다. 참조 우리의 [응용 프로그램 아이콘](~/mac/deploy-test/app-icon.md) 자세한 정보에 대 한 설명서입니다.

또한 macOS 응용 프로그램 전체에서 사용할 수 있는 미리 정의 된 이미지 집합을 제공 합니다.

[![예제 응용 프로그램의 실행](image-images/intro01.png "응용 프로그램의 예 실행")](image-images/intro01-large.png#lightbox)

이 문서에서는의 기본적인 Xamarin.Mac 응용 프로그램에서 이미지 및 아이콘 사용 하겠습니다. 것이 가장 좋습니다를 통해 협력 하는 [Hello, Mac](~/mac/get-started/hello-mac.md) 먼저, 특히 문서는 [Xcode 및 인터페이스 작성기 소개](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) 및 [콘센트 및 동작](~/mac/get-started/hello-mac.md#Outlets_and_Actions) 섹션으로이 문서에서 사용할 수 있는 주요 개념 및 기술을 설명 합니다.


## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.Mac 프로젝트에 이미지 추가

Xamarin.Mac 응용 프로그램에서 사용할 이미지를 추가할 때 여러 장소 및 방법을 개발자가 이미지 파일을 프로젝트의 소스를 포함할 수 있습니다.

- **[Deprecated] 주 프로젝트 트리** -프로젝트 트리에 직접 이미지를 추가할 수 있습니다. 코드에서 기본 프로젝트 트리에 저장 된 이미지를 호출할 때 폴더 지정 된 위치가 없고 합니다. 예: `NSImage image = NSImage.ImageNamed("tags.png");` 
- **[Deprecated] resources 폴더** -특수 **리소스** 이미지 (또는 모든 다른 이미지 또는 파일 개발자 일반, 아이콘 및 시작 화면 등 응용 프로그램의 일부가 될 모든 파일의 번들로 대 한 폴더는 추가 한다고). 에 저장 된 이미지를 호출 하는 경우는 **리소스** 코드에서 폴더를 주 프로젝트 트리에 이미지 처럼 저장, 없음 폴더 위치를 지정 합니다. 예: `NSImage.ImageNamed("tags.png")`
- **사용자 지정 폴더 또는 하위 [deprecated]** -개발자는 프로젝트 소스 트리를 사용자 지정 폴더를 추가 하 고 있는 이미지를 저장할 수 있습니다. 추가 프로젝트 구성에 도움이 되도록 하위 폴더에 파일이 추가 되는 위치를 중첩할 수 있습니다. 예를 들어, 개발자가 추가 하는 경우는 `Card` 프로젝트 폴더의 하위 폴더 `Hearts` 해당 폴더에 이미지를 저장 한 다음 **Jack.png** 에 `Hearts` 폴더 `NSImage.ImageNamed("Card/Hearts/Jack.png")` 에서 이미지 로드 런타임입니다.
- **[기본] 자산 카탈로그 이미지 집합** OS X El Capitan에 추가 된- **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지원 하 고에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 프로그램 응용 프로그램입니다. 이미지 자산 파일 이름에 의존 하지 않고 (**@1x**, **@2x**).

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>설정 자산 카탈로그 이미지에 이미지 추가

위에서 설명 했 듯이 **자산 카탈로그 이미지 집합** 모든 버전 또는 다양 한 장치를 지 원하는 응용 프로그램에 대 한 요소를 확장 하는 데 필요한 이미지의 표현을 포함 합니다. 이미지 자산 파일 이름에 의존 하지 않고 (해상도 독립적인 이미지와 이미지 명명법 위 참조), **이미지 집합** 자산 편집기를 사용 하 여 어떤 장치 및/또는 해결 하는 이미지로 속하는 지정 합니다.

1. **솔루션 패드**, 두 번 클릭는 **Assets.xcassets** 편집을 위해 열 파일입니다. 

    ![Assets.xcassets 선택](image-images/imageset01.png "는 Assets.xcassets 선택")
2. 마우스 오른쪽 단추로 클릭는 **자산 목록** 선택 **새 이미지 집합**: 

    [![새 이미지 집합을 추가](image-images/imageset02.png "새 이미지 집합 추가")](image-images/imageset02-large.png#lightbox)
3. 새 이미지 집합을 선택 하 고 편집기를 표시 됩니다. 

    [![새 이미지 집합을 선택 하면](image-images/imageset03.png "새 이미지 집합을 선택 하면")](image-images/imageset03-large.png#lightbox)
4. 여기에서 끌어서 이미지에서 다양 한 장치 및 필요한 해결 방법은 각각에 대해 합니다. 
5. 새 이미지 집합을 두 번 클릭 **이름** 에 **자산 목록** 편집 하려면: 

    [![이름 설정 이미지 편집](image-images/imageset04.png "이미지 편집 집합 이름")](image-images/imageset04-large.png#lightbox)
    
특별 한 **벡터** 클래스에 추가 **이미지 집합** 포함할 수 있도록 하는 _PDF_ 대신에 개별 비트맵 파일을 포함 하 여 casset에서 벡터 이미지 형식 해결 방법은 다릅니다. 에 대 한 단일 벡터 파일을 제공 하이 메서드를 사용 하 여 **@1x** 해상도 (벡터 PDF 파일 형식) 및 **@2x** 및 **@3x** 파일의 버전 컴파일 타임에 생성 되며 응용 프로그램의 번들에 포함 합니다.

[![이미지 편집기 인터페이스 설정](image-images/imageset05.png "이미지 편집기 인터페이스 설정")](image-images/imageset05-large.png#lightbox)

예를 들어, 포함 하는 경우는 `MonkeyIcon.pdf` 150px x 150px, 자산 컴파일할 때 최종 응용 프로그램 번들에 포함 되어 다음과 같은 비트맵의 해결 방법을 통해 벡터 자산 카탈로그의 파일:

1. **MonkeyIcon@1x.png** -150px x 150px 확인 합니다.
2. **MonkeyIcon@2x.png** -300px x 300px 확인 합니다.
3. **MonkeyIcon@3x.png** -450px x 450px 확인 합니다.

다음은 고려해 야 자산 카탈로그의 PDF 벡터 이미지를 사용 하는 경우:

- PDF는 컴파일 시간 및 최종 응용 프로그램에서 제공 하는 비트맵에 비트맵 래스터화된 있을 전체 벡터 지원 되지 않습니다.
- 자산 카탈로그에 설정 되 면 이미지의 크기를 조정할 수 없습니다. (또는 코드에서 자동 레이아웃 및 크기 클래스를 사용 하 여) 이미지 크기를 조정 하려는 경우 다른 비트맵 마찬가지로 이미지가 왜곡 될 됩니다.

사용 하는 경우는 **이미지 설정** Xcode의 인터페이스 작성기에서 수 선택 하기만 하면 해당 집합의 이름에 있는 드롭다운 목록에서는 **특성 검사기**: * *

![Xcode의 인터페이스 작성기에서 이미지를 선택 하면 설정](image-images/imageset06.png "이미지를 선택 하면 Xcode의 인터페이스 작성기에서 설정")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>새 자산 컬렉션 추가

자산 카탈로그에는 이미지와 함께 작업 하는 경우 모든 이미지를 추가 하는 대신 새 컬렉션을 만들려는 경우가 있을 수 있습니다는 **Assets.xcassets** 컬렉션입니다. 예를 들어 요청 시 리소스를 디자인 하는 경우.

프로젝트에 새 자산 카탈로그를 추가 합니다.

1. 프로젝트를 마우스 오른쪽 단추로 클릭는 **솔루션 패드** 선택 **추가** > **새 파일...**
2. 선택 **Mac** > **자산 카탈로그**를 입력 한 **이름** 를 수집 하 고 클릭에 대 한는 **새로** 단추: 

    ![새 자산 카탈로그 추가](image-images/asset01.png "새 자산 카탈로그 추가")

여기에서 작업할 수 있습니다 컬렉션 동일한 방식으로 기본 **Assets.xcassets** 프로젝트에 자동으로 포함 하는 컬렉션입니다.


### <a name="adding-images-to-resources"></a>리소스에 이미지 추가

> [!IMPORTANT]
> 이러한 방식의 macOS 응용 프로그램에서 이미지 작업에 Apple에 의해 사용 되지 합니다. 사용 해야 [자산 카탈로그 이미지 집합](#asset-catalogs) 하려면 관리자 앱의 이미지를 대신 합니다.

프로젝트에 포함 하는 데 필요한 이미지 파일 (또는 C# 코드에서 인터페이스 작성기에서) Xamarin.Mac 응용 프로그램에서 사용 하려면 먼저 **리소스** 폴더는 **번들 리소스**합니다. 파일을 프로젝트에 추가 하려면 다음을 수행 합니다.

1. 마우스 오른쪽 단추로 클릭는 **리소스** 에 프로젝트 폴더에에서는 **솔루션 패드** 선택 **추가** > **파일 추가 중...** : 

    ![파일 추가](image-images/add01.png "파일 추가")
2. **파일 추가** 대화 상자에서 이미지 파일을 프로젝트에 추가 하려면 선택 `BundleResource` 에 대 한는 **재정의 빌드 작업** 클릭는 **열려** 단추:

    [![추가할 파일을 선택 하면](image-images/add02.png "추가할 파일을 선택 합니다.")](image-images/add02-large.png#lightbox)
3. 파일이 없는 경우에 **리소스** 폴더를 묻는 하려는 경우 **복사**, **이동** 또는 **링크** 파일입니다. 되는 모든를 선택할 수 있는 일반적으로 필요 **복사**:

    ![추가 작업을 선택 하면](image-images/add04.png "추가 동작 선택")
4. 새 파일은 프로젝트에 포함 되며 사용 하기 위해 읽기: 

    ![솔루션 패드에 추가 된 새 이미지 파일](image-images/add03.png "솔루션 패드에 추가 된 새 이미지 파일")
5. 필요한 모든 이미지 파일에 대 한 프로세스를 반복 합니다.

Xamarin.Mac 응용 프로그램에서 원본 이미지는 png, jpg, 또는 pdf 파일을 사용할 수 있습니다. 다음 섹션에서 고해상도 버전의 우리의 이미지 추가에서 살펴보게 및 레 티 나 지원 하기 위해 아이콘 Mac을 기반으로 합니다.

> [!IMPORTANT]
> 이미지를 추가 하는 경우는 **리소스** 폴더를 그대로 둘 수 있습니다는 **재정의 빌드 작업** 로 설정 **기본**합니다. 기본값이이 폴더에 대 한 빌드 작업은 `BundleResource`합니다.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>응용 프로그램에서 제공 하는 모든 그래픽 리소스의 고해상도 버전을 제공 합니다.

모든 그래픽 자산 (아이콘, 사용자 지정 컨트롤, 사용자 지정 커서, 사용자 지정 아트 워크 등) Xamarin.Mac 응용 프로그램에 추가 하는 표준 해상도 버전 외에도 고해상도 버전 해야 합니다. 레 티 나 디스플레이에서 실행을 Mac 컴퓨터를 장착 때 응용 프로그램 최적화를 찾을 수 있도록 이것이 필요 합니다.


### <a name="adopt-the-2x-naming-convention"></a>채택 하는 @2x 명명 규칙

> [!IMPORTANT]
> 이러한 방식의 macOS 응용 프로그램에서 이미지 작업에 Apple에 의해 사용 되지 합니다. 사용 해야 [자산 카탈로그 이미지 집합](#asset-catalogs) 하려면 관리자 앱의 이미지를 대신 합니다.

표준 작업 문자와 고해상도 이미지를 만들 때 Xamarin.Mac 프로젝트에 포함 하는 경우 이미지 쌍에 대 한 명명 규칙을 따릅니다.

- **표준 해상도**  - **ImageName.filename 확장** (예: **tags.png**)
- **고해상도**   -  **ImageName@2x.filename-extension** (예: **tags@2x.png**)

프로젝트에 추가 하는 경우 다음과 같이 표시 됩니다.

![솔루션 패드에 이미지 파일](image-images/add03.png "솔루션 패드에 이미지 파일")

파일을 선택 하기만 하면 합니다 이미지 인터페이스 작성기에 있는 UI 요소에 할당 된 경우는 _ImageName_**.** _파일 이름 확장명_ 형식 (예: **tags.png**). 파일을 선택 합니다 동일 이미지를 사용 하 여 C# 코드에서에 대 한는 _ImageName_**.** _파일 이름 확장명_ 형식입니다.

Mac에서 실행 되는 하면 Xamarin.Mac 응용 프로그램의 _ImageName_**.** _파일 이름 확장명_ 형식 이미지 표준 확인 표시에 사용 될는 **ImageName@2x.filename-extension** 이미지 자동으로 선택 됩니다 레 티 나 디스플레이 기반 Mac로 만듭니다.


## <a name="using-images-in-interface-builder"></a>이미지를 사용 하 여 인터페이스 작성기

에 추가한 모든 이미지 리소스는 **리소스** 폴더 프로그램 Xamarin.Mac에 프로젝트를 빌드 작업으로 설정한 **BundleResource** 자동으로 인터페이스 작성기에서 볼 수 있으며 될 수 있습니다 (이미지를 처리) 하는 경우에 UI 요소의 일부로 선택 합니다.

인터페이스 작성기에서 이미지를 사용 하려면 다음을 수행 합니다.

1. 이미지를 추가할는 **리소스** 폴더는 **빌드 작업** 의 `BundleResource`: 

     ![솔루션 패드에서 이미지 리소스](image-images/ib00.png "솔루션 패드에서 이미지 리소스")
2. 두 번 클릭 하 여 **Main.storyboard** 인터페이스 작성기에서 편집을 위해 열 파일입니다. 

     [![주 스토리 보드 편집](image-images/ib01.png "주 스토리 보드를 편집 합니다.")](image-images/ib01-large.png#lightbox)
3. 디자인 화면에 이미지를 사용 하는 UI 요소를 끌어 옵니다 (예를 들어 한 **이미지 도구 모음 항목**): 

     ![도구 모음 항목 편집](image-images/ib02.png "도구 모음 항목 편집")
4. 이미지에 추가 선택 하 고 **리소스** 폴더에는 **이미지 이름** 드롭다운: 

     [![도구 모음 항목에 대 한 이미지를 선택 하면](image-images/ib03.png "도구 모음 항목에 대 한 이미지를 선택 합니다.")](image-images/ib03-large.png#lightbox)
5. 선택한 이미지는 디자인 화면에 표시 됩니다. 

     ![도구 모음 편집기에 표시 되는 이미지](image-images/ib04.png "도구 모음 편집기에 표시 되는 이미지")
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac 용 Visual Studio로 돌아갑니다.

이미지 속성 설정할 수 있는 UI 요소에는 위의 단계가 작동는 **특성 검사기**합니다. 다시, 포함 된 경우는 **@2x** 이미지 파일의 버전을 자동으로 사용 됩니다에 레 티 나 디스플레이 Mac을 기반으로 합니다.

> [!IMPORTANT]
> 이미지에서 사용할 수 없는 경우는 **이미지 이름** 드롭다운에서.storyboard 프로젝트가 Xcode에서 닫았다가 Visual Studio에서 Mac.에 대 한 이미지 여전히를 사용할 수 없는 경우 되도록 해당 **빌드 작업** 은 `BundleResource` 및 이미지에 추가 된는 **리소스** 폴더입니다.

## <a name="using-images-in-c-code"></a>이미지를 사용 하 여 C# 코드

이미지에 Xamarin.Mac 응용 프로그램의 C# 코드를 사용 하 여 메모리를 로드할 때 이미지에 저장 됩니다는 `NSImage` 개체입니다. 이미지 파일 (리소스에 포함) Xamarin.Mac 응용 프로그램 번들에 포함 된, 다음 코드를 사용 하 여 이미지 로드:

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

위의 코드에서는 정적 `ImageNamed("...")` 의 메서드는 `NSImage` 에서 메모리에 지정 된 이미지를 로드 하는 클래스는 **리소스** 폴더 이미지를 찾을 수 없는 경우 `null` 반환 됩니다. 포함 된 경우에 같은 인터페이스 작성기에서 할당 된 이미지로 **@2x** 이미지 파일의 버전을 자동으로 사용 됩니다에 레 티 나 디스플레이 Mac을 기반으로 합니다.

응용 프로그램의 번들 (Mac 파일 시스템)에서 외부 이미지를 로드 하려면 다음 코드를 사용 합니다.

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>템플릿 이미지 작업

MacOS 응용 프로그램의 디자인에 따라, 경우도 아이콘 또는 이미지 (예: 사용자 기본 설정에 따라) 색 구성표 변경에 맞게 사용자 인터페이스 내에서 사용자 지정 하는 경우가 있습니다.

이 효과 얻기 위해 스위치는 _렌더링 모드_ 의 이미지 자산을 **템플릿 이미지**:

[![템플릿 이미지 설정](image-images/templateimage01.png "템플릿 이미지를 설정 합니다.")](image-images/templateimage01-large.png#lightbox)

Xcode의 인터페이스 작성기에서 이미지 자산 UI 컨트롤에 할당 합니다.

![Xcode의 인터페이스 작성기에서 이미지를 선택 하면](image-images/templateimage02.png "Xcode의 인터페이스 작성기에서 이미지를 선택 하면")

또는 필요에 따라 코드에서 이미지 소스를 설정 합니다.

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

보기 컨트롤러에 다음 공용 함수를 추가 합니다.

```csharp
public NSImage ImageTintedWithColor (NSImage image, NSColor tint)
{
    var tintedImage = image.Copy () as NSImage;
    var frame = new CGRect (0, 0, image.Size.Width, image.Size.Height);

    // Apply tint
    tintedImage.LockFocus ();
    tint.Set ();
    NSGraphics.RectFill (frame, NSCompositingOperation.SourceAtop);
    tintedImage.UnlockFocus ();
    tintedImage.Template = false;

    // Return tinted image
    return tintedImage;
}
```

마지막으로 템플릿 이미지를 색칠 할 색을 지정 하려면 이미지에 대해이 함수를 호출:

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>이미지를 사용 하 여 테이블 뷰

에 있는 셀의 일부로 이미지를 포함 하는 `NSTableView`, 표 보기에서 데이터 반환 되는 방법을 변경 하려면 필요 합니다 `NSTableViewDelegate's` `GetViewForItem` 메서드를 사용 하 여는 `NSTableCellView` 일반적인 대신 `NSTextField`합니다. 예를 들어:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

몇 가지 흥미로운 몇 줄 있습니다. 첫째, 이미지를 포함 하려는 열에 대 한 만듭니다 새 `NSImageView` 필요한 크기와 위치도 생성 새 `NSTextField` 이미지 사용 여부에 따라 기본 위치에 놓습니다.

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

둘째, 부모에서 새 이미지 보기 및 텍스트 필드를 포함 하도록 해야 `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

마지막으로, 축소 하 고 테이블 보기 셀 함께 성장할 수는 텍스트 필드를 지시 해야 합니다.

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

예제 출력:

[![응용 프로그램에서 이미지를 표시의 예가](image-images/tables01.png "응용 프로그램에서 이미지를 표시 하는의 예")](image-images/tables01-large.png#lightbox)

표 보기를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서입니다.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>개요 보기와 이미지를 사용 하 여

에 있는 셀의 일부로 이미지를 포함 하는 `NSOutlineView`, 개요 보기에서 데이터 반환 되는 방법을 변경 해야 `NSTableViewDelegate's` `GetView` 메서드를 사용 하 여는 `NSTableCellView` 일반적인 대신 `NSTextField`합니다. 예를 들어:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break; 
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

몇 가지 흥미로운 몇 줄 있습니다. 첫째, 이미지를 포함 하려는 열에 대 한 만듭니다 새 `NSImageView` 필요한 크기와 위치도 생성 새 `NSTextField` 이미지 사용 여부에 따라 기본 위치에 놓습니다.

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

둘째, 부모에서 새 이미지 보기 및 텍스트 필드를 포함 하도록 해야 `NSTableCellView`:

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

마지막으로, 축소 하 고 테이블 보기 셀 함께 성장할 수는 텍스트 필드를 지시 해야 합니다.

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

예제 출력:

[![개요 보기에 표시 되는 이미지의 예로](image-images/outline01.png "개요 보기에 표시 되는 이미지의 예")](image-images/outline01-large.png#lightbox)

개요 보기를 사용한 작업에 대 한 자세한 내용은 참조 하십시오 우리의 [개요 뷰](~/mac/user-interface/outline-view.md) 설명서입니다.


## <a name="summary"></a>요약

이 문서 통합 이미지 및 아이콘 Xamarin.Mac 응용 프로그램에서 작업에 대해 자세히를 하였습니다. 다양 한 유형을 보여 준다는 사실을 알았습니다 하 고 이미지, 이미지 및 아이콘 Xcode의 인터페이스 작성기에서 사용 하는 방법 및 C# 코드에서 이미지 및 아이콘 작업 하는 방법을 사용 합니다.



## <a name="related-links"></a>관련 링크

- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 뷰](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [macOS X Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X용 고해상도 정보](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
