---
title: "백그라운드에서 실행 되도록 응용 프로그램 등록"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 5fcb41f4f60adc8ca5be761c2b9a7449387a89d0
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="registering-applications-to-run-in-the-background"></a>백그라운드에서 실행 되도록 응용 프로그램 등록

GPS 통해 사용자에 대 한 지침을 얻는 등의 중요 한, 장기 실행 작업을 수행 하는 응용 프로그램 지속적으로 요청할 경우 어떤 일이 생기 하지만 일부 응용 프로그램에 대 한 배경 권한이 작동에 대 한 개별 작업을 등록 하는? 이러한 응용 프로그램 대신 알려진된 배경 필요한 응용 프로그램으로 등록 해야 합니다.

앱 등록에 알립니다 iOS 응용 프로그램은 백그라운드에서 작업을 수행 하는 데 필요한 특별 한 권한이 제공 됩니다.

## <a name="application-registration-categories"></a>응용 프로그램 등록 범주

등록 된 응용 프로그램은 여러 가지 범주로 나눌 수 있습니다.:

-  **오디오** -음악 플레이어와 오디오 콘텐츠를 사용 하는 다른 응용 프로그램을 계속 등록할 수 없습니다가 경우에 응용 프로그램이 더 이상 포그라운드에서 오디오 재생 합니다. 이 범주에는 응용 프로그램을 백그라운드에서 작업 하는 동안 다운로드 또는 오디오 재생 이외의 아무런 작업도 하려고 iOS 종료 됩니다.
-  **VoIP** -음성을 통해 인터넷 프로토콜 (VoIP) 응용 프로그램은 백그라운드에서 오디오 처리 유지 오디오 응용 프로그램에 부여 된 동일한 권한 있게 합니다. 필요에 따라 전원에 연결을 유지 하는 VoIP 서비스에 응답 하도록 사용할 수 있습니다.
-  **외부 액세서리 및 Bluetooth** -Bluetooth 장치 및 기타 외부 하드웨어 액세서리와 통신 하는 응용 프로그램에 대 한 예약 된 이러한 범주는 등록을 허용 하려면 하드웨어에 연결 된 상태로 유지 합니다.
-  **뉴스 스탠드** -백그라운드에서 콘텐츠를 동기화 하는 뉴스 스탠드 응용 프로그램을 계속 합니다.
-  **위치** -GPS 사용 하는 응용 프로그램과 또는 네트워크 위치 데이터를 보내고 백그라운드에서 위치 업데이트를 받을 수 있습니다.
-  **Fetch (iOS 7 이상)** -배경 인출 권한이 응용 프로그램에 반환 될 때 사용자를 업데이트 된 콘텐츠로 표시 하는 정기적으로 새 내용에 대 한 공급자를 확인할 수에 대 한 등록 응용 프로그램입니다.
-  **원격 알림 (iOS 7 이상)** -응용 프로그램 전에 사용자가 응용 프로그램에 대 한 업데이트를 시작 알림을 사용 하는 공급자 로부터 알림을 수신 하도록 등록할 수 있습니다. 알림을 푸시 알림을 형태 또는 응용 프로그램을 자동으로 켜 지도록 하도록 선택할 수 있습니다.


응용 프로그램을 설정 하 여 등록할 수 있습니다는 **백그라운드 모드 필요한** 응용 프로그램의 속성 *Info.plist*합니다. 응용 프로그램은 필요한 수 만큼 범주에 등록할 수 있습니다.

 [![](registering-applications-to-run-in-background-images/bgmodes.png "백그라운드 모드를 설정합니다.")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

백그라운드 위치 업데이트에 대 한 응용 프로그램을 등록 하는 단계별 가이드에 대 한 참조는 [배경 위치 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md)합니다.

## <a name="application-does-not-run-in-background-property"></a>Background 속성에서는 응용 프로그램 실행 되지 않습니다.

다른 속성에 설정할 수 있는 *Info.plist* 는 *응용 프로그램이 백그라운드에서 실행 되지 않는*, 또는 `UIApplicationExitsOnSuspend` 속성:

 [![](registering-applications-to-run-in-background-images/plist.png "배경 실행을 사용 하지 않도록 설정")](registering-applications-to-run-in-background-images/plist.png#lightbox)

이 백그라운드 응용 프로그램 새로 고침 설정이 개발자 쪽에만 변경할 수 있습니다 점을 제외 하 고 iOS 7 이상에서 off로 설정 하는 것과 동일한 결과가 정확 하 게 이며 iOS 4 이상용 수 있습니다. 응용 프로그램 백그라운드를 입력 한 후 즉시 일시 중단 및 처리 작업을 수행할 수 없습니다.

응용 프로그램이 예기치 않은 동작을 방지 하는 대로 백그라운드 처리를 처리 하도록 설계 되지 않은 경우이 속성을 사용 합니다.

## <a name="background-fetch-and-remote-notifications"></a>백그라운드 가져오기 및 원격 알림

백그라운드 가져오기 및 원격 알림은 iOS 7에에서 도입 하는 특별 한 등록 범주입니다. 이러한 범주는 응용 프로그램의 새 콘텐츠는 공급자에서 검색 하 고 백그라운드에서 업데이트를 허용 합니다. 다음 섹션 인출 및 원격 알림을 자세히 탐색 하 고 또한 위치 인식 iOS 6에서 백그라운드에서 응용 프로그램 업데이트에 대 한 것을 의미 하는 대로 소개 합니다.
