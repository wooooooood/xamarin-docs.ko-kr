---
title: Xamarin.Mac - macOS Sierra Troubleshooting
description: 이 문서는 macOS Sierra Xamarin.Mac 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다. 팁은 Mac 앱 스토어, Apple Pay, 이진 호환성, CFNetwork, CloudKit, 등와 관련이 있습니다.
ms.prod: xamarin
ms.assetid: 323DD5EE-87CE-48E4-B234-1CF61B45A019
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 09/22/2016
ms.openlocfilehash: 322acff3279d0513266c7d9883726cac726334f7
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67830548"
---
# <a name="xamarinmac---macos-sierra-troubleshooting"></a>Xamarin.Mac - macOS Sierra Troubleshooting

_이 문서에서는 Xamarin.Mac 앱에 macOS Sierra 사용 하 여 작업에 대 한 몇 가지 문제 해결 팁을 제공 합니다._

다음 섹션에서는 Xamarin.mac 및 이러한 문제에 솔루션을 사용 하 여 macOS Sierra를 사용할 때 발생할 수 있는 몇 가지 알려진된 문제를 나열 합니다.

- [App Store](#App-Store)
- [Apple Pay](#Apple-Pay)
- [이진 호환성](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [Core 이미지](#CoreImage)
- [알림](#Notifications)
- [NSUserActivity](#NSUserActivity)
- [Safari](#Safari)

<a name="App-Store" />

## <a name="app-store"></a>App Store

알려진 문제:

- 샌드박스 환경에서 앱 내 구매를 테스트 하는 경우 인증 대화 상자는 두 번 나타날 수 있습니다.
- 샌드박스 환경에서 호스트 된 콘텐츠를 사용 하 여 앱 내 구매를 테스트할 때 콘텐츠 다운로드가 완료 될 때까지 앱 전경으로 상태로 전환 될 때마다 암호 대화 상자가 표시 됩니다.

<a name="Apple-Pay" />

## <a name="apple-pay"></a>Apple Pay

Apple Pay까지 새 결제 카드를 추가 하는 경우를 잘못 된 만료 날짜 또는 보안 코드 (CW)를 입력 하면 카드 프로 비전 프로세스를 종료 됩니다.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>이진 호환성

알려진 문제:

- 호출 `NSObject.ValueForKey` 는 `null` 키 예외가 발생 합니다.
- 둘 다 `NSURLSession` 하 고 `NSURLConnection` 에 대 한 TLS 핸드셰이크 중 더 이상 RC4 암호 그룹 `http://` Url입니다.
- 앱을 뷰의 요소의 기 하 도형 중 하나를 수정 하더라도 중지 될 수는 `ViewWillLayoutSubviews` 또는 `LayoutSubviews` 메서드.
- 모든 SSL/TLS 연결에 대해 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API SSLv3을 더 이상 지원 및 앱 sha-1 및 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.

<a name="CFNetwork-HTTP-Protocol" />

## <a name="cfnetwork-http-protocol"></a>CFNetwork HTTP Protocol

`HTTPBodyStream` 의 속성을 `NSMutableURLRequest` 이후 열려 있지 않은 스트림에 클래스를 설정 해야 합니다 `NSURLConnection` 및 `NSURLSession` 이제이 요구 사항을 엄격 하 게 적용 합니다.

<a name="CloudKit" />

## <a name="cloudkit"></a>CloudKit

장기 실행 작업을 반환 하는 한 _"파일을 저장할 수 있는 권한이 없는 있습니다."_ 오류가 발생 했습니다.

<a name="CoreImage" />

## <a name="core-image"></a>Core 이미지

`CIImageProcessor` API는 이제 임의의 입력된 이미지 수를 지원 합니다. `CIImageProcessor` MacOS Sierra 베타 1에에서 포함 된 API 제거 됩니다.

<a name="Notifications" />

## <a name="notifications"></a>알림

알림 콘텐츠 확장을 사용 하는 경우 뷰 컨트롤러 제대로 해제 되지 않습니다 및 확장 메모리 제한에 도달 하는 경우 충돌이 발생할 수 있습니다.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

핸드 오프 작업 후 합니다 `UserInfo` 의 속성을 `NSUserActivity` 개체는 비어 있을 수 있습니다. 명시적으로 호출 `BecomeCurrent` `NSUserActivity` 현재 문제를 해결 하는 개체입니다.

<a name="Safari" />

## <a name="safari"></a>Safari

WebGeolocation 필요한 보안 (`https://`) iOS 10과 macOS Sierra 위치 데이터의 악의적인 사용을 방지 하기 위해 둘 다에서 작동 하는 URL입니다.







## <a name="related-links"></a>관련 링크

- [Mac 샘플](https://developer.xamarin.com/samples/mac/)
- [OS X 10.12의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/MacOSX/WhatsNewInOSX/Articles/OSXv10.html#//apple_ref/doc/uid/TP40017145-SW1)
