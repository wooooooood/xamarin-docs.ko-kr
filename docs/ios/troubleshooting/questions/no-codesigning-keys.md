---
title: '이유는 내 iOS 빌드 실패: 키 집합에서 찾을 수 없는 유효한 iPhone 코드 서명 키?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9DF24C46-D521-4112-9B21-52EA4E8D90D0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 5334e3009906896644caa47c715f912fa379c627
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="why-does-my-ios-build-fail-with-no-valid-iphone-code-signing-keys-found-in-keychain"></a>이유는 내 iOS 빌드 실패: 키 집합에서 찾을 수 없는 유효한 iPhone 코드 서명 키?

## <a name="cause-of-the-error"></a>오류의 가능한 원인
이 오류 메시지에는 찾을 수 없는 하지만 문제의 프로젝트에서 올바른 코드 서명 자격 증명을 찾고 때 발생 합니다. 코드 서명 하는 것은; 실제 iOS 장치에 배포에 필요 뿐만 아니라 임시 및 응용 프로그램 빌드를 저장 합니다. 


### <a name="provisioning-devices"></a>장치를 프로 비전
전에 iOS 장치를 프로 비전 하지 않은 경우 다음 지침 안내해 전체 단계별 프로세스: [장치 프로 비전 가이드](~/ios/get-started/installation/device-provisioning/index.md)


## <a name="bug-when-using-ios-simulator"></a>IOS 시뮬레이터를 사용 하는 경우 버그

> [!NOTE]
> Visual Studio 용 Xamarin의 최신 버전에이 문제가 해결 되었습니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.


시뮬레이터를 Entitlements.plist 코드 서명 빌드; 추가 하는 Xamarin.Forms 템플릿을에 iOS 프로젝트를 일으킨 Xamarin.Visual Studio 3.11 버그가 했습니다. 효율적으로 시뮬레이터를 사용 하 여 테스트를 차단 합니다.

### <a name="how-to-fix"></a>문제를 해결 하는 방법
제거 하 여이 문제 해결을 할 수 있습니다는 `<CodesignEntitlements>` 의 디버그 플래그.csproj 파일을 만듭니다. 이 작업은 다음과 같이 수행할 수 있습니다.

*경고:.csproj 파일에서 오류 수 손상 프로젝트에이 작업을 수행 하기 전에 파일을 백업 하는 것이 좋습니다.*

1. 솔루션 창에서 iOS 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **프로젝트 언로드**
2. 프로젝트를 다시 클릭 마우스 오른쪽 단추로 선택한 **[이름].csproj 편집**
3. 다음과 같은 플래그와 함께 시작, 디버그 PropertyGroups 찾도록:
   - 디버그: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|iPhoneSimulator' ">`
   - 릴리스: `<PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhoneSimulator' ">`
4. 시뮬레이터를 사용 하는 빌드가 각 삭제 하거나 주석 다음 속성으로 처리 합니다. `<CodesignEntitlements>Entitlements.plist</CodesignEntitlements>`
5. 프로젝트를 다시 로드 되 고 시뮬레이터에 배포할 수 있습니다.

### <a name="next-steps"></a>다음 단계
추가 지원을 문의 또는 위의 정보를 활용 한 후에이 문제가 여전히 발생 하는 경우를 참조 하십시오 [지원 옵션에 대해 Xamarin에 대 한 사용할 수 있는?](~/cross-platform/troubleshooting/support-options.md) 연락처 옵션, 제안 사항에 대 한 내용은 하는 방법 필요한 경우 새 버그를 파일입니다. 
