---
title: "Mac 앱 구성"
description: "이 가이드에서는 게시할 Xamarin.Mac 앱을 구성하는 방법을 안내합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: 8f9294c10f8d3287a2985ede9aadf84ce663c38a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="mac-app-configuration"></a>Mac 앱 구성

_이 가이드에서는 게시할 Xamarin.Mac 앱을 구성하는 방법을 안내합니다._


## <a name="mac-app-configuration"></a>Mac 앱 구성

Mac용 Visual Studio에서 Mac 응용 프로그램 프로젝트를 마우스 오른쪽 단추로 클릭하고 **옵션**을 선택합니다.


### <a name="application-settings"></a>응용 프로그램 설정

Xamarin.Mac 앱의 응용 프로그램 설정을 변경하려면 **Solution Pad**에서 **Info.plist** 파일을 두 번 클릭합니다.

![Info.plist 파일 선택](app-configuration-images/config04.png "Info.plist 파일 선택")

그러면 앱에 사용 가능한 옵션이 표시됩니다.

 [![Info.plist 파일 편집](app-configuration-images/config01.png "Info.plist 파일 편집")](app-configuration-images/config01-large.png)

Xamarin.Mac으로 만든 Mac 응용 프로그램을 실행하려면 다음과 같은 시스템 요구 사항이 필요합니다.

- Mac OS X 10.7 이상을 실행하는 Mac 컴퓨터.


### <a name="signing-settings"></a>서명 설정

**프로젝트 옵션** 대화 상자의 **Mac 서명** 섹션에서 개발자는 Apple 앱 스토어를 통해 테스트, 셀프 릴리스 또는 릴리스에 사용할 Xamarin.Mac 앱을 서명할 수 있습니다.

[![Mac 서명 편집기](app-configuration-images/config02.png "Mac 서명 창")](app-configuration-images/config02-large.png)

여기서 앱이 컴파일될 때 앱을 서명하는 데 사용되는 ID, 프로비전 프로필 및 사용자 지정 자격을 선택합니다. 개발자는 필요에 따라 다른 Mac에 앱을 설치하는 데 사용되는 설치 관리자를 서명할 수 있습니다.


### <a name="build-settings"></a>빌드 설정

**프로젝트 옵션** 대화 상자의 **Mac 빌드** 섹션에서 개발자는 Xamarin.Mac 앱의 아키텍처를 선택하고, 앱에서 지원할 macOS 버전을 제어하고, 필요에 따라 앱이 컴파일되면 설치 패키지를 만들 수 있습니다.

 [![빌드 설정 편집](app-configuration-images/config03.png "빌드 설정 편집")](app-configuration-images/config03-large.png)


## <a name="related-links"></a>관련 링크

- [설치](/visualstudio/mac/installation/)
- [Hello, Mac 샘플](~/mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
