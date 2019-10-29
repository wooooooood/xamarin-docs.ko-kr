---
title: Google Cloud Messaging
description: GCM (Google Cloud Messaging)은 모바일 앱과 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 GCM이 작동 하는 방식에 대 한 개요를 제공 하 고, 앱에서 GCM을 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다.
ms.prod: xamarin
ms.assetid: DF8EF401-F63D-4BA0-B2C6-B22DF8FD60CB
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/02/2019
ms.openlocfilehash: 742555da24120eaeadcc4b6232b24d23f41da283
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023711"
---
# <a name="google-cloud-messaging"></a>Google Cloud Messaging

> [!WARNING]
> Google이 사용 되지 않는 GCM-4 월 10 일, 2018. 다음 문서 및 샘플 프로젝트는 더 이상 유지 되지 않을 수 있습니다. Google의 GCM 서버 및 클라이언트 Api는 2019 년 5 월 29 일에 곧 제거 될 예정입니다. Google은 GCM 앱을 FCM (Firebase Cloud Messaging)로 마이그레이션하는 것을 권장 합니다. GCM 사용 중단 및 마이그레이션에 대 한 자세한 내용은 [Google 사용 되지 않는 클라우드 메시징](https://developers.google.com/cloud-messaging/)을 참조 하세요.
>
> Xamarin에서 Firebase Cloud Messaging 사용을 시작 하려면 [Firebase 클라우드 메시징](firebase-cloud-messaging.md)을 참조 하세요.

_GCM (Google Cloud Messaging)은 모바일 앱과 서버 응용 프로그램 간의 메시징을 용이 하 게 하는 서비스입니다. 이 문서에서는 GCM이 작동 하는 방식에 대 한 개요를 제공 하 고, 앱에서 GCM을 사용할 수 있도록 Google 서비스를 구성 하는 방법을 설명 합니다._

[![Google Cloud Messaging 로고](google-cloud-messaging-images/preview-sml.png)](google-cloud-messaging-images/preview.png#lightbox)

이 항목에서는 앱과 앱 서버 간에 메시지를 라우팅하 Google Cloud Messaging는 방법에 대 한 개략적인 개요를 제공 하 고, 앱에서 GCM 서비스를 사용할 수 있도록 자격 증명을 획득 하는 단계별 절차를 제공 합니다.

## <a name="overview"></a>개요

GCM (Google Cloud Messaging)은 서버 응용 프로그램과 모바일 클라이언트 앱 간의 메시지 보내기, 라우팅 및 큐 처리를 처리 하는 서비스입니다. *클라이언트 앱* 은 장치에서 실행 되는 GCM 사용 앱입니다. 사용자 또는 회사에서 제공 하는 *앱 서버* 는 클라이언트 앱이 gcm을 통해 통신 하는 gcm 사용 서버입니다.

[![GCM은 클라이언트 앱과 앱 서버 사이에 상주 합니다.](google-cloud-messaging-images/01-server-gcm-app-sml.png)](google-cloud-messaging-images/01-server-gcm-app.png#lightbox)

GCM을 사용 하 여 앱 서버는 단일 장치, 장치 그룹 또는 토픽에 구독 된 여러 장치에 메시지를 보낼 수 있습니다. 클라이언트 앱은 GCM을 사용 하 여 앱 서버 (예: 원격 알림 수신)에서 다운스트림 메시지를 구독할 수 있습니다. 또한 GCM을 사용 하면 클라이언트 앱이 업스트림 메시지를 앱 서버로 다시 보낼 수 있습니다.

GCM 용 앱 서버를 구현 하는 방법에 대 한 자세한 내용은 [Gcm 연결 서버 정보](https://developers.google.com/cloud-messaging/server)를 참조 하세요.

## <a name="google-cloud-messaging-in-action"></a>Google Cloud Messaging 작업

다운스트림 메시지가 앱 서버에서 클라이언트 앱으로 전송 되 면 앱 서버는 *GCM 연결 서버*에 메시지를 보냅니다. 그러면 GCM 연결 서버가 클라이언트 앱을 실행 하는 장치에 메시지를 전달 합니다. HTTP 또는 [Xmpp](https://developers.google.com/cloud-messaging/ccs) (확장할 수 있는 메시징 및 현재 상태 프로토콜)를 통해 메시지를 보낼 수 있습니다. 클라이언트 앱은 항상 연결 되지 않거나 실행 되 고 있지 않기 때문에 GCM 연결 서버는 메시지를 큐 하 고 저장 한 후 다시 연결 하 여 사용할 수 있게 되 면 클라이언트 앱에 보냅니다. 마찬가지로, GCM은 앱 서버를 사용할 수 없는 경우 클라이언트 앱에서 앱 서버로 업스트림 메시지를 큐에 삽입 합니다.

GCM은 다음 자격 증명을 사용 하 여 앱 서버와 클라이언트 앱을 식별 하 고 이러한 자격 증명을 사용 하 여 GCM을 통해 메시지 트랜잭션에 권한을 부여 합니다.

- **Api 키 &ndash;** api *키를 사용 하면* 앱 서버에서 Google 서비스에 액세스할 수 있습니다. GCM은이 키를 사용 하 여 앱 서버를 인증 합니다.
    GCM 서비스를 사용 하려면 먼저 *프로젝트*를 만들어 [Google 개발자 콘솔](https://console.developers.google.com/) 에서 API 키를 가져와야 합니다. API 키를 안전 하 게 유지 해야 합니다. API 키를 보호 하는 방법에 대 한 자세한 내용은 [api 키를 안전 하 게 사용 하기 위한 모범 사례](https://support.google.com/cloud/answer/6310037?hl=en)를 참조 하세요.

- 보낸 **사람 id** &ndash; *발신자 id* 는 클라이언트 앱에 대 한 앱 서버 권한을 부여 하 &ndash; 클라이언트 앱에 메시지를 보낼 수 있는 앱 서버를 식별 하는 고유 번호입니다.
    보낸 사람 ID는 프로젝트 번호 이기도 합니다. 프로젝트를 등록할 때 Google 개발자 콘솔에서 보낸 사람 ID를 가져옵니다.

- **등록 토큰 &ndash;** 등록 *토큰* 은 지정 된 장치에서 클라이언트 앱의 GCM id입니다. 등록 토큰은 &ndash; 런타임에 생성 됩니다. 앱은 장치에서 실행 되는 동안 GCM에 처음 등록할 때 등록 토큰을 받습니다. 등록 토큰은 해당 특정 장치에서 실행 되는 클라이언트 앱의 인스턴스에 게 GCM에서 메시지를 수신 하는 권한을 부여 합니다.

- **응용 프로그램 id** &ndash; GCM에서 메시지를 수신 하도록 등록 하는 클라이언트 앱 (지정 된 장치와 무관)의 id입니다. Android에서 응용 프로그램 ID는 `com.xamarin.gcmexample`와 같은 **Androidmanifest**에 기록 된 패키지 이름입니다.

[Google Cloud Messaging 설정](#settingup) (이 가이드의 뒷부분)에서는 프로젝트를 만들고 이러한 자격 증명을 생성 하는 방법에 대 한 자세한 지침을 제공 합니다.

다음 섹션에서는 클라이언트 앱이 GCM을 통해 앱 서버와 통신할 때 이러한 자격 증명을 사용 하는 방법을 설명 합니다.

### <a name="registration-with-gcm"></a>GCM을 사용한 등록

장치에 설치 된 클라이언트 앱은 먼저 GCM에 등록 해야 메시지를 받을 수 있습니다. 클라이언트 앱은 다음 다이어그램에 표시 된 등록 단계를 완료 해야 합니다.

[![앱 등록 단계](google-cloud-messaging-images/02-app-registration-sml.png)](google-cloud-messaging-images/02-app-registration.png#lightbox)

1. 클라이언트 앱은 GCM에 연결 하 여 보낸 사람 ID를 GCM에 전달 하는 등록 토큰을 가져옵니다.

2. GCM은 클라이언트 앱에 등록 토큰을 반환 합니다.

3. 클라이언트 앱이 등록 토큰을 앱 서버에 전달 합니다.

앱 서버는 나중에 클라이언트 앱과 통신 하기 위해 등록 토큰을 캐시 합니다. 필요에 따라 앱 서버에서 다시 클라이언트 앱에 승인을 보내 등록 토큰을 받았음을 나타낼 수 있습니다. 이 핸드셰이크를 수행한 후 클라이언트 앱은 앱 서버에서 메시지를 수신 하거나 메시지를 보낼 수 있습니다.

클라이언트 앱이 더 이상 앱 서버에서 메시지를 수신 하지 않을 때 앱 서버에 등록 토큰을 삭제 하는 요청을 보낼 수 있습니다. 클라이언트 앱이 토픽 메시지를 수신 하는 경우 (이 문서의 뒷부분에서 설명) 항목에서 구독을 취소할 수 있습니다.
클라이언트 앱이 장치에서 제거 되 면 GCM이이를 감지 하 고 등록 토큰을 삭제 하도록 앱 서버에 자동으로 알립니다.

Google의 [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 은 등록 프로세스에 대해 자세히 설명 합니다. 등록 취소 및 구독 취소에 대해 설명 하 고, 클라이언트 앱이 제거 될 때 등록 취소 프로세스를 설명 합니다.

### <a name="downstream-messaging"></a>다운스트림 메시징

앱 서버는 클라이언트 앱에 다운스트림 메시지를 보낼 때 다음 다이어그램에 설명 된 단계를 따릅니다.

[다운스트림 메시징 저장소 및 전달 다이어그램![](google-cloud-messaging-images/03-downstream-sml.png)](google-cloud-messaging-images/03-downstream.png#lightbox)

1. 앱 서버는 GCM로 메시지를 보냅니다.

2. 클라이언트 장치를 사용할 수 없는 경우 GCM 서버는 나중에 전송할 수 있도록 메시지를 큐에 저장 합니다.

3. 클라이언트 장치를 사용할 수 있는 경우 GCM은 해당 장치의 클라이언트 앱에 메시지를 보냅니다.

4. 클라이언트 앱은 GCM에서 메시지를 수신 하 고 그에 따라 처리 합니다. 예를 들어 메시지가 원격 알림과 같은 경우 사용자에 게 표시 됩니다.

이 메시징 시나리오에서 (앱 서버는 단일 클라이언트 앱에 메시지를 보내는 경우) 메시지의 길이는 최대 4kB가 될 수 있습니다.

Android에서 다운스트림 GCM 메시지를 수신 하는 방법에 대 한 자세한 내용 (코드 샘플 포함)은 [원격 알림](~/android/data-cloud/google-messaging/remote-notifications-with-gcm.md)을 참조 하세요.

#### <a name="topic-messaging"></a>토픽 메시지

*토픽 메시징은* 앱 서버에서 토픽을 구독 하는 여러 클라이언트 앱 장치 (예: 날씨 예보)에 단일 메시지를 보내는 다운스트림 메시지 유형입니다. 토픽 메시지의 길이는 최대 2KB 이며 토픽 메시징은 앱 당 최대 100만 개의 구독을 지원 합니다. GCM이 토픽 메시징에만 사용 되는 경우 클라이언트 앱은 앱 서버에 등록 토큰을 보낼 필요가 없습니다. Google의 [토픽 구현 메시징은](https://developers.google.com/cloud-messaging/topic-messaging) 앱 서버에서 특정 토픽을 구독 하는 여러 장치에 메시지를 보내는 방법을 설명 합니다.

#### <a name="group-messaging"></a>그룹 메시징

*그룹 메시징은* 앱 서버에서 단일 메시지를 그룹에 속하는 여러 클라이언트 앱 장치 (예: 단일 사용자에 게 속한 장치 그룹)에 보내는 다운스트림 메시지 유형입니다. 그룹 메시지는 iOS 장치에 대 한 최대 2KB의 길이 이며 Android 장치에서는 최대 4KB입니다. 그룹은 최대 20 개의 구성원으로 제한 됩니다. Google의 [장치 그룹 메시징은](https://developers.google.com/cloud-messaging/notifications) 앱 서버가 그룹에 속한 장치에서 실행 되는 여러 클라이언트 앱 인스턴스에 단일 메시지를 보내는 방법을 설명 합니다.

### <a name="upstream-messaging"></a>업스트림 메시징

클라이언트 앱이 [Xmpp](https://developers.google.com/cloud-messaging/ccs)를 지 원하는 서버에 연결 하는 경우 다음 다이어그램에 설명 된 대로 메시지를 앱 서버에 다시 보낼 수 있습니다.

[![업스트림 메시징 다이어그램](google-cloud-messaging-images/04-upstream-sml.png)](google-cloud-messaging-images/04-upstream.png#lightbox)

1. 클라이언트 앱은 GCM XMPP 연결 서버에 메시지를 보냅니다.

2. 앱 서버의 연결이 끊어지면 GCM 서버는 나중에 전달할 수 있도록 메시지를 큐에 저장 합니다.

3. 앱 서버를 다시 연결할 때 GCM은 메시지를 앱 서버에 전달 합니다.

4. 앱 서버는 메시지를 구문 분석 하 여 클라이언트 앱의 id를 확인 한 다음 "ack"를 GCM에 전송 하 여 메시지 수신을 승인 합니다.

5. 앱 서버는 메시지를 처리 합니다.

Google의 [업스트림 메시지](https://developers.google.com/cloud-messaging/ccs#upstream) 는 JSON으로 인코딩된 메시지를 구조화 하 고 GOOGLE의 Xmpp 기반 클라우드 연결 서버를 실행 하는 앱 서버에 보내는 방법을 설명 합니다.

<a name="settingup" />

## <a name="setting-up-google-cloud-messaging"></a>Google Cloud Messaging 설정

앱에서 GCM 서비스를 사용 하려면 먼저 Google의 GCM 서버에 액세스 하기 위한 자격 증명을 얻어야 합니다. 다음 섹션에서는이 프로세스를 완료 하는 데 필요한 단계에 대해 설명 합니다.

### <a name="enable-google-services-for-your-app"></a>앱에 Google 서비스를 사용 하도록 설정

1. Google 계정 (예: gmail 주소)으로 [Google 개발자 콘솔](https://developers.google.com/mobile/add?platform=android) 에 로그인 하 고 새 프로젝트를 만듭니다. 기존 프로젝트가 있는 경우 GCM을 사용 하도록 설정할 프로젝트를 선택 합니다. 다음 예제에서는 **XamarinGCM** 이라는 새 프로젝트를 만듭니다.

    [XamarinGCM 프로젝트를 만드는![](google-cloud-messaging-images/05-create-gcm-app-sml.png)](google-cloud-messaging-images/05-create-gcm-app.png#lightbox)

2. 다음으로 앱에 대 한 패키지 이름 (이 예제에서는 패키지 이름은 **.com. xamarin.ios**)을 입력 하 고 **계속을 클릭 하 여 서비스를 선택 하 고 구성**합니다.

    [패키지 이름을 입력![](google-cloud-messaging-images/06-package-name-sml.png)](google-cloud-messaging-images/06-package-name.png#lightbox)

    이 패키지 이름은 앱에 대 한 응용 프로그램 ID 이기도 합니다.

3. **서비스 선택 및 구성** 섹션에는 앱에 추가할 수 있는 Google 서비스가 나열 됩니다. **클라우드 메시징**을 클릭 합니다.

    [클라우드 메시징![선택](google-cloud-messaging-images/07-choose-gcm-service-sml.png)](google-cloud-messaging-images/07-choose-gcm-service.png#lightbox)

4. 다음으로 **GOOGLE CLOUD MESSAGING 사용**을 클릭 합니다.

    [Google Cloud Messaging 사용![](google-cloud-messaging-images/08-enable-gcm-sml.png)](google-cloud-messaging-images/08-enable-gcm.png#lightbox)

5. 앱에 대 한 **서버 API 키** 및 **보낸 사람 ID** 가 생성 됩니다. 이러한 값을 기록 하 고 **닫기**를 클릭 합니다.

    [표시 된![서버 API 키 및 보낸 사람 ID](google-cloud-messaging-images/09-get-api-key-and-id-sml.png)](google-cloud-messaging-images/09-get-api-key-and-id.png#lightbox)

    API 키를 보호 하는 것은 공개적으로 사용 하기에 적합 하지 &ndash;. API 키가 손상 되 면 권한이 없는 서버에서 클라이언트 응용 프로그램에 메시지를 게시할 수 있습니다.
    [Api 키를 안전 하 게 사용 하](https://support.google.com/cloud/answer/6310037?hl=en) 는 모범 사례는 api 키를 보호 하는 데 유용한 지침을 제공 합니다.

### <a name="view-your-project-settings"></a>프로젝트 설정 보기

언제 든 지 [Google Cloud Console](https://console.cloud.google.com/) 에 로그인 하 고 프로젝트를 선택 하 여 프로젝트 설정을 볼 수 있습니다. 예를 들어 페이지 맨 위에 있는 풀 다운 메뉴에서 프로젝트를 선택 하 여 **보낸 사람 ID** 를 볼 수 있습니다 (이 예제에서는 프로젝트를 **XamarinGCM**라고 함). 보낸 사람 ID는 다음 스크린샷에 표시 된 것과 같은 프로젝트 번호입니다. 발신자 ID는 **9349932736**입니다.

[보낸 사람 ID를 볼![](google-cloud-messaging-images/10-view-server-id-sml.png)](google-cloud-messaging-images/10-view-server-id.png#lightbox)

**Api 키**를 보려면 **api 관리자** 를 클릭 한 다음 **자격 증명**을 클릭 합니다.

[API 키![보기](google-cloud-messaging-images/11-view-credentials-sml.png)](google-cloud-messaging-images/11-view-credentials.png#lightbox)

## <a name="for-further-reading"></a>추가 정보

- Google의 [클라이언트 앱 등록](https://developers.google.com/cloud-messaging/registration) 은 클라이언트 등록 프로세스에 대해 자세히 설명 하 고 자동 재시도를 구성 하 고 등록 상태를 동기화 상태로 유지 하는 방법에 대 한 정보를 제공 합니다.

- [Rfc 6120](https://tools.ietf.org/html/rfc6120) 및 [RFC 6121](https://tools.ietf.org/html/rfc6121) 은 xmpp (확장할 수 있는 메시징 및 현재 상태 프로토콜)를 설명 하 고 정의 합니다.

## <a name="summary"></a>요약

이 문서에서는 GCM (Google Cloud Messaging의 개요를 제공 했습니다. 앱 서버와 클라이언트 앱 간의 메시징을 식별 하 고 권한을 부여 하는 데 사용 되는 다양 한 자격 증명에 대해 설명 했습니다. 가장 일반적인 메시징 시나리오를 설명 하 고 gcm 서비스를 사용 하도록 GCM에 앱을 등록 하는 단계에 대해 자세히 설명 합니다.

## <a name="related-links"></a>관련 링크

- [클라우드 메시징](https://developers.google.com/cloud-messaging/)
