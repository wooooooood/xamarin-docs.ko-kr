---
title: Xamarin.iOS에서 Apple Pay 기능
description: 애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 Apple Pay 기능에 필요한 설정을 설명합니다.
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 856ed31cd2ee8fdc657cd1437f21052e1d7a79f2
ms.sourcegitcommit: 3d21bb1a6d9b78b65aa49917b545c39d44aa3e3c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70065493"
---
# <a name="apple-pay-capabilities-in-xamarinios"></a>Xamarin.iOS에서 Apple Pay 기능

_애플리케이션에 기능을 추가하려면 흔히 추가 프로비전 설정이 필요합니다. 이 가이드에서는 Apple Pay 기능에 필요한 설정을 설명합니다._

Apple Pay를 사용하면 사용자의 iOS 디바이스를 통해 실제 상품을 구입할 수 있습니다. 이 섹션에서는 Apple Developer Center에서 Apple Pay에 필요한 모든 구성 요소를 생성하는 방법을 설명합니다.

개발자 센터를 통해 새 앱을 프로비전할 때 수행해야 하는 단계는 세 가지입니다.

1. 가맹점 ID를 만듭니다.
2. Apply Pay 기능으로 Apply ID를 만들어서 가맹점을 추가합니다.
3. 가맹점 ID에 대한 인증서를 생성합니다.

아래 단계는 위의 항목을 만드는 과정을 안내합니다.

<a name="merchantid" />

## <a name="create-merchant-id"></a>가맹점 ID 만들기

가맹점 ID는 지불을 수락할 수 있음을 Apple Pay에 알리는 데 사용되며 PassKit의 `PaymentRequest` 메서드로 전달되어 Apple Pay 자격에 사용됩니다.

