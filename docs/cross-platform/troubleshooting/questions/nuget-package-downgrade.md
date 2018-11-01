---
title: NuGet 패키지를 다운그레이드하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 206336cbcdc85e5e2f3f010e947981cb96e7cd1a
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674720"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet 패키지를 다운그레이드하려면 어떻게 할까요?

Mac 용 visual Studio 및 Visual Studio가 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능 어떻게 작동 패키지 업데이트와 유사 합니다. 다음이 단계에 대 한 설명은 다음과 같습니다.

## <a name="visual-studio"></a>Visual Studio

1. 로 **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔**
2. 설정 아래의 프로젝트 **기본 프로젝트**
3. 이 구문을 사용 합니다.

    > Install-package [패키지 이름]-버전 [버전 메뉴에 대 한 탭]

또한 복사/붙여 넣을 수 있습니다 패키지의 NuGet 페이지에서 정확한 명령입니다. Xamarin.Forms에 대 한 예제: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. 프로젝트의 패키지 폴더를 마우스 오른쪽 단추로 클릭 및 선택 **패키지 추가**
2. searchbar에 필요한 패키지를 검색 하려면 다음 구문을 사용할 수 있습니다.

    `[PackageName] version:*`

### <a name="examples"></a>예제 
- 모든 Xamarin.Forms 패키지를 나열합니다. 

    `Xamarin.Forms version:`

- 모든 Xamarin.Forms 1.4.x 패키지를 나열합니다. 

    `Xamarin.Forms version:1.4`

*참고: 사이 공백을 추가 하는 경우 `version:` 및 버전 번호 검색 버전을 지정 하지 처럼 동작 합니다.*
