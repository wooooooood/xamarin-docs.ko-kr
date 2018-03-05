---
title: "자동 프로비저닝"
description: "Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드에서는 Mac용 Visual Studio의 자동 서명을 사용하여 개발 인증서와 프로필을 요청합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/17/2017
ms.openlocfilehash: d7532d052c57ad46caca0cd6d6ce26d0e77dc05f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="automatic-provisioning"></a>자동 프로비저닝

_Xamarin.iOS가 성공적으로 설치된 후 iOS 개발의 다음 단계는 iOS 장치를 프로비전하는 것입니다. 이 가이드에서는 Mac용 Visual Studio의 자동 서명을 사용하여 개발 인증서와 프로필을 요청합니다._

## <a name="requirements"></a>요구 사항

- Mac용 Visual Studio 7.3 이상
- Xcode 9 이상

> [!IMPORTANT]
>  이 가이드에서는 Mac용 Visual Studio를 사용하여 배포용 Apple 장치를 설정하는 방법과 응용 프로그램을 배포하는 방법을 설명합니다. 이 작업을 수행하는 방법이나 Windows에서 Visual Studio를 사용하여 이 작업을 수행하는 방법에 대한 수동 단계는 [수동 프로비저닝](~/ios/get-started/installation/device-provisioning/manual-provisioning.md) 가이드의 자세한 단계를 참조하는 것이 좋습니다.

## <a name="enabling-automatic-signing"></a>자동 서명 사용

자동 서명 프로세스를 시작하기 전에 [Apple 계정 관리](~/cross-platform/macios/apple-account-management.md) 가이드에 설명된 대로 Mac용 Visual Studio에 추가된 Apple ID가 있는지 확인해야 합니다. Apple ID를 추가했다면 모든 관련 _팀_을 사용할 수 있습니다. 따라서 팀에 대해 인증서, 프로필 및 다른 ID를 만들 수 있습니다. 팀 ID는 프로비저닝 프로필에 포함될 앱 ID에 대한 접두사를 만들 때도 사용됩니다. 이 요소가 있으면 Apple이 신원을 확인할 수 있습니다.

IOS 장치에 배포할 앱에 자동으로 서명하려면 다음을 수행합니다.

1. Mac용 Visual Studio에서 iOS 프로젝트를 엽니다.

2. **Info.plist** 파일을 엽니다.

    ![Info.plist 파일](automatic-provisioning-images/image1.png)

3. **서명** 섹션의 **팀** 드롭다운에서 해당하는 팀을 선택합니다.

    ![팀 선택기 드롭다운](automatic-provisioning-images/image2.png)

4. 아래 그림과 같이 **자동으로 서명 관리** 확인란을 선택합니다. 그러면 자동 서명 프로세스가 시작되고, Mac용 Visual Studio가 앱 ID, 프로비저닝 프로필 및 서명 ID를 생성하고 서명에 사용할 이러한 아티팩트를 설정합니다. 이 확인란을 선택하면 서명 ID를 선택하는 데 사용되는 수동 컨트롤이 비활성화됩니다.

    ![자동으로 팀 관리 옵션](automatic-provisioning-images/image3.png)

5. 새로 생성된 인증서 및 프로비저닝 프로필을 사용하려면 프로젝트 파일을 수정해야 함을 알리는 다음과 같은 대화 상자가 표시됩니다.

    ![프로젝트 파일을 수정해야 함을 알리는 대화 상자](automatic-provisioning-images/image4.png)

6. 몇 초 후 서명 인증서 및 프로비저닝 프로필이 생성되고 표시됩니다.

    ![성공적으로 생성된 인증서와 프로필](automatic-provisioning-images/image5.png)

    자동 서명이 실패하면 **자동 서명 패드**에 오류의 원인이 표시됩니다.

## <a name="triggering-automatic-provisioning"></a>자동 프로비저닝 트리거

자동 서명이 활성화되면 다음과 같은 상황이 발생할 경우 필요에 따라 Mac용 Visual Studio에서 이러한 아티팩트를 업데이트합니다.

* iOS 장치가 mac에 연결된 경우
    - 장치가 Apple Developer Portal에 등록되었는지 자동으로 검사합니다. 그렇지 않다면 추가하고 이를 포함하는 새 프로비저닝 프로필을 생성합니다.
* 앱의 번들 ID가 변경된 경우
    - 앱 ID를 업데이트합니다. 이 앱 ID를 포함하는 새 프로비저닝 프로필이 생성됩니다.
* 지원되는 기능은 Entitlements.plist 파일에 활성화됩니다.
    - 이 기능은 앱 ID에 추가되고, 업데이트된 앱 ID가 포함된 새 프로비저닝 프로필이 생성됩니다.
    - 일부 기능은 현재 지원되지 않습니다. 지원되는 기능에 대한 자세한 내용은 [기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드를 참조하세요.


## <a name="related-links"></a>관련 링크

- [무료 프로비전](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [앱 배포](~/ios/deploy-test/app-distribution/index.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [Apple - 앱 배포 가이드](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
