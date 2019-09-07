---
title: Apple Watch 장치에서 테스트
description: 이 문서에서는 실제 Apple Watch를 테스트 하기 위해 Xamarin으로 빌드된 watchOS 앱을 배포 하는 방법을 설명 합니다. 장치, 프로 비전 프로필 및 테스트에 대해 설명 하 고 몇 가지 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: 6d3756f4215174e17ec45518f430dc38270e3289
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768697"
---
# <a name="testing-on-apple-watch-devices"></a>Apple Watch 장치에서 테스트

앱 Id 및 앱 그룹을 만드는 [배포 단계](~/ios/watchos/deploy-test/index.md) 를 수행한 후 (필요한 경우)이 페이지의 지침을 사용 하 여 다음을 수행 합니다.

- Apple 개발자 센터에서 [장치를 설정](#devices) 하 고
- [개발 프로 비전 프로필을 만든](#profiles)다음
- Apple Watch를 [배포 하 고 테스트](#testing) 합니다.

<a name="devices" />

## <a name="devices"></a>장치

실제 iPhone 또는 iPad에서 iOS 앱을 테스트 하려면 장치를 개발자 센터에 등록 해야 합니다. 장치 목록은 다음과 같습니다. 새 장치를 추가 하려면 더하기 **+** 기호를 클릭 합니다.

![](device-images/devices-sml.png "장치 목록은 다음과 같습니다.")

Watch는 다르지 않습니다. 이제 앱을 배포 하기 전에 Apple Watch 장치를 추가 해야 합니다. **Xcode** (**Windows > 장치** 목록)를 사용 하 여 watch의 udid를 찾습니다. 쌍을 이루는 전화가 연결 되 면 감시 정보도 표시 됩니다.

[![](device-images/xcode-devices-sml.png "쌍을 이루는 조사식 정보")](device-images/xcode-devices.png#lightbox)

Watch의 UDID를 알고 있으면 개발자 센터의 장치 목록에 추가 합니다.

![](device-images/devices-watch-sml.png "장치 목록에서 조사식의 UDID")

시청 장치를 추가한 후에는 새로 만들거나 기존 개발 또는 임시 프로 비전 프로필에서 해당 장치를 선택 했는지 확인 합니다.

![](device-images/devices-provisioning.png "사용 가능한 장치 목록")

다운로드 하 여 다시 설치 하기 위해 기존 프로 비전 프로필을 편집 하는 경우 잊지 마세요.

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>개발 프로 비전 프로필

장치에서 테스트를 빌드하기 위해 솔루션에서 각 앱 ID에 대 한 **개발 프로 비전 프로필** 을 만들어야 합니다.

와일드 카드 앱 ID가 있는 경우 *프로 비전 프로필은 하나만 필요 합니다*. 하지만 각 프로젝트에 대해 별도의 앱 ID가 있는 경우 각 앱 ID에 대 한 프로 비전 프로필이 필요 합니다.

![](device-images/provisioningprofile-development.png "개발 프로 비전 프로필")

세 프로필을 모두 만들면 목록에 표시 됩니다. 각 항목을 다운로드 하 여 설치 해야 합니다.

![](device-images/provisioningprofiles.png "사용 가능한 개발 프로 비전 프로필")

**빌드 > IOS 번들 서명** 화면을 선택 하 고 **릴리스** 또는 **디버그 IPhone** 구성을 선택 하 여 **프로젝트 옵션** 에서 프로 비전 프로필을 확인할 수 있습니다.

**프로 비전 프로필** 목록에 일치 하는 모든 프로필이 표시 됩니다 .이 드롭다운 목록에서 사용자가 만든 일치 하는 프로필을 확인 해야 합니다.

![](device-images/options-selectprofile.png "프로 비전 프로필 목록")

<a name="testing" />

## <a name="testing-on-a-watch-device"></a>시청 장치에서 테스트

장치, 앱 Id 및 프로 비전 프로필을 구성 했으면 테스트할 준비가 된 것입니다.

1. IPhone이 연결 되어 있고 Watch가 이미 iPhone과 페어링 되었는지 확인 합니다.

2. 구성이 **릴리스** 또는 **디버그**로 설정 되었는지 확인 합니다.

3. 대상 목록에서 연결 된 iPhone 장치를 선택 했는지 확인 합니다.

4. 조사식 또는 확장이 아니라 iOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **시작 프로젝트로 설정**을 선택 합니다.

5. **실행** 단추를 클릭 하거나 **실행** 메뉴에서 **시작** 옵션을 선택 합니다.

6. 솔루션이 구축 되 고 iOS 앱이 iPhone에 배포 됩니다.
  IOS 앱 또는 감시 확장 프로비저닝이 올바르게 설정 되지 않은 경우 iPhone에 대 한 배포가 실패 합니다.

7. 배포가 성공적으로 완료 되 면 iPhone에서 자동으로 시계 앱을 페어링된 시계로 보내려고 시도 합니다. 앱 아이콘이 원형 *설치* 진행률 표시기와 함께 조사식 화면에 표시 됩니다.

8. Watch 앱이 성공적으로 설치 된 경우 아이콘이 조사식 화면에 그대로 남아 있습니다. 앱 테스트를 시작 하는 데 터치 합니다.

## <a name="troubleshooting"></a>문제 해결

배포 하는 동안 오류가 발생 하는 경우 **보기 > 패드** 를 사용 하 여 오류에 대 한 자세한 정보를 확인 > 합니다. 일부 오류 및 원인은 다음과 같습니다.

### <a name="error-mt3001-could-not-aot-the-assembly"></a>오류 MT3001: 어셈블리를 AOT 수 없습니다.

Apple Watch 장치에 배포 하기 위해 디버그 모드에서 빌드할 때 이러한 현상이 발생할 수 있습니다.

이 문제를 *일시적* 으로 해결 하려면 조사식 확장 **프로젝트 옵션 > 빌드 > watchOS 빌드** 창에서 **증분 빌드** 를 사용 하지 않도록 설정 합니다.

[![](device-images/disable-incremental-sml.png "증분 빌드 확인란")](device-images/disable-incremental.png#lightbox)

이 문제는 향후 릴리스에서 수정 될 예정 이며, 그 후에는 증분 빌드를 다시 사용 하도록 설정 하 여 더 빠른 빌드 시간을 활용할 수 있습니다.

### <a name="watch-app-fails-to-start-while-debugging-on-device"></a>장치에서 디버깅 하는 동안 감시 앱이 시작 되지 않음

물리적 장치에서 조사식 응용 프로그램을 디버깅 하려고 할 때 & 로딩 회전자 아이콘이 표시 됩니다 (결과적으로 시간 초과 됨). 이 문제는 향후 릴리스에서 해결 될 예정입니다. 해결 방법은 디버깅을 허용 하지 않는 릴리스 빌드를 실행 하는 것입니다.

### <a name="invalid-application-executable-or-application-verification-failed"></a>응용 프로그램 실행 파일이 잘못 되었거나 응용 프로그램을 확인 하지 못했습니다.

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "응용 프로그램 실행 파일이 잘못 되었습니다. 경고")

앱이 설치를 시도한 후 이러한 메시지가 *조사식 화면에* 표시 되는 경우 몇 가지 문제가 있을 수 있습니다.

- 시청 장치 자체가 Apple 개발자 센터에서 장치로 추가 되지 않았습니다. 지시에 따라 [장치를 올바르게 구성](#devices)합니다.

- 테스트에 사용 되는 개발 프로 비전 프로필에 감시 장치가 포함 되어 있지 않습니다. 또는 프로 비전 프로필에 조사식이 추가 된 후에 다시 다운로드 하 여 다시 설치 하지 않은 것입니다. 지침에 따라 [프로 비전 프로필을 올바르게 구성](#profiles)합니다.

- **IOS 장치 로그** 에이 포함 `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` 되어 있으면 Watch 앱의 **info.plist** 에 잘못 된 값 **osversion** 값이 있습니다.
  **8.2** 이어야 합니다. Xcode 6.3를 설치한 경우에는 해당 소스를 수동으로 편집 하 여 8.2로 설정 해야 할 수 있습니다.

- Watch 앱의 **info.plist** 에는 포함 되지 않은 권한 (예: 앱 그룹)이 잘못 포함 되어 있습니다.

- Watch 앱의 **앱 ID** 가 개발자 센터에서 포함 되지 않은 권한 (예: 앱 그룹)을 잘못 사용 했습니다.

### <a name="install-never-finished"></a>설치 완료 안 함

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

이 오류는 Watch 앱의 **info.plist** 파일에서 불필요 한 키 (및 유효 하지 않음)를 나타낼 수 있습니다. IOS 앱 또는 조사식 확장에 대 한 키를 시청 앱에 포함 해서는 안 됩니다.

<!--eg. NSLocationAlwaysUsageDescription -->

### <a name="waiting-for-debugger-to-connect"></a>"디버거가 연결 되기를 기다리는 중"

**응용 프로그램 출력** 창이 표시 되지 않는 경우

```csharp
waiting for debugger to connect
```

프로젝트에 포함 된 Nuget **에 대 한**종속성이 있는지 확인 합니다. 이는 인기 있는 [Microsoft Http 클라이언트 라이브러리](https://www.nuget.org/packages/Microsoft.Net.Http/)를 비롯 한 일부 microsoft 게시 라이브러리와 함께 자동으로 추가 됩니다.

**.Csproj** 에 추가 된 **Microsoft .targets. Build .targets** 파일은 배포 중에 iOS 확장의 패키징을 방해할 수 있습니다. [버그](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)를 추적할 수 있습니다.
가능한 해결 방법은 .csproj 파일을 편집 하 고 수동으로 **Microsoft. .targets. .targets** 를 마지막 요소로 이동 하는 것입니다.
