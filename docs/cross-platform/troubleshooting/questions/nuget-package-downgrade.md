---
title: NuGet 패키지를 다운그레이드하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: davidortinau
ms.author: daortin
ms.date: 05/08/2018
ms.openlocfilehash: 0c70859845915a821bb83b0f9d29528634b1a5de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73013616"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet 패키지를 다운그레이드하려면 어떻게 할까요?

Mac용 Visual Studio & Visual Studio에는 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능이 있습니다. 패키지 업데이트의 작동 방법과 비슷합니다. 이러한 단계는 아래에 설명 되어 있습니다.

## <a name="visual-studio"></a>Visual Studio

1. **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔로** 이동
2. **기본 프로젝트** 에서 프로젝트 설정
3. 다음 구문을 사용 합니다.

    > 설치 패키지 [PackageName]-버전 [버전 메뉴에 대 한 탭]

패키지의 NuGet 페이지에서 정확한 명령을 복사 하 여 붙여 넣을 수도 있습니다. Xamarin.ios에 대 한 예제: [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio for Mac

1. 프로젝트에서 패키지 폴더를 마우스 오른쪽 단추로 클릭 & **패키지 추가** 를 선택 합니다.
2. Searchbar에서 다음 구문을 사용 하 여 필요한 패키지를 검색할 수 있습니다.

    `[PackageName] version:*`

### <a name="examples"></a>예제 

- 모든 Xamarin Forms 패키지를 나열 합니다. 

    `Xamarin.Forms version:`

- 모든 Xamarin.ios 1.4. x 패키지를 나열 합니다. 

    `Xamarin.Forms version:1.4`

> [!NOTE]
> 버전 번호 & `version:` 사이에 공백을 추가 하는 경우 검색은 버전이 지정 되지 않은 것 처럼 동작 합니다.
