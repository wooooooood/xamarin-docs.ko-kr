---
title: 응용 프로그램 아이콘
description: 이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘에 필요한 이미지를 만들고, 그 이미지를 .icns 파일로 묶고, Xamarin.Mac 프로젝트에 아이콘을 포함하는 방법을 다룹니다.
ms.prod: xamarin
ms.assetid: 675b9405-d9a7-49f0-94ad-417f10a71d11
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3603e43b4b98d1387c718d0a6010d38aa01440c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="application-icon"></a>응용 프로그램 아이콘

_이 문서에서는 Xamarin.Mac 응용 프로그램의 아이콘에 필요한 이미지를 만들고, 그 이미지를 .icns 파일로 묶고, Xamarin.Mac 프로젝트에 아이콘을 포함하는 방법을 다룹니다._


## <a name="overview"></a>개요

Xamarin.Mac 응용 프로그램에서 C# 및 .NET을 작업할 때 개발자는 *Objective-C* 및 *Xcode*에서 작업하는 개발자와 동일한 이미지 및 아이콘 도구에 액세스할 수 있습니다.

좋은 아이콘이란 Xamarin.Mac 앱의 주 목적을 달성해야 하며 사용자가 앱을 사용할 때 예상하는 경험을 제공해야 합니다. 이 문서에서는 아이콘에 필요한 이미지 자산을 만들고, 해당 자산을 `AppIcons.appiconset` 파일에 패키징하고, Xamarin.Mac 앱에서 해당 파일을 사용하기 위한 모든 단계를 다룹니다.

![AppIcons.appiconset 편집기](app-icon-images/intro01.png "AppIcons.appiconset 편집기")


## <a name="application-icon"></a>응용 프로그램 아이콘

좋은 아이콘이란 Xamarin.Mac 앱의 주 목적을 달성해야 하며 사용자가 앱을 사용할 때 예상하는 경험을 제공해야 합니다. 모든 macOS 앱은 Finder, Dock, 실행 패드 및 컴퓨터의 다른 위치에 표시할 수 있도록 여러 가지 아이콘 크기를 포함해야 합니다.


## <a name="designing-the-icon"></a>아이콘 디자인

Apple에서는 응용 프로그램의 아이콘을 디자인할 때 다음 팁을 권장합니다.

- 아이콘을 현실적이고 독특한 모양으로 만드는 방법을 고려합니다.
- macOS 앱에 iOS에 상응하는 항목이 있는 경우 iOS 앱의 아이콘 다시 사용하지 않습니다.
- 사람들이 쉽게 알아볼 수 있는 범용 이미지를 사용합니다.
- 간단하게 만듭니다.
- 아이콘이 앱 스토리를 잘 전달하도록 색과 섀도를 제한적으로 사용합니다.
- 텍스트를 제안하기 위해 실제 텍스트를 _물결선으로 표시된_ 텍스트 또는 선과 혼합하지 않습니다.
- 실제 사진을 사용하기 보다는 아이콘의 주제를 이상화한 버전을 만듭니다.
- 아이콘에 macOS UI 요소를 사용하지 않습니다.
- 아이콘에 Apple 아이콘의 복제본을 사용하지 않습니다.

