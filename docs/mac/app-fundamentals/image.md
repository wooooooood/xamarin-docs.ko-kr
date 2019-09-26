---
title: Xamarin.ios의 이미지
description: 이 문서에서는 Xamarin.ios 응용 프로그램에서 이미지 및 아이콘 작업을 설명 합니다. 응용 프로그램의 아이콘을 만드는 데 필요한 이미지를 만들고 유지 관리 하는 방법 및 C# 코드와 Xcode의 Interface Builder에서 이미지를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: C6B539C2-FC6A-4C38-B839-32BFFB9B16A7
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/15/2017
ms.openlocfilehash: 99604b59e5557ba5a7aa3d5ba61bc1bff414f000
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/25/2019
ms.locfileid: "70770318"
---
# <a name="images-in-xamarinmac"></a>Xamarin.ios의 이미지

_이 문서에서는 Xamarin.ios 응용 프로그램에서 이미지 및 아이콘 작업을 설명 합니다. 응용 프로그램의 아이콘을 만드는 데 필요한 이미지를 만들고 유지 관리 하는 방법 및 C# 코드와 Xcode의 Interface Builder에서 이미지를 사용 하는 방법을 설명 합니다._

## <a name="overview"></a>개요

Xamarin.ios 응용 프로그램 C# 에서 및 .net을 사용 하는 경우 *목표-C* 및 *Xcode* 에서 작업 하는 개발자와 동일한 이미지 및 아이콘 도구에 액세스할 수 있습니다.

MacOS (이전의 Mac OS X) 응용 프로그램 내에서 이미지 자산을 사용 하는 몇 가지 방법이 있습니다. 단순히 응용 프로그램 UI의 일부로 이미지를 표시 하 고 도구 모음이 나 소스 목록 항목과 같은 UI 컨트롤에 할당 하 여 아이콘을 제공 하면 Xamarin.ios를 사용 하면 다음과 같은 방식으로 macOS 응용 프로그램에 유용한 아트 워크를 쉽게 추가할 수 있습니다. : 

- **UI 요소** -이미지는 배경으로 표시 되거나 이미지 뷰에서 응용 프로그램의 일부로 표시 될 수 있습니다 (`NSImageView`).
- **단추** -이미지를 단추 (`NSButton`)로 표시할 수 있습니다.
- **이미지 셀** -테이블 기반 컨트롤 (`NSTableView` 또는 `NSOutlineView`)의 일부로 이미지 셀 (`NSImageCell`)에 이미지를 사용할 수 있습니다.
- **도구 모음 항목** -이미지를 도구 모음 항목 (`NSToolbar``NSToolbarItem`)으로 도구 모음에 추가할 수 있습니다.
- 원본 **목록 아이콘** -원본 목록의 일부로 특수 형식이 지정 `NSOutlineView`된입니다.
- **앱 아이콘** -일련의 이미지를 하나의 `.icns` 집합으로 그룹화 하 고 응용 프로그램의 아이콘으로 사용할 수 있습니다. 자세한 내용은 [응용 프로그램 아이콘](~/mac/deploy-test/app-icon.md) 설명서를 참조 하세요.

또한 macOS는 응용 프로그램 전체에서 사용할 수 있는 미리 정의 된 이미지 집합을 제공 합니다.

