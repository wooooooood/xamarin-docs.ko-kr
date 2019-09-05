---
title: watchOS 3 문제 해결
description: 이 문서에서는 Xamarin에서 watchOS 3으로 작업할 때 유용한 몇 가지 문제 해결 팁을 제공 합니다. 팁은 활동, Apple Pay, 백그라운드 새로 고침, NSURLConnection, 개인 정보 보호 등에 관련 됩니다.
ms.prod: xamarin
ms.assetid: 5911D898-0E23-40CC-9F3C-5F61B4D50ADC
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: f10fb237bca92f49ac77657778ada8a47ed69c49
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292180"
---
# <a name="watchos-3-troubleshooting"></a>watchOS 3 문제 해결

_이 문서에서는 Xamarin Apple Watch 앱에서 watchOS 3으로 작업 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

이 페이지에는 Xamarin과 watchOS 3을 사용 하는 경우 발생할 수 있는 몇 가지 알려진 문제와 해당 문제에 대 한 솔루션이 나와 있습니다.

## <a name="activities"></a>활동

활동 공유가 제대로 작동 하려면 페어링된 모든 Apple Watch에서 watchOS 3을 실행 해야 합니다.

알려진 문제:

- 활동 공유 알림에 대 한 회신이 때때로 실패할 수 있습니다.
- 메시지와 함께 작업 공유 알림에 회신 하지 못할 수 있습니다.
- 활동 공유 알림 메시지 위에 있는 상황에 맞는 텍스트는 올바르지 않습니다.

## <a name="apple-pay"></a>Apple Pay

알려진 문제:

- Apple Pay에서 새 지불을 위해 잘못 된 만료 날짜 또는 CW 코드를 입력 하면 **다음** 에 도달할 때 실행 중인 프로세스의 작동이 중단 됩니다.
- 앱 내 Apple Pay에서 PIN 번호를 필요로 하는 구매는 충돌할 수 있습니다.

## <a name="auto-mac-unlock"></a>Mac 자동 잠금 해제

WatchOS 3 beta 2 이상 및 macOS Sierra beta 2 이상을 사용 하 여 사용자의 iCloud 계정에 대해 2 단계 인증을 사용 하도록 설정한 경우 해당 Apple Watch를 사용 하 여 해당 Mac을 자동으로 잠금 해제할 수 있습니다.

## <a name="background-refresh"></a>백그라운드 새로 고침

시스템 리소스를 위반 하면 다음 예외 코드로 인해 watchOS 3 앱이 충돌 합니다.

- **0xc51bad01** -앱이 너무 많은 CPU 시간을 사용 했습니다.
- **0xc51bad02** -앱이 너무 많은 벽 시간을 사용 했습니다.
- **0xc51bad03** -앱에 현재 작업을 완료 하는 데 충분 한 런타임이 없습니다.

## <a name="clock"></a>Clock

새로 설치 된 Apple Watch 앱의 복잡 한 것이 빈 상태로 표시 될 수 있습니다. 이 문제를 해결 하려면 Apple Watch를 다시 부팅 하십시오.

## <a name="connectivity"></a>연결

알려진 문제:

- watchOS는 Apple Watch에서 보호 된 사용자 데이터에 대 한 액세스 권한을 사용자에 게 묻지 않습니다. Watch 앱에서 데이터를 사용 하기 전에 iPhone 앱에 대 한 액세스 권한을 부여 합니다.
- Apple Watch는 모든 WatchConnectivity 전송에 실패 한 상태를 입력 하 고, Apple Watch를 다시 부팅 하 여 수정할 수 있습니다.

## <a name="notifications"></a>알림

미디어 첨부 파일이 너무 크면 사용자의 iPhone에는 표시 되지만 Apple Watch에는 표시 되지 않습니다.

## <a name="nsurlconnection"></a>NSURLConnection

이전 `NSURLConnection` TLS 프로토콜을 사용 하는 모든 연결이 실패 합니다. 모든 SSL/TLS 연결의 경우 이제 RC4 대칭 암호화가 기본적으로 사용 하지 않도록 설정 됩니다. 또한 보안 전송 API는 더 이상 SSLv3을 지원 하지 않으며, 가능한 한 빨리 SHA-1 및 3DES 암호화를 사용 하 여 앱을 중지 하는 것이 좋습니다.

WatchOS 3에서 SSL/TLS 연결 보안은 Apple에 의해 엄격히 적용 됩니다. 영향을 받는 서비스와 앱은 최신 TLS 프로토콜 버전을 사용 하도록 웹 서버를 업데이트 해야 합니다.

## <a name="nsurlsession"></a>NSURLSession

WatchOS 3부터 `NSMutableURLRequest` 클래스의 속성 `HTTPBodyStream` 은 `NSURLSession` 이제이 요구 사항을 엄격 하 게 적용 하기 때문 `NSURLConnection` 에 아직 열지 않은 스트림으로 설정 되어야 합니다.

## <a name="privacy"></a>개인 정보 취급 방침

알려진 문제:

Url로 `https://` 작업할 때 `NSURLConnection` 및는 TLS 핸드셰이크 중에 RC4 암호 그룹을 더 이상 지원 하지 않습니다. `NSURLSession` 다음 오류 코드 중 하나가 생성 될 수 있습니다.

- **-1200 또는-98** - `NSURLErrorSecurityConnectionFailed` 및 securetransport 오류
- **-1200 [3:-9824]** -Http 로드에 실패 했습니다.
- **-**  -  1200`NSURLConnection` 이 완료 되었으나 오류가 발생 했습니다.

WatchOS 3에서 SSL/TLS 연결 보안은 Apple에 의해 엄격히 적용 됩니다. 영향을 받는 서비스와 앱은 최신 TLS 프로토콜 버전을 사용 하도록 웹 서버를 업데이트 해야 합니다. 자세한 내용은 위의 [NSURLConnection](#nsurlconnection) 를 참조 하세요.

## <a name="snapshots"></a>스냅숏

새 `HandelBackgroundTask` API를 채택 하지 않은 WatchKit apps는 watchOS 3에서 더 이상 정기적으로 업데이트를 받지 않습니다. 

## <a name="watchkit"></a>WatchKit

SpriteKit 및 SceneKit 장면은 앱이 watchOS Dock에서 배경으로 들어가면 일시 중지 됩니다.

## <a name="related-links"></a>관련 링크

- [watchOS 샘플](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+watchOS)
- [WatchOS 3의 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewInwatchOS/Articles/watchOS3.html#//apple_ref/doc/uid/TP40017085-SW1)
