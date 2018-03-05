---
title: "프로비전 프로필"
description: "이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 프로비전 프로필을 만드는 방법을 안내합니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: bdff6c32-f7e3-4a97-a093-dbda48be8227
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: 4bb6f0c219fc973d3d2e458445c76fd7611681ec
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="provisioning-profiles"></a>프로비전 프로필

개발자는 프로비전 프로필을 통해 몇 가지 macOS(이전 이름: Mac OS X) 특정 기능(예: iCloud 및 푸시 알림)을 Xamarin.Mac 앱에 통합시킬 수 있습니다. 이러한 기능을 사용하는 개발 중인 각 응용 프로그램에 대해 Mac 프로비전 프로필을 만들고 다운로드하고 설치해야 합니다.

[ ![](profiles-images/certif13.png "Apple 프로비전 포털")](profiles-images/certif13.png)

<a name="Development_Provisioning_Profile" />

## <a name="development-provisioning-profile"></a>개발 프로비전 프로필

개발 프로비전 프로필을 사용하면 Mac 앱 스토어 대상 앱을 프로필에 설정된 특정 컴퓨터에서 테스트할 수 있습니다. iCloud 및 푸시 알림과 같은 macOS 기능을 사용할 때 특히 유용합니다.

> [!NOTE]
> 개발자는 개발 프로비전 프로필을 만들기 전에 Mac 개발 인증서를 미리 만들어야 합니다. 빌드를 생성하는 데 사용할 수 있는 **개발 프로비전 프로필**을 생성하려면 이 스크린샷을 참고하여 세부 정보를 제공합니다. **인증서** 상자에 선택할 수 있는 유효한 Mac 개발 인증서와 테스트를 위해 등록한 시스템이 하나 이상 있어야 합니다.

다음을 수행합니다.

1. 만들 프로비전 프로필 유형을 선택하고 **계속** 단추를 클릭합니다. 

     [ ![](profiles-images/certif14.png "프로필 유형 선택")](profiles-images/certif14.png)
2. 프로필을 만들 응용 프로그램의 ID를 선택하고 **계속** 단추를 클릭합니다. 

     [ ![](profiles-images/certif15.png "앱 ID 선택")](profiles-images/certif15.png)
3. 프로필에 서명하는 데 사용되는 개발자 ID를 선택하고 **계속**을 클릭합니다. 

     [ ![](profiles-images/certif16.png "개발자 ID 선택")](profiles-images/certif16.png)
4. 프로필을 사용할 수 있는 컴퓨터를 선택하고 **계속**을 클릭합니다. 

     [ ![](profiles-images/certif17.png "허용된 컴퓨터 선택")](profiles-images/certif17.png)
5. 이제 **프로필 이름**을 입력하고 **생성** 단추를 클릭합니다. 

     [ ![](profiles-images/certif18.png "프로필 생성")](profiles-images/certif18.png)
6. **다운로드** 단추를 클릭하여 새 프로필을 다운로드합니다. 

     [ ![](profiles-images/certif19.png "프로필 다운로드")](profiles-images/certif19.png)
7. 개발 프로비전 프로필은 Mac **시스템 기본 설정** 응용 프로그램의 Profiles Preferences(프로필 기본 설정) 창에 설치됩니다. 

     [ ![](profiles-images/certif20.png "프로필 설치")](profiles-images/certif20.png)
8. Profiles Preferences(프로필 기본 설정) 창에 설치된 모든 프로필이 표시됩니다. 

     [ ![](profiles-images/image47.png "설치된 모든 프로필 표시")](profiles-images/image47.png)
9. 프로필을 다시 다운로드해야 하는 경우에는 **Developer Certificate Utility**(개발자 인증서 유틸리티)에도 프로필이 표시됩니다. 

     [ ![](profiles-images/image48.png "개발자 인증서 유틸리티")](profiles-images/image48.png)

새 개발 프로비전 프로필은 새 앱 각각에 대해 만들거나 테스트할 새 컴퓨터를 추가하여 테스트할 때 만들어야 합니다.

<a name="Production_Provisioning_Profile" />

## <a name="production-provisioning-profile"></a>프로덕션 프로비전 프로필

프로덕션 프로비전 프로필은 Mac 앱 스토어에 제출할 패키지를 빌드하는 데 필요합니다.

다음을 수행합니다.

1. 만들 프로필 유형을 선택하고 **계속** 단추를 클릭합니다. 

    [ ![](profiles-images/certif21.png "프로필 유형 선택")](profiles-images/certif21.png)
2. 프로필을 만들 앱의 ID를 선택하고 **계속** 단추를 클릭합니다. 

    [ ![](profiles-images/certif15.png "앱 ID 선택")](profiles-images/certif15.png)
3. 프로필에 서명할 회사 ID를 선택하고 **계속** 단추를 클릭합니다. 

    [ ![](profiles-images/certif23.png "회사 ID 선택")](profiles-images/certif23.png)
4. **프로필 이름**을 입력하고 **생성** 단추를 클릭합니다. 

    [ ![](profiles-images/certif24.png "프로필 생성")](profiles-images/certif24.png)
5. **다운로드**를 클릭하여 프로비전 프로필 파일(확장 `.provisionprofile`)을 다운로드합니다. 

    [ ![](profiles-images/certif25.png "프로필 다운로드")](profiles-images/certif25.png)
6. **Xcode 구성 도우미**로 끌어다 놓거나 두 번 클릭하여 설치합니다. 그러면 프로필이 Xcode 구성 도우미에 표시됩니다. 

    [ ![](profiles-images/image51.png "프로필 설치")](profiles-images/image51.png)
7. 프로비전 프로필도 목록에 표시됩니다. 

    [ ![](profiles-images/certif26.png "설치된 프로필 표시")](profiles-images/certif26.png)


개발자가 앱 ID로 사용 중인 기능(예: iCloud 또는 푸시 알림을 사용하도록 설정)을 변경하면 해당 앱 ID에 대한 프로비전 프로필을 다시 만들어야 합니다.

## <a name="related-links"></a>관련 링크

- [설치](~//mac/get-started/installation.md)
- [Hello, Mac 샘플](~//mac/get-started/hello-mac.md)
- [Mac 앱 스토어에서 앱 배포](https://developer.apple.com/devcenter/mac/checklist/)
- [도구 가이드: 앱 코드 서명](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
