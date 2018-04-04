---
title: 문제 해결
description: 이 문서는 tvOS 10 Xamarin.tvOS 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다.
ms.prod: xamarin
ms.assetid: EA5564BB-C415-49A2-B70C-3DBF5E0F3FAB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8875e658ead17820655a2401079627875c14958b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting"></a>문제 해결

_이 문서는 tvOS 10 Xamarin.tvOS 앱에서 사용 하기 위한 몇 가지 문제 해결 팁을 제공 합니다._

다음 섹션에서는 tvOS 10 Xamarin.tvOS 및 이러한 문제와 솔루션 함께 사용할 때 발생할 수 있는 몇 가지 알려진된 문제를 나열 합니다.

- [App Store](#App-Store)
- [이진 호환성](#Binary-Compatibility)
- [CFNetwork HTTP Protocol](#CFNetwork-HTTP-Protocol)
- [CloudKit](#CloudKit)
- [CoreImage](#CoreImage)
- [NSUserActivity](#NSUserActivity)
- [UIKit](#UIKit)

<a name="App-Store" />

## <a name="app-store"></a>앱 스토어

알려진 문제:

 - 앱에서 바로 구매 샌드박스 환경에서 테스트 하는 경우 인증 대화 상자가 두 번 나타날 수 있습니다.
 - 앱에서 바로 구매 샌드박스 환경에서 호스트 된 콘텐츠를 테스트할 때는 콘텐츠 다운로드 완료 될 때까지 응용 프로그램은 전경으로 상태로 전환 될 때마다 암호 대화 상자가 표시 됩니다.

<a name="Binary-Compatibility" />

## <a name="binary-compatibility"></a>이진 호환성

알려진 문제:

 - 호출 `NSObject.ValueForKey` 됩니다는 `null` 키 예외가 발생 합니다.
 - 호출할 때 글꼴 이름으로 참조 `UIFont.WithName` 충돌이 발생 합니다.
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

`CIImageProcessor` API 이제 임의의 입력된 이미지 수를 지원 합니다. `CIImageProcessor` 10 tvOS 베타 1에에서 포함 된 API 제거 됩니다.

<a name="NSUserActivity" />

## <a name="nsuseractivity"></a>NSUserActivity

핸드 오프 작업 후의 `UserInfo` 속성은 `NSUserActivity` 개체는 비어 있을 수 있습니다. 명시적으로 호출 `BecomeCurrent` NSUserActivity' 현재 문제를 해결 하는 개체입니다.

<a name="UIKit" />

## <a name="uikit"></a>UIKit

알려진 문제:

 - 배경 모양을 변경 `UINavigationBar`, `UITabBar` 또는 `UIToolBar` 새 모양을 해결 하려면 레이아웃 단계에 발생할 수 있습니다. 내부에 이러한 모양을 수정 하는 한 `LayoutSubviews`, `UpdateConstraints`, `WillLayoutSubviews` 또는 `DidUpdateSubviews` 레이아웃 무한 루프에서 이벤트가 발생할 수 있습니다.
 - TvOS 10, 호출에 `RemoveGestureRecognizer` 의 메서드는 `UIView` 개체 모든 진행 중인 제스처 인식기를 명시적으로 취소 합니다.
 - 발표 된 컨트롤러 보기 이제 상태 표시줄의 모양을 영향을 줄 수 있습니다.
 - 10 tvOS 위해서 호출 하려면 개발자 `base.AwakeFromNib` 서브클래싱 할 경우 `UIViewController` 재정의 `AwakeFromNib` 메서드.
 - 사용자 지정을 사용 하 여 앱 `UIView` 재정의 하는 서브 클래스 `LayoutSubviews` 호출 하기 전에 레이아웃 작성 `base.LayoutSubviews` tvOS 10에에서는 레이아웃 무한 루프를 트리거할 수 있습니다.
 - 특정 방향 또는 flippable 이미지 자산은에 할당 된 경우를 대칭 이동 안 `UIButton` 개체입니다.





## <a name="related-links"></a>관련 링크

- [tvOS 샘플](https://developer.xamarin.com/samples/tvos/all/)
- [TvOS 10의에서 새로운 기능](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewinTVOS/Articles/tvOS10.html#//apple_ref/doc/uid/TP40017259-SW1)
