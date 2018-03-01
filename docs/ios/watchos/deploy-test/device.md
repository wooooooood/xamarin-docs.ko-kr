---
title: "조사식 장치에 대 한 테스트"
description: "Apple Watch 테스트 앱 배포"
ms.topic: article
ms.prod: xamarin
ms.assetid: A72A7D38-FAE8-4DD2-843D-54B74C5078D7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: fab2092837f9b9ca8ada53274c9644f131fb5659
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="testing-on-watch-devices"></a>조사식 장치에 대 한 테스트

한 후의 [배포 단계](~/ios/watchos/deploy-test/index.md) 이 페이지에 지침을 사용 합니다 (필요한 경우)의 앱 Id와 응용 프로그램 그룹을 만듭니다.

- [설치 하는 장치](#devices) Apple 개발자 센터에서 및
- [프로 비전 프로필 개발 만들기](#profiles), 한 다음
- [배포 및 테스트](#testing) Apple Watch 있습니다.

<a name="devices" />

## <a name="devices"></a>장치

실제 iPhone 또는 iPad에서 iOS 앱을 테스트 개발자 센터에 등록할 수 있도록 장치가 항상 필요 합니다. 장치 목록은 다음과 같습니다 (더하기 기호를 클릭  **+**  새 장치를 추가 하려면):

![](device-images/devices-sml.png "다음과 같은 장치 목록")

감시에도 마찬가지-앱을 배포 하기 전에 Apple Watch 장치를 추가 해야 합니다. 시계의 UDID 사용 하 여 검색 **Xcode** (**Windows > 장치** 목록). 쌍을 이루는 전화 연결 된 경우에 시계의 정보가 표시 됩니다.

[ ![](device-images/xcode-devices-sml.png "쌍을 이루는 조사식 정보")](device-images/xcode-devices.png)

시계의 알고 있을 때 UDID, 개발자 센터의 장치 목록에 추가 합니다.

![](device-images/devices-watch-sml.png "시계의 UDID 장치 목록에서")

조사식 장치 추가 되 면 모든 기존 또는 새 개발 또는 만들면 특별 프로 비전 프로필에 선택 되어 있는지 확인:

![](device-images/devices-provisioning.png "사용 가능한 장치 목록")

다운로드 한 다음 다시 설치 하려면 기존 프로 비전 프로필을 편집 하는 경우 기억 하십시오!

<a name="profiles" />

## <a name="development-provisioning-profiles"></a>프로 비전 프로필 개발

만들어야 하는 장치에서 테스트를 위해 작성 하는 **개발 프로 비전 프로필** 각 응용 프로그램 ID 솔루션에 대 한 합니다.

응용 프로그램 ID, 와일드 카드 있다면 *프로 비전 프로필은 하나만 있으면 됩니다*; 하지만 각 프로젝트에 대 한 별도 응용 프로그램 ID가 있는 경우 각 응용 프로그램 ID에 대 한 프로 비전 프로필을 해야 합니다.

![](device-images/provisioningprofile-development.png "개발 프로 비전 프로필")

모든 세 개의 프로필을 만든 후 목록에 나타납니다. 다운로드 하 여 각각을 설치 해야 합니다.

![](device-images/provisioningprofiles.png "사용할 수 있는 개발 프로 비전 프로필")

프로 비전 프로필을 확인할 수 있습니다는 **프로젝트 옵션** 선택 하 여는 **빌드 > iOS 번들 서명** 화면을 선택 하 고 **릴리스** 또는 **IPhone 디버그** 구성 합니다.

**프로 비전 프로필** 회원님이 만든이 드롭 다운 목록에서 일치 하는 프로필 표시 되어야-일치 하는 모든 프로필 목록에 표시 됩니다.

![](device-images/options-selectprofile.png "프로 비전 프로필 목록")


<a name="testing" />

## <a name="testing-on-a-watch-device"></a>조사식 장치 테스트

장치, 응용 프로그램 Id 및 프로비저닝 프로필을 구성 했으면, 테스트할 준비가 되었습니다.

1. IPhone 연결 되어 있고 시계는 iPhone과 이미 페어링 되었습니다. 있는지 확인 합니다.

2. 구성으로 설정 되었는지 확인 **릴리스** 또는 **디버그**합니다.

3. 확인 대상 목록에서 연결 된 iPhone 장치를 선택 합니다.

4. (하지 조사식 또는 확장 프로그램) iOS 앱 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 선택 **시작 프로젝트로 설정**합니다.

5. 클릭는 **실행** 단추 (선택 하거나는 **시작** 에서 옵션은 **실행** 메뉴).

6. 솔루션을 빌드합니다 하 고 iOS 앱은 iPhone에 배포 됩니다.
  IOS 응용 프로그램 또는 조사식 확장 프로 비전 올바르게 설정 되지 않은 경우 iPhone에 배포 하면 실패 합니다.

7. 배포가 성공적으로 완료 되 면 iPhone 자동으로 쌍을 이루는 시계를 watch 앱을 보내려고 시도 합니다. 응용 프로그램 아이콘 순환 watch 화면에 나타납니다. *설치* 진행률 표시기입니다.

8. 아이콘은 그대로 남아 watch 앱이 성공적으로 설치 하는 경우 응용 프로그램 테스트를 시작 하 여 터치 watch 화면의-!


## <a name="troubleshooting"></a>문제 해결

배포를 사용 하는 동안 오류가 발생 하는 경우는 **보기 > 패드 > 장치 로그** 오류에 대 한 추가 정보를 봅니다. 몇 가지 오류와 그 이유는 다음과 같습니다.

### <a name="error-mt3001-could-not-aot-the-assembly"></a>오류 MT3001: AOT을 어셈블리 하지 못했습니다.

이 Apple Watch 장치에 배포 하려면 디버그 모드에서 빌드하는 경우 발생할 수 있습니다.

*일시적으로* 이 문제 해결, 사용 하지 않도록 설정 **증분 빌드** 조사식 확장에서 **프로젝트 옵션 > 빌드 > watchOS 빌드** 창:

[ ![](device-images/disable-incremental-sml.png "증분 빌드 확인란")](device-images/disable-incremental.png)

이는 증분 빌드 다시 설정할 수 있습니다 빌드 시간 단축을 활용 하려면 이후 릴리스에서 수정 될 예정입니다.


#<a name="3-watch-app-fails-to-start-while-debugging-on-device"></a>장치에서 디버깅 하는 동안 시작 하려면 &#3; watch 앱 실패

표시 된 아이콘 및 로드 회전자 물리적 장치에서 조사식 응용 프로그램을 디버깅 하는 경우 (및 결국 제한 시간). 향후 릴리스에서 해결 될 예정 해결 방법 (함 디버깅을 허용 하지 것입니다) 릴리스 빌드를 실행 하는 것입니다.


### <a name="invalid-application-executable-or-application-verification-failed"></a>잘못 된 응용 프로그램 실행 파일 또는 응용 프로그램 확인 하지 못했습니다.

```csharp
Failed to install [APPNAME]
Invalid executable/Application Verification Failed
```

![](device-images/invalid-application-executable.png "잘못 된 응용 프로그램 실행 파일 경고")

이러한 메시지를 표시 하는 경우 *watch 화면의* 후 앱을 설치 하려고 했습니다, 몇 가지 문제가 될 수:

- Apple 개발자 센터에서 장치로 조사식 장치 자체 추가 되지 않았습니다. 지침에 따라 [장치를 올바르게 구성](#devices)합니다.

- 테스트용으로 사용 되 고 개발 프로 비전 프로필에 포함 된; 조사식 장치 없는 또는 이후 다시 다운로드 하 고 다시 설치 하지 않은 시계 프로 비전 프로필에 추가 되었습니다. 지침에 따라 [올바르게 프로 비전 프로필을 구성](#profiles)합니다.

- 경우는 **iOS 장치 로그** 포함 `The system version is lower than the minimum OS version specified for bundle...Have 8.2; need 8.3` 다음 조사식 응용 프로그램의 **Info.plist** 에 잘못 된 **MinimumOSVersion** 값입니다.
  이 되어야 **8.2** -소스를 수동으로 편집 해야 하는 Xcode 6.3을 설치한 경우 삽입 8.2로 설정 합니다.

- Watch 앱 **Entitlements.plist** 올바르게가 권한을 사용 하도록 설정 하지 않습니다 (예: 응용 프로그램 그룹의 경우)가 하지 않아야 합니다.

- Watch 앱 **앱 ID** 잘못 되지 않았는지 (예: 응용 프로그램 그룹의 경우)에 사용 권한을 포함할 수 없습니다. 개발자 센터에서.



### <a name="install-never-finished"></a>설치 작업이 완료 되지 않습니다

```csharp
SPErrorGizmoInstallNeverFinishedErrorMessage
```

이 오류는 Watch 앱의 불필요 한 (및 잘못 된) 키를 나타낼 수 **Info.plist** 파일입니다. Watch 앱에서 iOS 응용 프로그램 또는 조사식 확장을 위한 키에 포함 되지 않습니다.

<!--eg. NSLocationAlwaysUsageDescription -->


### <a name="waiting-for-debugger-to-connect"></a>"디버거를 연결할 때까지 기다리는"

경우는 **응용 프로그램 출력** 창 표시 중지

```csharp
waiting for debugger to connect
```

에 종속 되어 있는 프로젝트에 포함 된 NuGets 확인 **Microsoft.Bcl.Build**합니다. 이 특성은 자동으로 추가 포함 하는 인기 있는 일부 Microsoft 게시 라이브러리와 [Microsoft Http 클라이언트 라이브러리](http://www.nuget.org/packages/Microsoft.Net.Http/)합니다.

**Microsoft.Bcl.Build.targets** 파일에 추가 되 고 **.csproj** 배포 하는 동안 iOS 확장의 포장을 방해할 수 있습니다. 추적할 수 있습니다는 [버그](https://bugzilla.xamarin.com/show_bug.cgi?id=29912)합니다.
가능한 해결 방법은.csproj 파일을 편집 하 고 수동으로 이동 하는 것은 **Microsoft.Bcl.Build.targets** 마지막 요소 여야 합니다.

