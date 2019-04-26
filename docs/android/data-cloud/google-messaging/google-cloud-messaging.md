---
title: Google Cloud Messaging
description: Google Cloud Messaging (GCM)는 모바일 앱 및 서버 응용 프로그램 간의 메시징에 유용한 서비스입니다. GCM 작동 하는 방법의 개요를 제공 하는이 문서 및 앱이 GCM 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/12/2018
ms.openlocfilehash: c2ca567ffcb247622d1b3e8f3e0136c453723b96
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61017683"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

_Google Cloud Messaging (GCM)는 모바일 앱 및 서버 응용 프로그램 간의 메시징에 유용한 서비스입니다. GCM 작동 하는 방법의 개요를 제공 하는이 문서 및 앱이 GCM 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Google Cloud Messaging 로고](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 Google Cloud Messaging가 라우팅하는 방법 간에 앱 및 앱 서버의 메시지의 대략적인 개요를 제공 및 앱에서 GCM 서비스를 사용할 수 있도록 자격 증명을 획득 하는 것에 대 한 단계별 절차를 제공 합니다.

> [!NOTE]
> GCM에 의해 대체 되었습니다 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM 서버 및 클라이언트 Api [되지](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) 더 이상 사용할 수 있는 한 빨리 2019 년 4 월 11 일을 합니다.

## <a name="overview"></a>개요

Google Cloud Messaging (GCM)는 전송, 라우팅 및 서버 응용 프로그램 및 모바일 클라이언트 앱 사이 메시지 큐를 처리 하는 서비스입니다. A *클라이언트 앱* 은 장치에서 실행 되는 GCM 사용 앱. 합니다 *응용 프로그램 서버* (또는 귀하의 회사에서 제공)는 클라이언트 앱에서 GCM을 통해 통신할 GCM 지원 서버:

[![GCM은 클라이언트 앱과 앱 서버 간에 있습니다](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

앱 서버 GCM을 사용 하 여 단일 장치, 장치, 그룹 또는 항목에 가입 된 장치 수가 메시지를 보낼 수 있습니다. 클라이언트 앱 GCM을 사용 하 여 다운스트림 메시지 (예: 원격 알림을 받기) 응용 프로그램 서버에서 구독할 수 있습니다. 또한 GCM 수 있도록 클라이언트 앱의 앱 서버로 다시 업스트림 메시지를 보내도록 합니다.

GCM에 대 한 응용 프로그램 서버를 구현 하는 방법에 대 한 내용은 [GCM 연결 서버에 대 한](https://developers.google.com/cloud-messaging/server)합니다.



## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging 작업

앱 서버에서 메시지를 보낼 메시지를 보내면 다운스트림 응용 프로그램 서버에서 클라이언트 앱에 *GCM 연결 서버*; GCM 연결 서버를 클라이언트 앱을 실행 하는 장치에 메시지를 전달 합니다. HTTP를 통해 메시지를 보낼 수 있습니다 또는 [XMPP](https://developers.google.com/cloud-messaging/ccs) (Extensible Messaging 및 현재 상태 프로토콜). 클라이언트 앱은 항상 연결 되어 있지 않으므로 또는 실행 하 고 GCM 연결 서버 큐에 추가 하 고 저장소 메시지를 다시 연결 하 고 사용할 클라이언트 앱으로 전송 합니다. 마찬가지로, GCM가 큐에 넣기 업스트림 메시지를 클라이언트 앱에서 앱 서버에 앱 서버를 사용할 수 없는 경우.

GCM 앱 서버 및 클라이언트 앱을 식별 하려면 다음 자격 증명을 사용 하 고 이러한 자격 증명을 사용 하 여 GCM 통해 메시지 트랜잭션의 권한을 부여 하려면:

-   **API 키** &ndash; 는 *API 키* Google 서비스;에 응용 프로그램 서버 액세스를 제공 합니다. GCM 앱 서버 인증에이 키를 사용 합니다.
    API 키를 먼저 가져와야 GCM 서비스를 사용 하려면 먼저 합니다 [Google 개발자 콘솔](https://console.developers.google.com/) 만들어를 *프로젝트*합니다. API 키 보안을 유지 해야 합니다. API 키를 보호 하는 방법에 대 한 자세한 내용은 참조 하세요. [API 키를 사용 하 여 안전 하 게 하는 것에 대 한 유용한](https://support.google.com/cloud/answer/6310037?hl=en)합니다.

-   **보낸 사람 ID** &ndash; 는 *보낸 사람 ID* 앱 서버 클라이언트 앱에 권한을 부여 &ndash; 클라이언트 앱으로 메시지를 보내도록 허용 되는 앱 서버를 식별 하는 고유 번호를 것입니다.
    보낸 사람 ID가도 프로젝트 번호로; 프로젝트를 등록할 때 Google 개발자 콘솔에서 보낸 사람 ID를 가져옵니다.

-   **등록 토큰** &ndash; 는 *등록 토큰* GCM id 제공된 된 장치에 클라이언트 앱입니다. 등록 토큰은 런타임에 생성 &ndash; 처음 등록할 때 GCM을 사용 하 여 장치에서 실행 하는 동안 응용 프로그램 등록 토큰을 수신 합니다. 등록 토큰을 GCM에서 메시지를 받는 클라이언트 앱 (특정 장치에서 실행)의 인스턴스를 인증 합니다.

-   **응용 프로그램 ID** &ndash; GCM에서 메시지를 수신 하도록 등록 된 클라이언트 앱 (지정된 된 장치는 무관)의 id입니다. Android 응용 프로그램 ID에에 기록 된 패키지 이름과 **AndroidManifest.xml**와 같은 `com.xamarin.gcmexample`합니다.

[설정 구성 Google Cloud Messaging](#settingup) (이 가이드 뒷부분) 프로젝트를 만들고 이러한 자격 증명을 생성 하기 위한 자세한 지침을 제공 합니다.

다음 섹션에서는 클라이언트 앱이 GCM 통해 앱 서버와 통신 하는 경우 이러한 자격 증명은 사용 하는 방법을 설명 합니다.



### <a name="registration-with-gcm"></a>GCM 사용 하 여 등록

메시징 수행 되기 전 GCM을 사용 하 여 장치에 설치 된 클라이언트 앱을 먼저 등록 해야 합니다. 클라이언트 앱은 다음 다이어그램에 표시 되는 등록 단계를 완료 해야 합니다.

[![앱 등록 단계](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  GCM 보낸 사람 ID를 전달할 등록 토큰을 가져와야 하는 GCM 클라이언트 앱에 연결 합니다.

2.  GCM은 클라이언트 앱 등록 토큰을 반환합니다.

3.  클라이언트 앱에 앱 서버에 등록 토큰을 전달합니다.

앱 서버는 클라이언트 앱을 사용 하 여 후속 통신에 대 한 등록 토큰을 캐시 합니다. 필요에 따라 앱 서버 등록 토큰을 받았음을 나타내기 위해 클라이언트 앱에 다시 승인을 보낼 수 있습니다. 이 핸드셰이크에 발생 한 후 클라이언트 앱 수에서 메시지를 수신 (또는 메시지를 보낼) 앱 서버.

클라이언트 앱은 더 이상 앱 서버에서 메시지를 수신 하려고, 등록 토큰을 삭제 하려면 앱 서버에 요청을 보낼 수 없습니다. 클라이언트 앱 (이 문서의 뒷부분에서 설명) 항목 메시지를 받고, 항목에서 구독 취소할 수 있습니다.
클라이언트 앱이 장치에서 제거 된 경우 GCM이를 감지 하 고 자동으로 등록 토큰을 삭제 하려면 응용 프로그램 서버에 알립니다.

Google [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 자세한 세부 정보에서 등록 프로세스를 설명 합니다. 취소 하 고 구독을 설명 하 고 클라이언트 앱을 제거할 때에 등록 취소 과정을 설명 합니다.



### <a name="downstream-messaging"></a>다운스트림 메시징

앱 서버에서 클라이언트 앱에는 다운스트림 메시지를 보낼 때에 다음 다이어그램에 나와 있는 단계를 따릅니다.

[![다운스트림 메시징 저장소 및 전달 다이어그램](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  앱 서버 GCM에 메시지를 보냅니다.

2.  클라이언트 장치를 사용할 수 없는 경우 GCM 서버 나중에 큐에 메시지를 저장 합니다.

3.  클라이언트 장치를 사용할 수 있는 GCM 클라이언트 앱 해당 장치에 메시지를 보냅니다.

4.  클라이언트 앱은 GCM에서 메시지를 수신 하 고 적절 하 게 처리 합니다. 예를 들어 메시지 원격 알림을 인 경우 사용자에 게 표시 됩니다.

(앱 서버 보내는 메시지가 단일 클라이언트 앱)이 메시징 시나리오에서는 메시지가 최대 4kb 길이가 될 수 있습니다.

자세한 내용은 코드 샘플 등 Android에서 다운스트림 GCM 메시지를 수신 하는 방법에 대 한 [Remote Notifications](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)합니다.


#### <a name="topic-messaging"></a>항목에서는 메시징

*항목에서는 메시징* 는 다운스트림 메시지 유형의 앱 서버 (예: 일기 예보) 토픽을 구독 하는 여러 클라이언트 앱 장치에 단일 메시지를 보냅니다. 항목 메시지에는 길이가 최대 2KB 수 및 항목 메시징 앱 당 최대 1 백만 구독을 지원 합니다. GCM 메시징 항목 으로만 사용 되는, 클라이언트 앱 등록 토큰을 응용 프로그램 서버에 보낼 않아도 됩니다. Google [구현 항목 메시징](https://developers.google.com/cloud-messaging/topic-messaging) 특정 토픽을 구독 하는 여러 장치에 응용 프로그램 서버에서 메시지를 보내는 방법에 설명 합니다.



#### <a name="group-messaging"></a>그룹에서 메시징

*메시징 그룹* 는 다운스트림 메시지 유형의 앱 서버 그룹 (예를 들어 단일 사용자에 게 속한 장치는 그룹)에 속하는 여러 클라이언트 앱 장치에 단일 메시지를 보냅니다. 그룹 메시지에는 iOS 장치에 대 한 길이가 최대 2KB 수 및 Android 장치에 대 한 길이 최대 4kb입니다. 그룹은 멤버가 20 개로 제한 됩니다. Google [장치 그룹 메시징](https://developers.google.com/cloud-messaging/notifications) 앱 서버 그룹에 속해 있는 장치에서 실행 되는 여러 클라이언트 앱 인스턴스를 단일 메시지를 보내는 방법에 대해 설명 합니다.


### <a name="upstream-messaging"></a>업스트림 메시징

클라이언트 앱을 지 원하는 서버에 연결 하는 경우 [XMPP](https://developers.google.com/cloud-messaging/ccs), 다음 다이어그램에 설명 된 대로 메시지 앱 서버로 다시 보낼 수 있습니다.

[![업스트림 메시징 다이어그램](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  클라이언트 앱 GCM XMPP 연결 서버에 메시지를 보냅니다.

2.  앱 서버 연결이 끊기면 GCM 서버 나중에 전달 하는 것에 대 한 큐에 메시지를 저장 합니다.

3.  앱 서버 다시 연결 되 면 GCM 앱 서버에 메시지를 전달 합니다.

4.  앱 서버 클라이언트 앱의 id를 확인 하는 메시지를 구문 분석 한 다음 "ack" 메시지 수신을 승인 하는 GCM에 보냅니다.

5.  앱 서버는 메시지를 처리합니다.

Google [업스트림 메시지](https://developers.google.com/cloud-messaging/ccs#upstream) JSON 인코딩된 메시지의 구조 및 Google의 XMPP 기반 클라우드 연결 서버를 실행 하는 응용 프로그램 서버에 보내도록 하는 방법을 설명 합니다.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging 설정

앱에서 GCM 서비스를 사용할 수 있습니다, 전에 먼저 Google의 GCM 서버에 대 한 액세스 자격 증명을 가져와야 합니다. 다음 섹션에서는이 프로세스를 완료 하는 데 필요한 단계를 설명 합니다.



### <a name="enable-google-services-for-your-app"></a>앱에 대 한 Google 서비스를 사용 하도록 설정

1.  에 로그인 합니다 [Google 개발자 콘솔](https://developers.google.com/mobile/add?platform=android) 에 Google 계정 (즉, 사용자 gmail 주소) 및 새 프로젝트를 만듭니다. 기존 프로젝트를 만든 경우 하고자 한다면 GCM을 사용 하도록 설정 하는 프로젝트를 선택 합니다. 다음 예제에서는 새 프로젝트를 호출 **XamarinGCM** 만들어집니다.

    [![XamarinGCM 프로젝트 만들기](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  다음으로 앱에 대 한 패키지 이름 (이 예제에서는 패키지 이름은 **com.xamarin.gcmexample**)을 클릭 **선택 계속 서비스를 구성 하 고**:

    [![패키지 이름을 입력합니다.](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    참고가이 패키지 이름은 앱에 대 한 응용 프로그램 ID 이기도 합니다.

3.  **선택 하 고 서비스를 구성** 섹션에서는 앱에 추가할 수 있는 Google 서비스를 나열 합니다. 클릭 **클라우드 메시징**:

    [![클라우드 선택 메시징](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  다음으로, 클릭 **GOOGLE CLOUD MESSAGING을 사용 하도록 설정**:

    [![Google Cloud Messaging 사용](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **서버 API 키** 와 **보낸 사람 ID** 앱에 대해 생성 됩니다. 이러한 값을 기록 하 고 클릭 **닫기**:

    [![서버 API 키 및 발신자 ID 표시](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API 키를 보호 &ndash; 공용 아닙니다. API 키가 손상 되 면 권한이 없는 서버는 클라이언트 응용 프로그램에 메시지 게시 수 있습니다.
    [API 키를 사용 하 여 안전 하 게 하는 것에 대 한 유용한](https://support.google.com/cloud/answer/6310037?hl=en) API 키를 보호 하는 데 유용한 지침을 제공 합니다.



### <a name="view-your-project-settings"></a>프로젝트 설정 보기

에 로그인 하 여 언제 든 지 프로젝트 설정을 볼 수 있습니다 합니다 [Google 클라우드 콘솔](https://console.cloud.google.com/) 프로젝트를 선택 합니다. 예를 들어 볼 수 있습니다 합니다 **보낸 사람 ID** 페이지의 맨 위에 있는 풀 다운 메뉴에서에서 프로젝트를 선택 하 여 (이 예제에서는 프로젝트 라고 **XamarinGCM**). 보낸 사람 ID가이 스크린샷에 표시 된 대로 프로젝트 번호 (여기에 보낸 사람 ID가 **9349932736**):

[![보낸 사람 ID 보기](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

보려는 합니다 **API 키**, 클릭 **API 관리자** 을 클릭 한 다음 **자격 증명**:

[![API 키 보기](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>추가 정보

-   Google [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 클라이언트 등록 프로세스를 자세히 설명 합니다. 자동 다시 시도 구성 하 고 등록 상태를 동기화 상태로 유지 하는 방법에 대 한 정보를 제공 합니다.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) 하 고 [RFC 6121](https://tools.ietf.org/html/rfc6121) 설명 하 고 확장할 수 있는 메시징 및 현재 상태 프로토콜 (XMPP)를 정의 합니다.



## <a name="summary"></a>요약

이 문서에서는 Google Cloud Messaging (GCM)의 개요를 제공 합니다. 식별 하 고 앱 서버 및 클라이언트 응용 프로그램 간의 메시징 권한을 부여 하는 데 사용 되는 다양 한 자격 증명을 설명 했습니다. 이 가장 일반적인 메시징 시나리오에서 설명 하 고 GCM 서비스를 사용 하는 GCM을 사용 하 여 앱을 등록 하는 단계를 설명 합니다.


## <a name="related-links"></a>관련 링크

- [클라우드 메시징](https://developers.google.com/cloud-messaging/)
