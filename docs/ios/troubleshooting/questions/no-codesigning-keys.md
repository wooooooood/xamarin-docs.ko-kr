---
title: 키 집합에 유효한 iPhone 코드 서명 키가 없다는 오류와 함께 iOS 빌드에 실패하는 이유는 무엇인가요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 04/03/2018
ms.openlocfilehash: 0c777b8d5326963e959d8bb13d81d7058caa6bde
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030943"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>키 집합에 유효한 iPhone 코드 서명 키가 없다는 오류와 함께 iOS 빌드에 실패하는 이유는 무엇인가요?

## <a name="cause-of-the-error"></a>오류의 원인

이 오류 메시지는 해당 프로젝트가 유효한 코드 서명 자격 증명을 찾고 있지만 찾을 수 없는 경우에 발생 합니다. 물리적 iOS 장치에서 테스트 및 배포에는 코드 서명이 필요 합니다. 임시 & App store 빌드도 있습니다.

### <a name="provisioning-devices"></a>프로 비전 장치

이전에 iOS 장치를 프로 비전 하지 않은 경우 다음 가이드에서 전체 단계별 프로세스를 안내 합니다. [장치 프로 비전 가이드](~/ios/get-started/installation/device-provisioning/index.md)

## <a name="bug-when-using-ios-simulator"></a>IOS 시뮬레이터를 사용 하는 경우 버그

> [!NOTE]
> 이 문제는 최신 버전의 Visual Studio 용 Xamarin에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

Xamarin. Visual Studio 3.11에 버그가 있습니다 .이로 인해 Xamarin.ios 템플릿에서 iOS 프로젝트를 info.plist 하 여 시뮬레이터 빌드에 추가 했습니다. 시뮬레이터를 사용 하 여 테스트를 효과적으로 차단 합니다.

### <a name="how-to-fix"></a>해결 방법

.Csproj 파일의 디버그 빌드에서 `<CodesignEntitlements>` 플래그를 제거 하 여 문제를 해결할 수 있습니다. 다음과 같이이 작업을 수행할 수 있습니다.

> [!WARNING]
> .Csproj 파일의 오류는 프로젝트를 중단할 수 있으므로이를 시도 하기 전에 파일을 백업 하는 것이 좋습니다.

1. 솔루션 창에서 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **프로젝트 언로드** 를 선택 합니다.
2. 프로젝트를 다시 마우스 오른쪽 단추로 클릭 하 고 **편집 [ProjectName] .csproj를 선택 합니다.**
3. 디버그 PropertyGroups를 찾으려면 다음과 같이 플래그를 사용 하 여 시작 해야 합니다.
   - 디버그: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - 릴리스: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. 시뮬레이터를 사용 하는 각 빌드에서 다음 속성을 삭제 하거나 주석으로 처리 합니다. `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. 프로젝트를 다시 로드 하 고 시뮬레이터에 배포할 수 있어야 합니다.

### <a name="next-steps"></a>다음 단계
추가 지원이 필요 하면 microsoft에 문의 하거나, 위의 정보를 사용한 후에도이 문제가 계속 발생 하는 경우 [Xamarin에 사용할 수 있는 지원 옵션](~/cross-platform/troubleshooting/support-options.md) 을 참조 하세요. 연락처 옵션, 제안 사항 및 필요한 경우 새 버그를 제출 하는 방법에 대 한 자세한 내용은을 참조 하세요. .
