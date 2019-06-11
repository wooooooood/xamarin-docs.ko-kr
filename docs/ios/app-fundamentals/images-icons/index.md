---
title: 이미지 및 Xamarin.iOS에서 아이콘
description: 이 섹션에 시작 화면, Xamarin.iOS 앱 아이콘으로 사용 하는 등에서 이미지를 사용 하 여 작업을 설명 하는 문서 또는 포함 하는 다양 한 컨트롤 및 사용자 지정 문서 유형에 대 한 아이콘을 제공 합니다.
ms.prod: xamarin
ms.assetid: 0AB8CC07-11E4-0D75-4119-AED1A1252424
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a837d680a21b9cdbc39e42f5fa3520622e0b49aa
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827192"
---
# <a name="images-and-icons-in-xamarinios"></a>이미지 및 Xamarin.iOS에서 아이콘

_이 섹션에 시작 화면, Xamarin.iOS 앱 아이콘으로 사용 하는 등에서 이미지를 사용 하 여 작업을 설명 하는 문서 또는 포함 하는 다양 한 컨트롤 및 사용자 지정 문서 유형에 대 한 아이콘을 제공 합니다._

여러 가지 자산 iOS 앱 내에서 사용 되는 이미지입니다. 단순히와 같은 UI 컨트롤에 할당에 앱의 UI의 일부로 이미지를 표시 한 `UIButton` 또는 `UIImageView`, 아이콘 및 시작 화면을 제공 하려면 Xamarin.iOS 쉽게 다음과 같은 방법으로 iOS 앱에 뛰어난 아트 워크를 추가 하려면: 

- **해상도 독립적인 이미지** – 다른 장치 해상도 및 형식 (예: iPhone, iPad)에서 이미지를 사용 하 여 작업에 대 한 iOS의 기본 제공 지원을 사용 합니다.
- **자산 카탈로그 이미지 집합** -사용 **자산 카탈로그 이미지 집합** 관리 하 고 모든 버전의 앱에 필요한 지정 된 이미지 자산을 그룹화 합니다.
- **IOS 디자이너에서에서 이미지** -iOS 디자이너를 사용 하 여 컨트롤에 대 한 이미지를 설정 합니다.
- **코드 이미지** – 사용 합니다 `UIImage` 클래스의 메서드를 로드 및 이미지 자산으로 작업에서 UI 컨트롤에 할당할 C# 코드입니다.
- **응용 프로그램 아이콘** -모든 iOS 앱에 필요한 앱 아이콘을 정의 합니다. 이 사용자가 탭 아이콘을 앱을 시작 하려면 iOS 홈 화면에서. 또한 해당 하는 경우이 아이콘 Game Center에서 사용 됩니다.
- **아이콘 스포트라이트** -앱 추천 아이콘을 정의 합니다. 스포트라이트 검색에서 앱의 이름을 입력할 때마다이 아이콘이 표시 됩니다.
- **설정 아이콘** -앱의 정의 **설정을** 아이콘입니다. 사용자가 입력 하는 경우는 **설정을** 앱이 iOS 장치에서이 아이콘은 앱에 대 한 설정 목록 끝에 표시 됩니다. 
- **시작 화면** -앱의 시작 화면을 정의 합니다. 사용자가 앱 아이콘을 탭 한 후 및 첫 번째 보기 표시 되기 전에 빈 화면이 표시 됩니다. 다행 스럽게도 iOS 스토리 보드를 사용 하 여 빈 화면 대신 이미지를 표시 하는 것에 대 한 지원이 포함 되어 있습니다. 
- **iTunes 아이콘** -iTune 아이콘을 제공 합니다. 앱 (또는 회사 사용자에 대 한 실제 장치에서 테스트용 베타)를 제공 하는 임시 메서드를 사용 하는 경우 개발자는 또한 512x512 및 1024x1024 이미지 iTunes의 앱을 나타내는 데 사용할 포함 해야 합니다.
- **문서 아이콘** -Xamarin.iOS 앱을 지원 하거나 만드는 특정 문서 유형에 아이콘으로 이미지를 사용 합니다.

IOS 앱 뿐만 아니라 해당 자산을 사용 하는 여러 위치에 대 한 이미지 자산을 만들 때 계정에 수행 해야 하는 몇 가지 고려 사항이 있습니다. 이러한 각가 얼마나 많은 이미지 자산 해야 뿐만 아니라 해당 자산을 만드는 방법에 영향을 줍니다. 다음 항목에서는 필요한 이미지 자산, 해당 자산 응용 프로그램의 번들에 포함 된 방법 및 이미지 자산 필요한 기능을 제공 하는 데 사용 된는 어떻게 유형의 설명 합니다.


## <a name="displaying-an-imageiosapp-fundamentalsimages-iconsdisplaying-an-imagemd"></a>[이미지 표시](~/ios/app-fundamentals/images-icons/displaying-an-image.md)

이 문서에서는 이미지 자산 또는 C# 코드를 사용 하 여 iOS 디자이너에서에서 컨트롤에 할당 하 여 해당 이미지를 표시 하 고 Xamarin.iOS 앱에 포함 하 여 설명 합니다.

## <a name="application-iconsiosapp-fundamentalsimages-iconsapp-iconsmd"></a>[애플리케이션 아이콘](~/ios/app-fundamentals/images-icons/app-icons.md)

Xamarin.iOS 앱에 앱 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서에서는 다룹니다.

## <a name="alternate-app-iconsiosapp-fundamentalsimages-iconsalternate-app-iconsmd"></a>[대체 앱 아이콘](~/ios/app-fundamentals/images-icons/alternate-app-icons.md)

Apple이 앱을 해당 아이콘을 관리 하는 iOS 10.3 몇 가지 향상 된 기능 추가.

 - `ApplicationIconBadgeNumber` -앱 아이콘 배지를 Springboard에서 설정 하거나 가져옵니다.
 - `SupportsAlternateIcons` - `true` 앱 아이콘 집합을 대체 했습니다.
 - `AlternateIconName` -현재 선택한 대체 아이콘의 이름을 반환 합니다. 또는 `null` 기본 아이콘을 사용 하는 경우.
 - `SetAlternameIconName` -지정 된 대체 아이콘 앱 아이콘을 전환 하려면이 메서드를 사용 합니다.


## <a name="launch-screensiosapp-fundamentalsimages-iconslaunch-screensmd"></a>[시작 화면](~/ios/app-fundamentals/images-icons/launch-screens.md)

이 문서에서는 모든 iOS 장치 크기 및 해상도 대 한 유니버설 시작 화면을 제공 하는 특수 한 유형의 스토리 보드를 사용 하 여 설명 합니다.

## <a name="custom-document-typesiosapp-fundamentalsimages-iconscustom-document-typesmd"></a>[사용자 지정 문서 형식](~/ios/app-fundamentals/images-icons/custom-document-types.md)

Xamarin.iOS 앱에 사용자 지정 문서 형식 아이콘으로 사용할 이미지 자산 관리 및 포함 하 여이 문서에서는 합니다.



## <a name="related-links"></a>관련 링크

- [이미지 (샘플)를 사용 하 여 작업](https://developer.xamarin.com/samples/monotouch/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [사용자 지정 아이콘 및 이미지 생성 지침](https://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html)
