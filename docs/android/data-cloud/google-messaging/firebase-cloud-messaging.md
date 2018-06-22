---
title: Firebase 클라우드 메시징
description: Firebase 클라우드 메시징 (FCM)는 모바일 응용 프로그램 및 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 FCM 작동 하는 방법에 대 한 개요를 제공 하 고 앱 FCM를 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: ef2c23d16545d03dc267054a96f8b0f8883afcf1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769821"
---
# <a name="firebase-cloud-messaging"></a>Firebase 클라우드 메시징

_Firebase 클라우드 메시징 (FCM)는 모바일 응용 프로그램 및 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 FCM 작동 하는 방법에 대 한 개요를 제공 하 고 앱 FCM를 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Firebase Cloud Messaging hero 이미지](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 방법을 Xamarin.Android 응용 프로그램 및 응용 프로그램 서버 사이 메시지를 라우팅하 Firebase Cloud Messaging에 대 한 높은 수준의 개요를 제공 하 고 앱 FCM 서비스를 사용할 수 있도록 자격 증명을 인식 하기 위한 단계별 절차를 제공 합니다.



## <a name="overview"></a>개요

Firebase 클라우드 메시징 (FCM)는 전송, 라우팅 및 서버 응용 프로그램 및 모바일 클라이언트 응용 프로그램 사이 메시지 큐를 처리 하는 플랫폼 간 서비스. FCM에 메시징 GCM (Google Cloud), 후속 이며 Google Play 서비스를 기반으로 합니다.

다음 다이어그램에 볼 수 있듯이, FCM 메시지 발신자와 클라이언트 간의 중개자 역할을 합니다. A *클라이언트 응용 프로그램* 장치에서 실행 되는 FCM 사용이 가능한 앱입니다. *응용 프로그램 서버* (또는 귀하의 회사에서 제공)는 FCM 사용이 가능한 서버 FCM 통해와 통신 하는 클라이언트 응용 프로그램입니다. GCM, 달리 FCM 사용 하면 직접 Firebase 콘솔 알림 GUI를 통해 클라이언트 응용 프로그램으로 메시지를 보낼 수 있습니다.

[![클라이언트 응용 프로그램 및 응용 프로그램 서버 사이 작용 FCM](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM를 사용 하 여 응용 프로그램 서버 메시지를 보낼 수는 단일 장치, 장치, 그룹 또는 항목에 등록 된 장치의 수입니다. 클라이언트 응용 프로그램 (예: 원격 알림의 받을) 응용 프로그램 서버에서 다운스트림 메시지에 등록 하도록 FCM를 사용할 수 있습니다. 다른 유형의 Firebase 메시지에 대 한 자세한 내용은 참조 [FCM 메시지에 대 한](https://firebase.google.com/docs/cloud-messaging/concept-options)합니다.



## <a name="firebase-cloud-messaging-in-action"></a>메시징 작업에 firebase 클라우드

클라이언트 응용 프로그램에 응용 프로그램 서버에서 다운스트림 메시지를 보낼 응용 프로그램 서버 메시지를 보내는 *FCM 연결 서버* ; Google에서 제공 FCM 연결 서버에는 차례로 실행 하는 장치에는 메시지 전달의 클라이언트 응용 프로그램입니다. HTTP를 통해 메시지를 보낼 수 있습니다 또는 [XMPP](https://developers.google.com/cloud-messaging/ccs) (확장할 수 있는 메시징 및 현재 상태 프로토콜). 클라이언트 응용 프로그램은 항상 연결 되어 있지 않으므로 실행 되었거나 실행 FCM 연결 서버 큐에 넣습니다 및 저장소 메시지를 다시 연결 하 고 사용할 수 있을 때 클라이언트 응용 프로그램으로 전송 합니다. 마찬가지로, FCM가 큐에 넣지 업스트림 메시지를 클라이언트 응용 프로그램에서 응용 프로그램 서버에 응용 프로그램 서버를 사용할 수 없는 경우. FCM 연결 서버에 대 한 자세한 내용은 [에 대 한 Firebase 클라우드 메시징 서버](https://firebase.google.com/docs/cloud-messaging/server)합니다.

FCM 다음 자격 증명을 사용 하 여 응용 프로그램 서버 및 클라이언트 응용 프로그램을 식별 하 고 이러한 자격 증명을 사용 하 여 FCM 통해 메시지 트랜잭션의 권한을 부여 하려면:

-   **보낸 사람 ID** &ndash; 는 *보낸 사람 ID* Firebase 프로젝트를 만들 때 할당 되는 고유 숫자 값입니다. 클라이언트 응용 프로그램에 메시지를 보낼 수 있는 각 응용 프로그램 서버를 식별 하는 보낸 사람 ID 사용 됩니다. 보낸 사람 ID 이기도 프로젝트 번호입니다. 프로젝트를 등록할 때 Firebase 콘솔에서 보낸 사람 ID를 가져옵니다. 보낸 사람 ID의 예로 `496915549731`합니다.

-   **API 키** &ndash; 는 *API 키* Firebase 서비스;에 응용 프로그램 서버 액세스를 제공 합니다. 이 키를 사용 하 여 응용 프로그램 서버를 인증 하는 FCM 합니다. 이 자격 증명도 라고는 *서버 키* 또는 *웹 API 키*합니다. API 키의 예로 `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`합니다.

-   **앱 ID** &ndash; 응용 프로그램의 클라이언트 (지정된 된 장치 독립적) FCM에서 메시지를 받도록 등록 하는 id입니다. 앱 ID의 예로 `1:415712510732:android:0e1eb7a661af2460`합니다.

-   **등록 토큰** &ndash; 는 *등록 토큰* (라고도 하는 *인스턴스 ID*) 제공된 된 장치에 클라이언트 앱의 FCM id입니다. 런타임 시 등록 토큰이 생성 될 &ndash; 처음 등록할 때 FCM와 장치에서 실행 하는 동안 응용 프로그램 등록 토큰을 수신 합니다. 등록 토큰 FCM에서 메시지를 받을 (해당 특정 장치에서 실행) 하는 클라이언트 응용 프로그램의 인스턴스 권한도 부여 됩니다.
    등록 토큰의 예는 `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (매우 긴 문자열)입니다.

[설정은를 Firebase Cloud Messaging](#setup_fcm) (이 가이드의 뒷부분에) 프로젝트를 만들고 이러한 자격 증명을 생성 하기 위한 자세한 지침을 제공 합니다. 새 프로젝트를 만들 때의 [Firebase 콘솔](https://console.firebase.google.com/), 자격 증명 파일을 호출 **google services.json** 만들어집니다 &ndash; 에설명된대로Xamarin.Android프로젝트에이파일에추가[ FCM 사용 하 여 원격 알림을](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.

다음 섹션에서는 클라이언트 응용 프로그램이 FCM 통해 응용 프로그램 서버와 통신 하는 경우 이러한 자격 증명은 사용 하는 방법을 설명 합니다.


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM에 등록

클라이언트 응용 프로그램 메시징 수행 될 수 전에 FCM을 등록 해야 합니다. 클라이언트 앱 등록 단계는 다음 다이어그램에 표시 된 것을 완료 해야 합니다.

[![앱 등록 단계 다이어그램](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  클라이언트 응용 프로그램 보낸 사람 ID, API 키 및 앱 ID FCM를 전달 하면, 등록 토큰을 가져오는 데 FCM에 연결 합니다.

2.  FCM은 클라이언트 응용 프로그램 등록 토큰을 반환합니다.

3.  (선택 사항) 클라이언트 응용 프로그램의 앱 서버로 등록 토큰을 전달합니다.

응용 프로그램 서버는 후속 통신할 클라이언트 응용 프로그램 등록 토큰을 캐시 합니다. 응용 프로그램 서버 등록 토큰 받았음을 나타내기 위해 클라이언트 응용 프로그램에 다시 승인을 보낼 수 있습니다. 이 핸드셰이크 발생 한 후 클라이언트 응용 프로그램 수에서 메시지를 수신 (또는 메시지를 보낼) 응용 프로그램 서버. 클라이언트 응용 프로그램 이전 토큰 손상 되는 경우 새 등록 토큰을 받을 수 있습니다 (참조 [FCM 사용 하 여 원격 알림을](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) 응용 프로그램 등록 토큰 업데이트를 수신 하는 방법에 대 한 예제).

클라이언트 응용 프로그램이 더 이상 응용 프로그램 서버에서 메시지를 수신 하려면, 등록 토큰을 삭제 하려면 응용 프로그램 서버에 요청을 보낼 수 없습니다. 클라이언트 앱이 장치에서 제거 된 경우 FCM이를 감지 하 고 자동으로 등록 토큰을 삭제 하려면 응용 프로그램 서버에 알립니다.



### <a name="downstream-messaging"></a>다운스트림 메시징

다음 다이어그램에서는 Firebase 클라우드 메시징 저장 및 다운스트림 메시지 전달 방법을 보여 줍니다.

[![FCM 다운스트림 메시징에 대 한 저장 및 전달 사용](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

응용 프로그램 서버에 클라이언트 앱에는 다운스트림 메시지를 보내는 다음 단계는 위의 다이어그램에 열거형으로 사용 합니다.

1.  응용 프로그램 서버 FCM에 메시지를 보냅니다.

2.  클라이언트 장치에 사용할 수 없는 경우 FCM 서버 이후 전송을 위해 큐에 메시지를 저장 합니다. 메시지가 최대 4 주 동안 FCM 저장소에 보관 됩니다 (자세한 내용은 참조 [메시지의 수명이 설정](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  클라이언트 장치가 사용할 수 있을 때 FCM 해당 장치에 클라이언트 앱에 메시지를 전달 합니다.

4.  클라이언트 응용 프로그램 FCM에서 메시지를 받아 처리 한 사용자에 게 표시 합니다. 예를 들어 원격 알림 메시지가 사용 하는 경우 알림 영역에서 사용자에 게 표시 됩니다.

(여기서 응용 프로그램 서버는 메시지를 보냅니다 단일 클라이언트 응용 프로그램)이 메시징 시나리오에서는 메시지가 최대 4kb 길이가 될 수 있습니다.

Android에서 다운스트림 FCM 메시지를 수신 하는 방법에 대 한 자세한 내용은 참조 [FCM 사용 하 여 원격 알림을](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.

### <a name="topic-messaging"></a>항목 메시징

*항목 메시징* 메시지를 보낼 특정 항목에 옵트인 한 여러 장치에 응용 프로그램 서버에 대 한 가능 하 게 합니다. 작성 하 고 Firebase 콘솔 알림 GUI를 통해 항목 메시지를 보낼 수도 있습니다. FCM 라우팅 및 가입 된 클라이언트에 대 한 배달 항목 메시지를 처리합니다. 이 기능은 날씨, 주식 시세 알림과 뉴스 헤드라인 같은 메시지에 대 한 사용할 수 있습니다.

[![항목 메시징 다이어그램](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

다음 단계 (클라이언트 응용 프로그램 등록 토큰 앞에서 설명한 대로 가져옵니다) 한 후 메시징 항목에 사용 됩니다.

1.  클라이언트 응용 프로그램 FCM를 구독 메시지를 전송 하 여 주제를 구독 합니다.

2.  응용 프로그램 서버 배포에 대 한 항목 메시지 FCM를 보냅니다.

3.  FCM 해당 항목을 구독 하는 클라이언트에 항목 메시지를 전달 합니다.

Firebase 항목 메시징에 대 한 자세한 내용은 참조 Google의 [항목 Android에서 메시징](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)합니다.


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase 설정 클라우드 메시징

응용 프로그램에서 FCM 서비스를 사용 하려면 먼저 새 프로젝트를 만듭니다 (가져오거나 해야 기존 프로젝트)를 통해는 [Firebase 콘솔](https://console.firebase.google.com/)합니다. 앱에 대 한 Firebase Cloud Messaging 프로젝트를 만들려면 다음 단계를 사용 합니다.

1.  에 로그인 된 [Firebase 콘솔](https://console.firebase.google.com/) Google 계정 (예: Gmail 주소)와 클릭 **새 프로젝트 만들기**:

    [![새 프로젝트 단추 만들기](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    기존 프로젝트를 사용 하도록 설정한 경우 클릭 **Google 프로젝트 가져오기**합니다.

2.  에 **프로젝트를 만들** 대화 상자에서 프로젝트의 이름을 입력 하 고 클릭 **프로젝트 만들기**합니다. 다음 예제에서는 새 프로젝트 호출 **XamarinFCM** 만들어집니다.

    [![프로젝트 대화 상자 만들기](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase 콘솔에서 **개요**, 클릭 **Android 앱에 추가 Firebase**:

    [![Firebase Android 앱에 추가](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  다음 화면에서 응용 프로그램의 패키지 이름을 입력 합니다. 이 예제에서는 패키지 이름이 **com.xamarin.fcmexample**합니다. 이 값에는 Android 앱의 패키지 이름을 일치 해야 합니다. 응용 프로그램 애칭에 입력할 수도 있습니다는 **앱 애칭** 필드:

    [![앱 애칭으로 FCM 예제를 입력합니다.](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  응용 프로그램에서 동적 링크, 초대를 또는 Google 인증을 사용 하는 경우 인증서를 서명 하 여 디버그를 입력 해야 합니다. 서명 인증서를 찾는 방법에 대 한 자세한 내용은 참조 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)합니다.
    이 예제에서는 서명 인증서 비어 있습니다.

6.  클릭 **추가 앱**:

    [![앱 추가 단추를 클릭 하면](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    서버 API 키 및 클라이언트 ID는 앱에 대 한 자동으로 생성 됩니다. 이 정보에 패키지 되는 **google services.json** 를 클릭할 때 자동으로 다운로드 된 파일 **앱 추가**합니다.
    이 파일을 안전한 위치에 저장 해야 합니다.

추가 하는 방법의 자세한 예제 **google services.json** Android에서 FCM 푸시 알림 메시지를 사용 하는 응용 프로그램 프로젝트에서 참조 [FCM 사용 하 여 원격 알림을](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.



## <a name="for-further-reading"></a>추가 정보

-   Google의 [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging의 주요 기능에 대 한 개요, 작동 방법 및 설치 지침을 제공 합니다.

-   Google의 [응용 프로그램 서버 보내기 요청 빌드](https://firebase.google.com/docs/cloud-messaging/send-message) 응용 프로그램 서버와 함께 메시지를 보내는 방법에 설명 합니다.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) 및 [RFC 6121](https://tools.ietf.org/html/rfc6121) 설명 하 고 확장할 수 있는 메시징 및 현재 상태 프로토콜 (XMPP)를 정의 합니다.



## <a name="summary"></a>요약

이 문서는 Firebase 클라우드 메시징 (FCM)의 개요를 제공 합니다. 그를 식별 하 고 응용 프로그램 서버 및 클라이언트 응용 프로그램 사이의 메시징 권한을 부여 하는 데 사용 되는 다양 한 자격 증명을 설명 합니다. 등록 및 다운스트림 메시징 시나리오를 설명 하 고 FCM 서비스를 사용 하는 FCM 사용 앱을 등록 하기 위한 단계를 설명 합니다.


## <a name="related-links"></a>관련 링크

- [Firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/)
