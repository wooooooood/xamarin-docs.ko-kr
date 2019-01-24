---
title: Xamarin.iOS에서 앱 그룹 기능
description: 애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 앱 그룹 기능에 필요한 설정을 설명합니다.
ms.prod: xamarin
ms.assetid: 0A61220B-BBAC-492B-9D3B-578986E64064
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 56284f1d3c5fec479badf91852acba2bf538bddd
ms.sourcegitcommit: cb484bd529bf2d8e48e5b3d086bdfc31895ec209
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/14/2018
ms.locfileid: "53411741"
---
# <a name="app-group-capabilities-in-xamarinios"></a>Xamarin.iOS에서 앱 그룹 기능

_애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 앱 그룹 기능에 필요한 설정을 설명합니다._

앱 그룹을 사용하면 서로 다른 애플리케이션(또는 애플리케이션과 해당 확장 프로그램)이 공유 파일 저장소 위치에 액세스할 수 있습니다. 앱 그룹은 다음과 같은 데이터에 사용할 수 있습니다.

*   [Apple Watch 설정](~/ios/watchos/app-fundamentals/settings.md)
*   [공유 NSUserDefaults](~/ios/app-fundamentals/user-defaults.md)
*   [공유 파일](~/ios/watchos/app-fundamentals/parent-app.md#files)

## <a name="configure-a-new-app-group"></a>새 앱 그룹 구성

공유 위치는  [Apple Developer Center](https://developer.apple.com/account/)의  **Certificates, Identifiers & Profiles** (인증서, 식별자 및 프로필) 섹션에 구성된  [앱 그룹](https://developer.apple.com/library/content/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)을 사용하여 구성됩니다. 이 값은 각 프로젝트의 Entitlements.plist에서도 참조해야 합니다.

앱 그룹에는 일반적으로 그룹 접두사가 있는 번들 ID인 식별자가 있습니다. 예를 들어 번들 ID  `com.xamarin.WatchSettings` 에는 앱 그룹  `group.com.xamarin.WatchSettings`가 있습니다.

새 앱 그룹을 만들려면 다음을 수행합니다.

1.  Apple의  [iOS Developer Center](https://developer.apple.com/account/)로 이동하여  **계정** 을 열어서 로그인합니다.
2.  **Certificates, IDs & Profiles**(인증서, ID 및 프로필)을 선택합니다.
3.  **식별자**에서 **앱 그룹**을 선택하고 **+** 단추를 클릭하여 새 그룹을 만듭니다.
4.  새 그룹에 대해  **이름**  및  **식별자** 를 입력하고  **계속**  단추를 클릭합니다. 
   
    ![앱 그룹 추가 세부 정보](app-groups-capabilities-images/image52.png)

5.  **등록** 단추를 클릭하여 그룹을 만들고 **완료** 단추를 클릭하여 등록된 앱 그룹 목록으로 돌아갑니다.

## <a name="configure-an-app-to-use-app-groups"></a>앱 그룹을 사용하도록 앱 구성

앱 그룹을 만든 후 앱에서 사용할 수 있도록 앱 ID를 구성합니다.

다음을 수행합니다.

1.  Apple의  [iOS Developer Center](https://developer.apple.com/account/)로 이동하여 Apple 개발자 계정으로 로그인합니다.
2.  **Program Resources**(프로그램 리소스) 메뉴에서 **Certificates, IDs & Profiles**(인증서, ID 및 프로필)을 선택합니다.
3.  **식별자**에서 **앱 ID**를 선택하고 **+** 단추를 클릭하여 새 ID를 만듭니다.
4.  앱 ID의 이름을 입력하고 Explicit App ID(명시적 앱 ID)를 지정합니다.
5.  **App Services** 아래에서 **앱 그룹**을 사용하도록 설정한 다음, [계속] 단추를 클릭합니다.

    ![앱 그룹 앱 서비스 추가](app-groups-capabilities-images/image53.png)

6.  설정을 확인하고  **등록**  단추를 클릭하여 앱 ID를 만듭니다.
7.  **완료** 단추를 클릭하여 등록된 앱 ID 목록으로 돌아갑니다.
8.  목록에서 새로 만든 앱 ID를 선택한 다음,  **편집**  단추를 클릭합니다.

    ![목록에서 앱 ID 선택](app-groups-capabilities-images/image54.png)

9.  서비스  **앱 그룹**에서  **편집**  단추를 클릭합니다.

    ![목록에서 앱 ID 선택](app-groups-capabilities-images/image55.png)

10. 위에서 만든 앱 그룹을 선택하고  **계속**  단추를 클릭합니다.

    ![앱 그룹 추가](app-groups-capabilities-images/image56.png)

11. **할당** 단추를 클릭한 다음, **완료** 단추를 클릭하여 등록된 앱 ID 목록으로 돌아갑니다.
12. 앱 그룹을 사용할 앱(또는 확장 프로그램)에 대해 이 단계를 반복합니다.

## <a name="next-steps"></a>다음 단계
 
아래 목록에는 필요할 수도 있는 추가 단계가 설명되어 있습니다.

* 앱에서 프레임워크 네임스페이스를 사용합니다.
* 앱에 필요한 자격을 추가합니다. 필요한 자격 및 추가 방법에 대한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.
* 앱의  **iOS 번들 서명**에서  **사용자 지정 자격**이 **Entitlements.plist**로 설정되어 있는지 확인합니다. 이 설정은 디버그 및 iOS 시뮬레이터 빌드에 대한 기본 설정이  _아닙니다_ .

앱 서비스에 문제가 발생하면 주 가이드의 [문제 해결](~/ios/deploy-test/provisioning/capabilities/index.md) 섹션을 참조하세요.
