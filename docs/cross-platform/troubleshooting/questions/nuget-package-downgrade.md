---
title: NuGet 패키지를 다운그레이드하려면 어떻게 할까요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: 33f542d0da48f0cd3f7e1bcb85ae06137d8be3cd
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70288323"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>NuGet 패키지를 다운그레이드하려면 어떻게 할까요?

Mac용 Visual Studio & Visual Studio에는 이전 버전의 패키지를 선택 하 고 자동으로 설치 하는 기능이 있습니다. 패키지 업데이트의 작동 방법과 비슷합니다. 이러한 단계는 아래에 설명 되어 있습니다.

## <a name="visual-studio"></a>Visual Studio

1. **도구 > NuGet 패키지 관리자 > 패키지 관리자 콘솔로** 이동
2. **기본 프로젝트** 에서 프로젝트 설정
3. 다음 구문을 사용 합니다.

    > 설치 패키지 [PackageName]-버전 [버전 메뉴에 대 한 탭]

패키지의 NuGet 페이지에서 정확한 명령을 복사 하 여 붙여 넣을 수도 있습니다. Xamarin.ios에 대 한 예제:[https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Mac용 Visual Studio

1. 프로젝트에서 패키지 폴더를 마우스 오른쪽 단추로 클릭 & **패키지 추가** 를 선택 합니다.
2. Searchbar에서 다음 구문을 사용 하 여 필요한 패키지를 검색할 수 있습니다.

    `[PackageName] version:*`

### <a name="examples"></a>예 
- 모든 Xamarin Forms 패키지를 나열 합니다. 

    `Xamarin.Forms version:`

- 모든 Xamarin.ios 1.4. x 패키지를 나열 합니다. 

    `Xamarin.Forms version:1.4`

*참고: 버전 번호 & 사이 `version:` 에 공백을 추가 하는 경우에는 버전이 지정 되지 않은 것 처럼 검색 됩니다.*
