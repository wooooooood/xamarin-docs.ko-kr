---
title: Xamarin.ios의 이미지 및 아이콘
description: 이 섹션에는 Xamarin.ios 앱에서 이미지 작업을 수행 하는 다양 한 문서가 포함 되어 있습니다. 예를 들어 아이콘으로 사용 하거나, 화면을 시작 하거나, 컨트롤에 포함 하 고, 사용자 지정 문서 형식에 대 한 아이콘을 제공 합니다.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 6940e07c51dbc19615454e0c51188152db22c63f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767223"
---
# <a name="images-and-icons-in-xamarinios"></a>Xamarin.ios의 이미지 및 아이콘

_이 섹션에는 Xamarin.ios 앱에서 이미지 작업을 수행 하는 다양 한 문서가 포함 되어 있습니다. 예를 들어 아이콘으로 사용 하거나, 화면을 시작 하거나, 컨트롤에 포함 하 고, 사용자 지정 문서 형식에 대 한 아이콘을 제공 합니다._

IOS 앱 내에서 이미지 자산을 사용 하는 방법에는 여러 가지가 있습니다. 단순히 응용 프로그램 ui의 일부로 이미지를 표시 하 여 `UIButton` 또는 `UIImageView`와 같은 ui 컨트롤에 할당 하 여 아이콘 및 시작 화면을 제공 하는 것부터 xamarin.ios를 사용 하면 다음과 같은 방식으로 ios 앱에 유용한 아트 워크를 쉽게 추가할 수 있습니다. 

- **해상도 독립적 이미지** – 다양 한 장치 해상도 및 유형 (IPhone, iPad 등)에서 이미지 작업을 위한 iOS의 기본 제공 지원을 사용 합니다.
- **자산 카탈로그 이미지 집합** - **자산 카탈로그 이미지 집합** 을 사용 하 여 앱에 필요한 지정 된 이미지 자산의 모든 버전을 관리 하 고 그룹화 합니다.
- **Ios 디자이너의 이미지** -ios 디자이너를 사용 하 여 컨트롤에 대 한 이미지를 설정 합니다.
- **코드의 이미지** – `UIImage` 클래스의 메서드를 사용 하 여 이미지 자산을 로드 하 고 작업 하 여 코드의 C# UI 컨트롤에 할당 합니다.
- **응용 프로그램 아이콘** -모든 iOS 앱에 필요한 앱 아이콘을 정의 합니다. 사용자가 앱을 시작 하기 위해 iOS 홈 화면에서 탭 하는 아이콘입니다. 또한이 아이콘은 Game Center에서 사용 됩니다 (해당 하는 경우).
- **스포트라이트 아이콘** -앱의 스포트라이트 아이콘을 정의 합니다. 사용자가 스포트라이트 검색에서 앱 이름을 입력할 때마다이 아이콘이 표시 됩니다.
- **설정 아이콘** -앱의 **설정** 아이콘을 정의 합니다. 사용자가 iOS 장치에서 **설정** 앱을 입력 하면 앱에 대 한 설정 목록 끝에이 아이콘이 표시 됩니다. 
- **화면 시작** -앱의 시작 화면을 정의 합니다. 사용자가 앱 아이콘을 탭 하 고 첫 번째 보기가 표시 되기 전에 빈 화면이 표시 됩니다. 다행히 iOS에는 스토리 보드를 사용 하 여 빈 화면 대신 이미지를 표시 하는 기능이 포함 되어 있습니다. 
- **ITunes 아이콘** -itunes 아이콘을 제공 합니다. 앱을 제공 하는 임시 방법 (회사 사용자 또는 실제 장치에서 베타 테스트를 위한 임시 방법)을 사용 하는 경우 개발자는 iTunes에서 앱을 나타내는 데 사용 되는 512x512 및 1024x1024 이미지를 포함 해야 합니다.
- **문서 아이콘** -xamarin.ios 앱에서 지원 하거나 만드는 특정 문서 종류에 대 한 아이콘으로 이미지를 사용 합니다.

IOS 앱에 대 한 이미지 자산을 만들 때 고려해 야 할 몇 가지 고려 사항 및 이러한 자산이 사용 되는 여러 위치가 있습니다. 이러한 각 항목은 필요한 이미지 자산의 수 뿐만 아니라 해당 자산을 만드는 방법에도 영향을 줍니다. 다음 항목에서는 필요한 이미지 자산 유형, 응용 프로그램 번들에 이러한 자산이 포함 되는 방법 및 필요한 기능을 제공 하기 위해 이미지 자산을 사용 하는 방법에 대해 설명 합니다.

## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

이 문서에서는 Xamarin.ios 앱에 이미지 자산을 포함 하 고 코드를 사용 C# 하거나 iOS 디자이너의 컨트롤에 해당 이미지를 할당 하 여 해당 이미지를 표시 하는 방법에 대해 설명 합니다.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[애플리케이션 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md)

이 문서에서는 앱 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple은 앱의 아이콘 관리를 허용 하는 iOS 10.3에 대 한 몇 가지 향상 된 기능을 추가 했습니다.

- `ApplicationIconBadgeNumber`-Springboard에서 앱 아이콘의 배지를 가져오거나 설정 합니다.
- `SupportsAlternateIcons`-앱 `true` 에 대체 아이콘 집합이 있는 경우입니다.
- `AlternateIconName`-현재 선택한 대체 아이콘의 이름을 반환 하거나 `null` 기본 아이콘을 사용 하는 경우을 반환 합니다.
- `SetAlternameIconName`-이 메서드를 사용 하 여 앱의 아이콘을 지정 된 대체 아이콘으로 전환할 수 있습니다.

## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)

이 문서에서는 특수 한 유형의 Storyboard를 사용 하 여 모든 iOS 장치 크기 및 해상도에 대 한 범용 시작 화면을 제공 하는 방법을 설명 합니다.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[사용자 지정 문서 형식](~/ios/app-fundamentals/images-icons/custom-document-types.md)

이 문서에서는 사용자 지정 문서 형식 아이콘으로 사용할 Xamarin.ios 앱의 이미지 자산을 포함 하 고 관리 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [이미지 작업 (샘플)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithimages)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