1. [Apple Developer Center](https://developer.apple.com/account/)로 이동하여 Certificates, Identifier, and Profiles(인증서, 식별자 및 프로필) 섹션으로 이동합니다. 
 
    ![개발자 센터에서 가맹점 ID 선택](apple-pay-capabilities-images/image57.png)

2. **식별자** 아래에서 **가맹점 ID**를 선택한 다음, **+** 를 선택하여 새 가맹점 ID를 만듭니다.  

3. 아래 설명된 양식에 새로운 설명과 식별자를 입력합니다. 설명을 통해 ID를 식별할 수 있고 나중에 변경할 수 있습니다. 식별자는 고유해야 하며  `merchant` 문자열로 시작해야 합니다. 식별자는 다음과 같은 형식을 사용하는 것이 좋습니다. `merchant.com.[Your-App-Name]`
   
    ![새 가맹점 ID 세부 정보](apple-pay-capabilities-images/image58.png)

4. 세부 정보를 확인하고 ID를  **등록** 합니다. 
    
    ![가맹점 ID 확인](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>가맹점 ID가 포함된 Apple Pay 기능으로 앱 ID 만들기

1. [개발자 센터](https://developer.apple.com/account/)에서 **식별자** 아래 **앱 ID**를 클릭합니다. 
    
    ![개발자 센터에서 앱 ID 선택](apple-pay-capabilities-images/image6.png)

2. **+** 단추를 선택하여 새 앱 ID를 추가합니다. 
   
    ![새 앱 ID 추가 단추](apple-pay-capabilities-images/image27.png)

3. 앱 ID의 이름을 입력하고 Explicit App ID(명시적 앱 ID)를 지정합니다.    
   
    ![앱 ID 세부 정보 화면](apple-pay-capabilities-images/image35.png)

4. App Services 아래에서 Apple Pay를 선택합니다.    
  
    ![App Services Apple Pay](apple-pay-capabilities-images/image36.png)

5. **계속**과 **등록**을 차례로 선택합니다. 확인 화면에 Apple Pay가 노란색 기호로 선택된 구성 가능과 함께 표시됩니다. 
   
    ![Apple Pay에 대한 확인 화면](apple-pay-capabilities-images/image37.png)

6. 앱 ID 목록으로 돌아가서 방금 만든 항목을 선택합니다.  
   
    ![앱 ID 편집](apple-pay-capabilities-images/image38.png)

7. 펼쳐진 섹션의 맨 아래로 스크롤하여 **편집**을 클릭합니다.
8. 목록을 Apple Pay까지 아래로 스크롤하여 **편집** 단추를 클릭합니다.  
    
    ![Apple Pay 앱 ID 세부 정보 편집](apple-pay-capabilities-images/image39.png)

9. 이 앱 ID와 함께 사용할 가맹점 ID를 선택하고 **계속**을 클릭합니다.  
    
    ![앱 ID에 사용할 가맹점 ID 선택](apple-pay-capabilities-images/image40.png)

10. 가맹점 ID 할당을 확인하고 **할당**을 누릅니다.  
    
    ![확인 화면](apple-pay-capabilities-images/image41.png)

[기능 사용](~/ios/deploy-test/provisioning/capabilities/index.md) 가이드의 설명에 따라, 이 앱 ID를 사용하여 새 프로비전 프로필을 생성하거나 다시 생성할 수 있습니다. 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>가맹점 ID에 대한 인증서 만들기

인증서는 Apple에서 트랜잭션과 관련된 중요한 데이터를 암호화하는 데 필요합니다. 각각의 가맹점 ID에는 고유의 인증서가 있어야 합니다. 

인증서를 만들려면 다음 단계를 수행합니다.

1. 위에서 만든 가맹점 ID를 선택하고 **편집**을 누릅니다. 
    
    ![가맹점 ID 편집 대화 상자](apple-pay-capabilities-images/image42.png)

2. iOS 가맹점 ID 설정 화면에서 **인증서 만들기**를 클릭합니다. 
   
    ![결제 처리 인증서 만들기](apple-pay-capabilities-images/image43.png)

3. 다음 질문에 답변합니다. 

    ![결제가 중국에서 단독으로 처리되는 경우 알리기](apple-pay-capabilities-images/image44.png)

4. 이 지점에서 _인증서 서명 요청_을 생성하라는 메시지가 표시됩니다. 

    ![인증서 서명 요청 만들기](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
    > JudoPay 또는 Stripe와 같은 Apple Pay에 제공되는 결제 서비스 업체를 사용하는 경우, 해당 업체가 이 시점에서 사용할 수 있는 올바른 형식의 CSR을 제공할 수 있습니다. 요청에 대한 자세한 내용은 [JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) 및 [Stripe](https://stripe.com/docs/apple-pay/apps#csr) 사이트를 참조하세요. 자체 CSR을 만들려면 아래 5-8단계를 수행합니다. CSR이 있으면 9단계로 이동합니다.

5. Keychain Access(키 집합 액세스) 애플리케이션을 열고 **Keychain Access(키 집합 액세스) &gt; Certificate Assistant(인증서 도우미) &gt; Request a Certificate from a Certificate Authority(인증 기관의 인증서 요청)** 으로 이동합니다. 

     ![Mac에서 키 집합을 사용하여 CSR 만들기](apple-pay-capabilities-images/image46.png)

6. 이메일 주소를 입력하고, 프라이빗 키의 이름을 입력하고, CA Email Address(CA 이메일 주소)를 비워두고, **디스크에 저장** 옵션과 **Let me specify key pair information**(키 쌍 정보 직접 지정)을 차례로 선택합니다.

     ![인증서 정보 대화 상자](apple-pay-capabilities-images/image47.png)

7. CSR을 편리한 위치에 저장합니다. 

     ![CSR을 로컬 컴퓨터에 저장](apple-pay-capabilities-images/image48.png)

8. Key Pair information(키 쌍 정보) 화면에서 **키 크기**를 **256비트**로 설정하고 **알고리즘**을 **ECC**로 설정한 다음, **계속**을 클릭합니다.

     ![키 쌍 정보 입력 대화 상자](apple-pay-capabilities-images/image49.png)

9. 개발자 센터에서 **계속**을 클릭하여 CSR을 업로드합니다. 

     ![개발자 센터에 CSR 업로드 준비](apple-pay-capabilities-images/image50.png)

10. **파일 선택... 클릭하여** CSR을 선택하고 **계속**을 눌러서 개발자 포털에 업로드합니다. 

     ![개발자 센터에 CSR 업로드](apple-pay-capabilities-images/image51.png)

11. 인증서가 생성되면 다운로드하고 두 번 클릭하여 키 집합에 설치합니다.

Apple Pay 사용에 대한 자세한 내용은 다음 가이드를 참조하세요.

* [Apple Pay 소개](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>다음 단계
 
아래 목록에는 필요할 수도 있는 추가 단계가 설명되어 있습니다.

* 앱에서 프레임워크 네임스페이스를 사용합니다.
* 앱에 필요한 자격을 추가합니다. 필요한 자격 및 추가 방법에 대한 자세한 내용은 [자격 사용](~/ios/deploy-test/provisioning/entitlements.md) 가이드를 참조하세요.
* 앱의  **iOS 번들 서명**에서  **사용자 지정 자격**이 **Entitlements.plist**로 설정되어 있는지 확인합니다. 이 설정은 디버그 및 iOS 시뮬레이터 빌드에 대한 기본 설정이  _아닙니다_ .

앱 서비스에 문제가 발생하면 주 가이드의 [문제 해결](~/ios/deploy-test/provisioning/capabilities/index.md) 섹션을 참조하세요.
