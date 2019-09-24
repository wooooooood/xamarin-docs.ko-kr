---
title: iOS 플랫폼 기능 개요
description: 이 문서는 다양 한 iOS 버전 및 기타 iOS 플랫폼 기능에 도입 된 기능을 설명 하는 다양 한 가이드에 연결 됩니다.
ms.prod: xamarin
ms.assetid: 9F6A27E5-8A87-ADE2-D1EF-5684E7B8C999
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: c6385ff193c54fdab8f252c757cad810751b3f08
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206296"
---
# <a name="ios-platform-features-overview"></a>iOS 플랫폼 기능 개요

이 페이지에는 최신 iOS 릴리스와 Xamarin.ios를 사용 하 여 액세스할 수 있는 Apple 프레임 워크 중 일부를 강조 표시 하는 것도 나열 되어 있습니다.

## <a name="ios-releases"></a>iOS 릴리스

|  |  |
|-------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [IOS 13 소개](~/ios/platform/ios13/index.md) | 이 문서에서는 Xamarin.ios 13에 대해 설명 합니다.|
| [iOS 12 소개](~/ios/platform/introduction-to-ios12/index.md) | 이 문서에서는 Xamarin.ios 응용 프로그램을 빌드할 때 사용할 수 있는 iOS 12 기능에 대해 설명 합니다.|
| [iOS 11 소개](~/ios/platform/introduction-to-ios11/index.md) | 이 문서에서는 ARKit, Core ML, Core NFC, 끌어서 놓기, MapKit, PDFKit, SiriKit 및 비전과 같이 iOS 11 및 Xcode 9의 새로운 기능 및 업데이트 된 기능에 대해 설명 합니다. Xamarin.ios에서 이러한 기능을 사용 하는 방법을 설명 하는 가이드로 연결 됩니다. |
| [iOS 10 소개](~/ios/platform/introduction-to-ios10/index.md) | iOS 10에는 새로운 기능과 기능으로 앱을 개발할 수 있는 몇 가지 새로운 Api 및 서비스가 포함 되어 있습니다. IOS 10을 사용 하는 앱에는 지도, 메시지, 전화, Siri 확장 등의 새로운 기능이 있습니다. 이 섹션에서는 xamarin.ios 앱에서 이러한 기능을 활용 하기 위한에서는를 보여 줍니다. |
| [iOS 9 소개](~/ios/platform/introduction-to-ios9/index.md)   | 이 섹션에서는 iOS 8에서 업그레이드 하는 경우 iOS 9에서 변경 된 내용과 Xamarin.ios 앱에서 이러한 기능을 사용 하는 방법에 대해 정의 합니다. |
| [iOS 8 소개](~/ios/platform/introduction-to-ios8.md)         | ios 8은 iOS 7에서 운영 체제에 대 한 많은 변경 내용을 만들었습니다. 여기서는 무엇이 무엇이 고 사용 하는 방법을 보여 줍니다. |
| [iOS 7 소개](~/ios/platform/introduction-to-ios7/index.md)   | 보기 컨트롤러 전환을 비롯 하 여 iOS 7에 도입 된 주요 새로운 Api에 대 한 정보, Uiview Dynamics 및 텍스트 키트의 향상 된 기능 |
| [iOS 6 소개](~/ios/platform/introduction-to-ios6/index.md)   | 컬렉션 뷰, Pass Kit, Event Kit 및 소셜 프레임 워크를 포함 하 여 iOS 6에 도입 된 기능에 대 한 설명입니다. |

## <a name="apple-payiosplatformapple-paymd"></a>[Apple Pay](~/ios/platform/apple-pay.md)