Xamarin.Mac 앱의 아이콘을 디자인하기 전에 Apple의 [OS X 휴먼 인터페이스 지침](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)에서 [앱 아이콘 갤러리](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Gallery.html#//apple_ref/doc/uid/20000957-CH88-SW1) 및 [앱 아이콘 디자인](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/Designing.html#//apple_ref/doc/uid/20000957-CH87-SW1) 섹션을 읽어보세요.


## <a name="required-image-sizes-and-filenames"></a>필요한 이미지 크기 및 파일 이름

개발자가 Xamarin.Mac 앱에 사용할 다른 이미지 리소스와 마찬가지로, 앱 아이콘도 표준 버전과 Retina 해상도 버전을 제공해야 합니다. 또한 다른 이미지와 마찬가지로, 아이콘 파일 이름을 지정할 때 `@2x` 형식을 사용합니다.

- **표준해상도**  - _이미지이름_**.**_파일 이름-확장명_(예: **icon_512x512.png**)
- **고해상도**  - _이미지이름_**@2x.**_파일 이름-확장명_(예: **icon_512x512@2x.png**)

예를 들어 앱 아이콘의 512 x 512 버전을 제공하려면 파일 이름을 **icon_512x512.png** 및 **icon_512x512@2x.png**로 해야 합니다.

사용자가 보는 모든 위치에서 아이콘이 멋지게 보일 수 있도록 아래에 나열된 크기로 리소스를 제공합니다.

|파일 이름|크기(픽셀)|
|---|---|
|icon_512x512@2x.png|1024 x 1024|
|icon_512x512.png|512 x 512|
|icon_256x256@2x.png|512 x 512|
|icon_256x256.png|256 x 256|
|icon_128x128@2x.png|256 x 256|
|icon_128x128.png|128 x 128|
|icon_32x32@2x.png|64 x 64|
|icon_32x32.png|32 x 32|
|icon_16x16@2x.png|32 x 32|
|icon_16x16.png|16 x 16|

자세한 내용은 Apple의 [모든 앱 그래픽 리소스의 고해상도 버전 제공](https://developer.apple.com/library/mac/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Optimizing/Optimizing.html#//apple_ref/doc/uid/TP40012302-CH7-SW3) 설명서를 참조하세요.


## <a name="packaging-the-icon-resources"></a>아이콘 리소스 패키징

아이콘을 디자인하고 필요한 파일 크기 및 이름으로 저장하면 Mac용 Visual Studio에서 해당 아이콘을 Xamarin.Mac에 사용할 이미지 자산에 쉽게 할당할 수 있습니다.

다음을 수행합니다.

1. **Solution Pad**에서 **Assets.xcassets** > **AppIcons.appiconset**을 엽니다. 

    ![AppIcons.appiconset 편집](app-icon-images/intro01.png "AppIcons.appiconset 편집")
2. 필요한 각 아이콘 크기에서, 아이콘을 클릭하고 위에서 만든 해당 이미지 파일을 선택합니다. 

    [![아이콘 이미지 선택](app-icon-images/intro02.png "아이콘 이미지 선택")](app-icon-images/intro02-large.png#lightbox)
3. 변경 내용을 저장합니다.


## <a name="using-the-icon"></a>아이콘 사용

`AppIcons.appiconset` 파일을 작성한 후에는 Mac용 Visual Studio의 Xamarin.Mac 프로젝트에 할당해야 합니다.

다음을 수행합니다.

1. **Solution Pad**에서 **Info.plist**를 두 번 클릭하여 **프로젝트 옵션**을 엽니다.
2. **Mac OS X 응용 프로그램 대상** 섹션에서 **앱 아이콘**을 클릭하여 `AppIcons.appiconset` 파일을 선택합니다. 

    [![아이콘 집합 설정](app-icon-images/icon01.png "아이콘 집합 설정")](app-icon-images/icon01-large.png#lightbox)
3. 변경 내용을 저장합니다.

앱이 실행되면 새 아이콘이 도크에 표시됩니다.

![macOS dock의 앱 아이콘 예제](app-icon-images/icon04.png "macOS dock의 앱 아이콘 예제")


## <a name="summary"></a>요약

이 문서에서는 macOS 앱 아이콘을 만드는 데 필요한 이미지를 작업하고, 아이콘을 패키징하고, Xamarin.Mac 프로젝트에 아이콘을 포함하는 방법을 자세히 살펴보았습니다.


## <a name="related-links"></a>관련 링크

- [MacImages(샘플)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [이미지 작업](~/mac/app-fundamentals/image.md)
- [macOS Human Interface 지침 - 아이콘 및 이미지](https://developer.apple.com/macos/human-interface-guidelines/icons-and-images/image-size-and-resolution/)
- [OS X용 고해상도 정보](https://developer.apple.com/library/content/documentation/GraphicsAnimation/Conceptual/HighResolutionOSX/Introduction/Introduction.html)
- [Icns Builder](https://itunes.apple.com/us/app/icns-builder/id554660130?mt=12)
