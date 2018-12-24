---
title: Google 라이선스 서비스
ms.prod: xamarin
ms.assetid: E96BDCC3-454A-A797-5819-905E2BB1AC41
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 12/20/2017
ms.openlocfilehash: eedfcfe2ed274ddf541addec67e66250deab7899
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114628"
---
# <a name="google-licensing-services"></a>Google 라이선스 서비스

Google Play 이전의 Android 응용 프로그램은 Google Market에서 제공하는 레거시 복사 방지를 사용하여 권한이 있는 사용자만 디바이스에서 응용 프로그램을 실행할 수 있게 했습니다. 복사 방지 메커니즘에는 제한이 있어 이상적인 응용 프로그램 보호 솔루션이 되지 못했습니다.

Google 라이선스는 레거시 복사 방지 메커니즘을 대체합니다.
Google 라이선스는 유연하고 안전한 네트워크 기반 서비스로, Android 응용 프로그램이 쿼리를 통해 해당 응용 프로그램이 특정 디바이스에서 실행할 수 있는 라이선스가 있는지 판단합니다.

Google 라이선스는 Android 응용 프로그램이 라이선스 검사 시점, 라이선스 검사 주기, 라이선스 서버 응답 처리 방법에 대해 완전히 제어한다는 점에서 유연합니다.

Google 라이선스는 Google Play 서버와 응용 프로그램 간에 독점적으로 공유되는 RSA 키 쌍을 사용하여 각 응답이 서명되므로 안전합니다. Google Play는 개발자를 위한 공개 키를 제공하며 이 키는 Android 응용 프로그램에 포함되어 응답을 인증하는 데 사용됩니다. Google Play 서버는 내부적으로 개인 키를 유지합니다.

Google 라이선스를 구현하는 응용 프로그램은 디바이스에서 Google Play 응용 프로그램이 호스트하는 서비스에 대한 요청을 수행합니다. 그러면 Google Play가 Google 라이선스 서버에 이 요청을 보내고 해당 서버가 라이선스 상태를 응답합니다. 

[![라이선스 서버 워크플로 다이어그램](google-licensing-services-images/gp-licensing-service-overview.png)](google-licensing-services-images/gp-licensing-service-overview.png#lightbox)

위의 다이어그램은 이 워크플로 보여줍니다. 

-   응용 프로그램은 패키지 이름 *nonce*(암호화 인증자)를 제공합니다. 이 이름은 서버 응답과, 응답을 비동기로 처리할 수 있는 콜백의 유효성 검사에 사용됩니다.  

-   Google Play는 Google 계정과 같은 정보와 IMSI 번호 같은 디바이스 자체 정보를 제공합니다. 

Google 라이선스 서비스도 APK 확장 파일의 핵심 구성 요소입니다(이 문서의 후반부에서 논의). APK 확장 파일은 Google 라이선스 서비스를 사용하여 다운로드되는 확장 파일의 URL을 가져옵니다.


## <a name="requirements"></a>요구 사항

Google Play를 통해 구입하지 않은 응용 프로그램은 Google 라이선스 서비스의 혜택을 받지 못합니다. Google Play가 디바이스에 설치되지 않은 경우에도 라이선스 서비스를 사용하는 응용 프로그램이 해당 디바이스에서 정상 작동합니다.

Google Play가 작동하려면 인터넷 액세스가 필요합니다. 응용 프로그램은 디바이스가 Google Play 라이선스 서버에 액세스할 수 없는 경우를 대비하여 라이선스를 캐시할 수 있습니다.

무료 응용 프로그램은 응용 프로그램이 APK 확장 파일을 사용할 때만 Google 라이선스가 필요합니다.
