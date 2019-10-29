---
title: Firebase 클라우드 메시징
description: FCM (Firebase Cloud Messaging)는 모바일 앱과 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 FCM가 작동 하는 방식에 대 한 개요를 제공 하 고, 앱이 FCM을 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: E5314D7F-2AAC-40DA-BEBA-27C834F078DD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/31/2018
ms.openlocfilehash: c97c931445122cbaa613b87e3778f4dc9e92f4d0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023662"
---
# <a name="firebase-cloud-messaging"></a>Firebase 클라우드 메시징

_FCM (Firebase Cloud Messaging)는 모바일 앱과 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 FCM가 작동 하는 방식에 대 한 개요를 제공 하 고, 앱이 FCM을 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Firebase 클라우드 메시징 주인공 이미지](firebase-cloud-messaging-images/preview.png)](firebase-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 Firebase 클라우드 메시징에서 Xamarin Android 앱과 앱 서버 간에 메시지를 라우팅하는 방법에 대 한 개략적인 개요를 제공 하 고, 앱에서 FCM 서비스를 사용할 수 있도록 자격 증명을 획득 하는 단계별 절차를 제공 합니다.

## <a name="overview"></a>개요

FCM (Firebase Cloud Messaging)은 서버 응용 프로그램과 모바일 클라이언트 앱 간에 메시지의 전송, 라우팅 및 큐를 처리 하는 플랫폼 간 서비스입니다. FCM는 GCM (Google Cloud Messaging의 후속 작업) 이며 Google Play 서비스를 기반으로 합니다.

다음 다이어그램에 나와 있는 것 처럼 FCM은 메시지 발신자와 클라이언트 간의 중개자 역할을 합니다. *클라이언트 앱* 은 장치에서 실행 되는 FCM 사용 앱입니다. 사용자 또는 회사에서 제공 하는 *앱 서버* 는 클라이언트 앱이 FCM를 통해 통신 하는 FCM 사용 서버입니다. GCM과 달리 FCM를 사용 하면 Firebase Console Notification GUI를 통해 직접 클라이언트 앱에 메시지를 보낼 수 있습니다.

[클라이언트 앱과 앱 서버 간에![FCM](firebase-cloud-messaging-images/01-server-fcm-app-sml.png)](firebase-cloud-messaging-images/01-server-fcm-app.png#lightbox)

FCM를 사용 하 여 앱 서버는 단일 장치, 장치 그룹 또는 토픽에 구독 된 여러 장치에 메시지를 보낼 수 있습니다. 클라이언트 앱은 FCM를 사용 하 여 앱 서버에서 다운스트림 메시지를 구독할 수 있습니다 (예: 원격 알림 수신). 여러 유형의 Firebase 메시지에 대 한 자세한 내용은 [FCM 메시지 정보](https://firebase.google.com/docs/cloud-messaging/concept-options)를 참조 하세요.

## <a name="fcm-in-action"></a>Firebase 클라우드 메시징 작동

다운스트림 메시지가 앱 서버에서 클라이언트 앱으로 전송 되 면 앱 서버는 Google에서 제공 하는 *FCM 연결 서버* 에 메시지를 보냅니다. 그러면 FCM 연결 서버가 클라이언트 앱을 실행 하는 장치에 메시지를 전달 합니다. HTTP 또는 [Xmpp](https://developers.google.com/cloud-messaging/ccs) (확장할 수 있는 메시징 및 현재 상태 프로토콜)를 통해 메시지를 보낼 수 있습니다. 클라이언트 앱은 항상 연결 되어 있거나 실행 되지 않기 때문에 FCM 연결 서버는 메시지를 큐 하 고 저장 하 여 다시 연결 하 여 사용할 수 있게 되 면 클라이언트 앱에 보냅니다. 마찬가지로, FCM는 앱 서버를 사용할 수 없는 경우 클라이언트 앱에서 앱 서버로 업스트림 메시지를 큐에 삽입 합니다. FCM 연결 서버에 대 한 자세한 내용은 [Firebase 클라우드 메시징 서버 정보](https://firebase.google.com/docs/cloud-messaging/server)를 참조 하세요.

FCM는 다음 자격 증명을 사용 하 여 앱 서버 및 클라이언트 앱을 식별 하 고, 이러한 자격 증명을 사용 하 여 FCM를 통해 메시지 트랜잭션에 권한을 부여 합니다.

- <a name="fcm-in-action-sender-id"></a>보낸 **사람 id** &ndash; *발신자 id* 는 Firebase 프로젝트를 만들 때 할당 되는 고유 숫자 값입니다. 보낸 사람 ID는 클라이언트 앱에 메시지를 보낼 수 있는 각 앱 서버를 식별 하는 데 사용 됩니다. 보낸 사람 ID는 프로젝트 번호 이기도 합니다. 프로젝트를 등록할 때 Firebase 콘솔에서 발신자 ID를 가져옵니다. 보낸 사람 ID의 예는 `496915549731`입니다.

- <a name="fcm-in-action-api-key"></a>**Api 키 &ndash; api** 키는 Firebase services에 대 한 앱 서버 액세스 *를 제공 합니다* . FCM는이 키를 사용 하 여 앱 서버를 인증 합니다. 이 자격 증명은 *서버 키* 또는 *Web API 키*라고도 합니다. API 키의 예는 `AJzbSyCTcpfRT1YRqbz-jIwp1h06YdauvewGDzk`합니다.

- <a name="fcm-in-action-app-id"></a>**앱 id** &ndash; FCM에서 메시지를 수신 하도록 등록 하는 클라이언트 앱의 id (지정 된 장치와 무관)입니다. 앱 ID의 예는 `1:415712510732:android:0e1eb7a661af2460`합니다.

- <a name="fcm-in-action-registration-token"></a>**등록 토큰 &ndash; 등록** 토큰 ( *인스턴스 ID*라고도 함 *)은 지정* 된 장치에서 클라이언트 앱의 FCM id입니다. 등록 토큰은 &ndash; 런타임에 생성 됩니다. 앱은 장치에서 실행 되는 동안 FCM에 처음 등록할 때 등록 토큰을 받습니다. 등록 토큰은 특정 장치에서 실행 되는 클라이언트 앱의 인스턴스가 FCM에서 메시지를 받을 수 있도록 권한을 부여 합니다.
    등록 토큰의 예는 `fkBQTHxKKhs:AP91bHuEedxM4xFAUn0z ... JKZS` (매우 긴 문자열)입니다.

[Firebase 클라우드 메시징 설정](#setup_fcm) (이 가이드의 뒷부분)에서는 프로젝트를 만들고 이러한 자격 증명을 생성 하는 방법에 대 한 자세한 지침을 제공 합니다. [Firebase 콘솔](https://console.firebase.google.com/)에서 새 프로젝트를 만들 때 [FCM를 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)에 설명 된 대로 xamarin.ios 프로젝트에이 파일을 추가 하 &ndash; **google-service. json** 이라는 자격 증명 파일이 만들어집니다.

다음 섹션에서는 클라이언트 앱이 FCM를 통해 앱 서버와 통신할 때 이러한 자격 증명을 사용 하는 방법을 설명 합니다.

<a name="registration" />

### <a name="registration-with-fcm"></a>FCM를 사용 하 여 등록

메시징 하려면 먼저 클라이언트 앱이 FCM에 등록 되어야 합니다. 클라이언트 앱은 다음 다이어그램에 표시 된 등록 단계를 완료 해야 합니다.

[![앱 등록 단계 다이어그램](firebase-cloud-messaging-images/02-app-registration-sml.png)](firebase-cloud-messaging-images/02-app-registration.png#lightbox)

1. 클라이언트 앱은 FCM에 연결 하 여 보낸 사람 ID, API 키 및 앱 ID를 FCM에 전달 하는 등록 토큰을 가져옵니다.

2. FCM는 클라이언트 앱에 등록 토큰을 반환 합니다.

3. 클라이언트 앱 (선택 사항)이 등록 토큰을 앱 서버에 전달 합니다.

앱 서버는 나중에 클라이언트 앱과 통신 하기 위해 등록 토큰을 캐시 합니다. 앱 서버는 등록 토큰이 수신 되었음을 나타내는 승인을 클라이언트 앱에 다시 보낼 수 있습니다. 이 핸드셰이크를 수행한 후 클라이언트 앱은 앱 서버에서 메시지를 수신 하거나 메시지를 보낼 수 있습니다. 이전 토큰이 손상 된 경우 클라이언트 앱에서 새 등록 토큰을 받을 수 있습니다. 앱이 등록 토큰 업데이트를 받는 방법에 대 한 예제는 [FCM를 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md) 을 참조 하세요.

클라이언트 앱이 더 이상 앱 서버에서 메시지를 수신 하지 않을 때 앱 서버에 등록 토큰을 삭제 하는 요청을 보낼 수 있습니다. 클라이언트 앱이 장치에서 제거 되 면 FCM가이를 감지 하 고 등록 토큰을 삭제 하도록 앱 서버에 자동으로 알립니다.

### <a name="downstream-messaging"></a>다운스트림 메시징

다음 다이어그램에서는 Firebase 클라우드 메시징에서 다운스트림 메시지를 저장 하 고 전달 하는 방법을 보여 줍니다.

[![FCM에서 다운스트림 메시징의 저장 및 전달 사용](firebase-cloud-messaging-images/03-downstream-sml.png)](firebase-cloud-messaging-images/03-downstream.png#lightbox)

앱 서버는 클라이언트 앱에 다운스트림 메시지를 보낼 때 위 다이어그램에 열거 된 것과 같은 단계를 사용 합니다.

1. 앱 서버는 FCM로 메시지를 보냅니다.

2. 클라이언트 장치를 사용할 수 없는 경우 FCM 서버는 나중에 전송할 수 있도록 메시지를 큐에 저장 합니다. 메시지는 최대 4 주 동안 FCM 저장소에 유지 됩니다. 자세한 내용은 [메시지의 수명 설정](https://firebase.google.com/docs/cloud-messaging/concept-options#ttl)을 참조 하세요.

3. 클라이언트 장치를 사용할 수 있는 경우 FCM는 해당 장치의 클라이언트 앱에 메시지를 전달 합니다.

4. 클라이언트 앱은 FCM에서 메시지를 수신 하 고 처리 한 다음 사용자에 게 표시 합니다. 예를 들어 메시지가 원격 알림 인 경우 알림 영역에 사용자에 게 표시 됩니다.

이 메시징 시나리오에서 (앱 서버는 단일 클라이언트 앱에 메시지를 보내는 경우) 메시지의 길이는 최대 4kB가 될 수 있습니다.

Android에서 다운스트림 FCM 메시지를 수신 하는 방법에 대 한 자세한 내용은 [FCM를 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)을 참조 하세요.

### <a name="topic-messaging"></a>토픽 메시지

*토픽 메시징은* 앱 서버에서 특정 토픽에 옵트인 한 여러 장치에 메시지를 보낼 수 있도록 합니다. Firebase Console Notification GUI를 통해 토픽 메시지를 작성 하 고 보낼 수도 있습니다. FCM는 구독 된 클라이언트에 대 한 토픽 메시지의 라우팅 및 전달을 처리 합니다. 이 기능은 날씨 경고, 주식 시세, 헤드라인 뉴스 등의 메시지에 사용할 수 있습니다.

[![토픽 메시징 다이어그램](firebase-cloud-messaging-images/04-topic-messaging-sml.png)](firebase-cloud-messaging-images/04-topic-messaging.png#lightbox)

다음 단계는 이전에 설명한 대로 클라이언트 앱이 등록 토큰을 얻은 후 토픽 메시지에서 사용 됩니다.

1. 클라이언트 앱은 FCM에 구독 메시지를 전송 하 여 토픽을 구독 합니다.

2. 앱 서버는 배포를 위해 토픽 메시지를 FCM에 보냅니다.

3. FCM 토픽 메시지를 해당 항목을 구독 한 클라이언트에 전달 합니다.

Firebase 토픽 메시징에 대 한 자세한 내용은 Google 's [Android의 토픽 메시지](https://firebase.google.com/docs/cloud-messaging/android/topic-messaging)를 참조 하세요.

<a name="setup_fcm" />

## <a name="setting-up-firebase-cloud-messaging"></a>Firebase 클라우드 메시징 설정

앱에서 FCM 서비스를 사용 하려면 먼저 [Firebase 콘솔](https://console.firebase.google.com/)을 통해 새 프로젝트를 만들거나 기존 프로젝트를 가져와야 합니다. 다음 단계를 사용 하 여 앱에 대 한 Firebase 클라우드 메시징 프로젝트를 만듭니다.

1. Google 계정 (예: Gmail 주소)으로 [Firebase 콘솔](https://console.firebase.google.com/) 에 로그인 하 고 **새 프로젝트 만들기**를 클릭 합니다.

    [새 프로젝트 만들기 단추![](firebase-cloud-messaging-images/05-firebase-console-sml.png)](firebase-cloud-messaging-images/05-firebase-console.png#lightbox)

    기존 프로젝트가 있는 경우 **Google 프로젝트 가져오기**를 클릭 합니다.

2. **프로젝트 만들기** 대화 상자에서 프로젝트의 이름을 입력 하 고 **프로젝트 만들기**를 클릭 합니다. 다음 예제에서는 **XamarinFCM** 이라는 새 프로젝트를 만듭니다.

    [프로젝트 만들기 대화 상자![](firebase-cloud-messaging-images/06-create-a-project-sml.png)](firebase-cloud-messaging-images/06-create-a-project.png#lightbox)

3. Firebase 콘솔 **개요**에서 **Android 앱에 Firebase 추가**를 클릭 합니다.

    [Android 앱에 Firebase 추가![](firebase-cloud-messaging-images/07-add-firebase-sml.png)](firebase-cloud-messaging-images/07-add-firebase.png#lightbox)

4. 다음 화면에서 앱의 패키지 이름을 입력 합니다. 이 예에서는 패키지 이름이 **com .cfcmfcmexample**입니다. 이 값은 Android 앱의 패키지 이름과 일치 해야 합니다. 앱 애칭은 **앱 애칭** 필드에도 입력할 수 있습니다.

    [FCM 예제를 앱 애칭으로 입력![](firebase-cloud-messaging-images/08-package-name-sml.png)](firebase-cloud-messaging-images/08-package-name.png#lightbox)

5. 앱에서 동적 링크, 초대 또는 Google 인증을 사용 하는 경우 디버그 서명 인증서도 입력 해야 합니다. 서명 인증서를 찾는 방법에 대 한 자세한 내용은 [키 저장소의 MD5 또는 SHA1 서명 찾기](~/android/deploy-test/signing/keystore-signature.md)를 참조 하세요.
    이 예제에서는 서명 인증서를 비워 둡니다.

6. **앱 추가**를 클릭 합니다.

    [앱 추가 단추를 클릭![](firebase-cloud-messaging-images/09-add-app-sml.png)](firebase-cloud-messaging-images/09-add-app.png#lightbox)

    앱에 대 한 서버 API 키와 클라이언트 ID가 자동으로 생성 됩니다. 이 정보는 **앱 추가**를 클릭 하면 자동으로 다운로드 되는 **google 서비스의 json** 파일에 패키지 됩니다.
    이 파일은 안전한 위치에 저장 해야 합니다.

Android에서 FCM 푸시 알림 메시지를 수신 하기 위해 **google 서비스** 를 앱 프로젝트에 추가 하는 방법에 대 한 자세한 예제는 [FCM를 사용 하 여 원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md)을 참조 하세요.

## <a name="for-further-reading"></a>추가 정보

- Google의 [Firebase 클라우드 메시징은](https://firebase.google.com/docs/cloud-messaging/) Firebase 클라우드 메시징의 주요 기능에 대 한 개요, 작동 방법에 대 한 설명 및 설치 지침을 제공 합니다.

- Google의 [빌드 앱 서버 보내기 요청](https://firebase.google.com/docs/cloud-messaging/send-message) 은 앱 서버와 메시지를 보내는 방법을 설명 합니다.

- [Rfc 6120](https://tools.ietf.org/html/rfc6120) 및 [RFC 6121](https://tools.ietf.org/html/rfc6121) 은 xmpp (확장할 수 있는 메시징 및 현재 상태 프로토콜)를 설명 하 고 정의 합니다.

- [FCM 메시지 정보](https://firebase.google.com/docs/cloud-messaging/concept-options) 는 Firebase 클라우드 메시징과 함께 보낼 수 있는 다양 한 유형의 메시지를 설명 합니다.

## <a name="summary"></a>요약

이 문서에서는 FCM (Firebase Cloud Messaging)의 개요를 제공 했습니다. 앱 서버와 클라이언트 앱 간의 메시징을 식별 하 고 권한을 부여 하는 데 사용 되는 다양 한 자격 증명에 대해 설명 했습니다. 등록 및 다운스트림 메시징 시나리오를 설명 하 고 FCM services를 사용 하기 위해 FCM에 앱을 등록 하는 단계에 대해 설명 했습니다.

## <a name="related-links"></a>관련 링크

- [Firebase 클라우드 메시징](https://firebase.google.com/docs/cloud-messaging/)
