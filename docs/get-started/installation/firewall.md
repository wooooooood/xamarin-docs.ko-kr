---
title: Xamarin 방화벽 구성 지침
description: 이 문서에서는 Xamarin이 기업 환경에서 작업하도록 방화벽에서 허용해야 하는 호스트의 목록을 제공합니다.
ms.prod: xamarin
ms.assetid: 658f699b-8cca-48f7-ae54-fa956384b6d6
author: asb3993
ms.author: amburns
ms.date: 10/05/2018
ms.openlocfilehash: 68689ce7d92a038d0724e1441f68fddcb1d0bba8
ms.sourcegitcommit: d62732ce6f3f9d8dc929d72d4acac3e592cba073
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57199659"
---
# <a name="xamarin-firewall-configuration-instructions"></a>Xamarin 방화벽 구성 지침

_Xamarin의 플랫폼을 회사에서 실행할 수 있도록 방화벽에서 허용 목록을 지정해야 하는 호스트 목록입니다._

Xamarin 제품을 설치하고 제대로 작동시키기 위해 특정 엔드포인트에 액세스하여 소프트웨어에 필수 도구 및 업데이트를 다운로드해야 합니다. 사용자 또는 회사에 엄격한 방화벽이 설정된 경우 설치, 라이선스, 구성 요소 등과 관련된 문제가 발생할 수 있습니다. 이 문서에서는 Xamarin이 작동하도록 하기 위해 방화벽에서 허용 목록을 지정해야 하는 알려진 엔드포인트 중 일부에 대해 설명합니다. 이 목록에는 다운로드에 포함된 타사 도구에 필요한 엔드포인트가 포함되지 않습니다. 이 목록을 완료한 후에도 여전히 문제가 발생하는 경우 Apple 또는 Android 설치 문제 해결 가이드를 참조합니다.

## <a name="endpoints-to-whitelist"></a>엔드포인트부터 허용 목록까지

### <a name="xamarin-installer"></a>Xamarin 설치 관리자

최신 버전의 Xamarin 설치 관리자를 사용하는 경우 소프트웨어를 제대로 설치하기 위해 다음과 같이 알려진 주소를 추가해야 합니다.

- xamarin.com(설치 관리자 매니페스트)
- dl.xamarin.com(패키지 다운로드 위치)
- dl.google.com(Android SDK 다운로드)
- download.oracle.com(JDK)
- visualstudio.com(설정 패키지 다운로드 위치)
- go.microsoft.com(설정 URL 확인)
- aka.ms(설정 URL 확인)

Mac을 사용하고 Xamarin.Android 설치 문제가 발생하는 경우 macOS를 사용하여 Java를 다운로드할 수 있도록 하세요.

### <a name="nuget-including-xamarinforms"></a>NuGet(Xamarin.Forms 포함)

다음 주소를 추가하여 NuGet에 액세스해야 합니다(Xamarin.Forms는 NuGet으로 패키지됨).

- www\.nuget.org(NuGet에 액세스)
- az320820.vo.msecnd.net(NuGet 다운로드)
- dl-ssl.google.com(Android 및 Xamarin.Forms용 Google 구성 요소)

### <a name="software-updates"></a>소프트웨어 업데이트

다음 주소를 추가하여 소프트웨어 업데이트를 적절하게 다운로드할 수 있어야 합니다.

- software.xamarin.com(업데이트 서비스)
- download.visualstudio.microsoft.com
- dl.xamarin.com

## <a name="xamarin-mac-agent"></a>Xamarin Mac Agent

Xamarin을 사용하는 Mac 빌드 호스트에 Visual Studio를 연결하려면 Mac 에이전트에서는 SSH 포트를 열어야 합니다. 기본적으로 **포트 22**입니다.

## <a name="summary"></a>요약

이 가이드에서는 컴퓨터에 Xamarin 제품을 설치하고 제대로 업데이트할 수 있도록 허용 목록을 지정해야 하는 엔드포인트에 대해 다룹니다.
