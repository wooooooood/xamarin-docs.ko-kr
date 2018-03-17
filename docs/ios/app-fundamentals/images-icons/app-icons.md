---
title: "응용 프로그램 아이콘"
description: "응용 프로그램 아이콘으로 사용할 Xamarin.iOS 앱의 이미지 자산 관리 및 포함 하 여이 문서 다룹니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: 68372d90b0567c662f0ae43e315663832f1f769b
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="application-icons"></a>응용 프로그램 아이콘

_응용 프로그램 아이콘으로 사용할 Xamarin.iOS 앱의 이미지 자산 관리 및 포함 하 여이 문서 다룹니다._

다음 항목에 대해 자세히 설명합니다.

* [응용 프로그램, 스포트라이트 및 설정 아이콘](#icon-types) -다양 한 유형의 iOS 앱에 필요한 아이콘입니다.
* [아이콘 자산 카탈로그와 함께 관리](#managing) -자산 카탈로그를 사용 하 여 관리 응용 프로그램 아이콘입니다.
* [iTunes 아트 워크](#itunes) -응용 프로그램을 제공 하는 임시 방법에 필요한 iTunes 아트 워크를 제공 합니다.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>응용 프로그램, 스포트라이트 및 설정 아이콘

UI 컨트롤에 대 한 및 문서 아이콘으로 Xamarin.iOS 앱 이미지 자산을 사용할 수 있는 동일한 방법으로 이미지 자산 데 사용할 수 응용 프로그램 아이콘을 제공 합니다. 다음 스크린샷에서 iPad에서 iOS의 아이콘을 사용 하를 세 가지를 보여 줍니다.

- **응용 프로그램 아이콘** -모든 iOS 응용 프로그램 응용 프로그램 아이콘을 정의 해야 합니다. 이 사용자는 탭 아이콘에서 앱을 시작 하려면 iOS 홈 화면입니다. 또한 해당 하는 경우이 아이콘 게임 센터에서 사용 됩니다. 예제: 

    [![](app-icons-images/000.png "응용 프로그램 아이콘")](app-icons-images/000-full.png#lightbox)
- **아이콘을 강조할** 스포트라이트 검색에서 응용 프로그램의 이름을 입력할 때마다-이 아이콘이 표시 됩니다. 예제: 

    [![](app-icons-images/000a.png "스포트라이트 아이콘")](app-icons-images/000a-full.png#lightbox)
- **설정 아이콘** -사용자가을 입력 하는 경우는 **설정** 이 아이콘은 iOS 장치에서 앱의 끝에 표시 되는 **설정을** 응용 프로그램에 대 한 목록입니다. 예제: 

    [![](app-icons-images/000b.png "설정 아이콘")](app-icons-images/000b-full.png#lightbox)

다음 이미지 자산 크기 및 해상도 모든 iOS 9 (또는 그 이상)를 통해 iOS 5를 대상으로 하는 Xamarin.iOS 앱에 필요한 아이콘 유형을 지 원하는 데 필요 합니다.

### <a name="iphone-icon-sizes"></a>iPhone 아이콘 크기

- **iPhone: 9 및 10 iOS (iPhone 6 및 7 +)**

    ||3x|
    |---|---|
    |응용 프로그램 아이콘|180x180|
    |스포트라이트|120x120|
    |설정|87x87|

- **iPhone: iOS 7 및 8**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|60x60<sup>1</sup>|120x120|
    |스포트라이트|40x40<sup>2</sup>|80x80|
    |설정|-|-|

- **iPhone: iOS 5 및 6**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|57x57|114x114|
    |스포트라이트|29x29|58x58|
    |설정|29x29<sup>3, 4</sup>|58x58<sup>3, 4</sup>|

### <a name="ipad-icon-sizes"></a>iPad 아이콘 크기

- **iPad: iOS 9 & 10**

    ||2 x (iPad Pro)|
    |---|---|
    |응용 프로그램 아이콘|167x167<sup>6</sup>|
    |스포트라이트|120x120<sup>6</sup>|
    |설정|58x58<sup>5</sup>|

- **iPad: iOS 7 & 8**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|76x76|152x152|
    |스포트라이트|40x40|80x80|
    |설정|-|-|

- **iPad: iOS 5 & 6**

    ||1x|2x|
    |---|---|---|
    |응용 프로그램 아이콘|72x72|144x144|
    |스포트라이트|50x50|100x100|
    |설정|29x29<sup>3, 5</sup>|58x58<sup>3, 5</sup>|

 1. Mac에서 Xcode를 둘 다 Visual Studio는 더 이상 iOS 7에 대 한 1 x 이미지 설정 지원.
 2. IOS 7에 대 한 1 x 이미지 설정 자산 카탈로그를 사용 하는 경우 지원 되지 않습니다.
 3. iOS 7 및 8 iOS 5 및 6으로 동일한 이미지 크기를 사용합니다.
 4. 스포트라이트 아이콘으로 동일한 이미지 및 크기를 사용합니다.
 5. IPhone로 같은 크기 아이콘을 사용합니다.
 6. 자산 카탈로그 이미지 집합 에서만 지원 됩니다.
 
 아이콘에 대 한 자세한 내용은 Apple의를 참조 하십시오 [아이콘 및 이미지 크기](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) 설명서입니다.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>자산 카탈로그와 함께 관리 아이콘

아이콘의 경우 특별 한 `AppIcons` 이미지 집합에 추가할 수는 `Assets.xcassets` 응용 프로그램의 프로젝트 파일에에서 있습니다. 해상도 모두 지 원하는 데 필요한 이미지의 모든 버전에 포함 된는 _xcasset_ 그룹화 합니다. Mac 용 Visual Studio에서 특별 한 편집기를 사용 하면 개발자는 포함 하 고 이러한 이미지를 그래픽으로 설정 합니다.

자산 카탈로그를 사용 하려면 다음을 수행 합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 으로 아래로 스크롤하여는 **앱 아이콘** 섹션.
3. **소스** 드롭다운 목록에서 확인 **AppIcons** 을 선택 합니다. 

    ![](app-icons-images/migrate01.png "AppIcons 선택 되어 있는지 확인")
4. **솔루션 탐색기**, 두 번 클릭는 `Assets.xcassets` 편집을 위해 열 파일입니다. 

    ![](app-icons-images/asset01.png "솔루션 탐색기에서 Assets.xcassets 파일")
5. 선택 `AppIcons` 표시 하는 자산 목록에서는 `Icon Editor`: 

    ![](app-icons-images/asset02.png "AppIcons 편집기")
6. 하나 제공 된 형식 아이콘에서 클릭 하 고 필요한 형식/크기에 대 한 이미지 파일을 선택 또는 이미지에 폴더에서 끌어서 놓으면 원하는 크기에 합니다.
7. 클릭는 **열려** 단추를 프로젝트에 이미지를 포함 하 고는 xcasset에 설정 합니다.
8. 필요한 모든 이미지를 반복 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭은 **Info.plist** 파일에 **솔루션 탐색기**:

    ![](app-icons-images/icon01w.png "Info.plist 선택")
2. 클릭는 **시각적 자산** 탭을 클릭할는 **사용 하 여 자산 카탈로그** 단추 **앱 아이콘**: 

    ![](app-icons-images/icon02w.png "시각적 자산 탭 선택")
4. **솔루션 탐색기**를 확장 하 고는 **자산 카탈로그** 폴더: 

    ![](app-icons-images/image009.png "자산 카탈로그 폴더를 확장 합니다.")
5. 두 번 클릭 하 여 **미디어** 편집기에서 열려는 파일: 

    ![](app-icons-images/image010.png "편집기에서 미디어 파일을 엽니다.")
6. 아래는 **속성 탐색기** 개발자는 다양 한 유형 및 필요한 아이콘 크기를 선택할 수 있습니다.
7. 지정한 아이콘 유형을 클릭 하 고 필요한 형식/크기에 대 한 이미지 파일을 선택 합니다.
8. 클릭는 **열려** 단추를 프로젝트에 이미지를 포함 하 고는 xcasset에 설정 합니다.
9. 필요한 모든 이미지를 반복 합니다.

-----

이것은을 포함 하 고 응용 프로그램에 대 한 응용 프로그램, 스포트라이트 및 설정 아이콘을 제공 하는 데 사용 될 이미지 자산 관리 기본 방법입니다.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Info.plist에서 자산 카탈로그로 마이그레이션

사용 하 여 기존 Xamarin.iOS 앱에 대 한는 `Info.plist` 의 아이콘을 관리 하는 파일, 것이 가장 좋습니다는 개발자도 전환 사용 하 여 `AppIcons` 내 이미지 자산은 `Assets.xcassets`합니다.

다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 으로 아래로 스크롤하여는 **앱 아이콘** 섹션.
3. **소스** 드롭다운 목록에서 선택 **자산 카탈로그에 대 한 마이그레이션**: 

    ![](app-icons-images/migrate02.png "자산 카탈로그에 마이그레이션 선택")
4. 에 정의 된 항목은 기존는 `Info.plist` 파일을 마이그레이션할는 `AppIcons` 에 추가 이미지 설정 `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "AppIcons 이미지는 Assets.xcassets에서 설정")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. IPhone 아이콘 섹션 클릭 하십시오. 

    ![](app-icons-images/image007.png "지정한 iPhone 아이콘 편집기")
3. 으로 아래로 스크롤하여는 **아이콘** 섹션.
4. **자산 카탈로그** 드롭다운 목록에서 선택 **사용 하 여 자산 카탈로그**합니다.
5. 에 정의 된 항목은 기존는 `Info.plist` 파일을 마이그레이션할는 `Images` 집합에 추가 `Assets.xcassets`합니다.
6. 변경 내용을 `Info.plist` 파일에 저장합니다.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>iTunes 아트워크

(또는 회사 사용자에 대 한 실제 장치에서 베타 테스트에 대 한) 응용 프로그램으로 제공할 임시 방법을 사용 하 고, 개발자는 또한 512 x 512와 iTunes에 앱을 나타내는 데 사용할 수는 1024 x 1024 이미지를 포함 하도록 해야 합니다.

iTunes 아트워크를 지정하려면 다음을 수행합니다.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.
2. 스크롤하여는 **iTunes 아트 워크** 편집기의 섹션: 

    ![](app-icons-images/itunes01.png "ITunes 편집기의 아트 워크 섹션으로 스크롤하여")
3. 모든 누락 된 이미지에 대 한 편집기에서 축소판 그림을 클릭 파일 열기 대화 상자에서 원하는 iTunes 아트 워크에 대 한 이미지 파일을 선택 하 고 클릭는 **확인** 단추입니다.
4. 필요한 이미지가 지정 된 응용 프로그램에 대 한 모든 때까지이 단계를 반복 합니다.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. 두 번 클릭는 `Info.plist` 파일에 **솔루션 탐색기** 를 편집 하기 위해 엽니다.

2. 클릭는 **시각적 자산** 탭을 확장 하 고는 **iTunes 아트 워크**: 

    ![](app-icons-images/itunes01w.png "ITunes 아트 워크 Visual Studio에서 편집")
4. 모든 누락 된 이미지에 대 한 편집기에서 축소판 그림을 클릭 파일 열기 대화 상자에서 원하는 iTunes 아트 워크에 대 한 이미지 파일을 선택 하 고 클릭는 **열려** 단추입니다.
5. 필요한 이미지가 지정 된 응용 프로그램에 대 한 모든 때까지이 단계를 반복 합니다.

-----

## <a name="related-links"></a>관련 링크

- [이미지 (샘플) 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
