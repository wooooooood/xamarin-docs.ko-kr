---
title: Xamarin.iOS에서 iCloud 기능
description: 응용 프로그램에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 iCloud 기능에 필요한 설정을 설명합니다.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 13d0e7a70c91c6e3e422f2e91cefc403627a340c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105521"
---
# <a name="icloud-capabilities-in-xamarinios"></a>Xamarin.iOS에서 iCloud 기능

_응용 프로그램에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 iCloud 기능에 필요한 설정을 설명합니다._

iCloud는 iOS 사용자에게 콘텐츠를 저장하고 디바이스 간에 공유할 수 있는 편리하고 간단한 방법을 제공합니다. 개발자가 iCloud를 사용하여 사용자를 위한 저장 수단을 제공할 수 있는 방법은 네 가지입니다. 키-값 저장소, UIDocument 저장소, CoreData 및 CloudKit을 사용하여 개별 파일 및 디렉터리에 대한 저장소를 직접 제공합니다. 자세한 내용은 [iCloud 소개](~/ios/data-cloud/introduction-to-icloud.md) 가이드를 참조하세요.

iCloud 기능을 응용 프로그램에 추가하는 것은 _컨테이너_ 때문에 다른 App Services보다 약간 더 어렵습니다. 컨테이너는 iCloud에서 앱에 대한 정보를 저장하는 데 사용되며 단일 iCloud 계정에 포함된 모든 정보를 분리(예: 사용자 iOS 디바이스에서 샌드 박싱)할 수 있습니다. 컨테이너에 대한 자세한 내용은 [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md) 가이드를 참조하세요.

> [!IMPORTANT]
> Apple에서는 개발자가 유럽 연합의 GDPR(일반 데이터 보호 규정)을 제대로 처리하는 데 도움이 되는 [도구를 제공합니다](https://developer.apple.com/support/allowing-users-to-manage-data/).

<a name="icloud-developer-center" />

## <a name="developer-center"></a>개발자 센터

개발자 센터를 통해 새 앱을 프로비전할 때 수행해야 하는 단계는 두 가지입니다.

1.  컨테이너를 만듭니다.
2.  iCloud 기능으로 앱 ID를 만들어서 ID에 컨테이너를 추가합니다.
3. 이 앱 ID가 포함된 프로비전 프로필을 만듭니다.

아래 단계는 이러한 단계를 안내합니다.

1.  [Apple Developer Center](https://developer.apple.com/account/)로 이동하여 Certificates, Identifier, and Profiles(인증서, 식별자 및 프로필) 섹션으로 이동합니다. 
    
     ![Apple Developer Center 기본 페이지](icloud-capabilities-images/image22.png)

2.  **식별자** 아래에서 **iCloud Containers**를 선택한 다음, **+** 를 선택하여 새 컨테이너를 만듭니다.  
    
    ![iCloud Container 화면](icloud-capabilities-images/image23.png)

3.  iCloud 컨테이너에 대한 **설명** 및 고유 **식별자**를 입력합니다. 
    
    ![iCloud 컨테이너 등록 화면](icloud-capabilities-images/image24.png)

4.  **계속**을 눌러 정보가 정확한지 확인하고 **등록**을 눌러서 iCloud Container를 만듭니다.  
    
    ![iCloud 컨테이너 등록 화면](icloud-capabilities-images/image25.png)

새 앱 ID를 만들어서 ID에 컨테이너를 추가하려면 다음을 수행합니다.

1.  [개발자 센터](https://developer.apple.com/account/)에서 **식별자** 아래 **앱 ID**를 클릭합니다. 
    
    ![개발자 센터의 식별자 섹션](icloud-capabilities-images/image26.png)

2.  **+** 단추를 선택하여 새 앱 ID를 추가합니다. 
    
    ![새 앱 ID 추가 단추](icloud-capabilities-images/image27.png)

3.  앱 ID의 **이름**을 입력하고 **Explicit App ID**(명시적 앱 ID)를 지정합니다.
    
    ![새 앱 ID 입력 세부 정보](icloud-capabilities-images/image28.png)

4.  **App Services** 아래에서 **iCloud**를 선택하고 **Include CloudKit support**(CloudKit 지원 포함)를 선택합니다.
    
    ![iCloud 앱 서비스 선택](icloud-capabilities-images/image29.png)

5.  **계속**과 **등록**을 차례로 선택합니다. 확인 화면에 iCloud가 노란색 기호로 선택된 구성 가능과 함께 표시됩니다.   
    
    ![확인 화면](icloud-capabilities-images/image30.png)

6.  앱 ID 목록으로 돌아가서 방금 만든 항목을 선택합니다. 
    
    ![앱 ID 선택 화면](icloud-capabilities-images/image31.png)

7.  펼쳐진 섹션의 맨 아래로 스크롤하여 **편집**을 클릭합니다.
    
    ![앱 ID 편집](icloud-capabilities-images/image32.png)

8.  목록을 iCloud까지 아래로 스크롤하여 **편집** 단추를 클릭합니다.  
    
    ![iCloud 앱 ID 편집](icloud-capabilities-images/image33.png)

9.  앱 ID와 함께 사용할 컨테이너를 선택합니다.  
    
    ![컨테이너 선택 화면](icloud-capabilities-images/image34.png)

10. 컨테이너 할당을 확인하고 **할당**을 누릅니다.
 
[기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드의 설명에 따라, 이 앱 ID를 사용하여 새 프로비전 프로필을 생성하거나 다시 생성할 수 있습니다. 

iCloud 사용에 대한 자세한 내용은 다음 가이드를 참조하세요.

*   [iCloud 소개](~/ios/data-cloud/introduction-to-icloud.md)
*   [CloudKit 소개](~/ios/data-cloud/intro-to-cloudkit.md)
*   [문서 선택기 소개](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>다음 단계
 
아래 목록에는 필요할 수도 있는 추가 단계가 설명되어 있습니다.

* 앱에서 프레임워크 네임스페이스를 사용합니다.
* 앱에 필요한 자격을 추가합니다. 필요한 자격 및 추가 방법에 대한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.
* 앱의  **iOS 번들 서명**에서  **사용자 지정 자격**이 **Entitlements.plist**로 설정되어 있는지 확인합니다. 이 설정은 디버그 및 iOS 시뮬레이터 빌드에 대한 기본 설정이  _아닙니다_ .

앱 서비스에 문제가 발생하면 주 가이드의 [문제 해결](~/ios/deploy-test/provisioning/capabilities/index.md) 섹션을 참조하세요.