Apple Pay은 iOS 8과 함께 도입 되어 사용자가 iOS 장치를 통해 음식, 엔터테인먼트 및 멤버 자격과 같은 물리적 상품에 대해 지불할 수 있습니다. IPhone 6 및 iPhone 6 Plus에서 사용할 수 있으며, 매장 내 구매를 위해 Apple Watch와 쌍으로 연결 될 수도 있습니다. IPhone에서 사용 되는 경우 사용자의 신용 카드 또는 직불 카드에 대 한 트랜잭션을 확인 하 고 권한을 부여 하는 방법으로 Touch ID를 사용 합니다.

## <a name="callkitiosplatformcallkitmd"></a>[CallKit](~/ios/platform/callkit.md)

IOS 10의 새로운 CallKit API는 VOIP 앱이 iPhone UI와 통합 되 게 하 고 최종 사용자에 게 친숙 한 인터페이스와 경험을 제공할 수 있는 방법을 제공 합니다. 이 API를 사용 하면 사용자가 iOS 장치의 잠금 화면에서 VOIP 호출을 확인 하 고 상호 작용 하 고 Phone 앱의 **즐겨찾기** 및 **최근** 보기를 사용 하 여 연락처를 관리할 수 있습니다.

## <a name="contacts-and-contactsuiiosplatformcontactsmd"></a>[연락처 및 ContactsUI](~/ios/platform/contacts.md)

Ios 9가 도입 되면서 Apple은 ios 8 및 이전 버전에서 사용 `Contacts` 하 `ContactsUI`는 기존 주소록 및 주소록 UI 프레임 워크를 대체 하는 두 가지 새로운 프레임 워크 및를 출시 했습니다.

## <a name="document-pickeriosplatformdocument-pickermd"></a>[문서 선택기](~/ios/platform/document-picker.md)

문서 선택기를 사용 하면 앱 간에 문서를 공유할 수 있습니다. 이러한 문서는 iCloud 또는 다른 앱의 디렉터리에 저장 될 수 있습니다. 문서는 사용자가 장치에 설치한 [문서 공급자 확장](~/ios/platform/extensions.md) 집합을 통해 공유 됩니다.

## <a name="eventkitiosplatformeventkitmd"></a>[EventKit](~/ios/platform/eventkit.md)

iOS에는 달력 응용 프로그램 및 미리 알림 응용 프로그램 이라는 두 가지 달력 관련 응용 프로그램이 있습니다. 달력 응용 프로그램에서 일정 데이터를 관리 하는 방법을 이해할 수 있을 정도로 간단 하지만 미리 알림 응용 프로그램은 명확 하지 않습니다. 알림을 사용 하는 경우, 완료 시기 등을 기준으로 미리 알림이 실제로 연결 된 날짜를 사용할 수 있습니다. 이와 같이 iOS는 일정 이벤트 또는 미리 알림이 일정 *데이터베이스*라는 한 위치에서 모든 달력 데이터를 저장 합니다.

## <a name="ios-extensionsiosplatformextensionsmd"></a>[iOS 확장](~/ios/platform/extensions.md)

Ios 8에 도입 된 확장은 사용자가 특수 `UIViewControllers` 입력 또는 기타 컨텍스트를 수행 하기 위해 요청 하는 사용자 지정 키보드 유형으로 **알림 센터**내에서와 같은 표준 컨텍스트 내에서 ios에 의해 제공 되는 특수화입니다. 확장에서 특수 효과 필터를 제공할 수 있는 사진을 편집 하는 것과 같습니다.

## <a name="graphics-and-animation-in-iosiosplatformgraphics-animation-iosindexmd"></a>[IOS의 그래픽 및 애니메이션](~/ios/platform/graphics-animation-ios/index.md)

IOS의 그래픽 및 애니메이션은 CoreImage, 핵심 그래픽 및 핵심 애니메이션과 같은 iOS의 핵심 그래픽 개념을 다룹니다.

## <a name="handoffiosplatformhandoffmd"></a>[Handoff](~/ios/platform/handoff.md)

