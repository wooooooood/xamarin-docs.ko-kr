---
title: Apple Watch 장치에서 테스트
description: 이 문서에는 실제 Apple Watch 테스트를 위해 Xamarin으로 빌드된 watchOS 앱을 배포 하는 방법을 설명 합니다. 장치를 프로 비전 프로필을 테스트에 대해 설명 하 고 몇 가지 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 9c15e9205b96a02caa182e47b71c6d36c8bff1aa
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61282948"
---
# <a name="testing-on-apple-watch-devices"></a>Apple Watch 장치에서 테스트

수행한 후 합니다 [배포 단계](~/ios/watchos/deploy-test/index.md) (필요한 경우) 앱 Id 및 앱 그룹을 만들고,이 페이지의 지침을 사용 하려면:

- [설치 하는 장치](#devices) Apple 개발자 센터에서 및
- [개발 프로 비전 프로필을 만드는](#profiles), 한 다음
- [배포 및 테스트](#testing) 는 Apple Watch.

<a name="devices" />

## <a name="devices"></a>디바이스

실제 iPhone 또는 iPad에서 iOS 앱을 테스트 개발자 센터에 등록할 수 있도록 장치에 항상 필요 합니다. 장치 목록에는 다음과 같습니다 (더하기 기호를 클릭 **+** 새 장치를 추가 하려면):

![](device-images/devices-sml.png "장치 목록에 다음과 같이 표시 됩니다.")

조사식도 다르지 않습니다-앱을 배포 하기 전에 Apple Watch 장치를 추가 해야 합니다. 시계의 UDID를 사용 하 여 찾을 **Xcode** (**Windows > 장치** 목록). 쌍을 이루는 전화 연결 되어 있을 때 시계의 정보도 표시 됩니다.

[![](device-images/xcode-devices-sml.png "쌍을 이루는 보기 정보")](device-images/xcode-devices.png#lightbox)

시계의 알고 있을 때 UDID를 개발자 센터의 장치 목록에 추가 합니다.

![](device-images/devices-watch-sml.png "시계의 장치 목록에서 UDID")

조사식 장치에 추가 되 면 모든 기존 또는 새 개발 또는 만든 임시 프로 비전 프로필에서 선택 된 확인 합니다.

![](device-images/devices-provisioning.png "사용 가능한 장치 목록")

다운로드 한 다음 다시 설치 하려면 기존 프로 비전 프로필을 편집 하는 경우 잊지 마십시오!

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>개발 프로 비전 프로필

만들어야 장치에서 테스트를 작성 하는 **개발 프로 비전 프로필** 솔루션의 각 앱 ID에 대 한 합니다.

와일드 카드 앱 ID를 있다면 *하나만 프로 비전 프로필 해야*; 각 프로젝트에 대 한 별도 앱 ID가 있는 경우 각 앱 ID에 대 한 프로 비전 프로필을 해야 하지만:

![](device-images/provisioningprofile-development.png "개발 프로 비전 프로필")

세 개의 프로필 모두 만든 경우 목록에 나타납니다. 다운로드 하 고 각각을 설치 해야 합니다.

![](device-images/provisioningprofiles.png "개발 프로 비전 프로필")

프로 비전 프로필을 확인할 수 있습니다는 **프로젝트 옵션** 를 선택 하 여 합니다 **빌드 > iOS 번들 서명** 화면을 선택 하는 **릴리스** 또는 **IPhone 디버그** 구성 합니다.

합니다 **프로 비전 프로필** 목록 일치 하는 모든 프로필에 표시 됩니다-사용자가 만든이 드롭다운 목록에서 일치 하는 프로필을 표시 합니다.

![](device-images/options-selectprofile.png "프로 비전 프로필 목록")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>조사식 장치 테스트

장치, 앱 Id 및 프로 비전 프로필을 구성한 후 테스트할 수 있습니다.

1. IPhone에서 연결 되어 있고 시계 iPhone을 사용 하 여 이미 쌍을 이룹니다 있는지 확인 합니다.

2. 구성으로 설정 되었는지 확인 **릴리스** 하거나 **디버그**합니다.

3. 연결 된 iPhone 장치 대상 목록에서 선택 되었는지 확인 합니다.

4. (없습니다 조사식 또는 확장)에 iOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다.

5. 클릭 합니다 **실행** 단추 (선택 또는 **시작** 에서 옵션을 **실행** 메뉴).

6. 솔루션 빌드하고 iOS 앱의 iPhone에 배포 됩니다.
  IOS 앱 또는 조사식 확장을 프로 비전 올바르게 설정 하지 않으면 iPhone에 배포가 실패 합니다.

7. 배포가 성공적으로 완료 되 면 하는 경우 iPhone 쌍을 이루는 조사식 watch 앱 보내기 자동으로 하려고 합니다. 앱 아이콘에 순환 watch 화면에 나타납니다 *설치* 진행률 표시기입니다.

8. 아이콘 남아 watch 앱이 성공적으로 설치 하는 경우 watch 화면에 앱을 테스트 하기 시작 하려면 터치!


## <a name="troubleshooting"></a>문제 해결

배포를 사용 하는 동안 오류가 발생 하는 경우는 **보기 > 패드 > 장치 로그** 오류에 대 한 자세한 내용은 합니다. 일부 오류와 원인이 다음과 같습니다.

### <a name="error-mt3001-could-not-aot-the-assembly"></a>오류 MT3001: AOT을 어셈블리 없습니다 수 없습니다.

이 Apple Watch 장치에 배포 하려면 디버그 모드에서 빌드하는 경우 발생할 수 있습니다.

하 *임시로* 이 문제를 해결, 사용 안 함 **증분 빌드** 조사식 확장에서 **프로젝트 옵션 > 빌드 > watchOS 빌드** 창:

[![](device-images/disable-incremental-sml.png "증분 빌드 확인란")](device-images/disable-incremental.png#lightbox)

이는 증분 빌드 다시 활성화할 수 있습니다 빌드 시간 단축을 활용 하려면, 향후 릴리스에서 수정 될 예정입니다.


### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>Watch 앱 장치에서 디버깅 하는 동안 시작 실패

물리적 장치에 아이콘 및 로드 회전자 watch 앱을 디버그 하는 동안 표시 되는 경우 (및 결과적으로 시간 제한). 향후 릴리스에서 해결 될 것이 해결 방법 (디버깅을 허용 하지 것입니다)는 릴리스 빌드를 실행 하는 것입니다.


### <a name="invalid-application-executable-or-application-verification-failed"></a>잘못 된 응용 프로그램 실행 파일 또는 응용 프로그램 확인 하지 못했습니다.

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "잘못 된 응용 프로그램 실행 경고")

이러한 메시지는 표시 하는 경우 *watch 화면의* 후 앱을 설치 하려고 했습니다, 몇 가지 문제가 있을 수 있습니다.

- Apple 개발자 센터에서 장치로 Watch 장치 자체 추가 되지 않았습니다. 지침에 따라 [장치를 올바르게 구성](#devices)합니다.

- 테스트에 사용 되는 개발 프로 비전 프로필에 포함 된; Watch 장치 없는 시계 프로 비전 프로필에 추가 된 후 다시 다운로드 되 고 다시 설치 되지 않았습니다. 지침에 따라 [프로 비전 프로필을 올바르게 구성](#profiles)합니다.

- 경우는 **iOS 장치 로그** 포함 `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` 다음 Watch 앱의 **Info.plist** 잘못 되었습니다 **MinimumOSVersion** 값입니다.
  이 되어야 **8.2** -소스를 수동으로 편집 해야 하는 Xcode 6.3을 설치한 경우 insert 8.2로 설정 합니다.

- Watch 앱의 **Entitlements.plist** 올바르게 자격 사용 하도록 설정하지 없습니다 (예: 앱 그룹)가 하지 않아야 합니다.

- Watch 앱의 **앱 ID** 제대로 되지 않은 (예: 앱 그룹) 사용 하도록 설정 하는 자격 없어야 하는 개발자 센터에서.



### <a name="install-never-finished"></a>설치 완료 되지 않습니다

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

이 오류는 Watch 앱의 불필요 한 (및 잘못 된) 키를 나타낼 수 있습니다 **Info.plist** 파일입니다. Watch 앱에서 iOS 앱 또는 조사식 확장을 위한 키를 포함 하지 않아야 합니다.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"디버거를 연결할 때까지 기다리는"

경우는 **응용 프로그램 출력** 창 표시 중지

```csharp
waiting for debugger to connect
```

하는 경우 모든 프로젝트에 포함 된 Nuget에 대 한 종속성 확인 **Microsoft.Bcl.Build**합니다. 포함 하 여 널리 사용 되는 Microsoft에서 게시 일부 라이브러리를 사용 하 여 자동으로 추가 됩니다 [Microsoft Http 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http/)합니다.

합니다 **Microsoft.Bcl.Build.targets** 에 추가 되는 파일을 **.csproj** 배포 하는 동안 iOS 확장의 포장을 방해할 수 있습니다. 추적할 수 있습니다 합니다 [버그](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)합니다.
.Csproj 파일을 편집 하 여 수동으로 이동이 가능한 문제를 해결 합니다 **Microsoft.Bcl.Build.targets** 마지막 요소 여야 합니다.

