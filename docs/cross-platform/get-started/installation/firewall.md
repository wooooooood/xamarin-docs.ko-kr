---
title: "Xamarin 방화벽 구성 지침"
description: "Xamarin의 플랫폼을 회사에서 실행할 수 있도록 방화벽에서 허용 목록을 지정해야 하는 호스트 목록입니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: de68c1a8ceec381faf1b867c708e04030d39c73a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin 방화벽 구성 지침

_Xamarin의 플랫폼을 회사에서 실행할 수 있도록 방화벽에서 허용 목록을 지정해야 하는 호스트 목록입니다._

Xamarin 제품을 설치하고 제대로 작동시키기 위해 특정 끝점에 액세스하여 소프트웨어에 필수 도구 및 업데이트를 다운로드해야 합니다. 사용자 또는 회사에 엄격한 방화벽이 설정된 경우 설치, 라이선스, 구성 요소 등과 관련된 문제가 발생할 수 있습니다. 이 문서에서는 Xamarin이 작동하도록 하기 위해 방화벽에서 허용 목록을 지정해야 하는 알려진 끝점 중 일부에 대해 설명합니다. 이 목록에는 다운로드에 포함된 타사 도구에 필요한 끝점이 포함되지 않습니다. 이 목록을 완료한 후에도 여전히 문제가 발생하는 경우 Apple 또는 Android 설치 문제 해결 가이드를 참조합니다.

## <a name="endpoints-to-whitelist"></a>끝점부터 허용 목록까지

### <a name="xamarin-installer"></a>Xamarin 설치 관리자

최신 버전의 Xamarin 설치 관리자를 사용하는 경우 소프트웨어를 제대로 설치하기 위해 다음과 같이 알려진 주소를 추가해야 합니다.

-  xamarin.com(설치 관리자 매니페스트)
-  dl.xamarin.com(패키지 다운로드 위치)
-  dl.google.com(Android SDK 다운로드)
-  download.oracle.com(JDK)
-  visualstudio.com(설정 패키지 다운로드 위치)
-  go.microsoft.com(설정 URL 확인)
-  aka.ms(설정 URL 확인)

Mac을 사용하고 Xamarin.Android 설치 문제가 발생하는 경우 macOS를 사용하여 Java를 다운로드할 수 있도록 하세요.


### <a name="components-store-and-nuget-including-xamarinforms"></a>구성 요소 저장소 및 NuGet(Xamarin.Forms 포함)

다음 주소를 추가하여 Xamarin 구성 요소 저장소 또는 NuGet에 액세스해야 합니다(Xamarin.Forms는 NuGet으로 패키지됨).

-  components.xamarin.com(Xamarin 구성 요소 저장소 사용)
-  xampubdl.blob.core.windows.net(호스트 구성 요소 저장소 다운로드)
-  www.nuget.org(NuGet에 액세스)
-  az320820.vo.msecnd.net(NuGet 다운로드)
-  dl-ssl.google.com(Google 구성 요소)


### <a name="software-updates"></a>소프트웨어 업데이트

다음 주소를 추가하여 소프트웨어 업데이트를 적절하게 다운로드할 수 있어야 합니다.

-  software.xamarin.com(업데이트 서비스)
-  download.visualstudio.microsoft.com
-  dl.xamarin.com

### <a name="xamarin-insights"></a>Xamarin Insights

다음 주소를 추가하여 작업이 Xamarin Insights 서버에 도달하도록 해야 합니다.

* https://xaapi.xamarin.com


## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

Xamarin을 사용하는 Mac 빌드 호스트에 Visual Studio를 연결하려면 Mac 에이전트에서는 SSH 포트를 열어야 합니다. 기본적으로 **포트 22**입니다.

## <a name="summary"></a>요약

이 가이드에서는 컴퓨터에 Xamarin 제품을 설치하고 제대로 업데이트할 수 있도록 허용 목록을 지정해야 하는 끝점에 대해 다룹니다.