---
title: 3 watchOS 문제 해결
description: 이 문서에서는 watchOS 3 Xamarin에서 작업할 때 유용한 몇 가지 문제 해결 팁을 제공 합니다. 팁 활동, Apple Pay, 백그라운드 새로 고침, NSURLConnection, 개인 정보 보호 및 더와 관련이 있습니다.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 0aca2c96533e17e4aeb2f57d38a87d39f700fb45
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791030"
---
# <a name="watchos-3-troubleshooting"></a>3 watchOS 문제 해결

_이 문서에서는 watchOS 3 Xamarin Apple Watch 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

이 페이지는 watchOS 3을 사용 하 여 Xamarin 및 해당 문제를 해결 하는 경우 발생할 수 있는 몇 가지 알려진된 문제를 나열 합니다.

## <a name="activities"></a>활동

제대로 작동 하려면 공유 하는 활동에 대 한 모든 쌍을 이루는 Apple 조사식 3 watchOS 실행 되어야 합니다.

알려진 문제:

- 경우에 따라 활동 공유 알림에 회신 하는 작동 하지 않습니다.
- 활동 공유 알림 메시지에 회신 실패할 수 있습니다.
- 작업 공유 알림 메시지는 위에 상황별 텍스트 올바르지 않을 수 있습니다.

## <a name="apple-pay"></a>Apple Pay

알려진 문제:

- 잘못 된 만료 날짜 또는 CW 코드 입력 된 값에 대 한 새 지불 특별히 주의 Apple Pay에 적중 하는 경우 **다음** 실행 중인 프로세스의 작동이 중단 됩니다.
- PIN 번호를 필요로 하는 앱에서 Apple Pay 구매 충돌할 수 있습니다.

## <a name="auto-mac-unlock"></a>Mac 자동 잠금 해제

사용자의 iCloud 계정과에서 2 단계 인증을 사용 하는 경우 3 watchOS 베타 2 (또는 그 이상), macOS 시에라 베타 2 (이상)를 사용 하면 사용 하 여 자신의 Apple Watch 자동으로 해당 Mac. 잠금 해제

## <a name="background-refresh"></a>백그라운드 새로 고침

시스템 리소스를 위반 합니다. 다음 예외 코드를 갖는 watchOS 3 응용 프로그램 크래시 발생 합니다.

- **0xc51bad01** -앱 너무 많은 CPU 시간을 사용한 합니다.
- **0xc51bad02** -앱 벽 시간이 너무 많이 소비 합니다.
- **0xc51bad03** -응용 프로그램의 현재 작업을 완료 하기에 충분 한 런타임이 없습니다.

## <a name="clock"></a>Clock

새로 설치 된 Apple Watch 응용 프로그램에서 문제가 빈 값으로 표시 될 수 있습니다. Apple Watch이 문제를 해결 하려면 다시 부팅 합니다.

## <a name="connectivity"></a>연결

알려진 문제:

- watchOS는 Apple Watch 보호 된 사용자 데이터에 대 한 액세스 권한을 사용자에 게 확인 합니다. Watch 앱에서 데이터를 사용 하기 전에 iPhone 앱에서 액세스를 부여 합니다.
- Apple Watch 모든 WatchConnectivity 전송이 실패 상태를 입력할 수 Apple Watch 문제를 해결 하려면 다시 부팅 합니다.

## <a name="notifications"></a>알림

미디어 첨부 파일 너무 클 경우에 사용자의 iPhone 있는데 해당 Apple Watch 하지 표시 됩니다.

## <a name="nsurlconnection"></a>NSURLConnection

모든 `NSURLConnection` 이전 TLS 프로토콜을 사용 하 여 연결 실패 합니다. 모든 SSL/TLS 연결에 대 한 RC4 대칭 암호화는 이제 기본적으로 비활성화 됩니다. 또한 보안 전송 API는 더 이상 SSLv3 지원 있고 응용 프로그램 s h A-1과 3DES 암호화를 사용 하 여 가능한 한 빨리 중지 하는 것이 좋습니다.

3 watchOS 기준으로 SSL/TLS 연결 보안 Apple에서 엄격 하 게 적용 되 고 됩니다. 영향을 받는 서비스 및 앱에 최신 TLS 프로토콜 버전을 사용 하도록 웹 서버를 업데이트 해야 합니다.

## <a name="nsurlsession"></a>NSURLSession

WatchOS 3, 현재는 `HTTPBodyStream` 속성의는 `NSMutableURLRequest` 이후 열려 있지 않은 스트림에 클래스를 설정 해야 `NSURLConnection` 및 `NSURLSession` 이제이 요구 사항을 엄격 하 게 적용 합니다.

## <a name="privacy"></a>개인 정보 보호

알려진 문제:

작업할 때 `https://` Url 모두 `NSURLSession` 및 `NSURLConnection` 더 이상 TLS 핸드셰이크 중 RC4 암호 그룹을 지원 합니다. 다음 오류 코드 중 하나를 생성 될 수 있습니다.

- **-1200 또는-98** - `NSURLErrorSecurityConnectionFailed` 및 SecureTransport 오류입니다.
- **[3:-9824]-1200** -http 로드 실패 합니다.
- **-1200**  -  `NSURLConnection` 완료 되었지만 오류가 발생 합니다.

3 watchOS 기준으로 SSL/TLS 연결 보안 Apple에서 엄격 하 게 적용 되 고 됩니다. 영향을 받는 서비스 및 앱에 최신 TLS 프로토콜 버전을 사용 하도록 웹 서버를 업데이트 해야 합니다. 참조 [NSURLConnection](#NSURLConnection) 위의 자세한 정보에 대 한 합니다.

## <a name="snapshots"></a>스냅숏

새 채택한 WatchKit 앱 `HandelBackgroundTask` API watchOS 3에서에서의 정기적 업데이트를 더 이상 받지 것입니다. 

## <a name="watchkit"></a>WatchKit

응용 프로그램 watchOS 도킹의에서 배경으로 들어갈 때 SpriteKit 및 SceneKit 장면 일시 중지 됩니다.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://developer.xamarin.com/samples/watchos/all/)
- [WatchOS 3의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
