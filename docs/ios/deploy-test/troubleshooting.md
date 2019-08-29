---
title: Xamarin.iOS 테스트 및 배포 - 문제 해결
description: 이 문서에서는 코드 서명, 프로비저닝, TestFlight 및 Mac 빌드 호스트에서 Windows로 iOS 앱 번들 복사와 관련된 문제 해결 팁을 제공합니다.
ms.prod: xamarin
ms.assetid: 65286D09-F74D-4F22-B6CD-D1BCD7FC7992
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/23/2017
ms.openlocfilehash: a290f29707bd59a22f612f31e544a211488eba0d
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121491"
---
# <a name="xamarinios-testing-and-deployment---troubleshooting"></a>Xamarin.iOS 테스트 및 배포 - 문제 해결

## <a name="code-signing--provisioning"></a>코드 서명 및 프로비전

iOS를 통한 코드 서명 및 프로비전은 매우 어려울 수 있으므로 코드 서명 인증서 및 프로비전 프로필이 순서대로 있는지 확인해야 합니다.

- 대규모 팀은 가능한 한 Xcode에서 다음과 같은 "문제 해결" 단추를 사용하지 않아야 합니다.

    [![](troubleshooting-images/fixissue.png "문제 해결 대화 상자")](troubleshooting-images/fixissue.png#lightbox)

    그러면 새 프로비전 프로필과 인증서가 만들어집니다. 이렇게 하면 팀 구성원이 프로비전 프로필을 클릭할 때마다 해당 프로필을 만들어 프로필의 순서가 혼란스럽게 될 수 있습니다. 최악의 경우 회사의 다른 모든 사용자에 대한 인증서를 철회하여 앱의 작동이 중지됩니다.

- 키 집합 액세스를 체계적으로 유지하고, 만료된 인증서 및 프로필은 삭제합니다. 엔터프라이즈 인증서는 3년 동안 지속되지만, 다른 인증서는 1년 동안만 지속됩니다. 인증서는 갱신할 수 없으므로 이전 인증서가 만료되기 직전에 새 인증서를 만들어야 합니다. 이전 인증서를 철회 및 삭제하고 새 인증서로 앱에 다시 서명해야 합니다.

- 새 프로비전 프로필이 설치되면 이전 프로비전 프로필을 제거합니다. 즉 Mac용 Visual Studio는 사용할 프로필을 결정할 수 없습니다. 이렇게 하려면 먼저 Apple Developer Center에서 프로필을 삭제한 다음, *기본 설정 > 계정 > 자세히 보기...* 로 차례로 이동합니다. 프로비전 프로필을 선택하고 **Finder에 표시**를 클릭합니다. 그러면 Mac 파일 시스템에서 프로필의 위치가 표시되며 Finder를 사용하여 삭제할 수 있습니다.

- 필요한 모든 인증서와 해당 프라이빗 키를 사용할 수 있는지 확인합니다. 각 팀마다 개발자 인증서(자체 디바이스에 앱을 설치함)와 배포 인증서(다른 디바이스에 설치함)가 필요합니다.

- 새 프로비전 프로필 또는 인증서가 설치되면 Mac/Visual Studio용 Xcode와 Visual Studio를 다시 시작합니다.

## <a name="testflight"></a>TestFlight

테스트가 계획대로 원활하게 수행되지 않는 경우도 있습니다.  다음 단계는 TestFlight와 관련된 문제를 해결하는 데 도움이 될 수 있습니다.

- TestFlight는 iOS 8 이상을 대상으로 하는 앱에서만 사용할 수 있습니다.

- 베타 자격이 있는 *앱 스토어 배포 프로필*이 있어야 합니다.

- **새 iOS 앱 제출** 창은 앱의 **Info.plist**와 정확히 동일한 정보를 포함해야 하며 모든 섹션을 채워야 합니다. TestFlight에 업로드하기 전에 앱에 대한 아이콘을 지정해야 합니다.

- 새 빌드를 업로드하는 경우 iTunes Connect에 빌드가 나타날 때까지 1~5분 정도 걸립니다.

- [TestFlight 베타 테스트 스위치](~/ios/deploy-test/testflight.md#beta-testing)는 앱의 버전 각각에 대해 설정해야 합니다.

- 내부 테스터이기도 한 개발자 팀의 각 구성원이 **내부 테스터** 스위치를 설정해야 합니다.

- 다른 iTunes Connect 계정에 속하거나 이 계정을 소유하고 있는 사용자는 내부 테스터가 될 수 없습니다. 이러한 사용자는 외부 테스터로만 추가할 수 있습니다.

- 내부 및 외부 사용자는 별도로 추가, 선택 및 초대합니다. 각 목록은 별도로 관리해야 합니다.

- Apple은 외부 테스터에게 배포할 각 빌드를 승인해야 합니다. 빌드 버전이 변경되면 Apple에 의한 새로운 베타 검토가 필요합니다. 빌드 번호가 변경되면 검토는 선택 사항입니다.

- 외부 테스터에게 배포되는 빌드에는 메타데이터가 추가되어야 합니다. 이는 **My Apps > 시험판**에서 빌드 번호를 클릭하여 액세스할 수 있습니다.

- 검토를 위해 매일 두 개의 빌드만 제출할 수 있습니다. 버전이 변경되면 검토가 강제로 수행되므로 버전 번호는 하루에 두 번만 변경할 수 있습니다.

<a name="Automatically_copy_app_bundles_back_to_Windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>.app 번들을 Windows로 다시 자동 복사

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]