Apple은 iOS 8 및 OS X Yosemite (10.10)에 전달 하 여 사용자가 장치 중 하나에서 시작 된 활동을 동일한 앱을 실행 하는 다른 장치 또는 동일한 활동을 지 원하는 다른 앱으로 전송 하는 일반적인 메커니즘을 제공 합니다.

## <a name="healthkitiosplatformhealthkitmd"></a>[HealthKit](~/ios/platform/healthkit.md)

상태 키트는 사용자의 상태 관련 정보에 대 한 보안 데이터 저장소를 제공 합니다. 상태 키트 앱은 사용자의 명시적 사용 권한을 사용 하 여이 데이터 저장소에 대 한 읽기 및 쓰기를 제공 하 고 관련 데이터가 추가 될 때 알림을 받을 수 있습니다. 앱에서 데이터를 제공 하거나, 사용자가 Apple의 제공 된 상태 앱을 사용 하 여 모든 데이터의 대시보드를 볼 수 있습니다.

## <a name="homekitiosplatformhomekitmd"></a>[HomeKit](~/ios/platform/homekit.md)

Apple은 iOS 8에서 HomeKit를 도입 하 여 사용자의 홈에서 홈 automation 장치를 검색 하 고이를 전달 하는 공통 프레임 워크를 제공 합니다. HomeKit는 장치를 구성 하 고 제어 하는 작업을 설정 하기 위한 공통 플랫폼을 제공 합니다.

## <a name="in-app-purchasingiosplatformin-app-purchasingindexmd"></a>[앱에서 바로 구매](~/ios/platform/in-app-purchasing/index.md)

iOS 응용 프로그램은 휴대폰 키트를 사용 하 여 디지털 제품 또는 서비스를 판매할 수 있습니다. 즉, apple의 서버와 통신 하 여 Apple ID를 통해 재무 트랜잭션을 수행 하는 iOS에서 제공 하는 Api 집합입니다. 보관 키트 Api는 주로 제품 정보 검색 및 트랜잭션 수행과 관련 된 것입니다. 사용자 인터페이스 구성 요소는 없습니다. 앱에서 바로 구매를 구현 하는 응용 프로그램은 사용자에 게 필요한 제품이 나 서비스를 제공 하는 사용자 지정 코드를 사용 하 여 사용자 인터페이스를 빌드하고 구매한 항목을 추적 해야 합니다.

## <a name="ios-gaming-apisiosplatformgamingindexmd"></a>[iOS 게임 Api](~/ios/platform/gaming/index.md)

Apple은 Xamarin.ios 앱에서 게임 그래픽과 오디오를 보다 쉽게 구현할 수 있도록 하는 iOS 9의 게임 Api에 대 한 몇 가지 기술적 향상을 만들었습니다. 여기에는 높은 수준의 프레임 워크를 통한 개발 용이성 및 향상 된 속도 및 그래픽 기능을 위한 iOS 장치의 GPU 활용 포함 됩니다.

## <a name="message-app-integrationiosplatformmessage-app-integrationindexmd"></a>[메시지 앱 통합](~/ios/platform/message-app-integration/index.md)

IOS 10의 새로운 기능으로, 메시지 앱 확장은 **메시지** 앱과 통합 되며 사용자에 게 새로운 기능을 제공 합니다. 확장은 텍스트, 스티커, 미디어 파일 및 대화형 메시지를 보낼 수 있습니다.

## <a name="multitasking-for-ipadiosplatformmultitaskingmd"></a>[iPad용 멀티태스킹](~/ios/platform/multitasking.md)

iOS 9는 특정 iPad 하드웨어에서 두 앱을 동시에 실행 하기 위한 멀티태스킹 지원을 추가 합니다. IPad 용 멀티태스킹은 다음 기능을 통해 지원 됩니다. 그림에서 위로 이동, 분할 보기 & 그림을 표시 합니다.

## <a name="passkitiosplatformpasskitmd"></a>[PassKit](~/ios/platform/passkit.md)

