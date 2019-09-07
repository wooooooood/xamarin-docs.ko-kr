---
title: 백그라운드에서 실행할 Xamarin.ios 앱 등록
description: 이 문서에서는 백그라운드에서 실행할 Xamarin.ios 응용 프로그램을 등록 하는 방법에 대해 설명 합니다. 오디오 앱, VoIP 앱, 외부 액세서리 및 bluetooth 등에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 6466d4c7edf6fde38fd3e9e8a6aaa48c2e5f9b4a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757687"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>백그라운드에서 실행할 Xamarin.ios 앱 등록

백그라운드 권한에 대해 개별 작업을 등록 하는 작업은 일부 응용 프로그램에서 작동 하지만 GPS를 통해 사용자에 대 한 지침을 가져오는 것과 같이 중요 한 장기 실행 작업을 수행 하기 위해 응용 프로그램을 지속적으로 호출 하는 경우 어떻게 되나요? 이러한 응용 프로그램은 대신 알려진 백그라운드 필수 응용 프로그램으로 등록 해야 합니다.

앱을 등록 하면 응용 프로그램에 백그라운드에서 작업을 수행 하는 데 필요한 특별 한 권한이 부여 되어야 한다는 신호를 iOS에 등록 합니다.

## <a name="application-registration-categories"></a>응용 프로그램 등록 범주

등록 된 앱은 다음과 같은 여러 범주로 나눌 수 있습니다.

- **오디오 음악** 플레이어 및 오디오 콘텐츠로 작업 하는 기타 응용 프로그램을 등록 하 여 앱이 더 이상 포그라운드에 있지 않아도 오디오를 계속 재생할 수 있습니다. 이 범주의 앱이 오디오 재생 또는 백그라운드에서 다운로드 이외의 다른 작업을 수행 하려고 하면 iOS에서 종료 됩니다.
- **Voip** -voip (Voice Over Internet Protocol) 응용 프로그램은 오디오 응용 프로그램에 부여 된 것과 동일한 권한을 받아 오디오를 백그라운드에서 계속 처리할 수 있습니다. 또한 연결을 유지 하기 위해 해당 장치를 구동 하는 VoIP 서비스에 필요에 따라 응답할 수 있습니다.
- **외부 액세서리 및 bluetooth** -bluetooth 장치 및 기타 외부 하드웨어 액세서리와 통신 해야 하는 응용 프로그램용으로 예약 된 이러한 범주에 등록 하면 앱이 하드웨어에 연결 된 상태를 유지할 수 있습니다.
- **Newsstand** -Newsstand 응용 프로그램은 백그라운드에서 콘텐츠를 계속 동기화 할 수 있습니다.
- **Location** -GPS 또는 네트워크 위치 데이터를 사용 하는 응용 프로그램은 백그라운드에서 위치 업데이트를 보내고 받을 수 있습니다.
- **Fetch (iOS 7 이상)** -백그라운드 가져오기 권한에 대해 등록 된 응용 프로그램은 공급자에 게 정기적으로 새 콘텐츠를 확인 하 여 사용자가 응용 프로그램으로 돌아올 때 업데이트 된 콘텐츠를 제공할 수 있습니다.
- **원격 알림 (iOS 7 이상)** -응용 프로그램을 등록 하 여 공급자 로부터 알림을 수신 하 고 사용자가 응용 프로그램을 열기 전에 알림을 사용 하 여 업데이트를 시작할 수 있습니다. 알림은 푸시 알림 형태로 제공 되거나 응용 프로그램을 자동으로 절전 모드에서 해제 하도록 선택할 수 있습니다.

응용 프로그램의 *info.plist*에서 **필요한 백그라운드 모드** 속성을 설정 하 여 응용 프로그램을 등록할 수 있습니다. 응용 프로그램은 필요한 만큼 범주에 등록할 수 있습니다.

 [![](registering-applications-to-run-in-background-images/bgmodes.png "배경 모드 설정")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

백그라운드 위치 업데이트를 위해 응용 프로그램을 등록 하는 단계별 가이드는 [백그라운드 위치 연습](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md)을 참조 하세요.

## <a name="application-does-not-run-in-background-property"></a>응용 프로그램이 백그라운드 속성에서 실행 되지 않음

Info.plist에서 설정할 수 있는 다른 속성은 *응용 프로그램이 백그라운드에서 실행 되지*않거나 `UIApplicationExitsOnSuspend` 속성으로 실행 되지 않습니다 *.*

 [![](registering-applications-to-run-in-background-images/plist.png "백그라운드 실행 해제")](registering-applications-to-run-in-background-images/plist.png#lightbox)

이는 개발자 쪽 에서만 변경 될 수 있고 iOS 4 이상에서 사용할 수 있는 경우를 제외 하 고는 iOS 7 이상에서 백그라운드 앱 새로 고침 설정을 off로 설정 하는 것과 정확히 동일한 효과를 가집니다. 응용 프로그램은 백그라운드에 입력 한 직후에 일시 중단 되며 처리를 수행할 수 없습니다.

응용 프로그램이 백그라운드 처리를 처리 하도록 설계 되지 않은 경우 예기치 않은 동작을 방지 하는 데 도움이 되므로이 속성을 사용 합니다.

## <a name="background-fetch-and-remote-notifications"></a>백그라운드 페치 및 원격 알림

백그라운드 페치 및 원격 알림은 iOS 7에 도입 된 특별 등록 범주입니다. 이러한 범주를 통해 응용 프로그램은 공급자에서 새 콘텐츠를 받고 백그라운드에서 업데이트할 수 있습니다. 다음 섹션에서는 fetch 및 원격 알림에 대해 자세히 알아보고, iOS 6의 백그라운드에서 응용 프로그램을 업데이트 하는 수단으로 위치 인식을 소개 합니다.
