---
title: 문제 해결
description: 이 문서에서는 macOS 시에라 Xamarin.Mac 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/22/2016
ms.openlocfilehash: 7ea4ec48399b42ce69b0346b1a88a1d9fb9fbf6e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>문제 해결

_이 문서에서는 macOS 시에라 Xamarin.Mac 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

다음 섹션에서는 macOS 시에라 Xamarin.mac 및 이러한 문제와 솔루션 함께 사용할 때 발생할 수 있는 몇 가지 알려진된 문제가 나열:

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [이진 호환성](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [알림](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>앱 스토어

알려진 문제:

- 앱에서 바로 구매 샌드박스 환경에서 테스트 하는 경우 인증 대화 상자가 두 번 나타날 수 있습니다.
- 앱에서 바로 구매 샌드박스 환경에서 호스트 된 콘텐츠를 테스트할 때는 콘텐츠 다운로드 완료 될 때까지 응용 프로그램은 전경으로 상태로 전환 될 때마다 암호 대화 상자가 표시 됩니다.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Apple Pay를 새 결제 카드를 추가 하는 잘못 된 만료 날짜 또는 보안 코드 (CW)를 입력 하는 경우 프로 비전 프로세스 카드 종료 됩니다.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>이진 호환성

알려진 문제:

- 호출 `NSObject.ValueForKey` 됩니다는 `null` 키 예외가 발생 합니다.
- 둘 다 `NSURLSession` 및 NSURLConnection` no longer RC4 cipher suites during the TLS handshake for `http://' Url입니다.
- 슈퍼 뷰의 geometry를 수정 하는 경우 앱이 중지 될 수는 `ViewWillLayoutSubviews` 또는 `LayoutSubviews` 메서드.
- 모든 SSL/TLS 연결에 대 한 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API는 더 이상 SSLv3 지원 있고 응용 프로그램 s h A-1과 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP 프로토콜

`HTTPBodyStream` 속성은 `NSMutableURLRequest` 이후 열려 있지 않은 스트림에 클래스를 설정 해야 `NSURLConnection` 및 `NSURLSession` 이제이 요구 사항을 엄격 하 게 적용 합니다.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

장기 실행 작업을 반환 하는 한 _"않아도 파일을 저장할 수 있는 권한이 있습니다."_ 오류가 발생 했습니다.

<a name="CoreImage" />

## <a name="coreimage"></a>CoreImage

`CIImageProcessor` API 이제 임의의 입력된 이미지 수를 지원 합니다. `CIImageProcessor` MacOS 시에라 베타 1에에서 포함 된 API 제거 됩니다.

<a name="Notifications" />

## <a name="notifications"></a>알림

알림 콘텐츠 Extensions를 사용할 때는 컨트롤러 보기 올바르게 해제 되지 않습니다 및 확장 메모리 제한에 도달 하는 경우 충돌이 발생할 수 있습니다.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

핸드 오프 작업 후의 `UserInfo` 속성은 `NSUserActivity` 개체는 비어 있을 수 있습니다. 명시적으로 호출 `BecomeCurrent` `NSUserActivity` 현재 문제를 해결 하는 개체입니다.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation 필요는 보안 (`https://`) URL iOS 10와 macOS 위치 데이터를 악의적으로 사용 하지 않으려면 시에라 모두에서 작동 하도록 합니다.







## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
