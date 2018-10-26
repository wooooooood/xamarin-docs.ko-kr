---
title: Xamarin.iOS 앱이 백그라운드에서 실행 되도록 등록
description: 이 문서에서는 백그라운드에서 실행할 Xamarin.iOS 응용 프로그램을 등록 하는 방법을 설명 합니다. 오디오 앱, VoIP 앱, 외부 액세서리 및 bluetooth 및 자세히 설명합니다.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a0a66571d0249ef6fd65ff382f14c38f48a8af37
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105170"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>Xamarin.iOS 앱이 백그라운드에서 실행 되도록 등록

일부 응용 프로그램 이지만 GPS 통해 사용자에 대 한 지침을 얻는 등의 중요 한, 장기 실행 작업을 수행 하는 응용 프로그램 지속적으로 요청할 경우 백그라운드 권한이 작동에 대 한 개별 작업을 등록 하는? 대신 알려진된 백그라운드 필요한 응용 프로그램으로 이러한 응용 프로그램을 등록 되어야 합니다.

앱 등록에 신호를 보냅니다 iOS 응용 프로그램에서 백그라운드 작업을 수행 하는 데 필요한 특별 한 권한이 제공 되어야 합니다.

## <a name="application-registration-categories"></a>응용 프로그램 등록 범주

등록 된 앱은 여러 가지 범주로 나눌 수 있습니다.

-  **오디오** -오디오 콘텐츠를 사용 하는 다른 프로그램과 음악 플레이어를 계속 등록할 수 없습니다 앱 경우에 더 이상 포그라운드에 오디오 재생 합니다. 이 범주에서 앱을 백그라운드에서 다운로드 또는 오디오 재생 이외의 작업을 수행할 하려고 iOS 종료 됩니다.
-  **VoIP** -음성을 통해 인터넷 프로토콜 (VoIP) 응용 프로그램 계속 백그라운드에서 오디오를 처리 하는 오디오 응용 프로그램에 부여 된 동일한 권한을 활용 합니다. 또한 필요에 따라 해당 연결을 유지 하기는 VoIP 서비스에 응답할 수 있습니다.
-  **외부 액세서리 및 Bluetooth** -앱이 하드웨어에 연결을 유지할 수 있게 이러한 범주로 등록 Bluetooth 장치 및 기타 외부 하드웨어 액세서리와 통신 해야 하는 응용 프로그램을 예약 합니다.
-  **Newsstand** -백그라운드에서 콘텐츠를 동기화 하는 Newsstand 응용 프로그램을 계속 합니다.
-  **위치** -GPS를 사용 하는 응용 프로그램 또는 네트워크 위치 데이터를 보내고 백그라운드에서 위치 업데이트를 받을 수 있습니다.
-  **Fetch (iOS 7 이상)** -백그라운드 페치 권한이 응용 프로그램에 반환 될 때 업데이트 된 콘텐츠를 사용 하 여 사용자를 제공 하는 정기적으로 새 내용에 대 한 공급자를 확인할 수에 대해 등록 된 응용 프로그램입니다.
-  **원격 알림 (iOS 7 이상)** -응용 프로그램 공급자 로부터 알림을 수신 하도록 등록 하는 알림을 사용 하 여 사용자가 응용 프로그램 전에 업데이트를 시작 합니다. 알림, 푸시 알림의 형태로 제공 하거나 응용 프로그램을 자동으로 절전 모드를 선택할 수 있습니다.


응용 프로그램을 설정 하 여 등록할 수는 **필수 백그라운드 모드** 응용 프로그램에서 속성 *Info.plist*합니다. 응용 프로그램 필요한 만큼 범주에서 등록할 수 있습니다.

 [![](registering-applications-to-run-in-background-images/bgmodes.png "백그라운드 모드를 설정합니다.")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

백그라운드 위치 업데이트에 대 한 응용 프로그램을 등록 하는 단계별 가이드를 참조 하세요. 합니다 [백그라운드 위치 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md)합니다.

## <a name="application-does-not-run-in-background-property"></a>Background 속성에서 응용 프로그램이 실행 되지 않습니다.

다른 속성을 설정할 수 있습니다 *Info.plist* 되는 *응용 프로그램이 백그라운드에서 실행 되지 않습니다*, 또는 `UIApplicationExitsOnSuspend` 속성:

 [![](registering-applications-to-run-in-background-images/plist.png "실행 중인 백그라운드를 사용 하지 않도록 설정")](registering-applications-to-run-in-background-images/plist.png#lightbox)

이 개발자 측에서 변경할 수 있습니다만 제외 하 고 iOS 7 이상에서 해제 하면서 앱 새로 고침 설정을 설정 하는 것과 동일한 효과가 정확한 이며 iOS 4 이상을 사용할 수 있습니다. 응용 프로그램이 백그라운드에서 입력 한 후에 즉시 일시 중단 되 고 처리 작업을 수행할 수 없습니다.

예기치 않은 동작이 방지 하는 대로 백그라운드 처리를 처리 하는 응용 프로그램 설계 되지 않은 경우이 속성을 사용 합니다.

## <a name="background-fetch-and-remote-notifications"></a>백그라운드 페치 및 원격 알림

백그라운드 페치 원격 알림와 iOS 7에에서 도입 된 특별 한 등록 범주입니다. 이러한 범주는 응용 프로그램에서 공급자를 새 콘텐츠를 받고 백그라운드에서 업데이트를 허용 합니다. 다음 섹션에서는 인출 하 고 원격 알림을 더 자세히 설명 하 고 또한 iOS 6 백그라운드에서 응용 프로그램 업데이트에 대 한 방법으로 위치 인식을 소개 합니다.