Passbook는 Iphone에 대 한 앱 이며 iOS 6과 함께 iPod를 사용 합니다. 휴대폰에서 고객 거래를 ' 실제 세계 '와 연결 하기 위한 바코드 및 기타 정보를 저장 하 고 표시 합니다. 통과는 판매자에 의해 생성 되 고 전자 메일, Url 또는 판매자의 고유한 iOS 앱 내에서 고객에 게 전송 됩니다. Passbook은 휴대폰의 모든 패스를 저장 및 구성 하 고, 장치의 날짜/시간 또는 위치에 따라 잠금 화면에 전달 미리 알림을 표시 합니다.

이 문서에서는 Passbook를 사용 하 여 Pass Kit API를 사용 하는 방법을 소개 하 고 서버에서 패스를 구현 하는 방법을 설명 합니다.

## <a name="photokitiosplatformphotokitmd"></a>[PhotoKit](~/ios/platform/photokit.md)

Photo Kit는 응용 프로그램이 시스템 이미지 라이브러리를 쿼리하고 사용자 지정 사용자 인터페이스를 만들어 콘텐츠를 보고 수정할 수 있도록 하는 새로운 프레임 워크입니다. 여기에는 이미지 및 비디오 자산을 나타내는 여러 클래스 뿐만 아니라 앨범 및 폴더와 같은 자산 컬렉션도 포함 됩니다.

## <a name="request-app-reviewiosplatformrequest-app-reviewmd"></a>[앱 검토 요청](~/ios/platform/request-app-review.md)

Ios 10.3의 새로운 기능으로 `RequestReview()` ,이 방법을 사용 하면 ios 앱에서 사용자에 게 평가 또는 검토를 요청할 수 있습니다. 사용자가 앱 스토어에서 설치한 배송 앱에서이 메서드를 호출 하면 iOS 10은 개발자에 대 한 전체 등급 및 검토 프로세스를 처리 합니다. 이 프로세스는 앱 스토어 정책에 의해 제어 되므로 경고가 표시 될 수도 있고 표시 되지 않을 수도 있습니다.

## <a name="search-apisiosplatformsearchindexmd"></a>[API 검색](~/ios/platform/search/index.md)

검색은 Xamarin.ios 앱 내에서 정보 및 기능에 액세스 하는 뛰어난 새 방법을 제공 하기 위해 iOS 9에서 확장 되었습니다. 새 앱 검색 Api를 사용 하 여 앱 콘텐츠는 스포트라이트 및 Safari 검색 결과, 전달 및 Siri 미리 알림 및 제안을 통해 검색할 수 있게 됩니다. 이렇게 하면 사용자가 앱 내에서 작업 및 정보 심층에 빠르게 액세스할 수 있습니다.

## <a name="sirikitiosplatformsirikitindexmd"></a>[SiriKit](~/ios/platform/sirikit/index.md)

IOS 10의 새로운 기능으로 SiriKit를 사용 하면 iOS 앱이 앱 확장 및 새 **의도** 및 **의도 UI** 프레임 워크를 사용 하 여 Ios 장치에서 siri 및 Maps 앱을 사용 하 여 사용자가 액세스할 수 있는 서비스를 제공할 수 있습니다.

## <a name="social-frameworkiosplatformsocial-frameworkmd"></a>[소셜 프레임 워크](~/ios/platform/social-framework.md)

소셜 프레임 워크는 _Twitter_ 및 _Facebook_을 비롯 한 소셜 네트워크와의 상호 작용을 위한 통합 API 뿐만 아니라 중국의 사용자를 위한 _SinaWeibo_ 을 제공 합니다.

## <a name="speech-recognitioniosplatformspeechmd"></a>[음성 인식](~/ios/platform/speech.md)