[![응용 프로그램의 예제 실행](image-images/intro01.png "응용 프로그램의 예제 실행")](image-images/intro01-large.png#lightbox)

이 문서에서는 Xamarin.ios 응용 프로그램에서 이미지 및 아이콘을 사용 하는 기본 사항을 설명 합니다. [Hello, Mac](~/mac/get-started/hello-mac.md) 문서를 먼저 사용 하는 것이 가장 좋습니다. 특히 [Xcode 및 Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 및 [콘센트 및 작업](~/mac/get-started/hello-mac.md#outlets-and-actions) 섹션을 소개 하 고,에서 사용할 주요 개념 및 기술을 설명 하 고 있습니다. 이 문서를 참조 하세요.

## <a name="adding-images-to-a-xamarinmac-project"></a>Xamarin.ios 프로젝트에 이미지 추가

Xamarin.ios 응용 프로그램에서 사용할 이미지를 추가 하는 경우 개발자가 프로젝트 소스에 이미지 파일을 포함할 수 있는 몇 가지 위치와 방법이 있습니다.

- **주 프로젝트 트리 [사용 되지 않음]** -프로젝트 트리에 직접 이미지를 추가할 수 있습니다. 코드에서 주 프로젝트 트리에 저장 된 이미지를 호출 하는 경우에는 폴더 위치가 지정 되지 않습니다. 예: `NSImage image = NSImage.ImageNamed("tags.png");` 
- **리소스 폴더 [사용 되지 않음]** -특수 **리소스** 폴더는 아이콘, 시작 화면 또는 일반 이미지 (또는 개발자가 추가 하려는 다른 이미지 또는 파일)와 같은 응용 프로그램 번들의 일부가 될 파일에 대 한 것입니다. 주 프로젝트 트리에 저장 된 이미지와 마찬가지로 코드에서 **Resources** 폴더에 저장 된 이미지를 호출 하는 경우에는 폴더 위치가 지정 되지 않습니다. 예: `NSImage.ImageNamed("tags.png")`
- **사용자 지정 폴더 또는 하위 폴더 [사용 되지 않음]** -개발자가 프로젝트 소스 트리에 사용자 지정 폴더를 추가 하 고 여기에 이미지를 저장할 수 있습니다. 프로젝트를 구성 하는 데 도움이 되도록 파일이 추가 된 위치를 하위 폴더에 중첩 시킬 수 있습니다. 예를 들어 `Card` 개발자가 프로젝트에 폴더를 추가 하 고 해당 폴더 `Hearts` 에 하위 폴더를 추가한 경우 이미지에 `Hearts` `NSImage.ImageNamed("Card/Hearts/Jack.png")` **.png** 이미지를 저장 하 여 런타임에 이미지를 로드 합니다.
- **Asset Catalog 이미지 집합 [기본 설정]** -OS X El Capitan에 추가 됨, **자산 카탈로그 이미지 집합** 은 응용 프로그램에 대 한 다양 한 장치 및 크기 조정 요소를 지 원하는 데 필요한 이미지의 모든 버전 또는 표현을 포함 합니다. 이미지 자산 파일 이름 ( **@1x** , **@2x** )에 의존 하지 않습니다.

<a name="asset-catalogs" />

### <a name="adding-images-to-an-asset-catalog-image-set"></a>자산 카탈로그 이미지 집합에 이미지 추가

위에서 설명한 것 처럼 **자산 카탈로그 이미지 집합** 에는 응용 프로그램의 다양 한 장치 및 크기 조정 요소를 지 원하는 데 필요한 이미지의 모든 버전 또는 표현이 포함 되어 있습니다. 이미지 자산 파일 이름에 의존 하는 대신 (위의 확인 독립적 이미지 및 이미지 명명법 참조) **이미지 집합** 은 자산 편집기를 사용 하 여 장치 및/또는 해상도에 속하는 이미지를 지정 합니다.

1. **Solution Pad**에서 **assets.xcassets** 파일을 두 번 클릭 하 여 편집용으로 엽니다. 

    ![Assets.xcassets 선택](image-images/imageset01.png "Assets.xcassets 선택")
2. **자산 목록을** 마우스 오른쪽 단추로 클릭 하 고 **새 이미지 집합**을 선택 합니다. 

    [![새 이미지 집합 추가](image-images/imageset02.png "새 이미지 집합 추가")](image-images/imageset02-large.png#lightbox)
3. 새 이미지 집합을 선택 하면 편집기가 표시 됩니다. 

    [![새 이미지 집합 선택](image-images/imageset03.png "새 이미지 집합 선택")](image-images/imageset03-large.png#lightbox)
4. 여기에서 필요한 각각의 서로 다른 장치 및 해상도에 대 한 이미지를 끌어올 수 있습니다. 
5. **자산 목록** 에서 새 이미지 집합의 **이름을** 두 번 클릭 하 여 편집 합니다. 

    [![이미지 집합 이름 편집](image-images/imageset04.png "이미지 집합 이름 편집")](image-images/imageset04-large.png#lightbox)
    
다른 해상도의 개별 비트맵 파일을 포함 하는 대신 casset에 _PDF_ 형식의 벡터 이미지를 포함할 수 있도록 하는 **이미지 집합** 에 추가 된 특수 **vector** 클래스입니다. 이 메서드를 사용 하 여 **@1x** 해상도에 대 한 단일 벡터 파일 (벡터 PDF 파일로 서식 지정)을 제공 하면 및 **@3x** 파일의 버전은 **@2x** 컴파일 시간에 생성 되 고 응용 프로그램의 번들에 포함 됩니다.

[![이미지 집합 편집기 인터페이스](image-images/imageset05.png "이미지 집합 편집기 인터페이스")](image-images/imageset05-large.png#lightbox)

예를 들어 150px x 150px의 `MonkeyIcon.pdf` 해상도를 사용 하 여 자산 카탈로그의 벡터로 파일을 포함 하는 경우 컴파일할 때 다음 비트맵 자산이 최종 앱 번들에 포함 됩니다.

1. **MonkeyIcon@1x.png** -150px x 150px resolution.
2. **MonkeyIcon@2x.png** -300px x 300px 해상도입니다.
3. **MonkeyIcon@3x.png** -450px x 450px resolution.

자산 카탈로그에서 PDF 벡터 이미지를 사용 하는 경우 다음 사항을 고려해 야 합니다.

- PDF는 컴파일 시간에 비트맵으로 래스터화됩니다. 최종 응용 프로그램에서 제공 되는 비트맵으로 래스터화됩니다.
- 자산 카탈로그에 설정 된 이미지의 크기는 조정할 수 없습니다. 코드에서 또는 자동 레이아웃 및 크기 클래스를 사용 하 여 이미지의 크기를 조정 하려고 하면 다른 비트맵과 마찬가지로 이미지가 왜곡 됩니다.

Xcode의 Interface Builder에서 **이미지 집합** 을 사용 하는 경우 **특성 검사자**의 드롭다운 목록에서 집합의 이름을 선택 하면 됩니다.

![Xcode의 Interface Builder에서 이미지 집합 선택](image-images/imageset06.png "Xcode의 Interface Builder에서 이미지 집합 선택")

<a name="Adding-new-Assets-Collections"/>

### <a name="adding-new-assets-collections"></a>새 자산 컬렉션 추가

자산 카탈로그에서 이미지를 사용 하는 경우 **assets.xcassets** 컬렉션에 모든 이미지를 추가 하는 대신 새 컬렉션을 만들 수 있습니다. 예를 들어 주문형 리소스를 디자인할 때입니다.

프로젝트에 새 자산 카탈로그를 추가 하려면 다음을 수행 합니다.

1. **Solution Pad** 에서 프로젝트를 마우스 오른쪽 단추로 클릭 하 고**새 파일** **추가** > ...를 선택 합니다.
2. **Mac**자산 카탈로그를 선택 하 고, 컬렉션의 이름을 입력 하 고, 새로 만들기 단추를 클릭 합니다. >  

    ![새 자산 카탈로그 추가](image-images/asset01.png "새 자산 카탈로그 추가")

여기에서 기본 **assets.xcassets** 컬렉션이 프로젝트에 자동으로 포함 되는 것과 동일한 방식으로 컬렉션을 사용할 수 있습니다.

### <a name="adding-images-to-resources"></a>리소스에 이미지 추가

> [!IMPORTANT]
> MacOS 앱에서 이미지를 사용 하는이 방법은 Apple에서 더 이상 사용 되지 않습니다. 대신 [자산 카탈로그 이미지 집합](#asset-catalogs) 을 사용 하 여 앱의 이미지를 관리 해야 합니다.

Xamarin.ios 응용 프로그램 ( C# 코드 또는 Interface Builder)에서 이미지 파일을 사용 하려면 먼저 프로젝트의 **Resources** 폴더에 **번들 리소스로**포함 해야 합니다. 프로젝트에 파일을 추가 하려면 다음을 수행 합니다.

1. **Solution Pad** 에서 프로젝트의 **Resources** 폴더를 마우스 오른쪽 단추로 클릭 하 고 **추가** > **파일**추가 ...를 선택 합니다. 

    ![파일 추가](image-images/add01.png "파일 추가")
2. **파일 추가** 대화 상자에서 프로젝트에 추가할 이미지 파일을 선택 하 고 **빌드 재정의 작업** 에 `BundleResource` 대해를 선택한 후 **열기** 단추를 클릭 합니다.

    [![추가할 파일 선택](image-images/add02.png "추가할 파일 선택")](image-images/add02-large.png#lightbox)
3. 파일이 **Resources** 폴더에 아직 없는 경우 파일을 **복사**, **이동** 또는 **연결할** 것인지 묻는 메시지가 표시 됩니다. 일반적으로 **복사**되는 요구 사항에 맞는 모든 것을 선택 합니다.

    ![추가 작업 선택](image-images/add04.png "추가 작업 선택")
4. 새 파일이 프로젝트에 포함 되 고 사용 하기 위해 읽혀집니다. 

    ![Solution Pad에 추가 된 새 이미지 파일](image-images/add03.png "Solution Pad에 추가 된 새 이미지 파일")
5. 필요한 모든 이미지 파일에 대해이 프로세스를 반복 합니다.

Xamarin.ios 응용 프로그램의 원본 이미지로 png, jpg 또는 pdf 파일을 사용할 수 있습니다. 다음 섹션에서는 레 티 나 기반 Mac을 지원 하기 위해 이미지 및 아이콘의 고해상도 버전을 추가 하는 방법을 살펴보겠습니다.

> [!IMPORTANT]
> **리소스** 폴더에 이미지를 추가 하는 경우 **빌드 재정의 작업** 을 **기본값으로**설정 된 상태로 둘 수 있습니다. 이 폴더의 기본 빌드 작업은 `BundleResource`입니다.

## <a name="provide-high-resolution-versions-of-all-app-graphics-resources"></a>모든 앱 그래픽 리소스의 고해상도 버전 제공

Xamarin.ios 응용 프로그램 (아이콘, 사용자 지정 컨트롤, 사용자 지정 커서, 사용자 지정 아트 워크 등)에 추가 하는 모든 그래픽 자산은 표준 해상도 버전 외에도 고해상도 버전이 있어야 합니다. 이는 응용 프로그램이 레 티 나 Display 장착 된 Mac 컴퓨터에서 실행 될 때 가장 적합 한 것을 확인 하는 데 필요 합니다.

### <a name="adopt-the-2x-naming-convention"></a>@2x 명명 규칙 채택

> [!IMPORTANT]
> MacOS 앱에서 이미지를 사용 하는이 방법은 Apple에서 더 이상 사용 되지 않습니다. 대신 [자산 카탈로그 이미지 집합](#asset-catalogs) 을 사용 하 여 앱의 이미지를 관리 해야 합니다.

표준 및 고해상도 버전의 이미지를 만들 때 Xamarin.ios 프로젝트에 포함할 때 이미지 쌍에 대해이 명명 규칙을 따릅니다.

- **표준 해상도**  - **ImageName. 파일 이름-확장명** (예: **태그도 .png**)
- **고해상도** (예: **)tags@2x.png**   -  **ImageName@2x.filename-extension**

프로젝트에 추가 되 면 다음과 같이 표시 됩니다.

![Solution Pad의 이미지 파일](image-images/add03.png "Solution Pad의 이미지 파일")

이미지가 Interface Builder의 UI 요소에 할당 되 면 _ImageName_에서 파일을 선택 하기만 하면 됩니다 **.** _파일 이름-확장명_ 형식 (예: **tags .png**) 코드에서 C# 이미지를 사용 하는 경우와 마찬가지로 ImageName에서 파일을 선택 합니다 **.** _파일 이름-확장명_ 형식입니다.

Xamarin.ios 응용 프로그램이 Mac에서 실행 되는 경우 _ImageName_ **.** _파일 이름-확장_ 형식 이미지는 표준 해상도 디스플레이에서 사용 되며, **ImageName@2x.filename-extension** 레 티 나 디스플레이 기반 mac에서 이미지가 자동으로 선택 됩니다.

## <a name="using-images-in-interface-builder"></a>Interface Builder에서 이미지 사용

Xamarin.ios 프로젝트의 **Resources** 폴더에 추가한 모든 이미지 리소스를 설정 하 고, 빌드 작업을 **BundleResource** 에 자동으로 Interface Builder 표시 하 고,이를 처리 하는 경우 UI 요소의 일부로 선택할 수 있습니다. 이미지).

Interface builder에서 이미지를 사용 하려면 다음을 수행 합니다.

1. 의 빌드`BundleResource` **작업** 을 사용 하 여 리소스 폴더에 이미지를 추가 합니다. 

     ![Solution Pad의 이미지 리소스](image-images/ib00.png "Solution Pad의 이미지 리소스")
2. **주 storyboard** 파일을 두 번 클릭 하 여 Interface Builder에서 편집할 수 있도록 엽니다. 

     [![주 스토리 보드 편집](image-images/ib01.png "주 스토리 보드 편집")](image-images/ib01-large.png#lightbox)
3. 이미지를 사용 하는 UI 요소를 디자인 화면 (예: **이미지 도구 모음 항목**)으로 끌어 옵니다. 

     ![도구 모음 항목 편집](image-images/ib02.png "도구 모음 항목 편집")
4. **이미지 이름** 드롭다운에서 **Resources** 폴더에 추가한 이미지를 선택 합니다. 

     [![도구 모음 항목의 이미지 선택](image-images/ib03.png "도구 모음 항목의 이미지 선택")](image-images/ib03-large.png#lightbox)
5. 선택한 이미지가 디자인 화면에 표시 됩니다. 

     ![도구 모음 편집기에 표시 되는 이미지](image-images/ib04.png "도구 모음 편집기에 표시 되는 이미지")
6. 변경 내용을 저장 하 고 Xcode와 동기화 할 Mac용 Visual Studio로 돌아갑니다.

위의 단계는 **특성 검사자**에서 이미지 속성을 설정할 수 있도록 하는 모든 UI 요소에 대해 작동 합니다. 이미지 파일의 **@2x** 버전을 포함 한 경우에는 레 티 나 표시 기반 mac에서 자동으로 사용 됩니다.

> [!IMPORTANT]
> 이미지 **이름** 드롭다운에서 이미지를 사용할 수 없는 경우 Xcode에서 storyboard 프로젝트를 닫고 Mac용 Visual Studio에서 다시 엽니다. 이미지를 아직 사용할 수 없는 경우 해당 **빌드 작업이** 이 `BundleResource` 고 이미지가 **Resources** 폴더에 추가 되었는지 확인 합니다.

## <a name="using-images-in-c-code"></a>코드에서 C# 이미지 사용

Xamarin.ios 응용 프로그램의 코드를 사용 C# 하 여 메모리에 이미지를 로드할 때 이미지는 `NSImage` 개체에 저장 됩니다. 이미지 파일이 Xamarin.ios 응용 프로그램 번들에 포함 된 경우 (리소스에 포함) 다음 코드를 사용 하 여 이미지를 로드 합니다.

```csharp
NSImage image = NSImage.ImageNamed("tags.png");
```

위의 코드에서는 `NSImage` 클래스의 정적 `ImageNamed("...")` 메서드를 사용 하 여 **리소스** 폴더에서 지정 된 이미지를 메모리로 로드 합니다. 이미지를 찾을 `null` 수 없는 경우이 반환 됩니다. Interface Builder에서 할당 된 이미지와 마찬가지로, 이미지 파일의 **@2x** 버전을 포함 한 경우 레 티 나 표시 기반 mac에서 자동으로 사용 됩니다.

Mac 파일 시스템에서 응용 프로그램 번들 외부에 이미지를 로드 하려면 다음 코드를 사용 합니다.

```csharp
NSImage image = new NSImage("/Users/KMullins/Documents/photo.jpg")
```

<a name="Working-with-Template-Images"/>

## <a name="working-with-template-images"></a>템플릿 이미지 작업

MacOS 앱의 디자인에 따라 색 구성표의 변경 (예: 사용자 기본 설정에 따라)과 일치 하도록 사용자 인터페이스 내에서 아이콘이 나 이미지를 사용자 지정 해야 하는 경우가 있을 수 있습니다.

이 효과를 얻으려면 이미지 자산의 _렌더링 모드_ 를 **템플릿 이미지로**전환 합니다.

[![템플릿 이미지 설정](image-images/templateimage01.png "템플릿 이미지 설정")](image-images/templateimage01-large.png#lightbox)

Xcode의 Interface Builder에서 UI 컨트롤에 이미지 자산을 할당 합니다.

![Xcode의 Interface Builder에서 이미지 선택](image-images/templateimage02.png "Xcode의 Interface Builder에서 이미지 선택")

또는 필요에 따라 코드에서 이미지 소스를 설정 합니다.

```csharp
MyIcon.Image = NSImage.ImageNamed ("MessageIcon");
```

다음 공용 함수를 뷰 컨트롤러에 추가 합니다.

```csharp
public NSImage ImageTintedWithColor(NSImage sourceImage, NSColor tintColor)
    => NSImage.ImageWithSize(sourceImage.Size, false, rect => {
        // Draw the original source image
        sourceImage.DrawInRect(rect, CGRect.Empty, NSCompositingOperation.SourceOver, 1f);

        // Apply tint
        tintColor.Set();
        NSGraphics.RectFill(rect, NSCompositingOperation.SourceAtop);

        return true;
    });
```

> [!IMPORTANT]
> 특히 macos mojave에서 어두운 모드를 사용 하는 경우 사용자 지정 렌더링 `LockFocus` `NSImage` 된 개체를 다시 지정 하는 경우 API를 사용 하지 않는 것이 중요 합니다. 이러한 이미지는 정적이 되며 모양이 나 디스플레이 밀도 변경을 고려 하도록 자동으로 업데이트 되지 않습니다.
>
> 위의 처리기 기반 메커니즘을 `NSImage` 사용 하는 경우와 같이가 호스팅될 `NSImageView`때 동적 조건을 다시 렌더링 하면 자동으로 발생 합니다.

마지막으로 템플릿 이미지의 농도를 검사 하려면 이미지에 대해이 함수를 색으로 호출 합니다.

```csharp
MyIcon.Image = ImageTintedWithColor (MyIcon.Image, NSColor.Red);
```

<a name="Using_Images_with_Table_Views" />

## <a name="using-images-with-table-views"></a>테이블 뷰에서 이미지 사용

에 `NSTableView`있는 셀의 일부로 이미지를 포함 하려면 테이블 `NSTableViewDelegate's` `GetViewForItem` 뷰의 메서드에서 데이터가 반환 되는 방법을 변경 하 여 일반적인 `NSTextField`대신를 `NSTableCellView` 사용 해야 합니다. 예:

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

여기에는 몇 줄의 관심이 있습니다. 첫째, 이미지를 포함 하려는 열에 대해 필요한 크기와 위치를 새로 `NSImageView` 만들고, 이미지를 사용 하 고 있는지 여부에 따라 새 `NSTextField` 을 만들고 기본 위치를 배치 합니다.

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

두 번째로, 부모 `NSTableCellView`에 새 이미지 뷰와 텍스트 필드를 포함 해야 합니다.

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

마지막으로 테이블 뷰 셀을 사용 하 여 축소 하 고 확장할 수 있는 텍스트 필드에 지시 해야 합니다.

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

예제 출력:

[![앱에서 이미지를 표시] 하는 예제 (image-images/tables01.png "앱에서 이미지를 표시") 하는 예제](image-images/tables01-large.png#lightbox)

테이블 뷰 작업에 대 한 자세한 내용은 [테이블 뷰](~/mac/user-interface/table-view.md) 설명서를 참조 하세요.

<a name="Using_Images_with_Outline_Views" />

## <a name="using-images-with-outline-views"></a>개요 보기가 포함 된 이미지 사용

`NSOutlineView`에서 셀의 일부로 이미지를 포함 하려면 개요 `NSTableViewDelegate's` `GetView` 뷰의 메서드에서 데이터를 반환 하는 방법을 변경 하 여 일반적인 `NSTextField`대신를 `NSTableCellView` 사용 해야 합니다. 예:

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

여기에는 몇 줄의 관심이 있습니다. 첫째, 이미지를 포함 하려는 열에 대해 필요한 크기와 위치를 새로 `NSImageView` 만들고, 이미지를 사용 하 고 있는지 여부에 따라 새 `NSTextField` 을 만들고 기본 위치를 배치 합니다.

```csharp
if (tableColumn.Title == "Product") {
    view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
    view.AddSubview (view.ImageView);
    view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
} else {
    view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
}
```

두 번째로, 부모 `NSTableCellView`에 새 이미지 뷰와 텍스트 필드를 포함 해야 합니다.

```csharp
view.AddSubview (view.ImageView);
...

view.AddSubview (view.TextField);
...

```

마지막으로 테이블 뷰 셀을 사용 하 여 축소 하 고 확장할 수 있는 텍스트 필드에 지시 해야 합니다.

```csharp
view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
```

예제 출력:

[![개요 뷰에 표시 되는 이미지의 예](image-images/outline01.png "개요 뷰에 표시 되는 이미지의 예")](image-images/outline01-large.png#lightbox)

개요 보기를 사용 하는 방법에 대 한 자세한 내용은 [개요 보기](~/mac/user-interface/outline-view.md) 설명서를 참조 하세요.

## <a name="summary"></a>요약

이 문서에서는 Xamarin.ios 응용 프로그램에서 이미지 및 아이콘을 사용 하는 방법에 대해 자세히 살펴봅니다. Xcode의 Interface Builder에서 이미지와 아이콘을 사용 하는 방법 및 코드에서 C# 이미지와 아이콘을 사용 하는 방법에 대 한 다양 한 형식 및 사용 방법을 살펴보았습니다.

## <a name="related-links"></a>관련 링크

- [MacImages(샘플)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [테이블 보기](~/mac/user-interface/table-view.md)
- [개요 보기](~/mac/user-interface/outline-view.md)
- [macOS X 휴먼 인터페이스 지침](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [OS X용 고해상도 정보](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
