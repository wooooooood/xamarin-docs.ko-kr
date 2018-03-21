---
title: "NuGet 패키지를 다운 그레이드 하는 합니까?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 7befac774732d6f9e432d43ac9bdc635b25bf431
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet 패키지를 다운 그레이드 하는 합니까?

Mac 용 visual Studio 및 Visual Studio는 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능이 와 마찬가지로 어떻게 작동 패키지 업데이트 됩니다. 다음이 단계에 대 한 설명은 다음과 같습니다.

## <a name="visual-studio"></a>Visual Studio
1. 로 이동 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**
2. 프로젝트 설정 **기본 프로젝트**
3. 이 구문을 사용 합니다.

    > Install-package [PackageName]-[버전 메뉴에 대 한 탭] 버전

있습니다 수 또한 복사/붙여넣기 패키지의 NuGet 페이지에서 정확한 명령입니다. Xamarin.Forms에 대 한 예: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac
1. 프로젝트의 패키지 폴더를 마우스 오른쪽 단추로 클릭 및 선택 **패키지 추가**
2. searchbar 필요한 패키지를 검색 하려면 다음 구문을 사용할 수 있습니다.

    `[PackageName] version:*`

### <a name="examples"></a>예제 
- 모든 Xamarin.Forms 패키지를 나열합니다. 

    `Xamarin.Forms version:`
- 모든 Xamarin.Forms 1.4.x 패키지를 나열합니다. 

    `Xamarin.Forms version:1.4`

*참고: 사이 공백을 추가 하는 경우 `version:` &의 버전 번호 검색 처럼 동작 버전이 지정 되지 않았습니다.*

