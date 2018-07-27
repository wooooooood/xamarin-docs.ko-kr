---
title: Xamarin.iOS 응용 프로그램 아이콘
description: '이 문서에서는 Xamarin.iOS에서 다양 한 응용 프로그램 아이콘을 사용 하는 방법을 설명 합니다: 자체 응용 프로그램 아이콘, 추천 아이콘, 설정 아이콘 및 iTunes 아트 워크.'
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: cd67c564461721ade6f3eb269b461ddea5e2d2c4
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39276004"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin.iOS 응용 프로그램 아이콘

다음 항목에 대해 자세히 설명합니다.

* [응용 프로그램, 스포트라이트 및 설정 아이콘](#icon-types) -다양 한 유형의 iOS 앱에 필요한 아이콘입니다.
* [자산 카탈로그를 사용 하 여 아이콘 관리](#managing) -자산 카탈로그를 사용 하 여 관리 응용 프로그램 아이콘입니다.
* [iTunes 아트 워크](#itunes) -응용 프로그램을 제공 하는 임시 메서드에 대 한 필수 iTunes 아트 워크를 제공 합니다.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>응용 프로그램, 스포트라이트 및 설정 아이콘

UI 컨트롤을 문서 아이콘으로 Xamarin.iOS 앱 이미지 자산을 사용할 수 있는 동일한 방법으로 이미지 자산 제공 응용 프로그램 아이콘을 사용할 수 있습니다. 다음 스크린샷에서 iPad에서 iOS에서 아이콘의 세 사용을 하는 방법을 보여 줍니다.

- **응용 프로그램 아이콘** -모든 iOS 앱에 응용 프로그램 아이콘을 정의 해야 합니다. 이 사용자가 탭 아이콘을 앱을 시작 하려면 iOS 홈 화면에서. 또한 해당 하는 경우이 아이콘 Game Center에서 사용 됩니다. 예제: 

    [![](app-icons-images/000.png "응용 프로그램 아이콘")](app-icons-images/000-full.png#lightbox)
- **아이콘 스포트라이트** 스포트라이트 검색에서 앱의 이름을 입력할 때마다-이 아이콘이 표시 됩니다. 예제: 

    [![](app-icons-images/000a.png "추천 아이콘")](app-icons-images/000a-full.png#lightbox)
- **설정 아이콘** -사용자가 입력 하는 경우는 **설정** 앱이 iOS 장치에서이 아이콘은 끝에 표시 됩니다는 **설정** 앱에 대 한 목록입니다. 예제: 

    [![](app-icons-images/000b.png "설정 아이콘")](app-icons-images/000b-full.png#lightbox)

다음 이미지 자산 크기와 해상도 모두 iOS 9 (또는 그 이상)을 통해 iOS 5를 대상으로 하는 Xamarin.iOS 앱에 필요한 아이콘 형식을 지원 하기 위해 필요 합니다.

### <a name="iphone-icon-sizes"></a>iPhone 아이콘 크기

- **iPhone: 9 및 10 iOS (iPhone 6 및 7 +)**

    ||3x|
    |---|---|
    |응용 프로그램 아이콘|180x180|
    |추천|120x120|
    |설정|87x87|

- **iPhone: iOS 7 및 8**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|60x60<sup>1</sup>|120x120|
    |추천|40x40<sup>2</sup>|80x80|
    |설정|-|-|

- **iPhone: iOS 5 및 6**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|57x57|114x114|
    |추천|29x29|58x58|
    |설정|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad 아이콘 크기

- **iPad: iOS 9 & 10**

    ||2 x (iPad Pro)|
    |---|---|
    |응용 프로그램 아이콘|167x167<sup>6</sup>|
    |추천|120x120<sup>6</sup>|
    |설정|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|76x76|152x152|
    |추천|40x40|80x80|
    |설정|-|-|

- **iPad: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|72x72|144x144|
    |추천|50x50|100x100|
    |설정|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Mac에서 Xcode를 둘 다 Visual Studio는 더 이상 ios 7 1 x 이미지 설정 지원.
 2. 자산 카탈로그를 사용 하는 경우 iOS 7 용 1x 이미지 설정 지원 되지 않습니다.
 3. iOS 5 및 6으로 동일한 이미지 크기를 사용 하는 iOS 7 및 8입니다.
 4. 동일한 이미지 및 크기를 추천 아이콘으로 사용합니다.
 5. IPhone로 동일한 크기 아이콘을 사용합니다.
 6. 자산 카탈로그 이미지 집합 에서만 지원 됩니다.
 
 아이콘에 대 한 자세한 내용은 Apple의를 참조 하세요 [아이콘 및 이미지 크기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) 설명서.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>자산 카탈로그를 사용 하 여 관리 아이콘

아이콘의 경우 특수 `AppIcon` 이미지 집합에 추가할 수 있습니다는 `Assets.xcassets` 앱의 프로젝트 파일에에서. 모든 해상도 지 원하는 데 필요한 이미지의 모든 버전에 포함 되는 _xcasset_ 있으며 두 패싯은 함께 그룹화 합니다. Mac 용 Visual Studio의 특별 한 편집기 인 개발자를 등이 이러한 이미지를 그래픽으로 설정할 수 있습니다.

자산 카탈로그를 사용 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 아래로 스크롤하여 합니다 **앱 아이콘** 섹션입니다.
3. **소스** 드롭다운 목록 **AppIcon** 선택: 

    ![](app-icons-images/migrate01.png "AppIcon 선택 되어 있는지 확인")
4. **솔루션 탐색기**를 두 번 클릭 합니다 `Assets.xcassets` 편집용으로 연 파일: 

    ![](app-icons-images/asset01.png "솔루션 탐색기에서 Assets.xcassets 파일")
5. 선택 `AppIcon` 표시할 자산 목록에서를 `Icon Editor`:

    ![](app-icons-images/asset02.png "AppIcon 편집기")
6. 중 아이콘 형식 지정 된 클릭 및 필요한 형식/크기에 대 한 이미지 파일을 선택 또는 이미지에서 폴더에서 끌어서 놓습니다 원하는 크기입니다.
7. 클릭 합니다 **열려** 프로젝트에 이미지를 포함 하는 xcasset에서 설정 단추입니다.
8. 필요한 모든 이미지에 대해 반복 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭 합니다 **Info.plist** 파일을 **솔루션 탐색기**:

    ![](app-icons-images/icon01w.png "Info.plist를 선택 합니다.")
2. 클릭 합니다 **시각적 자산** 탭을 클릭 합니다 **자산 카탈로그 사용** 단추 **앱 아이콘**: 

    ![](app-icons-images/icon02w.png "시각적 자산 탭 선택")
4. **솔루션 탐색기**를 확장 합니다 **자산 카탈로그** 폴더: 

    ![](app-icons-images/image009.png "자산 카탈로그 폴더를 확장 합니다.")
5. 두 번 클릭 합니다 **미디어** 파일 편집기에서 엽니다. 

    ![](app-icons-images/image010.png "편집기에서 미디어 파일을 엽니다.")
6. 아래는 **속성 탐색기** 개발자는 다양 한 형식 및 필요한 아이콘의 크기를 선택할 수 있습니다.
7. 지정 된 아이콘 유형에 클릭 하 고 필요한 형식/크기에 대 한 이미지 파일을 선택 합니다.
8. 클릭 합니다 **열려** 프로젝트에 이미지를 포함 하는 xcasset에서 설정 단추입니다.
9. 필요한 모든 이미지에 대해 반복 합니다.

-----

이것이 등 앱에 대 한 응용 프로그램, 스포트라이트 및 설정 아이콘을 제공 하는 데 사용할 이미지 자산을 관리 하는 기본 방법입니다.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>자산 카탈로그를 Info.plist에서 마이그레이션

사용 하 여 기존 Xamarin.iOS 앱에 대 한 합니다 `Info.plist` 의 아이콘을 관리 하는 파일, 것이 가장 좋습니다는 개발자 전환 사용 하는 `AppIcons` 내 이미지 자산은 `Assets.xcassets`.

다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 아래로 스크롤하여 합니다 **앱 아이콘** 섹션입니다.
3. **소스** 드롭다운 목록에서 **자산 카탈로그로 마이그레이션**: 

    ![](app-icons-images/migrate02.png "자산 카탈로그를 마이그레이션 선택")
4. 에 정의 된 모든 기존 아이콘을 `Info.plist` 파일을 마이그레이션할를 `AppIcons` 추가할 이미지 설정 `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Assets.xcassets AppIcons 이미지 집합")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. IPhone 아이콘 섹션 클릭 합니다. 

    ![](app-icons-images/image007.png "지정한 iPhone 아이콘 편집기")
3. 아래로 스크롤하여 합니다 **아이콘** 섹션입니다.
4. **자산 카탈로그** 드롭다운 목록에서 **사용 하 여 자산 카탈로그**합니다.
5. 에 정의 된 모든 기존 아이콘을 `Info.plist` 파일을 마이그레이션할를 `Images` 집합에 추가할 `Assets.xcassets`합니다.
6. 변경 내용을 `Info.plist` 파일에 저장합니다.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes 아트워크

앱 (또는 회사 사용자에 대 한 실제 장치에서 테스트용 베타)를 제공 하는 임시 메서드를 사용 하는 경우 개발자는 또한 512x512 및 1024x1024 이미지 iTunes의 앱을 나타내는 데 사용할 포함 해야 합니다.

iTunes 아트워크를 지정하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.
2. 스크롤하여 합니다 **iTunes 아트 워크** 편집기의 섹션: 

    ![](app-icons-images/itunes01.png "ITunes 아트 워크 편집기 섹션으로 스크롤하여")
3. 누락 된 이미지가 편집기에서 썸네일을 클릭, 파일 열기 대화 상자에서 원하는 iTunes 아트 워크에 대 한 이미지 파일을 선택 하 고 클릭 합니다 **확인** 단추입니다.
4. 필요한 이미지가 지정 된 앱에 대 한 모든 때까지이 단계를 반복 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭 합니다 `Info.plist` 파일을 **솔루션 탐색기** 을 편집용으로 엽니다.

2. 클릭 합니다 **시각적 자산** 탭을 확장 합니다 **iTunes 아트 워크**: 

    ![](app-icons-images/itunes01w.png "ITunes 아트 워크 Visual Studio에서 편집")
4. 누락 된 이미지가 편집기에서 썸네일을 클릭, 파일 열기 대화 상자에서 원하는 iTunes 아트 워크에 대 한 이미지 파일을 선택 하 고 클릭 합니다 **열고** 단추입니다.
5. 필요한 이미지가 지정 된 앱에 대 한 모든 때까지이 단계를 반복 합니다.

-----

## <a name="related-links"></a>관련 링크

- [이미지 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 만들기 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
