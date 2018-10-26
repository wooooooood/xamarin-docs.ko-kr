---
title: Firebase Cloud Messaging
description: Firebase Cloud Messaging (FCM)는 모바일 앱 및 서버 응용 프로그램 간의 메시징 원활 하 게 서비스. 이 문서에서는 FCM 작동 하는 방법의 개요를 제공 하 고 앱 FCM를 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: bf0523a98c8e68691228b6e305265277e2925fa3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116119"
---
# <a name="firebase-cloud-messaging"></a>Firebase Cloud Messaging

_Firebase Cloud Messaging (FCM)는 모바일 앱 및 서버 응용 프로그램 간의 메시징 원활 하 게 서비스. 이 문서에서는 FCM 작동 하는 방법의 개요를 제공 하 고 앱 FCM를 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Firebase Cloud Messaging 영웅 이미지](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 어떻게 xamarin android 앱 및 앱 서버의 간에 메시지를 라우팅합니다 Firebase Cloud Messaging의 대략적인 개요를 제공 하 고 앱 FCM 서비스를 사용할 수 있도록 자격 증명을 가져오는 단계별 절차를 제공 합니다.

## <a name="overview"></a>개요

Firebase Cloud Messaging (FCM)는 전송, 라우팅 및 서버 응용 프로그램 및 모바일 클라이언트 앱 사이 메시지 큐를 처리 하는 플랫폼 간 서비스. FCM을 메시징 GCM (Google Cloud), 후속 이며 Google Play 서비스를 기반 합니다.

다음 다이어그램에서 볼 수 있듯이, FCM 메시지 발신자와 클라이언트 간의 중개자로 작동 합니다. A *클라이언트 앱* FCM 지원 장치에서 실행 되는 앱입니다. 합니다 *앱 서버* (또는 귀하의 회사에서 제공)는 클라이언트 앱 FCM를 통해 통신 하는 FCM 지원 서버입니다. GCM 달리 FCM 수 있도록 Firebase 콘솔 알림을 GUI를 통해 직접 클라이언트 앱으로 메시지를 보낼 수 있습니다.

[![FCM은 클라이언트 앱과 응용 프로그램 서버 사이 위치](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM을 사용 하 여 앱 서버가 메시지를 보낼 수 있습니다. 단일 장치, 장치, 그룹 또는 항목에 가입 된 장치 수 있습니다. 클라이언트 앱을 FCM을 사용 하 여 다운스트림 메시지 (예: 원격 알림을 받기) 응용 프로그램 서버에서 구독할 수 있습니다. 다양 한 유형의 Firebase 메시지에 대 한 자세한 내용은 참조 하세요. [FCM 메시지에 대 한](https://firebase.google.com/docs/cloud-messaging/concept-options)합니다.

## <a name="fcm-in-action"></a>Firebase Cloud Messaging 작업

앱 서버에서 메시지를 보낼 클라이언트 앱에 응용 프로그램 서버에서 다운스트림 메시지를 보낼 때를 *FCM 연결 서버* Google; 제공한 FCM 연결 서버를 실행 하는 장치에 메시지를 전달 합니다 클라이언트 앱입니다. HTTP를 통해 메시지를 보낼 수 있습니다 또는 [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging 및 현재 상태 프로토콜). 클라이언트 앱은 항상 연결 되어 있지 않으므로 또는 실행 하 고 FCM 연결 서버 큐에 추가 하 고 저장소 메시지를 다시 연결 하 고 사용할 클라이언트 앱으로 전송 합니다. 마찬가지로, FCM가 큐에 넣기 업스트림 메시지를 클라이언트 앱에서 앱 서버에 앱 서버를 사용할 수 없는 경우. FCM 연결 서버에 대 한 자세한 내용은 참조 하세요 [에 대 한 Firebase 클라우드 메시징 서버](https://firebase.google.com/docs/cloud-messaging/server)합니다.

FCM 앱 서버 및 클라이언트 앱을 식별 하려면 다음 자격 증명을 사용 하 고 이러한 자격 증명을 사용 하 여 FCM 통해 메시지 트랜잭션의 권한을 부여 하려면:

-   <a name="fcm-in-action-sender-id"></a>**보낸 사람 ID** &ndash; 는 *보낸 사람 ID* 는 Firebase 프로젝트를 만들 때 할당 되는 고유한 숫자 값입니다. 보낸 사람 ID가 클라이언트 앱에 메시지를 보낼 수 있는 각 앱 서버를 식별 하려면 사용 됩니다. 보낸 사람 ID가도 프로젝트 번호로; 프로젝트를 등록할 때 Firebase 콘솔에서 보낸 사람 ID를 가져옵니다. 보낸 사람 ID의 예로 `496915549731`합니다.

-   <a name="fcm-in-action-api-key"></a>**API 키** &ndash; 는 *API 키* Firebase 서비스; 앱 서버 액세스를 제공 합니다. 이 키를 사용 하 여 앱 서버를 인증 하는 FCM 합니다. 이 자격 증명은 라고도 합니다 *서버 키* 또는 *웹 API 키*합니다. API 키의 예는 `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`합니다.

-   <a name="fcm-in-action-app-id"></a>**앱 ID** &ndash; FCM에서 메시지를 받도록 등록 하는 클라이언트 앱 (지정된 된 장치는 무관)의 id입니다. 앱 ID의 예로 `1:415712510732:android:0e1eb7a661af2460`합니다.

-   <a name="fcm-in-action-registration-token"></a>**등록 토큰** &ndash; 는 *등록 토큰* (라고도 합니다 *인스턴스 ID*) FCM id 제공된 된 장치에 클라이언트 앱입니다. 등록 토큰은 런타임에 생성 &ndash; 처음 등록할 때 FCM을 사용 하 여 장치에서 실행 하는 동안 응용 프로그램 등록 토큰을 수신 합니다. 등록 토큰을 FCM에서 메시지를 받는 클라이언트 앱 (특정 장치에서 실행)의 인스턴스를 인증 합니다.
    등록 토큰의 예는 `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (매우 긴 문자열)입니다.

[설정 구성 Firebase Cloud Messaging](#setup_fcm) (이 가이드 뒷부분) 프로젝트를 만들고 이러한 자격 증명을 생성 하기 위한 자세한 지침을 제공 합니다. 새 프로젝트를 만들 때 합니다 [Firebase 콘솔](https://console.firebase.google.com/)를 자격 증명 파일을 호출 **google-services.json** 만들어집니다 &ndash; 에설명된대로이파일을Xamarin.Android프로젝트에추가[ FCM 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.

다음 섹션에서는 클라이언트 응용 프로그램이 FCM 통해 앱 서버와 통신 하는 경우 이러한 자격 증명은 사용 하는 방법을 설명 합니다.


<a name="registration" />

### <a name="registration-with-fcm"></a>FCM 사용 하 여 등록

메시징 수행 되기 전 FCM을 사용 하 여 클라이언트 앱을 먼저 등록 해야 합니다. 클라이언트 앱은 다음 다이어그램에 표시 되는 등록 단계를 완료 해야 합니다.

[![앱 등록 단계 다이어그램](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1.  클라이언트 앱 FCM에 보낸 사람 ID, API 키 및 앱 ID를 전달 하는 등록 토큰을 가져오려면 FCM에 연결 합니다.

2.  FCM 등록 토큰을 클라이언트 앱 돌아갑니다.

3.  클라이언트 앱에는 (선택 사항) 앱 서버에 등록 토큰을 전달합니다.

앱 서버는 클라이언트 앱을 사용 하 여 후속 통신에 대 한 등록 토큰을 캐시 합니다. 앱 서버 등록 토큰을 받았음을 나타내기 위해 클라이언트 앱에 다시 승인을 보낼 수 있습니다. 이 핸드셰이크에 발생 한 후 클라이언트 앱 수에서 메시지를 수신 (또는 메시지를 보낼) 앱 서버. 이전 토큰이 손상 된 경우 클라이언트 앱은 새 등록 토큰을 받을 수 있습니다 (참조 [FCM 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) 앱 등록 토큰 업데이트를 수신 하는 방법의 예).

클라이언트 앱은 더 이상 앱 서버에서 메시지를 수신 하려고, 등록 토큰을 삭제 하려면 앱 서버에 요청을 보낼 수 없습니다. 클라이언트 앱이 장치에서 제거 된 경우 FCM이를 감지 하 고 자동으로 등록 토큰을 삭제 하려면 응용 프로그램 서버에 알립니다.



### <a name="downstream-messaging"></a>다운스트림 메시징

다음 다이어그램에서는 Firebase Cloud Messaging 저장 및 다운스트림 메시지를 전달 하는 방법을 보여 줍니다.

[![FCM 다운스트림 메시징에 대 한 저장 및 전달 사용](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

앱 서버는 클라이언트 앱에 다운스트림 메시지를 보내면, 위의 다이어그램에 열거형으로 다음 단계를 사용 합니다.

1.  앱 서버는 FCM에 메시지를 보냅니다.

2.  클라이언트 장치를 사용할 수 없는 경우 FCM 서버 나중에 큐에 메시지를 저장 합니다. 메시지를 최대 4 주 동안 FCM 저장소에 유지 됩니다 (자세한 내용은 [메시지의 수명을 설정](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)).

3.  클라이언트 장치를 사용할 수 있는 경우 FCM 해당 장치에 클라이언트 앱에 메시지를 전달 합니다.

4.  클라이언트 앱 FCM에서 메시지를 받고 처리 하며 사용자에 게 표시 합니다. 예를 들어 메시지 원격 알림을 경우 알림 영역에서 사용자에 게 표시 됩니다.

(앱 서버 보내는 메시지가 단일 클라이언트 앱)이 메시징 시나리오에서는 메시지가 최대 4kb 길이가 될 수 있습니다.

Android에서 다운스트림 FCM 메시지를 수신 하는 방법에 대 한 자세한 내용은 [FCM 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.

### <a name="topic-messaging"></a>항목에서는 메시징

*항목에서는 메시징* 응용 프로그램 서버는 특정 항목에 옵트인 하는 여러 장치로 메시지를 보낼 수 있도록 합니다. 작성 하 고 Firebase 콘솔 알림을 GUI를 통해 토픽 메시지를 보낼 수도 있습니다. FCM에 구독 클라이언트로 항목 메시지를 배달 하 고 라우팅을 처리합니다. 이 기능은 날씨, 주식 시세 알림과 헤드라인 뉴스와 같은 메시지에 대해 사용할 수 있습니다.

[![메시징 항목 다이어그램](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

(클라이언트 앱에는 앞에서 설명한 대로 등록 토큰을 가져옵니다) 한 후 다음 단계를 항목 메시징에 사용 됩니다.

1.  클라이언트 앱 FCM에 구독 메시지를 전송 하 여 토픽을 구독 합니다.

2.  앱 서버 배포에 대 한 FCM에 토픽 메시지를 보냅니다.

3.  FCM 해당 항목에 구독 클라이언트로 항목 메시지를 전달 합니다.

Firebase 항목 메시징에 대 한 자세한 내용은 참조 Google [항목에서는 Android에서 메시징](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)합니다.


<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase Cloud Messaging 설정

앱에서 FCM 서비스를 사용 하려면 먼저 새 프로젝트를 만듭니다 (가져오거나 해야 기존 프로젝트를) 통해 합니다 [Firebase 콘솔](https://console.firebase.google.com/)합니다. 앱에 대 한 Firebase Cloud Messaging 프로젝트를 만들려면 다음 단계를 사용 합니다.

1.  에 로그인 합니다 [Firebase 콘솔](https://console.firebase.google.com/) Google 계정 (즉, Gmail 주소) 및 클릭 **새 프로젝트 만들기**:

    [![새 프로젝트 만들기 단추](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    기존 프로젝트에 있는 경우 클릭 **Google 프로젝트 가져오기**합니다.

2.  에 **프로젝트를 만듭니다** 대화 상자에서 프로젝트의 이름을 입력 하 고 클릭 **프로젝트 만들기**합니다. 다음 예제에서는 새 프로젝트를 호출 **XamarinFCM** 만들어집니다.

    [![만들기 프로젝트 대화 상자](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3.  Firebase 콘솔에서 **개요**, 클릭 **Android 앱에 Firebase 추가**:

    [![Android 앱에 Firebase 추가](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4.  다음 화면에서 앱의 패키지 이름을 입력 합니다. 이 예제에서는 패키지 이름은 **com.xamarin.fcmexample**합니다. 이 값에는 Android 앱의 패키지 이름과 일치 해야 합니다. 에 앱 애칭을 입력할 수도 있습니다는 **앱 애칭** 필드:

    [![앱 애칭으로 FCM 예제를 입력합니다.](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5.  앱 동적 링크나 초대를 Google 인증을 사용 하는 경우 디버그 서명 인증서도 입력 해야 합니다. 서명 인증서를 찾는 방법에 대 한 자세한 내용은 참조 하세요. [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)합니다.
    이 예제에서는 서명 인증서를 비워 둡니다.

6.  클릭 **앱 추가**:

    [![앱 추가 단추를 클릭합니다.](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    서버 API 키와 클라이언트 ID를 앱에 대 한 자동으로 생성 됩니다. 이 정보는 패키지를 **google-services.json** 을 클릭 하면 자동으로 다운로드 되는 파일 **앱 추가**합니다.
    이 파일을 안전한 위치에 저장 해야 합니다.

추가 하는 방법의 자세한 예제 **google-services.json** FCM Android에 푸시 알림 메시지를 받으려면 앱 프로젝트를 참조 하세요 [FCM 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)합니다.



## <a name="for-further-reading"></a>자세한 내용을 보려면

-   Google [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) Firebase Cloud Messaging의 주요 기능 개요, 작동 방법 및 설치 지침을 제공 합니다.

-   Google [앱 서버 보내기 요청 작성](https://firebase.google.com/docs/cloud-messaging/send-message) 앱 서버를 사용 하 여 메시지를 보내는 방법에 설명 합니다.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) 하 고 [RFC 6121](https://tools.ietf.org/html/rfc6121) 설명 하 고 확장할 수 있는 메시징 및 현재 상태 프로토콜 (XMPP)를 정의 합니다.

-   [FCM 메시지 정보](https://firebase.google.com/docs/cloud-messaging/concept-options) 다양 한 유형의 Firebase Cloud Messaging으로 보낼 수 있는 메시지에 설명 합니다.

## <a name="summary"></a>요약

이 문서에서는 Firebase 클라우드 메시징 (FCM)의 개요를 제공 합니다. 식별 하 고 앱 서버 및 클라이언트 응용 프로그램 간의 메시징 권한을 부여 하는 데 사용 되는 다양 한 자격 증명을 설명 했습니다. 등록 및 다운스트림 메시징 시나리오를 설명 하는 것 하 고 FCM 서비스를 사용 하는 FCM을 사용 하 여 앱을 등록 하는 단계를 설명 합니다.


## <a name="related-links"></a>관련 링크

- [Firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/)
