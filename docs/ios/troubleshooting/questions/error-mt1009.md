---
title: '오류 MT1009: 어셈블리를 복사할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: b01701566cec88ad4a1493cf5ab9778bd38792b0
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511811"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>오류 MT1009: 어셈블리를 복사할 수 없습니다.

> [!IMPORTANT]
> 이 문제는 최신 버전의 Xamarin.ios에서 해결 되었습니다. 그러나 최신 버전의 소프트웨어에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

[릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_7/xamarin.ios_7.2/index.md)에 설명 된 것 처럼이는 xamarin.ios 7.2.6의 알려진 문제입니다. 이 문제는 Xamarin.ios가 다른 사용자 계정으로 설치 된 후 개발자의 기본 계정을 사용 하 여 더 높은 권한을 필요로 하는 파일 사용 권한 때문에 발생 합니다.

이 문제를 해결 하려면 Mac 워크스테이션에서 Terminal. 앱을 열고 다음 명령을 실행 하세요.

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`
