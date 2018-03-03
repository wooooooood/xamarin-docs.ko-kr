---
title: "이미지 및 아이콘"
description: "이 섹션에는 다양 한 화면을 시작할 이미지 아이콘으로 사용할 때 처럼 Xamarin.iOS 앱에서는 작업을 포함 하는 문서에 포함 하 여 컨트롤 및 사용자 지정 문서 유형에 대 한 아이콘을 제공 하 합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 904ba5db84101651d10605fadf8e8861db0ddc1f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="images-and-icons"></a>이미지 및 아이콘

_이 섹션에는 다양 한 화면을 시작할 이미지 아이콘으로 사용할 때 처럼 Xamarin.iOS 앱에서는 작업을 포함 하는 문서에 포함 하 여 컨트롤 및 사용자 지정 문서 유형에 대 한 아이콘을 제공 하 합니다._

해당 이미지 자산 iOS 앱 내에서 사용 되는 여러 가지가입니다. 단순히와 같은 UI 컨트롤에 할당 하는 응용 프로그램의 UI의 일환으로 이미지를 표시 한 `UIButton` 또는 `UIImageView`, 아이콘 및 시작 화면을 제공 하에 Xamarin.iOS 손쉽게 iOS 앱에 다음과 같은 방법으로 멋진 아트 워크를 추가 하려면: 

- **해상도 독립적인 이미지** – 이미지 작업에 다른 장치 해상도와 형식 (iPhone, iPad 등)에 대 한 iOS의 기본 제공 지원을 사용 합니다.
- **자산 카탈로그 이미지 집합** -사용 하 여 **자산 카탈로그 이미지 집합** 관리 하 고 모든 버전의 응용 프로그램에 필요한 특정된 이미지 자산을 그룹화 합니다.
- **IOS 디자이너의에서 이미지** -iOS 디자이너를 사용 하 여 컨트롤에 대 한 이미지를 설정 합니다.
- **코드 이미지** – 사용은 `UIImage` 클래스의 메서드를 로드 및 이미지 자산을 사용 하 고 C# 코드에서 UI 컨트롤에 할당 합니다.
- **응용 프로그램 아이콘** -모든 iOS 앱에 필요한 앱 아이콘을 정의 합니다. 이 사용자는 탭 아이콘에서 앱을 시작 하려면 iOS 홈 화면입니다. 또한 해당 하는 경우이 아이콘 게임 센터에서 사용 됩니다.
- **아이콘을 강조할** -응용 프로그램의 스포트라이트 아이콘을 정의 합니다. 스포트라이트 검색에서 응용 프로그램의 이름을 입력할 때마다이 아이콘이 표시 됩니다.
- **설정 아이콘** -응용 프로그램의 정의 **설정을** 아이콘입니다. 사용자가을 입력 하는 경우는 **설정을** 이 아이콘은 iOS 장치에서 앱의 응용 프로그램에 대 한 설정 목록 끝에 표시 됩니다. 
- **화면을 시작할** -응용 프로그램의 실행 화면을 정의 합니다. 사용자가 앱 아이콘을 탭 한 후 및 첫 번째 뷰를 표시 될 때까지 빈 화면이 표시 됩니다. 다행히 iOS 스토리 보드를 사용 하 여 빈 화면 대신 이미지를 표시 하는 것에 대 한 지원이 포함 되어 있습니다. 
- **iTunes 아이콘** -iTune 아이콘을 제공 합니다. (또는 회사 사용자에 대 한 실제 장치에서 베타 테스트에 대 한) 응용 프로그램으로 제공할 임시 방법을 사용 하 고, 개발자는 또한 512 x 512와 iTunes에 앱을 나타내는 데 사용할 수는 1024 x 1024 이미지를 포함 하도록 해야 합니다.
- **문서 아이콘** -Xamarin.iOS 앱을 지원 하거나 만듭니다는 특정 문서 종류에 대 한 이미지를 아이콘으로 사용 합니다.

IOS 앱 뿐만 아니라 해당 자산 사용될지 여러 위치에 대 한 이미지 자산을 만들 때 고려해 해야 하는 몇 가지 고려 사항이 있습니다. 이러한 각 뿐 아니라 이미지 자산 규모는 필요할 수 있지만, 이러한 자산을 만드는 방법을에 영향을 줄 합니다. 다음 항목에서는 유형의 필요한 이미지 자산, 자산 응용 프로그램의 번들에 포함 된 방법 및 필요한 기능을 제공 하도록 이미지 자산 사용 되는 방법을 설명 합니다.


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너의에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[응용 프로그램 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md)

응용 프로그램 아이콘으로 사용할 Xamarin.iOS 앱의 이미지 자산 관리 및 포함 하 여이 문서 다룹니다.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple이 iOS 10.3 해당 아이콘을 관리 하는 응용 프로그램을 사용할 수 있는 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -응용 프로그램 아이콘의 배지는 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` -If `true` 응용 프로그램에 다른 아이콘 집합입니다.
 - `AlternateIconName` -현재 선택 된 대체 아이콘의 이름이 반환 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘을 응용 프로그램의 아이콘을 전환 하려면이 메서드를 사용 합니다.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)

이 문서에서는 모든 iOS 장치 크기 및 해상도 대 한 유니버설 시작 화면을 제공 하는 특수 한 유형의 스토리 보드를 사용 하 여 설명 합니다.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[사용자 지정 문서 유형](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Xamarin.iOS 앱에서 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서 다룹니다.



## <a name="related-links"></a>관련 링크

- [이미지 (샘플) 작업](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