iOS 10에는 앱에서 연속 음성 인식 및 높여줄 음성 (라이브 또는 녹화 된 오디오 스트림에서)을 텍스트로 사용할 수 있도록 하는 새로운 Speech API 포함 되어 있습니다.

## <a name="textkitiosplatformtextkitmd"></a>[TextKit](~/ios/platform/textkit.md)

텍스트 키트는 강력한 텍스트 레이아웃 및 렌더링 기능을 제공 하는 새로운 API입니다. 낮은 수준의 핵심 텍스트 프레임 워크를 기반으로 하지만 핵심 텍스트 보다 훨씬 더 쉽게 사용할 수 있습니다.

## <a name="3d-touchiosplatform3d-touchmd"></a>[3D Touch](~/ios/platform/3d-touch.md)

이 문서에서는 새로운 3D 터치 Api를 사용 하 여 새로운 iPhone 6s 및 iPhone 6s Plus 장치에서 실행 되는 Xamarin.ios 앱에 압력 민감한 제스처를 추가 하는 방법을 소개 합니다.

## <a name="touch-idiosplatformtouchidmd"></a>[Touch ID](~/ios/platform/touchid.md)

Touch ID는 암호와 유사 하 게 사용자를 인증 하는 수단으로 iOS 7에서 도입 되었습니다. 그러나 앱 스토어를 사용 하 고 iTunes를 사용 하 고 iCloud 키 집합을 인증 하는 경우에만 장치의 잠금을 해제 하는 것이 제한 되었습니다.

## <a name="user-notificationsiosplatformuser-notificationsindexmd"></a>[사용자 알림](~/ios/platform/user-notifications/index.md)

IOS 10의 새로운 기능으로, 사용자 알림 프레임 워크를 사용 하면 로컬 및 원격 알림을 배달 하 고 처리할 수 있습니다. 이 프레임 워크를 사용 하 여 앱 또는 앱 확장은 위치 또는 시간 등의 조건 집합을 지정 하 여 로컬 알림의 배달을 예약할 수 있습니다.

## <a name="wide-coloriosplatformwide-colormd"></a>[와이드 컬러](~/ios/platform/wide-color.md)

iOS 10 및 macOS Sierra는 핵심 그래픽, 핵심 이미지, 금속 및 AVFoundation과 같은 프레임 워크를 포함 하 여 시스템 전체에서 확장 범위 픽셀 형식 및 넓은 색 영역 색 공간에 대 한 지원을 향상 시킵니다. 넓은 색 표시를 사용 하는 장치에 대 한 지원은 전체 그래픽 스택에이 동작을 제공 하 여 추가로 줄어들 됩니다.

## <a name="binding-objective-cbinding-objective-cindexmd"></a>[Objective-C 바인딩](binding-objective-c/index.md)

IOS에서 작업 하는 경우 타사 목표-C 라이브러리를 사용 하려는 경우가 발생할 수 있습니다. 이러한 상황에서는 Monotouch.dialog의 바인딩 프로젝트를 사용 하 여 네이티브 목표- C# C 라이브러리에 대 한 바인딩을 만들 수 있습니다. 프로젝트는 iOS Api를 가져오는 데 사용 하는 것과 동일한 도구를 C#사용 합니다. 이 문서에서는 목표-C Api를 바인딩하는 방법을 설명 합니다.

## <a name="referencing-native-librariesnative-interopmd"></a>[네이티브 라이브러리 참조](native-interop.md)

Xamarin.ios는 네이티브 C 라이브러리와 목적-C 라이브러리를 모두 사용 하 여 연결을 지원 합니다. 이 문서에서는 네이티브 C 라이브러리를 Xamarin.ios 프로젝트와 연결 하는 방법을 설명 합니다.

## <a name="embedded-frameworksembedded-frameworksmd"></a>[포함된 프레임워크](embedded-frameworks.md)

Xamarin.ios 앱에 목표-C 사용자 프레임 워크를 포함 하는 방법을 설명 합니다.
