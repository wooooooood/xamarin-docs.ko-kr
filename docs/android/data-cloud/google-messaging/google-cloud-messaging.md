---
title: Google 클라우드 메시징
description: Google 클라우드 메시징 (GCM)는 모바일 응용 프로그램 및 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 GCM 작동 하는 방법에 대 한 개요를 제공 하 고 응용 프로그램 GCM에서 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/12/2018
ms.openlocfilehash: 29cccf414759a79a8ba74dfc35b7ba9f6a1cc5d6
ms.sourcegitcommit: bc39d85b4585fcb291bd30b8004b3f7edcac4602
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="google-cloud-messaging"></a>Google 클라우드 메시징

_Google 클라우드 메시징 (GCM)는 모바일 응용 프로그램 및 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 GCM 작동 하는 방법에 대 한 개요를 제공 하 고 응용 프로그램 GCM에서 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Google Cloud Messaging 로고](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 방법을 응용 프로그램 및 응용 프로그램 서버 사이 메시지를 라우팅하 Google Cloud Messaging에 대 한 높은 수준의 개요를 제공 하 고 앱 GCM 서비스를 사용할 수 있도록 자격 증명을 인식 하기 위한 단계별 절차를 제공 합니다.

> [!NOTE]
> GCM에 의해 대체 되었습니다 [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) (FCM).
> GCM 서버 및 클라이언트 Api [사용이 중단 된](https://firebase.googleblog.com/2018/04/time-to-upgrade-from-gcm-to-fcm.html) 는 더 이상 사용할 수 2019 년 4 월 11 빨리 합니다.

## <a name="overview"></a>개요

Google 클라우드 메시징 (GCM)는 전송, 라우팅 및 서버 응용 프로그램 및 모바일 클라이언트 응용 프로그램 사이 메시지 큐를 처리 하는 서비스입니다. A *클라이언트 응용 프로그램* 장치에서 실행 되는 GCM 사용이 가능한 앱입니다. *응용 프로그램 서버* (또는 귀하의 회사에서 제공)는 클라이언트 앱에서 GCM을 통해 통신할 GCM 사용이 가능한 서버:

[![클라이언트 응용 프로그램 및 응용 프로그램 서버 간에 있는 GCM](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM을 사용 하 여, 응용 프로그램 서버는 메시지 단일 장치, 장치, 그룹 또는 항목에 등록 된 장치의 수를 보낼 수 있습니다. 클라이언트 앱 (예: 원격 알림의 받을) 응용 프로그램 서버에서 다운스트림 메시지에 등록 하도록 GCM를 사용할 수 있습니다. 또한, GCM은 메시지를 보내는 업스트림 응용 프로그램 서버에 클라이언트 앱에 대 한 지원 합니다.

GCM에 대 한 응용 프로그램 서버를 구현 하는 방법에 대 한 정보를 참조 하십시오. [GCM 연결 서버에 대 한](https://developers.google.com/cloud-messaging/server)합니다.



## <a name="google-cloud-messaging-in-action"></a>Google 클라우드 메시징 작업

다운스트림 메시지 응용 프로그램 서버에서 클라이언트 응용 프로그램, 응용 프로그램 서버 메시지를 보내는 *GCM 서버에 연결할*; GCM 연결 서버에 클라이언트 앱을 실행 하는 장치에는 메시지를 전달 합니다. HTTP를 통해 메시지를 보낼 수 있습니다 또는 [XMPP](https://developers.google.com/cloud-messaging/ccs) (확장할 수 있는 메시징 및 현재 상태 프로토콜). 클라이언트 응용 프로그램은 항상 연결 되어 있지 않으므로 실행 되었거나 실행에서 GCM 연결 서버 큐에 넣습니다 및 저장소 메시지를 다시 연결 하 고 사용할 수 있을 때 클라이언트 응용 프로그램으로 전송 합니다. 마찬가지로, GCM가 큐에 넣지 업스트림 메시지를 클라이언트 응용 프로그램에서 응용 프로그램 서버에 응용 프로그램 서버를 사용할 수 없는 경우.

GCM 다음 자격 증명을 사용 하 여 응용 프로그램 서버 및 클라이언트 앱을 식별 하 고 이러한 자격 증명을 사용 하 여 GCM 통해 메시지 트랜잭션의 권한을 부여 하려면:

-   **API 키** &ndash; 는 *API 키* Google 서비스;에 응용 프로그램 서버 액세스를 제공 합니다. 이 키를 사용 하 여 응용 프로그램 서버를 인증 하는 GCM 합니다.
    먼저에서 API 키를 획득 해야 GCM 서비스를 사용 하려면 먼저는 [Google 개발자 콘솔](https://console.developers.google.com/) 만들어는 *프로젝트*합니다. API 키를 안전한; 유지 합니다. API 키를 보호 하는 방법에 대 한 자세한 내용은 참조 [API 키를 사용 하 여 안전 하 게 하는 것에 대 한 유용한](https://support.google.com/cloud/answer/6310037?hl=en)합니다.

-   **보낸 사람 ID** &ndash; 는 *보낸 사람 ID* 클라이언트 응용 프로그램에 응용 프로그램 서버에 권한을 부여 &ndash; 클라이언트 응용 프로그램에 메시지를 보낼 수 있는 응용 프로그램 서버를 식별 하는 고유 번호입니다.
    보낸 사람 ID 이기도 프로젝트 번호입니다. 프로젝트를 등록 하는 경우 Google 개발자 콘솔에서 보낸 사람 ID를 가져옵니다.

-   **등록 토큰** &ndash; 는 *등록 토큰* 제공된 된 장치에 클라이언트 앱의 GCM id입니다. 런타임 시 등록 토큰이 생성 될 &ndash; 처음 등록할 때 GCM과 함께 장치에서 실행 하는 동안 응용 프로그램 등록 토큰을 수신 합니다. GCM에서 메시지를 받도록 등록 토큰에 클라이언트 앱 (해당 특정 장치에서 실행)의 인스턴스 권한도 부여 됩니다.

-   **응용 프로그램 ID** &ndash; 응용 프로그램의 클라이언트 (지정된 된 장치 독립적) GCM에서 메시지를 수신 하도록 등록 하는 id입니다. Android에서는 응용 프로그램 ID는 패키지 이름에 기록 된 **AndroidManifest.xml**와 같은 `com.xamarin.gcmexample`합니다.

[설정은를 Google Cloud Messaging](#settingup) (이 가이드의 뒷부분에) 프로젝트를 만들고 이러한 자격 증명을 생성 하기 위한 자세한 지침을 제공 합니다.

다음 섹션에서는 클라이언트 응용 프로그램이 GCM 통해 응용 프로그램 서버와 통신 하는 경우 이러한 자격 증명은 사용 하는 방법을 설명 합니다.



### <a name="registration-with-gcm"></a>GCM 등록

메시징 수행 될 수 전에 GCM을 장치에 설치 하는 클라이언트 응용 프로그램 등록 해야 합니다. 클라이언트 앱 등록 단계는 다음 다이어그램에 표시 된 것을 완료 해야 합니다.

[![앱 등록 단계](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1.  GCM GCM에 보낸 사람 ID를 전달 하는 등록 토큰을 가져오려면 클라이언트 응용 프로그램에 연결 합니다.

2.  GCM은 클라이언트 응용 프로그램 등록 토큰을 반환합니다.

3.  클라이언트 응용 프로그램 등록 토큰의 앱 서버로 전달합니다.

응용 프로그램 서버는 후속 통신할 클라이언트 응용 프로그램 등록 토큰을 캐시 합니다. 필요에 따라 응용 프로그램 서버 등록 토큰 받았음을 나타내기 위해 클라이언트 응용 프로그램에 다시 승인을 보낼 수 있습니다. 이 핸드셰이크 발생 한 후 클라이언트 응용 프로그램 수에서 메시지를 수신 (또는 메시지를 보낼) 응용 프로그램 서버.

클라이언트 응용 프로그램이 더 이상 응용 프로그램 서버에서 메시지를 수신 하려면, 등록 토큰을 삭제 하려면 응용 프로그램 서버에 요청을 보낼 수 없습니다. 클라이언트 응용 프로그램 (이 문서의 뒷부분에 설명) 항목 메시지를 받고,이 항목에서 구독 취소할 수 있습니다.
클라이언트 앱이 장치에서 제거 된 경우 GCM에서이 점을 검색 하 고 자동으로 등록 토큰을 삭제 하려면 응용 프로그램 서버에 알립니다.

Google의 [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 자세히;에서 등록 프로세스를 설명 합니다. 등록을 취소 하 고 템플릿의 설명 및 클라이언트 응용 프로그램을 제거할 때에 등록 취소 과정을 설명 합니다.



### <a name="downstream-messaging"></a>다운스트림 메시징

응용 프로그램 서버에 클라이언트 앱에는 다운스트림 메시지를 보내는 다음 다이어그램에 표시 된 단계 따릅니다.

[![다운스트림 메시징 저장소 및 앞으로 다이어그램](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1.  응용 프로그램 서버 GCM에 메시지를 보냅니다.

2.  클라이언트 장치에 사용할 수 없는 경우 GCM 서버 이후 전송을 위해 큐에 메시지를 저장 합니다.

3.  클라이언트 장치가 사용할 수 있을 때 GCM 해당 장치에 클라이언트 앱에 메시지를 보냅니다.

4.  클라이언트 응용 프로그램 GCM에서 메시지를 받아 적절 하 게 처리 합니다. 예를 들어 원격 알림 메시지가 사용 하는 경우 사용자에 게 표시 됩니다.

(여기서 응용 프로그램 서버는 메시지를 보냅니다 단일 클라이언트 응용 프로그램)이 메시징 시나리오에서는 메시지가 최대 4kb 길이가 될 수 있습니다.

자세한 내용은 (코드 샘플 포함) Android에서 다운스트림 GCM 메시지를 수신 하는 방법에 대 한 [원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)합니다.


#### <a name="topic-messaging"></a>항목 메시징

*항목 메시징* 는 다운스트림 메시징 유형의 응용 프로그램 서버 (예: 일기 예보) 항목에 등록 하는 여러 클라이언트 응용 프로그램 장치를 단일 메시지를 보냅니다. 앱 당 최대 1 백만 구독을 지 원하는 항목 메시징 있으며 항목 메시지에는 길이가 최대 2KB 수 있습니다. GCM 메시징 항목에 대 한 사용량을 클라이언트 응용 프로그램 등록 토큰을 응용 프로그램 서버에 보내도록 않아도 됩니다. Google의 [항목 메시징 구현](https://developers.google.com/cloud-messaging/topic-messaging) 특정 항목에 등록 하는 여러 장치에 응용 프로그램 서버에서 메시지를 전송 하는 방법을 설명 합니다.



#### <a name="group-messaging"></a>그룹에서 메시징

*메시징 그룹* 는 다운스트림 메시징 유형의 응용 프로그램 서버 그룹 (예를 들어 단일 사용자에 속하는 장치 그룹)에 속하는 여러 클라이언트 응용 프로그램 장치를 단일 메시지를 보냅니다. 그룹 메시지가 iOS 장치에 대 한 길이가 최대 2KB 수 있는 4KB Android 장치에 대 한 길이가 최대 합니다. 그룹은 20 멤버의 최대 제한 합니다. Google의 [장치 그룹 메시징](https://developers.google.com/cloud-messaging/notifications) 앱 서버 그룹에 속한 장치에서 실행 되는 여러 클라이언트 응용 프로그램 인스턴스를 단일 메시지를 보낼 수 있는 방법을 설명 합니다.


### <a name="upstream-messaging"></a>업스트림 메시징

클라이언트 앱을 지 원하는 서버에 연결 하는 경우 [XMPP](https://developers.google.com/cloud-messaging/ccs), 다음 다이어그램에 나타난 것 처럼 메시지 응용 프로그램 서버에 다시 보낼 수 있습니다.

[![업스트림 메시징 다이어그램](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1.  클라이언트 응용 프로그램 GCM XMPP 연결 서버에 메시지를 보냅니다.

2.  앱 서버와 연결이 끊어진 경우 GCM 서버를 나중에 전달 하려면 큐에 메시지를 저장 합니다.

3.  응용 프로그램 서버를 다시 연결 된 경우 GCM의 앱 서버로 메시지를 전달 합니다.

4.  응용 프로그램 서버에 클라이언트 앱 id를 확인 하려면 메시지 구문 분석 한 후 "응답" 메시지 수신을 승인 하는 GCM에 보냅니다.

5.  응용 프로그램 서버에서 메시지를 처리합니다.

Google의 [업스트림 메시지](https://developers.google.com/cloud-messaging/ccs#upstream) JSON 인코딩된 메시지 및 Google의 XMPP 기반 클라우드 연결 서버를 실행 하는 응용 프로그램 서버에 보내야 하는 방법에 설명 합니다.


<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google 클라우드 메시징 설정

GCM 서비스 응용 프로그램을 사용 하려면 먼저 Google의 GCM 서버에 대 한 액세스 자격 증명에 먼저 획득 해야 합니다. 다음 섹션에서는이 프로세스를 완료 하는 데 필요한 단계를 설명 합니다.



### <a name="enable-google-services-for-your-app"></a>앱에 대 한 Google 서비스를 사용 하도록 설정

1.  에 로그인 된 [Google 개발자 콘솔](https://developers.google.com/mobile/add?platform=android) 프로그램 Google 계정 (즉, gmail 주소)를 새 프로젝트를 만듭니다. 기존 프로젝트를 사용 하도록 설정한 경우 될 GCM을 사용 하도록 설정 하는 프로젝트를 선택 합니다. 다음 예제에서는 새 프로젝트 호출 **XamarinGCM** 만들어집니다.

    [![XamarinGCM 프로젝트 만들기](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2.  다음으로 응용 프로그램에 대 한 패키지 이름을 입력 하세요 (이 예제에서는 패키지 이름이 **com.xamarin.gcmexample**)를 클릭 하 고 **계속을 선택 하 고 서비스를 구성**:

    [![패키지 이름을 입력합니다.](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    참고가 패키지 이름은 응용 프로그램에 대 한 응용 프로그램 ID 이기도 합니다.

3.  **선택 하 고 서비스 구성** 섹션 응용 프로그램에 추가할 수 있는 Google 서비스를 나열 합니다. 클릭 **메시징 클라우드**:

    [![클라우드 선택 메시징](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4.  그런 다음 클릭 **GOOGLE 클라우드 메시징 사용**:

    [![Google 클라우드 메시징 설정](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5.  A **서버 API 키** 및 **보낸 사람 ID** 응용 프로그램에 대해 생성 됩니다. 이러한 값을 기록 하 고 클릭 **닫기**:

    [![서버 API 키 및 표시 하는 보낸 사람 ID](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API 키를 보호 &ndash; 공용 용도가 아닙니다. API 키가 손상 될 경우 권한이 없는 서버 클라이언트 응용 프로그램에 메시지를 게시할 수 있습니다.
    [API 키를 사용 하 여 안전 하 게 하는 것에 대 한 유용한](https://support.google.com/cloud/answer/6310037?hl=en) API 키를 보호 하는 데 유용한 지침을 제공 합니다.



### <a name="view-your-project-settings"></a>프로젝트 설정 보기

에 로그인 하 여 프로젝트 설정을 언제 든 지 볼 수 있습니다는 [Google 클라우드 콘솔](https://console.cloud.google.com/) 프로젝트를 선택 하 고 있습니다. 예를 들어 볼 수 있습니다는 **보낸 사람 ID** 페이지 맨 위에 있는 풀 다운 메뉴에서에서 프로젝트를 선택 하 여 (이 예제에서는 프로젝트 라고 **XamarinGCM**). 보낸 사람 ID는이 스크린 샷에 표시 된 것 처럼 프로젝트 번호 (보낸 사람 ID 여기는 **9349932736**):

[![보낸 사람 ID 보기](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

보려는 **API 키**, 클릭 **API 관리자** 클릭 하 고 **자격 증명**:

[![API 키 보기](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)



## <a name="for-further-reading"></a>추가 정보

-   Google의 [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 좀 더 자세하게에서 클라이언트 등록 프로세스를 설명 하 고 자동 다시 시도 구성 하 고 동기화 등록 상태를 유지 하는 방법에 대 한 정보를 제공 합니다.

-   [RFC 6120](https://tools.ietf.org/html/rfc6120) 및 [RFC 6121](https://tools.ietf.org/html/rfc6121) 설명 하 고 확장할 수 있는 메시징 및 현재 상태 프로토콜 (XMPP)를 정의 합니다.



## <a name="summary"></a>요약

이 문서는 Google 클라우드 메시징 (GCM)의 개요를 제공 합니다. 그를 식별 하 고 응용 프로그램 서버 및 클라이언트 응용 프로그램 사이의 메시징 권한을 부여 하는 데 사용 되는 다양 한 자격 증명을 설명 합니다. 가장 일반적인 메시징 시나리오를 설명 하 고 GCM 서비스를 사용 하는 GCM과 함께 응용 프로그램을 등록 하는 단계를 설명 합니다.


## <a name="related-links"></a>관련 링크

- [메시징 클라우드](https://developers.google.com/cloud-messaging/)
