---
title: '이유는 내 iOS 빌드 실패: 키 집합에 없는 유효한 iPhone 코드 서명 키가?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 75f9a78fdc7d15df217378491f016478cde369ff
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351103"
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>이유는 내 iOS 빌드 실패: 키 집합에 없는 유효한 iPhone 코드 서명 키가?

## <a name="cause-of-the-error"></a>오류의 원인
이 오류 메시지 때 해당 프로젝트를 유효한 코드 서명 자격 증명을 찾고 발생 되지만 찾을 수 없습니다. 테스트 및 실제 iOS 장치에 배포에 반드시 코드 서명 뿐만 아니라 임시 및 앱 빌드를 저장 합니다. 


### <a name="provisioning-devices"></a>장치를 프로 비전
전에 iOS 장치를 프로 비전 하지 않은 경우 다음 가이드 하면 전체 단계별 과정: [장치 프로 비전 가이드](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>IOS 시뮬레이터를 사용 하는 경우 버그

> [!NOTE]
> Visual Studio 용 Xamarin의 최신 버전에서이 문제가 해결 되었습니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.


시뮬레이터에 Entitlements.plist codesign 빌드의; 추가할 Xamarin.Forms 템플릿에서 iOS 프로젝트를 일으키는 Xamarin.Visual Studio 3.11에 버그가 있었습니다. 시뮬레이터를 사용 하 여 테스트 효과적으로 차단 합니다.

### <a name="how-to-fix"></a>해결 방법
제거 하 여 문제를 해결 하면는 `<CodesignEntitlements>` 디버그에서 플래그.csproj 파일을 만듭니다. 이 작업은 다음과 같이 수행할 수 있습니다.

*경고:.csproj 파일에서 오류 프로젝트를 중단 하 수 있으므로이 작업을 수행 하기 전에 파일을 백업 하는 것이 좋습니다.*

1. 솔루션 창에서 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **프로젝트 언로드**
2. 프로젝트를 다시 클릭 마우스 오른쪽 단추로 선택한 **[ProjectName].csproj 편집**
3. 디버그 Propertygroup을 찾습니다, 그리고 다음과 같이 표시 하는 플래그를 사용 하 여 시작 해야 합니다.
   - 디버그: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - 릴리스: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. 시뮬레이터를 사용 하는 빌드 중 각각의 삭제 또는 속성 주석: `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. 프로젝트를 다시 로드 하 고 시뮬레이터에 배포할 수 있게 됩니다.

### <a name="next-steps"></a>다음 단계
추가 지원이 필요한 경우, 우리에 게 문의 하 여 위의 정보를 활용 하 여 한 후에이 문제가 여전히 발생 하는 경우를 참조 하세요 [Xamarin에 대해 사용할 수 있는 지원 옵션?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션을 제안에 대 한 정보에 대 한 방법 뿐만 아니라 필요한 경우 새 버그를 파일입니다. 
