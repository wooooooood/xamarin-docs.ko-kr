---
title: Xamarin.ios의 응용 프로그램 아이콘
description: 이 문서에서는 Xamarin.ios에서 다양 한 응용 프로그램 아이콘을 사용 하는 방법, 즉 응용 프로그램 아이콘 자체, 스포트라이트 아이콘, 설정 아이콘 및 iTunes 아트 워크를 사용 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/22/2017
ms.openlocfilehash: a0c5821b91c6394b8bae4a090899fa81eb4756e2
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763756"
---
# <a name="application-icons-in-xamarinios"></a>Xamarin.ios의 응용 프로그램 아이콘

다음 항목에 대해 자세히 설명합니다.

- [응용 프로그램, 스포트라이트 및 설정 아이콘](#icon-types) -iOS 앱에 필요한 여러 종류의 아이콘이 있습니다.
- [자산 카탈로그를](#managing) 사용 하 여 아이콘 관리-자산 카탈로그를 사용 하 여 응용 프로그램 아이콘 관리
- [ITunes 아트 워크](#itunes) -응용 프로그램을 제공 하는 임시 방법에 필요한 ITunes 아트 워크를 제공 합니다.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>응용 프로그램, 스포트라이트 및 설정 아이콘

Xamarin.ios 앱이 UI 컨트롤 및 문서 아이콘에 이미지 자산을 사용할 수 있는 것과 동일한 방식으로 이미지 자산을 사용 하 여 응용 프로그램 아이콘을 제공할 수 있습니다. IPad의 다음 스크린샷은 iOS에서 아이콘의 세 가지 사용을 보여 줍니다.

- **응용 프로그램 아이콘** -모든 iOS 앱은 응용 프로그램 아이콘을 정의 해야 합니다. 사용자가 앱을 시작 하기 위해 iOS 홈 화면에서 탭 하는 아이콘입니다. 또한이 아이콘은 Game Center에서 사용 됩니다 (해당 하는 경우). 예제: 

    [![](app-icons-images/000.png "응용 프로그램 아이콘")](app-icons-images/000-full.png#lightbox)
- **스포트라이트 아이콘** -사용자가 스포트라이트 검색에서 앱 이름을 입력할 때마다이 아이콘이 표시 됩니다. 예제: 

    [![](app-icons-images/000a.png "스포트라이트 아이콘")](app-icons-images/000a-full.png#lightbox)
- **설정 아이콘** -사용자가 iOS 장치에서 **설정** 앱을 입력 하면 앱에 대 한 **설정** 목록 끝에이 아이콘이 표시 됩니다. 예제: 

    [![](app-icons-images/000b.png "설정 아이콘")](app-icons-images/000b-full.png#lightbox)

다음 이미지 자산 크기 및 해상도는 iOS 5에서 iOS 9까지 ios 5 이상을 대상으로 하는 Xamarin.ios 앱에 필요한 모든 아이콘 형식을 지원 하기 위해 필요 합니다.

### <a name="iphone-icon-sizes"></a>iPhone 아이콘 크기

- **iPhone: iOS 9 & 10 (iPhone 6 & 7 Plus)**

    ||3x|
    |---|---|
    |애플리케이션 아이콘|180x180|
    |스포트라이트|120x120|
    |설정|87x87|

- **iPhone: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |애플리케이션 아이콘|60x60<sup>1</sup>|120x120|
    |스포트라이트|40x40<sup>2</sup>|80x80|
    |설정|-|-|

- **iPhone: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |애플리케이션 아이콘|57x57|114x114|
    |스포트라이트|29x29|58x58|
    |설정|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad 아이콘 크기

- **iPad: iOS 9 & 10**

    ||2x (iPad Pro)|
    |---|---|
    |애플리케이션 아이콘|167x167<sup>6</sup>|
    |스포트라이트|120x120<sup>6</sup>|
    |설정|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |애플리케이션 아이콘|76x76|152x152|
    |스포트라이트|40x40|80x80|
    |설정|-|-|

- **iPad: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |애플리케이션 아이콘|72x72|144x144|
    |스포트라이트|50x50|100x100|
    |설정|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Mac용 Visual Studio와 Xcode 모두 더 이상 iOS 7에 대 한 1x 이미지 설정을 지원 하지 않습니다.
 2. 자산 카탈로그를 사용 하는 경우 iOS 7에 대 한 1x 이미지 설정이 지원 되지 않습니다.
 3. iOS 7 & 8은 iOS 5 & 6과 동일한 이미지 크기를 사용 합니다.
 4. 스포트라이트 아이콘과 동일한 이미지 및 크기를 사용 합니다.
 5. IPhone과 동일한 크기의 아이콘을 사용 합니다.
 6. 자산 카탈로그 이미지 집합 에서만 지원 됩니다.

 아이콘에 대 한 자세한 내용은 Apple의 [아이콘 및 이미지 크기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) 설명서를 참조 하세요.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>자산 카탈로그를 사용 하 여 아이콘 관리

아이콘의 경우 특수 `AppIcon` 이미지 집합을 앱 프로젝트의 `Assets.xcassets` 파일에 추가할 수 있습니다. 모든 해상도를 지 원하는 데 필요한 모든 이미지 버전은 _xcasset_ 에 포함 되며 함께 그룹화 됩니다. 개발자는 Mac용 Visual Studio의 특수 편집기를 사용 하 여 이러한 이미지를 그래픽으로 포함 하 고 설정할 수 있습니다.

자산 카탈로그를 사용 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **앱 아이콘** 섹션까지 아래로 스크롤합니다.
3. **원본** 드롭다운 목록에서 **AppIcon** 이 선택 되어 있는지 확인 합니다. 

    ![](app-icons-images/migrate01.png "AppIcon가 선택 되어 있는지 확인 합니다.")
4. **솔루션 탐색기**에서 `Assets.xcassets` 파일을 두 번 클릭 하 여 편집용으로 엽니다. 

    ![](app-icons-images/asset01.png "솔루션 탐색기 assets.xcassets 파일")
5. 자산 `AppIcon` 목록에서를 선택 하 여 다음을 `Icon Editor`표시 합니다.

    ![](app-icons-images/asset02.png "AppIcon 편집기")
6. 지정 된 아이콘 종류를 클릭 하 고 필요한 형식/크기에 대 한 이미지 파일을 선택 하거나 폴더에서 이미지를 끌어서 원하는 크기에 놓습니다.
7. **열기** 단추를 클릭 하 여 프로젝트에 이미지를 포함 하 고 xcasset에 설정 합니다.
8. 필요한 모든 이미지에 대해 반복 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. **솔루션 탐색기**에서 **info.plist** 파일을 두 번 클릭 합니다.

    ![](app-icons-images/icon01w.png "Info.plist를 선택 합니다.")
2. **시각적 자산** 탭을 클릭 하 고 **앱 아이콘**에서 **Asset Catalog 사용** 단추를 클릭 합니다. 

    ![](app-icons-images/icon02w.png "시각적 자산 탭을 선택 합니다.")
3. **솔루션 탐색기**에서 **Asset Catalog** 폴더를 확장 합니다. 

    ![](app-icons-images/image009.png "Asset Catalog 폴더를 확장 합니다.")
4. **미디어** 파일을 두 번 클릭 하 여 편집기에서 엽니다. 

    ![](app-icons-images/image010.png "편집기에서 미디어 파일을 엽니다.")
5. 개발자는 **속성 탐색기** 에서 필요한 아이콘의 다양 한 유형과 크기를 선택할 수 있습니다.
6. 지정 된 아이콘 유형을 클릭 하 고 필요한 유형/크기에 대 한 이미지 파일을 선택 합니다.
7. **열기** 단추를 클릭 하 여 프로젝트에 이미지를 포함 하 고 xcasset에 설정 합니다.
8. 필요한 모든 이미지에 대해 반복 합니다.

-----

이 방법은 응용 프로그램, 스포트라이트 및 앱에 대 한 설정 아이콘을 제공 하는 데 사용 되는 이미지 자산을 포함 하 고 관리 하는 데 선호 되는 방법입니다.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Info.plist에서 자산 카탈로그로 마이그레이션

`Info.plist` 파일을 사용 하 여 아이콘을 관리 하는 기존 xamarin.ios 앱의 경우 개발자가 내 `AppIcons` `Assets.xcassets`이미지 자산을 사용 하도록 전환 하는 것이 좋습니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. **앱 아이콘** 섹션까지 아래로 스크롤합니다.
3. **원본** 드롭다운 목록에서 **자산 카탈로그로 마이그레이션을**선택 합니다. 

    ![](app-icons-images/migrate02.png "자산 카탈로그로 마이그레이션을 선택 합니다.")
4. `Info.plist` 파일에 정의 된 모든 기존 아이콘은에 `Assets.xcassets`추가 된 `AppIcons` 이미지 집합으로 마이그레이션됩니다. 

     ![](app-icons-images/migrate03.png "Assets.xcassets에 설정 된 AppIcons 이미지입니다.")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. IPhone 아이콘 섹션을 클릭 합니다. 

    ![](app-icons-images/image007.png "보고서 iPhone 아이콘 편집기")
3. **아이콘** 섹션까지 아래로 스크롤합니다.
4. **자산 카탈로그** 드롭다운 목록에서 **자산 카탈로그 사용**을 선택 합니다.
5. `Info.plist` 파일에 정의 된 모든 기존 아이콘은에 `Assets.xcassets`추가 된 `Images` 집합으로 마이그레이션됩니다.
6. 변경 내용을 `Info.plist` 파일에 저장합니다.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes 아트워크

앱을 제공 하는 임시 방법 (회사 사용자 또는 실제 장치에서 베타 테스트를 위해)을 사용 하는 경우 개발자는 iTunes에서 앱을 나타내는 데 사용 되는 512x512와 1024x1024 이미지를 포함 해야 합니다.

iTunes 아트워크를 지정하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.
2. 편집기의 **ITunes 아트 워크** 섹션으로 스크롤합니다. 

    ![](app-icons-images/itunes01.png "편집기의 iTunes 아트 워크 섹션으로 스크롤합니다.")
3. 누락 된 이미지의 경우 편집기에서 축소판 그림을 클릭 하 고 파일 열기 대화 상자에서 원하는 iTunes 아트 워크의 이미지 파일을 선택 하 고 **확인** 단추를 클릭 합니다.
4. 앱에 필요한 모든 이미지가 지정 될 때까지이 단계를 반복 합니다.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. `Info.plist` **솔루션 탐색기** 파일을 두 번 클릭 하 여 편집용으로 엽니다.

2. **시각적 자산** 탭을 클릭 하 고 **iTunes 아트 워크**를 확장 합니다. 

    ![](app-icons-images/itunes01w.png "Visual Studio에서 iTunes 아트 워크 편집")
3. 누락 된 이미지의 경우 편집기에서 축소판 그림을 클릭 하 고 파일 열기 대화 상자에서 원하는 iTunes 아트 워크의 이미지 파일을 선택 하 고 **열기** 단추를 클릭 합니다.
4. 앱에 필요한 모든 이미지가 지정 될 때까지이 단계를 반복 합니다.

-----

## <a name="related-links"></a>관련 링크

- [이미지 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
